---
title: Überwachen der serverseitigen Latenz bei Vorgängen in Azure Cosmos DB
description: Hier erfahren Sie, wie Sie die serverseitige Latenz bei Vorgängen im Azure Cosmos DB-Konto oder einem Container überwachen. Besitzer eines Azure Cosmos DB-Kontos können die serverseitigen Latenzprobleme bei Ihren Azure Cosmos-Konten verstehen.
ms.service: cosmos-db
ms.topic: how-to
ms.author: esarroyo
author: StefArroyo
ms.date: 04/07/2020
ms.openlocfilehash: 883552a556ce0cde191cb3dfeccca0ac09c96a10
ms.sourcegitcommit: 82d82642daa5c452a39c3b3d57cd849c06df21b0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2021
ms.locfileid: "113359621"
---
# <a name="how-to-monitor-the-server-side-latency-for-operations-in-an-azure-cosmos-db-container-or-account"></a>Überwachen der serverseitigen Latenz bei Vorgängen in einem Azure Cosmos DB-Container oder -Konto
[!INCLUDE[appliesto-all-apis](includes/appliesto-all-apis.md)]

Azure Monitor für Azure Cosmos DB bietet eine Metrikansicht zum Überwachen Ihres Kontos und zum Erstellen von Dashboards. Weil die Azure Cosmos DB-Metriken standardmäßig erfasst werden, müssen Sie bei diesem Feature nichts explizit aktivieren oder konfigurieren. Die serverseitige Latenzmetrik wird zum Anzeigen der serverseitigen Latenz eines Vorgangs verwendet. Azure Cosmos DB bietet eine SLA von weniger als 10 ms für Punktlese-/Punktschreibvorgänge mit direkter Konnektivität. Bei Punktlese- und -schreibvorgängen werden die SLAs so berechnet, wie es im [SLA-Dokument](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_3/) ausführlich erläutert wird.

Sie können die serverseitige Latenz überwachen, wenn Sie eine ungewöhnlich hohe Latenz bei Punktvorgängen feststellen. Beispiele:

* Ein GET- oder SET-Vorgang mit Partitionsschlüssel und ID im Modus für direkte Konnektivität
* Ein Lese- oder Schreibvorgang oder
* Eine Abfrage

Sie können im Diagnoseprotokoll nachschlagen, um die Größe der zurückgegebenen Daten zu ermitteln. Wenn Sie eine dauerhafte hohe Latenz bei Abfragevorgängen feststellen, sollten Sie im Diagnoseprotokoll nach einem höheren verwendeten [Durchsatz oder RU/s](cosmosdb-monitor-logs-basic-queries.md) suchen. Die serverseitige Latenz gibt die Zeitspanne an, die in der Back-End-Infrastruktur aufgewendet wurde, bevor die Daten an den Client zurückgegeben wurden. Es ist wichtig, sich diese Metrik anzusehen, um jegliche Probleme mit der Back-End-Latenz auszuschließen.

## <a name="view-the-server-side-latency-metric"></a>Anzeigen der serverseitigen Latenzmetrik

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
   
2. Wählen Sie in der Navigationsleiste auf der linken Seite die Option **Monitor** und dann **Metrik** aus.

   :::image type="content" source="./media/monitor-server-side-latency/monitor-metrics-blade.png" alt-text="Bereich „Metriken“ in Azure Monitor":::

3. Klicken Sie im Bereich **Metriken** auf **Ressource auswählen**. Wählen Sie dann das erforderliche **Abonnement** und die **Ressourcengruppe** aus. Wählen Sie unter **Ressourcentyp** die Option **Azure Cosmos DB-Konten** aus. Wählen Sie dann eins der vorhandenen Azure Cosmos-Konten und anschließend **Anwenden** aus.
   
   :::image type="content" source="./media/monitor-server-side-latency/select-cosmos-db-account.png" alt-text="Auswählen des Azure Cosmos DB-Kontos zum Anzeigen von Metriken":::

4. Wählen Sie als Nächstes in der Liste der verfügbaren Metriken die Metrik **Serverseitige Latenz** aus. Ausführliche Informationen zu allen verfügbaren Metriken in dieser Liste finden Sie im Artikel [Metriken nach Kategorie](monitor-cosmos-db-reference.md). In diesem Beispiel wählen Sie **Serverseitige Latenz** und als Aggregationswert **Durchschn.** aus. Zusätzlich zu diesen Angaben können Sie auch **Zeitbereich** und **Zeitgranularität** für die Metriken auswählen. Sie können Metriken maximal für die letzten 30 Tage anzeigen.  Nach Anwendung des Filters wird ein darauf basierendes Diagramm angezeigt. Die serverseitige Latenz pro Minute wird für den ausgewählten Zeitraum angezeigt.  

   :::image type="content" source="./media/monitor-server-side-latency/server-side-latency-metric.png" alt-text="Auswählen der serverseitigen Latenzmetrik über das Azure-Portal":::

## <a name="filters-for-server-side-latency"></a>Filter für serverseitige Latenz

Sie können Metriken und die angezeigten Diagramme auch nach bestimmten Werten für **CollectionName**, **ConnectionMode**, **DatabaseName**, **OperationType**, **Region** und **PublicAPIType** filtern. 

Wählen Sie zum Filtern der Metriken **Filter hinzufügen** und dann die erforderliche Eigenschaft aus, z. B. **PublicAPIType**. Wählen Sie anschließend den Wert **sql** aus. Fügen Sie einen weiteren Filter für **OperationType** hinzu. Im Diagramm wird dann die serverseitige Latenz bei verschiedenen Vorgänge während des ausgewählten Zeitraums angezeigt. Die über eine gespeicherte Prozedur ausgeführten Vorgänge werden nicht protokolliert und sind daher nicht unter der Metrik „OperationType“ verfügbar.

Die **serverseitigen Latenzmetriken** für jeden Vorgang werden wie in der folgenden Abbildung dargestellt angezeigt:

:::image type="content" source="./media/monitor-server-side-latency/server-side-latency-filters.png" alt-text="Filter für serverseitige Latenzmetriken":::

Sie können die Metriken mit der Option **Teilung anwenden** auch gruppieren.  

## <a name="next-steps"></a>Nächste Schritte

* Überwachen der Azure Cosmos DB-Daten anhand der [Diagnoseeinstellungen](cosmosdb-monitor-resource-logs.md) in Azure
* [Überwachen von Azure Cosmos DB-Vorgängen auf Steuerungsebene](audit-control-plane-logs.md)
