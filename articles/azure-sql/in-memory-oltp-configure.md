---
title: In-Memory OLTP verbessert SQL-Transaktionsleistung
description: Verwenden von In-Memory OLTP zum Verbessern der Transaktionsleistung in einer vorhandenen Datenbank in Azure SQL-Datenbank und Azure SQL Managed Instance.
services: sql-database
ms.service: sql-database
ms.custom: sqldbrb=2
ms.subservice: performance
ms.topic: how-to
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.reviewer: mathoma
ms.date: 11/07/2018
ms.openlocfilehash: 2d1e00059948b6b3347c41910f8c1f75d9635da5
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122349796"
---
# <a name="use-in-memory-oltp-to-improve-your-application-performance-in-azure-sql-database-and-azure-sql-managed-instance"></a>Verwenden von In-Memory-OLTP zur Verbesserung der Anwendungsleistung in Azure SQL-Datenbank und Azure SQL Managed Instance
[!INCLUDE[appliesto-sqldb-sqlmi](includes/appliesto-sqldb-sqlmi.md)]

[In-Memory-OLTP](in-memory-oltp-overview.md) kann verwendet werden, um die Leistung der Transaktionsverarbeitung, der Datenerfassung und der vorübergehenden Datenszenarien in Datenbanken des Tarifs [„Premium“ und „Unternehmenskritisch“](database/service-tiers-vcore.md) zu verbessern, ohne den Tarif zu erhöhen.

> [!NOTE]
> Erfahren Sie mehr darüber, wie ein [Quorum die Workload der wichtigen Datenbanken verdoppelt, während die DTU mit Azure SQL-Datenbank um 70 % verringert wird](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database).

Führen Sie diese Schritte durch, um In-Memory-OLTP in Ihrer vorhandenen Datenbank zu übernehmen.

## <a name="step-1-ensure-you-are-using-a-premium-and-business-critical-tier-database"></a>Schritt 1: Sicherstellen, dass eine Datenbank mit dem Tarif „Premium“ und „Unternehmenskritisch“ verwendet wird

In-Memory-OLTP wird nur für Datenbanken mit dem Tarif „Premium“ und „Unternehmenskritisch“ unterstützt. In-Memory wird unterstützt, wenn das zurückgegebene Ergebnis 1 ist (nicht 0):

```sql
SELECT DatabasePropertyEx(Db_Name(), 'IsXTPSupported');
```

*XTP* steht für *Extreme Transaction Processing*.

## <a name="step-2-identify-objects-to-migrate-to-in-memory-oltp"></a>Schritt 2: Identifizieren der zu In-Memory OLTP zu migrierenden Objekte

SSMS umfasst einen Bericht **Übersicht der Transaktionsleistungsanalyse** , den Sie für eine Datenbank mit einer aktiven Workload erstellen können. Der Bericht identifiziert Tabellen und gespeicherte Prozeduren, die für die Migration zu In-Memory OLTP geeignet sind.

So generieren Sie den Bericht in SSMS

* Klicken Sie im **Objekt-Explorer** mit der rechten Maustaste auf den Datenbankknoten.
* Klicken Sie auf **Berichte** > **Standardberichte** > **Übersicht der Transaktionsleistungsanalyse**.

Weitere Informationen finden Sie unter [Bestimmen, ob eine Tabelle oder eine gespeicherte Prozedur zu In-Memory OLTP portiert werden soll](/sql/relational-databases/in-memory-oltp/determining-if-a-table-or-stored-procedure-should-be-ported-to-in-memory-oltp).

## <a name="step-3-create-a-comparable-test-database"></a>Schritt 3: Erstellen einer vergleichbaren Testdatenbank

Nehmen Sie an, der Bericht zeigt, dass die Datenbank über eine Tabelle verfügt, für die eine Umwandlung in eine speicheroptimierte Tabelle von Vorteil wäre. Sie sollten diese Angabe zunächst zur Bestätigung testen.

Sie benötigen eine Testkopie der Produktionsdatenbank. Die Testdatenbank muss sich auf der gleichen Dienstebene befinden wie die Produktionsdatenbank.

Um das Testen zu erleichtern, optimieren Sie die Testdatenbank wie folgt:

1. Stellen Sie über SSMS eine Verbindung mit der Testdatenbank her.
2. Um zu vermeiden, dass die Option WITH (SNAPSHOT) in Abfragen benötigt wird, legen Sie die Datenbankoption wie in der folgenden T-SQL-Anweisung dargestellt fest:

   ```sql
   ALTER DATABASE CURRENT
    SET
        MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT = ON;
   ```

## <a name="step-4-migrate-tables"></a>Schritt 4: Migrieren von Tabellen

Sie müssen eine speicheroptimierte Kopie der Tabelle, die Sie testen möchten, erstellen und auffüllen. Zum Erstellen können Sie eine der folgenden Möglichkeiten wählen:

* Den praktischen Speicheroptimierungs-Assistenten in SSMS.
* Manuelles T-SQL.

### <a name="memory-optimization-wizard-in-ssms"></a>Speicheroptimierungs-Assistent in SSMS

So verwenden Sie diese Migrationsoption:

1. Stellen Sie über SSMS eine Verbindung mit der Testdatenbank her.
2. Klicken Sie im **Objekt-Explorer** mit der rechten Maustaste auf die Tabelle, und klicken Sie dann auf **Advisor für die Speicheroptimierung**.

   Der Assistent für den **Ratgeber für die Tabellenspeicheroptimierung** wird angezeigt.
3. Klicken Sie im Assistenten auf **Migrationsüberprüfung** (oder die Schaltfläche **Weiter**), um zu überprüfen, ob die Tabelle nicht unterstützte Features enthält, die in den speicheroptimierten Tabellen nicht unterstützt werden. Weitere Informationen finden Sie unter

   * Die *Prüfliste für die Speicheroptimierung* im [Advisor für die Speicheroptimierung](/sql/relational-databases/in-memory-oltp/memory-optimization-advisor).
   * [Von In-Memory OLTP nicht unterstützte Transact-SQL-Konstrukte](/sql/relational-databases/in-memory-oltp/transact-sql-constructs-not-supported-by-in-memory-oltp).
   * [Migrieren zu In-Memory OLTP](/sql/relational-databases/in-memory-oltp/plan-your-adoption-of-in-memory-oltp-features-in-sql-server).
4. Wenn die Tabelle nicht unterstützte Features enthält, kann der Ratgeber das aktuelle Schema und die Datenmigration für Sie ausführen.

### <a name="manual-t-sql"></a>Manuelles T-SQL

So verwenden Sie diese Migrationsoption:

1. Verbinden Sie sich mit der Testdatenbank mit SSMS (oder einem ähnlichen Dienstprogramm).
2. Rufen Sie das vollständige T-SQL-Skript für die Tabelle und ihre Indizes ab.

   * Klicken Sie in SSMS mit der rechten Maustaste auf Ihren Tabellenknoten.
   * Klicken Sie auf **Skript für Tabelle als** > **CREATE in** > **Fenster 'Neue Abfrage'** .
3. Fügen Sie im Skriptfenster WITH (MEMORY_OPTIMIZED = ON) der CREATE TABLE-Anweisung hinzu.
4. Wenn ein Index als CLUSTERED gekennzeichnet ist, ändern Sie ihn in NONCLUSTERED.
5. Benennen Sie die vorhandene Tabelle mittels SP_RENAME um.
6. Erstellen Sie die neue speicheroptimierte Kopie der Tabelle, indem Sie das bearbeitete CREATE TABLE-Skript ausführen.
7. Kopieren Sie die Daten mit INSERT...SELECT * INTO in Ihre speicheroptimierte Tabelle:

```sql
INSERT INTO [<new_memory_optimized_table>]
        SELECT * FROM [<old_disk_based_table>];
```

## <a name="step-5-optional-migrate-stored-procedures"></a>Schritt 5 (optional): Migrieren gespeicherter Prozeduren

Das In-Memory-Feature kann auch eine gespeicherte Prozedur zur Verbesserung der Leistung ändern.

### <a name="considerations-with-natively-compiled-stored-procedures"></a>Aspekte bei systemeigen kompilierten gespeicherten Prozeduren

Für die T-SQL-WITH-Klausel einer systemeigen kompilierten gespeicherten Prozedur sind folgende Optionen erforderlich:

* NATIVE_COMPILATION
* SCHEMABINDING: Bezieht sich auf Tabellen, deren Spaltendefinitionen nicht in einer Weise geändert werden dürfen, die sich auf die gespeicherte Prozedur auswirkt, sofern Sie die gespeicherte Prozedur nicht löschen.

Ein systemeigenes Modul muss einen großen [ATOMIC-Block](/sql/relational-databases/in-memory-oltp/atomic-blocks-in-native-procedures) zum Verwalten von Transaktionen verwenden. Es gibt keine Rolle für ein explizites BEGIN TRANSACTION oder ROLLBACK TRANSACTION. Wenn Ihr Code einen Verstoß gegen eine Geschäftsregel erkennt, kann er den atomischen Block mit einer [Throw](/sql/t-sql/language-elements/throw-transact-sql) -Anweisung beenden.

### <a name="typical-create-procedure-for-natively-compiled"></a>In der Regel CREATE PROCEDURE für systemeigen kompilierte gespeicherte Prozeduren verwenden

In der Regel entspricht der T-SQL-Code zum Erstellen einer systemeigen kompilierten gespeicherten Prozedur folgender Vorlage:

```sql
CREATE PROCEDURE schemaname.procedurename
    @param1 type1, …
    WITH NATIVE_COMPILATION, SCHEMABINDING
    AS
        BEGIN ATOMIC WITH
            (TRANSACTION ISOLATION LEVEL = SNAPSHOT,
            LANGUAGE = N'your_language__see_sys.languages'
            )
        …
        END;
```

* Für TRANSACTION_ISOLATION_LEVEL ist SNAPSHOT der häufigste Wert für die systemeigen kompilierte gespeicherte Prozedur. Eine Teilmenge der anderen Werte wird jedoch ebenfalls unterstützt:
  
  * REPEATABLE READ
  * SERIALIZABLE
* Der Wert LANGUAGE muss in der Ansicht „sys.languages“ vorhanden sein.

### <a name="how-to-migrate-a-stored-procedure"></a>Migrieren einer gespeicherten Prozedur

Die Migrationsschritte sind wie folgt:

1. Rufen Sie das CREATE PROCEDURE-Skript für die regulär übersetzte gespeicherte Prozedur auf.
2. Schreiben Sie ihren Header entsprechend der vorherigen Vorlage neu.
3. Prüfen Sie, ob der T-SQL-Code der gespeicherten Prozedur Features verwendet, die für systemeigen kompilierte gespeicherte Prozeduren nicht unterstützt werden. Implementieren Sie ggf. Problemumgehungen.

   Weitere Informationen finden Sie unter [Migrationsprobleme bei systemeigen kompilierten gespeicherten Prozeduren](/sql/relational-databases/in-memory-oltp/a-guide-to-query-processing-for-memory-optimized-tables).
4. Benennen Sie die alte gespeicherte Prozedur mithilfe von SP_RENAME um. Oder löschen Sie sie einfach (DROP).
5. Führen Sie das bearbeitete CREATE PROCEDURE T-SQL-Skript aus.

## <a name="step-6-run-your-workload-in-test"></a>Schritt 6: Ausführen der Workload im Test

Führen Sie eine Workload in der Testdatenbank aus, die der Workload ähnelt, die in der Produktionsdatenbank ausgeführt wird. Dies sollte den Leistungsgewinn zeigen, den Sie mit der Verwendung des In-Memory-Features für Tabellen und gespeicherte Prozeduren erzielen.

Wichtige Attribute der Workload sind:

* Anzahl gleichzeitiger Verbindungen.
* Lese-/Schreib-Verhältnis.

Zum Anpassen und Ausführen der Testworkload empfiehlt sich das praktische Tool `ostress.exe`, das in diesem Artikel zu [In-Memory-OLTP](in-memory-oltp-overview.md) erläutert wird.

Um die Netzwerklatenz zu minimieren, führen Sie den Test in der gleichen geografischen Azure-Region aus, in der die Datenbank vorhanden ist.

## <a name="step-7-post-implementation-monitoring"></a>Schritt 7: Überwachen nach der Implementierung

Sie sollten die Leistungseffekte Ihrer In-Memory-Implementierungen in der Produktion überwachen:

* [Überwachen Sie die In-Memory-Speicherung](in-memory-oltp-monitor-space.md).
* [Überwachen Sie die Verwendung dynamischer Verwaltungssichten](database/monitoring-with-dmvs.md).

## <a name="related-links"></a>Verwandte Links

* [In-Memory OLTP (In-Memory Optimization)](/sql/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization)
* [Einführung zu systemeigen kompilierten gespeicherten Prozeduren](/sql/relational-databases/in-memory-oltp/a-guide-to-query-processing-for-memory-optimized-tables)
* [Advisor für die Speicheroptimierung](/sql/relational-databases/in-memory-oltp/memory-optimization-advisor)
