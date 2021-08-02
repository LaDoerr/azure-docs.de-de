---
title: Schützen von Arbeitsbereichsressourcen mit virtuellen Netzwerken (VNETs)
titleSuffix: Azure Machine Learning
description: Schützen Sie Arbeitsbereichsressourcen und Compute-Umgebungen von Azure Machine Learning mithilfe eines isolierten Azure Virtual Network (VNet).
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.reviewer: larryfr
ms.author: peterlu
author: peterclu
ms.date: 06/11/2021
ms.topic: how-to
ms.custom: devx-track-python, references_regions, contperf-fy21q1,contperf-fy21q4,FY21Q4-aml-seo-hack
ms.openlocfilehash: c5e5461163b28ff53e77121a8e48dc478887ea6c
ms.sourcegitcommit: 3bb9f8cee51e3b9c711679b460ab7b7363a62e6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2021
ms.locfileid: "112081063"
---
<!-- # Virtual network isolation and privacy overview -->
# <a name="secure-azure-machine-learning-workspace-resources-using-virtual-networks-vnets"></a>Schützen von Azure Machine Learning-Arbeitsbereichsressourcen mit virtuellen Netzwerken (VNets)

Schützen Sie Arbeitsbereichsressourcen und Compute-Umgebungen von Azure Machine Learning mithilfe virtueller Netzwerke (VNets). Hier wird anhand eines Beispielszenarios veranschaulicht, wie Sie ein vollständiges virtuelles Netzwerk konfigurieren.

Der Artikel ist Teil einer fünf teiligen Artikelreihe, in der Sie schrittweise durch die Absicherung eines Azure Machine Learning-Workflows geführt werden. Wir empfehlen dringend, erst diesen Übersichtsartikel zu lesen, um zunächst die Konzepte zu verstehen. 

Hier sind die übrigen Artikel der Reihe:

**1. Übersicht zu VNETs** > [2. Schützen des Arbeitsbereichs](how-to-secure-workspace-vnet.md) > [3. Schützen der Trainingsumgebung](how-to-secure-training-vnet.md) > [4. Schützen der Rückschlussumgebung](how-to-secure-inferencing-vnet.md) > [5. Aktivieren der Studio-Funktionalität](how-to-enable-studio-virtual-network.md)

## <a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird vorausgesetzt, dass Sie mit den folgenden Themen vertraut sind:
+ [Virtuelle Azure-Netzwerke](../virtual-network/virtual-networks-overview.md)
+ [IP-Netzwerke](../virtual-network/public-ip-addresses.md)
+ [Azure Private Link](how-to-configure-private-link.md)
+ [Netzwerksicherheitsgruppen (NSGs)](../virtual-network/network-security-groups-overview.md):
+ [Netzwerkfirewalls](../firewall/overview.md)
## <a name="example-scenario"></a>Beispielszenario

In diesem Abschnitt erfahren Sie, wie ein Netzwerkszenario grundsätzlich eingerichtet wird, um die Azure Machine Learning-Kommunikation mit privaten IP-Adressen zu schützen.

In der folgenden Tabelle sehen Sie in einer Gegenüberstellung, wie Dienste mit bzw. ohne VNET auf verschiedene Teile eines Azure Machine Learning-Netzwerks zugreifen.

| Szenario | Arbeitsbereich | Zugeordnete Ressourcen | Trainingscompute-Umgebung | Rückschlusscompute-Umgebung |
|-|-|-|-|-|-|
|**Kein virtuelles Netzwerk**| Öffentliche IP-Adresse | Öffentliche IP-Adresse | Öffentliche IP-Adresse | Öffentliche IP-Adresse |
|**Sichere Ressourcen in einem virtuellen Netzwerk**| Private IP-Adresse (privater Endpunkt) | Öffentliche IP-Adresse (Dienstendpunkt) <br> **oder** <br> Private IP-Adresse (privater Endpunkt) | Private IP-Adresse | Private IP-Adresse  | 

* **Arbeitsbereich:** Erstellen Sie einen privaten Endpunkt in Ihrem VNET, um im Arbeitsbereich eine Private Link-Verbindung herzustellen. Der private Endpunkt verbindet den Arbeitsbereich über mehrere private IP-Adressen mit dem VNET.
* **Zugehörige Ressourcen:** Stellen Sie mithilfe von Dienstendpunkten oder privaten Endpunkten eine Verbindung mit Arbeitsbereichsressourcen wie Azure Storage, Azure Key Vault oder Azure Container Services her.
    * **Dienstendpunkte** geben die Identität Ihres virtuellen Netzwerks gegenüber dem Azure-Dienst an. Nachdem Sie Dienstendpunkte in Ihrem virtuellen Netzwerk aktiviert haben, können Sie eine virtuelle Netzwerkregel hinzufügen, um die Azure-Dienstressourcen in Ihrem virtuellen Netzwerk zu schützen. Dienstendpunkte nutzen öffentliche IP-Adressen.
    * **Private Endpunkte** sind Netzwerkschnittstellen, die das Herstellen einer sicheren Verbindung mit einem Dienst gestatten, der über Private Link betrieben wird. Ein privater Endpunkt verwendet eine private IP-Adresse in Ihrem VNET und bindet den Dienst so effektiv in das VNET ein.
* **Trainingscomputezugriff:** Hiermit greifen Sie sicher auf Computeziele zu, die zu Trainingszwecken verwendet werden, wie beispielsweise Azure Machine Learning-Compute-Instanz oder Azure Machine Learning-Computecluster. 
* **Rückschlusscomputezugriff:** Hiermit greifen Sie mit privaten IP-Adressen auf AKS-Computecluster (Azure Kubernetes Services) zu.


In den nächsten fünf Abschnitten wird gezeigt, wie Sie das oben beschriebene Netzwerkszenario absichern. Zum Schützen Ihres Netzwerks müssen Sie folgende Schritte ausführen:

1. [**Arbeitsbereich und zugehörige Ressourcen schützen**](#secure-the-workspace-and-associated-resources)
1. [**Trainingsumgebung schützen**](#secure-the-training-environment)
1. [**Rückschlussumgebung schützen**](#secure-the-inferencing-environment)
1. [**Studio-Funktionalität aktivieren**](#optional-enable-studio-functionality) (optional)
1. [**Firewalleinstellungen**](#configure-firewall-settings) konfigurieren
1. [DNS-Namensauflösung](#custom-dns) konfigurieren
## <a name="secure-the-workspace-and-associated-resources"></a>Schützen des Arbeitsbereichs und zugehöriger Ressourcen

Führen Sie die folgenden Schritte aus, um Ihren Arbeitsbereich und zugehörige Ressourcen zu schützen. Danach können Ihre Dienste im virtuellen Netzwerk kommunizieren.

1. Erstellen Sie einen [Arbeitsbereich mit Private Link-Unterstützung](how-to-secure-workspace-vnet.md#secure-the-workspace-with-private-endpoint), um die Kommunikation zwischen Ihrem VNET und dem Arbeitsbereich zu ermöglichen.
1. Fügen Sie dem virtuellen Netzwerk die folgenden Dienste hinzu. Verwenden Sie dazu _entweder_ einen __Dienstendpunkt__ oder einen __privaten Endpunkt__. Gewähren Sie außerdem vertrauenswürdigen Microsoft-Diensten Zugriff auf diese Dienste:
    
    | Dienst | Endpunktinformationen | Zulassen vertrauenswürdiger Informationen |
    | ----- | ----- | ----- |
    | __Azure Key Vault__| [Dienstendpunkt](../key-vault/general/overview-vnet-service-endpoints.md)</br>[Privater Endpunkt](../key-vault/general/private-link-service.md) | [Erlauben der Umgehung dieser Firewall für vertrauenswürdige Microsoft-Dienste](how-to-secure-workspace-vnet.md#secure-azure-key-vault) |
    | __Azure Storage-Konto__ | [Dienstendpunkt](how-to-secure-workspace-vnet.md#secure-azure-storage-accounts-with-service-endpoints)</br>[Privater Endpunkt](how-to-secure-workspace-vnet.md#secure-azure-storage-accounts-with-private-endpoints) | [Gewähren von Zugriff für vertrauenswürdige Azure-Dienste](../storage/common/storage-network-security.md#grant-access-to-trusted-azure-services) |
    | __Azure Container Registry__ | [Dienstendpunkt](how-to-secure-workspace-vnet.md#enable-azure-container-registry-acr)</br>[Privater Endpunkt](../container-registry/container-registry-private-link.md) | [Zulassen vertrauenswürdiger Dienste](../container-registry/allow-access-trusted-services.md) |


![Architekturdiagramm, das darstellt, wie der Arbeitsbereich und die zugehörigen Ressourcen über Dienstendpunkte oder private Endpunkte in einem VNET miteinander kommunizieren](./media/how-to-network-security-overview/secure-workspace-resources.png)

Ausführliche Anweisungen zum Ausführen dieser Schritte finden Sie unter [Secure an Azure Machine Learning workspace](how-to-secure-workspace-vnet.md) (Schützen eines Azure Machine Learning-Arbeitsbereichs). 

### <a name="limitations"></a>Einschränkungen

Für das Schützen Ihres Arbeitsbereichs und zugehöriger Ressourcen in einem virtuellen Netzwerk gelten die folgenden Einschränkungen:
- Die Verwendung eines Azure Machine Learning-Arbeitsbereichs mit Private Link ist in den Regionen Azure Government und Azure China 21Vianet nicht verfügbar.
- Alle Ressourcen müssen sich innerhalb desselben VNET befinden. Sie können allerdings Subnetze innerhalb eines VNET einsetzen.

## <a name="secure-the-training-environment"></a>Schützen der Trainingsumgebung

In diesem Abschnitt erfahren Sie, wie Sie die Trainingsumgebung in Azure Machine Learning schützen. Außerdem wird beschrieben, wie Azure Machine Learning einen Trainingsauftrag durchführt. So können Sie besser verstehen, wie die Netzwerkkonfigurationen kooperieren.

Führen Sie die folgenden Schritte aus, um die Trainingsumgebung zu schützen:

1. [Erstellen Sie im virtuellen Netzwerk eine Azure Machine Learning-Compute-Instanz und einen Azure Machine Learning-Computercluster](how-to-secure-training-vnet.md#compute-instance), um den Trainingsauftrag auszuführen.
1. [Erlauben Sie die von Azure Batch eingehende Kommunikation](how-to-secure-training-vnet.md#mlcports), damit Batch Aufträge an Ihre Computeressourcen übermitteln kann. 

![Architekturdiagramm, das die Absicherung verwalteter Computecluster und -instanzen zeigt](./media/how-to-network-security-overview/secure-training-environment.png)

Eine ausführliche Anleitung zum Ausführen dieser Schritte finden Sie unter [Secure an Azure Machine Learning training environment with virtual networks](how-to-secure-training-vnet.md) (Schützen einer Trainingsumgebung mit VNETs). 

### <a name="example-training-job-submission"></a>Beispiel für die Übermittlung eines Trainingsauftrags 

In diesem Abschnitt erfahren Sie, wie Azure Machine Learning für eine sichere Kommunikation zwischen Diensten sorgt, um einen Trainingsauftrag zu übermitteln. Sie können daraus ersehen, wie die von Ihnen vorgenommenen Konfigurationen in der Summe eine sichere Kommunikation gewährleisten.

1. Der Client lädt Trainingsskripts und Trainingsdaten in Speicherkonten hoch, die mit einem Dienst- oder einem privaten Endpunkt geschützt sind.

1. Der Client sendet einen Trainingsauftrag über den privaten Endpunkt an den Azure Machine Learning-Arbeitsbereich.

1. Der Azure Batch-Dienst empfängt den Auftrag vom Arbeitsbereich und übergibt den Trainingsauftrag über den öffentlichen Lastenausgleich, der mit der Computeressource bereitgestellt wird, an die Compute-Umgebung. 

1. Die Computeressource erhält den Auftrag und startet das Training. Die Computeressource greift auf sichere Speicherkonten zu, um Trainingsdateien herunter- und die Ausgabe hochzuladen.

### <a name="limitations"></a>Einschränkungen

- Die Azure-Compute-Instanz und die Azure-Computecluster müssen sich im selben VNET, in derselben Region und im gleichen Abonnement wie der Arbeitsbereich und die zugehörigen Ressourcen befinden. 

## <a name="secure-the-inferencing-environment"></a>Schützen der Rückschlussumgebung

In diesem Abschnitt erfahren Sie, welche Möglichkeiten Ihnen zum Schützen einer Rückschlussumgebung zur Verfügung stehen. Es wird empfohlen, für umfangreiche Produktionsbereitstellungen AKS-Cluster (Azure Kubernetes Services) zu verwenden.

Für AKS-Cluster in einem virtuellen Netzwerk stehen Ihnen zwei Optionen bereit:

- Sie können einen AKS-Standardcluster bereitstellen oder ihn Ihrem VNET hinzufügen.
- Sie können Ihrem VNET einen privaten AKS-Cluster hinzufügen.

**AKS-Standardcluster** weisen eine Steuerungsebene mit öffentlichen IP-Adressen auf. Sie können entweder Ihrem VNET bei der Bereitstellung einen AKS-Standardcluster hinzufügen oder nach der Erstellung einen Cluster anfügen.

**Private AKS-Cluster** weisen eine Steuerungsebene auf, auf die nur über private IP-Adressen zugegriffen werden kann. Private AKS-Cluster müssen nach der Erstellung des Clusters angefügt werden.

Eine ausführliche Anleitung zum Hinzufügen von Standard-und privaten Clustern finden Sie unter [Secure an inferencing environment](how-to-secure-inferencing-vnet.md) (Schützen einer Rückschlussumgebung). 

Das folgende Netzwerkdiagramm zeigt einen geschützten Azure Machine Learning-Arbeitsbereich mit einem privaten AKS-Cluster, der an das virtuelle Netzwerk angefügt wurde.

![Architekturdiagramm, das Anfügen eines privaten AKS-Clusters an das virtuelle Netzwerk zeigt. Die AKS-Steuerungsebene wird außerhalb des Kunden-VNET platziert.](./media/how-to-network-security-overview/secure-inferencing-environment.png)

### <a name="limitations"></a>Einschränkungen
- AKS-Cluster müssen demselben VNET angehören wie der Arbeitsbereich und die zugehörigen Ressourcen. 

## <a name="optional-enable-public-access"></a>Optional: Aktivieren des öffentlichen Zugriffs

Sie können den Arbeitsbereich hinter einem VNet mithilfe eines privaten Endpunkts schützen und weiterhin den Zugriff über das öffentliche Internet zulassen. Die anfängliche Konfiguration entspricht der zum [Schützen von Arbeitsbereich und zugehörigen Ressourcen](#secure-the-workspace-and-associated-resources). 

Nachdem Sie den Arbeitsbereich mit einer privaten Verbindung geschützt haben, [aktivieren Sie den öffentlichen Zugriff](how-to-configure-private-link.md#enable-public-access). Anschließend können Sie über das öffentliche Internet und das VNet auf den Arbeitsbereich zugreifen.

### <a name="limitations"></a>Einschränkungen

- Wenn Sie Azure Machine Learning Studio über das öffentliche Internet verwenden, können einige Features wie der Designer möglicherweise nicht auf Ihre Daten zugreifen. Dieses Problem tritt auf, wenn die Daten in einem Dienst gespeichert werden, der hinter dem VNet geschützt ist. Dies gilt z. B. für ein Azure Storage-Konto.
## <a name="optional-enable-studio-functionality"></a>Aktivieren der Studio-Funktionalität (optional)

[Schützen des Arbeitsbereichs](#secure-the-workspace-and-associated-resources) > [Schützen der Trainingsumgebung](#secure-the-training-environment) > [Schützen der Rückschlussumgebung](#secure-the-inferencing-environment) > **Aktivieren der Studio-Funktionalität** > [Konfigurieren der Firewalleinstellungen](#configure-firewall-settings)

Wenn sich Ihr Speicher in einem VNet befindet, müssen Sie zunächst zusätzliche Konfigurationsschritte durchführen, um die volle Funktionalität im [Studio](overview-what-is-machine-learning-studio.md) zu aktivieren. Standardmäßig sind die folgenden Features deaktiviert:

* Vorschau der Daten im Studio.
* Visualisieren von Daten im Designer.
* Bereitstellen eines Modells im Designer.
* Senden eines AutoML-Experiments.
* Starten eines Beschriftungsprojekts.

Informationen zur Aktivierung der vollen Studio-Funktionalität innerhalb eines VNets finden Sie unter [Verwenden von Azure Machine Learning Studio in einem virtuellen Azure-Netzwerk](how-to-enable-studio-virtual-network.md#configure-data-access-in-the-studio). Das Studio unterstützt Speicherkonten entweder unter Verwendung von Dienstendpunkten oder privaten Endpunkten.

### <a name="limitations"></a>Einschränkungen

Von der [ML-gestützten Datenbeschriftung](how-to-create-labeling-projects.md#use-ml-assisted-data-labeling) werden keine Standardspeicherkonten unterstützt, die hinter einem virtuellen Netzwerk geschützt sind. Sie müssen ein nicht standardmäßiges Speicherkonto für die ML-unterstützte Datenbeschriftung verwenden. Beachten Sie, dass das nicht standardmäßige Speicherkonto hinter dem virtuellen Netzwerk gesichert werden kann. 

## <a name="configure-firewall-settings"></a>Konfigurieren der Firewalleinstellungen

Konfigurieren Sie Ihre Firewall für die Steuerung des Zugriffs auf Ihren Azure Machine Learning-Arbeitsbereich und das öffentliche Internet. Zwar empfehlen wir die Verwendung von Azure Firewall, aber Sie sollten Ihr Netzwerk auch mit anderen Firewallprodukten schützen können. Wenn Sie Fragen dazu haben, wie Sie die Kommunikation über die Firewall zulassen, lesen Sie die Dokumentation für die von Ihnen verwendete Firewall.

Weitere Informationen zu Firewalleinstellungen finden Sie unter [Verwenden des Arbeitsbereichs hinter einer Firewall für Azure Machine Learning](how-to-access-azureml-behind-firewall.md).

## <a name="custom-dns"></a>Benutzerdefinierter DNS

Wenn Sie eine benutzerdefinierte DNS-Lösung für Ihr virtuelles Netzwerk verwenden möchten, müssen Sie Hosteinträge für Ihren Arbeitsbereich hinzufügen.

Weitere Informationen zu den erforderlichen Domänennamen und IP-Adressen finden Sie unter [Verwenden eines Arbeitsbereichs mit einem benutzerdefinierten DNS-Server](how-to-custom-dns.md).

## <a name="next-steps"></a>Nächste Schritte

Dieser Artikel ist der erste Teil einer fünfteiligen Serie zu virtuellen Netzwerken. Weitere Informationen zum Schützen eines virtuellen Netzwerks finden Sie in den verbleibenden Artikeln:

* [Teil 2: Virtuelle Netzwerke im Überblick](how-to-secure-workspace-vnet.md)
* [Teil 3: Schützen der Trainingsumgebung](how-to-secure-training-vnet.md)
* [Teil 4: Schützen der Rückschlussumgebung](how-to-secure-inferencing-vnet.md)
* [Teil 5: Verwenden von Studio in einem virtuellen Netzwerk](how-to-enable-studio-virtual-network.md)

Sehen Sie sich ebenso den Artikel zur Verwendung des [benutzerdefinierten DNS](how-to-custom-dns.md) für die Namensauflösung an.