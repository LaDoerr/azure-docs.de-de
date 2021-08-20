---
title: Verbinden von F5 BIG-IP-Daten mit Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie F5 BIG-IP-Daten mit Azure Sentinel verbinden.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: d068223f-395e-46d6-bb94-7ca1afd3503c
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/30/2019
ms.author: yelevin
ms.openlocfilehash: aae69e8ff7cb0c3002bb9333f6d7685b9e247a28
ms.sourcegitcommit: 05dd6452632e00645ec0716a5943c7ac6c9bec7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2021
ms.locfileid: "122351163"
---
# <a name="connect-your-f5-big-ip-appliance"></a>Verbinden Ihrer F5 BIG-IP-Appliance 

Mit dem Connector für F5 BIG-IP können Sie alle F5 BIG-IP-Protokolle einfach mit Azure Sentinel verbinden, um Arbeitsmappen anzuzeigen, benutzerdefinierte Warnungen zu erstellen und Untersuchungen zu verbessern. Dadurch erhalten Sie einen besseren Einblick in das Netzwerk Ihrer Organisation und bessere Möglichkeiten für Sicherheitsvorgänge. Die Integration von F5 BIG-IP und Azure Sentinel stützt sich auf die REST-API.

[!INCLUDE [reference-to-feature-availability](includes/reference-to-feature-availability.md)]

> [!NOTE]
> Daten werden am geografischen Standort des Arbeitsbereichs gespeichert, in dem Sie Azure Sentinel ausführen.

## <a name="configure-and-connect-f5-big-ip"></a>Konfigurieren und Verbinden von F5 BIG-IP 

F5 BIG-IP kann in Azure Sentinel integriert werden und Protokolle dann direkt in Azure Sentinel exportieren.

1. Klicken Sie im Azure Sentinel-Portal auf **Datenconnectors**, und wählen Sie **F5 BIG-IP** und dann **Connectorseite öffnen** aus. 
1. Zum Verbinden von F5 BIG-IP müssen Sie eine JSON-Deklaration an den API-Endpunkt des Systems senden. Anweisungen hierzu finden Sie unter [Integrieren von F5 BIG-IP in Azure Sentinel](https://devcentral.f5.com/s/articles/Integrating-the-F5-BIGIP-with-Azure-Sentinel).
8. Kopieren Sie auf der Seite „F5 BIG-IP-Connector“ die Arbeitsbereichs-ID und den Primärschlüssel, und fügen Sie diese Werte wie unter [Streamen von Daten an Azure Log Analytics](https://devcentral.f5.com/s/articles/Integrating-the-F5-BIGIP-with-Azure-Sentinel#streaming-data-to-azure-log-analytics) beschrieben ein.
1. Nachdem Sie die Anweisungen für F5 BIG-IP ausgeführt haben, werden auf der Seite „Azure Sentinel-Connector“ die verbundenen Datentypen angezeigt.
1. Um das relevante Schema in Log Analytics für die F5 BIG-IP-Ereignisse zu verwenden, suchen Sie nach **F5Telemetry_LTM_CL-**, **F5Telemetry_system_CL** und **F5Telemetry_ASM_CL**.


## <a name="validate-connectivity"></a>Überprüfen der Konnektivität

Es kann bis zu 20 Minuten dauern, bis Ihre Protokolle in Log Analytics angezeigt werden. 



## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument wurde beschrieben, wie Sie F5 BIG-IP mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](detect-threats-built-in.md).
- [Verwenden Sie Arbeitsmappen](monitor-your-data.md), um Ihre Daten zu überwachen.