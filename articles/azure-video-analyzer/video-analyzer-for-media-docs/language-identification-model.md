---
title: Verwenden von Azure Video Analyzer for Media (früher Video Indexer) zum automatischen Identifizieren gesprochener Sprachen – Azure
titleSuffix: Azure Video Analyzer for Media
description: In diesem Artikel wird beschrieben, wie das Modell zur Sprachenerkennung von Azure Video Analyzer for Media (früher Video Indexer) verwendet wird, um in einem Video automatisch die gesprochene Sprache zu identifizieren.
services: azure-video-analyzer
author: juliako
manager: femila
ms.topic: article
ms.subservice: azure-video-analyzer-media
ms.date: 04/12/2020
ms.author: ellbe
ms.openlocfilehash: 8caada82b0c4b0c99e2d18b8586cc0b2c9d575f4
ms.sourcegitcommit: 0af634af87404d6970d82fcf1e75598c8da7a044
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/15/2021
ms.locfileid: "112119540"
---
# <a name="automatically-identify-the-spoken-language-with-language-identification-model"></a>Automatisches Identifizieren von gesprochener Sprache mit dem Modell zur Sprachenerkennung

Azure Video Analyzer for Media (früher Video Indexer) unterstützt die automatische Identifizierung von Sprachen (Language Identification, LID). Bei diesem Prozess wird gesprochene Sprache für Audiodaten automatisch identifiziert, und die Mediendatei wird dann zur Transkription in der vorherrschenden Sprache übermittelt. 

LID unterstützt derzeit folgende Sprachen: Chinesisch (Mandarin), Deutsch, Englisch, Französisch, Italienisch, Japanisch, Portugiesisch (Brasilien), Russisch und Spanisch. 

Lesen Sie unbedingt den Abschnitt [Richtlinien und Einschränkungen](#guidelines-and-limitations) weiter unten.

## <a name="choosing-auto-language-identification-on-indexing"></a>Auswählen der automatischen Sprachidentifizierung bei der Indizierung

Wählen Sie beim Indizieren bzw. [erneuten Indizieren](https://api-portal.videoindexer.ai/api-details#api=Operations&operation=Re-Index-Video) eines Videos mit der API die Option `auto detect` für den Parameter `sourceLanguage`.

Navigieren Sie bei Verwendung des Portals auf der Startseite von [Video Analyzer for Media](https://www.videoindexer.ai/) zu Ihren **Kontovideos**, und zeigen Sie auf den Namen des Videos, das Sie neu indizieren möchten. Klicken Sie unten rechts auf die Schaltfläche „Erneut indizieren“. Wählen Sie im Dialogfeld **Video erneut indizieren** im Dropdownfeld **Ausgangssprache des Videos** die Option *Automatische Erkennung*.

![Automatische Erkennung](./media/language-identification-model/auto-detect.png)

## <a name="model-output"></a>Modellausgabe

Video Analyzer for Media führt für das Video die Transkription in die Sprache mit dem höchsten Wahrscheinlichkeitswert durch, wenn der Zuverlässigkeitsgrad `> 0.6` ist. Falls die Sprache nicht zuverlässig identifiziert werden kann, wird angenommen, dass die gesprochene Sprache Englisch ist. 

Die dominante Sprache für das Modell ist im Insights-JSON-Code als `sourceLanguage`-Attribut verfügbar (unter „root/videos/insights“). Eine entsprechende Zuverlässigkeitsbewertung ist auch im `sourceLanguageConfidence`-Attribut verfügbar.

```json
"insights": {
        "version": "1.0.0.0",
        "duration": "0:05:30.902",
        "sourceLanguage": "fr-FR",
        "language": "fr-FR",
        "transcript": [...],
        . . .
        "sourceLanguageConfidence": 0.8563
      },
```

## <a name="guidelines-and-limitations"></a>Richtlinien und Einschränkungen

* Die automatische Identifizierung von Sprachen unterstützt die folgenden Sprachen: 

    Chinesisch (Mandarin), Deutsch, Englisch, Französisch, Italienisch, Japanisch, Portugiesisch (Brasilien), Russisch und Spanisch.
* Video Analyzer for Media unterstützt zwar auch Arabisch (modernes Standard-Arabisch und Levantine), Hindi und Koreanisch, aber diese Sprachen werden in LID nicht unterstützt.
* Falls die Audiodaten andere Sprachen enthalten, die nicht in der obigen Liste mit den unterstützten Sprachen aufgeführt sind, kommt es zu einem unerwarteten Ergebnis.
* Falls die Sprache von Video Analyzer for Media nicht mit einem ausreichenden Zuverlässigkeitsgrad (`>0.6`) identifiziert werden kann, wird als Fallbacksprache Englisch verwendet.
* Audiodaten mit einer Mischung von Sprachen werden derzeit nicht unterstützt. Falls die Audiodaten eine Sprachenmischung enthalten, kommt es zu einem unerwarteten Ergebnis. 
* Wenn die Audiodaten eine geringe Qualität aufweisen, kann sich dies auf die Ergebnisse des Modells auswirken.
* Die Audiodaten müssen für das Modell mindestens eine Minute mit gesprochener Sprache enthalten.
* Das Modell ist so konzipiert, dass eine spontane Unterhaltung erkannt werden kann (keine Sprachbefehle, Gesang usw.).

## <a name="next-steps"></a>Nächste Schritte

* [Übersicht](video-indexer-overview.md)
* [Automatisches Erkennen und Transkribieren mehrsprachiger Inhalte](multi-language-identification-transcription.md)
