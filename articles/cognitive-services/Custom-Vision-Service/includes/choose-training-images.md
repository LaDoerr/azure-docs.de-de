---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: include
ms.date: 07/17/2019
ms.author: pafarley
ms.openlocfilehash: 68965017dc3f6d062f5a5c18acb1774d38e385c4
ms.sourcegitcommit: 20abee54e48f9b40b83d39c5b970bd0193812cb6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2021
ms.locfileid: "113659583"
---
Es wird empfohlen, im ersten Trainingssatz mindestens 30 Bilder pro Tag zu verwenden. Sie sollten auch einige zusätzliche Bilder sammeln, um Ihr Modell zu testen, nachdem es trainiert wurde.

Verwenden Sie zum effektiven Trainieren Ihres Modells Bilder mit optischer Vielfalt. Wählen Sie Bilder aus, die sich nach folgenden Aspekten unterscheiden:
* Kamerawinkel
* Belichtung
* background
* Visueller Stil
* Einzelne/gruppierte Motive
* size
* type

Stellen Sie außerdem sicher, dass alle Ihre Trainingsbilder die folgenden Kriterien erfüllen:
* JPG-, PNG-, BMP- oder GIF-Format
* Höchstens 6 MB groß (4 MB für Vorhersagebilder)
* Mindestens 256 Pixel an der kürzesten Seite; kürzere Bilder werden von Custom Vision Service automatisch hochskaliert.

> [!NOTE]
> Benötigen Sie für Ihr Training ein größeres Spektrum von Bildern? Mit Trove, einem Microsoft Garage-Projekt, können Sie Bilder zu Trainingszwecken sammeln und erwerben. Die gesammelten Bilder können Sie dann herunterladen und wie gewohnt in Ihr Custom Vision-Projekt importieren. Weitere Informationen finden Sie unter [Trove](https://www.microsoft.com/en-us/ai/trove?activetab=pivot1:primaryr3).
