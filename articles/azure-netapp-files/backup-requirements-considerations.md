---
title: 'Azure NetApp Files-Sicherung: Anforderungen und Überlegungen | Microsoft-Dokumentation'
description: Hier werden die Anforderungen und Überlegungen beschrieben, die Sie beachten müssen, bevor Sie die Azure NetApp Files-Sicherung verwenden.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/07/2021
ms.author: b-juche
ms.openlocfilehash: 5e622f86b7b0b9d757dc32089db86c7bfca7fe63
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2021
ms.locfileid: "130239750"
---
# <a name="requirements-and-considerations-for-azure-netapp-files-backup"></a>Azure NetApp Files-Sicherung: Anforderungen und Überlegungen 

In diesem Artikel werden die Anforderungen und Überlegungen beschrieben, die Sie beachten müssen, bevor Sie die Azure NetApp Files-Sicherung verwenden.

## <a name="requirements-and-considerations"></a>Anforderungen und Überlegungen

Vor der Verwendung der Azure NetApp Files-Sicherung müssen Sie einige Anforderungen und Überlegungen berücksichtigen: 

* Die Azure NetApp Files-Sicherung ist in den Regionen verfügbar, die mit Ihrem Azure NetApp Files-Abonnement verknüpft sind. Die Azure NetApp Files-Sicherung in einer Region kann nur ein Azure NetApp Files-Volume schützen, das sich in derselben Region befindet. Beispielsweise werden vom Dienst in „USA, Westen 2“ erstellte Sicherungen für ein Volume in „USA, Westen 2“ an den Azure-Speicher gesendet, der sich ebenfalls in „USA, Westen 2“ befindet. Azure NetApp Files unterstützt keine Sicherungen oder Sicherungsreplikationen in einer anderen Region.  

* Nach dem Abschluss einer Sicherung kann es bis zu fünf Minuten dauern, bis sie angezeigt wird.

* Derzeit unterstützt das Azure NetApp Files-Sicherungsfeature das Sichern der täglichen, wöchentlichen und monatlichen lokalen Momentaufnahmen im Azure-Speicher, die von der zugehörigen Momentaufnahmerichtlinie erstellt wurden. Stündliche Sicherungen werden derzeit nicht unterstützt.

* Die Azure NetApp Files-Sicherung verwendet das Konto für [zonenredundanten Speicher (ZRS)](../storage/common/storage-redundancy.md#redundancy-in-the-primary-region), das die Daten synchron über drei Azure-Verfügbarkeitszonen in der Region repliziert, mit Ausnahme der unten aufgeführten Regionen, in denen nur [lokal redundanter Speicher (LRS)](../storage/common/storage-redundancy.md#redundancy-in-the-primary-region) unterstützt wird:   

    * USA (Westen)   

    Aus lokal redundantem Speicher können Daten nach Serverrack- und Laufwerkausfällen wiederhergestellt werden. Bei einem Katastrophenfall wie einem Brand oder einer Überschwemmung in einem Rechenzentrum ist es jedoch möglich, dass alle Replikate in einem Speicherkonto mit LRS verloren gehen oder nicht mehr wiederhergestellt werden können. 

* Die Verwendung von richtlinienbasierten (geplanten) Azure NetApp Files-Sicherungen erfordert, dass die Momentaufnahmerichtlinie konfiguriert und aktiviert ist. Siehe [Verwalten von Momentaufnahmen mithilfe von Azure NetApp Files](azure-netapp-files-manage-snapshots.md).   
    Für das zu sichernde Volume ist eine konfigurierte Momentaufnahmerichtlinie zum Erstellen von Momentaufnahmen erforderlich. Die konfigurierte Anzahl von Sicherungen wird im Azure-Speicher gespeichert. 

* Wenn ein Problem auftritt (z. B. kein ausreichender Speicherplatz auf dem Volume) und die Momentaufnahmerichtlinie keine neuen Momentaufnahmen mehr erstellt, sind für das Sicherungsfeature keine neuen Momentaufnahmen vorhanden, die gesichert werden müssen. 

* In einer regionsübergreifenden Replikationseinstellung kann Azure NetApp Files Backup nur auf einem Quellvolume konfiguriert werden. Sie wird auf einem regionenübergreifenden Replikationsvolume *Zielvolume* nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte

* [Grundlegendes zur Azure NetApp Files-Sicherung](backup-introduction.md)
* [Ressourcenlimits für Azure NetApp Files](azure-netapp-files-resource-limits.md)
* [Konfigurieren von richtlinienbasierten Sicherungen](backup-configure-policy-based.md)
* [Konfigurieren von manuellen Sicherungen](backup-configure-manual.md)
* [Verwalten von Sicherungsrichtlinien](backup-manage-policies.md)
* [Durchsuchen von Sicherungen](backup-search.md)
* [Wiederherstellen einer Sicherung auf einem neuen Volume](backup-restore-new-volume.md)
* [Deaktivieren der Sicherungsfunktionalität für ein Volume](backup-disable.md)
* [Sicherungen eines Volumes löschen](backup-delete.md)
* [Metriken zur Volume-Sicherung](azure-netapp-files-metrics.md#volume-backup-metrics)
* [Häufig gestellte Fragen zur Azure NetApp Files-Sicherung](faq-backup.md)
* [Funktionsweise von Azure NetApp Files-Momentaufnahmen](snapshots-introduction.md)
