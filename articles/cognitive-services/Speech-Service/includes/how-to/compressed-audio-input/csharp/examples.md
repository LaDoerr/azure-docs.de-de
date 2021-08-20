---
author: laujan
ms.service: cognitive-services
ms.topic: include
ms.date: 03/09/2020
ms.author: lajanuar
ms.custom: devx-track-csharp
ms.openlocfilehash: e6a11521cd0694bd46a37f830e0f61105e4d7a91
ms.sourcegitcommit: e7d500f8cef40ab3409736acd0893cad02e24fc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122262969"
---
Wenn Sie das Speech-SDK so konfigurieren möchten, dass komprimierte Audioeingaben akzeptiert werden, erstellen Sie `PullAudioInputStream` oder `PushAudioInputStream`. Erstellen Sie dann eine `AudioConfig` aus einer Instanz Ihrer stream-Klasse, und geben Sie dabei das Komprimierungsformat des Streams an. Entsprechende Beispielcodeausschnitte finden Sie unter [Informationen zur API für Audioeingabestreams des Speech SDK](../../../../how-to-use-audio-input-streams.md).

Angenommen, Sie verfügen über die Eingabestreamklasse `pushStream` und verwenden OPUS/OGG. Ihr Code könnte wie folgt aussehen:

```csharp
using Microsoft.CognitiveServices.Speech;
using Microsoft.CognitiveServices.Speech.Audio;

// ... omitted for brevity

var speechConfig =
    SpeechConfig.FromSubscription(
        "YourSubscriptionKey",
        "YourServiceRegion");

// Create an audio config specifying the compressed
// audio format and the instance of your input stream class.
var audioFormat =
    AudioStreamFormat.GetCompressedFormat(
        AudioStreamContainerFormat.OGG_OPUS);
var audioConfig =
    AudioConfig.FromStreamInput(
        pushStream,
        audioFormat);

using var recognizer = new SpeechRecognizer(speechConfig, audioConfig);
var result = await recognizer.RecognizeOnceAsync();

var text = result.Text;
```