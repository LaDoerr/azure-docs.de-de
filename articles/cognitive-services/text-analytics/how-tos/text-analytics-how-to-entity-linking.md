---
title: Verwenden der Entitätserkennung mit der Textanalyse-API
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie die Identität einer im Text gefundenen Entität mit der Textanalyse-REST-API identifizieren und unterscheiden können.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 06/15/2021
ms.author: aahi
ms.openlocfilehash: 989045e552a42d6ebf06d6c0ecadcb1a1f0bb5ff
ms.sourcegitcommit: cc099517b76bf4b5421944bd1bfdaa54153458a0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2021
ms.locfileid: "113549974"
---
# <a name="how-to-use-named-entity-recognition-in-text-analytics"></a>Verwenden der Erkennung benannter Entitäten in der Textanalyse

Mit der Textanalyse-API kann aus unstrukturiertem Text eine Liste mit eindeutig unterscheidbaren Entitäten und Links zu weiteren Informationen im Web zurückgegeben werden. Die API unterstützt sowohl benannte Entitätserkennung (Named Entity Recognition, NER) für mehrere Entitätskategorien als auch Entitätsverknüpfung.

## <a name="entity-linking"></a>Entitätsverknüpfung

Die Entitätsverknüpfung ist die Möglichkeit, die Identität einer im Text gefundenen Entität zu identifizieren und eindeutig zu machen (um beispielsweise zu ermitteln, ob das Wort „Mars“ den Planeten oder den römischen Kriegsgott bezeichnet). Um in diesem Prozess erkannte Entitäten im Text zu verknüpfen, ist eine Wissensdatenbank in der entsprechenden Sprache erforderlich. Die Entitätsverknüpfung verwendet [Wikipedia](https://www.wikipedia.org/) als diese Wissensdatenbank.

## <a name="named-entity-recognition-ner"></a>Erkennung benannter Entitäten (NER)

NER (Named Entity Recognition) ist die Möglichkeit, unterschiedliche Entitäten im Text zu identifizieren und sie in vordefinierte Klassen oder Typen zu kategorisieren wie z. B.: „Person“, „Standort“, „Ereignis“, „Produkt“ und „Organisation“.  

## <a name="personally-identifiable-information-pii"></a>Personenbezogene Informationen (Personally Identifiable Information, PII)

Das Feature PII ist ein Bestandteil von NER und kann vertrauliche Entitäten in Text identifizieren und bearbeiten, die einer einzelnen Person zugeordnet sind, wie z. B.: Telefonnummer, E-Mail-Adresse, Postanschrift, Reisepassnummer.

## <a name="named-entity-recognition-features-and-versions"></a>Features und Versionen der Erkennung benannter Entitäten

| Funktion                                                         | NER v3.0 | NER v3.1 |
|-----------------------------------------------------------------|--------|----------|
| Methoden für Einzel- und Batchabfragen                          | X      | X        |
| Erweiterte Entitätserkennung in verschiedenen Kategorien           | X      | X        |
| Separate Endpunkte zum Senden von Entitätsverknüpfungs- und NER-Anforderungen. | X      | X        |
| Erkennung von Entitäten mit personenbezogenen (`PII`) und gesundheitsbezogenen (`PHI`) Informationen        |        | X        |
| Bearbeitung von `PII`        |        | X        |

Weitere Informationen finden Sie unter [Sprachunterstützung](../language-support.md).

Named Entity Recognition v3 bietet erweiterte Erkennung über mehrere Typen hinweg. Derzeit können mit NER v3.0 Entitäten der [Kategorie mit den allgemeinen Entitäten](../named-entity-types.md) erkannt werden.

Erkennung benannter Entitäten v3.1 enthält die Erkennungsfunktionen von v3.0 sowie Folgendes: 
* Die Möglichkeit zur Erkennung personenbezogener Informationen (`PII`) mithilfe des Endpunkts `v3.1/entities/recognition/pii`. 
* Einen optionalen Parameter `domain=phi` zur Erkennung vertraulicher gesundheitsbezogener Informationen (`PHI`).
* Einen [asynchronen Vorgang](text-analytics-how-to-call-api.md) mithilfe des Endpunkts `/analyze`.

Weitere Informationen finden Sie im Artikel zu [Entitätskategorien](../named-entity-types.md) und unten im Abschnitt [Anforderungsendpunkte](#request-endpoints). Weitere Informationen zu Zuverlässigkeitsbewertungen finden Sie unter dem [Hinweis zur Transparenz der Textanalyse](/legal/cognitive-services/text-analytics/transparency-note?context=/azure/cognitive-services/text-analytics/context/context). 

## <a name="sending-a-rest-api-request"></a>Senden einer REST-API-Anforderung

### <a name="preparation"></a>Vorbereitung

Sie benötigen JSON-Dokumente im folgenden Format: ID, Text, Sprache.

Jedes Dokument darf jeweils maximal 5.120 Zeichen groß sein, und pro Sammlung sind bis zu 1.000 Elemente (IDs) zulässig. Die Sammlung wird im Hauptteil der Anforderung übermittelt.

### <a name="structure-the-request"></a>Strukturieren der Anforderung

Erstellen Sie eine POST-Anforderung. Um eine Anforderung schnell zu strukturieren und zu senden, können Sie in den folgenden Links [Postman](text-analytics-how-to-call-api.md) oder die **API-Testkonsole** verwenden. 

> [!NOTE]
> Den Schlüssel und den Endpunkt für Ihre Textanalyseressource finden Sie im Azure-Portal. Sie befinden sich auf der Seite **Schnellstart** der Ressource unter **Ressourcenverwaltung**. 


### <a name="request-endpoints"></a>Anforderungsendpunkte

#### <a name="version-31"></a>[Version 3.1](#tab/version-3-1)

In „Erkennung benannter Entitäten“ `v3.1` werden separate Endpunkte für NER, personenbezogene Informationen und Entitätsverknüpfungsanforderungen verwendet. Verwenden Sie basierend auf Ihrer Anforderung eines der URL-Formate unten.

**Entitätsverknüpfung**
* `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.1/entities/linking`

[Referenz zur Version 3.1 der Erkennung benannter Entitäten für `Linking`](https://westus2.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1/operations/EntitiesLinking)

**Erkennung benannter Entitäten**
* Allgemeine Entitäten: `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.1/entities/recognition/general`

[Referenz zur Version 3.1 der Erkennung benannter Entitäten für `General`](https://westus2.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1/operations/EntitiesRecognitionGeneral)

**Personenbezogene Informationen (Personally Identifiable Information, PII)**
* Personenbezogene Informationen (`PII`): `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.1/entities/recognition/pii`

Sie können auch den optionalen Parameter `domain=phi` verwenden, um im Text Informationen zur Gesundheit (`PHI`) zu erkennen. 

`https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.1/entities/recognition/pii?domain=phi`

Ab `v3.1` enthält die JSON-Antwort eine Eigenschaft `redactedText` mit dem geänderten Eingabetext, in dem die erkannten Entitäten personenbezogener Informationen durch ein `*` für jedes Zeichen in den Entitäten ersetzt werden.

[Referenz zur Version 3.1 der Erkennung benannter Entitäten für `PII`](https://westus2.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1/operations/EntitiesRecognitionPii)

Die API versucht, die [aufgelisteten Entitätskategorien](../named-entity-types.md?tabs=personal) für eine bestimmte Dokumentsprache zu erkennen. Wenn Sie angeben möchten, welche Entitäten erkannt und zurückgegeben werden sollen, verwenden Sie den optionalen Parameter `piiCategories` mit den entsprechenden Entitätskategorien. Mit diesem Parameter lassen sich auch Entitäten erkennen, die für Ihre Dokumentsprache standardmäßig nicht aktiviert sind. Im folgenden Beispiel wird die Führerscheinnummer eines französischen Fahrers, die in einem englischen Text vorkommen kann, zusammen mit den englischen Standardentitäten erkannt.

> [!TIP]
> Wenn Sie beim Angeben von Entitätskategorien `default` nicht angeben, gibt die API nur die von Ihnen angegebenen Entitätskategorien zurück.

`https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.1/entities/recognition/pii?piiCategories=default,FRDriversLicenseNumber`

**Asynchroner Vorgang**

Ab `v3.1` können Sie NER- und Entitätsverknüpfungsanforderungen mithilfe des `/analyze`-Endpunkts asynchron senden.

* Asynchroner Vorgang – `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.1/analyze`

Informationen zum Senden von asynchronen Anforderungen finden Sie unter [Aufrufen der Textanalyse-API](text-analytics-how-to-call-api.md).

#### <a name="version-30"></a>[Version 3.0](#tab/version-3)

Named Entity Recognition v3 verwendet separate Endpunkte für NER-Anforderungen und Entitätsverknüpfungsanforderungen. Verwenden Sie basierend auf Ihrer Anforderung ein URL-Format unten:

**Entitätsverknüpfung**
* `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0/entities/linking`

[Referenz zur Version 3.1 der Erkennung benannter Entitäten für `Linking`](https://westus2.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1/operations/EntitiesRecognitionGeneral)

**Erkennung benannter Entitäten**
* `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0/entities/recognition/general`

[Referenz zur Version 3.0 der Erkennung benannter Entitäten für `General`](https://westus2.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0/operations/EntitiesRecognitionGeneral)

---

Legen Sie einen Anforderungsheader fest, um Ihren Textanalyse-API-Schlüssel einzubeziehen. Geben Sie im Anforderungstext die von Ihnen vorbereiteten JSON-Dokumente an.

## <a name="example-requests"></a>Beispielanforderungen

#### <a name="version-31"></a>[Version 3.1](#tab/version-3-1)

### <a name="example-synchronous-ner-request"></a>Beispiel einer synchronen NER-Anforderung 

Das folgende JSON-Beispiel zeigt Inhalte, die Sie ggf. an die API senden können. Das Anforderungsformat ist für beide Versionen der API identisch.

```json
{
  "documents": [
    {
        "id": "1",
        "language": "en",
        "text": "Our tour guide took us up the Space Needle during our trip to Seattle last week."
    }
  ]
}
```

### <a name="example-synchronous-pii-request"></a>Beispiel einer synchronen Anforderung personenbezogener Informationen

Das folgende JSON-Beispiel zeigt Inhalte, die Sie ggf. an die API senden können, um personenbezogene Informationen im Text zu erkennen.

```json
{
  "documents": [
    {
        "id": "1",
        "language": "en",
        "text": "You can even pre-order from their online menu at www.contososteakhouse.com, call 312-555-0176 or send email to order@contososteakhouse.com!"
    }
  ]
}
```

### <a name="example-asynchronous-ner-request"></a>Beispiel einer asynchronen NER-Anforderung

Wenn Sie den `/analyze`-Endpunkt für [asynchrone Vorgänge](text-analytics-how-to-call-api.md) verwenden, erhalten Sie eine Antwort, die die Aufgaben enthält, die Sie an die API gesendet haben.

```json
{
    "displayName": "My Job",
    "analysisInput": {
        "documents": [
            {
                "id": "doc1",
                "text": "It's incredibly sunny outside! I'm so happy"
            },
            {
                "id": "doc2",
                "text": "Pike place market is my favorite Seattle attraction."
            }
        ]
    },
    "tasks": {
        "entityRecognitionTasks": [
            {
                "parameters": {
                    "model-version": "latest"
                }
            }
        ],
        "entityRecognitionPiiTasks": [{
            "parameters": {
                "model-version": "latest"
            }
        }]
    }
}
```

#### <a name="version-30"></a>[Version 3.0](#tab/version-3)

### <a name="example-synchronous-ner-request"></a>Beispiel einer synchronen NER-Anforderung 

Version 3.0 enthält nur den synchronen Vorgang. Das folgende JSON-Beispiel zeigt Inhalte, die Sie ggf. an die API senden können. Das Anforderungsformat ist für beide Versionen der API identisch.

```json
{
  "documents": [
    {
        "id": "1",
        "language": "en",
        "text": "Our tour guide took us up the Space Needle during our trip to Seattle last week."
    }
  ]
}
```

---

## <a name="post-the-request"></a>Übermitteln der Anforderung

Die Analyse erfolgt, wenn die Anforderung eingeht. Informationen zur Größe und Anzahl von Anforderungen, die Sie pro Minute und Sekunde senden können, finden Sie im Artikel [Datengrenzwerte](../overview.md#data-limits).

Die Textanalyse-API ist zustandslos. Auf Ihrem Konto werden keine Daten gespeichert, und die Ergebnisse werden sofort in der Antwort zurückgegeben.

## <a name="view-results"></a>Anzeigen der Ergebnisse

Alle POST-Anforderungen geben eine Antwort im JSON-Format mit den IDs und erkannten Entitätseigenschaften zurück.

Die Ausgabe wird umgehend zurückgegeben. Sie können die Ergebnisse an eine Anwendung streamen, die JSON akzeptiert, oder die Ausgabe in einer Datei auf dem lokalen System speichern und sie anschließend in eine Anwendung importieren, in der Sie die Daten sortieren, durchsuchen und bearbeiten können. Aufgrund der Unterstützung von Emojis und mehreren Sprachen enthält der Antworttext unter Umständen Textversätze. Weitere Informationen finden Sie unter [Verarbeiten von Textversätzen](../concepts/text-offsets.md).

### <a name="example-responses"></a>Beispielantworten

Version 3 stellt separate Endpunkte für die allgemeine Erkennung benannter Entitäten (NER), personenbezogene Informationen und Entitätsverknüpfung bereit. Version 3.1-preview enthält einen asynchronen Analysemodus. Die Antworten für diese Vorgänge finden Sie unten. 

#### <a name="version-31"></a>[Version 3.1](#tab/version-3-1)

### <a name="synchronous-example-results"></a>Ergebnisse des Beispiels für einen synchronen Vorgang

Beispiel für eine Antwort mit allgemeiner Erkennung benannter Entitäten:

```json
{
    "documents": [
        {
            "id": "1",
            "entities": [
                {
                    "text": "tour guide",
                    "category": "PersonType",
                    "offset": 4,
                    "length": 10,
                    "confidenceScore": 0.94
                },
                {
                    "text": "Space Needle",
                    "category": "Location",
                    "offset": 30,
                    "length": 12,
                    "confidenceScore": 0.96
                },
                {
                    "text": "Seattle",
                    "category": "Location",
                    "subcategory": "GPE",
                    "offset": 62,
                    "length": 7,
                    "confidenceScore": 1.0
                },
                {
                    "text": "last week",
                    "category": "DateTime",
                    "subcategory": "DateRange",
                    "offset": 70,
                    "length": 9,
                    "confidenceScore": 0.8
                }
            ],
            "warnings": []
        }
    ],
    "errors": [],
    "modelVersion": "2021-06-01"
}
```

Beispiel für eine Antwort mit personenbezogenen Informationen:

```json
{
    "documents": [
        {
            "redactedText": "You can even pre-order from their online menu at www.contososteakhouse.com, call ************ or send email to ***************************!",
            "id": "1",
            "entities": [
                {
                    "text": "312-555-0176",
                    "category": "PhoneNumber",
                    "offset": 81,
                    "length": 12,
                    "confidenceScore": 0.8
                },
                {
                    "text": "order@contososteakhouse.com",
                    "category": "Email",
                    "offset": 111,
                    "length": 27,
                    "confidenceScore": 0.8
                },
                {
                    "text": "contososteakhouse",
                    "category": "Organization",
                    "offset": 117,
                    "length": 17,
                    "confidenceScore": 0.45
                }
            ],
            "warnings": []
        }
    ],
    "errors": [],
    "modelVersion": "2021-01-15"
}
```

Beispiel für eine Antwort mit Entitätsverknüpfung:

```json
{
    "documents": [
        {
            "id": "1",
            "entities": [
                {
                    "bingId": "f8dd5b08-206d-2554-6e4a-893f51f4de7e",
                    "name": "Space Needle",
                    "matches": [
                        {
                            "text": "Space Needle",
                            "offset": 30,
                            "length": 12,
                            "confidenceScore": 0.4
                        }
                    ],
                    "language": "en",
                    "id": "Space Needle",
                    "url": "https://en.wikipedia.org/wiki/Space_Needle",
                    "dataSource": "Wikipedia"
                },
                {
                    "bingId": "5fbba6b8-85e1-4d41-9444-d9055436e473",
                    "name": "Seattle",
                    "matches": [
                        {
                            "text": "Seattle",
                            "offset": 62,
                            "length": 7,
                            "confidenceScore": 0.25
                        }
                    ],
                    "language": "en",
                    "id": "Seattle",
                    "url": "https://en.wikipedia.org/wiki/Seattle",
                    "dataSource": "Wikipedia"
                }
            ],
            "warnings": []
        }
    ],
    "errors": [],
    "modelVersion": "2021-06-01"
}
```

### <a name="example-asynchronous-result"></a>Ergebnis des Beispiels für einen asynchronen Vorgang

```json
{
    "jobId": "f480e1f9-0b61-4d47-93da-240f084582cf",
    "lastUpdateDateTime": "2021-07-06T19:03:15Z",
    "createdDateTime": "2021-07-06T19:02:47Z",
    "expirationDateTime": "2021-07-07T19:02:47Z",
    "status": "succeeded",
    "errors": [],
    "displayName": "My Job",
    "tasks": {
        "completed": 2,
        "failed": 0,
        "inProgress": 0,
        "total": 2,
        "entityRecognitionTasks": [
            {
                "lastUpdateDateTime": "2021-07-06T19:03:15.212633Z",
                "taskName": "NamedEntityRecognition_latest",
                "state": "succeeded",
                "results": {
                    "documents": [
                        {
                            "id": "doc1",
                            "entities": [],
                            "warnings": []
                        },
                        {
                            "id": "doc2",
                            "entities": [
                                {
                                    "text": "Pike place market",
                                    "category": "Location",
                                    "offset": 0,
                                    "length": 17,
                                    "confidenceScore": 0.95
                                },
                                {
                                    "text": "Seattle",
                                    "category": "Location",
                                    "subcategory": "GPE",
                                    "offset": 33,
                                    "length": 7,
                                    "confidenceScore": 0.99
                                }
                            ],
                            "warnings": []
                        }
                    ],
                    "errors": [],
                    "modelVersion": "2021-06-01"
                }
            }
        ],
        "entityRecognitionPiiTasks": [
            {
                "lastUpdateDateTime": "2021-07-06T19:03:03.2063832Z",
                "taskName": "PersonallyIdentifiableInformation_latest",
                "state": "succeeded",
                "results": {
                    "documents": [
                        {
                            "redactedText": "It's incredibly sunny outside! I'm so happy",
                            "id": "doc1",
                            "entities": [],
                            "warnings": []
                        },
                        {
                            "redactedText": "Pike place market is my favorite Seattle attraction.",
                            "id": "doc2",
                            "entities": [],
                            "warnings": []
                        }
                    ],
                    "errors": [],
                    "modelVersion": "2021-01-15"
                }
            }
        ]
    }
}
```


#### <a name="version-30"></a>[Version 3.0](#tab/version-3)

Beispiel für eine Antwort mit allgemeiner Erkennung benannter Entitäten:
```json
{
  "documents": [
    {
      "id": "1",
      "entities": [
        {
          "text": "tour guide",
          "category": "PersonType",
          "offset": 4,
          "length": 10,
          "confidenceScore": 0.45
        },
        {
          "text": "Space Needle",
          "category": "Location",
          "offset": 30,
          "length": 12,
          "confidenceScore": 0.38
        },
        {
          "text": "trip",
          "category": "Event",
          "offset": 54,
          "length": 4,
          "confidenceScore": 0.78
        },
        {
          "text": "Seattle",
          "category": "Location",
          "subcategory": "GPE",
          "offset": 62,
          "length": 7,
          "confidenceScore": 0.78
        },
        {
          "text": "last week",
          "category": "DateTime",
          "subcategory": "DateRange",
          "offset": 70,
          "length": 9,
          "confidenceScore": 0.8
        }
      ],
      "warnings": []
    }
  ],
  "errors": [],
  "modelVersion": "2020-04-01"
}
```

---


## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben Sie sich mit Konzepten und mit dem Workflow für die Entitätsverknüpfung unter Verwendung der Textanalyse in Cognitive Services vertraut gemacht. Zusammenfassung:

* JSON-Dokumente im Anforderungstext umfassen eine ID, Text und einen Sprachcode.
* POST-Anforderungen werden an einen oder mehrere Endpunkte gesendet. Dabei werden ein personalisierter [Zugriffsschlüssel und ein Endpunkt](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) verwendet, der für Ihr Abonnement gültig ist.
* Die Antwortausgabe, die aus verknüpften Entitäten besteht (z.B. Zuverlässigkeitsbewertungen, Offsets und Weblinks für jede Dokument-ID), kann in allen Anwendungen verwendet werden.

## <a name="next-steps"></a>Nächste Schritte

* [Übersicht über die Textanalyse](../overview.md)
* [Verwenden der Textanalyse-Clientbibliothek](../quickstarts/client-libraries-rest-api.md)
* [Modellversionen](../concepts/model-versioning.md)
* [Neuigkeiten](../whats-new.md)
