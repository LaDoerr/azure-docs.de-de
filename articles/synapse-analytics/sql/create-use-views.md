---
title: Erstellen und Verwenden von Ansichten in einem serverlosen SQL-Pool
description: In diesem Abschnitt erfahren Sie, wie Sie Ansichten erstellen und zum Umschließen von Abfragen für einen serverlosen SQL-Pool verwenden. Ansichten ermöglichen Ihnen die erneute Verwendung dieser Abfragen. Ansichten sind ebenfalls erforderlich, wenn Sie Tools wie Power BI zusammen mit einem serverlosen SQL-Pool verwenden möchten.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: sql
ms.date: 05/20/2020
ms.author: stefanazaric
ms.reviewer: jrasnick
ms.openlocfilehash: da026012c4084783d30f548cbdffc8951d74bcd6
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111751215"
---
# <a name="create-and-use-views-using-serverless-sql-pool-in-azure-synapse-analytics"></a>Erstellen und Verwenden von Ansichten mit einem serverlosen SQL-Pool in Azure Synapse Analytics

In diesem Abschnitt erfahren Sie, wie Sie Ansichten erstellen und zum Umschließen von Abfragen für einen serverlosen SQL-Pool verwenden. Ansichten ermöglichen Ihnen die erneute Verwendung dieser Abfragen. Ansichten sind ebenfalls erforderlich, wenn Sie Tools wie Power BI zusammen mit einem serverlosen SQL-Pool verwenden möchten.

## <a name="prerequisites"></a>Voraussetzungen

Im ersten Schritt erstellen Sie eine Datenbank, in der die Ansicht erstellt werden soll, und Sie initialisieren die zum Authentifizieren beim Azure-Speicher erforderlichen Objekte, indem Sie das [Setupskript](https://github.com/Azure-Samples/Synapse/blob/master/SQL/Samples/LdwSample/SampleDB.sql) für diese Datenbank ausführen. Alle Abfragen in diesem Artikel werden in Ihrer Beispieldatenbank ausgeführt.

## <a name="views-over-external-data"></a>Sichten externer Daten

Ansichten können auf die gleiche Weise wie reguläre SQL Server-Ansichten erstellt werden. Mit der folgenden Abfrage wird eine Ansicht erstellt, die die Datei *population.csv* liest.

> [!NOTE]
> Ändern Sie die erste Zeile in der Abfrage (d. h. [mydbname]), sodass die von Ihnen erstellte Datenbank verwendet wird.

```sql
USE [mydbname];
GO

DROP VIEW IF EXISTS populationView;
GO

CREATE VIEW populationView AS
SELECT * 
FROM OPENROWSET(
        BULK 'csv/population/population.csv',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT = 'CSV', 
        FIELDTERMINATOR =',', 
        ROWTERMINATOR = '\n'
    )
WITH (
    [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
    [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2,
    [year] smallint,
    [population] bigint
) AS [r];
```

In der Ansicht wird `EXTERNAL DATA SOURCE` mit einer Stamm-URL Ihres Speichers als `DATA_SOURCE` verwendet und den Dateien ein neuer relativer Dateipfad hinzugefügt.

### <a name="delta-lake-views"></a>Delta Lake-Sichten

Wenn Sie die Sichten für den Ordner „Delta Lake“ erstellen, müssen Sie nach der Option `BULK` anstelle des Dateipfads den Speicherort im Stammordner angeben.

> [!div class="mx-imgBorder"]
>![Delta Lake-Ordner „ECDC COVID-19“](./media/shared/covid-delta-lake-studio.png)

Die `OPENROWSET`-Funktion, die Daten aus dem Delta Lake-Ordner liest, untersucht die Ordnerstruktur und bestimmt automatisch die Dateispeicherorte.

```sql
create or alter view CovidDeltaLake
as
select *
from openrowset(
           bulk 'covid',
           data_source = 'DeltaLakeStorage',
           format = 'delta'
    ) with (
           date_rep date,
           cases int,
           geo_id varchar(6)
           ) as rows
```

## <a name="partitioned-views"></a>Partitionierte Sichten

Wenn Sie über Dateien verfügen, die in der hierarchischen Ordnerstruktur partitioniert sind, können Sie das Partitionsmuster mithilfe der Platzhalter im Dateipfad beschreiben. Verwenden Sie die Funktion `FILEPATH`, um Teile des Ordnerpfads als Partitionierungsspalten verfügbar zu machen.

```sql
CREATE VIEW TaxiView
AS SELECT *, nyc.filepath(1) AS [year], nyc.filepath(2) AS [month]
FROM
    OPENROWSET(
        BULK 'parquet/taxi/year=*/month=*/*.parquet',
        DATA_SOURCE = 'sqlondemanddemo',
        FORMAT='PARQUET'
    ) AS nyc
```

Die partitionierten Ansichten führen die Löschung der Ordnerpartition durch, wenn Sie diese Ansicht mit den Filtern für die Partitionierungsspalten abfragen. Dadurch kann die Leistung Ihrer Abfragen verbessert werden.

### <a name="delta-lake-partitioned-views"></a>Partitionierte Delta Lake-Sichten

Wenn Sie die partitionierten Sichten für Delta Lake-Speicher erstellen, können Sie nur einen Delta Lake-Stammordner angeben und müssen die Spalten für die Partitionierung nicht explizit mit der Funktion `FILEPATH` verfügbar machen:

```sql
CREATE OR ALTER VIEW YellowTaxiView
AS SELECT *
FROM  
    OPENROWSET(
        BULK 'yellow',
        DATA_SOURCE = 'DeltaLakeStorage',
        FORMAT='DELTA'
    ) nyc
```

Die `OPENROWSET`-Funktion untersucht die Struktur des zugrunde liegenden Delta Lake-Ordners, ermittelt automatisch die Spalten für die Partitionierung und macht sie verfügbar. Die Partitionslöschung erfolgt automatisch, wenn Sie in der `WHERE`-Klausel einer Abfrage die Partitionierungsspalte angeben.

Der Ordnername in der `OPENROWSET`-Funktion (`yellow` in diesem Beispiel), der mit dem in der Datenquelle `DeltaLakeStorage` definierten URI `LOCATION` verkettet ist, muss auf den Delta Lake-Stammordner verweisen, der einen Unterordner namens `_delta_log` enthält.

> [!div class="mx-imgBorder"]
>![Delta Lake-Ordner „Yellow Taxi“](./media/shared/yellow-taxi-delta-lake.png)

Verwenden Sie die `WITH`-Klausel nicht in der Funktion `OPENROWSET`, wenn Sie partitionierte Delta Lake-Daten abfragen. Aufgrund eines bekannten Problems in der Vorschauversion werden von der `WITH`-Klausel die Werte aus den zugrunde liegenden Partitionierungsspalten nicht richtig zurückgegeben. Die Partitionsentfernung funktioniert problemlos, wenn Sie die Funktion `OPENROWSET` direkt mit der `WITH`-Klausel (ohne Sichten) verwenden.  

## <a name="use-a-view"></a>Verwenden einer Ansicht

Ansichten können in Ihren Abfragen auf die gleiche Weise verwendet werden wie in SQL Server-Abfragen.

Die folgende Abfrage veranschaulicht die Verwendung der Ansicht *population_csv*, die Sie unter [Erstellen einer Ansicht](#views-over-external-data) erstellt haben. Sie gibt Länder-/Regionsnamen mit der entsprechenden Bevölkerung im Jahr 2019 in absteigender Reihenfolge zurück.

> [!NOTE]
> Ändern Sie die erste Zeile in der Abfrage (d. h. [mydbname]), sodass die von Ihnen erstellte Datenbank verwendet wird.

```sql
USE [mydbname];
GO

SELECT
    country_name, population
FROM populationView
WHERE
    [year] = 2019
ORDER BY
    [population] DESC;
```

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Abfragen verschiedener Dateitypen finden Sie in den Artikeln zum [Abfragen einer einzelnen CSV-Datei](query-single-csv-file.md), [Abfragen von Parquet-Dateien](query-parquet-files.md) und [Abfragen von JSON-Dateien](query-json-files.md).
