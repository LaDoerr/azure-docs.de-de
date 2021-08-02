---
title: Überwachen der Verfügbarkeit mit URL-Pingtests – Azure Monitor
description: Richten Sie Pingtests in Application Insights ein. Erhalten Sie Benachrichtigungen, wenn eine Website nicht mehr zur Verfügung steht oder langsam reagiert.
ms.topic: conceptual
ms.date: 05/25/2021
ms.reviewer: sdash
ms.openlocfilehash: e7e52cb5db6db5fbd1e73713d3d3558be9e859f0
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111968403"
---
# <a name="monitor-availability-with-url-ping-tests"></a>Überwachen der Verfügbarkeit mit URL-Pingtests

Der Begriff „URL-Pingtest“ ist irreführend. Diese Tests setzen nämlich kein ICMP (Internet Control Message Protocol) ein, um die Verfügbarkeit der jeweiligen Website zu prüfen. Stattdessen verwenden sie erweiterte Funktionen von HTTP-Anforderungen, um zu überprüfen, ob ein Endpunkt reagiert. Außerdem wird die Leistung gemessen, die in Zusammenhang mit dieser Antwort steht, und es können benutzerdefinierte Erfolgskriterien festgelegt werden, die an erweiterte Features wie das Analysieren von abhängigen Anforderungen und das Zulassen von Wiederholungsversuchen gekoppelt sind.

Um einen Verfügbarkeitstest zu erstellen, müssen Sie eine vorhandene Application Insight-Ressource verwenden oder [eine Application Insights-Ressource erstellen.](create-new-resource.md)

Öffnen Sie den Bereich „Verfügbarkeit“, und klicken Sie auf   **Test erstellen**, um Ihre erste Verfügbarkeitsanforderung zu erstellen.

:::image type="content" source="./media/monitor-web-app-availability/availability-create-test-001.png" alt-text="Screenshot: Erstellen eines Tests.":::

## <a name="create-a-test"></a>Erstellen eines Tests

|Einstellung| Erklärung
|----|----|----|
|**URL** |  Die URL kann zu einer beliebigen Webseite führen, die Sie testen möchten, aber sie muss im öffentlichen Internet sichtbar sein. Die URL kann eine Abfragezeichenfolge enthalten. So können Sie beispielsweise Ihre Datenbank abfragen. Wenn die URL in eine Umleitung aufgelöst wird, werden bis zu 10 Umleitungen verfolgt.|
|**Abhängige Anforderungen analysieren**| Der Test fordert Bilder, Skripts, Formatdateien und andere Dateien an, die Teil der zu testenden Webseite sind. Die aufgezeichnete Antwortzeit enthält auch die Zeit, die zum Abrufen dieser Dateien erforderlich ist. Der Test schlägt fehl, wenn eine dieser Ressourcen innerhalb des Zeitlimits für den gesamten Test nicht erfolgreich heruntergeladen werden kann. Wenn die Option nicht aktiviert ist, wird beim Test nur die Datei unter der von Ihnen angegebenen URL angefordert. Wenn diese Option aktiviert wird, wird strenger geprüft. Der Test könnte in Fällen fehlschlagen, die möglicherweise beim manuellen Durchsuchen der Website nicht auftreten würden.
|**Enable retries** (Wiederholungen aktivieren)|Wenn der Test fehlschlägt, wird er nach kurzer Zeit wiederholt. Nur wenn drei aufeinander folgende Versuche scheitern, wird ein Fehler gemeldet. Nachfolgende Tests werden dann in der üblichen Häufigkeit ausgeführt. Die Wiederholung wird bis zum nächsten Erfolg vorübergehend eingestellt. Diese Regel wird an jedem Teststandort unabhängig angewendet. **Es wird empfohlen, diese Option zu verwenden.** Im Durchschnitt treten ca. 80% der Fehler bei einer Wiederholung nicht mehr auf.|
|**Testhäufigkeit**| Legt fest, wie oft der Test von jedem Teststandort aus ausgeführt wird. Mit einer Standardfrequenz von fünf Minuten und fünf Teststandorten wird Ihre Website im Durchschnitt jede Minute getestet.|
|**Teststandorte**| Die Orte, von denen aus unsere Server Webanforderungen an Ihre URL senden. **Es wird empfohlen, mindestens fünf Teststandorte festzulegen**, um sicherzustellen, dass Sie Probleme mit Ihrer Website von Netzwerkproblemen unterscheiden können. Sie können bis zu 16 Standorte auswählen.

**Wenn Ihre URL im öffentlichen Internet nicht sichtbar ist, können auswählen, dass Ihre Firewall geöffnet werden soll, sodass nur die Testtransaktionen passieren können.** Weitere Informationen zu Firewallausnahmen für unsere Agents für Verfügbarkeitstests finden Sie im [Leitfaden für Verfügbarkeitstests](./ip-addresses.md#availability-tests).

> [!NOTE]
> Es wird dringend empfohlen, Tests an mehreren Standorten auszuführen **(mindestens fünf)** . Dies dient dazu, Fehlalarme zu vermeiden, die durch vorübergehende Probleme an einem bestimmten Standort entstehen können. Darüber hinaus haben wir festgestellt, dass in einer optimalen Konfiguration die **Anzahl von Teststandorten dem Warnungsschwellenwert für Standorte + 2 entspricht**.

## <a name="success-criteria"></a>Erfolgskriterien

|Einstellung| Erklärung
|----|----|----|
| **Testtimeout** |Reduzieren Sie diesen Wert, um über langsame Antworten benachrichtigt zu werden. Der Test wird als ein Fehler gezählt, wenn die Antworten von Ihrer Website nicht innerhalb dieses Zeitraums empfangen wurden. Bei Auswahl von **Abhängige Anforderungen analysieren** müssen alle Bilder, Styledateien, Skripts und anderen abhängigen Ressourcen innerhalb dieses Zeitraums empfangen werden.|
| **HTTP-Antwort** | Der zurückgegebene Statuscode, der als Erfolg gezählt wird. 200 ist der Code, der angibt, dass eine normale Webseite zurückgegeben wurde.|
| **Inhaltsübereinstimmung** | Eine Zeichenfolge, z. B. „Willkommen!“ Wir vergewissern uns, dass in jeder Antwort eine exakte Übereinstimmung unter Berücksichtigung der Groß-und Kleinschreibung vorkommt. Dies muss eine Zeichenfolge in Klartext, ohne Platzhalter sein. Vergessen Sie nicht, diese zu aktualisieren, wenn sich der Seiteninhalt ändert. **Inhaltsübereinstimmungen werden nur für englische Zeichen unterstützt.** |

## <a name="alerts"></a>Alerts

|Einstellung| Erklärung
|----|----|----|
|**Near-realtime (Preview)** (Nahezu in Echtzeit (Vorschauversion)) | Es wird empfohlen, Warnungen zu verwenden, die nahezu in Echtzeit angezeigt werden. Diese Art von Warnung ist bereits vollständig konfiguriert, sobald Sie einen Verfügbarkeitstest erstellt haben.  |
|**Schwellenwert für den Warnungsspeicherort**|Es wird ein Mindestwert von 3/5 Standorten empfohlen. Das optimale Verhältnis zwischen dem Schwellenwert für den Warnungsspeicherort und der Anzahl von Teststandorten lautet **Warnungsschwellenwert für Standort** = **Anzahl von Teststandorten -2, bei einer Mindestanzahl von fünf Teststandorten.**|

## <a name="location-population-tags"></a>Auffüllungstags für den Standort

Beim Bereitstellen eines URL-Pingtests für die Verfügbarkeit mithilfe von Azure Resource Manager können für das Attribut für den geografischen Standort die folgenden Auffüllungstags verwendet werden.

### <a name="azure-gov"></a>Azure Gov

| Anzeigename   | Auffüllungsname     |
|----------------|---------------------|
| US Government, Virginia | usgov-va-azr        |
| US Gov Arizona  | usgov-phx-azr       |
| USGov Texas    | usgov-tx-azr        |
| USDoD, Osten     | usgov-ddeast-azr    |
| USDoD, Mitte  | usgov-ddcentral-azr |

### <a name="azure-china"></a>Azure China

| Anzeigename   | Auffüllungsname     |
|----------------|---------------------|
| China, Osten     | mc-cne-azr          |
| China, Osten 2   | mc-cne2-azr         |
| China, Norden    | mc-cnn-azr          |
| China, Norden 2  | mc-cnn2-azr         |

#### <a name="azure"></a>Azure

| Anzeigename                           | Auffüllungsname   |
|----------------------------------------|-------------------|
| Australien, Osten                         | emea-au-syd-edge  |
| Brasilien Süd                           | latam-br-gru-edge |
| USA (Mitte)                             | us-fl-mia-edge    |
| Asien, Osten                              | apac-hk-hkn-azr   |
| East US                                | us-va-ash-azr     |
| Frankreich, Süden (ehemals Frankreich, Mitte) | emea-ch-zrh-edge  |
| Frankreich, Mitte                         | emea-fr-pra-edge  |
| Japan, Osten                             | apac-jp-kaw-edge  |
| Nordeuropa                           | emea-gb-db3-azr   |
| USA Nord Mitte                       | us-il-ch1-azr     |
| USA Süd Mitte                       | us-tx-sn1-azr     |
| Asien, Südosten                         | apac-sg-sin-azr   |
| UK, Westen                                | emea-se-sto-edge  |
| Europa, Westen                            | emea-nl-ams-azr   |
| USA (Westen)                                | us-ca-sjc-azr     |
| UK, Süden                               | emea-ru-msa-edge  |

## <a name="see-your-availability-test-results"></a>Anzeigen der Verfügbarkeitstestergebnisse

Verfügbarkeitstestergebnisse können sowohl in Zeilenansichten als auch in Punktdiagrammen visualisiert werden.

Klicken Sie nach einigen Minuten auf **Aktualisieren**, um Ihre Testergebnisse anzuzeigen.

:::image type="content" source="./media/monitor-web-app-availability/availability-refresh-002.png" alt-text="Screenshot: Seite „Verfügbarkeit“, auf der die Schaltfläche „Aktualisieren“ hervorgehoben ist":::

Das Punktdiagramm zeigt Stichproben der Testergebnisse an, die Diagnosedetails zu Testschritten enthalten. Die Test-Engine speichert Diagnosedetails für Tests mit Fehlern. Für erfolgreiche Tests werden Diagnosedetails für eine Teilmenge der Ausführungen gespeichert. Bewegen Sie den Mauszeiger über einen der grünen oder roten Punkte, um den Test, den Testnamen und den Standort anzuzeigen.

:::image type="content" source="./media/monitor-web-app-availability/availability-scatter-plot-003.png" alt-text="Zeilenansicht." border="false":::

Wählen Sie einen bestimmten Test oder Standort aus, oder verringern Sie den Zeitraum, um weitere Ergebnisse um den gewünschten Zeitraum anzuzeigen. Verwenden Sie den Suchexplorer, um Ergebnisse von allen Ausführungen anzuzeigen, oder Analytics-Abfragen, um benutzerdefinierte Berichte für diese Daten auszuführen.

## <a name="inspect-and-edit-tests"></a>Überprüfen und Bearbeiten von Tests

Wenn Sie einen Test bearbeiten, vorübergehend deaktivieren oder löschen möchten, klicken Sie auf die drei Punkte neben dem jeweiligen Testnamen. Es kann bis zu 20 Minuten dauern, bis Konfigurationsänderungen an alle Test-Agent weitergegeben werden.

:::image type="content" source="./media/monitor-web-app-availability/edit.png" alt-text="Testdetails anzeigen, Webtest bearbeiten und deaktivieren." border="false":::

Eventuell möchten Sie Verfügbarkeitstests oder die damit verknüpften Warnungsregeln deaktivieren, während Sie Ihren Dienst warten.

## <a name="if-you-see-failures"></a>Anleitung zum Vorgehen bei Fehlern

Einen roten Punkt auswählen.

:::image type="content" source="./media/monitor-web-app-availability/end-to-end.png" alt-text="Screenshot: Details der Registerkarte End-to-End-Transaktionen." border="false":::

Aus einem Verfügbarkeitstestergebnis können Sie die Transaktionsdetails für alle Komponenten ablesen. Mögliche nächste Schritte:

* Überprüfen Sie den Bericht zur Problembehandlung, um zu ermitteln, was möglicherweise dazu geführt hat, dass der Test fehlschlägt, obwohl Ihre Anwendung aber weiterhin verfügbar ist.
* Untersuchen Sie die vom Server erhaltene Antwort.
* Diagnostizieren Sie den Fehler mit korrelierten serverseitigen Telemetriedaten, die während der Verarbeitung des fehlerhaften Verfügbarkeitstests gesammelt wurden.
* Protokollieren Sie in Git oder Azure Boards ein Problem oder ein Arbeitselement, um das Problem nachzuverfolgen. Der Fehler enthält einen Link zu diesem Ereignis.
* Öffnen Sie das Webtestergebnis in Visual Studio.

Weitere Informationen zur End-to-End-Transaktionsdiagnose finden Sie in der Dokumentation zur [Transaktionsdiagnose](./transaction-diagnostics.md).

Klicken Sie auf die Ausnahmezeile, um die Details der serverseitigen Ausnahme anzuzeigen, die zum Fehlschlagen des synthetischen Verfügbarkeitstest geführt hat. Sie können auch die [Debugmomentaufnahme](./snapshot-debugger.md) abrufen, um eine umfangreichere Diagnose auf Codeebene durchzuführen.

:::image type="content" source="./media/monitor-web-app-availability/open-instance-4.png" alt-text="Serverseitige Diagnose.":::

Zusätzlich zu den reinen Ergebnissen können Sie im [Metrik-Explorer](../essentials/metrics-getting-started.md) zwei wichtige Verfügbarkeitsmetriken abrufen:

1. Verfügbarkeit: Prozentsatz der erfolgreichen Tests für alle Testausführungen.
2. Testdauer: Durchschnittliche Testdauer für alle Testausführungen.

## <a name="automation"></a>Automation

* [Verwenden Sie PowerShell-Skripts zum automatischen Einrichten eines Verfügbarkeitstests](./powershell.md#add-an-availability-test).
* Richten Sie einen [Webhook](../alerts/alerts-webhooks.md) ein, der bei einer Warnung aufgerufen wird.


## <a name="next-steps"></a>Nächste Schritte

* [Availability Alerts (Verfügbarkeitswarnungen)](availability-alerts.md)
* [Multi-step web tests (Mehrstufige Webtests)](availability-multistep.md)
* [Problembehandlung](troubleshoot-availability.md)
* [Webtests: Azure Resource Manager-Vorlage](/azure/templates/microsoft.insights/webtests?tabs=json)