---
title: Bereitstellen eines Azure Arc-Datencontrollers vom Azure Portal | Direkter Verbindungsmodus
description: Hier wird erläutert, wie der Datencontroller im direkten Verbindungsmodus vom Microsoft Azure-Portal bereitgestellt wird.
author: dnethi
ms.author: dinethi
ms.reviewer: mikeray
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
ms.date: 07/13/2021
ms.topic: overview
ms.openlocfilehash: c1c72ea44dc392e169f505c4820f99affdc82615
ms.sourcegitcommit: 3941df51ce4fca760797fa4e09216fcfb5d2d8f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2021
ms.locfileid: "114602521"
---
#  <a name="create-azure-arc-data-controller-from-azure-portal---direct-connectivity-mode"></a>Bereitstellen eines Azure Arc-Datencontrollers vom Microsoft Azure-Portal im direkten Verbindungsmodus

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

In diesem Artikel wird beschrieben, wie Sie den Azure Arc-Datencontroller während der aktuellen Vorschau dieses Features im direkten Verbindungsmodus bereitstellen. 

## <a name="complete-prerequisites"></a>Erfüllen der Voraussetzungen

Vergewissern Sie sich zunächst, dass Sie die Voraussetzungen unter [Bereitstellen eines Datencontrollers: direkter Verbindungsmodus (Voraussetzungen)](create-data-controller-direct-prerequisites.md) erfüllen.

>[!NOTE]
>Sie müssen zunächst eine Arc-fähige Kubernetes-Datendiensterweiterung unter Verwendung der Azure CLI bereitstellen.
>
>```azurecli
>az k8s-extension create -c "{connected_cluster_name}" -g "{resource_group_name}" --name "arcdataservices" --cluster-type "connectedClusters" --extension-type "microsoft.arcdataservices" --scope "cluster" --release-namespace {namespace} --config "Microsoft.CustomLocation.ServiceAccount=sa-bootstrapper"
>```


## <a name="deploy-azure-arc-data-controller"></a>Bereitstellen eines Azure Arc-Datencontrollers

Der Azure Arc Datencontroller-Erstellungsablauf kann über das Azure-Portal auf eine der folgenden Arten gestartet werden:

- Suchen Sie in der Suchleiste im Azure-Portal nach „Azure Arc-Datencontroller“ und wählen Sie „+ Erstellen“ aus
- Auf der Übersichtsseite Ihres Azure Arc Kubernetes-Clusters,
  - Wählen Sie unter „Einstellungen“ die Option „Erweiterungen (Vorschau)“ aus.
  - Wählen Sie auf der Übersichtsseite Erweiterungen die Option „Hinzufügen“ und dann „Azure Arc-Datencontroller“ aus
  - Wählen Sie im Azure Arc-Datencontroller Marketplace-Katalog die Option „Erstellen“ aus
  
Jede dieser Aktionen sollte Sie zur Seite „Azure Arc-Voraussetzungen für den Datencontroller“ des Erstellungsablaufs führen.

- Stellen Sie sicher, das die Option „Direkter Konnektivitätsmodus“ im Azure Arc Kubernetes-Cluster“ aktiviert ist. Wählen Sie „Weiter“ und dann „Datencontroller-Details“ aus
- Wählen Sie auf der Seite **Datencontroller-Details** folgendes aus:
  - Wählen Sie das Azure-Abonnement und die Ressourcengruppe aus, in die der Azure Arc-Datencontroller projiziert wird.
  - Geben Sie einen **Namen** für den Datencontroller ein
  - Wählen Sie einen bereits erstellten **Benutzerdefinierten Speicherort** oder wählen Sie „Neu erstellen“, um einen neuen benutzerdefinierten Speicherort zu erstellen. Wenn Sie einen neuen benutzerdefinierten Speicherort erstellen möchten, geben Sie einen Namen für den neuen benutzerdefinierten Speicherort ein. Wählen Sie dann in der Dropdownliste den Azure Arc-fähigen Kubernetes-Cluster aus, und geben Sie einen Namespace ein, der dem neuen benutzerdefinierten Speicherort zugeordnet werden soll. Wählen Sie schließlich im Fenster „Neuen benutzerdefinierten Speicherort erstellen“ die Option „Erstellen“ aus. Erfahren Sie mehr über [benutzerdefinierte Speicherorte](../kubernetes/conceptual-custom-locations.md)
  - **Kubernetes-Konfiguration**: Wählen Sie in der Dropdownliste eine Kubernetes-Konfigurationsvorlage aus, die ihrer Kubernetes-Verteilung am besten entspricht. Wenn Sie ihre eigenen Einstellungen verwenden oder über ein benutzerdefiniertes Profil verfügen, das Sie verwenden möchten, wählen Sie in der Dropdownliste die Option „Benutzerdefinierte Vorlage“ aus. Geben Sie auf dem Blatt, das auf der rechten Seite geöffnet wird, die Details für die Docker-Anmeldeinformationen, die Repository-Informationen, das Imagetag, die Image-Pullrichtlinie, den Infrastrukturtyp und die Speichereinstellungen für die Daten, die Protokolle und deren Größe, den Diensttyp und die Ports für den Controller und den Verwaltungsproxy ein. Wählen Sie „Übernehmen“ aus, wenn alle erforderlichen Informationen bereitgestellt sind. Sie können auch Ihre eigene Vorlagendatei hochladen, indem Sie oben auf dem Blatt „Eine Vorlage hochladen (JSON)“ auswählen. Wenn Sie benutzerdefinierte Einstellungen verwenden und eine Kopie dieser Einstellungen herunterladen möchten, verwenden Sie hierzu „Diese Vorlage herunterladen (JSON)“. Erfahren Sie mehr über die [benutzerdefinierten Konfigurationsprofile](create-custom-configuration-template.md).
  - Wählen Sie den passenden **Diensttyp** für Ihre Umgebung aus
  - **Administratorkonto**: Geben Sie die Anmeldeinformationen für die Datencontroller-Anmeldung und das Kennwort ein
  - **Dienstprinzipal**: Geben Sie die Client-ID, die Mandanten-ID und die Geheimen Clientinformationen für das zu verwendende Dienstprinzipalkonto ein.
  - Klicken Sie auf die Schaltfläche „Weiter“ und dann „Zusätzliche Einstellungen“, um fortzufahren, nachdem alle erforderlichen Informationen bereitgestellt wurden.
- Auf der Seite mit den **Zusätzliche Einstellungen**:
  - Wenn Sie Ihre Protokolle automatisch in Azure Log Analytics hochladen möchten, geben Sie die Log Analytics-Arbeitsbereichs-ID und den gemeinsam genutzten Log Analytics-Zugriffsschlüssel ein
  - Wenn Sie Ihre Protokolle NICHT automatisch in Azure Log Analytics hochladen möchten, deaktivieren Sie das Kontrollkästchen „Hochladen von Protokollen aktivieren“.
  - Wählen Sie: „Weiter“ und dann „Tags“, um fortzufahren.
- Auf der Seite **Tags**, geben Sie die Namen und die Werte für Ihre Tags ein und wählen „Weiter“ und dann „Überprüfen + Erstellen“.
- Auf der Seite **Überprüfen + Erstellen** wird Ihnen die Zusammenfassung Ihrer Bereitstellung angezeigt. Stellen Sie sicher, dass alle Einstellungen korrekt sind und wählen Sie „Erstellen“ aus, um die Bereitstellung des Azure Arc-Datencontrollers zu starten.

## <a name="monitor-the-creation-from-azure-portal"></a>Das Überwachen der Erstellung über das Azure-Portal

Wenn Sie im vorherigen Schritt auf die Schaltfläche „Erstellen“ klicken, sollte die Übersichtsseite für die Azure-Bereitstellung gestartet werden, auf der der Fortschritt der Bereitstellung des Azure Arc-Datencontrollers angezeigt wird.

## <a name="monitor-the-creation-from-your-kubernetes-cluster"></a>Das Überwachen der Erstellung über Ihren Kubernetes-Cluster

Der Fortschritt der Bereitstellung des Azure Arc Datencontrollers kann wie folgt überwacht werden:

- Überprüfen Sie, ob die CRDs erstellt werden, indem Sie ```kubectl get crd ``` über Ihren Cluster ausführen  
- Überprüfen Sie, ob der Namespace erstellt wird, indem Sie ```kubectl get ns``` über Ihren Cluster ausführen
- Überprüfen Sie, ob der benutzerdefinierte Speicherort durch Ausführen ```az customlocation list --resource-group <resourcegroup> -o table``` von erstellt wird 
- Überprüfen Sie den Status der Pod-Bereitstellung, indem Sie ```kubectl get pods -ns <namespace>``` ausführen

## <a name="next-steps"></a>Nächste Schritte

[Erstellen einer Azure Arc-fähigen SQL-verwalteten Instanz](create-sql-managed-instance.md)

[Erstellen einer Azure Arc-fähigen Servergruppe für PostgreSQL Hyperscale](create-postgresql-hyperscale-server-group.md)
