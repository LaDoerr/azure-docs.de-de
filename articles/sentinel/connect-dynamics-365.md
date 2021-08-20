---
title: Herstellen einer Verbindung von Dynamics 365-Protokollen mit Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit dem Connector für Dynamics 365 Common Data Service-Aktivitäten (CDS) Informationen zu laufenden Administrator-, Benutzer- und Supportaktivitäten abrufen.
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
ms.date: 06/03/2021
ms.author: yelevin
ms.openlocfilehash: d8151ac8f0ae26bc3567ec3a2a533703b401a90f
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122340066"
---
# <a name="connect-dynamics-365-activity-logs-to-azure-sentinel"></a>Verbinden von Dynamics 365-Aktivitätsprotokollen mit Azure Sentinel

Der Connector für [Dynamics 365](/office365/servicedescriptions/microsoft-dynamics-365-online-service-description) Common Data Service-Aktivitäten (CDS) bietet Erkenntnisse zu Administrator-, Benutzer- und Supportaktivitäten und zu Protokollierungsereignissen von Microsoft Social Engagement. Indem Sie Dynamics 365 CRM-Protokolle mit Azure Sentinel verbinden, können Sie diese Daten in Arbeitsmappen anzeigen oder damit benutzerdefinierte Warnungen erstellen oder Ihren Untersuchungsprozess optimieren. Dieser neue Azure Sentinel-Connector sammelt die Dynamics CDS-Daten über die Office-Verwaltungs-API. Weitere Informationen zu den Dynamics CDS-Aktivitäten, die in Power Platform überwacht werden, finden Sie unter [Aktivieren und Verwenden der Aktivitätsprotokollierung](/power-platform/admin/enable-use-comprehensive-auditing).

[!INCLUDE [reference-to-feature-availability](includes/reference-to-feature-availability.md)]

## <a name="prerequisites"></a>Voraussetzungen

- Sie benötigen Lese- und Schreibberechtigungen für Ihren Azure Sentinel-Arbeitsbereich.

- Sie benötigen eine [Microsoft Dynamics 365-Produktionslizenz](/office365/servicedescriptions/microsoft-dynamics-365-online-service-description). Dieser Connector ist für Sandboxumgebungen nicht verfügbar.
    - Für die Aktivitätsprotokollierung ist ein Microsoft 365 Enterprise [E3- oder E5](/power-platform/admin/enable-use-comprehensive-auditing#requirements)-Abonnement erforderlich.

- So rufen Sie Daten von der Office-Verwaltungs-API ab
    - Sie müssen ein globaler Administrator Ihres Mandanten sein.

    - Die [Office-Überwachungsprotokollierung](/office365/servicedescriptions/office-365-platform-service-description/office-365-securitycompliance-center) muss im [Office Security and Compliance Center](/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance) aktiviert werden.

## <a name="enable-the-dynamics-365-activities-data-connector"></a>Aktivieren des Datenconnectors für Dynamics 365-Aktivitäten

1. Klicken Sie im Azure Sentinel-Navigationsmenü auf **Data connectors** (Datenconnectors).

1. Wählen Sie im **Katalog für Datenconnectors** die Option **Dynamics 365** und dann im Vorschaubereich **Connectorseite öffnen** aus.

1. Klicken Sie auf der Registerkarte **Anweisungen** unter **Konfiguration** auf **Verbinden**. 

    Nachdem der Connector aktiviert wurde, dauert es ca. 30 Minuten, bis eingehende Daten im Graph angezeigt werden. 

    Laut dem [Office-Überwachungsprotokoll im Compliance Center](/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance#requirements-to-search-the-audit-log) kann es nach dem Eintreten eines Ereignisses zwischen 30 Minuten und 24 Stunden dauern, bis der zugehörige Datensatz aus dem Überwachungsprotokoll in den Ergebnissen zurückgegeben wird.

1. Sie finden die Microsoft Dynamics-Überwachungsprotokolle in der Tabelle `Dynamics365Activity`. Weitere Informationen finden Sie in der [Schemareferenz](/azure/azure-monitor/reference/tables/dynamics365activity) der Tabelle.

## <a name="querying-the-data"></a>Abfragen der Daten

1. Wählen Sie im Azure Sentinel-Navigationsmenü die Option **Protokolle** aus.

1. Führen Sie die folgende Abfrage aus, um sicherzustellen, dass Protokolle eintreffen:

    ```kusto
    Dynamics365Activity
    | take 10
    ```


## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument haben Sie erfahren, wie Sie Dynamics 365-Aktivitätsdaten mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](get-visibility.md).
- Beginnen Sie mit dem Erkennen von Bedrohungen mit Azure Sentinel mithilfe von [integrierten](detect-threats-built-in.md) oder [benutzerdefinierten](detect-threats-custom.md) Regeln.
