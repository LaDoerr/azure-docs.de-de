---
title: Erstellen skalierbarer Clouddatenbanken
description: Erstellen skalierbarer .NET-Datenbank-Apps mit der Clientbibliothek für elastische Datenbanken
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: scoriani
ms.author: scoriani
ms.reviewer: mathoma
ms.date: 09/25/2018
ms.openlocfilehash: 05ad9620046f19984d26dc78bcc3de06d054b5eb
ms.sourcegitcommit: a038863c0a99dfda16133bcb08b172b6b4c86db8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2021
ms.locfileid: "113004122"
---
# <a name="building-scalable-cloud-databases"></a>Erstellen skalierbarer Clouddatenbanken
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Das horizontale Skalieren von Datenbanken kann problemlos mithilfe von skalierbaren Tools und Features für die Azure SQL-Datenbank ausgeführt werden. Insbesondere können Sie die **Clientbibliothek für elastische Datenbanken** verwenden, um horizontal skalierte Datenbanken zu erstellen und zu verwalten. Mit diesem Feature können Sie ganz einfach Shardanwendungen mithilfe von Hunderten – oder sogar Tausenden – von Datenbanken in Azure SQL-Datenbank entwickeln.

Downloads:

* Die Java-Version der Bibliothek finden Sie im [Maven Central Repository](https://search.maven.org/#search%7Cga%7C1%7Celastic-db-tools).
* Die .NET-Version der Bibliothek finden Sie unter [NuGet](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).

## <a name="documentation"></a>Dokumentation

1. [Erste Schritte mit Tools für elastische Datenbanken](elastic-scale-get-started.md)
2. [Features für elastische Datenbanken](elastic-scale-introduction.md)
3. [Shard-Zuordnungsverwaltung](elastic-scale-shard-map-management.md)
4. [Migrieren von vorhandenen Datenbanken für die Aufskalierung](elastic-convert-to-use-elastic-tools.md)
5. [Datenabhängiges Routing](elastic-scale-data-dependent-routing.md)
6. [Multishardabfragen](elastic-scale-multishard-querying.md)
7. [Hinzufügen eines Shards mithilfe der Tools für elastische Datenbanken](elastic-scale-add-a-shard.md)
8. [Mehrinstanzenfähige Anwendungen mit Tools für elastische Datenbanken und Sicherheit auf Zeilenebene](saas-tenancy-elastic-tools-multi-tenant-row-level-security.md)
9. [Aktualisieren von Clientbibliothek-Apps](elastic-scale-upgrade-client-library.md) 
10. [Übersicht über elastische Abfragen](elastic-query-overview.md)
11. [Tools für elastische Datenbanken – Glossar](elastic-scale-glossary.md)
12. [Clientbibliothek für elastische Datenbanken mit Entity Framework](elastic-scale-use-entity-framework-applications-visual-studio.md)
13. [Clientbibliothek für elastische Datenbanken mit Dapper](elastic-scale-working-with-dapper.md)
14. [Split-Merge-Tool](elastic-scale-overview-split-and-merge.md)
15. [Leistungsindikatoren für den Shardzuordnungs-Manager](elastic-database-client-library.md) 
16. [Häufig gestellte Fragen zu Tools für elastische Datenbanken](elastic-scale-faq.yml)

## <a name="client-capabilities"></a>Clientfunktionen

Horizontales Hochskalieren von Anwendungen mit *Sharding* konfrontiert Entwickler und Administrator gleichermaßen mit Herausforderungen. Die Clientbibliothek vereinfacht die Verwaltungsaufgaben durch Tools, mit denen sowohl Entwickler als auch Administratoren horizontal skalierte Datenbanken verwalten können. In einem typischen Beispiel gibt es viele Datenbanken zu verwalten, die als „Shards“ bezeichnet werden. Kunden werden in der gleichen Datenbank zusammengestellt, und es gibt eine Datenbank pro Kunde (ein Einzelmandantenschema). Die Clientbibliothek enthält die folgenden Features:

- **Shardzuordnungsverwaltung:** Eine spezielle Datenbank wird erstellt, der so genannte „Shardzuordnungs-Manager“. Die Shardzuordnungsverwaltung gibt einer Anwendung die Möglichkeit, verschiedene Metadaten über die Shards zu verwalten. Entwickler können diese Funktion verwenden, um Datenbanken als Shards zu registrieren, Zuordnungen einzelner Sharding-Schlüssel oder -Schlüsselbereiche zu diesen Datenbanken zu beschreiben und diese Metadaten zu verwalten, während sich die Anzahl und Zusammensetzung der Datenbanken gemäß den Kapazitätsänderungen weiter entwickelt. Ohne die Clientbibliothek für elastische Datenbanken ist das Schreiben des Verwaltungscodes beim Implementieren von Sharding sehr zeitaufwendig. Details finden Sie unter [Shard-Zuordnungsverwaltung](elastic-scale-shard-map-management.md).

- **Datenabhängiges Routing**: Angenommen, eine Anforderung geht bei der Anwendung ein. Anhand des Shardschlüsselwerts der Anforderung muss die Anwendung die richtige Datenbank basierend auf dem Schlüsselwert ermitteln. Anschließend öffnet sie eine Verbindung mit der Datenbank, um die Anforderung zu verarbeiten. Das datenabhängige Routing bietet die Möglichkeit, Verbindungen durch einen einfachen Aufruf über die Shard-Zuordnung der Anwendung zu öffnen. Das datenabhängige Routing ist weiterer Bereich des Infrastrukturcodes, der jetzt durch Funktionen der Clientbibliothek für elastische Datenbanken abgedeckt wird. Details finden Sie unter [Datenabhängiges Routing](elastic-scale-data-dependent-routing.md).
- **Abfragen von mehreren Shards (MSQ)** : Das Abfragen mehrerer Shards erfolgt, wenn eine Anforderung mehrere (oder alle) Shards umfasst. In einer Abfrage mehrerer Shards wird derselbe T-SQL-Code für alle Shards oder eine Gruppe von Shards ausgeführt. Die Ergebnisse der beteiligten Shards werden unter Verwendung der UNION ALL-Semantik in einem Gesamtergebnis zusammengeführt. Die Funktionalität wird über die Clientbibliothek verfügbar gemacht. Sie verarbeitet u.a. Aufgaben wie Verbindungsverwaltung, Threadverwaltung, Fehlerbehandlung und Verarbeiten von Zwischenergebnissen. MSQ kann Hunderte von Shards abfragen. Weitere Einzelheiten finden Sie unter [Abfragen mehrerer Shards](elastic-scale-multishard-querying.md).

Im Allgemeinen steht den Endkunden mit den Tools für elastische Datenbanken bei der Übermittlung lokaler Shardvorgänge die volle T-SQL-Funktionalität zur Verfügung, anders als bei shardübergreifenden Vorgängen, die eine eigene Semantik haben.



## <a name="next-steps"></a>Nächste Schritte

- Clientbibliothek für elastische Datenbanken ([Java](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-elasticdb-tools%22), [.NET](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)): zum **Herunterladen** der Bibliothek

- [Erste Schritte mit Tools für elastische Datenbanken:](elastic-scale-get-started.md) zum Testen der **Beispiel-App**, die Clientfunktionen veranschaulicht

- GitHub ([Java](https://github.com/Microsoft/elastic-db-tools-for-java/blob/master/README.md), [.NET](https://github.com/Azure/elastic-db-tools)) – für Beiträge zum Code
- [Übersicht über elastische Abfragen in Azure SQL-Datenbank](elastic-query-overview.md): Zum Ausführen von elastischen Abfragen.

- [Verschieben von Daten zwischen horizontal skalierten Clouddatenbanken](elastic-scale-overview-split-and-merge.md): Für Anweisungen zur Verwendung des **Tools zum Aufteilen bzw. Zusammenführen**.



<!-- Additional resources H2 -->

[!INCLUDE [elastic-scale-include](../../../includes/elastic-scale-include.md)]


<!--Anchors-->
<!--Image references-->

[1]: ./media/sql-database-elastic-database-client-library/glossary.png

