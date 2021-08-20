---
title: Migrationsleitfaden für Paketerstellung und Übertragung
description: In diesem Artikel finden Sie einen szenariobasierten Migrationsleitfaden für die Paketerstellung und Übertragung, der Sie bei der Migration von Azure Media Services V2 zu V3 unterstützt.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.topic: conceptual
ms.workload: media
ms.date: 03/25/2021
ms.author: inhenkel
ms.openlocfilehash: d8cba5c69802828a0bd486d902d85199f3ae1d00
ms.sourcegitcommit: 5f659d2a9abb92f178103146b38257c864bc8c31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2021
ms.locfileid: "122356580"
---
# <a name="packaging-and-delivery-scenario-based-migration-guidance"></a>Szenariobasierter Migrationsleitfaden für Paketerstellung und Übertragung

![Logo des Migrationsleitfadens](./media/migration-guide/azure-media-services-logo-migration-guide.svg)

<hr color="#5ea0ef" size="10">

![Schritte bei der Migration 2](./media/migration-guide/steps-4.svg)

In diesem Artikel finden Sie einen szenariobasierten Migrationsleitfaden für die Paketerstellung und Übertragung, der Sie bei der Migration von Azure Media Services V2 zu V3 unterstützt.

Wichtige Änderungen bei der Art und Weise, wie Inhalte in der V3-API veröffentlicht werden. Das neue Veröffentlichungsmodell wurde vereinfacht und verwendet weniger Entitäten zum Erstellen eines Streaminglocators. Die API wurde von den zuvor benötigten vier Entitäten auf lediglich zwei Entitäten reduziert. Richtlinien für symmetrische Schlüssel und Streaminglocators ersetzen nun den Bedarf an `ContentKeyAuthorizationPolicy`, `AssetDeliveryPolicy`, `ContentKey` und `AccessPolicy`.

## <a name="packaging-and-delivery-in-v3"></a>Paketerstellung und Übertragung in V3

1. Erstellen von [Richtlinien für symmetrische Schlüssel](drm-content-key-policy-concept.md)
1. Erstellen von [Streaminglocators](stream-streaming-locators-concept.md)
1. Abrufen der [Streamingpfade](create-streaming-locator-build-url.md) 
    1. Konfigurieren dieser Pfade für einen [DASH](encode-dynamic-packaging-concept.md#deliver-dash)- oder [HLS](encode-dynamic-packaging-concept.md#deliver-hls)-Player

Die zugehörigen Schritte finden Sie in den nachstehenden Konzepten, Tutorials und Schrittanleitungen zur Veröffentlichung.

## <a name="publishing-concepts-tutorials-and-how-to-guides"></a>Konzepte, Tutorials und Schrittanleitungen zur Veröffentlichung

### <a name="concepts"></a>Konzepte

- [Dynamische Paketerstellung in Media Services v3](encode-dynamic-packaging-concept.md)
- [Filter](filters-concept.md)
- [Filtern von Manifesten mithilfe von Dynamic Packager](filters-dynamic-manifest-concept.md)
- [Streamingendpunkte (Ursprung) in Azure Media Services](stream-streaming-endpoint-concept.md)
- [Streamen von Inhalten mit CDN-Integration](stream-scale-streaming-cdn-concept.md)
- [Streaminglocators](stream-streaming-locators-concept.md)

### <a name="how-to-guides"></a>Anleitungen

- [Verwalten von Streamingendpunkten in Media Services V3](stream-manage-streaming-endpoints-how-to.md)
- [CLI-Beispiel: Veröffentlichen eines Medienobjekts](cli-publish-asset.md)
- [Erstellen eines Streaminglocators und von URLs](create-streaming-locator-build-url.md)
- [Herunterladen der Ergebnisse eines Auftrags](job-download-results-how-to.md)
- [Signalisieren beschreibender Audiospuren](signal-descriptive-audio-howto.md)
- [Vollständiges Setup von Azure Media Player](../azure-media-player/azure-media-player-full-setup.md)
- [Verwenden des Video.js-Players mit Azure Media Services](player-how-to-video-js-player.md)
- [Verwenden von Shaka Player mit Azure Media Services](player-shaka-player-how-to.md)

## <a name="samples"></a>Beispiele

Sehen Sie sich auch den [Vergleich von V2- und V3-Code in den Codebeispielen](migrate-v-2-v-3-migration-samples.md) an.
