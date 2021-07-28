---
title: Gesichtserkennung und -attribute – Konzepte
titleSuffix: Azure Cognitive Services
description: Bei der Gesichtserkennung werden menschliche Gesichter in einem Bild gesucht und optional verschiedene Arten von gesichtsbezogenen Daten zurückgegeben.
services: cognitive-services
author: PatrickFarley
manager: nitime
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: pafarley
ms.openlocfilehash: cb005621a3439d5c7976221d816536b46efb986c
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111744993"
---
# <a name="face-detection-and-attributes"></a>Gesichtserkennung und -attribute

Dieser Artikel erläutert die Konzepte der Gesichtserkennung und der Attributdaten für Gesichter. Bei der Gesichtserkennung werden menschliche Gesichter in einem Bild gesucht und optional verschiedene Arten von gesichtsbezogenen Daten zurückgegeben.

Sie verwenden den Vorgang [Face – Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), um Gesichter in einem Bild zu erkennen. Jedes erkannte Gesicht entspricht mindestens einem faceRectangle-Feld in der Antwort. Dieser Satz von Pixelkoordinaten für links, oben, Breite und Höhe markieren das erkannte Gesicht. Mithilfe dieser Koordinaten finden Sie die Position des Gesichts und seine Größe. In der API-Antwort werden Gesichter in Reihenfolge nach Größe – vom größten zum kleinsten Gesicht – aufgeführt.

## <a name="face-id"></a>Gesichts-ID

Die Gesichtserkennungs-ID ist eine Zeichenfolge mit dem eindeutigen Bezeichner für jedes erkannte Gesicht in einem Bild. Sie können eine Gesichtserkennungs-ID in Ihrem API-Aufruf [Face – Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) anfordern.

## <a name="face-landmarks"></a>Gesichtszüge

Gesichtsmerkmale bestehen aus mehreren einfach zu findenden Punkten im Gesicht. Beispiel sind die Pupillen oder die Nasenspitze. Standardmäßig sind 27 Bezugspunkte vordefiniert. Die folgende Abbildung zeigt alle 27 Punkte:

![Abbildung eines Gesichts mit allen 27 beschrifteten Gesichtsmerkmalpunkten](../Images/landmarks.1.jpg)

Die Koordinaten der Punkte werden in Pixel zurückgegeben.

## <a name="attributes"></a>Attributes

Attribute sind ein Satz von Merkmalen, die optional durch die API [Face – Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) erkannt werden können. Die folgenden Attribute können erkannt werden:

* **Accessoires** Gibt an, ob das Gesicht Accessoires aufweist. Dieses Attribut gibt die möglichen Accessoires zurück, einschließlich Kopfbedeckung, Brille und Maske, mit einem Vertrauenswert zwischen 0 (null) und 1 für jedes Accessoire.
* **Alter:** Das geschätzte Alter eines bestimmten Gesichts in Jahren.
* **Weichzeichnung:** Die Unschärfe des Gesichts im Bild. Dieses Attribut gibt einen Wert zwischen 0 und 1 und eine informelle Bewertung von gering, mittel oder hoch zurück.
* **Emotionen:** Eine Liste von Emotionen und deren Erkennungssicherheit für das jeweilige Gesicht. Die Zuverlässigkeitsbewertungen werden normalisiert, und die Bewertungen über alle Emotionen hinweg ergeben eine Summe von eins. Die zurückgegebenen Emotionen sind Glück, Traurigkeit, Neutralität, Wut, Verachtung, Ekel, Überraschung und Angst.
* **Belichtung:** Die Belichtung des Gesichts im Bild. Dieses Attribut gibt einen Wert zwischen 0 und 1 und die informelle Bewertung underExposure, goodExposure oder overExposure zurück.
* **Gesichtsbehaarung:** Das geschätzte Vorhandensein von Gesichtsbehaarung und die Länge für das jeweilige Gesicht.
* **Geschlecht:** Das geschätzte Geschlecht des jeweiligen Gesichts. Mögliche Werte sind „male“, „female“ und „genderless“.
* **Brille:** Gibt an, ob das Gesicht eine Brille aufweist. Mögliche Werte sind NoGlasses, ReadingGlasses, SunGlasses und SwimmingGoggles.
* **Haare:** Die Art der Haare des Gesichts. Dieses Attribut zeigt an, ob die Haare sichtbar sind, ob Haarausfall erkannt wurde und welche Farbe die Haare haben.
* **Kopfhaltung:** Die Ausrichtung des Gesichts im dreidimensionalen Raum. Dieses Attribut wird durch die Roll-, Gier- und Nickwinkel in Grad beschrieben, die nach der [rechten Regel](https://en.wikipedia.org/wiki/Right-hand_rule) definiert sind. Die Reihenfolge der drei Winkel ist Rollwinkel-Gierwinkel-Nickwinkel, und der Wertebereich jedes Winkels reicht von -180 Grad bis 180 Grad. Die 3D-Ausrichtung des Gesichts wird der Reihe nach durch den Roll-, Gier- und Nickwinkel geschätzt. Die Winkelzuordnungen sind im folgenden Diagramm dargestellt:

    ![Kopf mit Bezeichnungen für Roll-, Nick- und Gierachse](../Images/headpose.1.jpg)
* **Makeup:** Gibt an, ob das Gesicht Makeup trägt. Dieses Attribut gibt einen booleschen Wert für eyeMakeup und lipMakeup zurück.
* **Maske:**  Gibt an, ob das Gesicht eine Maske trägt. Dieses Attribut gibt einen möglichen Maskentyp und einen booleschen Wert zurück, um anzugeben, ob Nase und Mund bedeckt sind.
* **Rauschen:** Visuelle Störungen, die im Gesicht erkannt wurden. Dieses Attribut gibt einen Wert zwischen 0 und 1 und eine informelle Bewertung von gering, mittel oder hoch zurück.
* **Okklusion:** Gibt an, ob Objekte Teile des Gesichts verdecken. Dieses Attribut gibt einen booleschen Wert für die eyeOccluded, foreheadOccluded und mouthOccluded zurück.
* **Lächeln:** Gibt die Stärke des Lächelns des jeweiligen Gesichts an. Dieser Wert liegt zwischen 0 (kein Lächeln) und 1 (deutliches Lächeln).

> [!IMPORTANT]
> Gesichtsattribute werden mithilfe statistischer Algorithmen vorhergesagt. Sie sind möglicherweise nicht immer genau. Treffen Sie Entscheidungen auf Grundlage von Attributdaten mit Umsicht.

## <a name="input-data"></a>Eingabedaten

Anhand der folgenden Tipps können Sie sicherstellen, dass Ihre Eingabebilder möglichst genaue Erkennungsergebnisse liefern:

* Als Eingabebildformate werden JPEG, PNG, GIF (der erste Frame) und BMP unterstützt.
* Die Bilddateien dürfen maximal 6 MB groß sein.
* Die minimal erkennbare Gesichtsgröße beträgt 36×36 Pixel auf einem Bild, das selbst nicht größer als 1.920×1.080 Pixel sein darf. Auf Bildern, die größer als 1.920×1.080 Pixel sind, ist eine proportional größere minimale Gesichtsgröße möglich. Eine Verkleinerung der Gesichtsgröße kann dazu führen, dass einige Gesichter nicht erkannt werden, selbst wenn sie größer als die minimal erkennbare Gesichtsgröße sind.
* Die maximal erkennbare Gesichtsgröße beträgt 4.096×4.096 Pixel.
* Gesichter außerhalb der Größenspanne von 36×36 bis 4.096×4.096 Pixel werden nicht erkannt.
* Einige Gesichter können möglicherweise aufgrund technischer Probleme nicht erkannt werden. Extreme Gesichtswinkel (Kopfhaltung) oder verdeckte Gesichter (Objekte wie Sonnenbrille oder Hände bedecken Teile des Gesichts) können die Erkennung beeinträchtigen. Frontalansichten und nahezu der Frontalansicht entsprechende Ansichten von Gesichtern führen zu den besten Ergebnissen.

Eingabedaten mit Ausrichtungsinformationen:
* Einige Eingabebilder im JPEG-Format können Ausrichtungsinformationen in Exif-Metadaten (Exchangeable Image File Format) enthalten. Wenn die Exif-Ausrichtung verfügbar ist, werden die Bilder automatisch in die richtige Ausrichtung gedreht, bevor sie zur Gesichtserkennung gesendet werden. Das Gesichtsrechteck, die Orientierungspunkte und die Kopfhaltung für jedes erkannte Gesicht werden auf der Grundlage des gedrehten Bildes geschätzt.
* Um das Gesichtsrechteck und die Orientierungspunkte richtig anzuzeigen, müssen Sie sicherstellen, dass das Bild richtig gedreht ist. Die meisten Tools zur Bildvisualisierung drehen das Bild standardmäßig automatisch entsprechend seiner Exif-Ausrichtung. Bei anderen Tools müssen Sie die Drehung möglicherweise mithilfe Ihres eigenen Codes anwenden. Die folgenden Beispiele zeigen ein Gesichtsrechteck auf einem gedrehten Bild (links) und einem nicht gedrehten Bild (rechts).

![Zwei Bilder mit Gesichtern mit/ohne Drehung](../Images/image-rotation.png)

Wenn Sie Gesichter in einem Videofeed ermitteln möchten, können Sie die Leistung verbessern, indem Sie bestimmte Einstellungen an Ihrer Videokamera anpassen:

* **Glättung:** Viele Videokameras wenden einen glättenden Effekt an. Sie sollten diesen deaktivieren, da er einen Weichzeichnungseffekt zwischen den Frames einfügt und damit die Schärfe reduziert.
* **Belichtungszeit:** Eine kürzere Belichtungszeit verringert die Anzahl der Bewegungen zwischen den Frames und macht damit die einzelnen Frame deutlicher. Es werden Belichtungszeiten von 1/60 Sekunde oder kürzer empfohlen.
* **Verschlusswinkel:** Einige Kameras geben anstelle der Belichtungszeit den Verschlusswinkel an. Sie sollten nach Möglichkeit einen niedrigeren Verschlusswinkel verwenden. Dies führt zu deutlicheren Videoframes.

    >[!NOTE]
    > Eine Kamera mit einem niedrigeren Verschlusswinkel nimmt pro Frame weniger Licht auf, sodass das Bild dunkler wird. Sie müssen die beste Einstellung selbst bestimmen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun mit den Konzepten der Gesichtserkennung vertraut sind, erfahren Sie, wie Sie ein Skript schreiben, das Gesichter in einem vorgegebenen Bild erkennt.

* [Gesichtserkennung in einem Bild](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md)
