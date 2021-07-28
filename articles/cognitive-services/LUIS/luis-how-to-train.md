---
title: 'Trainieren von Apps: LUIS'
titleSuffix: Azure Cognitive Services
description: Sie trainieren die Version Ihrer LUIS-App, indem Sie ihr Fähigkeiten vermitteln, mit denen sie natürliche Sprache besser verstehen kann. Trainieren Sie Ihre LUIS-App, nachdem Sie Ihr Modell aktualisiert haben, indem Sie Entitäten, Absichten und Äußerungen hinzufügen, bearbeiten, bezeichnen oder löschen.
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: how-to
ms.date: 11/15/2019
ms.openlocfilehash: 0ff8376eea2eca459b93adbf46b81dbeb7639d40
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111954334"
---
# <a name="train-your-active-version-of-the-luis-app"></a>Trainieren Ihrer aktiven Version der LUIS-App

Sie trainieren Ihre LUIS-App, indem Sie ihr Fähigkeiten vermitteln, mit denen sie natürliche Sprache besser verstehen kann. Trainieren Sie Ihre LUIS-App, nachdem Sie Ihr Modell aktualisiert haben, indem Sie Entitäten, Absichten und Äußerungen hinzufügen, bearbeiten, bezeichnen oder löschen.

Das Trainieren und [Testen](./luis-interactive-test.md) eine App ist ein iterativer Vorgang. Nachdem Sie Ihre LUIS-App trainiert haben, sollten Sie sie mit Beispieläußerungen testen, um festzustellen, ob die Absichten und Entitäten ordnungsgemäß erkannt werden. Ist das nicht der Fall, aktualisieren Sie die LUIS-App, und trainieren und testen Sie sie erneut.

Das Training wird auf die aktive Version im LUIS-Portal angewandt.

## <a name="how-to-train-interactively"></a>Interaktives Trainieren

Sie müssen Ihre LUIS-App mindestens zunächst mindestens einmal im [LUIS-Portal](https://www.luis.ai) trainieren, damit Sie den iterativen Prozess starten können. Vergewissern Sie sich vor dem Training, dass jeder Absicht mindestens eine Äußerung zugeordnet ist.

1. Greifen Sie auf Ihre App zu, indem Sie ihren Namen auf der Seite **Meine Apps** auswählen.

1. Klicken Sie in Ihrer App im oberen Bereich auf **Trainieren**.

1. Wenn das Training abgeschlossen ist, wird im oberen Bereich des Browsers eine Benachrichtigung angezeigt.

## <a name="training-date-and-time"></a>Datum und Uhrzeit für das Training

Das Datum und die Uhrzeit für das Training werden in der Zeitzone GMT + 2 angegeben.

## <a name="train-with-all-data"></a>Trainieren mit allen Daten

Das Training verwendet einen kleinen Prozentsatz negativer Stichproben. Sie können stattdessen alle verfügbaren Daten verwenden, indem Sie entweder das Portal oder die API verwenden. 

### <a name="using-the-luis-portal"></a>Mithilfe des LUIS-Portals

Melden Sie sich beim [LUIS-Portal](https://www.luis.ai/) an, und klicken Sie auf Ihre App. Wählen Sie oben auf dem Bildschirm **Verwalten** und dann **Einstellungen** aus, und aktivieren oder deaktivieren Sie die Option **Use non-deterministic training** (Nicht deterministisches Training verwenden). Wenn diese Option deaktiviert ist, werden alle verfügbaren Daten im Training verwendet.

![Eine Schaltfläche zum Aktivieren oder Deaktivieren nicht deterministischer Trainings](./media/non-determinstic-training.png)

### <a name="using-the-version-settings-api"></a>Verwenden der API für Versionseinstellungen

Verwenden Sie die [Versionseinstellungs-API](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/versions-update-application-version-settings) mit der Option `UseAllTrainingData` (auf TRUE festgelegt), um dieses Feature zu deaktivieren.

## <a name="unnecessary-training"></a>Unnötiges Trainieren

Sie müssen nicht nach jeder einzelnen Änderung trainieren. Trainings sollten durchgeführt werden, nachdem mehrere Änderungen auf das Modell angewandt wurden und Sie im nächsten Schritt testen oder veröffentlichen möchten. Wenn Sie weder testen noch veröffentlichen möchten, ist kein Training erforderlich.

## <a name="training-with-the-rest-apis"></a>Trainieren mit der REST-API

Das Trainieren umfasst im LUIS-Portal einen einzigen Schritt – das Klicken auf die Schaltfläche **Trainieren**. Mit der REST-APIs sind zwei Schritte erforderlich. Die erste besteht darin, mit einer HTTP POST-Anforderung das [Trainieren anzufordern](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c45). Fragen Sie dann den [Trainingsstatus](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c46) mit HTTP Get ab.

Um zu ermitteln, wann das Training abgeschlossen ist, müssen Sie den Status abrufen, bis alle Modelle erfolgreich trainiert wurden.

## <a name="next-steps"></a>Nächste Schritte

* [Interaktives Testen](luis-interactive-test.md)
* [Batchtests](luis-how-to-batch-test.md)