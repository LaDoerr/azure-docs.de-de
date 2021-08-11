---
title: 'Schnellstart: Textmining mithilfe der Textanalyse-Clientbibliothek'
titleSuffix: Azure Cognitive Services
description: Verwenden Sie diese Schnellstartanleitung, um Stimmungsanalyse und mehr mithilfe der Textanalyse-API aus Azure Cognitive Services durchzuführen.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 07/08/2021
ms.author: aahi
keywords: Textmining, Stimmungsanalyse, Textanalyse
ms.custom: devx-track-python, devx-track-js, devx-track-csharp, cog-serv-seo-aug-2020
zone_pivot_groups: programming-languages-text-analytics
ms.openlocfilehash: 69026d9abd8e0f3a978d9c327e9cb9c2e0140d04
ms.sourcegitcommit: e0ef8440877c65e7f92adf7729d25c459f1b7549
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2021
ms.locfileid: "113565776"
---
# <a name="quickstart-use-the-text-analytics-client-library-and-rest-api"></a>Schnellstart: Verwenden der Textanalyse-Clientbibliothek und der REST-API

Dieser Artikel hilft Ihnen beim Einstieg in die Verwendung der Textanalyse-Clientbibliothek und der REST-API. Befolgen Sie diese Schritte, um die Codebeispiele für das Text Mining zu testen:

* Stimmungsanalyse
* Opinion Mining
* Spracherkennung
* Entitätserkennung
* Erkennung personenbezogener Informationen
* Schlüsselwortextraktion


::: zone pivot="programming-language-csharp"

> [!IMPORTANT]
> * Die neueste stabile Version der Textanalyse-API ist `3.1`.
>    * Befolgen Sie nur die Anweisungen für die von Ihnen verwendete Version.
> * Im Code dieses Artikels werden der Einfachheit halber synchrone Methoden und ein ungeschützter Anmeldeinformationsspeicher verwendet. Für Produktionsszenarien wird aus Leistungs- und Skalierbarkeitsgründen die Verwendung der asynchronen Batchmethoden empfohlen. Informationen finden Sie in der Referenzdokumentation weiter unten.
> * Wenn Sie die Textanalyse für Gesundheit oder asynchrone Vorgänge verwenden möchten, sehen Sie sich auf GitHub die Beispiele für [C#](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics/Azure.AI.TextAnalytics), [Python](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/textanalytics/azure-ai-textanalytics/) oder [Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/textanalytics/azure-ai-textanalytics) an.

[!INCLUDE [C# quickstart](../includes/quickstarts/csharp-sdk.md)]

::: zone-end

::: zone pivot="programming-language-java"

> [!IMPORTANT]
> * Die neueste stabile Version der Textanalyse-API ist `3.1`.
> * Im Code dieses Artikels werden der Einfachheit halber synchrone Methoden und ein ungeschützter Anmeldeinformationsspeicher verwendet. Für Produktionsszenarien wird aus Leistungs- und Skalierbarkeitsgründen die Verwendung der asynchronen Batchmethoden empfohlen. Informationen finden Sie in der Referenzdokumentation weiter unten.
Wenn Sie die Textanalyse für Gesundheit oder asynchrone Vorgänge verwenden möchten, sehen Sie sich auf GitHub die Beispiele für [C#](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics/Azure.AI.TextAnalytics), [Python](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/textanalytics/azure-ai-textanalytics/) oder [Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/textanalytics/azure-ai-textanalytics) an.

[!INCLUDE [Java quickstart](../includes/quickstarts/java-sdk.md)]

::: zone-end

::: zone pivot="programming-language-javascript"

> [!IMPORTANT]
> * Die neueste stabile Version der Textanalyse-API ist `3.1`.
>    * Befolgen Sie nur die Anweisungen für die von Ihnen verwendete Version.
> * Im Code dieses Artikels werden der Einfachheit halber synchrone Methoden und ein ungeschützter Anmeldeinformationsspeicher verwendet. Für Produktionsszenarien wird aus Leistungs- und Skalierbarkeitsgründen die Verwendung der asynchronen Batchmethoden empfohlen. Informationen finden Sie in der Referenzdokumentation weiter unten.
> * Diese Version der Textanalyse-Clientbibliothek können Sie auch [in Ihrem Browser](https://github.com/Azure/azure-sdk-for-js/blob/master/documentation/Bundling.md) ausführen.

[!INCLUDE [NodeJS quickstart](../includes/quickstarts/nodejs-sdk.md)]

::: zone-end

::: zone pivot="programming-language-python"

> [!IMPORTANT]
> * Die neueste stabile Version der Textanalyse-API ist `3.1`.
>    * Befolgen Sie nur die Anweisungen für die von Ihnen verwendete Version.
> * Im Code dieses Artikels werden der Einfachheit halber synchrone Methoden und ein ungeschützter Anmeldeinformationsspeicher verwendet. Für Produktionsszenarien wird aus Leistungs- und Skalierbarkeitsgründen die Verwendung der asynchronen Batchmethoden empfohlen. Informationen finden Sie in der Referenzdokumentation weiter unten. Wenn Sie die Textanalyse für Gesundheit oder asynchrone Vorgänge verwenden möchten, sehen Sie sich auf GitHub die Beispiele für [C#](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics/Azure.AI.TextAnalytics), [Python](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/textanalytics/azure-ai-textanalytics/) oder [Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/textanalytics/azure-ai-textanalytics) an.

[!INCLUDE [Python quickstart](../includes/quickstarts/python-sdk.md)]

::: zone-end

::: zone pivot="rest-api"

> [!IMPORTANT]
> * Die neueste stabile Version der Textanalyse-API ist `3.1`.
>    * Befolgen Sie nur die Anweisungen für die von Ihnen verwendete Version.

[!INCLUDE [REST API quickstart](../includes/quickstarts/rest-api.md)]

::: zone-end

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie ein Cognitive Services-Abonnement bereinigen und entfernen möchten, können Sie die Ressource oder die Ressourcengruppe löschen. Wenn Sie die Ressourcengruppe löschen, werden auch alle anderen Ressourcen gelöscht, die ihr zugeordnet sind.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure-Befehlszeilenschnittstelle](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Lösung erkunden](../text-analytics-user-scenarios.md#analyze-recorded-inbound-customer-calls)

* [Übersicht über die Textanalyse](../overview.md)
* [Standpunktanalyse](../how-tos/text-analytics-how-to-sentiment-analysis.md)
* [Entitätserkennung](../how-tos/text-analytics-how-to-entity-linking.md)
* [Sprache erkennen](../how-tos/text-analytics-how-to-keyword-extraction.md)
* [Spracherkennung](../how-tos/text-analytics-how-to-language-detection.md)
