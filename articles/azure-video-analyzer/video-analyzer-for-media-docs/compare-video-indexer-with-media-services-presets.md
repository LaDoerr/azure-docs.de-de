---
title: Vergleich der Voreinstellungen von Azure Video Analyzer for Media (früher Video Indexer) und Azure Media Services v3
description: In diesem Artikel werden die Voreinstellungen von Azure Video Analyzer for Media (früher Video Indexer) und Azure Media Services v3 gegenübergestellt.
services: azure-video-analyzer
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.subservice: azure-video-analyzer-media
ms.date: 02/24/2020
ms.author: juliako
ms.openlocfilehash: cdd354adc7a34cdcb7e0811ffd2a5d0b50018fee
ms.sourcegitcommit: 0af634af87404d6970d82fcf1e75598c8da7a044
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/15/2021
ms.locfileid: "112121499"
---
# <a name="compare-azure-media-services-v3-presets-and-video-analyzer-for-media"></a>Vergleich der Voreinstellungen von Azure Media Services v3 und Video Analyzer for Media 

In diesem Artikel werden die Funktionen der **APIs von Video Analyzer for Media (ehemals Video Indexer)** und **Media Services v3-APIs** verglichen. 

Derzeit gibt es eine Überschneidung bei den Features der [APIs von Video Analyzer for Media](https://api-portal.videoindexer.ai/) und der [APIs von Media Services v3](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01/Encoding.json). In der folgenden Tabelle finden Sie die aktuelle Richtlinie zum Verständnis der Unterschiede und Gemeinsamkeiten. 

## <a name="compare"></a>Vergleichen

|Funktion|APIs von Video Analyzer for Media |Voreinstellungen für das Video- und Audioanalysetool<br/>in APIs von Azure Media Services v3|
|---|---|---|
|Medienerkenntnisse|[Erweitert](video-indexer-output-json-v2.md) |[Grundlagen](../../media-services/latest/analyze-video-audio-files-concept.md)|
|Erfahrungen|Die vollständige Liste der unterstützten Features finden Sie hier: <br/> [Übersicht](video-indexer-overview.md)|Gibt nur Videoerkenntnisse zurück.|
|Abrechnung|[Media Services – Preise](https://azure.microsoft.com/pricing/details/media-services/#analytics)|[Media Services – Preise](https://azure.microsoft.com/pricing/details/media-services/#analytics)|
|Kompatibilität|Aktuelle Complianceupdates finden Sie in [Azure Compliance Offerings.pdf](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942/file/178110/23/Microsoft%20Azure%20Compliance%20Offerings.pdf), indem Sie nach „Video Analyzer for Media“ suchen, um zu prüfen, ob der Dienst mit einem bestimmten Zertifikat konform ist.|Aktuelle Complianceupdates finden Sie in [Azure Compliance Offerings.pdf](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942/file/178110/23/Microsoft%20Azure%20Compliance%20Offerings.pdf), indem Sie nach „Media Services“ suchen, um zu prüfen, ob der Dienst mit einem bestimmten Zertifikat konform ist.|
|Kostenlose Testversion|East US|Nicht verfügbar|
|Regionale Verfügbarkeit|Siehe [Verfügbarkeit von Cognitive Services nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services)|Siehe [Verfügbarkeit von Media Services nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=media-services)|

## <a name="next-steps"></a>Nächste Schritte

[Video Analyzer for Media – Übersicht](video-indexer-overview.md)

[Media Services v3 – Übersicht](../../media-services/latest/media-services-overview.md)
