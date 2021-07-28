---
title: 'Übersicht: Azure Database for MySQL Single Server'
description: Hier erfahren Sie mehr zu Azure Database for MySQL Single Server, einem relationalen Datenbankdienst in der Microsoft-Cloud, der auf MySQL Community Edition basiert.
author: savjani
ms.service: mysql
ms.author: pariks
ms.custom: mvc
ms.topic: overview
ms.date: 8/20/2020
ms.openlocfilehash: 9752b97e2f9bcff2f08dd2a280014e2c3d4a06c6
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110470268"
---
# <a name="azure-database-for-mysql-single-server"></a>Azure Database for MySQL Single Server

[Azure Database for MySQL](overview.md), ein Dienst, der auf MySQL Community Edition basiert, ist in zwei Bereitstellungsmodi verfügbar:
- Einzelner Server 
- Flexible Server (Vorschau)

In diesem Artikel erhalten Sie eine Übersicht und eine Einführung in die grundlegenden Konzepte des Bereitstellungsmodells Azure Database for MySQL Single Server. Weitere Informationen zum Bereitstellungsmodus Flexible Server finden Sie in der Übersicht zu [Flexible Server](flexible-server/index.yml). Informationen dazu, wie Sie entscheiden, welche Bereitstellungsoption für Ihre Workload geeignet ist, finden Sie unter [Auswählen der richtigen MySQL Server-Option in Azure](select-right-deployment-type.md).

## <a name="overview"></a>Übersicht

Bei Single Server handelt es sich um einen vollständig verwalteten Datenbankdienst mit minimalen Anforderungen für die Anpassung der Datenbank. Die Single Server-Plattform ist für die Verarbeitung der meisten Datenbankverwaltungsfunktionen (z. B. Patching, Sicherungen, Hochverfügbarkeit und Sicherheit) mit minimaler Benutzerkonfiguration und -steuerung konzipiert. Die Architektur ist für eine Verfügbarkeit von 99,99 Prozent in einer einzelnen Verfügbarkeitszone optimiert. Sie unterstützt die MySQL Community-Versionen 5.6, 5.7 und 8.0. Der Dienst ist ab heute in vielen [Azure-Regionen](https://azure.microsoft.com/global-infrastructure/services/) allgemein verfügbar. 

Single Server-Instanzen eignen sich am besten für cloudnative Anwendungen, da sie für das automatisierte Patchen ohne genauen Patchzeitplan und ohne benutzerdefinierte MySQL-Konfigurationseinstellungen konzipiert sind. 

## <a name="high-availability"></a>Hochverfügbarkeit

Der Bereitstellungsmodus mit einem Einzelserver wurde für integrierte Hochverfügbarkeit und Flexibilität bei geringeren Kosten konzipiert. In der Architektur sind Compute und Speicher voneinander getrennt. Die Datenbank-Engine wird in einem eigenentwickelten Computecontainer ausgeführt, während sich die Datendateien im Azure-Speicher befinden. Der Speicher verwaltet drei lokal redundante synchrone Kopien der Datenbankdateien, sodass die Datenbeständigkeit sichergestellt wird. 

Wenn bei geplanten oder ungeplanten Failoverereignissen der Server ausfällt, gewährleistet der Dienst die Hochverfügbarkeit der Server mit dem folgenden automatisierten Verfahren:

1. Ein neuer Computecontainer wird bereitgestellt.
2. Der Speicher mit Datendateien wird dem neuen Container zugeordnet. 
3. Die MySQL-Datenbank-Engine wird im neuen Computecontainer online geschaltet.
4. Der Gatewaydienst stellt ein transparentes Failover sicher, sodass keine Änderungen an der Anwendung erforderlich sind. 
  
Für gewöhnlich dauert das Failover zwischen 60 und 120 Sekunden. Da der Single Server-Dienst cloudnativ konzipiert wurde, ist die Unterstützung von 99,99 Prozent Verfügbarkeit möglich, wodurch die Kosten für einen passiven unmittelbar betriebsbereiten Standbyserver entfallen.

Durch das globale Netzwerk aus durch Microsoft verwalteten Rechenzentren stellt die branchenführende Verfügbarkeit von Azure mit einer Vereinbarung zum Servicelevel (SLA) von 99,99 Prozent sicher, dass Ihre Anwendungen rund um die Uhr ausgeführt werden.

:::image type="content" source="media/single-server-overview/1-singleserverarchitecture.png" alt-text="Konzeptionelle Abbildung der Architektur von Azure Database for MySQL Single Server"::: 

## <a name="automated-patching"></a>Automatisiertes Patchen 

Der Dienst führt automatisches Patchen für die zugrunde liegende Hardware, das Betriebssystem und die Datenbank-Engine durch. Das Patchen umfasst Sicherheits-und Softwareupdates. Bei der MySQL-Engine werden Upgrades von Nebenversionen automatisch ausgeführt und sind im Patchzyklus enthalten. Für das Patchen sind keine Benutzeraktionen oder Konfigurationseinstellungen erforderlich. Die Patchhäufigkeit wird basierend auf der Wichtigkeit der Nutzlast durch den Dienst verwaltet. Im Allgemeinen gilt für den Dienst im Rahmen der kontinuierlichen Integration und Veröffentlichung ein monatlicher Releasezeitplan. Benutzer können die [Benachrichtigungen zu geplanten Wartungen](concepts-monitoring.md) abonnieren, um 72 Stunden vorher über anstehende Wartungen benachrichtigt zu werden.

## <a name="automatic-backups"></a>Automatische Sicherungen

Für Azure Database for MySQL Single Server werden Sicherungen automatisch erstellt und in einem vom Benutzer konfigurierten lokal redundanten oder georedundanten Speicher gespeichert. Mithilfe von Sicherungen können Sie den Server zu einem beliebigen Zeitpunkt innerhalb des Aufbewahrungszeitraums der Sicherung wiederherstellen. Die Standardaufbewahrungsdauer für Sicherungen beträgt sieben Tage. Die Aufbewahrungsdauer kann optional auf bis zu 35 Tage festgelegt werden. Zur Verschlüsselung aller Sicherungen wird die AES-Verschlüsselung mit 256 Bit verwendet. Ausführliche Informationen finden Sie im Artikel zu [Sicherungen](concepts-backup.md).

## <a name="adjust-performance-and-scale-within-seconds"></a>Anpassen von Leistung und Skalierung in nur wenigen Sekunden

Der Single Server-Dienst ist in drei SKU-Tarifen verfügbar: „Basic“, „Allgemein“ und „Arbeitsspeicheroptimiert“. Der Basic-Tarif eignet sich am besten für die kostengünstige Entwicklung und Workloads mit geringer Parallelität. Die Tarife „Universell“ und „Arbeitsspeicheroptimiert“ eignen sich besser für Produktionsworkloads, die eine hohe Parallelität, Skalierung und vorhersagbare Leistung erfordern. Sie können Ihre erste App mit einer kleinen Datenbank für wenige USD im Monat erstellen und die Skalierung dann so ändern, dass sie den Anforderungen Ihrer Lösung entspricht. Die Speicherskalierung geschieht online und unterstützt das automatische Wachstum des Speichers. Durch die dynamische Skalierung kann Ihre Datenbank transparent auf schnell wechselnde Ressourcenanforderungen reagieren. Sie zahlen nur für die Ressourcen, die Sie verwenden. Einzelheiten hierzu finden Sie unter [Tarife](./concepts-pricing-tiers.md).

## <a name="enterprise-grade-security-compliance-and-governance"></a>Sicherheit, Compliance und Governance auf Unternehmensniveau

Der Single Server-Dienst nutzt das FIPS 140-2-zertifizierte Kryptografiemodul für die Speicherverschlüsselung ruhender Daten. Daten einschließlich Sicherungen und während der Ausführung von Abfragen erstellte temporäre Daten werden auf dem Datenträger verschlüsselt. Der Dienst verwendet das in der Azure Storage-Verschlüsselung enthaltene AES-256-Bit-Verschlüsselungsverfahren, und die Schlüssel können vom System (Standard) oder vom [Kunden](concepts-data-encryption-mysql.md) verwaltet werden. Der Dienst verschlüsselt Daten während der Übertragung mit der Transport Layer Security (SSL/TLS), was standardmäßig erzwungen wird. Der Dienst unterstützt die TLS-Versionen 1.2, 1.1 und 1.0 mit der Möglichkeit, die [mindestens erforderliche TLS-Version](concepts-ssl-connection-security.md) zu erzwingen. 

Der Dienst ermöglicht über eine [private Verbindung](concepts-data-access-security-private-link.md) den privaten Zugriff auf die Server und bietet über den optionalen Plan [Azure Defender für relationale Open-Source-Datenbanken](../security-center/defender-for-databases-introduction.md) Schutz gegen Bedrohungen. Azure Defender erkennt anomale Aktivitäten, die auf ungewöhnliche und potenziell schädliche Versuche hinweisen, auf Datenbanken zuzugreifen oder diese missbräuchlich zu nutzen.

Neben der nativen Authentifizierung unterstützt der Single Server-Dienst zudem die Authentifizierung mit [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md). Bei der Authentifizierung mit Azure AD wird eine Verbindung mit den MySQL-Servern mithilfe von Identitäten hergestellt, die in Azure AD definiert und verwaltet werden. Mit der Azure AD-Authentifizierung können Sie Datenbankbenutzeridentitäten und andere Azure-Dienste an einem zentralen Ort verwalten, wodurch die Zugriffssteuerung vereinfacht und zentralisiert wird.

[Überwachungsprotokolle](concepts-audit-logs.md) sind verfügbar, um alle Aktivitäten auf Datenbankebene zu überwachen. 

Der Single Server-Dienst ist mit allen branchenführenden Zertifizierungen wie FedRAMP, HIPAA und PCI-DSS kompatibel. Weitere Informationen zur Sicherheit der Azure-Plattform finden Sie im [Azure Trust Center](https://www.microsoft.com/trustcenter/security). 

Weitere Informationen zu Sicherheitsfeatures von Azure Database for MySQL finden Sie in der [Übersicht über die Sicherheit](concepts-security.md).

## <a name="monitoring-and-alerting"></a>Überwachung und Warnung

Der Single Server-Dienst verfügt über integrierte Features zur Leistungsüberwachung und Warnung. Alle Azure-Metriken werden im Minutentakt erfasst, und für jede Metrik steht ein Verlauf von 30 Tagen zur Verfügung. Sie können Warnungen für die Metriken konfigurieren. Der Dienst ermöglicht das Konfigurieren von Protokollen für langsame Abfragen und verfügt über eine differenzierte Funktion für [das Speichern von Abfragen](concepts-query-store.md). Der Abfragespeicher vereinfacht das Beheben von Leistungsproblemen, da er es Ihnen ermöglicht, die am längsten ausgeführten und ressourcenintensivsten Abfragen schnell zu ermitteln. Mit diesen Tools können Sie Ihre Workloads schnell optimieren und Ihren Server für optimale Leistung konfigurieren. Weitere Informationen finden Sie unter [Überwachung](concepts-monitoring.md).

## <a name="migration"></a>Migration

Der Dienst führt die MySQL Community-Version aus. Dies ermöglicht vollständige Anwendungskompatibilität und erfordert minimale Refactoringkosten für die Migration vorhandener Anwendungen, die in der MySQL-Engine entwickelt wurden, zum Single Server-Dienst. Die Migration zum Single Server-Dienst kann mithilfe einer der folgenden Optionen durchgeführt werden:

- **Sichern und Wiederherstellen**: Für Offlinemigrationen, bei denen sich Benutzer einige Ausfallzeiten leisten können, kann das Sichern und Wiederherstellen mit Communitytools wie mysqldump/mydumper die schnellste Option für die Migration sein. Ausführliche Informationen finden Sie unter [Migrieren der MySQL-Datenbank auf Azure-Datenbank für MySQL durch Sicherungen und Wiederherstellungen](concepts-migrate-dump-restore.md). 
- **Azure Database Migration Service**: Für eine nahtlose und einfachere Migration zum Single Server-Dienst mit minimalen Ausfallzeiten können Sie [Azure Database Migration Service](../dms/tutorial-mysql-azure-mysql-online.md) verwenden. 
- **Datenreplikation**: Bei Migrationen mit minimaler Ausfallzeit kann die Datenreplikation, die von der auf einer binären Protokolldatei basierenden Replikation abhängt, ebenfalls genutzt werden. Die Datenreplikation wird von Branchenexperten für minimale Ausfallzeiten bei Migrationen bevorzugt, da sie mehr Kontrolle über die Migration ermöglicht. Weitere Informationen finden Sie im Artikel zur [Datenreplikation](concepts-data-in-replication.md).

## <a name="contacts"></a>Kontakte
Sollten Sie Fragen oder Vorschläge im Zusammenhang mit der Verwendung von Azure Database for MySQL haben, senden Sie eine E-Mail an das zuständige Team ([@Ask Azure DB for MySQL](mailto:AskAzureDBforMySQL@service.microsoft.com)). Bei dieser E-Mail-Adresse handelt es sich nicht um einen Alias für den technischen Support.

Weitere Kontaktmöglichkeiten:

- Wenn Sie den Azure-Support kontaktieren möchten, [fordern Sie im Azure-Portal ein Ticket an](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- Um ein Problem mit Ihrem Konto zu beheben, richten Sie im Azure-Portal eine [Anfrage an den Support](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
- Wenn Sie Feedback abgeben oder Vorschläge für neue Features einreichen möchten, erstellen Sie einen Eintrag über [UserVoice](https://feedback.azure.com/forums/597982-azure-database-for-mysql).

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie nun die Grundlagen zum Bereitstellungsmodus Azure Database for MySQL Single Server kennen, können Sie mit den folgenden Themen fortfahren:

- Erstellen Sie Ihren ersten Server. 
  - [Erstellen eines Servers für Azure-Datenbank für MySQL mithilfe des Azure-Portals](quickstart-create-mysql-server-database-using-azure-portal.md)
  - [Erstellen eines Servers für Azure-Datenbank für MySQL mithilfe der Azure CLI](quickstart-create-mysql-server-database-using-azure-cli.md)
  - [Azure CLI-Beispiele für Azure-Datenbank für MySQL](sample-scripts-azure-cli.md)

- Erstellen Sie Ihre erste App in Ihrer bevorzugten Sprache:
  - [Python](./connect-python.md)
  - [Node.JS](./connect-nodejs.md)
  - [Java](./connect-java.md)
  - [Ruby](./connect-ruby.md)
  - [PHP](./connect-php.md)
  - [.NET (C#)](./connect-csharp.md)
  - [Go](./connect-go.md)
