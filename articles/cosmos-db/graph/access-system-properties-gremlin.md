---
title: Zugreifen auf Systemdokumenteigenschaften über den Azure Cosmos DB-Graph
description: Hier erfahren Sie, wie Sie Cosmos DB-Systemdokumenteigenschaften über die Gremlin-API lesen und schreiben.
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: how-to
ms.date: 09/10/2019
author: manishmsfte
ms.author: mansha
ms.openlocfilehash: 1ca6b81bc94d0a3a30d5b53b7617e1c4c5dbe421
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122340088"
---
# <a name="system-document-properties"></a>Systemdokumenteigenschaften
[!INCLUDE[appliesto-gremlin-api](../includes/appliesto-gremlin-api.md)]

Azure Cosmos DB verfügt für jedes Dokument über [Systemeigenschaften](/rest/api/cosmos-db/databases), z. B. ```_ts```, ```_self```, ```_attachments```, ```_rid``` und ```_etag```. Darüber hinaus fügt die Gremlin-Engine die Eigenschaften ```inVPartition``` und ```outVPartition``` für Kanten hinzu. Standardmäßig sind diese Eigenschaften für einen Durchlauf verfügbar. Es ist aber möglich, einzelne oder auch alle Eigenschaften in den Gremlin-Durchlauf einzubeziehen.

```
g.withStrategies(ProjectionStrategy.build().IncludeSystemProperties('_ts').create())
```

## <a name="e-tag"></a>ETag

Diese Eigenschaft wird für die Steuerung der optimistischen Nebenläufigkeit genutzt. Wenn eine Anwendung einen Vorgang in mehrere separate Durchläufe unterteilen muss, können mithilfe der Eigenschaft „ETag“ Datenverluste in gleichzeitigen Schreibvorgängen verhindert werden.

```
g.withStrategies(ProjectionStrategy.build().IncludeSystemProperties('_etag').create()).V('1').has('_etag', '"00000100-0000-0800-0000-5d03edac0000"').property('test', '1')
```

## <a name="time-to-live-ttl"></a>Gültigkeitsdauer (TTL)

Wenn für die Sammlung der Dokumentablauf aktiviert ist und für Dokumente die ```ttl```-Eigenschaft festgelegt ist, ist diese Eigenschaft im Gremlin-Durchlauf als reguläre Scheitelpunkt- oder Kanteneigenschaft verfügbar. ```ProjectionStrategy``` ist nicht erforderlich, um die Eigenschaft für die Gültigkeitsdauer verfügbar machen zu können.

Der mit dem unten angegebenen Durchlauf erstellte Scheitelpunkt wird in **123 Sekunden** automatisch gelöscht.

```
g.addV('vertex-one').property('ttl', 123)
```

## <a name="next-steps"></a>Nächste Schritte
* [Wie stellt die SQL-API Parallelität bereit?](../faq.yml#how-does-the-sql-api-provide-concurrency-)
* [Gültigkeitsdauer (TTL)](../time-to-live.md) in Azure Cosmos DB
