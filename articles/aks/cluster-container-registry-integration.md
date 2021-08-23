---
title: Integrieren von Azure Container Registry in Azure Kubernetes Service (AKS)
description: Erfahren Sie, wie Sie Azure Kubernetes Service (AKS) und Azure Container Registry integrieren
services: container-service
manager: gwallace
ms.topic: article
ms.date: 01/08/2021
ms.openlocfilehash: 850586db4edf721981315c67317790429dd67d64
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110465988"
---
# <a name="authenticate-with-azure-container-registry-from-azure-kubernetes-service"></a>Authentifizieren per Azure Container Registry über Azure Kubernetes Service

Wenn Sie Azure Container Registry (ACR) mit dem Azure Kubernetes Service (AKS) nutzen, ist es erforderlich, einen Authentifizierungsmechanismus einzurichten. Dieser Vorgang ist als Teil der Befehlszeilenschnittstelle und des Portals implementiert, indem ACR die erforderlichen Berechtigungen erteilt werden. In diesem Artikel werden Beispiele für die Konfiguration der Authentifizierung zwischen diesen beiden Azure-Diensten beschrieben. 

Sie können die AKS- und ACR-Integration mit einigen einfachen Befehlen über die Azure-Befehlszeilenschnittstelle einrichten. Diese Integration weist der verwalteten Identität, die dem AKS-Cluster zugeordnet ist, die AcrPull-Rolle zu.

> [!NOTE]
> In diesem Artikel wird die automatische Authentifizierung zwischen AKS und ACR beschrieben. Wenn Sie ein Image aus einer privaten externen Registrierung abrufen, verwenden Sie ein [Geheimnis für Imagepullvorgänge][Image Pull Secret].

## <a name="before-you-begin"></a>Voraussetzungen

Voraussetzungen für diese Beispiele sind:

* Rolle **Besitzer**, **Azure-Kontoadministrator** oder **Azure Co-Administrator** im **Azure-Abonnement**
* Azure-Befehlszeilenschnittstelle Version 2.7.0 oder höher

Wenn Sie vermeiden möchten, dass die Rolle **Besitzer**, **Azure-Kontoadministrator** oder **Azure Co-Administrator** benötigt wird, können Sie eine vorhandene verwaltete Identität zur Authentifizierung von ACR aus AKS verwenden. Weitere Informationen finden Sie unter [Verwenden einer verwalteten Azure-Identität für die Azure Container Registry-Authentifizierung](../container-registry/container-registry-authentication-managed-identity.md).

## <a name="create-a-new-aks-cluster-with-acr-integration"></a>Erstellen eines neuen AKS-Clusters mit ACR-Integration

Sie können die AKS- und ACR-Integration während der erstmaligen Erstellung Ihres AKS-Clusters einrichten.  Damit ein AKS-Cluster mit ACR interagieren kann, wird eine **verwaltete Azure Active Directory-Identität** verwendet. Mit dem folgenden CLI-Befehl können Sie eine vorhandene ACR-Instanz in Ihrem Abonnement autorisieren und die entsprechende **ACRPull**-Rolle für die verwaltete Identität konfigurieren. Geben Sie gültige Werte für die unten stehenden Parameter an.

```azurecli
# set this to the name of your Azure Container Registry.  It must be globally unique
MYACR=myContainerRegistry

# Run the following line to create an Azure Container Registry if you do not already have one
az acr create -n $MYACR -g myContainerRegistryResourceGroup --sku basic

# Create an AKS cluster with ACR integration
az aks create -n myAKSCluster -g myResourceGroup --generate-ssh-keys --attach-acr $MYACR
```

Alternativ können Sie den ACR-Namen mithilfe einer ACR-Ressourcen-ID angeben, die das folgende Format aufweist:

`/subscriptions/\<subscription-id\>/resourceGroups/\<resource-group-name\>/providers/Microsoft.ContainerRegistry/registries/\<name\>`

> [!NOTE]
> Wenn Sie einen ACR verwenden, der sich in einem anderen Abonnement als Ihr AKS-Cluster befindet, verwenden Sie beim Anfügen oder Trennen eines AKS-Clusters die ACR-Ressourcen-ID.

```azurecli
az aks create -n myAKSCluster -g myResourceGroup --generate-ssh-keys --attach-acr /subscriptions/<subscription-id>/resourceGroups/myContainerRegistryResourceGroup/providers/Microsoft.ContainerRegistry/registries/myContainerRegistry
```

Dieser Schritt kann mehrere Minuten in Anspruch nehmen.

## <a name="configure-acr-integration-for-existing-aks-clusters"></a>Konfigurieren der ACR-Integration für vorhandene AKS-Cluster

Integrieren Sie eine vorhandene ACR-Instanz in vorhandene AKS-Cluster, indem Sie wie unten gezeigt gültige Werte für **acr-name** oder **acr-resource-id** angeben.

```azurecli
az aks update -n myAKSCluster -g myResourceGroup --attach-acr <acr-name>
```

oder

```azurecli
az aks update -n myAKSCluster -g myResourceGroup --attach-acr <acr-resource-id>
```

> [!NOTE]
> Beim Ausführen von `az aks update --attach-acr` werden die Berechtigungen des Benutzers verwendet, der den Befehl zum Erstellen der ACR-Rollenzuweisung ausführt. Diese Rolle wird der verwalteten Kubelet-Identität zugewiesen. Weitere Informationen zu den verwalteten AKS-Identitäten finden Sie unter [Zusammenfassung der verwalteten Identitäten][summary-msi].

Sie können die Integration zwischen einer ACR und einem AKS-Cluster auch folgendermaßen entfernen:

```azurecli
az aks update -n myAKSCluster -g myResourceGroup --detach-acr <acr-name>
```

oder

```azurecli
az aks update -n myAKSCluster -g myResourceGroup --detach-acr <acr-resource-id>
```

## <a name="working-with-acr--aks"></a>Arbeiten mit ACR und AKS

### <a name="import-an-image-into-your-acr"></a>Importieren eines Images in Ihre ACR

Sie importieren ein Image aus dem Docker-Hub in Ihre ACR, indem Sie Folgendes ausführen:


```azurecli
az acr import  -n <acr-name> --source docker.io/library/nginx:latest --image nginx:v1
```

### <a name="deploy-the-sample-image-from-acr-to-aks"></a>Bereitstellen des Beispielimages aus ACR in AKS

Sicherstellen, dass Sie über die richtigen AKS-Anmeldeinformationen verfügen

```azurecli
az aks get-credentials -g myResourceGroup -n myAKSCluster
```

Erstellen Sie eine Datei namens **acr-nginx.yaml**, die Folgendes enthält: Ersetzen Sie **acr-name** durch den Ressourcennamen Ihrer Registrierung. Beispiel: *myContainerRegistry*.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx0-deployment
  labels:
    app: nginx0-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx0
  template:
    metadata:
      labels:
        app: nginx0
    spec:
      containers:
      - name: nginx
        image: <acr-name>.azurecr.io/nginx:v1
        ports:
        - containerPort: 80
```

Führen Sie als Nächstes diese Bereitstellung in Ihrem AKS-Cluster aus:

```console
kubectl apply -f acr-nginx.yaml
```

Sie können die Bereitstellung überwachen, indem Sie Folgendes ausführen:

```console
kubectl get pods
```

Sie sollten über zwei laufende Pods verfügen.

```output
NAME                                 READY   STATUS    RESTARTS   AGE
nginx0-deployment-669dfc4d4b-x74kr   1/1     Running   0          20s
nginx0-deployment-669dfc4d4b-xdpd6   1/1     Running   0          20s
```

### <a name="troubleshooting"></a>Problembehandlung
* Führen Sie den Befehl [az aks check-acr](/cli/azure/aks#az_aks_check_acr) aus, um zu überprüfen, ob über den AKS-Cluster auf die Registrierung zugegriffen werden kann.
* Weitere Informationen zur [ACR-Überwachung](../container-registry/monitor-service.md)
* Weitere Informationen zur [ACR-Integrität](../container-registry/container-registry-check-health.md)

<!-- LINKS - external -->
[AKS AKS CLI]: /cli/azure/aks#az_aks_create
[Image Pull secret]: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/

[summary-msi]: use-managed-identity.md#summary-of-managed-identities