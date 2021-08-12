---
title: 'Schnellstart: Azure Time Series Insights-Explorer: Azure Time Series Insights | Microsoft-Dokumentation'
description: Erfahren Sie mehr zum Einstieg in den Azure Time Series Insights-Explorer. Visualisieren Sie große Mengen von IoT-Daten, und lernen Sie die wichtigsten Features Ihrer Umgebung anhand einer Tour kennen.
ms.service: time-series-insights
services: time-series-insights
author: narmeens
ms.author: narsam
manager: cnovak
ms.reviewer: orspodek
ms.topic: quickstart
ms.workload: big-data
ms.custom: mvc seodec18
ms.date: 09/30/2020
ms.openlocfilehash: cf8e8cac032ab303f39a414afdaba2b4cd730844
ms.sourcegitcommit: 4f185f97599da236cbed0b5daef27ec95a2bb85f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2021
ms.locfileid: "112369000"
---
# <a name="quickstart-explore-azure-time-series-insights-gen1"></a>Schnellstart: Erkunden von Azure Time Series Insights Gen1

> [!CAUTION]
> Dies ist ein Artikel zu Azure Time Series Insights Gen1.

Dieser Schnellstart für den Azure Time Series Insights-Explorer unterstützt Sie bei den ersten Schritten mit Azure Time Series Insights in einer kostenlosen Demoumgebung. In dieser Schnellstartanleitung erfahren Sie, wie Sie große IoT-Datenmengen im Webbrowser visualisieren, und lernen die wichtigsten Features kennen, die nun allgemein verfügbar sind.

Azure Time Series Insights ist ein vollständig verwalteter Analyse-, Speicher- und Visualisierungsdienst, der die simultane Untersuchung und Analyse von Milliarden von IoT-Ereignissen vereinfacht. Sie erhalten eine globale Ansicht Ihrer Daten und können basierend darauf Ihre IoT-Lösung schneller überprüfen und kostenintensive Ausfallzeiten unternehmenskritischer Geräte vermeiden. Mit Azure Time Series Insights können Sie versteckte Trends aufdecken, Anomalien erkennen und Ursachenanalysen nahezu in Echtzeit durchführen.

Zur Erhöhung der Flexibilität kann Azure Time Series Insights über seine leistungsfähigen [REST-APIs](./concepts-query-overview.md) und das [Client-SDK](https://github.com/microsoft/tsiclient) einer bereits vorhandenen Anwendung hinzugefügt werden. Die APIs ermöglichen es Ihnen, Zeitreihendaten in einer Clientanwendung Ihrer Wahl zu speichern, abzufragen und zu nutzen. Sie können auch das Client-SDK verwenden, um Ihrer vorhandenen Anwendung Benutzeroberflächenkomponenten hinzuzufügen.

Dieser Schnellstart zum Azure Time Series Insights-Explorer bietet eine Einführung in die Features.

> [!IMPORTANT]
> Erstellen Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio), sofern noch nicht geschehen.

## <a name="prepare-the-demo-environment"></a>Vorbereiten der Demoumgebung

1. Navigieren Sie in Ihrem Browser zur [Gen1-Demo](https://insights.timeseries.azure.com/demo).

1. Wenn Sie dazu aufgefordert werden, melden Sie sich mit Ihren Azure-Kontoanmeldeinformationen beim Azure Time Series Insights-Explorer an.

1. Die Seite zur Schnelleinführung in Azure Time Series Insights wird angezeigt. Wählen Sie **Weiter** aus, um die Schnelleinführung zu starten.

   [![Willkommensseite der Schnellstartanleitung: Auswählen von „Weiter“](media/quickstart/quickstart-welcome.png)](media/quickstart/quickstart-welcome.png#lightbox)

## <a name="explore-the-demo-environment"></a>Erkunden der Demoumgebung

1. Der **Zeitauswahlbereich** wird angezeigt. Wählen Sie in diesem Bereich den Zeitrahmen aus, der dargestellt werden soll.

   [![Zeitauswahlbereich](media/quickstart/quickstart-time-selection-panel.png)](media/quickstart/quickstart-time-selection-panel.png#lightbox)

1. Wählen Sie einen Zeitrahmen aus, und ziehen Sie ihn in den Bereich. Wählen Sie anschließend **Suchen** aus.

   [![Auswählen eines Zeitrahmens](media/quickstart/quickstart-select-time.png)](media/quickstart/quickstart-select-time.png#lightbox)

   In Azure Time Series Insights wird eine Diagrammvisualisierung für den angegebenen Zeitrahmen angezeigt. Sie können verschiedene Aktionen im Liniendiagramm ausführen. Nutzen Sie beispielsweise Optionen zum Filtern, Anheften, Sortieren und Stapeln.

   Um zum **Zeitauswahlbereich** zurückzukehren, wählen Sie wie dargestellt den Pfeil nach unten aus:

   [![Diagramm](media/quickstart/quickstart-select-down-arrow.png)](media/quickstart/quickstart-select-down-arrow.png#lightbox)

1. Wählen Sie im **Begriffsbereich** die Option **Hinzufügen** aus, um einen neuen Suchbegriff hinzuzufügen.

   [![Bereich „Suchbegriffe hinzufügen“](media/quickstart/quickstart-add-terms.png)](media/quickstart/quickstart-add-terms.png#lightbox)

1. Im Diagramm können Sie eine Region auswählen, mit der rechten Maustaste auf die Region klicken und **Explore Events** (Ereignisse durchsuchen) auswählen.

   [![Ereignisse untersuchen](media/quickstart/quickstart-explore-events.png)](media/quickstart/quickstart-explore-events.png#lightbox)

   Ein Raster der Rohdaten aus der durchsuchten Region wird angezeigt.

   [![Ereignisse untersuchen: Rasterdatenansicht](media/quickstart/quickstart-explore-events-grid-data.png)](media/quickstart/quickstart-explore-events-grid-data.png#lightbox)

## <a name="select-and-filter-data"></a>Auswählen und Filtern von Daten

1. Bearbeiten Sie Ihre Begriffe, um die Werte im Diagramm zu ändern. Fügen Sie einen weiteren Begriff zum übergreifenden Korrelieren verschiedener Werttypen hinzu.

   [![Hinzufügen eines Begriffs](media/quickstart/quickstart-add-a-term.png)](media/quickstart/quickstart-add-a-term.png#lightbox)

1. Lassen Sie das Feld **Datenreihe filtern** leer, um alle ausgewählten Suchbegriffe anzuzeigen, oder geben Sie im Feld **Datenreihe filtern** einen Filterbegriff ein, um eine improvisierte Filterung von Datenreihen durchzuführen.

   [![Datenreihen filtern](media/quickstart/quickstart-filter-series.png)](media/quickstart/quickstart-filter-series.png#lightbox)

   Geben Sie für den Schnellstart **Station5** ein, um die Temperatur und den Druck für die Station übergreifend zu korrelieren.

Nach Abschluss des Schnellstarts können Sie das Beispiel-DataSet verwenden, um verschiedene Visualisierungen zu erstellen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Bereinigen Sie nach dem Tutorial die von Ihnen erstellten Ressourcen:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) im Menü auf der linken Seite **Alle Ressourcen** aus, und suchen Sie Ihre Azure Time Series Insights-Ressourcengruppe.
1. Löschen Sie entweder die gesamte Ressourcengruppe (und alle darin enthaltenen Ressourcen), indem Sie auf **Löschen** klicken, oder entfernen Sie die Ressourcen einzeln.

## <a name="next-steps"></a>Nächste Schritte

* Sie sind jetzt dazu bereit, Ihre eigene Azure Time Series Insights-Umgebung zu erstellen. Weitere Informationen finden Sie unter [Planen Ihrer Azure Time Series Insights-Umgebung](time-series-insights-environment-planning.md).
