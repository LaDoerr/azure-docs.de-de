---
title: SMS-Konzepte in Azure Communication Services
titleSuffix: An Azure Communication Services concept document
description: Hier finden Sie Informationen zu SMS-Konzepten in Communication Services.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 06/30/2021
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: ab9a0b5c50bfbcf0cca3ab91072850420fcca2d0
ms.sourcegitcommit: 98308c4b775a049a4a035ccf60c8b163f86f04ca
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/30/2021
ms.locfileid: "113108025"
---
# <a name="sms-concepts"></a>SMS-Konzepte

[!INCLUDE [Regional Availability Notice](../../includes/regional-availability-include.md)]

Azure Communication Services ermöglicht das Senden und Empfangen von SMS-Textnachrichten unter Verwendung der Communication Services SMS SDKs. Diese SDKs können zur Unterstützung von Kundendienstszenarien, Terminerinnerungen, zweistufiger Authentifizierung und anderer Echtzeitkommunikationsanforderungen verwendet werden. Das SMS-Feature von Communication Services ermöglicht das zuverlässige Senden von Nachrichten sowie die Bereitstellung von Zustellbarkeits- und Antwortmetriken.

Zu den wichtigsten Features der Azure Communication Services SMS SDKs gehören folgende:

-  **Unkomplizierte Einrichtungsumgebung** zum Hinzufügen von SMS-Funktionen zu Ihren Anwendungen
- **Unterstützung von Hochgeschwindigkeitsnachrichten** über gebührenfreie Nummern für A2P-Anwendungsfälle (Application to Person) in den USA
- Unterstützung von **Massennachrichten**, um das gleichzeitige Senden von Nachrichten an mehrere Empfänger zu ermöglichen
- **Bidirektionale Konversationen** zur Unterstützung von Szenarien wie Kundensupport, Warnungen und Terminerinnerungen
- **Zuverlässige Zustellung** mit Zustellberichten in Echtzeit für Nachrichten, die von Ihrer Anwendung gesendet wurden
- **Analysen** zur Nachverfolgung von SMS-Verwendungsmustern
- **Unterstützung von Abmeldungen**, um Abmeldungen für gebührenfreie Nummern automatisch zu erkennen und zu berücksichtigen. Abmeldungen für gebührenfreie Nummern in den USA werden von US-Netzbetreibern vorgeschrieben und durchgesetzt.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Schnellstart: Senden einer SMS-Nachricht](../../quickstarts/telephony-sms/send.md)

Die Artikel zu den folgenden Themen könnten Sie auch interessieren:

- Machen Sie sich mit dem [SMS SDK](../telephony-sms/sdk-features.md) vertraut.
- Beziehen einer für SMS geeigneten [Telefonnummer](../../quickstarts/telephony-sms/get-phone-number.md)
- [Telefonnummerntypen in Azure Communication Services](../telephony-sms/plan-solution.md)
