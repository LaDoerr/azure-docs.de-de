---
title: Analysetools für linguistische Analyse und Textverarbeitung
titleSuffix: Azure Cognitive Search
description: Weisen Sie durchsuchbaren Textfeldern in einem Index Analysen zu, um die Lucene-Standardanalyse durch benutzerdefinierte, vordefinierte oder sprachspezifische Alternativen zu ersetzen.
author: HeidiSteen
manager: nitinme
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 09/08/2021
ms.custom: devx-track-csharp
ms.openlocfilehash: 68b6f6794a690313648dfaaaaf49fdd3150b6171
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2021
ms.locfileid: "124735330"
---
# <a name="analyzers-for-text-processing-in-azure-cognitive-search"></a>Analysetools für Textverarbeitung in der kognitiven Azure-Suche

Ein *Analysetool* ist eine Komponente der [Volltextsuch-Engine](search-lucene-query-architecture.md), das für die Verarbeitung von Zeichenfolgen während der Indizierung und Abfrageausführung verantwortlich ist. Die Textverarbeitung (auch als lexikalische Analyse bezeichnet) ist transformativ und ändert eine Zeichenfolge durch folgende Aktionen:

+ Entfernen nicht unbedingt benötigter Wörter (Stoppwörter) und Satzzeichen
+ Unterteilen von Ausdrücken und Wörtern mit Bindestrichen in Einzelwörter
+ Umwandeln von Wörtern in Großbuchstaben in Wörter in Kleinbuchstaben
+ Kürzen von Wörtern in primitive Stammformen für die Speichereffizienz, damit Übereinstimmungen unabhängig von der Zeitform gefunden werden

Die Analyse gilt für `Edm.String`-Felder, die als „durchsuchbar“ gekennzeichnet sind, was auf die Volltextsuche hinweist. 

Bei Feldern dieser Konfiguration erfolgt die Analyse während der Indizierung, wenn Token erstellt werden und dann noch mal während der Abfrageausführung, wenn Abfragen analysiert werden und die Engine diese auf übereinstimmende Token überprüft. Eine Übereinstimmung ist wahrscheinlicher, wenn dasselbe Analysetool sowohl für die Indizierung als auch für Abfragen verwendet wird. Sie können das Tool jedoch für jede Workload in Abhängigkeit von Ihren Anforderungen einzeln festlegen.

Abfragetypen, bei denen es sich *nicht* um eine Volltextsuche handelt (z. B. Filter oder Fuzzysuche), durchlaufen auf Abfrageseite nicht die Analysephase. Stattdessen sendet der Parser diese Zeichenfolgen direkt an die Suchmaschine, wobei das Muster verwendet wird, das Sie als Grundlage für die Suche bereitstellen. In der Regel erfordern diese Abfrageformulare ganze Zeichenfolgentoken für den Musterabgleich. Sie benötigen möglicherweise [benutzerdefinierte Analysetools](index-add-custom-analyzers.md), um bei der Indizierung ganze Begriffstoken sicherzustellen. Weitere Informationen darüber, wann und warum Abfragebegriffe analysiert werden, finden Sie unter [Funktionsweise der Volltextsuche in der kognitiven Azure-Suche](search-lucene-query-architecture.md).

Weitere Hintergrundinformationen zur lexikalischen Analyse finden Sie im folgenden Videoclip.

> [!VIDEO https://www.youtube.com/embed/Y_X6USgvB1g?version=3&start=132&end=189]

## <a name="default-analyzer"></a>Standardanalysemodul  

In Azure Cognitive Search wird ein Analysetool automatisch für alle Zeichenfolgenfelder aufgerufen, die als durchsuchbar markiert sind. 

Azure Cognitive Search verwendet standardmäßig das [Standardanalysetool von Apache Lucene (Standard-Lucene)](https://lucene.apache.org/core/6_6_1/core/org/apache/lucene/analysis/standard/StandardAnalyzer.html). Dieses unterteilt Text gemäß den Regeln der [Unicode-Textsegmentierung](https://unicode.org/reports/tr29/) in einzelne Elemente. Darüber hinaus konvertiert das Standardanalyseprogramm alle Zeichen in Kleinbuchstaben. Während der Indizierung und der Abfrageverarbeitung durchlaufen sowohl indizierte Dokumente als auch Suchbegriffe die Analyse.  

Dieser Standard kann feldspezifisch überschrieben werden. Alternative Analysetools können ein [Sprachanalysetool](index-add-language-analyzers.md) für die linguistische Verarbeitung, ein [benutzerdefiniertes Analysetool](index-add-custom-analyzers.md) oder ein integriertes Analysetool aus der [Liste der verfügbaren Analysetools](index-add-custom-analyzers.md#built-in-analyzers) sein.

## <a name="types-of-analyzers"></a>Typen von Analysetools

Die folgende Liste gibt Aufschluss darüber, welche Analysetools in der kognitiven Azure-Suche unterstützt werden.

| Category | BESCHREIBUNG |
|----------|-------------|
| [Lucene-Standardanalyse](https://lucene.apache.org/core/6_6_1/core/org/apache/lucene/analysis/standard/StandardAnalyzer.html) | Standard. Muss nicht angegeben oder konfiguriert werden. Dieses allgemeine Analysetool eignet sich für viele Sprachen und Szenarios.|
| Integrierte Analysetools | Wird unverändert verwendet und wird anhand des Namens referenziert. Es gibt zwei Typen: Sprache und sprachunabhängig. </br></br>[Spezialisierte (sprachunabhängige) Analysetools](index-add-custom-analyzers.md#built-in-analyzers) werden verwendet, wenn Texteingaben eine spezielle oder minimale Verarbeitung erfordern. Beispiele für die Analysetools in dieser Kategorie sind **Asciifolding**, **Keyword**, **Pattern**, **Simple**, **Stop** und **Whitespace**. </br></br>[Sprachanalyzer](index-add-language-analyzers.md) werden verwendet, wenn umfassende linguistische Unterstützung für einzelne Sprachen benötigt werden. Die kognitive Azure-Suche unterstützt 35 Lucene-Sprachanalysen und 50 Microsoft-Analysen für die Verarbeitung natürlicher Sprache. |
|[Benutzerdefinierte Analysen](/rest/api/searchservice/Custom-analyzers-in-Azure-Search) | Eine benutzerdefinierte Konfiguration einer Kombination vorhandener Elemente, bestehend aus einem Tokenizer (erforderlich) und optionalen Filtern („char“ oder „token“).|

Ein paar integrierte Analysetools wie **Pattern** oder **Stop** unterstützen eine begrenzte Anzahl von Konfigurationsoptionen. Erstellen Sie zum Festlegen dieser Optionen ein benutzerdefiniertes Analysetool, bestehend aus dem integrierten Analysetool und einer der unter [Integrierte Analysetools](index-add-custom-analyzers.md#built-in-analyzers) dokumentierten alternativen Optionen. Versehen Sie Ihre neue Konfiguration wie üblich mit einem Namen (etwa *myPatternAnalyzer*), damit sie von der Lucene-Musteranalyse zu unterscheiden ist.

## <a name="how-to-specify-analyzers"></a>Angeben von Analyzern

Das Festlegen eines Analysetools ist optional. Es wird generell empfohlen, zunächst das Lucene-Standardanalysetool zu verwenden, um herauszufinden, wie es funktioniert. Wenn Abfragen nicht die erwarteten Ergebnisse zurückgeben, ist der Wechsel zu einem anderen Analysetool häufig die richtige Lösung.

1. Wenn Sie im [Index](/rest/api/searchservice/create-index) eine Felddefinition erstellen, legen Sie die Eigenschaft „analyzer“ auf einen der folgenden Werte fest: ein [integriertes Analysetool](index-add-custom-analyzers.md#built-in-analyzers) wie **Keyword**, ein [Sprachanalysetool](index-add-language-analyzers.md) wie `en.microsoft` oder ein benutzerdefiniertes Analysetool (definiert im gleichen Indexschema).  
 
   ```json
     "fields": [
    {
      "name": "Description",
      "type": "Edm.String",
      "retrievable": true,
      "searchable": true,
      "analyzer": "en.microsoft",
      "indexAnalyzer": null,
      "searchAnalyzer": null
    },
   ```

   Wenn Sie ein [Sprachanalysetool](index-add-language-analyzers.md) verwenden, müssen Sie die Eigenschaft „analyzer“ verwenden, um dieses anzugeben. Die Eigenschaften „searchAnalyzer“ und „indexAnalyzer“ können bei Sprachanalysetools nicht angewendet werden.

1. Alternativ können Sie „indexAnalyzer“ und „searchAnalyzer“ festlegen, um für jede Workload unterschiedliche Analysetools zu verwenden. Diese Eigenschaften werden gleichzeitig festgelegt und ersetzen die Eigenschaft „analyzer“, die NULL sein muss. Sie können verschiedene Analysetools für die Indizierung und Abfragen verwenden, wenn eine dieser Aktivitäten eine bestimmte Transformation erfordert, die die andere nicht benötigt.

   ```json
     "fields": [
    {
      "name": "ProductGroup",
      "type": "Edm.String",
      "retrievable": true,
      "searchable": true,
      "analyzer": null,
      "indexAnalyzer": "keyword",
      "searchAnalyzer": "standard"
    },
   ```

1. Erstellen Sie für benutzerdefinierte Analysetools einen Eintrag im Abschnitt **[Analysetool]** des Indexes, und weisen Sie der Felddefinition dann gemäß den beiden vorherigen Schritten das benutzerdefinierte Analysetool zu. Weitere Informationen finden Sie unter [Create Index](/rest/api/searchservice/create-index) (Index erstellen) sowie unter [Add a custom analyzer](index-add-custom-analyzers.md) (Benutzerdefiniertes Analysetool hinzufügen).

## <a name="when-to-add-analyzers"></a>Gründe zum Hinzufügen von Analysetools

Der beste Zeitpunkt zum Hinzufügen und Zuweisen von Analysetools ist während der aktiven Entwicklung, wenn das Löschen und Neuerstellen von Indizes Routine ist.

Da Analysetools für das Umwandeln von Ausdrücken in Token verwendet werden, sollten Sie ein Analysetool zuweisen, wenn das Feld erstellt wird. Das Zuweisen eines Analysetools oder von „indexAnalyzer“ zu einem Feld, das bereits physisch erstellt wurde, ist nicht zulässig (obwohl Sie die Eigenschaft „searchAnalyzer“ jederzeit ohne Auswirkung auf den Index ändern können).

Sie müssen den gesamten Index löschen und neu erstellen, um das Analysetool eines vorhandenen Felds zu ändern (einzelne Felder können nicht neu erstellt werden). Für Indizes in der Produktion können Sie ein erneutes Erstellen aufschieben, indem Sie ein neues Feld mit der neuen Analysetoolzuweisung erstellen und diese anstelle der alten verwenden. Integrieren Sie das neue Feld mit [Update Index](/rest/api/searchservice/update-index) (Index aktualisieren), und füllen Sie es mit [mergeOrUpload](/rest/api/searchservice/addupdate-or-delete-documents). Später können Sie den Index als Bestandteil der geplanten Indexwartung bereinigen, um veraltete Felder zu entfernen.

Rufen Sie [Update Index](/rest/api/searchservice/update-index) (Index aktualisieren) auf, um ein neues Feld zu einem vorhandenen Index hinzuzufügen, und verwenden Sie [mergeOrUpload](/rest/api/searchservice/addupdate-or-delete-documents), um es aufzufüllen.

Wenn Sie diesen Fehler vermeiden möchten, übergeben Sie das Flag „allowIndexDowntime“ in [Index aktualisieren](/rest/api/searchservice/update-index), um ein benutzerdefiniertes Analysetool zu einem vorhandenen Index hinzuzufügen.

*„Indexupdate ist nicht zulässig, weil es eine Downtime verursachen würde. Um einem vorhandenen Index neue Analysetools, Tokenizer, Tokenfilter oder Zeichenfilter hinzuzufügen, legen Sie für den Abfrageparameter „allowIndexDowntime“ in der Indexupdateanforderung „True“ fest. Beachten Sie, dass dieser Vorgang Ihren Index für mindestens ein paar Sekunden offline schaltet, sodass Indizierungs- und Abfrageanforderungen nicht gelingen. Leistung und Schreibverfügbarkeit des Indexes können nach der Indexaktualisierung mehrere Minuten lang eingeschränkt sein, bei sehr großen Indizes auch länger.“*

## <a name="recommendations-for-working-with-analyzers"></a>Empfehlungen zum Arbeiten mit Analysetools

Dieser Abschnitt enthält Tipps zur Verwendung von Analyzern.

### <a name="one-analyzer-for-read-write-unless-you-have-specific-requirements"></a>Eine einzelne Analyse für Lese-/Schreibvorgänge, sofern keine besonderen Anforderungen gelten

Azure Cognitive Search ermöglicht das Angeben verschiedener Analysetools für das Indizieren und Suchen über zusätzliche indexAnalyzer- und searchAnalyzer-Feldeigenschaften. Wenn keine Angabe vorhanden ist, wird das mit der analyzer-Eigenschaft festgelegte Analysetool sowohl für die Indizierung als auch für die Suche verwendet. Ohne Angabe des Analysetools wird das Lucene-Standardanalysetool verwendet.

Allgemeine Regel: Verwenden Sie für die Indizierung und für Abfragen die gleiche Analyse, es sei denn, es müssen besondere Anforderungen erfüllt werden. Führen Sie die Tests sorgfältig durch. Wenn sich die Textverarbeitung zur Such- und Indizierungszeit unterscheidet, besteht das Risiko der Nichtübereinstimmung zwischen Abfrageausdrücken und indizierten Ausdrücken, wenn die Konfigurationen des Analyzers für die Suche und die Indizierung nicht abgestimmt sind.

### <a name="test-during-active-development"></a>Testen während der aktiven Entwicklung

Wenn Sie die Standardanalyse überschreiben, muss der Index neu erstellt werden. Überlegen Sie sich möglichst während der aktiven Entwicklung, welche Analysen Sie verwenden möchten, bevor Sie einen Index in die Produktionsumgebung einbringen.

### <a name="inspect-tokenized-terms"></a>Überprüfen von tokenisierten Begriffen

Wenn eine Suche nicht die erwarteten Ergebnisse zurückgibt, ist das wahrscheinlichste Szenario, dass Abweichungen zwischen Begriffseingaben für die Abfrage und tokenisierten Begriffen im Index vorliegen. Wenn die Token nicht identisch sind, können Übereinstimmungen nicht materialisiert werden. Zur Überprüfung der Tokenizer-Ausgabe wird empfohlen, die [Analyse-API](/rest/api/searchservice/test-analyzer) als Untersuchungstool zu verwenden. Die Antwort besteht aus Token, die von einem bestimmten Analyzer generiert wurden.

<a name="examples"></a>

## <a name="rest-examples"></a>REST-Beispiele

Die folgenden Beispiele zeigen Analysedefinitionen für einige gängige Szenarien.

+ [Beispiel eines benutzerdefinierten Analysetools](#Custom-analyzer-example)
+ [Beispiel für das Zuweisen eines Analysetools zu einem Feld](#Per-field-analyzer-assignment-example)
+ [Mischen von Analysetools für Indizierung und Suchvorgänge](#Mixing-analyzers-for-indexing-and-search-operations)
+ [Beispiel für ein Sprachanalysetool](#Language-analyzer-example)

<a name="Custom-analyzer-example"></a>

### <a name="custom-analyzer-example"></a>Beispiel eines benutzerdefinierten Analysetools

Dieses Beispiel zeigt eine Analysedefinition mit benutzerdefinierten Optionen. Benutzerdefinierte Optionen für char-Filter, Tokenizer und token-Filter werden separat als benannte Konstrukte angegeben, auf die dann in der Analysedefinition verwiesen wird. Vordefinierte Elemente werden unverändert verwendet und einfach über ihren Namen angegeben.

Das Beispiel im Detail:

+ Analysen sind eine Eigenschaft der Feldklasse für ein durchsuchbares Feld.

+ Eine benutzerdefinierte Analyse ist Teil einer Indexdefinition. Sie kann geringfügig (etwa durch Anpassung einer einzelnen Option in einem einzelnen Filter) oder an mehreren Stellen angepasst werden.

+ In diesem Fall ist „my_analyzer“ die benutzerdefinierte Analyse, die wiederum den angepassten Standard-Tokenizer „my_standard_tokenizer“ und zwei token-Filter („lowercase“ und den angepassten asciifolding-Filter „my_asciifolding“) verwendet.

+ Zudem werden 2 benutzerdefinierte char-Filter definiert: „map_dash“ und „remove_whitespace“. Der erste ersetzt alle Striche durch Unterstriche, der zweite entfernt alle Leerzeichen. Leerzeichen müssen in den Zuordnungsregeln UTF-8-kodiert sein. Die char-Filter werden vor der Tokenisierung angewendet und wirken sich auf die resultierenden Token aus (der Standardtokenisierer trennt bei Gedankenstrichen und Leerzeichen, nicht aber bei Unterstrichen).

```json
  {
     "name":"myindex",
     "fields":[
        {
           "name":"id",
           "type":"Edm.String",
           "key":true,
           "searchable":false
        },
        {
           "name":"text",
           "type":"Edm.String",
           "searchable":true,
           "analyzer":"my_analyzer"
        }
     ],
     "analyzers":[
        {
           "name":"my_analyzer",
           "@odata.type":"#Microsoft.Azure.Search.CustomAnalyzer",
           "charFilters":[
              "map_dash",
              "remove_whitespace"
           ],
           "tokenizer":"my_standard_tokenizer",
           "tokenFilters":[
              "my_asciifolding",
              "lowercase"
           ]
        }
     ],
     "charFilters":[
        {
           "name":"map_dash",
           "@odata.type":"#Microsoft.Azure.Search.MappingCharFilter",
           "mappings":["-=>_"]
        },
        {
           "name":"remove_whitespace",
           "@odata.type":"#Microsoft.Azure.Search.MappingCharFilter",
           "mappings":["\\u0020=>"]
        }
     ],
     "tokenizers":[
        {
           "name":"my_standard_tokenizer",
           "@odata.type":"#Microsoft.Azure.Search.StandardTokenizerV2",
           "maxTokenLength":20
        }
     ],
     "tokenFilters":[
        {
           "name":"my_asciifolding",
           "@odata.type":"#Microsoft.Azure.Search.AsciiFoldingTokenFilter",
           "preserveOriginal":true
        }
     ]
  }
```

<a name="Per-field-analyzer-assignment-example"></a>

### <a name="per-field-analyzer-assignment-example"></a>Beispiel für die Zuweisung eines Analysetools je Feld

Die Standardanalyse ist die Standardeinstellung. Angenommen, Sie möchten die Standardeinstellung durch eine andere vordefinierte Analyse (beispielsweise „pattern“) ersetzen. Wenn Sie keine benutzerdefinierten Optionen festlegen, müssen Sie nur den Namen in der Felddefinition angeben.

Das Element „analyzer“ überschreibt die Standardanalyse für das jeweilige Feld. Es findet keine globale Überschreibung statt. In diesem Beispiel verwendet `text1` die Analyse „pattern“ und `text2` die Standardanalyse, da hierfür keine Analyse angegeben ist.

```json
  {
     "name":"myindex",
     "fields":[
        {
           "name":"id",
           "type":"Edm.String",
           "key":true,
           "searchable":false
        },
        {
           "name":"text1",
           "type":"Edm.String",
           "searchable":true,
           "analyzer":"pattern"
        },
        {
           "name":"text2",
           "type":"Edm.String",
           "searchable":true
        }
     ]
  }
```

<a name="Mixing-analyzers-for-indexing-and-search-operations"></a>

### <a name="mixing-analyzers-for-indexing-and-search-operations"></a>Mischen von Analysetools für Indizierung und Suchvorgänge

Die APIs enthalten zusätzliche Indexattribute, die es ermöglichen, unterschiedliche Analyzer für Indizierung und Suchvorgänge anzugeben. searchAnalyzer- und indexAnalyzer-Attribut müssen als Paar angegeben werden und ersetzen das Einzelattribut analyzer.


```json
  {
     "name":"myindex",
     "fields":[
        {
           "name":"id",
           "type":"Edm.String",
           "key":true,
           "searchable":false
        },
        {
           "name":"text",
           "type":"Edm.String",
           "searchable":true,
           "indexAnalyzer":"whitespace",
           "searchAnalyzer":"simple"
        },
     ],
  }
```

<a name="Language-analyzer-example"></a>

### <a name="language-analyzer-example"></a>Beispiel für ein Sprachanalysetool

Für Felder mit Zeichenfolgen in unterschiedlichen Sprachen kann eine Sprachanalyse verwendet werden, während für andere Felder weiterhin die Standardanalyse (oder eine andere vordefinierte oder benutzerdefinierte Analyse) verwendet wird. Bei Verwendung einer Sprachanalyse muss sie sowohl für die Indizierung als auch für Suchvorgänge verwendet werden. Bei Feldern mit Sprachanalyse können für Indizierung und Suche keine unterschiedlichen Analysen verwendet werden.

```json
  {
     "name":"myindex",
     "fields":[
        {
           "name":"id",
           "type":"Edm.String",
           "key":true,
           "searchable":false
        },
        {
           "name":"text",
           "type":"Edm.String",
           "searchable":true,
           "indexAnalyzer":"whitespace",
           "searchAnalyzer":"simple"
        },
        {
           "name":"text_fr",
           "type":"Edm.String",
           "searchable":true,
           "analyzer":"fr.lucene"
        }
     ],
  }
```

## <a name="c-examples"></a>C#-Beispiele

Wenn Sie die.NET SDK-Codebeispiele verwenden, können Sie diese Beispiele anhängen, um Analysetools zu verwenden oder zu konfigurieren.

+ [Zuweisen eines integrierten Analysetools](#Assign-a-language-analyzer)
+ [Konfigurieren eines Analysetools](#Define-a-custom-analyzer)

<a name="Assign-a-language-analyzer"></a>

### <a name="assign-a-language-analyzer"></a>Zuweisen eines Sprachanalysetools

Jedes in unveränderter Form ohne Konfiguration verwendete Analysetool wird in einer Felddefinition angegeben. Es ist nicht erforderlich, einen Eintrag im Abschnitt **Analysetools** des Indexes zu erstellen. 

Sprachanalysen werden in der vorliegenden Form verwendet. Um sie zu verwenden, rufen Sie [LexicalAnalyzer](/dotnet/api/azure.search.documents.indexes.models.lexicalanalyzer) unter Angabe des Typs [LexicalAnalyzerName](/dotnet/api/azure.search.documents.indexes.models.lexicalanalyzername) auf, mit dem ein in Azure Cognitive Search unterstütztes Textanalysetool bereitgestellt wird.

Benutzerdefinierte Analysetools werden in der Felddefinition auf ähnliche Weise angegeben. Hierzu müssen Sie das Analysetool in der Indexdefinition angeben. Eine entsprechende Anleitung finden Sie im nächsten Abschnitt.

```csharp
    public partial class Hotel
    {
       . . . 
        [SearchableField(AnalyzerName = LexicalAnalyzerName.Values.EnLucene)]
        public string Description { get; set; }

        [SearchableField(AnalyzerName = LexicalAnalyzerName.Values.FrLucene)]
        [JsonPropertyName("Description_fr")]
        public string DescriptionFr { get; set; }

        [SearchableField(AnalyzerName = "url-analyze")]
        public string Url { get; set; }
      . . .
    }
```

<a name="Define-a-custom-analyzer"></a>

### <a name="define-a-custom-analyzer"></a>Definieren eines benutzerdefinierten Analysetools

Wenn eine Anpassung oder Konfiguration erforderlich ist, fügen Sie einem Index ein Analysetoolkonstrukt hinzu. Nach der Definition können Sie es der Felddefinition hinzufügen, wie im vorherigen Beispiel gezeigt.

Erstellen Sie ein [CustomAnalyzer](/dotnet/api/azure.search.documents.indexes.models.customanalyzer)-Objekt. Ein benutzerdefiniertes Analysetool ist eine benutzerdefinierte Kombination aus einem bekannten Tokenizer, keinem oder mehreren Tokenfiltern und keinem oder mehreren Zeichenfilternamen:

+ [CustomAnalyzer.Tokenizer](/dotnet/api/microsoft.azure.search.models.customanalyzer.tokenizer)
+ [CustomAnalyzer.TokenFilters](/dotnet/api/microsoft.azure.search.models.customanalyzer.tokenfilters)
+ [CustomAnalyzer.CharFilters](/dotnet/api/microsoft.azure.search.models.customanalyzer.charfilters)

Im folgenden Beispiel wird ein benutzerdefiniertes Analysetool mit dem Namen „url-analyze“ erstellt, für das der [Tokenizer „uax_url_email“](/dotnet/api/microsoft.azure.search.models.customanalyzer.tokenizer) und der [Kleinbuchstaben-Tokenfilter](/dotnet/api/microsoft.azure.search.models.tokenfiltername.lowercase) verwendet werden.

```csharp
private static void CreateIndex(string indexName, SearchIndexClient adminClient)
{
   FieldBuilder fieldBuilder = new FieldBuilder();
   var searchFields = fieldBuilder.Build(typeof(Hotel));

   var analyzer = new CustomAnalyzer("url-analyze", "uax_url_email")
   {
         TokenFilters = { TokenFilterName.Lowercase }
   };

   var definition = new SearchIndex(indexName, searchFields);

   definition.Analyzers.Add(analyzer);

   adminClient.CreateOrUpdateIndex(definition);
}
```

Weitere Beispiele finden Sie unter [CustomAnalyzerTests.cs](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Microsoft.Azure.Search/tests/Tests/CustomAnalyzerTests.cs).

## <a name="next-steps"></a>Nächste Schritte

Eine ausführliche Beschreibung der Abfrageausführung finden Sie in der [Volltextsuche in Azure Cognitive Search](search-lucene-query-architecture.md). In diesem Artikel werden anhand von Beispielen Verhaltensweisen erklärt, die auf den ersten Blick vielleicht nicht intuitiv erscheinen.

Weitere Informationen zu Analysetools erhalten Sie in den folgenden Artikeln:

+ [Hinzufügen eines Sprachanalysetools](index-add-language-analyzers.md)
+ [Hinzufügen eines benutzerdefinierten Analysetools](index-add-custom-analyzers.md)
+ [Erstellen von Suchindizes in Azure Cognitive Search](search-what-is-an-index.md)
+ [Erstellen eines mehrsprachigen Index](search-language-support.md)