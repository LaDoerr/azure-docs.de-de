---
title: Verarbeiten der Neustarts von Linux-Knoten mit kured
titleSuffix: Azure Kubernetes Service
description: Erfahren Sie, wie Linux-Knoten mit kured in Azure Kubernetes Service (AKS) aktualisiert und automatisch neu gestartet werden
services: container-service
ms.topic: article
ms.date: 02/28/2019
ms.openlocfilehash: a81d778b8346a03622ef837b6732e7d50e807652
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122355851"
---
# <a name="apply-security-and-kernel-updates-to-linux-nodes-in-azure-kubernetes-service-aks"></a>Anwenden von Sicherheits- und Kernelupdates auf Linux-Knoten in Azure Kubernetes Service (AKS)

Sicherheitsupdates werden automatisch auf Linux-Knoten in AKS angewendet, um Ihre Cluster zu schützen. Diese Updates enthalten Sicherheitsfixes für das Betriebssystem oder Kernelupdates. Einige dieser Updates erfordern den Neustart eines Knotens, um den Vorgang abzuschließen. AKS startet diese Linux-Knoten nicht automatisch neu, um das Update abzuschließen.

Der Prozess, mit dem Windows Server-Knoten aktuell gehalten werden, ist ein wenig anders. Windows Server-Knoten werden nicht täglich aktualisiert. Stattdessen führen Sie ein AKS-Upgrade aus, das neue Knoten mit dem neuesten Basisimage und den neuesten Patches für Windows Server bereitstellt. Weitere Informationen zu AKS-Clustern, die Windows Server-Knoten verwenden, finden Sie unter [Durchführen eines Upgrades für einen Knotenpool][nodepool-upgrade].

In diesem Artikel erfahren Sie, wie Sie mit dem Open-Source-Daemon [kured (KUbernetes REboot Daemon)][kured] nach Linux-Knoten suchen, die einen Neustart erfordern, und dann automatisch die Neuplanung der ausgeführten Pods und den Knotenneustart verarbeiten.

> [!NOTE]
> `Kured` ist ein Open Source-Projekt von Weaveworks. Die Unterstützung dieses Projekts in AKS wird bestmöglich umgesetzt. Zusätzliche Unterstützung erhalten Sie im Slack-Channel namens #weave-community.

## <a name="before-you-begin"></a>Voraussetzungen

Es wird vorausgesetzt, dass Sie über ein AKS-Cluster verfügen. Wenn Sie einen AKS-Cluster benötigen, erhalten Sie weitere Informationen im AKS-Schnellstart. Verwenden Sie dafür entweder die [Azure CLI][aks-quickstart-cli] oder das [Azure-Portal][aks-quickstart-portal].

Außerdem muss mindestens die Version 2.0.59 der Azure CLI installiert und konfiguriert sein. Führen Sie `az --version` aus, um die Version zu ermitteln. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sie bei Bedarf unter [Installieren der Azure CLI][install-azure-cli].

## <a name="understand-the-aks-node-update-experience"></a>Grundlegendes zum AKS-Knotenupdate

In einem AKS-Cluster werden die Kubernetes-Knoten als virtuelle Azure-Computer (VMs) ausgeführt. Diese Linux-basierten VMs verwenden ein Ubuntu-Image, bei dem das Betriebssystem so konfiguriert ist, dass es automatisch jede Nacht nach Updates sucht. Wenn Sicherheits- oder Kernelupdates verfügbar sind, werden sie automatisch heruntergeladen und installiert.

![Aktualisieren und Neustarten von AKS-Knoten mit kured](media/node-updates-kured/node-reboot-process.png)

Einige Sicherheitsupdates, z.B. Kernelupdates, erfordern einen Knotenneustart, damit der Vorgang abgeschlossen wird. Ein Linux-Knoten, für den ein Neustart erforderlich ist, erstellt eine Datei namens */var/run/reboot-required*. Dieser Neustart erfolgt nicht automatisch.

Sie können eigene Workflows und Prozesse für Neustarts von Knoten nutzen oder `kured` verwenden, um den Prozess zu orchestrieren. Mit `kured` wird ein [DaemonSet][DaemonSet] bereitgestellt, das einen Pod auf jedem Linux-Knoten im Cluster ausführt. Diese Pods im DaemonSet suchen nach dem Vorhandensein der Datei */var/run/reboot-required* und initiieren dann einen Prozess, um die Knoten neu zu starten.

### <a name="node-image-upgrades"></a>Upgrades für Knotenimages

Unbeaufsichtigte Upgrades wenden Updates auf das Betriebssystem des Linux-Knotens an, aber das Image, das zum Erstellen von Knoten für Ihren Cluster verwendet wird, bleibt unverändert. Wenn Ihrem Cluster ein neuer Linux-Knoten hinzugefügt wird, wird das ursprüngliche Image zum Erstellen des Knotens verwendet. Dieser neue Knoten empfängt alle Sicherheits- und Kernelupdates, die während der automatischen Überprüfung jede Nacht verfügbar sind, bleibt jedoch ungepatcht, bis alle Überprüfungen und Neustarts abgeschlossen sind.

Alternativ können Sie das Knotenimageupgrade verwenden, um nach Knotenimages zu suchen und diese zu aktualisieren, die von Ihrem Cluster verwendet werden. Ausführlichere Informationen zu Knotenimageupgrades finden Sie unter [Upgrade für AKS-Knotenimages (Azure Kubernetes Service)][node-image-upgrade].

### <a name="node-upgrades"></a>Knotenupgrades

In AKS gibt es einen weiterer Prozess, mit dem Sie ein *Upgrade* eines Clusters durchführen können. Ein Upgrade ist in der Regel der Wechsel zu einer neueren Version von Kubernetes, nicht nur das Anwenden von Sicherheitsupdates auf Knoten. Ein AKS-Upgrade führt folgende Aktionen aus:

* Ein neuer Knoten wird mit den neuesten Sicherheitsupdates und der aktuellen Kubernetes-Version bereitgestellt.
* Ein alter Knoten wird gesperrt und geleert.
* Pods werden auf dem neuen Knoten geplant.
* Der alte Knoten wird gelöscht.

Sie können während eines Upgrades nicht die gleiche Kubernetes-Version behalten. Sie müssen eine neuere Version von Kubernetes angeben. Um ein Upgrade auf die neueste Version von Kubernetes durchzuführen, können Sie [Ihren AKS-Cluster upgraden][aks-upgrade].

## <a name="deploy-kured-in-an-aks-cluster"></a>Bereitstellen von kured in einem AKS-Cluster

Installieren Sie zur Bereitstellung des `kured` DaemonSets das folgende offizielle Kured Helm-Chart. Dies erstellt eine Rolle und eine Clusterrolle, Bindungen und ein Dienstkonto. Dann wird das DaemonSet mit `kured` bereitgestellt.

```console
# Add the Kured Helm repository
helm repo add kured https://weaveworks.github.io/kured

# Update your local Helm chart repository cache
helm repo update

# Create a dedicated namespace where you would like to deploy kured into
kubectl create namespace kured

# Install kured in that namespace with Helm 3 (only on Linux nodes, kured is not working on Windows nodes)
helm install kured kured/kured --namespace kured --set nodeSelector."kubernetes\.io/os"=linux
```

Sie können auch zusätzliche Parameter für `kured` konfigurieren, z.B. die Integration in Prometheus oder Slack. Weitere Informationen zu zusätzlichen Konfigurationsparametern finden Sie im [Kured Helm-Chart][kured-install].

## <a name="update-cluster-nodes"></a>Aktualisieren von Clusterknoten

In der Standardeinstellung suchen Linux-Knoten in AKS jeden Abend nach Updates. Wenn Sie nicht warten möchten, können Sie ein Update manuell ausführen, um zu überprüfen, ob `kured` ordnungsgemäß ausgeführt wird. Führen Sie zunächst die Schritte zum Herstellen einer [SSH-Verbindung mit einem Ihrer AKS-Knoten][aks-ssh] aus. Wenn eine SSH-Verbindung mit dem Linux-Knoten besteht, suchen Sie nach Updates, und wenden Sie diese wie folgt an:

```console
sudo apt-get update && sudo apt-get upgrade -y
```

Wenn Updates angewendet wurden, für die ein Knotenneustart erforderlich ist, wird eine Datei in */var/run/reboot-required* geschrieben. `Kured` prüft alle 60 Minuten auf Knoten, die einen Neustart erfordern.

## <a name="monitor-and-review-reboot-process"></a>Überwachen und Überprüfen des Neustarts

Wenn eines der Replikate im DaemonSet festgestellt hat, dass ein Knotenneustart erforderlich ist, wird der Knoten über die Kubernetes-API gesperrt. Diese Sperre verhindert, dass weitere Pods auf dem Knoten geplant werden. Die Sperre gibt auch an, dass immer nur ein Knoten zur Zeit neu gestartet werden soll. Wenn der Knoten gesperrt ist, werden ausgeführte Pods vom Knoten entfernt, und der Knoten wird neu gestartet.

Sie können den Status der Knoten mit dem Befehl [kubectl get nodes][kubectl-get-nodes] überwachen. Die folgende Beispielausgabe zeigt einen Knoten mit dem Status *SchedulingDisabled*, da der Knoten auf den Neustart vorbereitet wird:

```
NAME                       STATUS                     ROLES     AGE       VERSION
aks-nodepool1-28993262-0   Ready,SchedulingDisabled   agent     1h        v1.11.7
```

Nachdem das Update abgeschlossen wurde, können Sie den Status der Knoten mit dem Befehl [kubectl get nodes][kubectl-get-nodes] mit dem Parameter `--output wide` anzeigen. Durch diese zusätzliche Ausgabe können Sie einen Unterschied bei *KERNEL-VERSION* der zugrunde liegende Knoten erkennen, wie in der folgenden Beispielausgabe gezeigt. *aks-nodepool1-28993262-0* wurde in einem früheren Schritt aktualisiert und zeigt die Kernelversion *4.15.0-1039-azure* an. Der Knoten *aks-nodepool1-28993262-1*, der noch nicht aktualisiert wurde, zeigt die Kernelversion *4.15.0-1037-azure* an.

```
NAME                       STATUS    ROLES     AGE       VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION      CONTAINER-RUNTIME
aks-nodepool1-28993262-0   Ready     agent     1h        v1.11.7   10.240.0.4    <none>        Ubuntu 16.04.6 LTS   4.15.0-1039-azure   docker://3.0.4
aks-nodepool1-28993262-1   Ready     agent     1h        v1.11.7   10.240.0.5    <none>        Ubuntu 16.04.6 LTS   4.15.0-1037-azure   docker://3.0.4
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurde beschrieben, wie Sie mit `kured` Linux-Knoten bei einem Sicherheitsupdate automatisch neu starten können. Um ein Upgrade auf die neueste Version von Kubernetes durchzuführen, können Sie [Ihren AKS-Cluster upgraden][aks-upgrade].

Weitere Informationen zu AKS-Clustern, die Windows Server-Knoten verwenden, finden Sie unter [Durchführen eines Upgrades für einen Knotenpool][nodepool-upgrade].

<!-- LINKS - external -->
[kured]: https://github.com/weaveworks/kured
[kured-install]: https://github.com/weaveworks/kured/tree/master/charts/kured
[kubectl-get-nodes]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[DaemonSet]: concepts-clusters-workloads.md#statefulsets-and-daemonsets
[aks-ssh]: ssh.md
[aks-upgrade]: upgrade-cluster.md
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
[node-image-upgrade]: node-image-upgrade.md
