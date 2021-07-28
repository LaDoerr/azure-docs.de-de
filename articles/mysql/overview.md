---
title: 'Übersicht: Azure Database for MySQL'
description: Hier finden Sie Informationen zum Azure Database for MySQL-Dienst, einem relationalen Datenbankdienst in der Microsoft-Cloud, der auf der MySQL Community-Edition basiert.
author: savjani
ms.service: mysql
ms.author: pariks
ms.custom: mvc
ms.topic: overview
ms.date: 3/18/2020
ms.openlocfilehash: 19ee525ffea10b908a30b00089e1bbf197288863
ms.sourcegitcommit: 8b38eff08c8743a095635a1765c9c44358340aa8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/30/2021
ms.locfileid: "113084568"
---
# <a name="what-is-azure-database-for-mysql"></a>Was ist Azure-Datenbank für MySQL?

[!INCLUDE[applies-to-mysql-single-flexible-server](includes/applies-to-mysql-single-flexible-server.md)]

Azure Database for MySQL ist ein relationaler Datenbankdienst in der Microsoft-Cloud, der auf den Versionen 5.6, 5.7 und 8.0 der Datenbank-Engine [MySQL Community Edition](https://www.mysql.com/products/community/) (verfügbar unter der GPLv2-Lizenz) basiert. Azure-Datenbank für MySQL bietet Folgendes:

- Integrierte Hochverfügbarkeit
- Schutz von Daten durch automatische Sicherungen und Point-in-Time-Wiederherstellungen für bis zu 35 Tage.
- Automatisierte Wartung für die zugrunde liegende Hardware, das Betriebssystem und die Datenbank-Engine, um die Sicherheit des Diensts zu gewährleisten und sicherzustellen, dass er stets auf dem aktuellen Stand ist.
- Vorhersagbare Leistung inklusive nutzungsbasierter Bezahlung.
- Elastische Skalierung innerhalb weniger Sekunden.
- Kostenoptimierungsoptionen mit der Möglichkeit, Server anzuhalten/zu starten. 
- Sicherheit auf Unternehmensniveau und branchenführende Compliance, um sowohl ruhende vertrauliche Daten als auch Daten während der Übertragung zu schützen.
- Überwachung und Automatisierung, um die Verwaltung und Überwachung für umfangreiche Bereitstellungen zu vereinfachen.
- Branchenführender Support.

Diese Funktionen erfordern fast keine Administration und werden alle ohne zusätzliche Kosten bereitgestellt. Mit diesen Funktionen können Sie sich auf die schnelle Entwicklung von Apps und die Verkürzung des Zeitraums bis zur Markteinführung konzentrieren, anstatt wertvolle Zeit und Ressourcen für die Verwaltung von virtuellen Computern und der Infrastruktur aufwenden zu müssen. Darüber hinaus können Sie Ihre Anwendung weiterhin mit den Open-Source-Tools und der Plattform Ihrer Wahl entwickeln und mit der Geschwindigkeit und Effizienz liefern, die Ihr Unternehmen verlangt, ohne dass Sie sich dafür neue Fähigkeiten aneignen müssen.

:::image type="content" source="media/overview/1-azure-db-for-mysql-conceptual-diagram.png" alt-text="Konzeptdiagramm zu Azure-Datenbank für MySQL":::

## <a name="deployment-models"></a>Bereitstellungsmodelle

Azure Database for MySQL, ein Dienst, der auf MySQL Community Edition basiert, ist in zwei Bereitstellungsmodi verfügbar:
- Einzelner Server 
- Flexible Server (Vorschau)
  
### <a name="azure-database-for-mysql---single-server"></a>Azure Database for MySQL Single Server

Bei Azure Database for MySQL Single Server handelt es sich um einen vollständig verwalteten Datenbankdienst mit minimalen Anforderungen für die Anpassung der Datenbank. Die Single Server-Plattform kann die meisten Funktionen zur Datenbankverwaltung bereitstellen, z. B. Patching, Sicherungen, Hochverfügbarkeit sowie Sicherheit mit minimaler Benutzerkonfiguration und -steuerung. Die Architektur ist für integrierte Hochverfügbarkeit mit einer Verfügbarkeit von 99,99 % bei einer einzelnen Verfügbarkeitszone optimiert. Sie unterstützt die MySQL Community-Versionen 5.6, 5.7 und 8.0. Der Dienst ist heute in vielen [Azure-Regionen](https://azure.microsoft.com/global-infrastructure/services/) allgemein verfügbar.

Für die Bereitstellungsoption „Einzelserver“ stehen drei Tarife zur Verfügung: „Basic“, „Allgemein“ und „Arbeitsspeicheroptimiert“. Jeder Tarif bietet unterschiedliche Ressourcenfunktionen für Ihre Datenbankworkloads. Sie können Ihre erste App mit einer kleinen Datenbank für wenige USD im Monat erstellen und die Skalierung dann so ändern, dass sie den Anforderungen Ihrer Lösung entspricht. Durch die dynamische Skalierung kann Ihre Datenbank transparent auf schnell wechselnde Ressourcenanforderungen reagieren. Sie bezahlen nur für die benötigten Ressourcen und auch nur dann, wenn Sie sie verwenden. Einzelheiten hierzu finden Sie unter [Tarife](concepts-pricing-tiers.md).

Single Server-Instanzen eignen sich am besten für cloudnative Anwendungen, da sie für das automatisierte Patchen ohne genauen Patchzeitplan und ohne benutzerdefinierte MySQL-Konfigurationseinstellungen konzipiert sind. 

Eine detaillierte Übersicht über den Single Server-Bereitstellungsmodus finden Sie unter [Übersicht über Single Server](single-server-overview.md).

### <a name="azure-database-for-mysql---flexible-server-preview"></a>Azure Database for MySQL-Server Flexible Server (Vorschau)

Azure Database for MySQL Flexible Server ist ein vollständig verwalteter Datenbankdienst, der eine differenziertere Steuerung und mehr Flexibilität bei den Verwaltungsfunktionen und Konfigurationseinstellungen der Datenbank bietet. Durch die höhere Flexibilität und umfassenderen Anpassungsmöglichkeiten lassen sich mit diesem Dienst unterschiedliche Benutzeranforderungen erfüllen. Mit der Flexible Server-Architektur können Benutzer Hochverfügbarkeit in einer einzelnen Verfügbarkeitszone sowie in mehreren Verfügbarkeitszonen auswählen. Flexible Server-Implementierungen bieten eine bessere Kostenoptimierung mit der Möglichkeit, den Server anzuhalten/zu starten, sowie einen burstfähigen Computetarif, der sich ideal für Workloads eignet, die nicht dauerhaft die volle Computekapazität benötigen. Der Dienst unterstützt die MySQL Community-Versionen 5.7 und 8.0. Aktuell befindet sich der Dienst in der Vorschauphase, und er ist in einer Vielzahl von [Azure-Regionen](https://azure.microsoft.com/global-infrastructure/services/) verfügbar.

Flexible Server-Implementierungen eignen sich am besten für folgende Szenarien: 
- Anwendungsentwicklungen, die bessere Steuerungs- und Anpassungsmöglichkeiten erfordern.
- Zonenredundante Hochverfügbarkeit
- Verwaltete Wartungsfenster

Eine detaillierte Übersicht über den Flexible Server-Bereitstellungsmodus finden Sie unter [Übersicht über Flexible Server](flexible-server/overview.md).

## <a name="contacts"></a>Kontakte
Sollten Sie Fragen oder Vorschläge im Zusammenhang mit der Verwendung von Azure Database for MySQL haben, senden Sie eine E-Mail an das zuständige Team ([@Ask Azure DB for MySQL](mailto:AskAzureDBforMySQL@service.microsoft.com)). Bei dieser E-Mail-Adresse handelt es sich nicht um einen Alias für den technischen Support.

Weitere Kontaktmöglichkeiten:

- Wenn Sie den Azure-Support kontaktieren möchten, [fordern Sie im Azure-Portal ein Ticket an](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- Um ein Problem mit Ihrem Konto zu beheben, richten Sie im Azure-Portal eine [Anfrage an den Support](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
- Wenn Sie Feedback abgeben oder Vorschläge für neue Features einreichen möchten, erstellen Sie einen Eintrag über [UserVoice](https://feedback.azure.com/forums/597982-azure-database-for-mysql).

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die beiden Bereitstellungsmodi für Azure Database for MySQL, und wählen Sie die idealen Optionen für Ihre Anforderungen aus.

- [Single Server](single-server/index.yml)
- [Flexible Server](flexible-server/index.yml)
- [Auswählen der richtigen MySQL-Bereitstellungsoption für ihre Workload](select-right-deployment-type.md)
