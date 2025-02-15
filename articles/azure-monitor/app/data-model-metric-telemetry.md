---
title: Datenmodell für Metriktelemetrie – Azure Application Insights
description: Application Insights-Datenmodell für Metriktelemetrie
ms.topic: conceptual
ms.date: 04/25/2017
ms.reviewer: sergkanz
ms.openlocfilehash: df3ce8715134aa51ee819210cad4caf126f0e492
ms.sourcegitcommit: 1f29603291b885dc2812ef45aed026fbf9dedba0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/29/2021
ms.locfileid: "129229599"
---
# <a name="metric-telemetry-application-insights-data-model"></a>Metriktelemetrie: Application Insights-Datenmodell

Es gibt zwei Arten von Metriktelemetrie, die von [Application Insights](./app-insights-overview.md) unterstützt werden: Einzelmessung und vorab aggregierte Metrik. Eine Einzelmessung ist nur ein Name und Wert. Eine vorab aggregierte Metrik gibt den Mindest- und Höchstwert der Metrik im Aggregationsintervall und die Standardabweichung davon an.

Vorab aggregierte Metriktelemetrie setzt voraus, das die Aggregationsdauer eine Minute betrug.

Es gibt mehrere bekannte Metriknamen, die von Application Insights unterstützt werden. Diese Metriken werden in der Tabelle „performanceCounters“ platziert.

Metrik, die System- und Prozessleistungsindikatoren darstellt:

| **.NET-Name**             | **Plattformunabhängiger Name** | **REST-API-Name** | **Beschreibung**
| ------------------------- | -------------------------- | ----------------- | ---------------- 
| `\Processor(_Total)\% Processor Time` | In Bearbeitung... | [processorCpuPercentage](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessorCpuPercentage) | Computer-CPU insgesamt
| `\Memory\Available Bytes`                 | In Bearbeitung... | [memoryAvailableBytes](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FmemoryAvailableBytes) | Zeigt die Menge des physischen Speichers in Byte an, der für auf dem Computer ausgeführte Prozesse zur Verfügung steht. Diese wird durch Addieren der Menge des Speicherplatzes in den genullten, freien und Standbyspeicherlisten berechnet. Freier Speicher steht zur Verwendung bereit. Mit Leerdaten beschriebene Speicher bestehen aus Seiten von mit Nullen gefülltem Speicher, um zu verhindern, dass in nachfolgenden Prozessen Daten von vorherigen Prozessen angezeigt werden. Standbyspeicher sind Speicher, die aus dem Arbeitssatz eines Prozesses (dem physischen Speicher) bei der Übermittlung an einen Datenträger entfernt wurden, jedoch weiterhin für den Abruf verfügbar sind. Weitere Informationen finden Sie unter [Speicherobjekt](/previous-versions/ms804008(v=msdn.10)).
| `\Process(??APP_WIN32_PROC??)\% Processor Time` | In Bearbeitung... | [processCpuPercentage](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessCpuPercentage) | CPU des Prozesses, der die Anwendung hostet
| `\Process(??APP_WIN32_PROC??)\Private Bytes`      | In Bearbeitung... | [processPrivateBytes](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessPrivateBytes) | Vom Prozess, der die Anwendung hostet, belegter Arbeitsspeicher
| `\Process(??APP_WIN32_PROC??)\IO Data Bytes/sec` | In Bearbeitung... | [processIOBytesPerSecond](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessIOBytesPerSecond) | Rate der E/A-Vorgänge des Prozesses, der die Anwendung hostet
| `\ASP.NET Applications(??APP_W3SVC_PROC??)\Requests/Sec`             | In Bearbeitung... | [requestsPerSecond](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FrequestsPerSecond) | Rate der Anforderungen, die von der Anwendung verarbeitet werden 
| `\.NET CLR Exceptions(??APP_CLR_PROC??)\# of Exceps Thrown / sec`    | In Bearbeitung... | [exceptionsPerSecond](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FexceptionsPerSecond) | Rate der von der Anwendung ausgelösten Ausnahmen
| `\ASP.NET Applications(??APP_W3SVC_PROC??)\Request Execution Time`   | In Bearbeitung... | [requestExecutionTime](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FrequestExecutionTime) | Durchschnittliche Ausführungszeit von Anforderungen
| `\ASP.NET Applications(??APP_W3SVC_PROC??)\Requests In Application Queue` | In Bearbeitung... | [requestsInQueue](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FrequestsInQueue) | Anzahl der Anforderungen, die in einer Warteschlange auf die Verarbeitung warten

## <a name="name"></a>Name

Name der Metrik, der im Application Insights-Portal und auf der Benutzeroberfläche angezeigt werden soll. 

## <a name="value"></a>Wert

Einzelner Wert für Messung. Summe der einzelnen Messungen für die Aggregation.

## <a name="count"></a>Anzahl

Metrische Gewichtung der aggregierten Metrik. Darf nicht für eine Messung festgelegt werden.

## <a name="min"></a>Min

Mindestwert der aggregierten Metrik. Darf nicht für eine Messung festgelegt werden.

## <a name="max"></a>Max.

Höchstwert der aggregierten Metrik. Darf nicht für eine Messung festgelegt werden.

## <a name="standard-deviation"></a>Standardabweichung

Standardabweichung der aggregierten Metrik. Darf nicht für eine Messung festgelegt werden.

## <a name="custom-properties"></a>Benutzerdefinierte Eigenschaften

Metrik, deren benutzerdefinierte `CustomPerfCounter`-Eigenschaft auf `true` festgelegt ist, um anzugeben, dass die Metrik den Windows-Leistungsindikator darstellt. Diese Metriken werden in der Tabelle „performanceCounters“ platziert. Nicht in „customMetrics“. Der Name dieser Metrik wird außerdem analysiert, um Kategorie, Anzahl und Instanznamen zu extrahieren.

[!INCLUDE [application-insights-data-model-properties](../../../includes/application-insights-data-model-properties.md)]

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie die [Application Insights-API für benutzerdefinierte Ereignisse und Metriken](./api-custom-events-metrics.md#trackmetric) nutzen.
- Lesen Sie die Informationen zu den Application Insights-Typen und zum Datenmodell unter [Datenmodell](data-model.md).
- Lesen Sie die Informationen zu den von Application Insights unterstützten [Plattformen](./platforms.md).
