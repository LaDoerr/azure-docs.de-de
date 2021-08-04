---
title: Metriken für Azure NetApp Files | Microsoft-Dokumentation
description: 'Azure NetApp Files verfügt über Metriken für Folgendes: zugeteilter Speicher, tatsächliche Speichernutzung, IOPS pro Volume und Latenz. Anhand dieser Metriken können Sie Verbrauch und Leistung analysieren.'
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
ms.date: 05/06/2021
ms.author: b-juche
ms.openlocfilehash: e16e95bbb65bde6c4c0b38b9c68c0f7287b8b9b3
ms.sourcegitcommit: 89c4843ec85d1baea248e81724781d55bed86417
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108795580"
---
# <a name="metrics-for-azure-netapp-files"></a>Metriken für Azure NetApp Files

Azure NetApp Files verfügt über Metriken für Folgendes: zugeteilter Speicher, tatsächliche Speichernutzung, IOPS pro Volume und Latenz. Durch die Analyse dieser Metriken können Sie ein besseres Verständnis des Nutzungsmusters und der Volumeleistung Ihrer NetApp-Konten entwickeln.  

Sie finden Metriken für einen Kapazitätspool oder ein Volume, indem Sie den **Kapazitätspool** oder das **Volume** auswählen.  Klicken Sie dann auf **Metrik**, um die verfügbaren Metriken anzuzeigen: 

[ ![Screenshot, der zeigt, wie Sie zur Pull-Down-Metrik navigieren.](../media/azure-netapp-files/metrics-navigate-volume.png) ](../media/azure-netapp-files/metrics-navigate-volume.png#lightbox)

## <a name="usage-metrics-for-capacity-pools"></a><a name="capacity_pools"></a>Nutzungsmetriken für Kapazitätspools

- *Zugeordnete Poolgröße*   
    Die bereitgestellte Größe des Pools.

- *Zu Volumegröße zugeordneter Pool*  
    Die Gesamtsumme des Volumekontingents (GiB) eines bestimmten Kapazitätspools (Gesamtsumme der bereitgestellten Größen der Volumes im Kapazitätspool)  
    Diese Größe haben Sie beim Erstellen des Volumes ausgewählt.  

- *Vom Pool genutzte Größe*  
    Die Gesamtsumme des logischen Speichers (GiB), der in einem Kapazitätspool volumeübergreifend genutzt wird.  

- *Gesamtgröße der Momentaufnahme für den Pool*    
    Die Summe der Momentaufnahmegrößen aller Volumes in dem Pool

## <a name="usage-metrics-for-volumes"></a><a name="volumes"></a>Nutzungsmetriken für Volumes

- *Größe der verbrauchten Menge in Prozent*    
    Der Prozentsatz des verbrauchten Volumes, einschließlich Momentaufnahmen.  
- *Zugeordnete Größe des Volumes*   
    Die bereitgestellte Größe eines Volumes
- *Größe des Volumekontingents*    
    Die Kontingentgröße (GiB), mit der das Volume bereitgestellt wird   
- *Vom Volume genutzte Größe*   
    Logische Größe des Volumes (verwendete Bytes).  
    In diesem Größenwert ist der logische Speicherplatz enthalten, der von aktiven Dateisystemen und Momentaufnahmen verwendet wird.  
- *Größe der Volumemomentaufnahme*   
   Größe aller Momentaufnahmen in einem Volume  

## <a name="performance-metrics-for-volumes"></a>Leistungsmetriken für Volumes

> [!NOTE] 
> Die Volumelatenz für die *durchschnittliche Leselatenz* und die *durchschnittliche Schreiblatenz* wird im Speicherdienst gemessen und umfasst keine Netzwerklatenz.

- *Durchschnittliche Wartezeit beim Lesevorgang*   
    Die durchschnittliche Dauer von Lesevorgängen vom Volume in Millisekunden.
- *Durchschnittliche Wartezeit beim Schreibvorgang*   
    Die durchschnittliche Dauer von Schreibvorgängen vom Volume in Millisekunden.
- *Lese-IOPS*   
    Die Anzahl der Lesevorgänge auf dem Volume pro Sekunde.
- *Schreib-IOPS*   
    Die Anzahl der Schreibvorgänge auf dem Volume pro Sekunde.

## <a name="volume-replication-metrics"></a><a name="replication"></a>Metriken für die Volumereplikation

> [!NOTE] 
> * Die Netzwerkübertragungsgröße (z. B. die *Gesamtübertragung der Volumereplikation*-Metriken) kann sich von den Quell- oder Zielvolumes einer regionsübergreifenden Replikation unterscheiden. Dieses Verhalten ist darauf zurückzuführen, dass eine effiziente Replikations-Engine verwendet wird, um die Netzwerkübertragungskosten zu reduzieren.
> * Metriken für die Volumereplikation werden derzeit für Replikationszielvolumes und nicht für die Quelle der Replikationsbeziehung aufgefüllt.

- *Ist Volumereplikationsstatus „Fehlerfrei“*    
    Der Zustand der Replikationsbeziehung. Ein fehlerfreier Status wird durch `1` angegeben. Ein fehlerhafter Status wird durch `0` angegeben.

- *Wird Volumereplikation übertragen*    
    Gibt an, ob der Status der Volumereplikation „Wird übertragen“ lautet. 
 
- *Verzögerungszeit bei der Volumereplikation*   
    Die Zeit in Sekunden, um die die Daten in der Spiegelung im Vergleich zur Quelle verzögert sind 

- *Übertragungsdauer der letzten Volumereplikation*   
    Die Dauer der letzten Übertragung in Sekunden 

- *Übertragungsgröße der letzten Volumereplikation*    
    Bei der letzten Übertragung insgesamt übertragene Bytes 

- *Fortschritt der Volumereplikation*    
    Die insgesamt beim aktuellen Übertragungsvorgang übertragenen Daten. 

- *Insgesamt bei der Volumereplikation übertragene Bytes*   
    Die kumulative Anzahl der für die Beziehung übertragenen Bytes. 

## <a name="throughput-metrics-for-capacity-pools"></a>Durchsatzmetriken für Kapazitätspools   

* *Zugeordneter Pool für Volumedurchsatz*    
    Der Gesamtdurchsatz, der Volumes in einem bestimmten Kapazitätspool zugeordnet ist. Dies ist der gesamte zugeordnete Durchsatz der Volumes im Kapazitätspool.   

* *Verbrauchter Durchsatz eines Pools*   
    Der gesamte Durchsatz, der von Volumes in einem bestimmten Kapazitätspool genutzt wird   

* *Prozentsatz des zugeordneten Pools für Volumedurchsatz*   
    Prozentsatz des bereitgestellten Durchsatzes des Kapazitätspools, der Volumes zugeordnet ist   

* *Prozentsatz des verbrauchten Durchsatzes eines Pools*    
    Prozentsatz des bereitgestellten Durchsatzes des Kapazitätspools, der von Volumes verbraucht wird

## <a name="throughput-metrics-for-volumes"></a>Durchsatzmetriken für Volumes   

*  *Zugeordneter Volumedurchsatz*    
    Der Durchsatz des übergeordneten Kapazitätspools (MiB/s), dem das Volume zugeordnet ist. Dies ist der maximale Durchsatz, den das Volume nutzen kann.

* *Verbrauchter Durchsatz eines Volumes*    
    Der tatsächliche Durchsatz (MiB/s), den das Volume nutzt

* *Prozentsatz des verbrauchten Durchsatzes eines Volumes*   
    Prozentsatz des zugeordneten Durchsatzes, den das Volume nutzt. Das heißt, *Verbrauchter Durchsatz eines Volumes* ist ein Prozentsatz von *Zugeordneter Volumedurchsatz*.


## <a name="next-steps"></a>Nächste Schritte

* [Grundlegendes zur Speicherhierarchie von Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md)
* [Einrichten eines Kapazitätspools](azure-netapp-files-set-up-capacity-pool.md)
* [Erstellen eines Volumes für Azure NetApp Files](azure-netapp-files-create-volumes.md)
