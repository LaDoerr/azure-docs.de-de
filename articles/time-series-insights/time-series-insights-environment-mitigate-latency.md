---
title: Überwachen und Reduzieren der Drosselung – Azure Time Series Insights | Microsoft-Dokumentation
description: Erfahren Sie, wie Leistungsprobleme überwacht, diagnostiziert und verringert werden können, die zu Latenz und Drosselung in Azure Time Series Insights führen.
ms.service: time-series-insights
services: time-series-insights
author: tedvilutis
ms.author: tvilutis
manager: cnovak
ms.reviewer: orspodek
ms.devlang: csharp
ms.workload: big-data
ms.topic: troubleshooting
ms.date: 09/29/2020
ms.custom: seodec18
ms.openlocfilehash: 12d03576d3522146550356b62d6bb4d9eb7f14b3
ms.sourcegitcommit: 4f185f97599da236cbed0b5daef27ec95a2bb85f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2021
ms.locfileid: "112368861"
---
# <a name="monitor-and-mitigate-throttling-to-reduce-latency-in-azure-time-series-insights-gen1"></a>Überwachen und Mindern der Drosselung zur Verhinderung von Wartezeit in Azure Time Series Insights Gen1

> [!CAUTION]
> Dies ist ein Artikel zu Azure Time Series Insights Gen1.

Wenn die Menge der eingehenden Daten die Konfiguration Ihrer Umgebung übersteigt, kann dies zu Latenz oder Drosselung in Azure Time Series Insights führen.

Sie können Latenz und Drosselung vermeiden, indem Sie die Umgebung korrekt für die Menge der zu analysierenden Daten konfigurieren.

Am wahrscheinlichsten treten Latenz und Drosselung in folgenden Fällen auf:

- Sie fügen eine Ereignisquelle hinzu, die alte Daten enthält, die möglicherweise die zugewiesene Eingangsrate überschreiten. (Azure Time Series Insights muss auf den neuesten Stand gebracht werden.)
- Sie fügen einer Umgebung weitere Ereignisquellen hinzu, was zu einer Spitze durch zusätzliche Ereignisse führt (die die Kapazität der Umgebung überschreiten kann).
- Sie übertragen große Mengen an Verlaufsereignissen in eine Ereignisquelle, was zu einer Verzögerung führt. (Azure Time Series Insights muss auf den neuesten Stand gebracht werden.)
- Sie verknüpfen Verweisdaten mit Telemetriedaten, was zu einer umfangreicheren Ereignisgröße führt. Die maximal zulässige Paketgröße ist 32 KB. Datenpakete mit einer Größe von über 32 KB werden abgeschnitten.

## <a name="video"></a>Video

### <a name="learn-about-azure-time-series-insights-data-ingress-behavior-and-how-to-plan-for-itbr"></a>Erfahren Sie mehr über das Azure Time Series Insights-Dateneingangsverhalten und dessen Planung.</br>

> [!VIDEO https://www.youtube.com/embed/npeZLAd9lxo]

## <a name="monitor-latency-and-throttling-with-alerts"></a>Überwachen von Latenz und Drosselung mit Warnungen

Mithilfe von Warnungen können Sie in Ihrer Umgebung auftretende Latenzprobleme diagnostizieren und verringern.

1. Wählen Sie im Azure-Portal Ihre Azure Time Series Insights-Umgebung aus. Wählen Sie dann **Warnungen** aus.

   [![Hinzufügen einer Warnung zu Ihrer Azure Time Series Insights-Umgebung](media/environment-mitigate-latency/mitigate-latency-add-alert.png)](media/environment-mitigate-latency/mitigate-latency-add-alert.png#lightbox)

1. Wählen Sie **+ Neue Warnungsregel** aus. Daraufhin wird der Bereich **Regel erstellen** angezeigt. Wählen Sie unter **BEDINGUNG** die Option **Hinzufügen** aus.

   [![Bereich „Warnung hinzufügen“](media/environment-mitigate-latency/mitigate-latency-add-pane.png)](media/environment-mitigate-latency/mitigate-latency-add-pane.png#lightbox)

1. Konfigurieren Sie als Nächstes die genauen Bedingungen für die Signallogik.

   [![Signallogik konfigurieren](media/environment-mitigate-latency/configure-alert-rule.png)](media/environment-mitigate-latency/configure-alert-rule.png#lightbox)

   Hier können Sie Warnungen mit einigen der folgenden Bedingungen konfigurieren:

   |Metrik  |BESCHREIBUNG  |
   |---------|---------|
   |**Ingress Received Bytes** (Eingang empfangene Bytes)     | Anzahl der aus Ereignisquellen gelesenen Rohbytes. Die Anzahl der Rohbytes umfasst normalerweise den Eigenschaftennamen und den Eigenschaftswert.  |
   |**Ingress Received Invalid Messages** (Eingang empfangene ungültige Nachrichten)     | Anzahl der aus allen Azure Event Hubs- oder Azure IoT Hub-Ereignisquellen gelesenen ungültigen Nachrichten.      |
   |**Ingress Received Messages** (Eingang empfangene Nachrichten)   | Anzahl der aus allen Event Hubs- oder IoT Hub-Ereignisquellen gelesenen Nachrichten.        |
   |**Ingress Stored Bytes** (Eingang gespeicherte Bytes)     | Gesamtgröße der gespeicherten und für Abfragen verfügbaren Ereignisse. Die Größe wird lediglich für den Eigenschaftswert berechnet.        |
   |**Ingress Stored Events** (Eingang gespeicherte Ereignisse)     |   Anzahl der gespeicherten und für Abfragen verfügbaren vereinfachten Ereignisse.      |
   |**Eingang Zeitverzögerung für empfangene Nachrichten**    |  Der Unterschied in Sekunden zwischen dem Zeitpunkt, zu dem die Nachricht in der Ereignisquelle in die Warteschlange eingereiht wird, und dem Zeitpunkt der Verarbeitung im Eingang.      |
   |**Eingang Verzögerung aufgrund der Anzahl empfangener Nachrichten**    |  Dies ist der Unterschied zwischen der Sequenznummer der Nachricht, die auf der Ereignisquelle zuletzt in die Warteschlange eingereiht wurde, und der Sequenznummer der im Eingang verarbeiteten Nachricht.      |

   Wählen Sie **Fertig** aus.

1. Nachdem Sie die gewünschte Signallogik konfiguriert haben, überprüfen Sie die ausgewählte Warnungsregel visuell.

   [![Wartezeitansicht und Diagrammerstellung](media/environment-mitigate-latency/mitigate-latency-view-and-charting.png)](media/environment-mitigate-latency/mitigate-latency-view-and-charting.png#lightbox)

## <a name="throttling-and-ingress-management"></a>Drosselung und Eingangsverwaltung

- Im Falle einer Drosselung wird ein Wert für *Ingress Received Message Time Lag* (Zeitverzögerung empfangene Eingangsnachrichten) registriert, um Sie darüber zu informieren, wie viele Sekunden Ihre Azure Time Series Insights-Umgebung hinter dem tatsächlichen Zeitpunkt zurückliegt, zu dem die Nachricht bei der Ereignisquelle eintrifft (ohne Indizierungszeitraum von ca. 30 bis 60 Sekunden).

  *Eingang Verzögerung aufgrund der Anzahl empfangener Nachrichten* sollte ebenfalls über einen Wert verfügen, sodass Sie bestimmen können, um wie viele Nachrichten Sie zurückliegen.  Die einfachste Möglichkeit zum Aufholen des Rückstands besteht darin, die Kapazität Ihrer Umgebung auf eine Einstellung zu erhöhen, die Ihnen das Wettmachen des Unterschieds ermöglicht.

  Wenn Ihre S1-Umgebung also beispielsweise einen Rückstand von 5.000.000 Nachrichten aufweist, können Sie die Größe Ihrer Umgebung ungefähr einen Tag lang auf sechs Einheiten erhöhen, um den Rückstand aufzuholen.  Sie können die Größe auch stärker erhöhen, um den Rückstand schneller wettzumachen. Der Abgleichszeitraum tritt bei der anfänglichen Bereitstellung einer Umgebung häufig auf, z.B. wenn Sie diesen mit einer Ereignisquelle verbinden, die bereits Ereignisse enthält, oder wenn Sie einen Massenupload für viele Verlaufsdaten durchführen.

- Eine andere Möglichkeit besteht darin, eine Warnung vom Typ **Ingress Stored Events** (Eingang gespeicherte Ereignisse) für einen Zeitraum von zwei Stunden so festzulegen, dass der Schwellenwert leicht unterhalb der Gesamtkapazität Ihrer Umgebung liegt.  Anhand dieser Warnung können Sie feststellen, ob die Kapazität konstant ausgelastet ist. Dies gibt eine hohe Wahrscheinlichkeit von Latenz an.

  Wenn Sie beispielsweise drei S1-Einheiten (oder eine Eingangskapazität von 2.100 Ereignissen pro Minute) bereitgestellt haben, können Sie eine Warnung vom Typ **Ingress Stored Events** (Eingang gespeicherte Ereignisse) >= 1.900 Ereignisse für 2 Stunden festlegen. Wenn dieser Schwellenwert konstant überschritten und dadurch die Warnung ausgelöst wird, ist die Bereitstellung wahrscheinlich nicht ausreichend.

- Wenn Sie vermuten, dass eine Drosselung vorliegt, können Sie die Warnung vom Typ **Ingress Received Messages** (Eingang empfangene Nachrichten) mit den ausgehenden Nachrichten der Ereignisquelle vergleichen.  Wenn der Eingang beim Event Hub größer ist als der Wert für **Ingress Received Messages** (Eingang empfangene Nachrichten), wird der Azure Time Series Insights-Dienst wahrscheinlich gedrosselt.

## <a name="improving-performance"></a>Verbessern der Leistung

Um die Drosselung oder Latenz zu reduzieren, empfiehlt es sich, dies durch Erhöhen der Kapazität der Umgebung zu beheben.

Sie können Latenz und Drosselung vermeiden, indem Sie die Umgebung korrekt für die Menge der zu analysierenden Daten konfigurieren. Weitere Informationen zum Hinzufügen von Kapazität zu Ihrer Umgebung finden Sie unter [Skalieren Ihrer Umgebung](time-series-insights-how-to-scale-your-environment.md).

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über das [Diagnostizieren und Beheben von Problemen in der Azure Time Series Insights-Umgebung](time-series-insights-diagnose-and-solve-problems.md).

- Machen Sie sich mit der [Vorgehensweise zur Skalierung Ihrer Azure Time Series Insights-Umgebung](time-series-insights-how-to-scale-your-environment.md) vertraut.
