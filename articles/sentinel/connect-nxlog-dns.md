---
title: Verbinden von NXLog Windows DNS Logs-Daten mit Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit dem NXLog DNS Logs-Datenconnector entsprechende Windows DNS-Ereignisse in Azure Sentinel pullen. Zeigen Sie Windows DNS-Daten in Arbeitsmappen an, erstellen Sie Warnungen, und verbessern Sie die Untersuchung.
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
ms.date: 03/02/2021
ms.author: yelevin
ms.openlocfilehash: b80141d79807aea89e328b166d419e583a646ad5
ms.sourcegitcommit: 05dd6452632e00645ec0716a5943c7ac6c9bec7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2021
ms.locfileid: "122343375"
---
# <a name="connect-your-nxlog-windows-dns-logs-to-azure-sentinel"></a>Verbinden von NXLog Windows DNS Logs mit Azure Sentinel

> [!IMPORTANT]
> Der NXLog DNS Logs-Connector befindet sich derzeit in der **VORSCHAUVERSION**. Die [zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) enthalten zusätzliche rechtliche Bedingungen, die für Azure-Features gelten, die sich in der Beta- oder Vorschauversion befinden bzw. anderweitig noch nicht zur allgemeinen Verfügbarkeit freigegeben sind.

[!INCLUDE [reference-to-feature-availability](includes/reference-to-feature-availability.md)]

Mit dem [NXLog DNS Logs](https://nxlog.co/documentation/nxlog-user-guide/windows-dns-server.html)-Connector können Sie ganz einfach alle Windows DNS Server-Ereignisse in Echtzeit nach Azure Sentinel exportieren und erhalten so einen Einblick in die Aktivität der Domänennamenserver in Ihrem Unternehmen. Er verwendet die leistungsstarke [Ereignisablaufverfolgung für Windows (ETW)](https://nxlog.co/documentation/nxlog-user-guide/windows-dns-server.html#dns_windows_etw), um sowohl Überwachungs- als auch Analyseereignisse für den DNS-Server in Echtzeit zu erfassen und unterstützt die Anreicherung von Ereignissen mit benutzerdefinierten Feldern. Die Integration zwischen NXLog DNS Logs und Azure Sentinel wird über die REST-API ermöglicht.

> [!NOTE]
> Daten werden am geografischen Standort des Arbeitsbereichs gespeichert, in dem Sie Azure Sentinel ausführen.

## <a name="configure-and-connect-nxlog-dns-logs"></a>Konfigurieren und Verbinden von NXLog DNS Logs

NXLog kann so konfiguriert werden, dass Ereignisse im JSON-Format direkt an Azure Sentinel gesendet werden.

1. Klicken Sie im Azure Sentinel-Portal auf **Datenconnectors**, und wählen Sie den Connector **NXLog DNS Logs** aus.

1. Wählen Sie **Connectorseite öffnen** aus.

1. Befolgen Sie die Schrittanleitungen im *NXLog-Benutzerleitfaden* im Thema zur Integration von [Microsoft Azure Sentinel](https://nxlog.co/documentation/nxlog-user-guide/sentinel.html), um die Weiterleitung über die REST-API zu konfigurieren.

## <a name="find-your-data"></a>Suchen von Daten

Nachdem eine erfolgreiche Verbindung hergestellt wurde, werden die Daten in **Protokolle** unter dem Abschnitt **Benutzerdefinierte Protokolle** in der Tabelle *DNS_Logs_CL* angezeigt.

## <a name="validate-connectivity"></a>Überprüfen der Konnektivität

Es kann bis zu 20 Minuten dauern, bis Ihre Protokolle in Log Analytics angezeigt werden.

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument haben Sie erfahren, wie Sie mit NXLog entsprechende Windows DNS-Protokolle in Azure Sentinel erfassen. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:

- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](detect-threats-built-in.md).
- [Verwenden Sie Arbeitsmappen](monitor-your-data.md), um Ihre Daten zu überwachen.