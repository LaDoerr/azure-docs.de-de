---
title: Neuigkeiten Azure Event Grid
description: Hier finden Sie Informationen zu den Neuerungen in Azure Event Grid, z. B. aktuelle Versionshinweise, bekannte Probleme, Fehlerbehebungen, veraltete Funktionen und anstehende Änderungen.
ms.topic: overview
ms.date: 04/27/2021
ms.openlocfilehash: f516a9896f98c96e6f9cd2c651d9edac86474d14
ms.sourcegitcommit: e7d500f8cef40ab3409736acd0893cad02e24fc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122068201"
---
# <a name="whats-new-in-azure-event-grid"></a>Welche Neuerungen gibt es in Azure Event Grid?

>Lassen Sie sich über Updates auf dieser Seite benachrichtigen, indem Sie die URL `https://docs.microsoft.com/api/search/rss?search=%22Release+notes+-+Azure+Event+Grid%22&locale=en-us` kopieren und in Ihren ![RSS-Symbol](./media/whats-new/feed-icon-16x16.png)-Feedreader einfügen.

Es werden fortlaufend Verbesserungen an Azure Event Grid vorgenommen. Dieser Artikel bietet Informationen zu den Funktionen, die bei neuen Releases hinzugefügt oder aktualisiert werden, damit Sie auf dem neuesten Stand der aktuellen Entwicklungen bleiben. 

## <a name="610-preview-2020-10"></a>6.1.0-preview (2020-10)
- [Verwaltete Identitäten für Systemthemen](enable-identity-system-topics.md)
- [Benutzerdefinierte Übermittlungseigenschaften](delivery-properties.md)
- [Speicherwarteschlange: Gültigkeitsdauer für Nachrichten](delivery-properties.md#configure-time-to-live-on-outgoing-events-to-azure-storage-queues)
- [Verbesserungen der erweiterten Filterung](event-filtering.md#advanced-filtering)
    - Unterstützung der Filterung nach Arraydaten in eingehenden Ereignissen
    - Zulassen der Filterung nach Kontextattributen für CloudEvents-Erweiterungen
    - Neue Operatoren
        - StringNotContains
        - StringNotBeginsWith
        - StringNotEndsWith
        - NumberInRange
        - NumberNotInRange
        - IsNullOrUndefined
        - IsNotNull
- [Zulassen von Transformationen von Event Grid-Schemas in CloudEvents 1.0-Schemas für benutzerdefinierte Themen und Domänen](cloudevents-schema.md#configure-event-grid-for-cloudevents)
        

## <a name="600-2020-06"></a>6.0.0 (2020-06)
- Hinzugefügte Unterstützung für neue allgemein verfügbare Dienst-API-Version 2020-06-01.
- Neue Features, die nun allgemein verfügbar sind:
    - [Eingabezuordnungen](input-mappings.md)
    - [Benutzerdefiniertes Eingabeschema](input-mappings.md)
    - [Cloud Event V10-Schema](cloud-event-schema.md)
    - [Service Bus-Thema als Ziel](handler-service-bus.md)
    - [Azure-Funktion als Ziel](handler-functions.md)
    - [Webhookbatchverarbeitung](./edge/delivery-output-batching.md)
    - [Sicherer Webhook (Azure Active Directory-Unterstützung)](secure-webhook-delivery.md)
    - [IP-Filterung](configure-firewall.md)
    - [Unterstützung für Private Link-Dienst](configure-private-endpoints.md)
    - [Schema für Ereignisbereitstellung](event-schema.md)

## <a name="532-preview-2020-05"></a>5.3.2-preview (2020-05)
- Dieses Release enthält zusätzliche Fehlerbehebungen, um die Qualität zu verbessern.
- Als Version 5.3.1-preview entspricht dieses Release der API-Version 2020-04-01-Preview, die die folgenden neuen Funktionen umfasst: 
    - [Unterstützung für IP-Filterung beim Veröffentlichen von Ereignissen in Domänen und Themen](configure-firewall.md)
    - [Partnerthemen](./partner-events-overview.md)
    - [Systemthemen als überwachte Ressourcen im Azure-Portal](system-topics.md)
    - [Ereignisbereitstellung mit verwalteter Dienstidentität](managed-service-identity.md) 
    - [Unterstützung für Private Link-Dienst](configure-private-endpoints.md)

## <a name="531-preview-2020-04"></a>5.3.1-preview (2020-04)
- Dieses Release enthält verschiedene Fehlerbehebungen, um die Qualität zu verbessern.
- Als Version 5.3.0-preview entspricht dieses Release der API-Version 2020-04-01-Preview, die die folgenden neuen Funktionen umfasst: 
    - [Unterstützung für IP-Filterung beim Veröffentlichen von Ereignissen in Domänen und Themen](configure-firewall.md)
    - [Partnerthemen](./partner-events-overview.md)
    - [Systemthemen als überwachte Ressourcen im Azure-Portal](system-topics.md)
    - [Ereignisbereitstellung mit verwalteter Dienstidentität](managed-service-identity.md) 
    - [Unterstützung für Private Link-Dienst](configure-private-endpoints.md)

## <a name="530-preview-2020-03"></a>5.3.0-preview (2020-03)
- Zusätzlich zu den Features, die bereits in Version 5.2.0-preview hinzugefügt wurden, werden neue Features eingeführt. 
- Als Version 5.2.0-preview entspricht dieses Release der API-Version 2020-04-01-Preview.
- Es fügt Unterstützung für die folgenden neuen Funktionen hinzu: 
    - [Unterstützung für IP-Filterung beim Veröffentlichen von Ereignissen in Domänen und Themen](configure-firewall.md)
    - [Partnerthemen](./partner-events-overview.md)
    - [Systemthemen als überwachte Ressourcen im Azure-Portal](system-topics.md)
    - [Ereignisbereitstellung mit verwalteter Dienstidentität](managed-service-identity.md) 
    - [Unterstützung für Private Link-Dienst](configure-private-endpoints.md)

## <a name="520-preview-2020-01"></a>5.2.0-preview (2020-01)
- Dieses Release entspricht der API-Version 2020-04-01-Preview.
- Es fügt Unterstützung für die folgende neue Funktion hinzu:
    - [Unterstützung für IP-Filterung beim Veröffentlichen von Ereignissen in Domänen und Themen](configure-firewall.md)

## <a name="500-2019-05"></a>5.0.0 (2019-05)
- Dieses Release entspricht der API-Version `2019-06-01`.
- Es fügt Unterstützung für die folgenden neuen Funktionen hinzu:
    * [Domänen](event-domains.md)
    * Paginierung und Suchfilter für Ressourcenlistenvorgänge. Ein Beispiel finden Sie unter [Themen: Auflisten nach Abonnement](/rest/api/eventgrid/version2021-06-01-preview/partner-namespaces/list-by-subscription).
    * [Service Bus-Warteschlange als Ziel](handler-service-bus.md)
    * [Erweiterte Filterung](event-filtering.md#advanced-filtering)

## <a name="410-preview-2019-03"></a>4.1.0-preview (2019-03)
- Dieses Release entspricht der API-Version 2019-02-01-preview.
- Es fügt Unterstützung für die folgenden neuen Funktionen hinzu:
    * Paginierung und Suchfilter für Ressourcenlistenvorgänge. Ein Beispiel finden Sie unter [Themen: Auflisten nach Abonnement](/rest/api/eventgrid/version2021-06-01-preview/partner-namespaces/list-by-subscription).
    * [Manuelles Erstellen/Löschen von Domänenthemen](how-to-event-domains.md)
    * [Service Bus-Warteschlange als Ziel](handler-service-bus.md)

## <a name="400-2018-12"></a>4.0.0 (2018-12)
- Dieses Release entspricht der stabilen API-Version `2019-01-01`.
- Es unterstützt die allgemeine Verfügbarkeit (GA) der folgenden Funktionen im Zusammenhang mit Ereignisabonnements:
    * [Unzustellbare Nachricht als Ziel](manage-event-delivery.md)
    * [Azure Storage-Warteschlange als Ziel](handler-storage-queues.md)
    * [Azure Relay: Hybridverbindung als Ziel](handler-relay-hybrid-connections.md)
    * [Manuelle Handshakeüberprüfung](webhook-event-delivery.md)
    * [Unterstützung für Wiederholungsrichtlinien](delivery-and-retry.md)
- Auf Features, die sich noch in der Vorschauphase befinden (z. B. [Event Grid-Domänen](event-domains.md) oder [Unterstützung erweiterter Filter](event-filtering.md#advanced-filtering)) kann weiterhin mithilfe der Version 3.0.1-preview des SDK zugegriffen werden.

## <a name="301-preview-2018-10"></a>3.0.1-preview (2018-10)
- Abhängigkeit von [Version 10.0.3 des Newtonsoft-NuGet-Pakets](https://www.nuget.org/packages/Newtonsoft.Json/10.0.3).

## <a name="300-preview-2018-10"></a>3.0.0-preview (2018-10)
- Dieses Release ist ein Vorschau-SDK für die neuen Features, die in der API-Version 2018-09-15-preview eingeführt wurden. Dieses Release umfasst Unterstützung für Folgendes:
    - [Domänen und Domänenthemen](event-domains.md)
    - Einführung eines [Ablaufdatums für Ereignisabonnement](concepts.md#event-subscription-expiration)
    - Einführung [erweiterter Filter](event-filtering.md#advanced-filtering) für Ereignisabonnements
    - Die stabile Version des SDK, die auf API-Version `2018-01-01` abzielt, ist weiterhin als Version 1.3.0 vorhanden.

## <a name="next-steps"></a>Nächste Schritte
