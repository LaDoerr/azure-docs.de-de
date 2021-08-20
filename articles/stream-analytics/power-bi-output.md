---
title: Power BI-Ausgabe für Azure Stream Analytics
description: In diesem Artikel wird beschrieben, wie Daten aus Azure Stream Analytics in Power BI ausgegeben werden.
author: enkrumah
ms.author: ebnkruma
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 7/7/2021
ms.openlocfilehash: d3f8bab950af095a457347b825966a0845ef36f6
ms.sourcegitcommit: 0ab53a984dcd23b0a264e9148f837c12bb27dac0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2021
ms.locfileid: "113506779"
---
# <a name="power-bi-output-from-azure-stream-analytics"></a>Power BI-Ausgabe für Azure Stream Analytics

Sie können [Power BI](https://powerbi.microsoft.com/) als Ausgabe für einen Stream Analytics-Auftrag verwenden, um eine umfassende Visualisierungsumgebung für die Analyseergebnisse bereitzustellen. Diese Funktionalität kann für Vorgangsdashboards, die Erstellung von Berichten und eine metrikgesteuerte Berichterstellung verwendet werden.

Die Power BI-Ausgabe aus Stream Analytics ist zurzeit nicht in den Regionen Azure China 21Vianet und Azure Deutschland (T-Systems International) verfügbar.

## <a name="output-configuration"></a>Ausgabekonfiguration

In der folgenden Tabelle sind Eigenschaftsnamen und deren Beschreibungen aufgeführt, um Ihre Power BI-Ausgabe zu konfigurieren.

| Eigenschaftenname | BESCHREIBUNG |
| --- | --- |
| Ausgabealias |Stellen Sie einen Anzeigenamen bereit, der in Abfragen verwendet wird, um die Abfrageausgabe an diese Power BI-Ausgabe weiterzuleiten. |
| Gruppenarbeitsbereich |Um die gemeinsame Datennutzung mit anderen Power BI-Benutzern zu ermöglichen, können Sie Gruppen in Ihrem Power BI-Konto auswählen. Wählen Sie alternativ **Mein Arbeitsbereich**, wenn Sie nicht in eine Gruppe schreiben möchten. Zum Aktualisieren einer vorhandenen Gruppe muss die Power BI-Authentifizierung erneuert werden. |
| Datasetname |Geben Sie einen Datasetnamen an, den die Power BI-Ausgabe verwenden soll. |
| Tabellenname |Geben Sie einen Tabellennamen unter dem Dataset der Power BI-Ausgabe ein. Derzeit darf die Power BI-Ausgabe von Stream Analytics-Aufträgen nur eine Tabelle pro Dataset aufweisen. |
| Autorisieren der Verbindung | Sie müssen die Autorisierung mit Power BI durchführen, um Ihre Ausgabeeinstellungen zu konfigurieren. Nachdem Sie für diese Ausgabe den Zugriff auf Ihr Power BI-Dashboard gewährt haben, können Sie ihn widerrufen, indem Sie das Kennwort des Benutzerkontos ändern, die Auftragsausgabe löschen oder den Stream Analytics-Auftrag löschen. | 

Eine Schritt-für-Schritt-Anleitung zum Konfigurieren einer Power BI-Ausgabe und eines Power BI-Dashboards erhalten Sie im Tutorial [Azure Stream Analytics und Power BI](stream-analytics-power-bi-dashboard.md).

> [!NOTE]
> Erstellen Sie das Dataset und die Tabelle nicht explizit im Power BI-Dashboard. Das Dataset und die Tabelle werden automatisch mit Daten aufgefüllt, wenn der Auftrag gestartet wird und damit beginnt, Ausgaben an Power BI weiterzuleiten. Wenn die Auftragsabfrage keine Ergebnisse generiert, werden das Dataset und die Tabelle nicht erstellt. Wenn Power BI bereits über ein Dataset und eine Tabelle mit demselben Namen verfügt, der in diesem Stream Analytics-Auftrag angegeben wurde, werden die vorhandenen Daten überschrieben.
>

### <a name="create-a-schema"></a>Erstellen eines Schemas

Azure Stream Analytics erstellt für den Benutzer ein Power BI-Dataset und ein Tabellenschema, falls diese noch nicht vorhanden sind. In allen anderen Fällen wird die Tabelle mit neuen Werten aktualisiert. Derzeit kann innerhalb eines Datasets nur eine Tabelle vorhanden sein. 

Power BI verwendet die FIFO-Aufbewahrungsrichtlinie (First In, First Out). Daten werden in einer Tabelle gesammelt, bis diese 200.000 Zeilen erreicht.

> [!NOTE]
> Es ist nicht zu empfehlen, mehrere Ausgaben zum Schreiben in dasselbe Dataset zu verwenden, da dies mehrere Probleme verursachen kann. Jede Ausgabe versucht, das Power BI-Dataset unabhängig zu erstellen, was dazu führen kann, dass mehrere Datasets denselben Namen haben. Außerdem wird, wenn die Ausgaben keine konsistenten Schemas haben, das Datasetschema bei jedem Schreibvorgang geändert, wodurch eine zu großen Anzahl von Schemaänderungsanforderungen verursacht wird. Selbst wenn sich diese Probleme vermeiden lassen, sind mehrere Ausgaben weniger leistungsfähig als eine einzelne zusammengeführte Ausgabe.

### <a name="convert-a-data-type-from-stream-analytics-to-power-bi"></a>Konvertieren eines Datentyps aus Stream Analytics in Power BI

Azure Stream Analytics aktualisiert das Datenmodell dynamisch zur Laufzeit, wenn sich das Ausgabeschema ändert. Nachverfolgt werden Änderungen an Spaltennamen und -typen sowie das Hinzufügen/Entfernen von Spalten.

Die folgende Tabelle enthält die Datentypkonvertierungen von [Stream Analytics-Datentypen](/stream-analytics-query/data-types-azure-stream-analytics) in [EDM-Typen (Entity Data Model)](/dotnet/framework/data/adonet/entity-data-model) von Power BI für den Fall, dass noch kein Power BI-Dataset und noch keine Tabelle vorhanden sind.

Quelle: Stream Analytics | Ziel: Power BI
-----|-----
BIGINT | Int64
nvarchar(max) | String
datetime | Datetime
float | Double
Datensatzarray | Zeichenfolgentyp, Konstantenwert „IRecord“ oder „IArray“

### <a name="update-the-schema"></a>Aktualisieren des Schemas

Stream Analytics leitet das Datenmodellschema vom ersten Ereignissatz in der Ausgabe ab. Später wird das Datenmodellschema bei Bedarf aktualisiert, um eingehende Ereignisse zu berücksichtigen, die unter Umständen nicht in das ursprüngliche Schema passen.

Vermeiden Sie die `SELECT *`-Abfrage, um zeilenübergreifende dynamische Schemaaktualisierungen zu verhindern. Neben einer möglichen Beeinträchtigung der Leistung ist möglicherweise auch der Zeitaufwand für die Ergebnisse ungewiss. Wählen Sie die genauen Felder aus, die auf dem Power BI-Dashboard angezeigt werden sollen. Außerdem müssen die Datenwerte mit dem ausgewählten Datentyp kompatibel sein.

Vorher/Aktuell | Int64 | String | Datetime | Double
-----------------|-------|--------|----------|-------
Int64 | Int64 | String | String | Double
Double | Double | String | String | Double
String | String | String | String | String 
Datetime | String | String |  Datetime | String

## <a name="limitations-and-best-practices"></a>Einschränkungen und bewährte Methoden
Derzeit können Sie Power BI etwa einmal pro Sekunde aufrufen. Das Streaming visueller Elemente unterstützt Pakete mit bis zu 15 KB. Darüber hinaus tritt beim Streaming visueller Elemente ein Fehler auf (Push funktioniert jedoch weiterhin). Aufgrund dieser Einschränkungen eignet sich Power BI perfekt für Anwendungsfälle, bei denen Azure Stream Analytics eine erhebliche Datenlastverringerung ermöglicht. Wir empfehlen die Verwendung eines rollierenden oder springenden Fensters, um sicherzustellen, dass der Datenpush nicht höher ist als ein Push pro Sekunde und Ihre Abfrage innerhalb der Durchsatzanforderungen liegt.

Weitere Informationen zur Ausgabebatchgröße finden Sie unter [Einschränkungen für Power BI-REST-API](/power-bi/developer/automation/api-rest-api-limitations).

## <a name="next-steps"></a>Nächste Schritte

* [Verwenden von verwalteten Identitäten zum Authentifizieren von Azure Stream Analytics-Aufträgen in Power BI](powerbi-output-managed-identity.md)
* [Schnellstart: Erstellen eines Stream Analytics-Auftrags mithilfe des Azure-Portals](stream-analytics-quick-create-portal.md)
