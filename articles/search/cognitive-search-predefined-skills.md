---
title: Integrierte Text- und Bildverarbeitung während der Indizierung
titleSuffix: Azure Cognitive Search
description: Durch kognitive Skills für Datenextraktion, natürliche Sprache und Bildverarbeitung erhält der Rohdateninhalt in einer Pipeline der kognitiven Azure-Suche Semantik und Struktur.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 35b0d596abc1921cd8faa3e88b3b6a88b2895d26
ms.sourcegitcommit: bd65925eb409d0c516c48494c5b97960949aee05
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/06/2021
ms.locfileid: "111539723"
---
# <a name="built-in-cognitive-skills-for-text-and-image-processing-during-indexing-azure-cognitive-search"></a>Integrierte kognitive Qualifikationen für die Text-und Bildverarbeitung während der Indizierung (Azure Cognitive Search)

In diesem Artikel erfahren Sie mehr über die kognitiven Qualifikationen, die mit Azure Cognitive Search bereitgestellt werden und die Sie in ein Skillset einschließen können, um Inhalt und Struktur zu extrahieren. Eine *kognitive Qualifikation* ist ein Modul oder Vorgang zum Transformieren von Inhalten. Oft handelt es sich um eine Komponente, die Daten extrahiert oder die Struktur ableitet und Eingabedaten somit verständlicher macht. Die Ausgabe erfolgt fast immer textbasiert. Ein *Skillset* ist eine Sammlung von Skills, die die Anreicherungspipeline definieren. 

> [!NOTE]
> Wenn Sie den Umfang erweitern, indem Sie die Verarbeitungsfrequenz erhöhen oder weitere Dokumente oder KI-Algorithmen hinzufügen, müssen Sie [eine kostenpflichtige Cognitive Services-Ressource anfügen](cognitive-search-attach-cognitive-services.md). Gebühren fallen beim Aufrufen von APIs in Cognitive Services sowie für die Bildextraktion im Rahmen der Dokumententschlüsselungsphase in Azure Cognitive Search an. Für die Textextraktion aus Dokumenten fallen keine Gebühren an.
>
> Die Ausführung integrierter Qualifikationen wird nach dem bestehenden [nutzungsbasierten Preis für Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/) berechnet. Die Preise für die Bildextraktion sind in der [Preisübersicht für Azure Cognitive Search](https://azure.microsoft.com/pricing/details/search/) angegeben.
>
> Mit dem Feature [Inkrementelle Anreicherung (Vorschau)](cognitive-search-incremental-indexing-conceptual.md) können Sie einen Cache bereitstellen, der die Effizienz des Indexers erhöht. Dies wird erreicht, indem beim Ändern Ihres Skillsets nur die erforderlichen kognitiven Skills ausgeführt werden, wodurch Zeit und Geld eingespart werden können.


## <a name="built-in-skills"></a>Integrierte Skills

Mehrere Skills sind flexibel in dem, was sie verwenden oder generieren. Im Allgemeinen basieren die meisten Skills auf vortrainierten Modellen, d.h. Sie können das Modell nicht mit Ihren eigenen Trainingsdaten trainieren. In der folgenden Tabelle werden die von Microsoft bereitgestellten Skills aufgelistet und beschrieben. 

| Skill | BESCHREIBUNG |
|-------|-------------|
|[Microsoft.Skills.Text.CustomEntityLookupSkill](cognitive-search-skill-custom-entity-lookup.md)| Sucht nach Text aus einer benutzerdefinierten Liste von Wörtern und Ausdrücken.|
| [Microsoft.Skills.Text.KeyPhraseExtractionSkill](cognitive-search-skill-keyphrases.md) | Dieser Skill verwendet ein vortrainiertes Modell, um wichtige Phrasen zu erkennen, die auf der Platzierung von Begriffen, sprachlichen Regeln, der Nähe zu anderen Begriffen und der Ungewöhnlichkeit des Begriffs innerhalb der Quelldaten basieren. |
| [Microsoft.Skills.Text.LanguageDetectionSkill](cognitive-search-skill-language-detection.md)  | Dieser Skill verwendet ein vortrainiertes Modell, um die verwendete Sprache zu erkennen (eine Sprachen-ID pro Dokument). Wenn mehrere Sprachen in den gleichen Textsegmenten verwendet werden, wird die LCID der vorwiegend verwendeten Sprache ausgegeben.|
| [Microsoft.Skills.Text.MergeSkill](cognitive-search-skill-textmerger.md) | Konsolidiert Text aus einer Sammlung von Feldern in einem einzigen Feld.  |
| [Microsoft.Skills.Text.V3.EntityLinkingSkill](cognitive-search-skill-entity-linking-v3.md) | Dieser Skill verwendet ein vortrainiertes Modell, um in einem gegebenen Text Übereinstimmungen mit verknüpften Entitäten zu ermitteln. |
| [Microsoft.Skills.Text.V3.EntityRecognitionSkill](cognitive-search-skill-entity-recognition-v3.md) | Dieser Skill verwendet ein vortrainiertes Modell, um Entitäten für eine feste Gruppe von Kategorien zu erstellen: die Felder `"Person"`, `"Location"`, `"Organization"`, `"Quantity"`, `"DateTime"`, `"URL"`, `"Email"`, `"PersonType"`, `"Event"`, `"Product"`, `"Skill"`, `"Address"`, `"Phone Number"` und `"IP Address"`. |
| [Microsoft.Skills.Text.PIIDetectionSkill](cognitive-search-skill-pii-detection.md)  | Dieser Skill verwendet ein vortrainiertes Modell, um persönliche Informationen aus einem Text zu extrahieren. Darüber hinaus bietet der Skill verschiedene Maskierungsoptionen für die im Text erkannten persönlichen Informationsentitäten.  |
| [Microsoft.Skills.Text.V3.SentimentSkill](cognitive-search-skill-sentiment-v3.md)  | Dieser Skill verwendet ein vortrainiertes Modell, um für jeweils einen Datensatz Stimmungsbezeichnungen (z. B. „negativ“, „neutral“ und „positiv“) basierend auf der höchsten Zuverlässigkeitsbewertung anzugeben, die vom Dienst auf Satz- und Dokumentebene gefunden wird. |
| [Microsoft.Skills.Text.SplitSkill](cognitive-search-skill-textsplit.md) | Teilt Text in Seiten auf, damit Sie Inhalt inkrementell anreichern oder erweitern können. |
| [Microsoft.Skills.Text.TranslationSkill](cognitive-search-skill-text-translation.md) | Dieser Skill verwendet ein vortrainiertes Modell, um den Eingabetext zur Normalisierung oder Lokalisierung in verschiedene Sprachen zu übersetzen. |
| [Microsoft.Skills.Vision.ImageAnalysisSkill](cognitive-search-skill-image-analysis.md) | Dieser Skill verwendet einen Bilderkennungsalgorithmus, um den Inhalt eines Bildes zu identifizieren und eine Textbeschreibung zu erzeugen. |
| [Microsoft.Skills.Vision.OcrSkill](cognitive-search-skill-ocr.md) | Optische Zeichenerkennung |
| [Microsoft.Skills.Util.ConditionalSkill](cognitive-search-skill-conditional.md) | Ermöglicht Filterung, Zuweisung eines Standardwerts und Zusammenführung von Daten auf der Grundlage einer Bedingung.|
| [Microsoft.Skills.Util.DocumentExtractionSkill](cognitive-search-skill-document-extraction.md) | Extrahiert Inhalt aus einer Datei innerhalb der Anreicherungspipeline. |
| [Microsoft.Skills.Util.ShaperSkill](cognitive-search-skill-shaper.md) | Ordnet die Ausgabe einem komplexen Typ zu (ein mehrteiliger Datentyp, der für einen vollständigen Namen, eine mehrzeilige Adresse oder eine Kombination aus Nachname und persönlicher ID verwendet werden kann). |
| [Microsoft.Skills.Custom.WebApiSkill](cognitive-search-custom-skill-web-api.md) | Ermöglicht die Erweiterbarkeit einer KI-Anreicherungspipeline, indem ein HTTP-Aufruf in einer benutzerdefinierten Web-API ausgeführt wird. |
| [Microsoft.Skills.Custom.AmlSkill](cognitive-search-aml-skill.md) | Ermöglicht die Erweiterbarkeit einer KI-Anreicherungspipeline mit einem Azure Machine Learning-Modell |


Eine Anleitung zum Erstellen einer [benutzerdefinierten Qualifikation](cognitive-search-custom-skill-web-api.md) finden Sie unter [Hinzufügen eines benutzerdefinierten Skills zu einer Pipeline der kognitiven Suche](cognitive-search-custom-skill-interface.md) sowie unter [Beispiel: Erstellen eines benutzerdefinierten Skills für die KI-Anreicherung](cognitive-search-create-custom-skill-example.md)

## <a name="see-also"></a>Weitere Informationen

+ [Definieren eines Skillsets](cognitive-search-defining-skillset.md)
+ [Schnittstellendefinition für benutzerdefinierte Skills](cognitive-search-custom-skill-interface.md)
+ [Tutorial: Angereicherte Indizierung mit künstlicher Intelligenz (KI)](cognitive-search-tutorial-blob.md)
