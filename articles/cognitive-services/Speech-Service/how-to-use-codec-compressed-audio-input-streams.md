---
title: Streamen von per Codec komprimierten Audiodaten mit dem Speech SDK – Speech-Dienst
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie mit dem Speech SDK komprimierte Audiodaten an den Speech-Dienst streamen. Verfügbar für C++, C# und Java für Linux, Java in Android und Objective-C in iOS.
services: cognitive-services
author: amitkumarshukla
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: amishu
ms.custom: devx-track-csharp
zone_pivot_groups: programming-languages-set-twenty-two
ms.openlocfilehash: 183679eb9c4eb2480736dfb4cec8a343c67d0ed7
ms.sourcegitcommit: 67cdbe905eb67e969d7d0e211d87bc174b9b8dc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111854484"
---
# <a name="use-codec-compressed-audio-input"></a>Verwenden der per Codec komprimierten Audioeingabe

Das Speech SDK und die Speech CLI können komprimierte Audioformate mithilfe von GStreamer akzeptieren. Die Audiodaten werden von GStreamer dekomprimiert, bevor sie als unformatiertes PCM an den Speech-Dienst gesendet werden.

Plattform | Sprachen | Unterstützte GStreamer-Version
| :--- | ---: | :---:
Windows (ohne universelle Windows-Plattform)  | C++, C#, Java, Python | [1.18.3](https://gstreamer.freedesktop.org/data/pkg/windows/1.18.3/)
Linux  | C++, C#, Java, Python | [Unterstützte Linux-Distributionen und Zielarchitekturen](~/articles/cognitive-services/speech-service/speech-sdk.md)
Android  | Java | [1.18.3](https://gstreamer.freedesktop.org/data/pkg/android/1.18.3/)

## <a name="speech-sdk-version-required-for-compressed-audio-input"></a>Erforderliche Speech SDK-Version für komprimierte Audioeingaben
* Das Speech SDK, Version 1.10.0 oder höher ist für RHEL 8 und CentOS 8 erforderlich.
* Das Speech SDK, Version 1.11.0 oder höher ist für Windows erforderlich.
* Speech SDK Version 1.16.0 oder höher für die neueste GStreamer-Instanz unter Windows und Android.

[!INCLUDE [supported-audio-formats](includes/supported-audio-formats.md)]

## <a name="gstreamer-required-to-handle-compressed-audio"></a>GStreamer ist zum Verarbeiten komprimierter Audiodaten erforderlich.

::: zone pivot="programming-language-csharp"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/csharp/prerequisites.md)]
::: zone-end

::: zone pivot="programming-language-cpp"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/cpp/prerequisites.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/java/prerequisites.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/python/prerequisites.md)]
::: zone-end

## <a name="example-code-using-codec-compressed-audio-input"></a>Beispielcode für die Verwendung von per Codec komprimierter Audioeingabe

::: zone pivot="programming-language-csharp"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/csharp/examples.md)]
::: zone-end

::: zone pivot="programming-language-cpp"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/cpp/examples.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/java/examples.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/python/examples.md)]
::: zone-end

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Schnellstart: Erkennen von Spracheingaben per Mikrofon](./get-started-speech-to-text.md)
