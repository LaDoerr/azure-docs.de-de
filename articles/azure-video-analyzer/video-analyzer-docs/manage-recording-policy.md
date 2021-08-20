---
title: Verwalten der Aufzeichnungsrichtlinie mit Azure Video Analyzer
description: In diesem Thema wird erklärt, wie Sie die Aufzeichnungsrichtlinie mit Azure Video Analyzer verwalten können.
ms.service: azure-video-analyzer
ms.topic: how-to
ms.date: 06/01/2021
ms.openlocfilehash: 31b9d4f14a9c60687b9a6e8f32fcc3a1fba82856
ms.sourcegitcommit: 3941df51ce4fca760797fa4e09216fcfb5d2d8f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2021
ms.locfileid: "114601571"
---
# <a name="manage-recording-policy-with-video-analyzer"></a>Verwalten der Aufzeichnungsrichtlinie mit Azure Video Analyzer

Sie können Azure Video Analyzer für die [Aufzeichnung](video-recording.md) von Live-Video in der Cloud über einen Zeitraum von Wochen, Monaten oder Jahren verwenden. Diese Aufzeichnung kann entweder [kontinuierlich](continuous-video-recording.md), platzsparend oder [ereignisbasiert](event-based-video-recording-concept.md) sein. In jedem Fall können sich die Aufzeichnungen über Jahre erstrecken. Sie können die Dauer (in Tagen) dieses Cloudarchivs mithilfe der in Azure Storage integrierten [Tools zur Lebenszyklusverwaltung](../../storage/blobs/storage-lifecycle-management-concepts.md?tabs=azure-portal) verwalten.  

Ihr Video Analyzer-Konto ist mit einem Microsoft Azure-Speicherkonto verknüpft, und wenn Sie zu einer Videoressource aufzeichnen, werden die Mediendaten zu einem Container im Speicherkonto geschrieben. Mithilfe der Lebenszyklusverwaltung können Sie eine [Richtlinie](../../storage/blobs/storage-lifecycle-management-concepts.md?tabs=azure-portal#policy) für ein Speicherkonto definieren, in der Sie eine [Regel](../../storage/blobs/storage-lifecycle-management-concepts.md?tabs=azure-portal#rules) wie die folgende angeben können.

```
{
  "rules": [
    {
      "name": "NinetyDayRule",
      "enabled": true,
      "type": "Lifecycle",
      "definition": {
        "filters": {
          "blobTypes": [ "blockBlob" ]
        },
        "actions": {
          "baseBlob": {
            "tierToCool": { "daysAfterModificationGreaterThan": 30 },
            "delete": { "daysAfterModificationGreaterThan": 90 }
          }
        }
      }
    }
  ]
}
```

Für die obige Regel gilt Folgendes:

* Sie gilt für alle Blockblobs im Speicherkonto.
* Sie gibt an, dass die Blobs, die älter als 30 Tage sind, von der [heißen in die kalte Speicherebene](../../storage/blobs/storage-blob-storage-tiers.md?tabs=azure-portal) verschoben werden.
* Wenn Blobs älter als 90 Tage sind, müssen sie gelöscht werden.

Wenn Sie Azure Video Analyzer zum Aufzeichnen in einer Videoressource verwenden, spezifizieren Sie eine `segmentLength`Funktion, welche die Mindestdauer des Videos (in Sekunden) angibt, die angehängt werden soll, bevor es in das Speicherkonto geschrieben wird. Ihre Videoressource enthält eine Reihe von Segmenten, von denen jedes einen um `segmentLength` neueren Erstellungszeitstempel als das vorherige hat. Wenn die Richtlinie für die Lebenszyklusverwaltung greift, werden Segmente gelöscht, die älter als der angegebene Schwellenwert sind. Sie können jedoch weiterhin über die Azure Video Analyzer-APIs auf die verbleibenden Segmente zugreifen und diese wiedergeben. Weitere Informationen finden Sie unter [Wiedergeben von Aufzeichnungen](playback-recordings-how-to.md). 

## <a name="limitations"></a>Einschränkungen

Im Folgenden finden Sie einige bekannte Einschränkungen bei der Lebenszyklusverwaltung:

* In der Richtlinie können maximal 100 Regeln festgelegt werden, und in jeder Regel können bis zu 10 Container angegeben werden. Wenn Sie also unterschiedliche Aufzeichnungsrichtlinien benötigen (z. B. 3-Tage-Archiv für die Kamera auf dem Parkplatz, 30 Tage für die Kamera in der Laderampe und 180 Tage für die Kamera hinter der Kasse), dann können Sie mit einem einzigen Speicherkonto die Regeln für maximal 1000 Kameras anpassen.
* Updates für die Richtlinie zur Lebenszyklusverwaltung werden nicht sofort angewandt. Weitere Einzelheiten finden Sie in [diesem Abschnitt der häufig gestellten Fragen (FAQ)](../../storage/blobs/storage-lifecycle-management-concepts.md?tabs=azure-portal#faq).
* Wenn Sie eine Richtlinie anwenden, mit der Blobs auf die kalte Speicherebene verschoben werden, hat dies möglicherweise Einfluss auf die Wiedergabe dieses Teils des Archivs. Es können zusätzliche Wartezeiten oder sporadische Fehler auftreten. Azure Video Analyzer unterstützt nicht die Wiedergabe von Inhalten auf Archivebene.

## <a name="next-steps"></a>Nächste Schritte

[Wiedergeben von Aufzeichnungen](playback-recordings-how-to.md)
