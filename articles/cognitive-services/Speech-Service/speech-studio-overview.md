---
title: 'Übersicht über Speech Studio: Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: Speech Studio besteht aus einer Reihe von benutzeroberflächenbasierten Tools zum Erstellen und Integrieren von Features aus dem Azure Speech-Dienst in Ihre Anwendungen.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/07/2021
ms.author: trbye
ms.openlocfilehash: ddf7b85f5775ee25a260f19ae81e43afff95facb
ms.sourcegitcommit: f9e368733d7fca2877d9013ae73a8a63911cb88f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111902335"
---
# <a name="what-is-speech-studio"></a>Was ist Speech Studio?

[Speech Studio](https://speech.microsoft.com) besteht aus einer Reihe von benutzeroberflächenbasierten Tools zum Erstellen und Integrieren von Features aus dem Azure Speech-Dienst in Ihre Anwendungen. Sie erstellen Projekte in Speech Studio mithilfe eines Ansatzes ohne Code und verweisen dann mithilfe des [Speech SDK](speech-sdk.md), der [Speech CLI](spx-overview.md) oder verschiedener REST-APIs auf die Ressourcen, die Sie in Ihren Anwendungen erstellen.

## <a name="set-up-your-azure-account"></a>Einrichten Ihres Azure-Kontos

Sie benötigen ein Azure-Konto und ein Abonnement für die Speech-Dienste, bevor Sie [Speech Studio](https://speech.microsoft.com) verwenden zu können. Falls nicht, können Sie [den Speech-Dienst kostenlos testen](overview.md#try-the-speech-service-for-free).

> [!NOTE]
> Stellen Sie sicher, dass Sie ein Standard-Abonnement (S0) erstellen. Free-Abonnements (F0) werden nicht unterstützt.

Nachdem Sie ein Azure-Konto und ein Speech-Dienstabonnement erstellt haben:

1. Melden Sie sich in [Speech Studio](https://speech.microsoft.com) an.
1. Wählen Sie das Abonnement aus, das Sie bearbeiten möchten, und erstellen Sie ein Speech-Projekt.
1. Wenn Sie Ihr Abonnement ändern möchten, klicken Sie im oberen Menü auf die Zahnradschaltfläche.

## <a name="speech-studio-features"></a>Speech Studio-Features

Die folgenden Features des Speech-Diensts sind als Projekttypen in Speech Studio verfügbar.

* **Spracherkennung in Echtzeit**: Testen Sie die Spracherkennung schnell, indem Sie Audiodateien ziehen und ablegen, ohne Code zu verwenden. Dies ist ein Demotool, mit dem Sie sehen können, wie die Spracherkennung in Ihren Audiobeispielen funktioniert. In der [Übersicht](speech-to-text.md) für die Spracherkennung können Sie jedoch die gesamte verfügbare Funktionalität erkunden.
* **Custom Speech**: Mit Custom Speech können Sie Spracherkennungsmodelle erstellen, die auf bestimmte Vokabularsätze und Sprechstile zugeschnitten sind. Im Gegensatz zur Verwendung eines Basismodells für die Spracherkennung werden Custom Speech-Modelle Teil Ihres einzigartigen Wettbewerbsvorteils, da sie nicht öffentlich zugänglich sind. Informationen zum Hochladen von Beispielaudiodaten zum Erstellen eines Custom Speech-Modells finden Sie in der [Schnellstartanleitung](how-to-custom-speech-test-and-train.md).
* **Aussprachebewertung**: Die Aussprachebewertung bewertet die Aussprache und gibt den Rednern Feedback zur Genauigkeit und zum Redefluss der gesprochenen Audioinformationen. Speech Studio bietet eine Sandbox zum schnellen Testen dieses Features ohne Code. Weitere Informationen zur Verwendung des Features mit dem Speech SDK in Ihren Anwendungen finden Sie im Artikel zur [Schrittanleitung](how-to-pronunciation-assessment.md).
* **Sprachkatalog**: Erstellen Sie Apps und Dienste, die eine natürliche Sprache verwenden. Wählen Sie aus mehr als 170 Stimmen in über 70 Sprachen und Varianten aus. Erwecken Sie Ihre Szenarien mit sehr ausdrucksstarken und menschenähnlichen neuronalen Stimmen zum Leben.
* **Custom Voice**: Mit Custom Voice können Sie benutzerdefinierte, einmalige Stimmen für die Sprachsynthese erstellen. Sie stellen Audiodateien zur Verfügung, erstellen übereinstimmende Transkriptionen in Speech Studio und verwenden dann die benutzerdefinierten Stimmen in Ihren Anwendungen. Weitere Informationen zum Erstellen und Verwenden von benutzerdefinierten Stimmen über Endpunkte finden Sie im Artikel zur [Schrittanleitung](how-to-custom-voice-create-voice.md). 
* **Audioinhaltserstellung**: [Audioinhaltserstellung](how-to-audio-content-creation.md) ist ein benutzerfreundliches Tool, mit dem Sie sehr natürliche Audioinhalte für eine Vielzahl von Szenarien erstellen können, wie etwa Hörbücher, Nachrichtensendungen, Videoerzählungen und Chatbots. Mit Speech Studio können Sie Ihre erstellten Audiodateien exportieren, um sie in Ihren Anwendungen zu verwenden.
* **Benutzerdefiniertes Schlüsselwort**: Ein benutzerdefiniertes Schlüsselwort ist ein Wort oder ein kurzer Ausdruck, mit dem Ihr Produkt per Sprache aktiviert werden kann. Sie erstellen ein benutzerdefiniertes Schlüsselwort in Speech Studio und generieren dann eine Binärdatei für [die Verwendung mit dem Speech SDK](custom-keyword-basics.md) in Ihren Anwendungen.
* **Benutzerdefinierte Befehle**: Mit benutzerdefinierten Befehlen ist es einfach, funktionsreiche Apps mit Sprachsteuerung zu erstellen, die für Voice-First-Interaktionserfahrungen optimiert sind. Sie bieten eine Benutzeroberfläche für die Erstellung ohne Code in Speech Studio, ein automatisches Hostingmodell und eine relativ geringe Komplexität, sodass Sie sich auf die Entwicklung der besten Lösung für Ihre Szenarien mit Sprachsteuerung konzentrieren können. Weitere Informationen finden Sie in der [Schrittanleitung](how-to-develop-custom-commands-application.md) zum Erstellen von Anwendungen für benutzerdefinierte Befehle und auch im Leitfaden zum [Integrieren Ihrer „Benutzerdefinierte Befehle“-Anwendung in das Speech SDK](how-to-custom-commands-setup-speech-sdk.md).

## <a name="next-steps"></a>Nächste Schritte

[Erkunden Sie Speech Studio](https://speech.microsoft.com), und erstellen Sie ein Projekt.




