---
title: 'Alle vordefinierten Entitäten: LUIS'
titleSuffix: Azure Cognitive Services
description: Dieser Artikel enthält Listen mit vordefinierten Entitäten, die in LUIS (Language Understanding Intelligent Service) enthalten sind.
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 05/05/2021
ms.openlocfilehash: afd985af692390e51ef7008ecdb369ded3885080
ms.sourcegitcommit: 1fbd591a67e6422edb6de8fc901ac7063172f49e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2021
ms.locfileid: "109486033"
---
# <a name="entities-per-culture-in-your-luis-model"></a>Entitäten nach Kultur in Ihrem LUIS-Modell

LUIS stellt vordefinierte Entitäten bereit.

## <a name="entity-resolution"></a>Entitätsauflösung
Wenn eine vordefinierte Entität in Ihrer Anwendung enthalten ist, schließt LUIS die entsprechende Entitätsauflösung in die Endpunktantwort ein. Alle Beispieläußerungen sind ebenfalls mit der Entität gekennzeichnet.

Das Verhalten vordefinierter Entitäten kann nicht geändert werden. Sie können jedoch die Auflösung verbessern, indem Sie [die vordefinierte Entität einer Entität für maschinelles Lernen oder einer untergeordneten Entität als Feature hinzufügen](luis-concept-entity-types.md#prebuilt-entity).

## <a name="availability"></a>Verfügbarkeit
Sofern nicht anders vermerkt, sind vordefinierte Entitäten in allen LUIS-Anwendungsgebietsschemas (Kulturen) verfügbar. Die folgende Tabelle zeigt die vordefinierten Entitäten, die für die jeweilige Kultur unterstützt werden.

|Kultur|Subkulturen|Notizen|
|--|--|--|
|Chinesisch|[zh-CN](#chinese-entity-support)||
|Niederländisch|[nl-NL](#dutch-entity-support)||
|Englisch|[en-US (Amerika)](#english-american-entity-support)||
|Französisch|[fr-CA (Kanada)](#french-canadian-entity-support), [fr-FR (Frankreich)](#french-france-entity-support), ||
|Deutsch|[de-DE](#german-entity-support)||
|Italienisch|[it-IT](#italian-entity-support)||
|Japanisch|[ja-JP](#japanese-entity-support)||
|Koreanisch|[ko-KR](#korean-entity-support)||
|Portugiesisch|[pt-BR (Brasilien)](#portuguese-brazil-entity-support)||
|Spanisch|[es-ES (Spanien)](#spanish-spain-entity-support), [es-MX (Mexiko)](#spanish-mexico-entity-support)||
|Türkisch|[Türkisch](#turkish-entity-support)||

## <a name="prediction-endpoint-runtime"></a>Vorhersageendpunkt-Laufzeit

Die Verfügbarkeit einer vordefinierten Entität in einer bestimmten Sprache wird durch die Laufzeitversion des Vorhersageendpunkts bestimmt.

## <a name="chinese-entity-support"></a>Unterstützung chinesischer Entitäten

Die folgenden Entitäten werden unterstützt:

| Vordefinierte Entität | zh-CN |
| --------------- | :---: |
[Age](luis-reference-prebuilt-age.md):<br>year<br>month<br>week<br>day   |    V2 und V3   |
[Währung (Geld)](luis-reference-prebuilt-currency.md):<br>Dollar<br>Untereinheit (z.B. „penny“)  |    V2 und V3   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>daterange<br>time<br>timerange   |    V2 und V3   |
[Dimension](luis-reference-prebuilt-dimension.md):<br>Volume<br>area<br>weight<br>information (Beispiel: bit/byte)<br>length (Beispiel: meter)<br>speed (Beispiel: mile per hour)  |    V2 und V3   |
[E-Mail](luis-reference-prebuilt-email.md)   |    V2 und V3   |
[Number](luis-reference-prebuilt-number.md)   |    V2 und V3   |
[Ordinal](luis-reference-prebuilt-ordinal.md)   |    V2 und V3   |
[Percentage](luis-reference-prebuilt-percentage.md)   |    V2 und V3   |
[PersonName](luis-reference-prebuilt-person.md)   |    V2 und V3   |
[Phonenumber](luis-reference-prebuilt-phonenumber.md)   |    V2 und V3   |
[Temperature](luis-reference-prebuilt-temperature.md):<br>fahrenheit<br>kelvin<br>rankine<br>delisle<br>celsius   |    V2 und V3   |
[URL](luis-reference-prebuilt-url.md)   |    V2 und V3   |
<!---[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   |-->
<!---[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    -   |-->
<!---[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |-->

## <a name="dutch-entity-support"></a>Unterstützung niederländischer Entitäten

Die folgenden Entitäten werden unterstützt:

| Vordefinierte Entität | nl-NL |
| --------------- | :---: |
[Age](luis-reference-prebuilt-age.md):<br>year<br>month<br>week<br>day   |    V2 und V3   |
[Währung (Geld)](luis-reference-prebuilt-currency.md):<br>Dollar<br>Untereinheit (z.B. „penny“)  |    V2 und V3   |
[Dimension](luis-reference-prebuilt-dimension.md):<br>Volume<br>area<br>weight<br>information (Beispiel: bit/byte)<br>length (Beispiel: meter)<br>speed (Beispiel: mile per hour)  |    V2 und V3   |
[E-Mail](luis-reference-prebuilt-email.md)   |    V2 und V3   |
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    V2 und V3   |
[Number](luis-reference-prebuilt-number.md)   |    V2 und V3   |
[Ordinal](luis-reference-prebuilt-ordinal.md)   |    V2 und V3   |
[Percentage](luis-reference-prebuilt-percentage.md)   |    V2 und V3   |
[Phonenumber](luis-reference-prebuilt-phonenumber.md)   |    V2 und V3   |
[Temperature](luis-reference-prebuilt-temperature.md):<br>fahrenheit<br>kelvin<br>rankine<br>delisle<br>celsius   |    V2 und V3   |
[URL](luis-reference-prebuilt-url.md)   |    V2 und V3   |
<!---[Datetime](luis-reference-prebuilt-deprecated.md)   |    -   |-->
<!---[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   |-->
<!---[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |-->
<!---[PersonName](luis-reference-prebuilt-person.md)   |    -   |-->

## <a name="english-american-entity-support"></a>Unterstützung englischer (Amerika) Entitäten

Die folgenden Entitäten werden unterstützt:

| Vordefinierte Entität | de-DE |
| --------------- | :---: |
[Age](luis-reference-prebuilt-age.md):<br>year<br>month<br>week<br>day   |    V2 und V3   |
[Währung (Geld)](luis-reference-prebuilt-currency.md):<br>Dollar<br>Untereinheit (z.B. „penny“)  |    V2 und V3   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>daterange<br>time<br>timerange   |    V2 und V3   |
[Dimension](luis-reference-prebuilt-dimension.md):<br>Volume<br>area<br>weight<br>information (Beispiel: bit/byte)<br>length (Beispiel: meter)<br>speed (Beispiel: mile per hour)  |    V2 und V3   |
[E-Mail](luis-reference-prebuilt-email.md)   |    V2 und V3   |
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    V2 und V3   |
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    V2 und V3   |
[Number](luis-reference-prebuilt-number.md)   |    V2 und V3   |
[Ordinal](luis-reference-prebuilt-ordinal.md)   |    V2 und V3   |
[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    V2 und V3   |
[Percentage](luis-reference-prebuilt-percentage.md)   |    V2 und V3   |
[PersonName](luis-reference-prebuilt-person.md)   |    V2 und V3   |
[Phonenumber](luis-reference-prebuilt-phonenumber.md)   |    V2 und V3   |
[Temperature](luis-reference-prebuilt-temperature.md):<br>fahrenheit<br>kelvin<br>rankine<br>delisle<br>celsius   |    V2 und V3   |
[URL](luis-reference-prebuilt-url.md)   |    V2 und V3   |

## <a name="french-france-entity-support"></a>Unterstützung französischer (Frankreich) Entitäten

Die folgenden Entitäten werden unterstützt:

| Vordefinierte Entität | fr-FR |
| --------------- | :---: |
[Age](luis-reference-prebuilt-age.md):<br>year<br>month<br>week<br>day   |    V2 und V3   |
[Währung (Geld)](luis-reference-prebuilt-currency.md):<br>Dollar<br>Untereinheit (z.B. „penny“)  |    V2 und V3   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>daterange<br>time<br>timerange   |    V2 und V3   |
[Dimension](luis-reference-prebuilt-dimension.md):<br>Volume<br>area<br>weight<br>information (Beispiel: bit/byte)<br>length (Beispiel: meter)<br>speed (Beispiel: mile per hour)  |    V2 und V3   |
[E-Mail](luis-reference-prebuilt-email.md)   |    V2 und V3   |
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   |
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    V2 und V3   |
[Number](luis-reference-prebuilt-number.md)   |    V2 und V3   |
[Ordinal](luis-reference-prebuilt-ordinal.md)   |    V2 und V3   |
[Percentage](luis-reference-prebuilt-percentage.md)   |    V2 und V3   |
[Phonenumber](luis-reference-prebuilt-phonenumber.md)   |    V2 und V3   |
[Temperature](luis-reference-prebuilt-temperature.md):<br>fahrenheit<br>kelvin<br>rankine<br>delisle<br>celsius   |    V2 und V3   |
[URL](luis-reference-prebuilt-url.md)   |    V2 und V3   |
<!---[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |-->
<!---[PersonName](luis-reference-prebuilt-person.md)   |   -   |-->

## <a name="french-canadian-entity-support"></a>Unterstützung französischer (Kanada) Entitäten

Die folgenden Entitäten werden unterstützt:

| Vordefinierte Entität | fr-CA |
| --------------- | :---: |
[Age](luis-reference-prebuilt-age.md):<br>year<br>month<br>week<br>day   |    V2 und V3   |
[Währung (Geld)](luis-reference-prebuilt-currency.md):<br>Dollar<br>Untereinheit (z.B. „penny“)  |    V2 und V3   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>daterange<br>time<br>timerange   |    V2 und V3   |
[Dimension](luis-reference-prebuilt-dimension.md):<br>Volume<br>area<br>weight<br>information (Beispiel: bit/byte)<br>length (Beispiel: meter)<br>speed (Beispiel: mile per hour)  |    V2 und V3   |
[E-Mail](luis-reference-prebuilt-email.md)   |    V2 und V3   |
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    V2 und V3   |
[Number](luis-reference-prebuilt-number.md)   |    V2 und V3   |
[Ordinal](luis-reference-prebuilt-ordinal.md)   |    V2 und V3   |
[Percentage](luis-reference-prebuilt-percentage.md)   |    V2 und V3   |
[Phonenumber](luis-reference-prebuilt-phonenumber.md)   |    V2 und V3   |
[Temperature](luis-reference-prebuilt-temperature.md):<br>fahrenheit<br>kelvin<br>rankine<br>delisle<br>celsius   |    V2 und V3   |
[URL](luis-reference-prebuilt-url.md)   |    V2 und V3   |
<!---[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   |-->
<!---[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |-->
<!---[PersonName](luis-reference-prebuilt-person.md)   |    -   |-->

## <a name="german-entity-support"></a>Unterstützung deutscher Entitäten

Die folgenden Entitäten werden unterstützt:

|Vordefinierte Entität | de-DE |
| -------------- | :---: |
[Age](luis-reference-prebuilt-age.md):<br>year<br>month<br>week<br>day   |    V2 und V3   |
[Währung (Geld)](luis-reference-prebuilt-currency.md):<br>Dollar<br>Untereinheit (z.B. „penny“)  |    V2 und V3   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>daterange<br>time<br>timerange   |    V2 und V3   |
[Dimension](luis-reference-prebuilt-dimension.md):<br>Volume<br>area<br>weight<br>information (Beispiel: bit/byte)<br>length (Beispiel: meter)<br>speed (Beispiel: mile per hour)  |    V2 und V3   |
[E-Mail](luis-reference-prebuilt-email.md)   |    V2 und V3   |
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    V2 und V3   |
[Number](luis-reference-prebuilt-number.md)   |    V2 und V3   |
[Ordinal](luis-reference-prebuilt-ordinal.md)   |    V2 und V3   |
[Percentage](luis-reference-prebuilt-percentage.md)   |    V2 und V3   |
[Phonenumber](luis-reference-prebuilt-phonenumber.md)   |    V2 und V3   |
[Temperature](luis-reference-prebuilt-temperature.md):<br>fahrenheit<br>kelvin<br>rankine<br>delisle<br>celsius   |    V2 und V3   |
[URL](luis-reference-prebuilt-url.md)   |    V2 und V3   |
<!---[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   |-->
<!---[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |-->
<!---[PersonName](luis-reference-prebuilt-person.md)   |    -   |-->

## <a name="italian-entity-support"></a>Unterstützung italienischer Entitäten

Die _Auflösung_ der vordefinierten italienischen Entitäten Alter, Währung, Dimension, Anzahl und Prozent hat sich in Vorschauversion V2 gegenüber V3 geändert.

Die folgenden Entitäten werden unterstützt:

| Vordefinierte Entität | it-IT |
| --------------- | :---: |
[Age](luis-reference-prebuilt-age.md):<br>year<br>month<br>week<br>day   |    V2 und V3   |
[Währung (Geld)](luis-reference-prebuilt-currency.md):<br>Dollar<br>Untereinheit (z.B. „penny“)  |    V2 und V3   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>daterange<br>time<br>timerange   |    V2 und V3   |
[Dimension](luis-reference-prebuilt-dimension.md):<br>Volume<br>area<br>weight<br>information (Beispiel: bit/byte)<br>length (Beispiel: meter)<br>speed (Beispiel: mile per hour)  |    V2 und V3   |
[E-Mail](luis-reference-prebuilt-email.md)   |    V2 und V3   |
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    V2 und V3   |
[Number](luis-reference-prebuilt-number.md)   |    V2 und V3   |
[Ordinal](luis-reference-prebuilt-ordinal.md)   |    V2 und V3   |
[Percentage](luis-reference-prebuilt-percentage.md)   |    V2 und V3   |
[Phonenumber](luis-reference-prebuilt-phonenumber.md)   |    V2 und V3   |
[Temperature](luis-reference-prebuilt-temperature.md):<br>fahrenheit<br>kelvin<br>rankine<br>delisle<br>celsius   |    V2 und V3   |
[URL](luis-reference-prebuilt-url.md)   |    V2 und V3   |
<!---[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   |-->
<!---[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |-->
<!---[PersonName](luis-reference-prebuilt-person.md)   |    -   |-->

## <a name="japanese-entity-support"></a>Unterstützung japanischer Entitäten

Die folgenden Entitäten werden unterstützt:

|Vordefinierte Entität | ja-JP |
| -------------- | :---: |
[Age](luis-reference-prebuilt-age.md):<br>year<br>month<br>week<br>day   |    V2, -   |
[Währung (Geld)](luis-reference-prebuilt-currency.md):<br>Dollar<br>Untereinheit (z.B. „penny“)  |    V2, -   |
[Dimension](luis-reference-prebuilt-dimension.md):<br>Volume<br>area<br>weight<br>information (Beispiel: bit/byte)<br>length (Beispiel: meter)<br>speed (Beispiel: mile per hour)  |    V2, -   |
[E-Mail](luis-reference-prebuilt-email.md)   |    V2 und V3   |
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    V2 und V3   |
[Number](luis-reference-prebuilt-number.md)   |    V2, -   |
[Ordinal](luis-reference-prebuilt-ordinal.md)   |    V2, -   |
[Percentage](luis-reference-prebuilt-percentage.md)   |    V2, -   |
[Phonenumber](luis-reference-prebuilt-phonenumber.md)   |    V2 und V3   |
[Temperature](luis-reference-prebuilt-temperature.md):<br>fahrenheit<br>kelvin<br>rankine<br>delisle<br>celsius   |    V2, -   |
[URL](luis-reference-prebuilt-url.md)   |    V2 und V3   |
<!---[Datetime](luis-reference-prebuilt-deprecated.md)   |    -   |-->
<!---[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   |-->
<!---[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |-->
<!---[PersonName](luis-reference-prebuilt-person.md)   |    -   |-->

## <a name="korean-entity-support"></a>Unterstützung koreanischer Entitäten

Die folgenden Entitäten werden unterstützt:

| Vordefinierte Entität | ko-KR |
| --------------- | :---: |
[E-Mail](luis-reference-prebuilt-email.md)   |    V2 und V3   |
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    V2 und V3   |
[Phonenumber](luis-reference-prebuilt-phonenumber.md)   |    V2 und V3   |
[URL](luis-reference-prebuilt-url.md)   |    V2 und V3   |
<!---[Age](luis-reference-prebuilt-age.md):<br>year<br>month<br>week<br>day   |    -   |-->
<!---[Currency (money)](luis-reference-prebuilt-currency.md):<br>Dollar<br>Untereinheit (z.B. „penny“)  |    -   |-->
<!---[Datetime](luis-reference-prebuilt-deprecated.md)   |    -   |-->
<!---[Dimension](luis-reference-prebuilt-dimension.md):<br>Volume<br>area<br>weight<br>information (Beispiel: bit/byte)<br>length (Beispiel: meter)<br>speed (Beispiel: mile per hour)  |    -   |-->
<!---[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   |-->
<!---[Number](luis-reference-prebuilt-number.md)   |    -   |-->
<!---[Ordinal](luis-reference-prebuilt-ordinal.md)   |    -   |-->
<!---[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |-->
<!---[Percentage](luis-reference-prebuilt-percentage.md)   |    -   |-->
<!---[PersonName](luis-reference-prebuilt-person.md)   |    -   |-->
<!---[Temperature](luis-reference-prebuilt-temperature.md):<br>fahrenheit<br>kelvin<br>rankine<br>delisle<br>celsius   |    -   |-->

## <a name="portuguese-brazil-entity-support"></a>Unterstützung portugiesischer (Brasilien) Entitäten

Die folgenden Entitäten werden unterstützt:

| Vordefinierte Entität | pt-BR |
| --------------- | :---: |
[Age](luis-reference-prebuilt-age.md):<br>year<br>month<br>week<br>day   |    V2 und V3   |
[Währung (Geld)](luis-reference-prebuilt-currency.md):<br>Dollar<br>Untereinheit (z.B. „penny“)  |    V2 und V3   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>daterange<br>time<br>timerange   |    V2 und V3   |
[Dimension](luis-reference-prebuilt-dimension.md):<br>Volume<br>area<br>weight<br>information (Beispiel: bit/byte)<br>length (Beispiel: meter)<br>speed (Beispiel: mile per hour)  |    V2 und V3   |
[E-Mail](luis-reference-prebuilt-email.md)   |    V2 und V3   |
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    V2 und V3   |
[Number](luis-reference-prebuilt-number.md)   |    V2 und V3   |
[Ordinal](luis-reference-prebuilt-ordinal.md)   |    V2 und V3   |
[Percentage](luis-reference-prebuilt-percentage.md)   |    V2 und V3   |
[Phonenumber](luis-reference-prebuilt-phonenumber.md)   |    V2 und V3   |
[Temperature](luis-reference-prebuilt-temperature.md):<br>fahrenheit<br>kelvin<br>rankine<br>delisle<br>celsius   |    V2 und V3   |
[URL](luis-reference-prebuilt-url.md)   |    V2 und V3   |
<!---[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   |-->
<!---[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |-->
<!---[PersonName](luis-reference-prebuilt-person.md)   |    -   |-->

Die keyPhrase-Entität ist nicht für alle Unterkulturen von Portugiesisch (Brasilien) verfügbar (```pt-BR```).

## <a name="spanish-spain-entity-support"></a>Unterstützung spanischer (Spanien) Entitäten

Die folgenden Entitäten werden unterstützt:

| Vordefinierte Entität | es-ES |
| --------------- | :---: |
[Age](luis-reference-prebuilt-age.md):<br>year<br>month<br>week<br>day   |    V2 und V3   |
[Währung (Geld)](luis-reference-prebuilt-currency.md):<br>Dollar<br>Untereinheit (z.B. „penny“)  |    V2 und V3   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>daterange<br>time<br>timerange   |    V2 und V3   |
[Dimension](luis-reference-prebuilt-dimension.md):<br>Volume<br>area<br>weight<br>information (Beispiel: bit/byte)<br>length (Beispiel: meter)<br>speed (Beispiel: mile per hour)  |    V2 und V3   |
[E-Mail](luis-reference-prebuilt-email.md)   |    V2 und V3   |
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    V2 und V3   |
[Number](luis-reference-prebuilt-number.md)   |    V2 und V3   |
[Ordinal](luis-reference-prebuilt-ordinal.md)   |    V2 und V3   |
[Percentage](luis-reference-prebuilt-percentage.md)   |    V2 und V3   |
[Phonenumber](luis-reference-prebuilt-phonenumber.md)   |    V2 und V3   |
[Temperature](luis-reference-prebuilt-temperature.md):<br>fahrenheit<br>kelvin<br>rankine<br>delisle<br>celsius   |    V2 und V3   |
[URL](luis-reference-prebuilt-url.md)   |    V2 und V3   |
<!---[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   |-->
<!---[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |-->
<!---[PersonName](luis-reference-prebuilt-person.md)   |    -   |-->

## <a name="spanish-mexico-entity-support"></a>Unterstützung spanischer (Mexiko) Entitäten

Die folgenden Entitäten werden unterstützt:

| Vordefinierte Entität | es-MX |
| --------------- | :---: |
[Age](luis-reference-prebuilt-age.md):<br>year<br>month<br>week<br>day   |    -   |
[Währung (Geld)](luis-reference-prebuilt-currency.md):<br>Dollar<br>Untereinheit (z.B. „penny“)  |    -   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>daterange<br>time<br>timerange   |    -   |
[Dimension](luis-reference-prebuilt-dimension.md):<br>Volume<br>area<br>weight<br>information (Beispiel: bit/byte)<br>length (Beispiel: meter)<br>speed (Beispiel: mile per hour)  |    -   |
[E-Mail](luis-reference-prebuilt-email.md)   |    V2 und V3   |
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    V2 und V3   |
[Number](luis-reference-prebuilt-number.md)   |    V2 und V3   |
[Ordinal](luis-reference-prebuilt-ordinal.md)   |    -   |
[Percentage](luis-reference-prebuilt-percentage.md)   |    -   |
[Phonenumber](luis-reference-prebuilt-phonenumber.md)   |    V2 und V3   |
[Temperature](luis-reference-prebuilt-temperature.md):<br>fahrenheit<br>kelvin<br>rankine<br>delisle<br>celsius   |    -   |
[URL](luis-reference-prebuilt-url.md)   |    V2 und V3   |
<!---[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   |-->
<!---[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |-->
<!---[PersonName](luis-reference-prebuilt-person.md)   |    -   |-->

<!--- See notes on [Deprecated prebuilt entities](luis-reference-prebuilt-deprecated.md)-->

## <a name="turkish-entity-support"></a>Unterstützung türkischer Entitäten

| Vordefinierte Entität | tr-TR |
| --------------- | :---: |
[Age](luis-reference-prebuilt-age.md):<br>year<br>month<br>week<br>day   |    -   |
[Währung (Geld)](luis-reference-prebuilt-currency.md):<br>Dollar<br>Untereinheit (z.B. „penny“)  |    -   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>daterange<br>time<br>timerange   |    -   |
[Dimension](luis-reference-prebuilt-dimension.md):<br>Volume<br>area<br>weight<br>information (Beispiel: bit/byte)<br>length (Beispiel: meter)<br>speed (Beispiel: mile per hour)  |    -   |
[E-Mail](luis-reference-prebuilt-email.md)   |    -   |
[Number](luis-reference-prebuilt-number.md)   |    -   |
[Ordinal](luis-reference-prebuilt-ordinal.md)   |    -   |
[Percentage](luis-reference-prebuilt-percentage.md)   |    -   |
[Temperature](luis-reference-prebuilt-temperature.md):<br>fahrenheit<br>kelvin<br>rankine<br>delisle<br>celsius   |    -   |
[URL](luis-reference-prebuilt-url.md)   |    -   |
<!---[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    V2 und V3   |-->
<!---Phonenumber](luis-reference-prebuilt-phonenumber.md)   |    -   |-->

<!--- See notes on [Deprecated prebuilt entities](luis-reference-prebuilt-deprecated.md). -->

## <a name="contribute-to-prebuilt-entity-cultures"></a>Beitragen zu vordefinierten Entitäten in verschiedenen Kulturen
Die vordefinierten Entitäten werden im Open Source-Projekt „Recognizers-Text“ entwickelt. Zum Projekt [beitragen](https://github.com/Microsoft/Recognizers-Text). Dieses Projekt enthält Beispiele zur Entität „currency“ (Währung) in den verschiedenen Kulturen.

„GeographyV2“ und „PersonName“ sind im Projekt „Recognizers-Text“ nicht enthalten. Bei Problemen mit diesen vordefinierten Entitäten stellen Sie eine [Supportanfrage](../../azure-portal/supportability/how-to-create-azure-support-request.md).

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Entitäten [number](luis-reference-prebuilt-number.md), [datetimeV2](luis-reference-prebuilt-datetimev2.md) und [currency](luis-reference-prebuilt-currency.md).
