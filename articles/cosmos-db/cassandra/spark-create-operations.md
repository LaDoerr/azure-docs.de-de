---
title: Erstellen oder Einfügen von Daten in die Cassandra-API für Azure Cosmos DB aus Spark
description: Dieser Artikel erläutert, wie Sie Beispieldaten in Cassandra-API-Tabellen für Azure Cosmos DB einfügen.
author: TheovanKraay
ms.author: thvankra
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: how-to
ms.date: 09/24/2018
ms.openlocfilehash: 1769ef181f977170bf205f4454ebd0254bfbe702
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122345927"
---
# <a name="createinsert-data-into-azure-cosmos-db-cassandra-api-from-spark"></a>Erstellen/Einfügen von Daten in die Cassandra-API für Azure Cosmos DB aus Spark
[!INCLUDE[appliesto-cassandra-api](../includes/appliesto-cassandra-api.md)]
 
Dieser Artikel erläutert, wie Sie Beispieldaten in eine Tabelle in der Cassandra-API für Azure Cosmos DB aus Spark einfügen.

## <a name="cassandra-api-configuration"></a>Konfiguration der Cassandra-API

```scala
import org.apache.spark.sql.cassandra._
//Spark connector
import com.datastax.spark.connector._
import com.datastax.spark.connector.cql.CassandraConnector

//if using Spark 2.x, CosmosDB library for multiple retry
//import com.microsoft.azure.cosmosdb.cassandra

//Connection-related
spark.conf.set("spark.cassandra.connection.host","YOUR_ACCOUNT_NAME.cassandra.cosmosdb.azure.com")
spark.conf.set("spark.cassandra.connection.port","10350")
spark.conf.set("spark.cassandra.connection.ssl.enabled","true")
spark.conf.set("spark.cassandra.auth.username","YOUR_ACCOUNT_NAME")
spark.conf.set("spark.cassandra.auth.password","YOUR_ACCOUNT_KEY")
// if using Spark 2.x
// spark.conf.set("spark.cassandra.connection.factory", "com.microsoft.azure.cosmosdb.cassandra.CosmosDbConnectionFactory")

//Throughput-related...adjust as needed
spark.conf.set("spark.cassandra.output.batch.size.rows", "1")
//spark.conf.set("spark.cassandra.connection.connections_per_executor_max", "10") // Spark 2.x
spark.conf.set("spark.cassandra.connection.remoteConnectionsPerExecutor", "10") // Spark 3.x
spark.conf.set("spark.cassandra.output.concurrent.writes", "1000")
spark.conf.set("spark.cassandra.concurrent.reads", "512")
spark.conf.set("spark.cassandra.output.batch.grouping.buffer.size", "1000")
spark.conf.set("spark.cassandra.connection.keep_alive_ms", "600000000")
```

> [!NOTE]
> Wenn Sie Spark 3.0 oder höher verwenden, müssen Sie die Cosmos DB-Hilfs- und Verbindungs-Factory nicht installieren. Sie sollten auch `remoteConnectionsPerExecutor` anstelle von `connections_per_executor_max` für den Spark 3-Connector verwenden (siehe oben). Sie sehen, dass verbindungsbezogene Eigenschaften im obigen Notebook definiert sind. Mit der folgenden Syntax können so Verbindungseigenschaften definiert werden, ohne diese auf Clusterebene definierem zu müssen (Spark-Kontextinitialisierung).

## <a name="dataframe-api"></a>Datenrahmen-API

### <a name="create-a-dataframe-with-sample-data"></a>Erstellen eines Datenrahmens mit Beispieldaten

```scala
// Generate a dataframe containing five records
val booksDF = Seq(
   ("b00001", "Arthur Conan Doyle", "A study in scarlet", 1887),
   ("b00023", "Arthur Conan Doyle", "A sign of four", 1890),
   ("b01001", "Arthur Conan Doyle", "The adventures of Sherlock Holmes", 1892),
   ("b00501", "Arthur Conan Doyle", "The memoirs of Sherlock Holmes", 1893),
   ("b00300", "Arthur Conan Doyle", "The hounds of Baskerville", 1901)
).toDF("book_id", "book_author", "book_name", "book_pub_year")

//Review schema
booksDF.printSchema

//Print
booksDF.show
```

> [!NOTE]
> Die Funktion „Erstellen, falls nicht vorhanden“ wird auf Zeilenebene noch nicht unterstützt.

### <a name="persist-to-azure-cosmos-db-cassandra-api"></a>Speichern in der Cassandra-API für Azure Cosmos DB

Beim Speichern von Daten können Sie auch Einstellungen für die Gültigkeitsdauer und die Konsistenzrichtlinien festlegen, wie im folgenden Beispiel gezeigt:

```scala
//Persist
booksDF.write
  .mode("append")
  .format("org.apache.spark.sql.cassandra")
  .options(Map( "table" -> "books", "keyspace" -> "books_ks", "output.consistency.level" -> "ALL", "ttl" -> "10000000"))
  .save()
```

> [!NOTE]
> Die Gültigkeitsdauer wird auf Spaltenebene noch nicht unterstützt.

#### <a name="validate-in-cqlsh"></a>Überprüfen in cqlsh

```sql
use books_ks;
select * from books;
```

## <a name="resilient-distributed-database-rdd-api"></a>RDD-API (Resilient Distributed Dataset)

### <a name="create-a-rdd-with-sample-data"></a>Erstellen eines RDD mit Beispieldaten
```scala
//Drop and re-create table to delete records created in the previous section 
val cdbConnector = CassandraConnector(sc)
cdbConnector.withSessionDo(session => session.execute("DROP TABLE IF EXISTS books_ks.books;"))

cdbConnector.withSessionDo(session => session.execute("CREATE TABLE IF NOT EXISTS books_ks.books(book_id TEXT,book_author TEXT, book_name TEXT,book_pub_year INT,book_price FLOAT, PRIMARY KEY(book_id,book_pub_year)) WITH cosmosdb_provisioned_throughput=4000 , WITH default_time_to_live=630720000;"))

//Create RDD
val booksRDD = sc.parallelize(Seq(
   ("b00001", "Arthur Conan Doyle", "A study in scarlet", 1887),
   ("b00023", "Arthur Conan Doyle", "A sign of four", 1890),
   ("b01001", "Arthur Conan Doyle", "The adventures of Sherlock Holmes", 1892),
   ("b00501", "Arthur Conan Doyle", "The memoirs of Sherlock Holmes", 1893),
   ("b00300", "Arthur Conan Doyle", "The hounds of Baskerville", 1901)
))

//Review
booksRDD.take(2).foreach(println)
```

> [!NOTE]
> Die Funktion „Erstellen, falls nicht vorhanden“ wird noch nicht unterstützt.

### <a name="persist-to-azure-cosmos-db-cassandra-api"></a>Speichern in der Cassandra-API für Azure Cosmos DB

Beim Speichern von Daten in der Cassandra-API können Sie auch Einstellungen für die Gültigkeitsdauer und die Konsistenzrichtlinien festlegen, wie im folgenden Beispiel gezeigt:

```scala
import com.datastax.spark.connector.writer._

//Persist
booksRDD.saveToCassandra("books_ks", "books", SomeColumns("book_id", "book_author", "book_name", "book_pub_year"),writeConf = WriteConf(ttl = TTLOption.constant(900000),consistencyLevel = ConsistencyLevel.ALL))
```

#### <a name="validate-in-cqlsh"></a>Überprüfen in cqlsh

```sql
use books_ks;
select * from books;
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Daten in die Cassandra-API-Tabelle für Azure Cosmos DB eingefügt haben, lesen Sie die folgenden Artikel, um andere Vorgänge mit den in der Cassandra-API für Cosmos DB gespeicherten Daten auszuführen:
 
* [Dient zum Lesen von Vorgängen.](spark-read-operation.md)
* [Upsertvorgänge](spark-upsert-operations.md)
* [Löschvorgänge](spark-delete-operation.md)
* [Aggregationsvorgänge](spark-aggregation-operations.md)
* [Tabellenkopiervorgänge an der Azure Cosmos DB-Cassandra-API von Spark aus](spark-table-copy-operations.md)
