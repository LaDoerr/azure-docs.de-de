---
title: Systemthemen in Azure Event Grid
description: Hier werden die Systemthemen in Azure Event Grid beschrieben.
ms.topic: conceptual
ms.date: 07/19/2021
ms.openlocfilehash: cb054b8085c422f56a6cf8c6cc492470aaa4be95
ms.sourcegitcommit: 7d63ce88bfe8188b1ae70c3d006a29068d066287
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2021
ms.locfileid: "114437032"
---
# <a name="system-topics-in-azure-event-grid"></a>Systemthemen in Azure Event Grid
Ein Systemthema in Event Grid stellt ein oder mehrere Ereignisse dar, die von Azure-Diensten wie Azure Storage und Azure Event Hubs veröffentlicht wurden. Beispielsweise kann ein Systemthema **alle Blobereignisse** oder nur für ein **bestimmtes Speicherkonto** veröffentlichte **Bloberstellungs-** und **Bloblöschungsereignisse** darstellen. Wenn in diesem Beispiel ein Blob in das Speicherkonto hochgeladen wird, veröffentlicht der Dienst Azure Storage ein **Bloberstellungsereignis** im Systemthema in Event Grid. Event Grid leitet dieses Ereignis dann an die [Abonnenten](event-handlers.md) des Themas weiter, die es empfangen und verarbeiten. 

> [!NOTE] 
> Nur Azure-Dienste können Ereignisse in Systemthemen veröffentlichen. Im Gegensatz zu benutzerdefinierten Themen und Domänen erhalten Sie daher keinen Endpunkt- oder Zugriffsschlüssel für das Veröffentlichen von Ereignissen.

## <a name="azure-services-that-support-system-topics"></a>Systemthemen unterstützende Azure-Dienste
Dies ist eine aktuelle Liste der Azure-Dienste, die das Erstellen von Systemthemen über sich unterstützen.

- [Azure API Management](event-schema-api-management.md)
- [Azure App Configuration](event-schema-app-configuration.md)
- [Azure App Service](event-schema-app-service.md)
- [Azure Blob Storage](event-schema-blob-storage.md)
- [Azure Communication Services](event-schema-communication-services.md) 
- [Azure Container Registry](event-schema-container-registry.md)
- [Azure Event Hubs](event-schema-event-hubs.md)
- [Azure FarmBeats](event-schema-farmbeats.md)
- [Azure IoT Hub](event-schema-iot-hub.md)
- [Azure Key Vault](event-schema-key-vault.md)
- [Azure Machine Learning](event-schema-machine-learning.md)
- [Azure Maps](event-schema-azure-maps.md)
- [Azure Media Services](event-schema-media-services.md)
- [Azure Policy](./event-schema-policy.md)
- [Azure-Ressourcengruppen](event-schema-resource-groups.md)
- [Azure Service Bus](event-schema-service-bus.md)
- [Azure SignalR](event-schema-azure-signalr.md)
- [Azure-Abonnements](event-schema-subscriptions.md)
- [Azure Cache for Redis](event-schema-azure-cache.md)

## <a name="system-topics-as-azure-resources"></a>Systemthemen als Azure-Ressourcen
Früher waren Systemthemen implizit und wurden der Einfachheit halber nicht verfügbar gemacht. Mittlerweile sind Systemthemen als Azure-Ressourcen sichtbar und bieten die folgenden Funktionen:

- [Anzeigen von Systemthemen im Azure-Portal](create-view-manage-system-topics.md#view-all-system-topics)
- Exportieren von Resource Manager-Vorlagen für Systemthemen und Ereignisabonnements im Azure-Portal
- [Einrichten von Diagnoseprotokollen für Systemthemen](enable-diagnostic-logs-topic.md#enable-diagnostic-logs-for-a-system-topic)
- Einrichten von Warnungen bei Veröffentlichungs- und Zustellungsfehlern 

## <a name="lifecycle-of-system-topics"></a>Lebenszyklus von Systemthemen
Sie können ein Systemthema auf zwei Arten erstellen: 

- Erstellen Sie ein [Ereignisabonnement für eine Azure-Ressource als Erweiterungsressource](/rest/api/eventgrid/version2020-06-01/eventsubscriptions/createorupdate). Hierbei wird automatisch ein Systemthema mit einem Namen im folgenden Format erstellt: `<Azure resource name>-<GUID>`. Das auf diese Weise erstellte Systemthema wird automatisch gelöscht, wenn das letzte Ereignisabonnement für das Thema gelöscht wird. 
- Erstellen Sie ein Systemthema für eine Azure-Ressource und dann ein Ereignisabonnement für dieses Systemthema. Wenn Sie diese Methode verwenden, können Sie einen Namen für das Systemthema angeben. Das Systemthema wird nicht automatisch gelöscht, wenn das letzte Ereignisabonnement gelöscht wird. Sie müssen es manuell löschen. 

    Wenn Sie das Azure-Portal verwenden, verwenden Sie immer diese Methode. Wenn Sie ein Ereignisabonnement über die Seite [**Ereignisse** einer Azure-Ressource](blob-event-quickstart-portal.md#subscribe-to-the-blob-storage) erstellen, wird zuerst das Systemthema erstellt und anschließend das Abonnement für das Thema. Über die Seite [**Event Grid System Topics**](create-view-manage-system-topics.md#create-a-system-topic) (Event Grid-Systemthemen) können Sie explizit zuerst ein Systemthema erstellen und dann ein Abonnement für dieses Thema. 

Wenn Sie die [CLI](create-view-manage-system-topics-cli.md), [REST](/rest/api/eventgrid/version2020-06-01/eventsubscriptions/createorupdate) oder eine [Azure Resource Manager-Vorlage](create-view-manage-system-topics-arm.md) verwenden, können Sie jede der oben genannten Methoden verwenden. Es wird empfohlen, dass Sie zuerst ein Systemthema erstellen und dann ein Abonnement für das Thema, da dies die aktuellste Methode zum Erstellen von Systemthemen ist.

Beim Erstellen von Systemthemen tritt ein Fehler auf, wenn Sie Azure-Richtlinien so eingerichtet haben, dass der Dienst Event Grid keine Systemthemen erstellen kann. Möglicherweise verfügen Sie z. B. über eine Richtlinie, die nur das Erstellen von bestimmten Ressourcentypen (z. B. Azure Storage oder Azure Event Hubs) im Abonnement zulässt. 

## <a name="location-and-resource-group-for-a-system-topic"></a>Ort und Ressourcengruppe für ein Systemthema
Für Azure-Ereignisquellen, die sich in einer bestimmten Region bzw. einem bestimmten Speicherort befinden, wird das Systemthema am gleichen Speicherort erstellt wie die Azure-Ereignisquelle. Wenn Sie beispielsweise ein Ereignisabonnement für Azure Blob Storage in der Region „USA, Osten“ erstellen, wird das Systemthema in „USA, Osten“ erstellt. Für globale Azure-Ereignisquellen wie Azure-Abonnements, Ressourcengruppen oder Azure Maps erstellt Event Grid das Systemthema an einem **globalen** Speicherort. 

Im Allgemeinen wird das Systemthema in derselben Ressourcengruppe erstellt, in der sich auch die Azure-Ereignisquelle befindet. Für Ereignisabonnements, die im Bereich des Azure-Abonnements erstellt wurden, wird das Systemthema in der Ressourcengruppe **Default-EventGrid** in der Region **USA, Westen 2** erstellt. Wenn die Ressourcengruppe nicht vorhanden ist, wird diese von Azure Event Grid vor dem Systemthema erstellt. 

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in folgenden Artikeln: 

- [Erstellen, Anzeigen und Verwalten von Event Grid-Systemthemen im Azure-Portal](create-view-manage-system-topics.md)
- [Erstellen, Anzeigen und Verwalten von Event Grid-Systemthemen über die Azure CLI](create-view-manage-system-topics-cli.md)
- [Erstellen von Systemthemen in Azure Event Grid mithilfe von Resource Manager-Vorlagen](create-view-manage-system-topics-arm.md)
