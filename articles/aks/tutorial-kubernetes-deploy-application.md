---
title: 'Tutorial zu Kubernetes in Azure: Bereitstellen einer Anwendung'
description: In diesem Azure Kubernetes Service-Tutorial (AKS) stellen Sie mithilfe eines in Azure Container Registry gespeicherten benutzerdefinierten Images eine Anwendung mit mehreren Containern in Ihrem Cluster bereit.
services: container-service
ms.topic: tutorial
ms.date: 05/24/2021
ms.custom: mvc, devx-track-azurepowershell
ms.openlocfilehash: 9632b3cb2e0e3b46cb024df0cc6dddd6829aed05
ms.sourcegitcommit: 20acb9ad4700559ca0d98c7c622770a0499dd7ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2021
ms.locfileid: "110697927"
---
# <a name="tutorial-run-applications-in-azure-kubernetes-service-aks"></a>Tutorial: Ausführen von Anwendungen in Azure Kubernetes Service (AKS)

Kubernetes bietet eine verteilte Plattform für containerbasierte Anwendungen. Sie erstellen Ihre eigenen Anwendungen und Dienste in einem Kubernetes-Cluster, stellen diese bereit und lassen den Cluster die Verfügbarkeit und Konnektivität verwalten. In diesem Tutorial – Teil 4 von 7 – wird eine Beispielanwendung in einem Kubernetes-Cluster bereitgestellt. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Aktualisieren einer Kubernetes-Manifestdatei
> * Ausführen einer Anwendung in Kubernetes
> * Testen der Anwendung

In nachfolgenden Tutorials wird diese Anwendung aufskaliert und aktualisiert.

Für diese Schnellstartanleitung werden Grundkenntnisse in Bezug auf die Kubernetes-Konzepte vorausgesetzt. Weitere Informationen finden Sie unter [Grundlegende Kubernetes-Konzepte für Azure Kubernetes Service (AKS)][kubernetes-concepts].

## <a name="before-you-begin"></a>Voraussetzungen

In vorherigen Tutorials wurde eine Anwendung in ein Containerimage gepackt, das Image wurde in Azure Container Registry hochgeladen, und es wurde ein Kubernetes-Cluster erstellt.

Für dieses Tutorial benötigen Sie die vorab erstellte Kubernetes-Manifestdatei `azure-vote-all-in-one-redis.yaml`. Diese Datei wurde in einem vorherigen Tutorial mit dem Anwendungsquellcode heruntergeladen. Stellen Sie sicher, dass Sie das Repository geklont und Verzeichnisse im geklonten Repository geändert haben. Wenn Sie diese Schritte nicht ausgeführt haben und dies jetzt nachholen möchten, beginnen Sie mit [Tutorial 1: Erstellen von Containerimages][aks-tutorial-prepare-app].

### <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Für dieses Tutorial müssen Sie mindestens Version 2.0.53 der Azure CLI ausführen. Führen Sie `az --version` aus, um die Version zu ermitteln. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sie bei Bedarf unter [Installieren der Azure CLI][azure-cli-install].

### <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

Für dieses Tutorial müssen Sie mindestens die Azure PowerShell-Version 5.9.0 ausführen. Führen Sie `Get-InstalledModule -Name Az` aus, um die Version zu ermitteln. Falls Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure PowerShell][azure-powershell-install] weitere Informationen.

---

## <a name="update-the-manifest-file"></a>Aktualisieren der Manifestdatei

In diesen Tutorials speichert eine Azure Container Registry (ACR)-Instanz das Containerimage für die Beispielanwendung. Zum Bereitstellen der Anwendung müssen Sie den Imagenamen in der Kubernetes-Manifestdatei aktualisieren, sodass er den ACR-Anmeldeservernamen enthält.

### <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Rufen Sie den ACR-Anmeldeservernamen mit dem Befehl [az acr list][az-acr-list] wie folgt auf:

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

### <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

Rufen Sie den Namen des ACR-Anmeldeservers mit dem Cmdlet [Get-AzContainerRegistry][get-azcontainerregistry] wie folgt ab:

```azurepowershell
(Get-AzContainerRegistry -ResourceGroupName myResourceGroup -Name <acrName>).LoginServer
```

---

Die im ersten Tutorial geklonte Beispielmanifestdatei aus dem Git-Repository verwendet den Anmeldeservernamen *microsoft*. Vergewissern Sie sich, dass Sie sich im geklonten *azure-voting-app-Redis*-Verzeichnis befinden, und öffnen Sie dann die Manifestdatei mit einem Text-Editor, z. B. `vi`:

```console
vi azure-vote-all-in-one-redis.yaml
```

Ersetzen Sie *microsoft* durch Ihren ACR-Anmeldeservernamen. Der Imagename befindet sich in Zeile 60 der Manifestdatei. Im folgenden Beispiel wird der standardmäßige Imagename angezeigt:

```yaml
containers:
- name: azure-vote-front
  image: mcr.microsoft.com/azuredocs/azure-vote-front:v1
```

Geben Sie Ihren eigenen ACR-Anmeldeservernamen an, sodass Ihre Manifestdatei wie im folgenden Beispiel aussieht:

```yaml
containers:
- name: azure-vote-front
  image: <acrName>.azurecr.io/azure-vote-front:v1
```

Speichern und schließen Sie die Datei. Verwenden Sie in `vi` den Befehl `:wq`.

## <a name="deploy-the-application"></a>Bereitstellen der Anwendung

Verwenden Sie zum Bereitstellen Ihrer Anwendung den Befehl [kubectl apply][kubectl-apply]. Dieser Befehl analysiert die Manifestdatei und erstellt die definierten Kubernetes-Objekte. Geben Sie die Beispielmanifestdatei wie im folgenden Beispiel an:

```console
kubectl apply -f azure-vote-all-in-one-redis.yaml
```

Die folgende Beispielausgabe zeigt die Ressourcen, die erfolgreich im AKS-Cluster erstellt wurden:

```console
$ kubectl apply -f azure-vote-all-in-one-redis.yaml

deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-the-application"></a>Testen der Anwendung

Wenn die Anwendung ausgeführt wird, macht ein Kubernetes-Dienst das Anwendungs-Front-End im Internet verfügbar. Dieser Vorgang kann einige Minuten dauern.

Verwenden Sie zum Überwachen des Fortschritts den Befehl [kubectl get service][kubectl-get] mit dem Argument `--watch`.

```console
kubectl get service azure-vote-front --watch
```

Die *EXTERNAL-IP*-Adresse für den Dienst *azure-vote-front* wird zunächst als *ausstehend* angezeigt:

```output
azure-vote-front   LoadBalancer   10.0.34.242   <pending>     80:30676/TCP   5s
```

Sobald die externe IP-Adresse (*EXTERNAL-IP*) von *ausstehend* in eine tatsächliche öffentliche IP-Adresse geändert wurde, wird, verwenden Sie `CTRL-C`, um die `kubectl`-Überwachung zu beenden. Die folgende Beispielausgabe zeigt eine gültige öffentliche IP-Adresse, die dem Dienst zugewiesen ist:

```output
azure-vote-front   LoadBalancer   10.0.34.242   52.179.23.131   80:30676/TCP   67s
```

Öffnen Sie die externe IP-Adresse Ihres Diensts in einem Webbrowser, um die Anwendung in Aktion zu sehen:

:::image type="content" source="./media/container-service-kubernetes-tutorials/azure-vote.png" alt-text="Screenshot: Ausführung der Azure-Abstimmungs-App mit dem Containerimage in einem AKS-Cluster, geöffnet in einem lokalen Webbrowser" lightbox="./media/container-service-kubernetes-tutorials/azure-vote.png":::

Wurde die Anwendung nicht geladen, liegt möglicherweise ein Autorisierungsproblem mit Ihrer Imageregistrierung vor. Verwenden Sie den Befehl `kubectl get pods`, um den Status Ihrer Container anzuzeigen. Können die Containerimages nicht gepullt werden, lesen Sie die Informationen unter [Authentifizieren bei Azure Container Registry aus Azure Kubernetes Service](cluster-container-registry-integration.md).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde eine Azure Vote-Beispielanwendung in einem Kubernetes-Cluster in AKS bereitgestellt. Sie haben Folgendes gelernt:

> [!div class="checklist"]
> * Aktualisieren einer Kubernetes-Manifestdatei
> * Ausführen einer Anwendung in Kubernetes
> * Testen der Anwendung

Fahren Sie mit dem nächsten Tutorial fort, wo Sie erfahren, wie eine Kubernetes-Anwendung und die zugrunde liegende Kubernetes-Infrastruktur skaliert werden.

> [!div class="nextstepaction"]
> [Scale Kubernetes application and infrastructure][aks-tutorial-scale] (Skalieren einer Kubernetes-Anwendung und -Infrastruktur)

<!-- LINKS - external -->
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-scale]: ./tutorial-kubernetes-scale.md
[az-acr-list]: /cli/azure/acr
[azure-cli-install]: /cli/azure/install-azure-cli
[kubernetes-concepts]: concepts-clusters-workloads.md
[kubernetes-service]: concepts-network.md#services
[azure-powershell-install]: /powershell/azure/install-az-ps
[get-azcontainerregistry]: /powershell/module/az.containerregistry/get-azcontainerregistry
