---
title: Verbinden von Qualys Vulnerability Management-Daten mit Azure Sentinel| Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Qualys Vulnerability Management-Daten mit Azure Sentinel verbinden.
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
ms.date: 07/17/2020
ms.author: yelevin
ms.openlocfilehash: 111db39a1822e47fb8a14e10d3833fd58153f5b5
ms.sourcegitcommit: 05dd6452632e00645ec0716a5943c7ac6c9bec7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2021
ms.locfileid: "122356500"
---
# <a name="connect-your-qualys-vm-to-azure-sentinel-with-azure-function"></a>Verbinden Ihrer Qualys VM-Instanz mithilfe von Azure Function mit Azure Sentinel

> [!IMPORTANT]
> Der Qualys-VM-Datenconnector in Azure Sentinel ist derzeit als öffentliche Vorschau verfügbar.
> Dieses Feature wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[!INCLUDE [reference-to-feature-availability](includes/reference-to-feature-availability.md)]

Mit dem Connector für Qualys Vulnerability Management (VM) können Sie alle Protokolle Ihrer [Qualys VM](https://www.qualys.com/apps/vulnerability-management/)-Sicherheitslösungen einfach mit Azure Sentinel verbinden, um Dashboards anzuzeigen, benutzerdefinierte Warnungen zu erstellen und Untersuchungen zu verbessern. Bei der Integration von Qualys VM und Azure Sentinel wird Azure Functions verwendet, um Protokolldaten per REST-API zu pullen.

> [!NOTE]
> Daten werden am geografischen Standort des Arbeitsbereichs gespeichert, in dem Sie Azure Sentinel ausführen.

## <a name="configure-and-connect-qualys-vm"></a>Konfigurieren und Verbinden von Qualys VM

Azure Functions kann in Qualys VM integriert werden und Ereignisse und Protokolle direkt aus Qualys VM pullen und an Azure Sentinel weiterleiten.

1. Klicken Sie im Azure Sentinel-Portal auf **Datenconnectors**, und wählen Sie den Connector **Qualys Vulnerability Management** aus.

1. Wählen Sie **Connectorseite öffnen** aus.

1. Befolgen Sie die Anweisungen auf der Seite **Qualys Vulnerability Management**.

## <a name="find-your-data"></a>Suchen von Daten

Nachdem Sie eine Verbindung hergestellt haben, werden die Daten in Log Analytics in der Tabelle **QualysHostDetection_CL** aufgeführt.

## <a name="validate-connectivity"></a>Überprüfen der Konnektivität

Es kann bis zu 20 Minuten dauern, bis Ihre Protokolle in Log Analytics angezeigt werden.

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument haben Sie erfahren, wie Sie Qualys VM mithilfe von Azure-Funktions-Apps mit Azure Sentinel verknüpfen. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:

- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](detect-threats-built-in.md).
- [Verwenden Sie Arbeitsmappen](monitor-your-data.md), um Ihre Daten zu überwachen.