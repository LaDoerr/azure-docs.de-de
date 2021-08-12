---
title: 'Auswahl von Azure-Messagingdiensten: Event Grid, Event Hubs und Service Bus'
description: 'Enthält eine Beschreibung der drei Azure-Messagingdienste: Azure Event Grid, Event Hubs und Service Bus. Hier erfahren Sie, welcher Dienst für welche Szenarien zu empfehlen ist.'
ms.topic: overview
ms.date: 07/22/2021
ms.openlocfilehash: 91f0eb53421cf61275a09582621dc1b5bbb8d2d1
ms.sourcegitcommit: 7d63ce88bfe8188b1ae70c3d006a29068d066287
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2021
ms.locfileid: "114442698"
---
# <a name="choose-between-azure-messaging-services---event-grid-event-hubs-and-service-bus"></a>Auswahl von Azure-Messagingdiensten: Event Grid, Event Hubs und Service Bus

Azure bietet drei Dienste, die Sie bei der Übermittlung von Ereignissen oder Nachrichten innerhalb einer Lösung unterstützen. Folgende Dienste stehen zur Verfügung:

- Azure Event Grid
- Azure Event Hubs
- Azure Service Bus

Trotz einiger Gemeinsamkeiten ist jeder dieser Dienste für bestimmte Szenarien konzipiert. Dieser Artikel beschreibt die Unterschiede zwischen den Diensten und unterstützt Sie bei der Wahl eines passenden Diensts für Ihre Anwendung. In vielen Fällen ergänzen sich die Messagingdienste gegenseitig und können zusammen verwendet werden.

## <a name="event-vs-message-services"></a>Gegenüberstellung von Ereignis- und Nachrichtendiensten
Zwischen Diensten, die ein Ereignis übermitteln, und Diensten, die eine Nachricht übermitteln, besteht ein wichtiger Unterschied.

### <a name="event"></a>Ereignis
Ein Ereignis ist eine einfache Benachrichtigung über eine Bedingung oder eine Zustandsänderung. Der Verleger des Ereignisses hat keine Erwartungen hinsichtlich der Behandlung des Ereignisses. Der Consumer des Ereignisses entscheidet, was mit der Benachrichtigung getan wird. Ereignisse können eigenständige Einheiten oder Teil einer Reihe sein.

Eigenständige Ereignisse geben Statusänderungen an und erfordern Aktionen. Um den nächsten Schritt zu machen, muss der Consumer nur zu wissen, dass etwas passiert ist. Die Ereignisdaten geben Aufschluss darüber, was geschehen ist, enthalten aber nicht die Daten, die das Ereignis ausgelöst haben. Ein Ereignis kann beispielsweise Consumer darüber informieren, dass eine Datei erstellt wurde. Das Ereignis kann allgemeine Informationen zu der Datei enthalten, aber nicht die eigentliche Datei. Eigenständige Ereignisse eignen sich ideal für serverlose Lösungen, die skaliert werden müssen.

Reihenereignisse geben eine Bedingung an und können analysiert werden. Die Ereignisse sind nach Zeit sortiert und voneinander abhängig. Der Consumer benötigt die sortierte Reihe von Ereignissen, um zu analysieren, was passiert ist.

### <a name="message"></a>Meldung
Bei einer Nachricht handelt es sich um Rohdaten, die von einem Dienst erzeugt wurden und für die Nutzung oder Speicherung an anderer Stelle vorgesehen sind. Die Nachricht enthält die Daten, die die Nachrichtenpipeline ausgelöst haben. Der Verleger der Nachricht hat eine Erwartung dazu, wie der Consumer die Nachricht verarbeitet. Ein Vertrag besteht zwischen den beiden Seiten. Beispiel: Der Verleger sendet eine Nachricht mit den Rohdaten und erwartet, dass der Consumer eine Datei aus den Daten erstellt und eine Antwort sendet, wenn die Arbeit abgeschlossen ist.


## <a name="azure-event-grid"></a>Azure Event Grid
Event Grid ist ein Ereignisbackplane, das ereignisgesteuerte, reaktive Programmierung ermöglicht. Es verwendet das Veröffentlichen-Abonnieren-Modell. Herausgeber geben Ereignisse aus, haben aber keinen Einfluss darauf, wie die Ereignisse verarbeitet werden. Abonnenten entscheiden, welche Ereignisse sie behandeln möchten.

Event Grid ist tief in Azure-Dienste integriert und kann in Drittanbieterdienste integriert werden. Es vereinfacht die Nutzung von Ereignissen und senkt Kosten, da nicht ständig Abfragen durchgeführt werden müssen. Event Grid leitet effizient und zuverlässig Ereignisse von Azure-eigenen und Azure-fremden Ressourcen weiter. Die Ereignisse werden an registrierte Abonnentenendpunkte verteilt. Die Ereignismeldung enthält die Informationen, die Sie benötigen, um auf Änderungen in Diensten und Anwendungen zu reagieren. Event Grid ist keine Datenpipeline und übermittelt nicht das eigentliche Objekt, das aktualisiert wurde.

Das Repository weist die folgenden Merkmale auf:

- Dynamisch skalierbar
- Niedrige Kosten
- Serverlos
- Mindestens eine Übermittlung eines Ereignisses

Weitere Informationen finden Sie unter [Übersicht über Event Grid](overview.md).

## <a name="azure-event-hubs"></a>Azure Event Hubs
Bei Azure Event Hubs handelt es sich um eine Big Data-Streamingplattform und einen Ereigniserfassungsdienst. Mit diesem Dienst können Millionen von Ereignissen pro Sekunde empfangen und verarbeitet werden. Es vereinfacht die Erfassung, Aufbewahrung und Wiedergabe von Telemetrie- und Ereignisdatenstromdaten. Die Daten können aus vielen parallelen Quellen stammen. Mit Event Hubs können Telemetrie- und Ereignisdaten für verschiedene datenstromverarbeitende Infrastrukturen und Analysediensten verfügbar gemacht werden. Die Daten sind entweder als Datenströme oder als gebündelte Ereignisbatches verfügbar. Dieser Dienst bietet eine Einzellösung zum schnellen Datenabruf für die Verarbeitung in Echtzeit sowie für die wiederholte Wiedergabe gespeicherter Rohdaten. Er kann die Streamingdaten zur Verarbeitung und Analyse in einer Datei erfassen.

Das Repository weist die folgenden Merkmale auf:

- Geringe Wartezeit
- Empfangen und Verarbeiten von Millionen von Ereignissen pro Sekunde
- Mindestens eine Übermittlung eines Ereignisses

Weitere Informationen finden Sie unter [Übersicht über Event Hubs](../event-hubs/event-hubs-about.md).

## <a name="azure-service-bus"></a>Azure Service Bus
Bei Service Bus handelt es sich um einen vollständig verwalteten Nachrichtenbroker für Unternehmen mit Nachrichtenwarteschlangen und Veröffentlichen-Abonnieren-Themen. Der Dienst ist für Unternehmensanwendungen konzipiert, die Transaktionen, Sortierung, Duplikaterkennung und sofortige Konsistenz erfordern. Mit Service Bus können native Cloudanwendungen eine zuverlässige Zustandsübergangsverwaltung für Geschäftsprozesse bereitstellen. Nutzen Sie Azure Service Bus bei Verwendung wichtiger Nachrichten, die nicht verloren gehen oder dupliziert werden dürfen. Dieser Dienst ermöglicht auch eine äußerst sichere Kommunikation über Hybrid Cloud-Lösungen hinweg und kann vorhandene lokale Systeme mit Cloudlösungen verbinden.

Service Bus ist ein System für Brokermessaging. Es speichert Nachrichten in einem „Broker“ (z. B. in einer Warteschlange), bis die Empfängerseite für den Empfang der Nachrichten bereit ist. Das Repository weist die folgenden Merkmale auf:

- Zuverlässige asynchrone Nachrichtenübermittlung (abfragebasiertes Enterprise Messaging-as-a-Service)
- Erweiterte Messagingfeatures wie FIFO (First-In-First-Out), Batchverarbeitung/Sitzungen, Transaktionen, unzustellbare Nachrichten, zeitliche Steuerung, Routing und Filterung sowie Duplikaterkennung
- Mindestens eine Übermittlung einer Nachricht
- Optional: geordnete Übermittlung von Nachrichten

Weitere Informationen finden Sie unter [Übersicht über Service Bus](../service-bus-messaging/service-bus-messaging-overview.md).

## <a name="comparison-of-services"></a>Vergleich von Diensten

| Dienst | Zweck | type | Verwendung |
| ------- | ------- | ---- | ----------- |
| Event Grid | Reaktive Programmierung | Ereignisverteilung (eigenständig) | Reagieren auf Statusänderungen |
| Event Hubs | Big Data-Pipeline | Ereignisstreaming (Reihe) | Streamen von Telemetriedaten und verteilten Daten |
| Service Bus | Hochwertiges Unternehmensmessaging | Meldung | Auftragsverarbeitung und Finanztransaktionen |

## <a name="use-the-services-together"></a>Gemeinsame Verwendung der Dienste
In manchen Fällen werden Dienste parallel verwendet, um bestimmte Rollen zu erfüllen. Eine E-Commerce-Website kann beispielsweise mit Service Bus Bestellungen verarbeiten, mit Event Hubs Telemetriedaten der Website erfassen und mit Event Grid auf Ereignisse wie etwa den Versand eines Artikels reagieren.

In anderen Fällen können die Dienste zu einer Ereignis- und Datenpipeline verknüpft werden. Event Grid wird verwendet, um auf Ereignisse in den anderen Diensten zu reagieren. Ein Beispiel, bei dem Daten mithilfe von Event Grid und Event Hubs in ein Data Warehouse migriert werden, finden Sie unter [Streamen von Big Data in ein Data Warehouse](event-grid-event-hubs-integration.md). Die folgende Abbildung zeigt den Workflow zum Streamen der Daten:

:::image type="content" source="./media/compare-messaging-services/overview.png" alt-text="Abbildung der möglichen Verbindung von Event Hubs, Service Bus und Event Grid":::

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in folgenden Artikeln: 
- [Asynchrone Messagingoptionen in Azure](/azure/architecture/guide/technology-choices/messaging)
- [Events, Data Points, and Messages - Choosing the right Azure messaging service for your data](https://azure.microsoft.com/blog/events-data-points-and-messages-choosing-the-right-azure-messaging-service-for-your-data/) Ereignisse, Datenpunkte und Nachrichten: Der passende Azure-Messagingdienst für Ihre Daten)

