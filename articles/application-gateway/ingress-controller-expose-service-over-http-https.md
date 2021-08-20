---
title: Bereitstellen eines AKS-Diensts über HTTP oder HTTPS mit Application Gateway
description: Dieser Artikel enthält Informationen zum Bereitstellen eines AKS-Diensts über HTTP oder HTTPS mithilfe von Application Gateway.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: how-to
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: 2764624d4f29432c10e0e7aa3ab49bb5a678d5e3
ms.sourcegitcommit: 2d412ea97cad0a2f66c434794429ea80da9d65aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2021
ms.locfileid: "122350304"
---
# <a name="expose-an-aks-service-over-http-or-https-using-application-gateway"></a>Bereitstellen eines AKS-Diensts über HTTP oder HTTPS mit Application Gateway 

Diese Tutorials helfen dabei, die Verwendung von [Kubernetes-Eingangsressourcen](https://kubernetes.io/docs/concepts/services-networking/ingress/) zu veranschaulichen, um einen Kubernetes-Beispieldienst über [Azure Application Gateway](https://azure.microsoft.com/services/application-gateway/) über HTTP oder HTTPS bereitzustellen.

## <a name="prerequisites"></a>Voraussetzungen

- Installiertes `ingress-azure`-Helm-Diagramm.
  - [**Greenfield-Bereitstellung:**](ingress-controller-install-new.md) Wenn Sie von Grund auf neu beginnen, lesen Sie diese Installationsanleitungen, in denen die Schritte zum Bereitstellen eines AKS-Clusters mit Application Gateway und zum Installieren des Application Gateway-Eingangsdatencontrollers im AKS-Cluster erläutert werden.
  - [**Brownfield-Bereitstellung:**](ingress-controller-install-existing.md) Wenn Sie über einen vorhandenen AKS-Cluster und eine Application Gateway-Instanz verfügen, lesen Sie diese Anleitungen zum Installieren des Application Gateway-Eingangsdatencontrollers im AKS-Cluster.
- Wenn Sie HTTPS für diese Anwendung verwenden möchten, benötigen Sie ein x509-Zertifikat und den zugehörigen privaten Schlüssel.

## <a name="deploy-guestbook-application"></a>Bereitstellen der `guestbook`-Anwendung

Bei der Guestbook-Anwendung handelt es sich um eine kanonische Kubernetes-Anwendung, die aus einem Web-UI-Front-End, einem Back-End und einer Redis-Datenbank besteht. Standardmäßig stellt `guestbook` die Anwendung über einen Dienst namens `frontend` an Port `80` bereit. Ohne eine Kubernetes-Eingangsressource kann auf den Dienst nicht von außerhalb des AKS-Clusters zugegriffen werden. Wir verwenden die Anwendung und richten Eingangsressourcen ein, um über HTTP und HTTPS auf die Anwendung zuzugreifen.

Befolgen Sie die nachstehenden Anweisungen zum Bereitstellen der Guestbook-Anwendung.

1. Laden Sie `guestbook-all-in-one.yaml`[hier](https://raw.githubusercontent.com/kubernetes/examples/master/guestbook/all-in-one/guestbook-all-in-one.yaml) herunter.
1. Stellen Sie `guestbook-all-in-one.yaml` in Ihrem AKS-Cluster bereit, indem Sie Folgendes ausführen:

  ```bash
  kubectl apply -f guestbook-all-in-one.yaml
  ```

Die `guestbook`-Anwendung wurde nun bereitgestellt.

## <a name="expose-services-over-http"></a>Bereitstellen von Diensten über HTTP

Um die Guestbook-Anwendung bereitzustellen, verwenden wir die folgende Eingangsressource:

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: guestbook
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
spec:
  rules:
  - http:
      paths:
      - backend:
          serviceName: frontend
          servicePort: 80
```

Die Eingangsressource stellt den `frontend`-Dienst der `guestbook-all-in-one`-Bereitstellung als Standard-Back-End der Application Gateway-Instanz bereit.

Speichern Sie die oben genannte Eingangsressource als `ing-guestbook.yaml`.

1. Stellen Sie `ing-guestbook.yaml` bereit, indem Sie Folgendes ausführen:

    ```bash
    kubectl apply -f ing-guestbook.yaml
    ```

1. Überprüfen Sie das Protokoll des Eingangscontrollers bezüglich des Bereitstellungsstatus.

Nun sollte die `guestbook`-Anwendung verfügbar sein. Sie können dies überprüfen, indem Sie die öffentliche Adresse der Application Gateway-Instanz besuchen.

## <a name="expose-services-over-https"></a>Bereitstellen von Diensten über HTTPS

### <a name="without-specified-hostname"></a>Ohne angegebenen Hostnamen

Ohne Angabe des Hostnamens ist der Guestbook-Dienst für alle Hostnamen verfügbar, die auf die Application Gateway-Instanz verweisen.

1. Vor dem Bereitstellen der Eingangsressource müssen Sie ein Kubernetes-Geheimnis erstellen, um das Zertifikat und den privaten Schlüssel zu hosten. Sie können ein Kubernetes-Geheimnis erstellen, indem Sie Folgendes ausführen:

    ```bash
    kubectl create secret tls <guestbook-secret-name> --key <path-to-key> --cert <path-to-cert>
    ```

1. Definieren Sie die folgende Eingangsressource. Geben Sie in der Eingangsressource den Namen des Geheimnisses im Abschnitt `secretName` an.

    ```yaml
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: guestbook
      annotations:
        kubernetes.io/ingress.class: azure/application-gateway
    spec:
      tls:
        - secretName: <guestbook-secret-name>
      rules:
      - http:
          paths:
          - backend:
              serviceName: frontend
              servicePort: 80
    ```

    > [!NOTE] 
    > Ersetzen Sie `<guestbook-secret-name>` in der obigen Eingangsressource durch den Namen Ihres Geheimnisses. Speichern Sie die oben angegebene Eingangsressource unter dem Dateinamen `ing-guestbook-tls.yaml`.

1. Stellen Sie „ing-guestbook-tls.yaml“ bereit, indem Sie Folgendes ausführen:

    ```bash
    kubectl apply -f ing-guestbook-tls.yaml
    ```

1. Überprüfen Sie das Protokoll des Eingangscontrollers bezüglich des Bereitstellungsstatus.

Nun ist die `guestbook`-Anwendung sowohl für HTTP als auch für HTTPS verfügbar.

### <a name="with-specified-hostname"></a>Mit angegebenem Hostnamen

Sie können auch den Hostnamen für die Eingangsressource angeben, um Multiplex für TLS-Konfigurationen und Dienste zu verwenden.
Wenn Sie den Hostnamen angeben, ist der Guestbook-Dienst nur auf dem angegebenen Host verfügbar.

1. Definieren Sie die folgende Eingangsressource.
    Geben Sie in der Eingangsressource den Namen des Geheimnisses im Abschnitt `secretName` an, und ersetzen Sie den Hostnamen im Abschnitt `hosts` entsprechend.

    ```yaml
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: guestbook
      annotations:
        kubernetes.io/ingress.class: azure/application-gateway
    spec:
      tls:
        - hosts:
          - <guestbook.contoso.com>
          secretName: <guestbook-secret-name>
      rules:
      - host: <guestbook.contoso.com>
        http:
          paths:
          - backend:
              serviceName: frontend
              servicePort: 80
    ```

1. Stellen Sie `ing-guestbook-tls-sni.yaml` bereit, indem Sie Folgendes ausführen:

    ```bash
    kubectl apply -f ing-guestbook-tls-sni.yaml
    ```

1. Überprüfen Sie das Protokoll des Eingangscontrollers bezüglich des Bereitstellungsstatus.

Nun ist die `guestbook`-Anwendung nur auf dem angegebenen Host (`<guestbook.contoso.com>` in diesem Beispiel) sowohl über HTTP als auch über HTTPS verfügbar.

## <a name="integrate-with-other-services"></a>Integration in andere Dienste

Die folgende Eingangsressource ermöglicht es Ihnen, zusätzliche Pfade in diese Eingangsressource einzufügen und diese Pfade an andere Dienste weiterzuleiten:

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: guestbook
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
spec:
  rules:
  - http:
      paths:
      - path: </other/*>
        backend:
          serviceName: <other-service>
          servicePort: 80
       - backend:
          serviceName: frontend
          servicePort: 80
```
