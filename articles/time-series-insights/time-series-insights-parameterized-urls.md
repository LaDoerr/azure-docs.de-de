---
title: Teilen von benutzerdefinierten Ansichten mit parametrisierten URLs – Azure Time Series Insights | Microsoft-Dokumentation
description: Sie erfahren, wie Sie parametrisierte URLs erstellen, um benutzerdefinierte Explorer-Ansichten in Azure Time Series Insights ganz einfach freizugeben.
ms.service: time-series-insights
services: time-series-insights
author: narmeens
ms.author: narsam
manager: cnovak
ms.reviewer: orspodek
ms.topic: conceptual
ms.workload: big-data
ms.date: 10/02/2020
ms.custom: seodec18
ms.openlocfilehash: 395420c61e2cc3f4167590f81e625bf7e67cbfac
ms.sourcegitcommit: 4f185f97599da236cbed0b5daef27ec95a2bb85f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2021
ms.locfileid: "112371227"
---
# <a name="share-a-custom-view-using-a-parameterized-url"></a>Teilen einer benutzerdefinierten Ansicht mit einer parametrisierten URL

Sie können eine parametrisierte URL der benutzerdefinierten Ansicht programmgesteuert erstellen, um im Azure Time Series Insights-Explorer eine benutzerdefinierte Ansicht zu teilen.

Der Azure Time Series Insights-Explorer unterstützt URL-Abfrageparameter, um Ansichten auf der Benutzeroberfläche direkt über die URL anzugeben. Indem Sie nur die URL verwenden, können Sie beispielsweise eine Zielumgebung, ein Suchprädikat und eine gewünschte Zeitspanne angeben. Wenn ein Benutzer die benutzerdefinierte URL auswählt, wird auf der Oberfläche ein direkter Link zu diesem Objekt im Azure Time Series Insights-Portal bereitgestellt. Es gelten die Richtlinien für den Datenzugriff.

> [!TIP]
>
> * Sehen Sie sich die kostenlose [Azure Time Series Insights-Demo](https://insights.timeseries.azure.com/samples) an.
> * Lesen Sie die zugehörige [Azure Time Series Insights-Explorer](./time-series-insights-explorer.md)-Dokumentation.

## <a name="environment-id"></a>Umgebungs-ID

Mit dem Parameter `environmentId=<guid>` wird die ID der Zielumgebung angegeben. Sie ist eine Komponente des FQDN für den Datenzugriff und im Azure-Portal oben rechts in der Umgebungsübersicht zu finden. Es handelt sich dabei um den gesamten Teil vor `env.timeseries.azure.com`.

Ein Beispielparameter für die Umgebungs-ID ist `?environmentId=10000000-0000-0000-0000-100000000108`.

## <a name="time"></a>Time

Sie können mit einer parametrisierten URL absolute oder relative Zeitwerte angeben.

### <a name="absolute-time-values"></a>Absolute Zeitwerte

Verwenden Sie für absolute Zeitwerte die Parameter `from=<integer>` und `to=<integer>`.

* `from=<integer>` ist ein Wert in JavaScript-Millisekunden für die Startzeit der Suchzeitspanne.
* `to=<integer>` ist ein Wert in JavaScript-Millisekunden für die Endzeit der Suchzeitspanne.

> [!TIP]
> Um Daten problemlos in JavaScript-Millisekunden zu übersetzen, testen Sie den [Epoch & UNIX Timestamp Converter](https://www.freeformatter.com/epoch-timestamp-to-date-converter.html).

### <a name="relative-time-values"></a>Relative Zeitwerte

Verwenden Sie für einen relativen Zeitwert `relativeMillis=<value>`, wobei *value* für einen Wert in JavaScript-Millisekunden aus dem aktuellsten von der API erhaltenen Zeitstempel steht.

Für `&relativeMillis=3600000` werden beispielsweise die Daten der letzten 60 Minuten angezeigt.

Die akzeptierten Werte entsprechen dem Menü **quick time** (Zeitauswahl) im Azure Time Series Insights-Explorer und umfassen Folgendes:

* `1800000` (Letzte 30 Minuten)
* `3600000` (Letzte 60 Minuten)
* `10800000` (Letzte 3 Stunden)
* `21600000` (Letzte 6 Stunden)
* `43200000` (Letzte 12 Stunden)
* `86400000` (Letzte 24 Stunden)
* `604800000` (Letzte 7 Tage)
* `2592000000` (Letzte 30 Stunden)

### <a name="optional-parameters"></a>Optionale Parameter

Der Parameter `timeSeriesDefinitions=<collection of term objects>` gibt Prädikatbegriffe an, die in einer Azure Time Series Insights-Ansicht angezeigt werden:

| Parameter | URL-Element | BESCHREIBUNG |
| --- | --- | --- |
| **name** | `\<string>` | Der Name der *Bedingung*. |
| **splitBy** | `\<string>` | Der Spaltenname für *Teilen nach*. |
| **measureName** | `\<string>` | Der Spaltenname des *Measure*. |
| **predicate** | `\<string>` | Die *where*-Klausel für die serverseitige Filterung. |
| **useSum** | `true` | Ein optionaler Parameter, der die Verwendung von „Summe“ für Ihr Measure angibt. |

> [!NOTE]
> Wenn `Events` das ausgewählte **useSum**-Measure ist, ist standardmäßig „Anzahl“ ausgewählt.
> Wenn `Events` nicht ausgewählt wurde, ist standardmäßig „Durchschnitt“ ausgewählt. |

* Das Schlüssel-Wert-Paar `multiChartStack=<true/false>` aktiviert das Stapeln im Diagramm.
* Das Schlüssel-Wert-Paar `multiChartSameScale=<true/false>` aktiviert dieselbe Y-Achsenskala begriffsübergreifend Innerhalb eines optionalen Parameters.
* Mit `timeBucketUnit=<Unit>&timeBucketSize=<integer>` können Sie den Intervallschieberegler anpassen, um eine präzisere oder eine glattere, stärker aggregierte Darstellung des Diagramms zu erhalten.
* Mit dem Parameter `timezoneOffset=<integer>` können Sie die gewünschte Zeitzone für das Diagrammdarstellung als UTC-Versatz festlegen.

| Paar(e) | BESCHREIBUNG |
| --- | --- |
| `multiChartStack=false` | Weil `true` standardmäßig aktiviert ist,übergeben Sie `false` an den Stapel. |
| `multiChartStack=false&multiChartSameScale=true` | Das Stapeln muss aktiviert sein, um ausdrucksübergreifend die gleiche Y-Achsenskala verwenden zu können.  Diese Funktion ist standardmäßig auf `false` festgelegt. Durch Übergeben von `true` wird sie aktiviert. |
| `timeBucketUnit=<Unit>&timeBucketSize=<integer>` | Einheiten = `days`, `hours`, `minutes`, `seconds`, `milliseconds`.  Die Einheit muss immer groß geschrieben werden. </br> Übergeben Sie zum Definieren der Anzahl von Einheiten die gewünschte ganze Zahl für **timeBucketSize**.  |
| `timezoneOffset=-<integer>` | Bei der ganzen Zahl handelt es sich immer um Millisekunden. |

> [!NOTE]
> **timeBucketUnit**-Werte können bis zu sieben Tage lang geglättet werden.
> **timezoneOffset**-Werte sind weder UTC noch Ortszeit.

### <a name="examples"></a>Beispiele

Sie können einer Azure Time Series Insights-Umgebung Zeitreihendefinitionen als URL-Parameter hinzuzufügen, indem Sie Folgendes anfügen:

```URL parameter
&timeSeriesDefinitions=[{"name":"F1PressureId","splitBy":"Id","measureName":"Pressure","predicate":"'Factory1'"},{"name":"F2TempStation","splitBy":"Station","measureName":"Temperature","predicate":"'Factory2'"},
{"name":"F3VibrationPL","splitBy":"ProductionLine","measureName":"Vibration","predicate":"'Factory3'"}]
```

Verwenden Sie das Beispiel zu Zeitreihendefinitionen für:

* Die Umgebungs-ID
* Die Daten der letzten 60 Minuten
* Die Begriffe (**F1PressureID**, **F2TempStation** und **F3VibrationPL**), aus denen die optionalen Parameter bestehen

Mit diesen Elementen können Sie die folgende parametrisierte URL für eine Ansicht erstellen:

```URL
https://insights.timeseries.azure.com/classic/samples?environmentId=10000000-0000-0000-0000-100000000108&relativeMillis=3600000&timeSeriesDefinitions=[{"name":"F1PressureId","splitBy":"Id","measureName":"Pressure","predicate":"'Factory1'"},{"name":"F2TempStation","splitBy":"Station","measureName":"Temperature","predicate":"'Factory2'"},{"name":"F3VibrationPL","splitBy":"ProductionLine","measureName":"Vibration","predicate":"'Factory3'"}]
```

[![Parametrisierte URL des Azure Time Series Insights-Explorers](media/parameterized-url/share-parameterized-url.png)](media/parameterized-url/share-parameterized-url.png#lightbox)

> [!TIP]
> Sehen Sie sich den Explorer [mithilfe der Beispiel-URL](https://insights.timeseries.azure.com/classic/samples?environmentId=10000000-0000-0000-0000-100000000108&relativeMillis=3600000&timeSeriesDefinitions=[%7B%22name%22:%22F1PressureId%22,%22splitBy%22:%22Id%22,%22measureName%22:%22Pressure%22,%22predicate%22:%22%27Factory1%27%22%7D,%7B%22name%22:%22F2TempStation%22,%22splitBy%22:%22Station%22,%22measureName%22:%22Temperature%22,%22predicate%22:%22%27Factory2%27%22%7D,%7B%22name%22:%22F3VibrationPL%22,%22splitBy%22:%22ProductionLine%22,%22measureName%22:%22Vibration%22,%22predicate%22:%22%27Factory3%27%22%7D]
) oben live an.

Die URL oben beschreibt und erstellt die parametrisierte Azure Time Series Insights-Explorer-Ansicht.

* Die parametrisierten Prädikate.

  [![Parametrisierte Prädikate im Azure Time Series Insights-Explorer.](media/parameterized-url/share-parameterized-url-predicates.png)](media/parameterized-url/share-parameterized-url-predicates.png#lightbox)

* Die freigegebene vollständige Diagrammansicht.

  [![Die freigegebene vollständige Diagrammansicht.](media/parameterized-url/share-parameterized-url-full-chart.png)](media/parameterized-url/share-parameterized-url-full-chart.png#lightbox)

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich über das [Abfragen von Daten mithilfe von C#](time-series-insights-query-data-csharp.md).

* Informieren Sie sich über den [Azure Time Series Insights-Explorer](./time-series-insights-explorer.md).
