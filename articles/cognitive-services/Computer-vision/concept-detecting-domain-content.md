---
title: Domänenspezifische Inhalte – maschinelles Sehen
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie eine Bildkategorisierungsdomäne angeben, um ausführlichere Informationen zu einem Bild zurückzugeben.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 02/08/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: ff00d61c5f53d0c702e16d8223a330cb09ee619e
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110465398"
---
# <a name="detect-domain-specific-content"></a>Erkennen domänenspezifischer Inhalte

Zusätzlich zur Kennzeichnung und allgemeinen Kategorisierung wird beim maschinellen Sehen auch die weiter gehende domänenspezifische Analyse unterstützt. Hierfür werden Modelle verwendet, die anhand von speziellen Daten trainiert wurden.

Es gibt zwei Möglichkeiten für die Verwendung von domänenspezifischen Modellen: allein (bereichsbezogene Analyse) oder als Erweiterung der Kategorisierungsfunktion.

### <a name="scoped-analysis"></a>Bereichsbezogene Analyse

Sie können ein Bild analysieren, indem Sie nur das domänenspezifische Modell nutzen. Rufen Sie hierfür die API [Models/\<model\>/Analyze](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2/operations/56f91f2e778daf14a499f21b) auf.

Hier ist eine JSON-Beispielantwort angegeben, die von der API **models/celebrities/analyze** für das jeweilige Bild zurückgegeben wird:

![Satya Nadella (stehend, lächelnd)](./images/satya.jpeg)

```json
{
  "result": {
    "celebrities": [{
      "faceRectangle": {
        "top": 391,
        "left": 318,
        "width": 184,
        "height": 184
      },
      "name": "Satya Nadella",
      "confidence": 0.99999856948852539
    }]
  },
  "requestId": "8217262a-1a90-4498-a242-68376a4b956b",
  "metadata": {
    "width": 800,
    "height": 1200,
    "format": "Jpeg"
  }
}
```

### <a name="enhanced-categorization-analysis"></a>Verbesserte Kategorisierungsanalyse

Sie können domänenspezifische Modelle auch verwenden, um die allgemeine Bildanalyse zu erweitern. Dies ist im Rahmen der [allgemeinen Kategorisierung](concept-categorizing-images.md) möglich, indem domänenspezifische Modelle im Parameter *details* des [Analyze](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2/operations/56f91f2e778daf14a499f21b)-API-Aufrufs angegeben werden.

In diesem Fall wird zuerst die Klassifizierung der 86-Kategorien-Taxonomie aufgerufen. Wenn erkannte Kategorien über ein passendes domänenspezifisches Modell verfügen, wird das Bild auch über dieses Modell übergeben, und die Ergebnisse werden hinzugefügt.

Mit der folgenden JSON-Antwort wird veranschaulicht, wie die domänenspezifische Analyse als `detail`-Knoten in eine umfassendere Kategorisierungsanalyse eingebunden werden kann.

```json
"categories":[
  {
    "name":"abstract_",
    "score":0.00390625
  },
  {
    "name":"people_",
    "score":0.83984375,
    "detail":{
      "celebrities":[
        {
          "name":"Satya Nadella",
          "faceRectangle":{
            "left":597,
            "top":162,
            "width":248,
            "height":248
          },
          "confidence":0.999028444
        }
      ],
      "landmarks":[
        {
          "name":"Forbidden City",
          "confidence":0.9978346
        }
      ]
    }
  }
]
```

## <a name="list-the-domain-specific-models"></a>Auflisten der domänenspezifischen Modelle

Derzeit werden für das maschinelle Sehen die folgenden domänenspezifischen Modelle unterstützt:

| Name | BESCHREIBUNG |
|------|-------------|
| Prominente | Erkennung berühmter Personen, die für Bilder unterstützt wird, die in die Kategorie `people_` klassifiziert werden |
| Wahrzeichen | Erkennung von Wahrzeichen, die für Bilder unterstützt wird, die in die Kategorien `outdoor_` oder `building_` klassifiziert werden |

Durch das Aufrufen der [Models](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2/operations/56f91f2e778daf14a499f20e)-API werden diese Informationen zusammen mit den Kategorien zurückgegeben, für die die einzelnen Modelle gelten können:

```json
{
  "models":[
    {
      "name":"celebrities",
      "categories":[
        "people_",
        "人_",
        "pessoas_",
        "gente_"
      ]
    },
    {
      "name":"landmarks",
      "categories":[
        "outdoor_",
        "户外_",
        "屋外_",
        "aoarlivre_",
        "alairelibre_",
        "building_",
        "建筑_",
        "建物_",
        "edifício_"
      ]
    }
  ]
}
```

## <a name="use-the-api"></a>Verwenden der API

Dieses Feature ist über die [Bildanalyse](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2/operations/56f91f2e778daf14a499f21b)-API verfügbar. Sie können diese API über ein natives SDK oder REST-Aufrufe aufrufen. Fügen Sie `Celebrities` oder `Landmarks` in den Abfrageparameter **details** ein. Nachdem Sie die vollständige JSON-Antwort erhalten haben, analysieren Sie einfach die Zeichenfolge auf den Inhalt im Abschnitt `"details"`.

* [Schnellstart: Verwenden der Clientbibliothek für maschinelles Sehen](./quickstarts-sdk/image-analysis-client-library.md?pivots=programming-language-csharp)
