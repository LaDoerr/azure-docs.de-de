---
title: Features und Funktionen von Azure Arc-fähigen SQL Managed Instance-Instanzen
description: Features und Funktionen von Azure Arc-fähigen SQL Managed Instance-Instanzen
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: dnethi
ms.author: dinethi
ms.reviewer: mikeray
ms.date: 07/30/2021
ms.topic: how-to
ms.openlocfilehash: ef855102f4877d26c1b6d16d73e99719e3e97ed1
ms.sourcegitcommit: 6c6b8ba688a7cc699b68615c92adb550fbd0610f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122356112"
---
# <a name="features-and-capabilities-of-azure-arc-enabled-sql-managed-instance"></a>Features und Funktionen von Azure Arc-fähigen SQL Managed Instance-Instanzen

Azure Arc-fähige SQL Managed Instance-Instanzen nutzen eine gemeinsame Codebasis mit der neuesten stabilen Version von SQL Server. Die meisten Standardfeatures für SQL-Sprache, Abfrageverarbeitung und Datenbankverwaltung sind identisch. Zu den Features, die in SQL Server und SQL-Datenbank oder SQL Managed Instance häufig vorkommen, gehören:

- Sprachfunktionen: [Ablaufsteuerungs-Schlüsselwörter](/sql/t-sql/language-elements/control-of-flow), [Cursor](/sql/t-sql/language-elements/cursors-transact-sql), [Datentypen](/sql/t-sql/data-types/data-types-transact-sql), [DML-Anweisungen](/sql/t-sql/queries/queries), [Prädikate](/sql/t-sql/queries/predicates), [Sequenznummern](/sql/relational-databases/sequence-numbers/sequence-numbers), [Gespeicherte Prozeduren](/sql/relational-databases/stored-procedures/stored-procedures-database-engine) und [Variablen](/sql/t-sql/language-elements/variables-transact-sql).
- Datenbankfunktionen: [Automatische Optimierung (Planerzwingung)](/sql/relational-databases/automatic-tuning/automatic-tuning), [Änderungsnachverfolgung](/sql/relational-databases/track-changes/about-change-tracking-sql-server), [Datenbanksortierung](/sql/relational-databases/collations/set-or-change-the-database-collation), [Eigenständige Datenbanken](/sql/relational-databases/databases/contained-databases), [Eigenständige Benutzer](/sql/relational-databases/security/contained-database-users-making-your-database-portable), [Datenkomprimierung](/sql/relational-databases/data-compression/data-compression), [Datenbankkonfigurationseinstellungen](/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql), [Onlineindexvorgänge](/sql/relational-databases/indexes/perform-index-operations-online), [Partitionierung](/sql/relational-databases/partitions/partitioned-tables-and-indexes) und [Temporäre Tabellen](/sql/relational-databases/tables/temporal-tables) ([weitere Informationen im Leitfaden zu den ersten Schritten](/sql/relational-databases/tables/getting-started-with-system-versioned-temporal-tables)).
- Sicherheitsfunktionen: [Anwendungsrollen](/sql/relational-databases/security/authentication-access/application-roles), [Dynamische Datenmaskierung](/sql/relational-databases/security/dynamic-data-masking) ([Erste Schritte mit der dynamischen Datenmaskierung für SQL-Datenbank im Azure-Portal](../../azure-sql/database/dynamic-data-masking-configure-portal.md)), [Sicherheit auf Zeilenebene](/sql/relational-databases/security/row-level-security)
- Funktionen für mehrere Modelle: [Graph-Verarbeitung](/sql/relational-databases/graphs/sql-graph-overview), [JSON-Daten](/sql/relational-databases/json/json-data-sql-server), [OPENXML](/sql/t-sql/functions/openxml-transact-sql), [Spatial](/sql/relational-databases/spatial/spatial-data-sql-server), [OPENJSON](/sql/t-sql/functions/openjson-transact-sql) und [XML-Indizes](/sql/t-sql/statements/create-xml-index-transact-sql)


## <a name="rdbms-high-availability"></a><a name="RDBMSHA"></a> RDBMS High Availability  
  
|Funktion|SQL Managed Instance mit Azure Arc-Unterstützung|
|-------------|----------------|
|Always On-Failoverclusterinstanzen<sup>1</sup>| Nicht zutreffend Ähnliche Funktionen verfügbar.|
|AlwaysOn-Verfügbarkeitsgruppen<sup>2</sup>|Dienstebene „Unternehmenskritisch“. In der Vorschauversion.|
|Basis-Verfügbarkeitsgruppen <sup>2</sup>|Nicht zutreffend Ähnliche Funktionen verfügbar.|
|Mindestreplikate für Commitverfügbarkeitsgruppen <sup>2</sup>|Dienstebene „Unternehmenskritisch“. In der Vorschauversion.|
|Verfügbarkeitsgruppe ohne Cluster|Ja|
|Datenbank sichern | Ja – `COPY_ONLY` Siehe [BACKUP – (Transact-SQL)](/sql/t-sql/statements/backup-transact-sql?view=azuresqldb-mi-current&preserve-view=true).|
|Sicherungskomprimierung|Ja|
|Sicherungsspiegelung |Ja|
|Verschlüsseln der Sicherung|Ja|
|Sicherung in Azure in (Sicherung über URL)|Ja|
|Datenbankmomentaufnahme|Ja|
|Schnelle Wiederherstellung|Ja|
|Hinzufügen von Speicher im laufenden Systembetrieb und CPU|Ja|
|Protokollversand|Derzeit nicht verfügbar.|
|Onlineseiten- und Onlinedateiwiederherstellung|Ja|
|Online-Indizierung|Ja|
|Onlineschemaänderung|Ja|
|Fortsetzbare Neuerstellung von online geschalteten Indizes|Ja|

<sup>1</sup> In dem Szenario, in dem ein Podfehler auftritt, wird eine neue SQL Managed Instance-Instanz gestartet und erneut an das persistente Volume angefügt, das die Daten enthält. [Hier erfahren Sie mehr über persistente Kubernetes-Volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes).

## <a name="rdbms-scalability-and-performance"></a><a name="RDBMSSP"></a> RDBMS Scalability and Performance  

| Funktion | SQL Managed Instance mit Azure Arc-Unterstützung |
|--|--|
| columnstore | Ja |
| Große Objektbinärdateien in gruppierten Columnstore-Indizes | Ja |
| Onlineneuerstellung für nicht gruppierten Columnstore-Index | Ja |
| In-Memory-OLTP | Ja |
| Persistenter Hauptspeicher | Ja |
| Tabellen- und Indexpartitionierung | Ja |
| Datenkomprimierung | Ja |
| Resource Governor | Ja |
| Parallelverarbeitung für partitionierte Tabellen | Ja |
| NUMA-basierter und großer Arbeitsspeicher für umfangreiche Seiten und Zuordnung von Pufferarrays | Ja |
| Ressourcenkontrolle für E/A-Vorgänge | Ja |
| Verzögerte Dauerhaftigkeit | Ja |
| Automatische Optimierung | Ja |
| Adaptive Joins im Batchmodus | Ja |
| Feedback zur Speicherzuweisung im Batchmodus | Ja |
| Verschachtelte Ausführung mit Tabellenwertfunktionen mit mehreren Anweisungen | Ja |
| Verbesserungen beim massenhaften Einfügen | Ja |

## <a name="rdbms-security"></a><a name="RDBMSS"></a> RDBMS Security

| Funktion | SQL Managed Instance mit Azure Arc-Unterstützung |
|--|--|
| Sicherheit auf Zeilenebene | Ja |
| Always Encrypted | Ja |
| Always Encrypted mit Secure Enclaves | Nein |
| Dynamische Datenmaskierung | Ja |
| Allgemeine Überwachung | Ja |
| Feine Überwachung | Ja |
| Transparente Datenbankverschlüsselung | Ja |
| Benutzerdefinierte Rollen | Ja |
| Eigenständige Datenbanken | Ja |
| Verschlüsselung von Sicherungen | Ja |
| SQL Server-Authentifizierung | Ja |
| Azure Active Directory-Authentifizierung | Nein |
| Windows-Authentifizierung | Nein |

## <a name="rdbms-manageability"></a><a name="RDBMSM"></a> RDBMS Manageability  

| Funktion | SQL Managed Instance mit Azure Arc-Unterstützung |
|--|--|
| Dedizierte Administratorverbindung | Ja |
| PowerShell-Skriptunterstützung | Ja |
| Unterstützung für Komponentenvorgänge der Datenschichtanwendung: Extrahieren, Bereitstellen, Aktualisieren, Löschen | Ja |
| Richtlinienautomatisierung (Überprüfung nach Zeitplan und Änderungen) | Ja |
| Sammler von Leistungsdaten | Ja |
| Standardleistungsberichte | Ja |
| Planhinweislisten und Planeinfrierung für Planhinweislisten | Ja |
| Direkte Abfrage von indizierten Sichten (mittels NOEXPAND-Hinweis) | Ja |
| Automatische Wartung für indizierte Sichten | Ja |
| Verteilte partitionierte Sichten | Ja |
| Parallele Indexvorgänge | Ja |
| Automatische Verwendung indizierter Sichten mittels Abfrageoptimierer | Ja |
| Parallele Konsistenzprüfung | Ja |

### <a name="programmability"></a><a name="Programmability"></a> Programmability  

| Funktion | SQL Managed Instance mit Azure Arc-Unterstützung |
|--|--|
| JSON | Ja |
| Abfragespeicher | Ja | 
| Temporal | Ja | 
| Systemeigene XML-Unterstützung | Ja | 
| XML-Indizierung | Ja | 
| MERGE- und UPSERT-Funktionen | Ja | 
| Datums- und Uhrzeitdatentypen | Ja | 
| Internationalisierungsunterstützung | Ja | 
| Volltextsuche und semantische Suche | Nein |
| Angabe der Sprache in einer Abfrage | Ja | 
| Service Broker (Messaging) | Ja | 
| Transact-SQL-Endpunkte | Ja | 
| Graph | Ja | 
| Machine Learning Services | Nein |
| PolyBase | Nein |


### <a name="tools"></a>Tools

Azure Arc-fähige SQL Managed Instance-Instanzen unterstützen verschiedene Datentools, die Ihnen bei der Verwaltung Ihrer Daten helfen können.

| **Tool** | SQL Managed Instance mit Azure Arc-Unterstützung|
| --- | --- | --- |
| Azure-Portal <sup>1</sup> | Nein |
| Azure CLI | Ja |
| [Azure Data Studio](/sql/azure-data-studio/what-is) | Ja |
| Azure PowerShell | Nein |
| [BACPAC Datei (Export)](/sql/relational-databases/data-tier-applications/export-a-data-tier-application) | Ja |
| [BACPAC Datei (Import)](/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database) | Ja |
| [SQL Server Data Tools (SSDT)](/sql/ssdt/download-sql-server-data-tools-ssdt) | Ja |
| [SQL Server Management Studio (SSMS)](/sql/ssms/download-sql-server-management-studio-ssms) | Ja |
| [SQL Server-PowerShell](/sql/relational-databases/scripting/sql-server-powershell) | Ja |
| [SQL Server Profiler](/sql/tools/sql-server-profiler/sql-server-profiler) | Ja |

<sup>1</sup> Das Azure-Portal kann zum Erstellen, Anzeigen und Löschen von Azure Arc-fähigen SQL Managed Instance-Instanzen verwendet werden.  Updates können derzeit nicht über das Azure-Portal durchgeführt werden.

   [!INCLUDE [use-insider-azure-data-studio](includes/use-insider-azure-data-studio.md)]

### <a name="unsupported-features--services"></a><a name="Unsupported"></a> Nicht unterstützte Features und Dienste

Die folgenden Features und Dienste stehen für Azure Arc-fähige SQL Managed Instance-Instanzen nicht zur Verfügung. Die Unterstützung dieser Features wird im Laufe der Zeit ausgeweitet.

| Bereich | Nicht unterstütztes Feature oder Dienst |
|-----|-----|
| **Datenbank-Engine** | Mergereplikation |
| &nbsp; | Stretch Database |
| &nbsp; | Verteilte Abfrage mit Drittanbieterverbindungen |
| &nbsp; | Verbindungsserver für andere Datenquellen als SQL Server- und Azure SQL-Produkte |
| &nbsp; | Erweiterte gespeicherte Systemprozeduren (XP_CMDSHELL usw.) |
| &nbsp; | Dateitabelle, FILESTREAM |
| &nbsp; | CLR-Assemblys mit festgelegter EXTERNAL_ACCESS- oder UNSAFE-Berechtigung |
| &nbsp; | Pufferpoolerweiterung |
| **SQL Server-Agent** |  Subsysteme: CmdExec, PowerShell, Queue Reader, SSIS, SSAS, SSRS |
| &nbsp; | Alerts |
| &nbsp; | Managed Backup |
| **Hochverfügbarkeit** | Datenbankspiegelung  |
| **Security** | Erweiterbare Schlüsselverwaltung |
| &nbsp; | Azure AD-Authentifizierung für Verbindungsserver | 
| &nbsp; | Azure AD-Authentifizierung für Verfügbarkeitsgruppen | 
