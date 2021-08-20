---
title: Verbinden von Microsoft Defender für Office 365-Daten (vormals Office 365 ATP) mit Azure Sentinel | Microsoft-Dokumentation
description: Erfassen Sie Microsoft Defender für Office 365-Daten in Azure Sentinel, um Erkenntnisse zu gewinnen und Szenarien mit automatisierter Reaktion einzurichten.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/06/2020
ms.author: yelevin
ms.openlocfilehash: 7c4482f788014aec45f7c2e2ca9c4820bd80bb33
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122339446"
---
# <a name="connect-alerts-from-microsoft-defender-for-office-365"></a>Verbinden von Benachrichtigungen aus Microsoft Defender für Office 365 

> [!IMPORTANT]
>
> - **Microsoft Defender für Office 365** wurde früher als **Office 365 Advanced Threat Protection (ATP)** bezeichnet.
>
>     Möglicherweise wird der alte Name für einen bestimmten Zeitraum weiterhin im Produkt verwendet (einschließlich des Datenconnectors in Azure Sentinel).
>
> - Die Erfassung von Microsoft Defender für Office 365-Benachrichtigungen befindet sich derzeit in der öffentlichen Vorschauphase (Public Preview). Dieses Feature wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
 
[!INCLUDE [reference-to-feature-availability](includes/reference-to-feature-availability.md)]

[Microsoft Defender für Office 365](/office365/servicedescriptions/office-365-advanced-threat-protection-service-description) schützt Ihre Organisation vor Zero-Day-Angriffen und anderen modernen Bedrohungen durch unbekannte Schadsoftware in E-Mails, schädlichen URL-Links und Tools für die Zusammenarbeit. Indem Sie Microsoft Defender für Office 365-Benachrichtigungen in Azure Sentinel erfassen, können Sie Informationen zu Bedrohungen in Verbindung mit E-Mails, Dateifreigabe und URLs für Ihre Sicherheitsvorgänge nutzen. Auf diese Weise können Sie Sicherheitsereignisse dann in Ihrer gesamten Organisation umfassender analysieren und Playbooks für effektive und sofortige Reaktionen entwickeln.

Mit dem Connector werden die folgenden Benachrichtigungen importiert:

- Erkennung eines Klicks auf eine potenziell schädliche URL 

- Entfernung von E-Mails mit Schadsoftware nach der Zustellung

- Entfernung von E-Mails mit Phishing-URLs nach der Zustellung 

- Meldung von E-Mails als Schadsoftware oder Phishing-Angriff durch Benutzer 

- Erkennung verdächtiger Sendemuster für E-Mails 

- Verhinderung des Sendens von E-Mails durch Benutzer 

Diese Benachrichtigungen können von Office-Kunden im **Office Security and Compliance Center** angezeigt werden.

## <a name="prerequisites"></a>Voraussetzungen

- Sie benötigen Lese- und Schreibberechtigungen für den Azure Sentinel-Arbeitsbereich, wenn Sie den Connector aktivieren.

- Sie müssen ein globaler Administrator oder Sicherheitsadministrator für den Mandanten des Azure Sentinel-Arbeitsbereichs sein.

- Sie müssen über eine gültige Lizenz für [Office 365 ATP-Plan 2](/microsoft-365/security/office-365-security/office-365-atp#office-365-atp-plan-1-and-plan-2) verfügen (in Lizenzen für Office 365 E5, Office 365 A5 und Microsoft 365 E5 enthalten und separat käuflich erhältlich). 

## <a name="connect-to-microsoft-defender-for-office-365"></a>Herstellen einer Verbindung mit Microsoft Defender für Office 365

Wenn Microsoft Defender für Office 365 bereitgestellt wurde und Richtlinien konfiguriert wurden, können die Benachrichtigungen leicht in Azure Sentinel erfasst werden.

1. Wählen Sie im Navigationsmenü von Azure Sentinel **Datenconnectors** aus.

1. Wählen Sie im Connectorkatalog die Option **Microsoft Defender für Office 365** (ggf. noch als *Office 365 Advanced Threat Protection* bezeichnet) und dann die Option **Connectorseite öffnen** aus.

1. Klicken Sie im Abschnitt **Konfiguration** auf **Verbinden**. 

1. Klicken Sie im Abschnitt **Incident erstellen** auf **Aktivieren**.

1. Suchen Sie nach **SecurityAlert**, und geben Sie unter **Anbietername** den Namen **OATP** an, um das relevante Schema zum Abfragen von Office 365 ATP-Benachrichtigungen zu verwenden.

1. Wählen Sie die Registerkarte **Nächste Schritte** aus, um die Abfragebeispiele und Analyseregelvorlagen anzuzeigen und zu verwenden, die im Microsoft Defender für Office 365-Connector enthalten sind.

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument wurde beschrieben, wie Sie Microsoft Defender für Office 365 mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](./detect-threats-built-in.md).