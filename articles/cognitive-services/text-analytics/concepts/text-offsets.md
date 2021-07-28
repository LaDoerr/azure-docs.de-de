---
title: Textversätze in der Textanalyse-API
titleSuffix: Azure Cognitive Services
description: Erfahren Sie mehr über Versätze, die durch mehrsprachige Codierungen und Emoji-Codierungen verursacht werden.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 05/18/2021
ms.author: aahi
ms.reviewer: jdesousa
ms.openlocfilehash: 348805670187f2362eb17dae40aa94e70f1daa24
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110084680"
---
# <a name="text-offsets-in-the-text-analytics-api-output"></a>Textversätze in der Ausgabe der Textanalyse-API

Die Unterstützung für mehrere Sprachen und Emojis hat zu Unicode-Codierungen geführt, die ein einzelnes angezeigtes Zeichen (ein so genanntes Graphem) mit mehreren [Codepunkten](https://wikipedia.org/wiki/Code_point) darstellen. Für Emojis wie 🌷 und 👍 werden beispielsweise mehrere Zeichen zum Erstellen der Form und zusätzliche Zeichen für visuelle Attribute wie etwa die Hautfarbe verwendet. Ebenso wird das Hindi-Wort `अनुच्छेद` mit fünf Buchstaben und drei Verbindungszeichen codiert.

Aufgrund der unterschiedlichen Längen der möglichen mehrsprachigen Codierungen und Emoji-Codierungen gibt die Textanalyse-API in der Antwort möglicherweise Versätze zurück.

## <a name="offsets-in-the-api-response"></a>Versätze in der API-Antwort 

Bedenken Sie Folgendes, wenn Versätze in der API-Antwort zurückgegeben werden, z. B. bei der [Erkennung benannter Entitäten](../how-tos/text-analytics-how-to-entity-linking.md) oder [Standpunktanalyse](../how-tos/text-analytics-how-to-sentiment-analysis.md):

* Elemente in der Antwort können spezifisch für den aufgerufenen Endpunkt sein. 
* HTTP POST/GET-Nutzlasten werden in [UTF-8](https://www.w3schools.com/charsets/ref_html_utf8.asp) codiert. Dies kann die Standardzeichencodierung Ihres clientseitigen Compilers oder Betriebssystems sein oder auch nicht.
* Versätze beziehen sich auf die auf dem [Unicode 8.0.0](https://unicode.org/versions/Unicode8.0.0)-Standard basierende Graphemanzahl, nicht auf die Zeichenanzahl.

## <a name="extracting-substrings-from-text-with-offsets"></a>Extrahieren von Teilzeichenfolgen aus Text mit Versätzen

Versätze können Probleme verursachen, wenn zeichenbasierte substring-Methoden verwendet werden, z. B. die .NET-Methode [substring()](/dotnet/api/system.string.substring). Eines dieser Probleme ist, dass eine substring-Methode aufgrund eines Versatzes in der Mitte einer Graphemcodierung mit mehreren Zeichen beendet werden kann und nicht an deren Ende.

Verwenden Sie in .NET ggf. die [StringInfo](/dotnet/api/system.globalization.stringinfo)-Klasse. Sie ermöglicht es Ihnen, eine Zeichenfolge nicht als einzelne Zeichenobjekte, sondern als eine Reihe von Textelementen zu verarbeiten. Sie können auch nach Graphem-Splitter-Bibliotheken in Ihrer bevorzugten Softwareumgebung suchen. 

Die Textanalyse-API gibt diese Textelemente aus Gründen der Einfachheit ebenfalls zurück.

## <a name="offsets-in-api-version-31-preview"></a>Offsets in API-Version 3.1-preview

In Version 3.1 der API unterstützen alle Textanalyse-API-Endpunkte, die einen Offset zurückgeben, den `stringIndexType`-Parameter. Mit diesem Parameter werden die `offset`- und `length`-Attribute in der API-Ausgabe an das angeforderte Zeichenfolgeniterationsschema angepasst. Derzeit werden drei Typen unterstützt:

1. `textElement_v8` (Standardwert): Durchläuft Grapheme gemäß der Definition des [Unicode 8.0.0](https://unicode.org/versions/Unicode8.0.0)-Standards.
2. `unicodeCodePoint`: Durchläuft [Unicode-Codepunkte](http://www.unicode.org/versions/Unicode13.0.0/ch02.pdf#G25564), das Standardschema für Python 3.
3. `utf16CodeUnit`: Durchläuft [UTF-16-Codeeinheiten](https://unicode.org/faq/utf_bom.html#UTF16), das Standardschema für JavaScript, Java und .NET.

Wenn der angeforderte `stringIndexType` der gewünschten Programmierumgebung entspricht, kann die Extraktion von Substrings mithilfe von Substringstandard- oder -slicemethoden erfolgen. 

## <a name="see-also"></a>Weitere Informationen

* [Übersicht über die Textanalyse](../overview.md)
* [Standpunktanalyse](../how-tos/text-analytics-how-to-sentiment-analysis.md)
* [Entitätserkennung](../how-tos/text-analytics-how-to-entity-linking.md)
* [Sprache erkennen](../how-tos/text-analytics-how-to-keyword-extraction.md)
* [Spracherkennung](../how-tos/text-analytics-how-to-language-detection.md)
