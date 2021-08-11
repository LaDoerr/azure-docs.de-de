---
title: Übersicht über Speicher – Azure Time Series Insights Gen2 | Microsoft-Dokumentation
description: Hier erfahren Sie mehr über Datenspeicherung in Azure Time Series Insights Gen2.
author: tedvilutis
ms.author: tvilutis
manager: diviso
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 01/21/2021
ms.custom: seodec18
ms.openlocfilehash: 9c0bbf9224f8864428d46e38487f614e0c3f61f0
ms.sourcegitcommit: 5da0bf89a039290326033f2aff26249bcac1fe17
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2021
ms.locfileid: "109715230"
---
# <a name="data-storage"></a>Datenspeicherung

In diesem Artikel wird Datenspeicherung in Azure Time Series Insights Gen2 beschrieben. Er behandelt Speicher der warmen und kalten Ebene, Datenverfügbarkeit und bewährte Methoden.

## <a name="provisioning"></a>Bereitstellung

Beim Erstellen einer Azure Time Series Insights Gen2-Umgebung haben Sie folgende Möglichkeiten:

* Kalter Datenspeicher:
  * Erstellen Sie eine neue Azure Storage-Ressource in dem Abonnement und der Region, das/die Sie für Ihre Umgebung ausgewählt haben.
  * Fügen Sie ein bereits vorhandenes Azure-Speicherkonto an. Diese Option ist nur verfügbar, wenn Sie über eine Azure Resource Manager-[Vorlagen](/azure/templates/microsoft.timeseriesinsights/allversions) bereitstellen, und sie wird nicht im Azure-Portal angezeigt.
* Warmer Datenspeicher:
  * Ein warmer Speicher ist optional und kann während oder nach der Bereitstellung aktiviert oder deaktiviert werden. Wenn Sie sich dazu entschließen, warmen Speicher zu einem späteren Zeitpunkt zu aktivieren, und Ihr kalter Speicher bereits Daten enthält, lesen Sie [diesen](concepts-storage.md#warm-store-behavior) Abschnitt weiter unten, um das erwartete Verhalten zu verstehen. Die Datenaufbewahrungsdauer für warmen Speicher kann auf 7 bis 31 Tage konfiguriert werden, was sich auch nach Bedarf anpassen lässt.

Wenn ein Ereignis erfasst wird, wird es sowohl im warmen Speicher (sofern aktiviert) als auch im kalten Speicher indiziert.

[![Speicherübersicht](media/concepts-storage/pipeline-to-storage.png)](media/concepts-storage/pipeline-to-storage.png#lightbox)

> [!WARNING]
> Als Besitzer des Azure-Blobspeicherkontos, in dem sich die kalten Daten befinden, haben Sie Vollzugriff auf alle Daten im Konto. Dieser Zugriff umfasst Berechtigungen zum Schreiben und Löschen. Bearbeiten oder löschen Sie keine von Azure Time Series Insights Gen2 geschriebenen Daten, weil dies zu Datenverlusten führen kann.

## <a name="data-availability"></a>Datenverfügbarkeit

Azure Time Series Insights Gen2 partitioniert und indiziert Daten, um eine optimale Abfrageleistung zu erzielen. Die Daten können nach der Indizierung sowohl aus dem warmen (sofern aktiviert) als auch aus dem kalten Speicher abgefragt werden. Die Menge an erfassten Daten und die Durchsatzrate pro Partition können sich auf diese Verfügbarkeit auswirken. Machen Sie sich mit den [Durchsatzeinschränkungen](./concepts-streaming-ingress-throughput-limits.md) und [bewährten Methoden](./concepts-streaming-ingestion-event-sources.md#streaming-ingestion-best-practices) für die Ereignisquelle vertraut, um die bestmögliche Leistung zu erzielen. Sie können auch eine [Verzögerungswarnung](./time-series-insights-environment-mitigate-latency.md#monitor-latency-and-throttling-with-alerts) konfigurieren, damit Sie benachrichtigt werden, wenn in Ihrer Umgebung Probleme beim Verarbeiten von Daten auftreten.

> [!IMPORTANT]
> Es kann bis zu 60 Sekunden dauern, bis Daten über die [Zeitreihenabfrage-APIs](./concepts-query-overview.md) verfügbar werden. Wenn Sie eine erhebliche Latenz von weit über 60 Sekunden feststellen, übermitteln Sie ein Supportticket über das Azure-Portal.
> 
> Es kann bis zu fünf Minuten dauern, bis Daten verfügbar werden, wenn Sie außerhalb von Azure Time Series Insights Gen2 direkt auf die Parquet-Dateien zugreifen. Im Abschnitt zum [Parquet-Dateiformat](#parquet-file-format-and-folder-structure) finden Sie weitere Informationen.

## <a name="warm-store"></a>Warmer Speicher

Daten in Ihrem warmen Speicher stehen nur über die [Zeitreihenabfrage-APIs](./concepts-query-overview.md), den [Azure Time Series Insights TSI-Explorer](./concepts-ux-panels.md) oder den [Power BI-Connector](./how-to-connect-power-bi.md) zur Verfügung. Abfragen des warmen Speichers sind kostenlos, und es gibt kein Kontingent dafür, aber es gibt ein [Limit von 30](/rest/api/time-series-insights/reference-api-limits#query-apis---limits) gleichzeitigen Anforderungen.

### <a name="warm-store-behavior"></a>Verhalten von warmem Speicher

* Wenn diese Option aktiviert ist, werden alle Daten, die in Ihre Umgebung gestreamt werden, unabhängig vom Ereigniszeitstempel an Ihren warmen Speicher weitergeleitet. Beachten Sie, dass die Streamingerfassungs-Pipeline für Quasi-Echtzeitstreaming und konstruiert ist. Die Erfassung von Verlaufsereignissen wird [nicht unterstützt](./concepts-streaming-ingestion-event-sources.md#historical-data-ingestion).
* Der Aufbewahrungszeitraum wird auf Grundlage des Indizierungszeitpunkts des Ereignisses im warmem Speicher berechnet, nicht anhand des Ereigniszeitstempels. Dies bedeutet, dass Daten nach Ablauf des Aufbewahrungszeitraums nicht mehr im warmen Speicher verfügbar sind, auch wenn der Ereigniszeitstempel in der Zukunft liegt.
  * Beispiel: Ein Ereignis mit 10-tägigen Wettervorhersagen wird erfasst und in einem warmen Speichercontainer indiziert, der mit einer Aufbewahrungsdauer von 7 Tagen konfiguriert ist. Nach sieben Tagen ist die Vorhersage im warmen Speicher nicht mehr verfügbar, kann aber im kalten Speicher abgefragt werden.
* Wenn Sie den warmen Speicher für eine vorhandene Umgebung aktivieren, die bereits über aktuelle Daten verfügt, die im kalten Speicher indiziert wurden, beachten Sie, dass der warme Speicher mit diesen Daten nicht mehr rückwirkend aufgefüllt wird.
* Wenn Sie den warmen Speicher gerade aktiviert haben und Probleme beim Anzeigen der aktuellen Daten im Explorer auftreten, können Sie Abfragen an den warmen Speicher vorübergehend deaktivieren:

   [![Deaktivieren von Abfragen des warmen Speichers](media/concepts-storage/toggle-warm.png)](media/concepts-storage/toggle-warm.png#lightbox)

## <a name="cold-store"></a>Kalter Speicher

In diesem Abschnitt werden die Azure Storage-Details beschrieben, die für Azure Time Series Insights Gen2 relevant sind.

Eine ausführliche Beschreibung des Azure Blobspeichers finden Sie in der [Einführung in Azure Blob Storage](../storage/blobs/storage-blobs-introduction.md).

### <a name="your-cold-storage-account"></a>Ihr Konto für kalten Speicher

Azure Time Series Insights Gen2 behält bis zu zwei Kopien jedes Ereignisses in Ihrem Azure Storage-Konto bei. Eine Kopie speichert Ereignisse nach Erfassungszeit, wobei der Zugriff auf Ereignisse in einer zeitlich geordneten Sequenz immer zulässig ist. Im Laufe der Zeit wird in Azure Time Series Insights Gen2 auch eine neu partitionierte Kopie der Daten erstellt, um sie für leistungsfähige Abfragen zu optimieren.

Alle Ihre Daten werden mit unbegrenzter Dauer in Ihrem Azure Storage-Konto gespeichert.

> [!WARNING]
> Schränken Sie den öffentlichen Internetzugriff auf das Konto, das von Time Series Insights verwendet wird, nicht ein. Andernfalls wird die erforderliche Verbindung unterbrochen.

#### <a name="writing-and-editing-blobs"></a>Schreiben und Bearbeiten von Blobs

Wenn Sie Abfrageleistung und Datenverfügbarkeit sicherstellen möchten, bearbeiten oder löschen Sie keine Blobs, die von Azure Time Series Insights Gen2 erstellt werden.

#### <a name="accessing-cold-store-data"></a>Zugreifen auf kalte Speicherdaten

Zusätzlich zum Zugriff auf Ihre Daten über den [Azure Time Series Insights-Explorer](./concepts-ux-panels.md) und [Zeitreihenabfrage-APIs](./concepts-query-overview.md) können Sie auch direkt über die im kalten Speicher gespeicherten Parquet-Dateien auf Ihre Daten zugreifen. Beispielsweise können Sie Daten in einem Jupyter-Notebook lesen, transformieren und bereinigen und dann verwenden, um Ihr Azure Machine Learning-Modell im gleichen Spark-Workflow zu trainieren.

Wenn Sie direkt über Ihr Azure Storage-Konto auf Daten zugreifen möchten, benötigen Sie Lesezugriff auf das Konto, das zum Speichern Ihrer Azure Time Series Insights Gen2-Daten verwendet wird. Sie können dann ausgewählte Daten basierend auf der Erstellungszeit der Parquet-Datei lesen, die sich im unten im Abschnitt [Parquet-Dateiformat](#parquet-file-format-and-folder-structure) beschriebenen Ordner `PT=Time` befindet.  Weitere Informationen zum Aktivieren des Lesezugriffs auf Ihr Speicherkonto finden Sie unter [Verwalten des Zugriffs auf Ihre Speicherkontoressourcen](../storage/blobs/anonymous-read-access-configure.md).

#### <a name="data-deletion"></a>Löschen von Daten

Löschen Sie nicht Ihre Azure Time Series Insights Gen2-Dateien. Verwalten Sie zugehörige Daten nur innerhalb von Azure Time Series Insights Gen2.

### <a name="parquet-file-format-and-folder-structure"></a>Parquet-Dateiformat und Ordnerstruktur

Parquet ist ein spaltenbasiertes Open-Source-Dateiformat, das für effiziente Speicherung und Leistung konzipiert wurde. Azure Time Series Insights Gen2 verwendet Parquet, um eine Time Series-ID-basierte Abfrageleistung im richtigen Maßstab zu ermöglichen.

Weitere Informationen zum Parquet-Dateityp finden Sie in der [Parquet-Dokumentation](https://parquet.apache.org/documentation/latest/).

Azure Time Series Insights Gen2 speichert Kopien Ihrer Daten folgendermaßen:

* Der Ordner `PT=Time` wird anhand des Erfassungszeitpunkts partitioniert und speichert Daten in etwa in der Reihenfolge ihres Eingangs. Diese Daten werden aufbewahrt, und Sie können von außerhalb von Azure Time Series Insight Gen2 direkt darauf zugreifen, z. B. über Ihre Spark-Notebooks. Der Zeitstempel `<YYYYMMDDHHMMSSfff>` entspricht dem Erfassungszeitpunkt der Daten. Die Werte `<MinEventTimeStamp>` und `<MaxEventTimeStamp>` entsprechen dem in der Datei enthaltenen Bereich für Ereigniszeitstempel. Der Pfad und der Dateiname sind folgendermaßen formatiert:

  `V=1/PT=Time/Y=<YYYY>/M=<MM>/<BlobCreationTimestamp>_<MinEventTimestamp>_<MaxEventTimestamp>_<TsiInternalSuffix>.parquet`

* Die Ordner `PT=Live` und `PT=Tsid` enthalten eine zweite Kopie Ihrer Daten, die für eine flexible Leistung für Zeitreihenabfragen neu partitioniert wurde. Diese Daten werden im Laufe der Zeit optimiert und sind nicht statisch. Während der Neupartitionierung können einige Ereignisse in mehreren Blobs vorhanden sein, und die Blobnamen können sich ändern.  Diese Ordner werden von Azure Time Series Insights Gen2 verwendet und sollten nicht direkt aufgerufen werden. Verwenden Sie für diesen Zweck nur `PT=Time`.

> [!NOTE]
>
> Daten im Ordner `PT=Time`, die vor Juni 2021 erstellt wurden, können ein Dateiformat ohne Ereigniszeitbereiche aufweisen: `V=1/PT=Time/Y=<YYYY>/M=<MM>/<BlobCreationTimestamp>_<TsiInternalSuffix>.parquet`.  Das interne Dateiformat ist identisch, und Dateien mit beiden Benennungsschemas können zusammen verwendet werden. 
>
> * `<YYYY>` entspricht einer vierstelligen Jahresdarstellung.
> * `<MM>` entspricht einer zweistelligen Monatsdarstellung.
> * Das Format `<YYYYMMDDHHMMSSfff>` entspricht einem Zeitstempel mit vierstelliger Jahresangabe (`YYYY`), zweistelliger Monatsangabe (`MM`), zweistelliger Tagesangabe (`DD`), zweistelliger Stundenangabe (`HH`), zweistelliger Minutenangabe (`MM`), zweistelliger Sekundenangabe (`SS`) und dreistelliger Millisekundenangabe (`fff`).

Azure Time Series Insights Gen2-Ereignisse werden dem Inhalt von Parquet-Dateien folgendermaßen zugeordnet:

* Jedes Ereignis wird einer einzelnen Zeile zugeordnet.
* Jede Zeile enthält die Spalte **timestamp** mit einem Zeitstempel für das Ereignis. Die timestamp-Eigenschaft ist nie NULL. Ihr Standardwert ist der **Zeitpunkt der Einreihung des Ereignisses in die Warteschlange**, wenn die Zeitstempeleigenschaft in der Ereignisquelle nicht angegeben ist. Der gespeicherte Zeitstempel wird immer in UTC angegeben.
* Jede Zeile enthält die TSID-Spalte(n) (Time Series-ID), wie beim Erstellen der Azure Time Series Insights Gen2-Umgebung definiert wurde. Der TSID-Eigenschaftsname enthält das Suffix `_string`.
* Alle anderen als Telemetriedaten gesendeten Eigenschaften werden Spaltennamen zugeordnet, die je nach Eigenschaftstyp auf `_bool` (boolesch), `_datetime` (Zeitstempel), `_long` (long), `_double` (double), `_string` (Zeichenfolge) oder `_dynamic` (dynamisch) enden.  Weitere Informationen finden Sie unter [Unterstützte Datentypen](./concepts-supported-data-types.md).
* Dieses Zuordnungsschema gilt für die erste Version des Dateiformats, auf die als **V=1** verwiesen und die im Basisordner desselben Namens gespeichert wird. Wenn diese Funktion weiterentwickelt wird, kann sich dieses Zuordnungsschema ändern und der Verweisname erhöht werden.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zur [Datenmodellierung](./concepts-model-overview.md).

* Planen Sie Ihre [Azure Time Series Insights Gen2-Umgebung](./how-to-plan-your-environment.md).
