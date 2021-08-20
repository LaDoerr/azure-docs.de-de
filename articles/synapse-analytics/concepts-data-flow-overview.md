---
title: Datenflüsse
description: Eine Übersicht über Datenflüsse in Azure Synapse Analytics
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: synapse-analytics
ms.subservice: pipeline
ms.topic: conceptual
ms.custom: references_regions
ms.date: 12/16/2020
ms.openlocfilehash: 525fed571576f6d5fc49d68abbbe26e46cf3291e
ms.sourcegitcommit: 6c6b8ba688a7cc699b68615c92adb550fbd0610f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122349979"
---
# <a name="data-flows-in-azure-synapse-analytics"></a>Datenflüsse in Azure Synapse Analytics

## <a name="what-are-data-flows"></a>Was sind Datenflüsse?

Datenflüsse sind visuell entworfene Datentransformationen in Azure Synapse Analytics. Mit Datenflüssen können Data Engineers eine Datentransformationslogik entwickeln, ohne Code schreiben zu müssen. Die daraus resultierenden Datenflüsse werden als Aktivitäten in Azure Synapse Analytics-Pipelines ausgeführt, die erweiterte Apache Spark-Cluster verwenden. Datenflussaktivitäten können mithilfe vorhandener Azure Synapse Analytics-Funktionen für Planung, Steuerung, Flows und Überwachung operationalisiert werden.

Datenflüsse bieten eine vollständig visuelle Darstellung, ohne dass Sie eine einzige Codezeile schreiben müssen. Ihre Datenflüsse werden in von Synapse verwalteten Ausführungsclustern für die erweiterte Datenverarbeitung ausgeführt. Azure Synapse Analytics verarbeitet die gesamte Codeübersetzung, Pfadoptimierung und Ausführung Ihrer Datenflussaufträge.

## <a name="getting-started"></a>Erste Schritte

Datenflüsse werden im Entwicklungsbereich von Synapse Studio erstellt. Um einen Datenfluss zu erstellen, wählen Sie das Pluszeichen neben **Entwickeln** und dann die Option **Datenfluss** aus. 

![Neuer Datenfluss](media/data-flow/new-data-flow.png)

Mit dieser Aktion gelangen Sie zur Datenflusscanvas, auf der Sie Ihre Transformationslogik erstellen können. Wählen Sie **Quelle hinzufügen** aus, um mit der Konfiguration Ihrer Quelltransformation zu beginnen. Weitere Informationen finden Sie im Artikel zur [Quelltransformation](../data-factory/data-flow-source.md?context=/azure/synapse-analytics/context/context).

## <a name="authoring-data-flows"></a>Erstellen von Datenflüssen

Ein Datenfluss verfügt über eine einzigartige Canvas, über die sich die Transformationslogik ganz einfach erstellen lässt. Die Datenflusscanvas ist in drei Bereiche unterteilt: die obere Leiste, das Diagramm und den Konfigurationsbereich. 

![Screenshot: Datenflusscanvas mit Beschriftungen für obere Leiste, Graph und Konfigurationsbereich](media/data-flow/canvas-1.png)

### <a name="graph"></a>Graph

Das Diagramm zeigt den Transformationsdatenstrom. Es zeigt die Herkunft der Quelldaten beim Fließen in eine oder mehrere Senken. Wählen Sie die Option **Quelle hinzufügen** aus, um eine neue Quelle hinzuzufügen. Wählen Sie zum Hinzufügen einer neuen Transformation unten rechts in einer vorhandenen Transformation das Pluszeichen aus. Informieren Sie sich über das [Verwalten des Datenflussdiagramms](../data-factory/concepts-data-flow-manage-graph.md?context=/azure/synapse-analytics/context/context).

![Screenshot: Graphbereich der Canvas mit einem Suchtextfeld](media/data-flow/canvas-2.png)

### <a name="configuration-panel"></a>Konfigurationsbereich

Im Konfigurationsbereich werden die spezifischen Einstellungen für die derzeit ausgewählte Transformation angezeigt. Wenn keine Transformation ausgewählt ist, wird der Datenfluss angezeigt. In der allgemeinen Datenflusskonfiguration können Sie Parameter über die Registerkarte **Parameter** hinzufügen. Weitere Informationen finden Sie unter [Datenflussparameter](../data-factory/parameters-data-flow.md?context=/azure/synapse-analytics/context/context).

Jede Transformation enthält mindestens vier Registerkarten für die Konfiguration.

#### <a name="transformation-settings"></a>Transformationseinstellungen

Die erste Registerkarte im Konfigurationsbereich jeder Transformation enthält die Einstellungen, die für diese Transformation spezifisch sind. Weitere Informationen finden Sie auf der Dokumentationsseite für diese Transformation.

![Registerkarte „Quelleinstellungen“](media/data-flow/source-1.png)

#### <a name="optimize"></a>Optimieren

Die Registerkarte **Optimieren** enthält Einstellungen zum Konfigurieren von Partitionierungsschemas. Weitere Informationen zum Optimieren Ihrer Datenflüsse finden Sie in der [Anleitung zur Leistung des Zuordnungsdatenflusses](../data-factory/concepts-data-flow-performance.md?context=/azure/synapse-analytics/context/context).

![Screenshot der Registerkarte „Optimieren“](media/data-flow/optimize.png)

#### <a name="inspect"></a>Überprüfen

Die Registerkarte **Überprüfen** bietet einen Einblick in die Metadaten des Datenstroms, den Sie transformieren. Sie können die Spaltenanzahl, geänderte Spalten, hinzugefügte Spalten, Datentypen, die Spaltensortierung und Spaltenverweise sehen. **Überprüfen** ist eine schreibgeschützte Ansicht Ihrer Metadaten. Der Debugmodus muss nicht aktiviert sein, um die Metadaten im Bereich **Überprüfen** anzeigen zu können.

![Registerkarte „Untersuchen“](media/data-flow/inspect.png)

Wenn Sie die Form Ihrer Daten durch Transformationen ändern, wird der Fluss der Metadatenänderungen im Bereich **Überprüfen** angezeigt. Falls in Ihrer Quelltransformation kein definiertes Schema vorhanden ist, werden im Bereich **Überprüfen** keine Metadaten angezeigt. Fehlende Metadaten kommen in Schemaabweichungsszenarien häufiger vor.

#### <a name="data-preview"></a>Datenvorschau

Bei aktiviertem Debugmodus können Sie auf der Registerkarte **Datenvorschau** eine interaktive Momentaufnahme der Daten bei jeder Transformation anzeigen. Weitere Informationen finden Sie unter [Datenvorschau im Debugmodus](../data-factory/concepts-data-flow-debug-mode.md?context=/azure/synapse-analytics/context/context#data-preview).

### <a name="top-bar"></a>Obere Leiste

Die obere Leiste enthält Aktionen, die sich auf den gesamten Datenfluss auswirken, z. B. Überprüfungen und Debugeinstellungen. Sie können auch den zugrunde liegenden JSON-Code und das Datenflussskript Ihrer Transformationslogik anzeigen.

## <a name="available-transformations"></a>Verfügbare Transformationen

Unter [Zuordnungsdatenfluss – Übersicht über Transformationen](../data-factory/data-flow-transformation-overview.md?context=/azure/synapse-analytics/context/context) finden Sie eine Liste der verfügbaren Transformationen.

## <a name="data-flow-activity"></a>Datenflussaktivität

Datenflüsse werden innerhalb von Azure Synapse Analytics-Pipelines mithilfe der [Datenflussaktivität](../data-factory/control-flow-execute-data-flow-activity.md?context=/azure/synapse-analytics/context/context) operationalisiert. Der Benutzer muss lediglich angeben, welche Integration Runtime verwendet werden soll, und Parameterwerte übergeben. Weitere Informationen finden Sie unter [Azure Integration Runtime](../data-factory/concepts-integration-runtime.md?context=/azure/synapse-analytics/context/context#azure-integration-runtime).

## <a name="debug-mode"></a>Debugmodus

Im Debugmodus können Sie die Ergebnisse jedes Transformationsschritts interaktiv anzeigen, während Sie Datenflüsse erstellen und debuggen. Die Debugsitzung kann sowohl beim Erstellen der Datenflusslogik als auch beim Ausführen von Debugläufen für die Pipeline mit Datenflussaktivitäten ausgeführt werden. Weitere Informationen finden Sie in der [Dokumentation zum Debugmodus](../data-factory/concepts-data-flow-debug-mode.md?context=/azure/synapse-analytics/context/context).

## <a name="monitoring-data-flows"></a>Überwachen von Datenflüssen

Datenflüsse lassen sich in vorhandene Azure Synapse Analytics-Überwachungsfunktionen integrieren. Informationen zum Verständnis der Ausgabe der Datenflussüberwachung finden Sie unter [Überwachen von Zuordnungsdatenflüssen](../data-factory/concepts-data-flow-monitoring.md?context=/azure/synapse-analytics/context/context).

Das Azure Synapse Analytics-Team hat eine [Anleitung zur Leistungsoptimierung](../data-factory/concepts-data-flow-performance.md?context=/azure/synapse-analytics/context/context) erstellt, mit deren Hilfe Sie die Ausführungszeit Ihrer Datenflüsse nach dem Erstellen der Geschäftslogik optimieren können.

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich über die Erstellung einer [Quelltransformation](../data-factory/data-flow-source.md?context=/azure/synapse-analytics/context/context).
* Informieren Sie sich darüber, wie Sie Ihre Datenflüsse im [Debugmodus](../data-factory/concepts-data-flow-debug-mode.md?context=/azure/synapse-analytics/context/context) erstellen.
