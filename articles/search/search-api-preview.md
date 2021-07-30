---
title: Liste der Previewfunktionen
titleSuffix: Azure Cognitive Search
description: Previewfunktionen werden veröffentlicht, damit Kunden Feedback zu ihrer Auslegung und Nutzbarkeit geben können. Dieser Artikel stellt eine umfassende Liste aller Funktionen dar, die sich derzeit in der Vorschau befinden.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 03/25/2021
ms.openlocfilehash: ca4210465039044587e61d5df92db1385f1be052
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110469833"
---
# <a name="preview-features-in-azure-cognitive-search"></a>Previewfunktionen in Azure Cognitive Search

Dieser Artikel stellt eine umfassende Liste aller Funktionen dar, die sich in der öffentlichen Vorschau befinden. Die Vorschaufunktion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Previewfunktionen, die in die allgemeine Verfügbarkeit übergehen, werden aus dieser Liste entfernt. Wenn eine Funktion im Folgenden nicht aufgeführt ist, können Sie davon ausgehen, dass sie allgemein verfügbar ist. Ankündigungen zur allgemeinen Verfügbarkeit finden Sie in den [Dienstupdates](https://azure.microsoft.com/updates/?product=search) sowie unter [Neuigkeiten](whats-new.md).

|Funktion&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  | Category | BESCHREIBUNG | Verfügbarkeit  |
|---------|------------------|-------------|---------------|
|  [**Power Query-Connectors**](search-how-to-index-power-query-data-sources.md) | Indexerdatenquelle | Indexer können jetzt von anderen Cloudplattformen aus indizieren. Wenn Sie einen Indexer verwenden, um externe Datenquellen zur Indizierung zu crawlen, können Sie jetzt Power Query-Connectors verwenden, um eine Verbindung zu Amazon Redshift, Elasticsearch, PostgreSQL, Salesforce Objects, Salesforce Reports, Smartsheet und Snowflake herzustellen. | [Registrierung](https://aka.ms/azure-cognitive-search/indexer-preview) ist erforderlich, damit Unterstützung für Ihr Abonnement im Back-End aktiviert werden kann. Greifen Sie auf diese Datenquelle mithilfe von [Datenquelle erstellen (REST)](/rest/api/searchservice/create-data-source) in der API-Version 2020-06-30-Preview oder über das Azure-Portal zu.|
|  [**MySQL-Indexerdatenquelle**](search-howto-index-mysql.md) | Indexerdatenquelle | Indizieren Sie Inhalte und Metadaten aus Azure MySQL-Datenquellen.| [Registrierung](https://aka.ms/azure-cognitive-search/indexer-preview) ist erforderlich, damit Unterstützung für Ihr Abonnement im Back-End aktiviert werden kann. Greifen Sie auf diese Datenquelle mithilfe von [Datenquelle erstellen (REST)](/rest/api/searchservice/create-data-source) in der API-Version 2020-06-30-Preview, über das [.NET SDK 11.2.1](/dotnet/api/azure.search.documents.indexes.models.searchindexerdatasourcetype.mysql) oder über das Azure-Portal zu. |
| [**Semantische Suche**](semantic-search-overview.md) | Relevanz (Bewertung) | Semantische Rangfolge von Ergebnissen, Beschriftungen und Antworten. | [Search-REST-API 2020-06-30-Preview](/rest/api/searchservice/preview-api/search-documents) und Such-Explorer (Portal). |
| [**speller**](cognitive-search-aml-skill.md) | Abfrage | Optionale Rechtschreibkorrektur für Abfragebegriffeingaben für einfache, vollständige und semantische Abfragen. | [Search-REST-API 2020-06-30-Preview](/rest/api/searchservice/preview-api/search-documents) |
| [**SharePoint Online-Indexer**](search-howto-index-sharepoint-online.md) | Indexerdatenquelle | Neue Datenquelle für die indexerbasierte Indizierung von SharePoint-Inhalten. | [Search-REST-API 2020-06-30-Preview](/rest/api/searchservice/preview-api/create-indexer) |
| [**AML-Skill (Azure Machine Learning)**](cognitive-search-aml-skill.md) | KI-Anreicherung| Ein neuer Skilltyp zum Integrieren eines Rückschlussendpunkts aus Azure Machine Learning. Erste Schritte mit [diesem Tutorial](cognitive-search-tutorial-aml-custom-skill.md). | Verwenden Sie [Search-REST-API 2020-06-30-Preview](/rest/api/searchservice/) oder 2019-05-06-Preview. Ebenfalls im Portal im Skillsetentwurf verfügbar. Dabei wird davon ausgegangen, das die Dienste Cognitive Search und Azure ML im gleichen Abonnement bereitgestellt wurden. |
| [**featuresMode-Parameter**](/rest/api/searchservice/preview-api/search-documents#query-parameters) | Relevanz (Bewertung) | Erweiterung der Relevanzbewertung um Details: Ähnlichkeitsbewertung pro Feld, Ausdruckshäufigkeit pro Feld und Anzahl der zugeordneten eindeutigen Token pro Feld. Sie können diese Datenpunkte in [benutzerdefinierten Bewertungslösungen](https://github.com/Azure-Samples/search-ranking-tutorial) verwenden. | Fügen Sie diesen Abfrageparameter mithilfe von [Dokumente durchsuchen (REST)](/rest/api/searchservice/preview-api/search-documents) in api-version=2020-06-30-Preview oder 2019-05-06-Preview hinzu. |
| [**Debugsitzungen**](cognitive-search-debug-session.md) | Portal, KI-Anreicherung (Skillset) | Ein in die Sitzung integrierter Skillset-Editor, der zum Untersuchen und Beheben von Problemen bei einem Skillset verwendet wird. Während einer Debugsitzung angewendete Korrekturen können in einem Skillset im Dienst gespeichert werden. | Nur im Portal mithilfe von Links zur Seitenmitte zum Öffnen einer Debugsitzung. |
| [**Natives vorläufiges Löschen von Blobs**](search-howto-index-changed-deleted-blobs.md) | Indexer, Azure-Blobs| Der Azure Blob Storage-Indexer in Azure Cognitive Search erkennt Blobs, die sich im vorläufig gelöschten Zustand befinden. Das entsprechende Suchdokument wird während der Indizierung entfernt. | Fügen Sie diese Konfigurationseinstellung mithilfe von [Indexer erstellen (REST)](/rest/api/searchservice/create-indexer) in api-version=2020-06-30-Preview oder api-version=2019-05-06-Preview hinzu. |
| [**Skill für PII-Erkennung**](cognitive-search-skill-pii-detection.md) | KI-Anreicherung (Skillset) | Ein kognitiver Skill, der bei der Indizierung verwendet wird. Er extrahiert personenbezogene Informationen aus einem Eingabetext und bietet verschiedene Maskierungsmöglichkeiten für diese Informationen. | Verweisen Sie auf diesen Preview-Skill über den Skillset-Editor im Portal oder mithilfe von [Skillset erstellen (REST)](/rest/api/searchservice/create-skillset) in api-version=2020-06-30-Preview oder api-version=2019-05-06-Preview. |
| [**Inkrementelle Anreicherung**](cognitive-search-incremental-indexing-conceptual.md) | Indexerkonfiguration| Fügt einer Anreicherungspipeline die Zwischenspeicherung hinzu, sodass Sie vorhandene Ausgaben wiederverwenden können, wenn die Inhalte durch eine Zieländerung, z. B. eine Aktualisierung am Skillset oder einem anderen Objekt, nicht geändert werden. Die Zwischenspeicherung betrifft nur angereicherte Dokumente, die über ein Skillset erstellt werden.| Fügen Sie diese Konfigurationseinstellung mithilfe von [Indexer erstellen (REST)](/rest/api/searchservice/create-indexer) in api-version=2020-06-30-Preview oder api-version=2019-05-06-Preview hinzu. |
| [**Cosmos DB-Indexer: MongoDB API, Gremlin API, Cassandra API**](search-howto-index-cosmosdb.md) | Indexerdatenquelle | Für Cosmos DB ist die SQL-API allgemein verfügbar, aber die MongoDB-, Gremlin- und Cassandra-APIs befinden sich in der Vorschauphase. | Nur für die Gremlin- und die Cassandra-API müssen Sie sich [zuerst registrieren](https://aka.ms/azure-cognitive-search/indexer-preview), damit Unterstützung für Ihr Abonnement im Back-End aktiviert werden kann. MongoDB-Datenquellen können im Portal konfiguriert werden. Darüber hinaus wird die Datenquellenkonfiguration für alle drei APIs mithilfe von [Datenquelle erstellen (REST)](/rest/api/searchservice/create-data-source) in api-version=2020-06-30-Preview oder api-version=2019-05-06-Preview unterstützt. |
| [**moreLikeThis**](search-more-like-this.md) | Abfrage | Sucht Dokumente, die für ein bestimmtes Dokument relevant sind. Dieses Feature war in früheren Vorschauversionen enthalten. | Fügen Sie diesen Abfrageparameter mithilfe von Aufrufen von [Dokumente durchsuchen (REST)](/rest/api/searchservice/search-documents) in api-version=2020-06-30-Preview, 2019-05-06-Preview, 2016-09-01-Preview, 2017-11-11-Preview hinzu. |

## <a name="how-to-call-a-preview-rest-api"></a>Aufrufen einer Vorschau-REST-API

Die kognitive Azure-Suche bietet immer zuerst Vorabversionen experimenteller Funktionen über die REST-API, dann über Vorabversionen des .NET SDK.

Vorschaufunktionen stehen für Tests und Experimente zur Verfügung, mit dem Ziel, Feedback zu Entwurf und Implementierung der Funktionen zu sammeln. Aus diesem Grund können sich Vorschaufunktionen mit der Zeit ändern – mitunter in einer Weise, die die Abwärtskompatibilität beeinträchtigt. Dies steht im Gegensatz zu Funktionen in der allgemein verfügbaren Version (GA), die stabil sind und sich bis auf kleinere abwärtskompatible Fehlerbehebungen und Verbesserungen eher nicht ändern. Außerdem werden Vorschaufunktionen nicht immer in der GA-Version übernommen.

Auch wenn einige Previewfunktionen im Portal und im .NET SDK verfügbar sein können, enthält die REST-API immer alle Previewfunktionen.

+ Für Suchvorgänge lautet die aktuelle Vorschauversion [ **`2020-06-30-Preview`**](/rest/api/searchservice/index-preview).

+ Für Verwaltungsvorgänge lautet die aktuelle Vorschauversion [ **`2019-10-01-Preview`**](/rest/api/searchmanagement/index-2019-10-01-preview).

Ältere Vorschauversionen sind zwar noch betriebsbereit, veralten jedoch mit der Zeit. Wenn Ihr Code `api-version=2019-05-06-Preview`, `api-version=2016-09-01-Preview` oder `api-version=2017-11-11-Preview` aufruft, sind diese Aufrufe noch gültig. Allerdings wird nur die neueste Vorschauversion mit Verbesserungen aktualisiert.

Die folgende Beispielsyntax veranschaulicht einen Aufruf der API-Vorschauversion.

```HTTP
POST https://[service name].search.windows.net/indexes/hotels-idx/docs/search?api-version=2020-06-30-Preview  
  Content-Type: application/json  
  api-key: [admin key]
```

Der Dienst für die kognitive Azure-Suche ist in mehreren Versionen verfügbar. Weitere Informationen finden Sie unter [API-Versionen](search-api-versions.md).

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die Referenzdokumentation für die Search-REST-Vorschau-API an. Wenn Probleme auftreten, können Sie sich über [Stack Overflow](https://stackoverflow.com/) an uns wenden oder [den Support kontaktieren](https://azure.microsoft.com/support/community/?product=search).

> [!div class="nextstepaction"]
> [Referenz zur REST-API für den Azure-Suchdienst (Vorschau)](/rest/api/searchservice/index-preview)