---
title: Supportmatrix für die Azure Disk-Sicherung
description: Enthält eine Zusammenfassung der Unterstützungseinstellungen und Einschränkungen für Azure Disk Backup.
ms.topic: conceptual
ms.date: 05/27/2021
ms.custom: references_regions
ms.openlocfilehash: 8b6ea48a326ec42aaf33af61b0ec1a6a07bae8fe
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111750927"
---
# <a name="azure-disk-backup-support-matrix"></a>Supportmatrix für die Azure Disk-Sicherung

Sie können [Azure Backup](./backup-overview.md) zum Schutz von Azure-Datenträgern verwenden. In diesem Artikel werden regionale Verfügbarkeit, unterstützte Szenarien und Einschränkungen zusammengefasst.

## <a name="supported-regions"></a>Unterstützte Regionen

Azure Backup ist in allen öffentlichen Cloudregionen verfügbar, mit Ausnahme von „Frankreich, Süden“, „Südafrika, Westen“. In Sovereign Cloud-Regionen ist der Dienst derzeit nicht verfügbar. Diese Regionen werden angekündigt, sobald sie verfügbar werden.

## <a name="limitations"></a>Einschränkungen

- Azure Disk Backup wird für Azure Managed Disks unterstützt, einschließlich freigegebener Datenträger (freigegebener Premium-SSDs). Nicht verwaltete Datenträger werden nicht unterstützt. Zurzeit wird Ultra Disks einschließlich freigegebener Datenträger von dieser Lösung nicht unterstützt, weil keine Momentaufnahmefunktion besteht.

- Azure Disk Backup unterstützt die Sicherung von Datenträgern mit Schreibbeschleunigung. Bei der Wiederherstellung wird der Datenträger jedoch als normaler Datenträger wiederhergestellt. Der Cache mit Schreibbeschleunigung kann auf dem Datenträger aktiviert werden, nachdem er in eine VM eingebunden wurde.

- Azure Backup bietet die Sicherung verwalteter Azure-Datenträger auf Betriebsebene (Momentaufnahme) mit Unterstützung für mehrere Sicherungen pro Tag. Die Sicherungen werden nicht in den Sicherungstresor kopiert.

- Derzeit wird die Wiederherstellung am ursprünglichen Speicherort (Original-Location Recovery, OLR) nicht unterstützt. Bei dieser Wiederherstellungsoption werden die vorhandenen Quelldatenträger, von denen die Sicherungen erstellt wurden, ersetzt. Sie können eine Wiederherstellung aus einem Wiederherstellungspunkt vornehmen, um einen neuen Datenträger entweder in der Ressourcengruppe des Quelldatenträgers, von dem die Sicherungen erstellt wurden, oder in einer beliebigen anderen Ressourcengruppe zu erstellen. Dies wird als Wiederherstellung an einem alternativen Speicherort (Alternate-Location Recovery, ALR) bezeichnet.

- Azure Backup für Managed Disks verwendet inkrementelle Momentaufnahmen, die auf 200 Momentaufnahmen pro Datenträger beschränkt sind. Um zusätzlich zu den geplanten Sicherungen noch bedarfsgesteuerte Sicherungen zu ermöglichen, schränkt die Sicherungsrichtlinie die Gesamtzahl der Sicherungen auf 180 ein. Weitere Informationen finden Sie unter [Inkrementelle Momentaufnahmen](../virtual-machines/disks-incremental-snapshots.md#restrictions) für verwaltete Datenträger.

- [Grenzwerte für Azure-Abonnements und -Dienste](../azure-resource-manager/management/azure-subscription-service-limits.md#virtual-machine-disk-limits) gelten für die Gesamtzahl der Datenträgermomentaufnahmen pro Region und Abonnement.

- Zeitpunktmomentaufnahmen von mehreren an eine VM angefügten Datenträgern werden nicht unterstützt.

- Der Sicherungstresor und die zu sichernden Datenträger können in demselben oder in unterschiedlichen Abonnements enthalten sein. Allerdings müssen sich der Sicherungstresor und die zu sichernden Datenträger in derselben Region befinden.

- Die zu sichernden Datenträger und die Ressourcengruppe für Momentaufnahmen, in der die Momentaufnahmen lokal gespeichert werden, müssen sich im selben Abonnement befinden.

- Ein Datenträger kann aus einer Sicherung in demselben oder in einem anderen Abonnement wiederhergestellt werden. Der wiederhergestellte Datenträger wird jedoch in derselben Region wie die Momentaufnahme erstellt.

- Über ADE verschlüsselte Datenträger können nicht geschützt werden.

- Datenträger, die mit plattformseitig oder kundenseitig verwalteten Schlüsseln verschlüsselt sind, werden unterstützt. Für die Wiederherstellungspunkte eines Datenträgers, der über kundenseitig verwaltete Schlüssel verschlüsselt wurde (CMK), verursacht die Wiederherstellung allerdings einen Fehler, wenn der KeyVault-Schlüssel des Datenträgerverschlüsselungssatzes deaktiviert oder gelöscht wurde.

- Zurzeit ist die Sicherungsrichtlinie nicht bearbeitbar, und auch die Ressourcengruppe für Momentaufnahmen, die einer Sicherungsinstanz beim Konfigurieren der Datenträgersicherung zugewiesen wird, kann nicht geändert werden.

- Derzeit ist das Azure-Portal zum Konfigurieren der Datenträgersicherung auf maximal 20 Datenträger aus demselben Abonnement beschränkt.

- Azure Disk Backup unterstützt PowerShell. Derzeit wird Azure CLI nicht unterstützt.

- Beim Konfigurieren der Sicherung müssen der für die Sicherung ausgewählte Datenträger und die Ressourcengruppe für Momentaufnahmen, in der die Momentaufnahmen gespeichert werden sollen, demselben Abonnement angehören. Sie können keine inkrementelle Momentaufnahme für einen bestimmten Datenträger außerhalb des Abonnements dieses Datenträgers erstellen. Weitere Informationen finden Sie unter [Inkrementelle Momentaufnahmen](../virtual-machines/disks-incremental-snapshots.md#restrictions) für verwaltete Datenträger. Weitere Informationen zum Auswählen einer Ressourcengruppe für Momentaufnahmen finden Sie unter [Konfigurieren der Sicherung](backup-managed-disks.md#configure-backup).

- Für erfolgreiche Sicherungs- und Wiederherstellungsvorgänge sind für die verwaltete Identität des Sicherungstresors Rollenzuweisungen erforderlich. Verwenden Sie nur die in der Dokumentation angegebenen Rollendefinitionen. Die Verwendung anderer Rollen wie z. B. „Besitzer“ oder „Mitwirkender“ wird nicht unterstützt. Wenn Sie kurz nach der Rollenzuweisung mit dem Konfigurieren von Sicherungs- oder Wiederherstellungsvorgängen beginnen, treten möglicherweise Probleme mit Berechtigungen auf. Dies liegt daran, dass die Rollenzuweisungen erst nach einigen Minuten in Kraft treten.

- Bei verwalteten Datenträgern kann die Leistungsstufe bei der Bereitstellung oder danach geändert werden, ohne die Größe des Datenträgers anzupassen. Die Azure Disk Backup-Lösung unterstützt die Leistungsstufenänderungen auf dem zu sichernden Quelldatenträger. Während der Wiederherstellung ist die Leistungsstufe des wiederhergestellten Datenträgers mit der des Quelldatenträgers zum Zeitpunkt der Sicherung identisch. Befolgen Sie die [hier](../virtual-machines/disks-performance-tiers-portal.md) aufgeführte Dokumentation, um die Leistungsstufe Ihres Datenträgers nach dem Wiederherstellungsvorgang zu ändern.

- Die Unterstützung für [private Verbindungen](../virtual-machines/disks-enable-private-links-for-import-export-portal.md) für verwaltete Datenträger ermöglicht es Ihnen, den Export und Import von verwalteten Datenträgern einzuschränken, sodass diese Vorgänge nur in Ihrem virtuellen Azure-Netzwerk stattfinden. Azure Disk Backup unterstützt die Sicherung von Datenträgern, auf denen private Endpunkte aktiviert sind. Dies bedeutet nicht, dass die Sicherungsdaten oder Momentaufnahmen über den privaten Endpunkt zugänglich sind.

- Sie können eine Sicherungsinstanz löschen. Dadurch wird die Sicherung beendet, und es werden alle Sicherungsdaten gelöscht. Aktuell können Sie eine Sicherung nicht deaktivieren, da die Option zum **Beenden der Sicherung und Beibehalten der Sicherungsdaten** nicht unterstützt wird.

- Folgende Grenzwerte gelten für Azure Disk Backup:
    
    | Einstellung | Maximales Limit |
    | --- | --- |
    | Anzahl von Backup-Richtlinien pro Backup-Tresor | 5.000 |
    | Anzahl von Backup-Instanzen pro Backup-Tresor | 5.000 |
    | Zulässige Anzahl von On-Demand-Sicherungen in einer Backup-Instanz pro Tag | 10 |
    | Zulässige Anzahl von Wiederherstellungen in einer Backup-Instanz pro Tag | 10 |

## <a name="next-steps"></a>Nächste Schritte

- [Sichern von Azure Managed Disks](backup-managed-disks.md)