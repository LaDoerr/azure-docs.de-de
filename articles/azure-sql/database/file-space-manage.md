---
title: Dateispeicherplatzverwaltung in Azure SQL-Datenbank
description: Diese Seite beschreibt, wie Sie in Azure SQL-Datenbank Dateispeicherplatz bei Einzel- und Pooldatenbanken verwalten. Sie enthält Codebeispiele, mit denen Sie ermitteln können, ob eine Einzel- oder Pooldatenbank verkleinert werden muss. Außerdem erhalten Sie hierin die entsprechenden Anweisungen zum Verkleinern der Datenbank.
services: sql-database
ms.service: sql-database
ms.subservice: deployment-configuration
ms.custom: sqldbrb=1, devx-track-azurepowershell
ms.devlang: ''
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: jrasnick, sstein
ms.date: 05/28/2021
ms.openlocfilehash: fb5ee8b096f64faa47756642b4e94bae429fb879
ms.sourcegitcommit: b11257b15f7f16ed01b9a78c471debb81c30f20c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2021
ms.locfileid: "111591258"
---
# <a name="manage-file-space-for-databases-in-azure-sql-database"></a>Verwalten von Dateispeicherplatz für Datenbanken in Azure SQL-Datenbank
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

In diesem Artikel werden verschiedene Arten von Speicherplatz für Datenbanken in Azure SQL-Datenbank sowie die Schritte beschrieben, die ausgeführt werden können, wenn der zugeordnete Speicherplatz explizit verwaltet werden muss.

> [!NOTE]
> Dieser Artikel gilt nicht für verwaltete Azure SQL-Instanzen.

## <a name="overview"></a>Übersicht

Bei Azure SQL-Datenbank gibt es Workloadmuster, bei denen die Zuordnung von zugrunde liegenden Datendateien für Datenbanken größer als die Menge der verwendeten Datenseiten werden kann. Dieser Fall kann eintreten, wenn der Platzbedarf zunimmt und Daten daraufhin gelöscht werden. Der Grund dafür ist, dass der zugeordnete Dateispeicherplatz nicht automatisch wieder freigegeben wird, wenn Daten gelöscht werden.

Die Überwachung der Dateispeicherplatzverwendung und die Verkleinerung von Datendateien können in folgenden Szenarien erforderlich sein:

- Ermöglichen von Datenwachstum in einem Pool für elastische Datenbanken, wenn der den Datenbanken zugeordnete Dateispeicherplatz die maximale Poolgröße erreicht
- Ermöglichen der Verringerung der maximalen Größe einer einzelnen Datenbank oder eines Pools für elastische Datenbanken
- Ermöglichen der Änderung einer einzelnen Datenbank oder eines Pools für elastische Datenbanken, um eine andere Dienstebene oder Leistungsstufe mit einer geringeren maximalen Größe zu verwenden

### <a name="monitoring-file-space-usage"></a>Überwachen der Dateispeicherplatzverwendung

Bei den meisten Speicherplatzmetriken, die in den folgenden APIs angezeigt werden, wird lediglich die Größe der verwendeten Datenseiten ermittelt:

- Azure Resource Manager-basierte Metrik-APIs einschließlich PowerShell [get-metrics](/powershell/module/az.monitor/get-azmetric)
- T-SQL: [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database)

Bei den folgenden APIs wird jedoch auch die Größe des Speicherplatzes ermittelt, der Datenbanken und Pools für elastische Datenbanken zugeordnet ist:

- T-SQL:  [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database)
- T-SQL: [sys.elastic_pool_resource_stats](/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database)

### <a name="shrinking-data-files"></a>Verkleinern von Datendateien

Azure SQL-Datenbank verkleinert Datendateien nicht automatisch, um aufgrund der möglichen Auswirkungen auf die Datenbankleistung ungenutzten zugewiesenen Speicherplatz freizugeben.  Kunden können Datendateien aber jederzeit selbst verkleinern, indem sie die Schritte unter [Freigeben von ungenutztem zugewiesenem Speicherplatz](#reclaim-unused-allocated-space) ausführen.

### <a name="shrinking-transaction-log-file"></a>Verkleinern der Transaktionsprotokolldatei

Im Gegensatz zu Datendateien verkleinert Azure SQL-Datenbank die Transaktionsprotokolldatei automatisch, um eine übermäßige Speicherplatznutzung zu vermeiden, die zu Fehlern führen kann, weil nicht genügend Speicherplatz verfügbar ist. In der Regel müssen Kunden die Transaktionsprotokolldatei nicht verkleinern.

Wenn das Transaktionsprotokoll in den Dienstebenen Premium und Unternehmenskritisch groß wird, kann es erheblich dazu beitragen, dass der Grenzwert für den [maximalen lokalen Speicher](resource-limits-logical-server.md#storage-space-governance) erreicht wird. Wenn sich der lokale Speicherverbrauch dem Grenzwert nähert, können Kunden das Transaktionsprotokoll mithilfe des Befehls [DBCC SHRINKFILE](/sql/t-sql/database-console-commands/dbcc-shrinkfile-transact-sql) verkleinern, wie im folgenden Beispiel gezeigt. Dadurch wird lokaler Speicher freigegeben, nachdem der Befehl abgeschlossen wurde, ohne auf den regelmäßigen automatischen Verkleinerungsvorgang zu warten.

```tsql
DBCC SHRINKFILE (2);
```

## <a name="understanding-types-of-storage-space-for-a-database"></a>Grundlegendes zu den Arten von Speicherplatz für eine Datenbank

Es wichtig, dass Sie mit den folgenden Speicherplatzmengen vertraut sind, damit Sie den Dateispeicherplatz einer Datenbank verwalten können.

|Datenbankmenge|Definition|Kommentare|
|---|---|---|
|**Genutzter Speicherplatz**|Der Speicherplatz, der zum Speichern von Datenbankdaten verwendet wird.|In der Regel erhöht (verringert) sich der Platzbedarf bei eingefügten (gelöschten) Daten. In manchen Fällen ändert sich der genutzte Speicherplatz beim Einfügen oder Löschen von Daten nicht, je nach Menge und Muster der an dem Vorgang beteiligten Daten und einer eventuellen Fragmentierung. Beispielsweise wird der genutzte Speicherplatz durch Löschen einer Zeile auf jeder Datenseite nicht zwangsläufig gesenkt.|
|**Zugeordneter Datenspeicherplatz**|Die Menge an formatiertem Dateispeicherplatz zum Speichern von Daten.|Die Menge des zugeordneten Speicherplatzes wächst automatisch an, wird aber nach dem Löschen nicht kleiner. Dieses Verhalten stellt sicher, dass Daten später schneller eingefügt werden, da der Platz nicht neu formatiert werden muss.|
|**Zugeordneter Datenspeicherplatz (ungenutzt)**|Die Differenz zwischen der Menge des zugeordneten Datenspeicherplatzes und des genutzten Datenspeicherplatzes.|Diese Menge ist die maximale Menge des freien Speicherplatzes, die freigegeben werden kann, indem Datendateien von Datenbanken verkleinert werden.|
|**Maximale Datengröße**|Die maximale Speicherplatzmenge, die zum Speichern von Datenbankdaten verwendet werden kann.|Die Menge des zugeordneten Datenspeicherplatzes kann die maximale Datengröße nicht überschreiten.|

Das folgende Diagramm veranschaulicht die Beziehung zwischen den verschiedenen Arten von Speicherplatz für eine Datenbank.

![Arten von Speicherplatz und Beziehungen](./media/file-space-manage/storage-types.png)

## <a name="query-a-single-database-for-storage-space-information"></a>Abfragen eines Singletons nach Speicherplatzinformationen

Die folgenden Abfragen können verwendet werden, um die Speicherplatzmengen für einen Singleton zu ermitteln.  

### <a name="database-data-space-used"></a>Genutzter Speicherplatz in Datenbank

Ändern Sie die folgende Abfrage, um den belegten Speicherplatz der Datenbank zurückzugeben.  Als Einheit für das Abfrageergebnis wird MB verwendet.

```sql
-- Connect to master
-- Database data space used in MB
SELECT TOP 1 storage_in_megabytes AS DatabaseDataSpaceUsedInMB
FROM sys.resource_stats
WHERE database_name = 'db1'
ORDER BY end_time DESC;
```

### <a name="database-data-space-allocated-and-unused-allocated-space"></a>Zugeordneter Datenbank-Datenspeicherplatz und ungenutzter zugeordneter Speicherplatz

Verwenden Sie die folgende Abfrage, um die Menge des Datenbank-Datenspeicherplatzes und die Menge des zugeordneten ungenutzten Speicherplatzes zurückzugeben.  Als Einheit für das Abfrageergebnis wird MB verwendet.

```sql
-- Connect to database
-- Database data space allocated in MB and database data space allocated unused in MB
SELECT SUM(size/128.0) AS DatabaseDataSpaceAllocatedInMB,
SUM(size/128.0 - CAST(FILEPROPERTY(name, 'SpaceUsed') AS int)/128.0) AS DatabaseDataSpaceAllocatedUnusedInMB
FROM sys.database_files
GROUP BY type_desc
HAVING type_desc = 'ROWS';
```

### <a name="database-data-max-size"></a>Max. Größe für Datenbankdaten

Ändern Sie die folgende Abfrage, um die maximale Größe für Datenbankdaten zurückgegeben.  Als Einheit für das Abfrageergebnis wird Byte verwendet.

```sql
-- Connect to database
-- Database data max size in bytes
SELECT DATABASEPROPERTYEX('db1', 'MaxSizeInBytes') AS DatabaseDataMaxSizeInBytes;
```

## <a name="understanding-types-of-storage-space-for-an-elastic-pool"></a>Grundlegendes zu den Arten von Speicherplatz für einen Pool für elastische Datenbanken

Es wichtig, dass Sie mit den folgenden Speicherplatzmengen vertraut sind, damit Sie den Dateispeicherplatz eines Pools für elastische Datenbanken verwalten können.

|Menge des Pools für elastische Datenbanken|Definition|Kommentare|
|---|---|---|
|**Genutzter Speicherplatz**|Die Summierung des Datenspeicherplatzes, der von allen Datenbanken im Pool für elastische Datenbanken verwendet wird.||
|**Zugeordneter Datenspeicherplatz**|Die Summierung des Datenspeicherplatzes, der von allen Datenbanken im Pool für elastische Datenbanken zugeordnet wird.||
|**Zugeordneter Datenspeicherplatz (ungenutzt)**|Die Differenz zwischen der Menge an zugeordnetem Datenspeicherplatz und dem Datenspeicherplatz, der von allen Datenbanken im Pool für elastische Datenbanken verwendet wird.|Diese Menge gibt die maximale Menge von Speicherplatz an, der für den Pool für elastische Datenbanken zugeordnet wird und freigegeben werden kann, indem die Datenbank-Datendateien verkleinert werden.|
|**Maximale Datengröße**|Die maximale Menge an Datenspeicherplatz, der vom Pool für elastische Datenbanken für alle seine Datenbanken verwendet werden kann.|Der zugeordnete Speicherplatz des Pools für elastische Datenbanken darf die maximale Größe des Pools nicht überschreiten.  In diesem Fall kann zugeordneter Speicherplatz, der nicht genutzt wird, freigegeben werden, indem Datenbank-Datendateien verkleinert werden.|

> [!NOTE]
> Die Fehlermeldung „Der Speichergrenzwert des Pools für elastische Datenbanken wurde erreicht“ ist ein Hinweis darauf, dass den Datenbankobjekten zwar genügend Speicherplatz zur Einhaltung des Speicherplatzlimits für Pools für elastische Datenbanken zugeteilt wurde, aber die Zuteilung ggf. ungenutzten Datenspeicherplatz aufweist. Erwägen Sie, das Speicherplatzlimit des Pools für elastische Datenbanken zu erhöhen. Eine alternative kurzfristige Lösung besteht darin, Speicherplatz für Daten freizugeben, indem Sie die Informationen unten im Abschnitt [**Freigeben von ungenutztem zugewiesenem Speicherplatz**](#reclaim-unused-allocated-space) verwenden. Sie sollten sich auch über die möglichen negativen Auswirkungen auf die Leistung bewusst sein, die sich bei einer Verkleinerung von Datenbankdateien ergeben können. Informationen hierzu finden Sie unten im Abschnitt [**Neuerstellen von Indizes**](#rebuild-indexes).

## <a name="query-an-elastic-pool-for-storage-space-information"></a>Abfragen von Speicherplatzinformationen für einen Pools für elastische Datenbanken

Die folgenden Abfragen können verwendet werden, um die Speicherplatzmengen für einen Pool für elastische Datenbanken zu ermitteln.  

### <a name="elastic-pool-data-space-used"></a>Pool für elastische Datenbanken – ungenutzter Speicherplatz

Ändern Sie die folgende Abfrage, um den belegten Speicherplatz eines Pools für elastische Datenbanken zurückzugeben.  Als Einheit für das Abfrageergebnis wird MB verwendet.

```sql
-- Connect to master
-- Elastic pool data space used in MB  
SELECT TOP 1 avg_storage_percent / 100.0 * elastic_pool_storage_limit_mb AS ElasticPoolDataSpaceUsedInMB
FROM sys.elastic_pool_resource_stats
WHERE elastic_pool_name = 'ep1'
ORDER BY end_time DESC;
```

### <a name="elastic-pool-data-space-allocated-and-unused-allocated-space"></a>Zugeordneter Datenspeicherplatz für Pool für elastische Datenbanken und ungenutzter zugeordneter Speicherplatz

Ändern Sie die folgenden Beispiele, um eine Tabelle zurückzugeben, in der der zugeordnete Speicherplatz und der ungenutzte zugeordnete Speicherplatz für jede Datenbank eines Pools für elastische Datenbanken aufgeführt ist. Die Datenbanken in der Tabelle werden absteigend sortiert – von der größten Menge an ungenutztem zugeordnetem Speicherplatz zur geringsten Menge an ungenutztem zugeordnetem Speicherplatz.  Als Einheit für das Abfrageergebnis wird MB verwendet.  

Die Abfrageergebnisse zum Bestimmen des für jede Datenbank im Pool zugeordneten Speicherplatzes können addiert werden, um den gesamten Speicherplatz zu ermitteln, der für den Pool für elastische Datenbanken zugeordnet ist. Der zugewiesene Speicherplatz des Pools für elastische Datenbanken darf die maximale Größe des Pools nicht überschreiten.  

> [!IMPORTANT]
> Das PowerShell Azure Resource Manager-Modul wird von Azure SQL-Datenbank weiterhin unterstützt, aber alle zukünftigen Entwicklungen erfolgen für das Az.Sql-Modul. Das AzureRM-Modul erhält mindestens bis Dezember 2020 weiterhin Fehlerbehebungen. Die Argumente für die Befehle im Az-Modul und den AzureRm-Modulen sind im Wesentlichen identisch. Weitere Informationen zur Kompatibilität finden Sie in der [Einführung in das neue Azure PowerShell Az-Modul](/powershell/azure/new-azureps-module-az).

Für die Installation des PowerShell-Skripts ist das SQL Server PowerShell-Modul erforderlich – Siehe [Herunterladen des PowerShell-Moduls](/sql/powershell/download-sql-server-ps-module).

```powershell
$resourceGroupName = "<resourceGroupName>"
$serverName = "<serverName>"
$poolName = "<poolName>"
$userName = "<userName>"
$password = "<password>"

# get list of databases in elastic pool
$databasesInPool = Get-AzSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName `
    -ServerName $serverName -ElasticPoolName $poolName
$databaseStorageMetrics = @()

# for each database in the elastic pool, get space allocated in MB and space allocated unused in MB
foreach ($database in $databasesInPool) {
    $sqlCommand = "SELECT DB_NAME() as DatabaseName, `
    SUM(size/128.0) AS DatabaseDataSpaceAllocatedInMB, `
    SUM(size/128.0 - CAST(FILEPROPERTY(name, 'SpaceUsed') AS int)/128.0) AS DatabaseDataSpaceAllocatedUnusedInMB `
    FROM sys.database_files `
    GROUP BY type_desc `
    HAVING type_desc = 'ROWS'"
    $serverFqdn = "tcp:" + $serverName + ".database.windows.net,1433"
    $databaseStorageMetrics = $databaseStorageMetrics + 
        (Invoke-Sqlcmd -ServerInstance $serverFqdn -Database $database.DatabaseName `
            -Username $userName -Password $password -Query $sqlCommand)
}

# display databases in descending order of space allocated unused
Write-Output "`n" "ElasticPoolName: $poolName"
Write-Output $databaseStorageMetrics | Sort -Property DatabaseDataSpaceAllocatedUnusedInMB -Descending | Format-Table
```

Der folgende Screenshot zeigt ein Beispiel für die Ausgabe des Skripts:

![Pool für elastische Datenbanken – zugeordneter Speicherplatz und ungenutzter zugewiesener Speicherplatz – Beispiel](./media/file-space-manage/elastic-pool-allocated-unused.png)

### <a name="elastic-pool-data-max-size"></a>Maximale Größe der Daten des Pools für elastische Datenbanken

Ändern Sie die folgende T-SQL-Abfrage, um die zuletzt aufgezeichnete maximale Größe der Daten des Pools für elastische Datenbanken zurückzugeben.  Als Einheit für das Abfrageergebnis wird MB verwendet.

```sql
-- Connect to master
-- Elastic pools max size in MB
SELECT TOP 1 elastic_pool_storage_limit_mb AS ElasticPoolMaxSizeInMB
FROM sys.elastic_pool_resource_stats
WHERE elastic_pool_name = 'ep1'
ORDER BY end_time DESC;
```

## <a name="reclaim-unused-allocated-space"></a>Freigeben von ungenutztem zugewiesenem Speicherplatz

> [!NOTE]
> Verkleinerungsbefehle können während der Ausführung der Datenbank zu einer Beeinträchtigung der Leistung führen und sollten daher nur zu Zeiten mit geringer Auslastung ausgeführt werden.

### <a name="dbcc-shrink"></a>DBCC-Verkleinerung

Nachdem die Datenbanken für das Freigeben von ungenutztem zugeordnetem Speicherplatz ermittelt wurden, können Sie den Namen der Datenbank im folgenden Befehl ändern, um die Datendateien für die einzelnen Datenbanken zu verkleinern.

```sql
-- Shrink database data space allocated.
DBCC SHRINKDATABASE (N'db1');
```

Verkleinerungsbefehle können während der Ausführung der Datenbank zu einer Beeinträchtigung der Leistung führen und sollten daher nur zu Zeiten mit geringer Auslastung ausgeführt werden.  

Sie sollten sich auch über die möglichen negativen Auswirkungen auf die Leistung bewusst sein, die sich bei einer Verkleinerung von Datenbankdateien ergeben können. Informationen hierzu finden Sie unten im Abschnitt [**Neuerstellen von Indizes**](#rebuild-indexes).

Weitere Informationen zu diesem Befehl finden Sie unter [SHRINKDATABASE](/sql/t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql).

### <a name="auto-shrink"></a>Automatisches Verkleinern

Alternativ kann für die Datenbank auch das automatische Verkleinern aktiviert werden.  Automatisches Verkleinern vereinfacht die Dateiverwaltung und wirkt sich im Vergleich zu `SHRINKDATABASE` oder `SHRINKFILE` weniger stark auf die Datenbankleistung aus. Das automatische Verkleinern kann besonders bei der Verwaltung von Pools für elastische Datenbanken mit vielen Datenbanken hilfreich sein, bei denen ein erhebliches Wachstum und eine Verringerung des belegten Speicherplatzes zu verzeichnen ist. Verglichen mit `SHRINKDATABASE` und `SHRINKFILE` ist das automatische Verkleinern allerdings beim Freigeben von Dateispeicherplatz unter Umständen weniger effizient.

Das automatische Verkleinern ist standardmäßig deaktiviert. Dies ist für die meisten Datenbanken die empfohlene Einstellung. Wenn das automatische Verkleinern aktiviert werden muss, wird empfohlen, das automatische Verkleinern wieder zu deaktivieren, sobald die Ziele der Speicherplatzverwaltung erreicht wurden, anstatt das automatische Verkleinern dauerhaft aktiviert zu lassen. Weitere Informationen finden Sie im Abschnitt unter [Überlegungen zu AUTO_SHRINK](/troubleshoot/sql/admin/considerations-autogrow-autoshrink#considerations-for-auto_shrink).

Führen Sie den folgenden Befehl in Ihrer Datenbank aus (nicht in der Masterdatenbank), um das automatische Verkleinern zu aktivieren.

```sql
-- Enable auto-shrink for the current database.
ALTER DATABASE CURRENT SET AUTO_SHRINK ON;
```

Weitere Informationen zu diesem Befehl finden Sie in den Optionen für [DATABASE SET](/sql/t-sql/statements/alter-database-transact-sql-set-options).

### <a name="rebuild-indexes"></a>Neuerstellen von Indizes

Nach dem Verkleinern von Datendateien sind Indizes möglicherweise fragmentiert und verlieren ihre leistungsoptimierende Wirkung. Im Falle einer Leistungsbeeinträchtigung empfiehlt es sich daher, die Datenbankindizes neu zu erstellen. Weitere Informationen zur Fragmentierung und Indexwartung finden Sie unter [Optimale Wartung von Indizes zum Verbessern der Leistung und Verringern der Ressourcenauslastung](/sql/relational-databases/indexes/reorganize-and-rebuild-indexes).

## <a name="next-steps"></a>Nächste Schritte

- Informationen zur maximalen Datenbankgröße finden Sie unter:
  - [Limits des vCore-basierten Kaufmodells für eine Einzeldatenbank in Azure SQL-Datenbank](resource-limits-vcore-single-databases.md)
  - [Ressourcenlimits für Einzeldatenbanken, die DTU-basierte Kaufmodell verwenden](resource-limits-dtu-single-databases.md)
  - [Limits des vCore-basierten Kaufmodells für Pools für elastische Datenbanken in Azure SQL-Datenbank](resource-limits-vcore-elastic-pools.md)
  - [Grenzwerte für Ressourcen für Pools für elastische Datenbanken, die das DTU-basierte Kaufmodell verwenden](resource-limits-dtu-elastic-pools.md)
- Weitere Informationen zum `SHRINKDATABASE`-Befehl finden Sie unter [SHRINKDATABASE](/sql/t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql).
- Weitere Informationen zum Fragmentieren und Neuerstellen von Indizes finden Sie unter [Neuorganisieren und Neuerstellen von Indizes](/sql/relational-databases/indexes/reorganize-and-rebuild-indexes).
