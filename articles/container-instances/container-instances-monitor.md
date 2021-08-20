---
title: Überwachen von Containerinstanzen
description: Vorgehensweise zur Überwachung des Verbrauchs von Computeressourcen, z. B. CPU und Arbeitsspeicher, durch Ihre Container in Azure Container Instances.
ms.topic: article
ms.date: 12/17/2020
ms.openlocfilehash: 7f485efc5bdc29760f0b4278b746940c947777e3
ms.sourcegitcommit: 192444210a0bd040008ef01babd140b23a95541b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/15/2021
ms.locfileid: "114219227"
---
# <a name="monitor-container-resources-in-azure-container-instances"></a>Überwachen von Containerressourcen in Azure Container Instances

[Azure Monitor][azure-monitoring] ermöglicht einen Einblick in die Computeressourcen, die von Ihren Containerinstanzen verwendet werden. Mit diesen Daten zur Ressourcenverwendung können Sie die besten Ressourceneinstellungen für Ihre Containergruppen ermitteln. Azure Monitor bietet außerdem Metriken, die die Netzwerkaktivität in Ihren Containerinstanzen nachverfolgen.

In diesem Dokument wird die Erfassung von Azure Monitor-Metriken für Containerinstanzen per Azure-Portal und Azure CLI ausführlich beschrieben.

> [!IMPORTANT]
> Azure Monitor-Metriken in Azure Container Instances befinden sich zurzeit in der Vorschauphase, und es gelten einige [Einschränkungen](#preview-limitations). Vorschauversionen werden Ihnen zur Verfügung gestellt, wenn Sie die [zusätzlichen Nutzungsbedingungen][terms-of-use] akzeptieren. Einige Aspekte dieses Features werden bis zur allgemeinen Verfügbarkeit unter Umständen noch geändert.

## <a name="preview-limitations"></a>Einschränkungen der Vorschau

Derzeit sind Azure Monitor-Metriken nur für Linux-Container verfügbar.

## <a name="available-metrics"></a>Verfügbare Metriken

Azure Monitor stellt die folgenden [Metriken für Azure Container Instances][supported-metrics] bereit. Diese Metriken sind sowohl für eine Containergruppe als auch für einzelne Container verfügbar. Standardmäßig werden die Metriken als Mittelwerte aggregiert.

- **CPU-Auslastung**, gemessen in **Millicores**. 
  - Ein Millicore ist ein Tausendstel eines CPU-Kerns. 500 Millicore geben also eine Auslastung von 0,5 CPU-Kernen an.
- **Arbeitsspeicherauslastung** in Byte
- **Empfangene Netzwerkbytes** pro Sekunde
- **Übertragene Netzwerkbytes** pro Sekunde 

## <a name="get-metrics---azure-portal"></a>Abrufen von Metriken – Azure-Portal

Wenn eine Containergruppe erstellt wird, sind im Azure-Portal Azure Monitor-Daten verfügbar. Um Metriken für eine Containergruppe anzuzeigen, wechseln Sie zur Seite **Übersicht** für die Containergruppe. Dort können Sie vorab erstellte Diagramme für jede der verfügbaren Metriken sehen.

![Duales Diagramm][dual-chart]

In einer Containergruppe, die mehrere Container enthält, verwenden Sie eine [Dimension][monitor-dimension], um Metriken nach Container anzuzeigen. Führen Sie die folgenden Schritte aus, um ein Diagramm mit einzelnen Containermetriken zu erstellen:

1. Auf der Seite **Übersicht** wählen Sie eins der Metrikdiagramme aus, z. B. **CPU**. 
1. Wählen Sie die Schaltfläche **Teilung anwenden** und dann **Containername** aus.

![Screenshot: Metriken für eine Containerinstanz, für die „Teilung anwenden“ und „Containername“ ausgewählt sind][dimension]

## <a name="get-metrics---azure-cli"></a>Abrufen von Metriken – Azure CLI

Metriken für Containerinstanzen können auch per Azure CLI erfasst werden. Rufen Sie zuerst die ID der Containergruppe ab, indem Sie den folgenden Befehl verwenden. Ersetzen Sie `<resource-group>` durch Ihre Ressourcengruppennamen und `<container-group>` durch den Namen Ihrer Containergruppe.


```azurecli
CONTAINER_GROUP=$(az container show --resource-group <resource-group> --name <container-group> --query id --output tsv)
```

Verwenden Sie den folgenden Befehl, um die Metriken zur Auslastung der **CPU** abzurufen.

```azurecli
az monitor metrics list --resource $CONTAINER_GROUP --metric CPUUsage --output table
```

```output
Timestamp            Name       Average
-------------------  ---------  ---------
2020-12-17 23:34:00  CPU Usage
. . .
2020-12-18 00:25:00  CPU Usage
2020-12-18 00:26:00  CPU Usage  0.4
2020-12-18 00:27:00  CPU Usage  0.0
```

Ändern Sie den Wert des Parameters `--metric` in dem Befehl, um andere [unterstützte Metriken][supported-metrics] abzurufen. Verwenden Sie beispielsweise den folgenden Befehl, um die Metriken zur Auslastung des **Arbeitsspeichers** abzurufen. 

```azurecli
az monitor metrics list --resource $CONTAINER_GROUP --metric MemoryUsage --output table
```

```output
Timestamp            Name          Average
-------------------  ------------  ----------
2019-04-23 22:59:00  Memory Usage
2019-04-23 23:00:00  Memory Usage
2019-04-23 23:01:00  Memory Usage  0.0
2019-04-23 23:02:00  Memory Usage  8859648.0
2019-04-23 23:03:00  Memory Usage  9181184.0
2019-04-23 23:04:00  Memory Usage  9580544.0
2019-04-23 23:05:00  Memory Usage  10280960.0
2019-04-23 23:06:00  Memory Usage  7815168.0
2019-04-23 23:07:00  Memory Usage  7739392.0
2019-04-23 23:08:00  Memory Usage  8212480.0
2019-04-23 23:09:00  Memory Usage  8159232.0
2019-04-23 23:10:00  Memory Usage  8093696.0
```

Für eine Gruppe mit mehreren Containern kann die Dimension `containerName` hinzugefügt werden, um Metriken pro Container zurückzugeben.

```azurecli
az monitor metrics list --resource $CONTAINER_GROUP --metric MemoryUsage --dimension containerName --output table
```

```output
Timestamp            Name          Containername             Average
-------------------  ------------  --------------------  -----------
2019-04-23 22:59:00  Memory Usage  aci-tutorial-app
2019-04-23 23:00:00  Memory Usage  aci-tutorial-app
2019-04-23 23:01:00  Memory Usage  aci-tutorial-app      0.0
2019-04-23 23:02:00  Memory Usage  aci-tutorial-app      16834560.0
2019-04-23 23:03:00  Memory Usage  aci-tutorial-app      17534976.0
2019-04-23 23:04:00  Memory Usage  aci-tutorial-app      18329600.0
2019-04-23 23:05:00  Memory Usage  aci-tutorial-app      19742720.0
2019-04-23 23:06:00  Memory Usage  aci-tutorial-app      14786560.0
2019-04-23 23:07:00  Memory Usage  aci-tutorial-app      14651392.0
2019-04-23 23:08:00  Memory Usage  aci-tutorial-app      15470592.0
2019-04-23 23:09:00  Memory Usage  aci-tutorial-app      15450112.0
2019-04-23 23:10:00  Memory Usage  aci-tutorial-app      15339520.0
2019-04-23 22:59:00  Memory Usage  aci-tutorial-sidecar
2019-04-23 23:00:00  Memory Usage  aci-tutorial-sidecar
2019-04-23 23:01:00  Memory Usage  aci-tutorial-sidecar  0.0
2019-04-23 23:02:00  Memory Usage  aci-tutorial-sidecar  884736.0
2019-04-23 23:03:00  Memory Usage  aci-tutorial-sidecar  827392.0
2019-04-23 23:04:00  Memory Usage  aci-tutorial-sidecar  831488.0
2019-04-23 23:05:00  Memory Usage  aci-tutorial-sidecar  819200.0
2019-04-23 23:06:00  Memory Usage  aci-tutorial-sidecar  843776.0
2019-04-23 23:07:00  Memory Usage  aci-tutorial-sidecar  827392.0
2019-04-23 23:08:00  Memory Usage  aci-tutorial-sidecar  954368.0
2019-04-23 23:09:00  Memory Usage  aci-tutorial-sidecar  868352.0
2019-04-23 23:10:00  Memory Usage  aci-tutorial-sidecar  847872.0
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Überwachung mit Azure finden Sie unter [Überwachen von Azure-Anwendungen und -Ressourcen][azure-monitoring].

Erfahren Sie, wie Sie [Metrikwarnungen][metric-alert] erstellen, um benachrichtigt zu werden, wenn eine Metrik für Azure Container Instances einen Schwellenwert überschreitet.

<!-- IMAGES -->
[cpu-chart]: ./media/container-instances-monitor/cpu-multi.png
[dimension]: ./media/container-instances-monitor/dimension.png
[dual-chart]: ./media/container-instances-monitor/metrics.png
[memory-chart]: ./media/container-instances-monitor/memory-multi.png

<!-- LINKS - External -->
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[azure-monitoring]: ../azure-monitor/overview.md
[metric-alert]: ..//azure-monitor/alerts/alerts-metric.md
[monitor-dimension]: ../azure-monitor/essentials/data-platform-metrics.md#multi-dimensional-metrics
[supported-metrics]: ../azure-monitor/essentials/metrics-supported.md#microsoftcontainerinstancecontainergroups
