---
title: Hochladen von Dateien in ein Azure Media Services-Konto über Azure StorSimple | Microsoft-Dokumentation
description: Dieser Artikel enthält eine kurze Übersicht über Azure StorSimple Data Manager. Darüber hinaus enthält der Artikel Links zu Tutorials, die zeigen, wie Sie Daten aus StorSimple extrahieren und als Objekte in ein Azure Media Services-Konto hochladen.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.assetid: 1dd09328-262b-43ef-8099-73241b49a925
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 3/10/2021
ms.author: inhenkel
ms.openlocfilehash: 9fb1dd99a86dcdda515d77d7a051ca88df26f089
ms.sourcegitcommit: e6de87b42dc320a3a2939bf1249020e5508cba94
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/27/2021
ms.locfileid: "114712139"
---
# <a name="upload-files-into-an-azure-media-services-account-from-azure-storsimple"></a>Hochladen von Dateien in ein Azure Media Services-Konto über Azure StorSimple 

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)] 

[!INCLUDE [v2 deprecation notice](../latest/includes/v2-deprecation-notice.md)]
>
> 
> Azure StorSimple Data Manager befindet sich derzeit in der privaten Vorschauphase. 
> 

## <a name="overview"></a>Übersicht

In Media Services laden Sie Ihre digitalen Dateien in ein Medienobjekt hoch. Das Medienobjekt kann Videos, Audiodateien, Bilder, Miniaturansichtssammlungen, Texttitel und Untertiteldateien (und die Metadaten zu diesen Dateien) enthalten. Nachdem die Dateien hochgeladen wurden, werden Ihre Inhalte zur weiteren Verarbeitung und zum Streaming sicher in der Cloud gespeichert.

[Azure StorSimple](../../storsimple/index.yml) verwendet den Cloudspeicher als Erweiterung der lokalen Lösung und verteilt die Daten automatisch auf die Ebenen des lokalen Speichers und des Cloudspeichers. Vor dem Senden an die Cloud werden Ihre Daten vom StorSimple-Gerät dedupliziert und komprimiert, sodass sich umfangreiche Dateien äußerst effizient an die Cloud senden lassen. Der Dienst [StorSimple Data Manager](../../storsimple/storsimple-data-manager-overview.md) stellt APIs bereit, mit deren Hilfe Sie Daten aus StorSimple extrahieren und als AMS-Objekte darstellen können.

## <a name="get-started"></a>Erste Schritte

1. [Erstellen Sie ein Media Services-Konto](media-services-portal-create-account.md), in das Sie die Objekte übertragen möchten.
2. Registrieren Sie sich für die Data Manager-Vorschau, wie im Artikel [StorSimple Data Manager](../../storsimple/storsimple-data-manager-overview.md) beschrieben.
3. Erstellen Sie ein StorSimple Data Manager-Konto.
4. Erstellen Sie einen Datentransformationsauftrag, der Daten aus einem StorSimple-Gerät extrahiert und sie als Objekte in ein AMS-Konto überträgt. 

    Wenn die Ausführung des Auftrags beginnt, wird eine Speicherwarteschlange erstellt. Diese Warteschlange wird mit Nachrichten über transformierte Blobs aufgefüllt, wenn diese bereit sind. Der Name dieser Warteschlange entspricht dem Namen der Auftragsdefinition. Anhand dieser Warteschlange können Sie ermitteln, wann ein Objekt bereit ist, und den gewünschten Media Services-Vorgang aufrufen, um ihn für das Objekt auszuführen. So können Sie mithilfe dieser Warteschlange beispielsweise eine Azure-Funktion auslösen, die den erforderlichen Media Services-Code enthält.

## <a name="see-also"></a>Weitere Informationen

[Verwenden des .NET SDK zum Initiieren einer Datentransformation (private Vorschau)](../../storsimple/storsimple-data-manager-dotnet-jobs.md)

## <a name="media-services-learning-paths"></a>Media Services-Lernpfade
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Nächste Schritte

Sie können nun Ihre hochgeladenen Medienobjekte codieren. Weitere Informationen finden Sie unter [Codieren von Medienobjekten](media-services-portal-encode.md).
