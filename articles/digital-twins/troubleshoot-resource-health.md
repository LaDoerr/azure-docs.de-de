---
title: Grundlegendes zur Ressourcenintegrität
titleSuffix: Azure Digital Twins
description: Erfahren Sie, wie Sie Azure Resource Health verwenden können, um die Integrität Ihrer Azure Digital Twins-Instanz zu überprüfen.
author: baanders
ms.author: baanders
ms.date: 10/6/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: f62e07bbc5b61ce07299f1d617b8aea1b331024e
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110475588"
---
# <a name="troubleshooting-azure-digital-twins-resource-health"></a>Problembehandlung von Azure Digital Twins: Ressourcenintegrität

[Azure Resource Health](../service-health/resource-health-overview.md) unterstützt Sie bei der Diagnose und bei Supportanfragen, wenn Dienstprobleme Auswirkungen auf Ihre Azure-Ressourcen haben. Der Dienst erstellt Berichte zur aktuellen und früheren Integrität Ihrer Ressourcen.

In diesem Artikel wird gezeigt, wie Sie Informationen zur **Ressourcenintegrität** für Ihre Azure Digital Twins-Instanzen abrufen können.

## <a name="use-azure-resource-health"></a>Verwenden von Azure Resource Health

Azure Resource Health kann Ihnen bei der Überwachung helfen, ob Ihre Azure Digital Twins-Instanz ordnungsgemäß ausgeführt wird. Sie können darüber auch erfahren, ob ein regionaler Ausfall Auswirkungen auf die Integrität Ihrer Instanz hat.

Um die Integrität Ihrer Instanz zu überprüfen, gehen Sie folgendermaßen vor:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu Ihrer Azure Digital Twins-Instanz. Sie finden sie, indem Sie ihren Namen auf der Suchleiste des Portals eingeben. 

2. Wählen Sie im Menü Ihrer Instanz die Option _**Ressourcenintegrität**_ unter *Support und Problembehandlung* aus. Dadurch gelangen Sie auf die Seite zur Ansicht des Verlaufs der Ressourcenintegrität. 

    :::image type="content" source="media/troubleshoot-resource-health/resource-health.png" alt-text="Screenshot, der die Seite „Ressourcenintegrität“ zeigt. Es gibt einen Abschnitt „Integritätsverlauf“ mit einem täglichen Bericht der letzten neun Tage.":::

In der obigen Abbildung ist dieser Fall als *Verfügbar* dargestellt, und zwar seit neun Tagen. Weitere Informationen über den Status *Verfügbar* und die anderen Statustypen, die auftreten können, finden Sie unter [Übersicht über Azure Resource Health](../service-health/resource-health-overview.md).

Weitere Informationen zu den verschiedenen unterschiedlichen Azure-Ressourcentypen finden Sie unter [Ressourcentypen und Integritätsprüfungen in Azure Resource Health](../service-health/resource-health-checks-resource-types.md).

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie in den folgenden Artikeln über andere Möglichkeiten, Ihre Azure Digital Twins-Instanz zu überwachen:
* [Problembehandlung: Anzeigen von Metriken mit Azure Monitor](troubleshoot-metrics.md)
* [Problembehandlung: Einrichten der Diagnose](troubleshoot-diagnostics.md).
* [Problembehandlung: Einrichten von Warnungen](troubleshoot-alerts.md).
