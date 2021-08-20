---
title: Verknüpfen von Barracuda CloudGen Firewall mit Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Barracuda CloudGen Firewall mit Azure Sentinel verknüpfen.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/30/2019
ms.author: yelevin
ms.openlocfilehash: 1ff3d5481b02e587d993bb4a5d7abf6aecd22ee8
ms.sourcegitcommit: 05dd6452632e00645ec0716a5943c7ac6c9bec7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2021
ms.locfileid: "122351155"
---
# <a name="connect-barracuda-cloudgen-firewall"></a>Verknüpfen von Barracuda CloudGen Firewall

Der Barracuda CloudGen Firewall-Connector (CGFW) ermöglicht Ihnen, Ihre Barracuda CGFW-Protokolle auf einfache Weise mit Azure Sentinel zu verknüpfen, um Dashboards anzuzeigen, benutzerdefinierte Warnungen zu erstellen und die Untersuchung von Daten zu verbessern. So erhalten Sie einen besseren Einblick in das Netzwerk Ihrer Organisation und bessere Möglichkeiten für Sicherheitsvorgänge.

[!INCLUDE [reference-to-feature-availability](includes/reference-to-feature-availability.md)]

## <a name="prerequisites"></a>Voraussetzungen

- Lese- und Schreibberechtigungen für den Azure Sentinel-Arbeitsbereich.

- Barracuda CloudGen Firewall muss so konfiguriert werden, dass Protokolle über Syslog exportiert werden.

## <a name="connect-azure-sentinel-to-barracuda-cloudgen-firewall"></a>Verknüpfen von Azure Sentinel mit Barracuda CloudGen Firewall

1. Navigieren Sie im Azure-Portal zu **Azure Sentinel** > **Datenconnectors**, und wählen Sie dann den Connector **Barracuda CloudGen Firewall** aus.

1. Wählen Sie **Connectorseite öffnen** aus.

1. Befolgen Sie die Anweisungen auf der Seite **Barracuda CloudGen Firewall**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie gelernt, wie Sie Barracuda CloudGen Firewall mit Azure Sentinel verknüpfen. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:

- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](detect-threats-built-in.md).
- [Verwenden Sie Arbeitsmappen](monitor-your-data.md), um Ihre Daten zu überwachen.