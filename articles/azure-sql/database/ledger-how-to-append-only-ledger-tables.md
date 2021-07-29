---
title: Erstellen und Verwenden von Ledgertabellen, die nur Anfügevorgänge unterstützen
description: Erfahren Sie, wie Sie Ledgertabellen, die nur Anfügevorgänge unterstützen, in Azure SQL-Datenbank erstellen und verwenden können.
ms.custom: ''
ms.date: 05/25/2021
ms.service: sql-database
ms.subservice: security
ms.reviewer: vanto
ms.topic: how-to
author: JasonMAnderson
ms.author: janders
ms.openlocfilehash: f433e56140a199cdb872bc733343a8cf88c818cb
ms.sourcegitcommit: 3bb9f8cee51e3b9c711679b460ab7b7363a62e6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2021
ms.locfileid: "112076400"
---
# <a name="create-and-use-append-only-ledger-tables"></a>Erstellen und Verwenden von Ledgertabellen, die nur Anfügevorgänge unterstützen

[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

> [!NOTE]
> Der Azure SQL-Datenbank-Ledger befindet sich derzeit in der öffentlichen Vorschau und ist in der Region „USA, Westen-Mitte“ verfügbar.

In diesem Artikel erfahren Sie, wie Sie [Ledgertabellen, die nur Anfügevorgänge unterstützen](ledger-append-only-ledger-tables.md), in Azure SQL-Datenbank erstellen können. Als Nächstes fügen Sie Werte in die Ledgertabelle, die nur Anfügevorgänge unterstützt, ein. Danach versuchen Sie, die Daten zu aktualisieren. Schließlich zeigen Sie die Ergebnisse unter Verwendung der Ledgeransicht an. Wir verwenden ein Beispiel für ein Kartenschlüsselzugangssystem für eine Einrichtung, das ein Nur-Anfügen-Systemmuster ist. In diesem Beispiel erhalten Sie einen praxisnahen Blick auf die Beziehung zwischen der Ledgertabelle, die nur Anfügevorgänge unterstützt, und der entsprechenden Ledgeransicht.

Weitere Informationen finden Sie unter [Ledgertabellen, die nur Anfügevorgänge unterstützen](ledger-append-only-ledger-tables.md).

## <a name="prerequisites"></a>Voraussetzungen

- Azure SQL-Datenbank mit aktiviertem Ledger. Falls Sie noch keine Datenbank in Azure SQL-Datenbank erstellt haben, finden Sie unter [Schnellstart: Erstellen einer Datenbank in Azure SQL-Datenbank mit aktiviertem Ledger](ledger-create-a-single-database-with-ledger-enabled.md) weitere Informationen.
- [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) oder [Azure Data Studio](/sql/azure-data-studio/download-azure-data-studio).

## <a name="create-an-append-only-ledger-table"></a>Erstellen einer Ledgertabelle, die nur Anfügevorgänge unterstützt

Wir erstellen eine `KeyCardEvents`-Tabelle mit dem folgenden Schema.

| Spaltenname | Datentyp | Beschreibung |
|--|--|--|
| EmployeeID | INT | Die eindeutige ID des Mitarbeiters, der in das Gebäude eintritt |
| AccessOperationDescription | nvarchar (MAX) | Der Zugangsvorgang des Mitarbeiters |
| Timestamp | datetime2 | Das Datum und die Uhrzeit, an dem bzw. zu der der Mitarbeiter auf das Gebäude zugegriffen hat |

> [!IMPORTANT]
> Für das Erstellen von Ledgertabellen, die nur Anfügevorgänge unterstützen, ist die Berechtigung **ENABLE LEDGER** erforderlich. Weitere Informationen zu Berechtigungen im Zusammenhang mit Ledgertabellen finden Sie unter [Berechtigungen](/sql/relational-databases/security/permissions-database-engine#asdbpermissions). 

1. Erstellen Sie in [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) oder [Azure Data Studio](/sql/azure-data-studio/download-azure-data-studio) ein neues Schema und die neue Tabelle namens `[AccessControl].[KeyCardEvents]`.

   ```sql
   CREATE SCHEMA [AccessControl] 
   CREATE TABLE [AccessControl].[KeyCardEvents]
       (
           [EmployeeID] INT NOT NULL,
           [AccessOperationDescription] NVARCHAR (MAX) NOT NULL,
           [Timestamp] Datetime2 NOT NULL
       )
       WITH (
          LEDGER = ON (
                       APPEND_ONLY = ON
                       )
         );
   ```

1. Fügen Sie in der Tabelle `[AccessControl].[KeyCardEvents]` ein neues Gebäudezugangsereignis mit den folgenden Werten hinzu.

   ```sql
   INSERT INTO [AccessControl].[KeyCardEvents]
   VALUES ('43869', 'Building42', '2020-05-02T19:58:47.1234567')
   ```

1. Zeigen Sie den Inhalt Ihrer KeyCardEvents-Tabelle an, und geben Sie die Spalten [GENERATED ALWAYS](/sql/t-sql/statements/create-table-transact-sql#generate-always-columns) an, die Ihrer [Ledgertabelle, die nur Anfügevorgänge unterstützt](ledger-append-only-ledger-tables.md), hinzugefügt werden.

   ```sql
   SELECT *
        ,[ledger_start_transaction_id]
        ,[ledger_start_sequence_number]
   FROM [AccessControl].[KeyCardEvents]
   ```

   :::image type="content" source="media/ledger/append-only-how-to-keycardevent-table.png" alt-text="Screenshot: Ergebnisse der Abfrage der KeyCardEvents-Tabelle.":::

1. Versuchen Sie, die `KeyCardEvents`-Tabelle zu aktualisieren, indem Sie `EmployeeID` von `43869` in `34184.`ändern.

   ```sql
   UPDATE [AccessControl].[KeyCardEvents] SET [EmployeeID] = 34184
   ```

   Sie erhalten eine Fehlermeldung mit dem Hinweis, dass Aktualisierungen für die Ledgertabelle, die nur Anfügevorgänge unterstützt, nicht zulässig sind.

   :::image type="content" source="media/ledger/append-only-how-to-1.png" alt-text="Screenshot mit der Fehlermeldung, dass nur Anfügevorgänge unterstützt werden.":::

## <a name="next-steps"></a>Nächste Schritte

- [Datenbankledger](ledger-database-ledger.md) 
- [Digestverwaltung und Datenbanküberprüfung](ledger-digest-management-and-database-verification.md)
- [Ledgertabellen, die nur Anfügevorgänge unterstützen](ledger-append-only-ledger-tables.md) 
- [Aktualisierbare Ledgertabellen](ledger-updatable-ledger-tables.md)
- [Erstellen und Verwenden aktualisierbarer Ledgertabellen](ledger-how-to-updatable-ledger-tables.md)
- [Zugreifen auf die in Azure Confidential Ledger (ACL) gespeicherten Hashes](ledger-how-to-access-acl-digest.md)
- [Überprüfen einer Ledgertabelle zum Erkennen von Manipulationen](ledger-verify-database.md)
