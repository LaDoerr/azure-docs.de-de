---
title: Anpassen eines Personenmodells mit der API von Azure Video Analyzer for Media (früher Video Indexer)
titleSuffix: Azure Media Services
description: Erfahren Sie, wie Sie ein Personenmodell mit der API von Azure Video Analyzer for Media (früher Video Indexer) anpassen.
services: media-services
author: anikaz
manager: johndeu
ms.topic: article
ms.date: 01/14/2020
ms.author: kumud
ms.openlocfilehash: 758b2b62b0ea7b321e20c2e724609b212abb6ef6
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110385770"
---
# <a name="customize-a-person-model-with-the-video-analyzer-for-media-api"></a>Anpassen eines Personenmodells mit der Video Analyzer for Media-API

Azure Video Analyzer for Media (früher Video Indexer) unterstützt die Gesichtserkennung und die Erkennung von Prominenten in Videoinhalten. Das Feature zur Erkennung von Prominenten umfasst ungefähr eine Million Gesichter, die auf häufig angeforderten Datenquellen wie IMDB, Wikipedia und den wichtigsten LinkedIn-Influencern basieren. Gesichter, die vom Feature zur Erkennung von Prominenten nicht erkannt werden, werden erfasst, bleiben aber unbenannt. Nachdem Sie Ihr Video in Video Analyzer for Media hochgeladen und die Ergebnisse erhalten haben, können Sie die Gesichter benennen, die zwar erfasst, aber nicht erkannt wurden. Sobald Sie ein Gesicht mit einem Namen versehen haben, werden Gesicht und Name dem Personenmodell Ihres Kontos hinzugefügt. Video Analyzer for Media erkennt dieses Gesicht dann in Ihren zukünftigen und früheren Videos.

Sie können die Video Analyzer for Media-API verwenden, um Gesichter zu bearbeiten, die in einem Video erkannt wurden. Dies wird in diesem Artikel beschrieben. Sie können auch die Video Analyzer for Media-Website verwenden (siehe Beschreibung im Artikel zum [Anpassen des Personenmodells mit der Video Analyzer for Media-Website](customize-person-model-with-api.md)).

## <a name="managing-multiple-person-models"></a>Verwalten mehrerer Personenmodelle

Video Analyzer for Media unterstützt mehrere Personenmodelle pro Konto. Dieses Feature ist derzeit nur über die Video Analyzer for Media-APIs verfügbar.

Wenn Ihr Konto verschiedenen Anwendungsszenarien entspricht, sollten Sie möglicherweise mehrere Personenmodelle pro Konto erstellen. Wenn sich Ihre Inhalte z. B. mit dem Thema „Sport“ befassen, können Sie dann für jede Sportart (Football, Basketball, Fußball usw.) ein separates Personenmodell erstellen.

Sobald ein Modell erstellt wurde, können Sie es verwenden, indem Sie die Modell-ID eines bestimmten Personenmodells beim Hochladen/Indizieren oder erneuten Indizieren eines Videos angeben. Beim Trainieren eines neuen Gesichts für ein Video wird das bestimmte benutzerdefinierte Modell, dem das Video zugeordnet war, aktualisiert.

Jedes Konto hat ein Limit von 50 Personenmodellen. Wenn Sie keine Unterstützung für Mehrpersonenmodelle benötigen, weisen Sie Ihrem Video beim Hochladen/Indizieren oder erneuten Indizieren keine Personenmodell-ID zu. In diesem Fall verwendet Video Analyzer for Media das benutzerdefinierte Standardpersonenmodell in Ihrem Konto.

## <a name="create-a-new-person-model"></a>Erstellen eines neuen Personenmodells

Um ein neues Personenmodell im angegebenen Konto zu erstellen, verwenden Sie die API zum [Erstellen eines neuen Personenmodells](https://api-portal.videoindexer.ai/api-details#api=Operations&operation=Create-Person-Model).

Die Antwort stellt den Namen und die generierte Modell-ID des Personenmodells bereit, das Sie gerade im Format des folgenden Beispiels erstellt haben.

```json
{
    "id": "227654b4-912c-4b92-ba4f-641d488e3720",
    "name": "Example Person Model"
}
```

Sie verwenden dann den Wert **id** für den Parameter **personModelId**, wenn [ein zu indizierendes Video hochgeladen](https://api-portal.videoindexer.ai/api-details#api=Operations&operation=Upload-Video) oder [ein Video neu indiziert](https://api-portal.videoindexer.ai/api-details#api=Operations&operation=Re-Index-Video) wird.

## <a name="delete-a-person-model"></a>Löschen eines Personenmodells

Um ein benutzerdefiniertes Personenmodell im angegebenen Konto zu löschen, verwenden Sie die API zum [Löschen eines Personenmodells](https://api-portal.videoindexer.ai/api-details#api=Operations&operation=Delete-Person-Model).

Sobald das Personenmodell erfolgreich gelöscht wurde, bleibt der Index Ihrer aktuellen Videos, die das gelöschte Modell verwendet haben, unverändert, bis Sie sie neu indizieren. Beim erneuten Indizieren werden die Gesichter, die im gelöschten Modell benannt wurden, von Video Analyzer for Media in Ihren aktuellen Videos, die mit diesem Modell indiziert wurden, nicht erkannt. Die Gesichter werden jedoch weiterhin ermittelt. Ihre aktuellen Videos, die mit dem gelöschten Modell indiziert wurden, verwenden nun das Standardpersonenmodell Ihres Kontos. Wenn Gesichter aus dem gelöschten Modell auch im Standardmodell Ihres Kontos benannt sind, werden diese Gesichter in den Videos weiterhin erkannt.

Es wird kein Inhalt zurückgegeben, wenn das Personenmodell erfolgreich gelöscht wurde.

## <a name="get-all-person-models"></a>Abrufen aller Personenmodelle

Um alle Personenmodelle im angegebenen Konto abzurufen, verwenden Sie die API zum [Abrufen eines Personenmodells](https://api-portal.videoindexer.ai/api-details#api=Operations&operation=Get-Person-Models).

Die Antwort enthält eine Liste aller Personenmodelle in Ihrem Konto (einschließlich des Standardpersonenmodells in dem angegebenen Konto) sowie deren Namen und IDs im Format des folgenden Beispiels.

```json
[
    {
        "id": "59f9c326-b141-4515-abe7-7d822518571f",
        "name": "Default"
    }, 
    {
        "id": "9ef2632d-310a-4510-92e1-cc70ae0230d4",
        "name": "Test"
    }
]
```

Sie können auswählen, welches Modell Sie für ein Video verwenden möchten, indem Sie den Wert `id` des Personenmodells für den Parameter `personModelId` verwenden, wenn Sie [ein zu indizierendes Video hochladen](https://api-portal.videoindexer.ai/api-details#api=Operations&operation=Upload-Video) oder [ein Video neu indizieren](https://api-portal.videoindexer.ai/api-details#api=Operations&operation=Re-Index-Video).

## <a name="update-a-face"></a>Gesicht aktualisieren

Mit diesem Befehl können Sie ein Gesicht in Ihrem Video mit einem Namen aktualisieren, indem Sie die ID des Videos und die ID des Gesichts verwenden. Durch diese Aktion wird das Personenmodell aktualisiert, dem das Video beim Hochladen/Indizieren oder erneuten Indizieren zugeordnet wurde. Wenn kein Personenmodell zugewiesen wurde, wird das Standardpersonenmodell des Kontos aktualisiert.

Das System erkennt dann die Vorkommen desselben Gesichts in Ihren anderen aktuellen Videos, die dasselbe Personenmodell verwenden. Die Erkennung des Gesichts in Ihren anderen aktuellen Videos kann einige Zeit in Anspruch nehmen, da es sich um einen Batchprozess handelt.

Sie können ein Gesicht, das von Video Analyzer for Media als Prominenten erkannt wurde, mit einem neuen Namen aktualisieren. Der neue Name, den Sie vergeben, hat Vorrang vor der integrierten Erkennung von Prominenten.

Um das Gesicht zu aktualisieren, verwenden Sie die API zum [Aktualisieren eines Gesichts im Video](https://api-portal.videoindexer.ai/api-details#api=Operations&operation=Update-Video-Face).

Namen sind für Personenmodelle eindeutig. Wenn Sie zwei verschiedenen Gesichtern in demselben Personenmodell denselben Parameterwert für `name` zuweisen, betrachtet Video Analyzer for Media die Gesichter als dieselbe Person und führt sie zusammen, nachdem Sie Ihr Video neu indizieren.

## <a name="next-steps"></a>Nächste Schritte

[Anpassen des Personenmodells mit der Video Analyzer for Media-Website](customize-person-model-with-website.md)
