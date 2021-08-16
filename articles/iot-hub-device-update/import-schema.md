---
title: Importieren von Updates in Device Update für IoT Hub – Schema und weitere Informationen | Microsoft-Dokumentation
description: Schema und andere verwandte Informationen (einschließlich Objekten), die beim Importieren von Updates in Device Update für IoT Hub verwendet werden.
author: andrewbrownmsft
ms.author: andbrown
ms.date: 2/25/2021
ms.topic: conceptual
ms.service: iot-hub-device-update
ms.openlocfilehash: 1d58d0b0ecb614779b2fd046a44ad16afd8ebeb9
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111956004"
---
# <a name="importing-updates-into-device-update-for-iot-hub---schema-and-other-information"></a>Importieren von Updates in Device Update für IoT Hub – Schema und weitere Informationen
Wenn Sie ein Update in Device Update für IoT Hub importieren möchten, müssen Sie zuerst die [Konzepte](import-concepts.md) und die [Schrittanleitung](import-update.md) lesen. Wenn Sie an den Details des Schemas interessiert sind, das beim Erstellen eines Importmanifests verwendet wird, sowie mehr zu verknüpften Objekten erfahren möchten, informieren Sie sich weiter unten.

## <a name="import-manifest-schema"></a>Schema des Importmanifests

| Name | Typ | BESCHREIBUNG | Beschränkungen |
| --------- | --------- | --------- | --------- |
| UpdateId | `UpdateId`-Objekt | Update-Identität. |
| UpdateType | Zeichenfolge | Updatetyp: <br/><br/> * Geben Sie `microsoft/apt:1` an, wenn Sie ein paketbasiertes Update mit dem Referenz-Agent ausführen.<br/> * Geben Sie `microsoft/swupdate:1` an, wenn Sie ein imagebasiertes Update mit dem Referenz-Agent ausführen.<br/> Geben Sie `microsoft/simulator:1` an, wenn Sie den Beispiel-Agent-Simulator verwenden.<br/> * Geben Sie einen benutzerdefinierten Typ an, wenn Sie einen benutzerdefinierten Agent entwickeln. | Format: <br/> `{provider}/{type}:{typeVersion}`<br/><br/> Insgesamt maximal 32 Zeichen |
| InstalledCriteria | Zeichenfolge | Vom Agent interpretierte Zeichenfolge zur Ermittlung, ob das Update erfolgreich angewendet wurde:  <br/> * Geben Sie den **Wert** von „SWVersion“ für den Updatetyp `microsoft/swupdate:1` an.<br/> * Geben Sie `{name}-{version}` für den Updatetyp `microsoft/apt:1` an, dessen Name und Version aus der APT-Datei abgerufen werden.<br/> * Geben Sie eine benutzerdefinierte Zeichenfolge an, wenn Sie einen benutzerdefinierten Agent entwickeln.<br/> | Maximal 64 Zeichen |
| Kompatibilität | Array von `CompatibilityInfo`[-Objekten](#compatibilityinfo-object) | Kompatibilitätsinformationen des Geräts, das mit diesem Update kompatibel ist. | Maximal 10 Elemente |
| CreatedDateTime | date/time | Datum und Uhrzeit, an dem/zu der das Update erstellt wurde. | Durch Trennzeichen getrenntes ISO 8601-Datums- und Uhrzeitformat, in UTC |
| ManifestVersion | Zeichenfolge | Schemaversion des Importmanifests. Geben Sie den Wert `2.0` an, der mit der Schnittstelle `urn:azureiot:AzureDeviceUpdateCore:1` und der Schnittstelle `urn:azureiot:AzureDeviceUpdateCore:4` kompatibel ist. | Muss gleich `2.0` sein. |
| Dateien | Array von `File`-Objekten | Aktualisieren von Nutzlastdateien | Maximal 5 Dateien |

## <a name="updateid-object"></a>UpdateId-Objekt

| Name | Typ | BESCHREIBUNG | Beschränkungen |
| --------- | --------- | --------- | --------- |
| Anbieter | Zeichenfolge | Anbieterteil der Update-Identität. | 1–64 Zeichen, alphanumerisch, Punkt und Bindestrich. |
| Name | Zeichenfolge | Namensteil der Update-Identität. | 1–64 Zeichen, alphanumerisch, Punkt und Bindestrich. |
| Version | version | Versionsteil der Update-Identität. | 2 bis 4 Teile, durch Punkte getrennte Versionsnummer zwischen 0 und 2147483647. Führende Nullen werden gelöscht. |

## <a name="file-object"></a>File-Objekt

| Name | Typ | BESCHREIBUNG | Beschränkungen |
| --------- | --------- | --------- | --------- |
| Dateiname | Zeichenfolge | Name der Datei | Muss innerhalb eines Updates eindeutig sein |
| SizeInBytes | Int64 | Größe der Datei in Bytes. | Maximal 800 MB pro einzelner Datei oder 800 MB zusammen pro Update |
| Hashes | `Hashes`-Objekt | JSON-Objekt mit Hash(es) der Datei |

## <a name="compatibilityinfo-object"></a>CompatibilityInfo-Objekt

| Name | Typ | BESCHREIBUNG | Beschränkungen |
| --- | --- | --- | --- |
| DeviceManufacturer | Zeichenfolge | Hersteller des Geräts, mit dem das Update kompatibel ist. | 1–64 Zeichen, alphanumerisch, Punkt und Bindestrich. |
| DeviceModel | Zeichenfolge | Modell des Geräts, mit dem das Update kompatibel ist. | 1–64 Zeichen, alphanumerisch, Punkt und Bindestrich. |

## <a name="hashes-object"></a>Hashes-Objekt

| Name | Erforderlich | Typ | BESCHREIBUNG |
| --------- | --------- | --------- | --------- |
| Sha256 | True | Zeichenfolge | Base64-codierter Hash der Datei mit dem SHA-256-Algorithmus. |

## <a name="example-import-request-body"></a>Beispiel für den Import-Anforderungstext

Wenn Sie die Beispielausgabe des Importmanifests von der Seite [Hinzufügen eines neuen Updates](./import-update.md#review-the-generated-import-manifest) verwenden und die [Rest-API](/rest/api/deviceupdate/updates) für Geräteupdates direkt aufrufen möchten, um den Import durchzuführen, sollte der entsprechende Anforderungstext wie folgt aussehen:

```json
{
  "importManifest": {
    "url": "http://<your Azure Storage location file path>/importManifest.json",
    "sizeInBytes": <size of import manifest file>,
    "hashes": {
      "sha256": "<hash of import manifest file>"
    }
  },
  "files": [
    {
      "filename": "file1.json",
      "url": "http://<your Azure Storage location file path>/file1.json"
    },
    {
          "filename": "file2.zip",
          "url": "http://<your Azure Storage location file path>/file2.zip"
    },
  ]
}
```

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über [Konzepte zum Importieren](./import-concepts.md).

Wenn Sie bereit sind, probieren Sie die [Schrittanleitung für den Import](./import-update.md) aus, in dem Sie Schritt für Schritt durch den Importvorgang geführt werden.