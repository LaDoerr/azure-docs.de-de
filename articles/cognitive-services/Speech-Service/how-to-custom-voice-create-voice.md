---
title: Erstellen einer benutzerdefinierten Stimme – Spracherkennungsdienst
titleSuffix: Azure Cognitive Services
description: Wenn Sie zum Hochladen Ihrer Daten bereit sind, navigieren Sie zum Custom Voice-Portal. Erstellen Sie ein Custom Voice-Projekt, oder wählen Sie ein bereits vorhandenes Projekt aus. Das Projekt muss die gleiche Sprache/das gleiche Gebietsschema und die gleichen Geschlechtseigenschaften besitzen wie die Daten, die Sie für Ihr Stimmtraining verwenden möchten.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: erhopf
ms.openlocfilehash: 449975a6f0b5799ce93dcb31f42e1a43a1d183f3
ms.sourcegitcommit: c385af80989f6555ef3dadc17117a78764f83963
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2021
ms.locfileid: "111412131"
---
# <a name="create-and-use-your-voice-model"></a>Erstellen und Verwenden Ihres Sprachmodells

Unter [Vorbereiten von Trainingsdaten](how-to-custom-voice-prepare-data.md) haben Sie mehr über die verschiedenen Datentypen erfahren, die Sie zum Trainieren einer benutzerdefinierten neuronalen Stimme und die verschiedenen Formatanforderungen verwenden können. Nachdem Sie Ihre Daten und die aufgezeichnete Erklärung des Sprechers vorbereitet haben, können Sie sie in [Speech Studio](https://aka.ms/custom-voice-portal) hochladen. In diesem Artikel erfahren Sie, wie Sie eine benutzerdefinierte neuronale Stimme über das Speech Studio-Portal trainieren. Hier finden Sie Informationen zu den [unterstützten Sprachen](language-support.md#customization) für die benutzerdefinierte neuronale Stimme.

## <a name="prerequisites"></a>Voraussetzungen

* Schließen Sie [Erste Schritte mit „Benutzerdefinierte neuronale Stimme“](how-to-custom-voice.md) ab.
* [Vorbereiten von Trainingsdaten](how-to-custom-voice-prepare-data.md)

## <a name="set-up-voice-talent"></a>Einrichten eines Sprechers

Ein Sprecher ist eine Einzelperson oder Zielsprecher, dessen Stimmen aufgezeichnet und verwendet werden, um neuronale Stimmmodelle zu erstellen. Bevor Sie eine Stimme erstellen, definieren Sie Ihre Stimmpersona, und wählen Sie einen geeigneten Sprecher aus. Weitere Informationen zum Aufzeichnen von Stimmbeispielen finden Sie im [Tutorial](record-custom-voice-samples.md).

Um eine neuronale Stimme zu trainieren, müssen Sie ein Sprecherprofil mit einer Audiodatei erstellen, die vom Sprecher aufgezeichnet wurde, der der Verwendung seiner Sprachdaten zum Trainieren eines benutzerdefinierten Stimmmodells zugestimmt hat. Stellen Sie beim Vorbereiten des Aufzeichnungsskripts sicher, dass Sie den folgenden Satz einschließen:

**„Ich [Vor- und Nachname nennen] akzeptiere, dass die Aufzeichnungen meiner Stimme von [Name des Unternehmens nennen] verwendet werden, um eine synthetische Version meiner Stimme zu erstellen und diese zu verwenden.“**
Dieser Satz wird verwendet, um zu überprüfen, ob die Trainingsdaten mit den Audiodaten in der Einverständniserklärung übereinstimmen. > Weitere Informationen zur [Überprüfung des Sprechers](/legal/cognitive-services/speech-service/custom-neural-voice/data-privacy-security-custom-neural-voice?context=%2fazure%2fcognitive-services%2fspeech-service%2fcontext%2fcontext)

> [!NOTE]
> Das Feature „Benutzerdefinierte neuronale Stimme“ ist mit eingeschränktem Zugriff verfügbar. Stellen Sie sicher, dass Sie die [Anforderungen für verantwortungsvolle KI](/legal/cognitive-services/speech-service/custom-neural-voice/limited-access-custom-neural-voice?context=%2fazure%2fcognitive-services%2fspeech-service%2fcontext%2fcontext) kennen, und [beantragen Sie dann den Zugriff](https://aka.ms/customneural). 

Bei den folgenden Schritten wird davon ausgegangen, dass Sie die Dateien für die aufgezeichnete Erklärung der Sprecher vorbereitet haben.  Wechseln Sie zu [Speech Studio](https://aka.ms/custom-voice-portal), um ein Projekt für eine benutzerdefinierte neuronale Stimme auszuwählen, und führen Sie dann die folgenden Schritte aus, um ein Sprecherprofil zu erstellen.

1. Navigieren Sie zu **Sprachsynthese** > **Custom Voice** > **Projekt auswählen** > **Sprecher einrichten**.

2. Klicken Sie auf **Sprecher hinzufügen**.

3. Klicken Sie als nächstes zum Definieren von Stimmmerkmalen auf das zu verwendende **Zielszenario**. Beschreiben Sie dann Ihre **Stimmmerkmale**.

> [!NOTE]
> Die von Ihnen bereitgestellten Szenarien müssen mit dem übereinstimmen, was Sie im Antragsformular beantragt haben.

4. Wechseln Sie dann zu **Upload voice talent statement** (Sprecheraussage hochladen), und befolgen Sie die Anweisungen zum Hochladen der Sprecheraussage, die Sie zuvor vorbereitet haben.

> [!NOTE]
> Stellen Sie sicher, dass die mündliche Aussage mit denselben Einstellungen wie Ihre Trainingsdaten aufgezeichnet wird, einschließlich der Aufzeichnungsumgebung und der Sprechweise.

5. Wechseln Sie abschließend zu **Überprüfen und übermitteln**. Sie können die Einstellungen überprüfen und auf **Übermitteln** klicken.

## <a name="upload-your-datasets"></a>Hochladen von Datasets

Wenn Sie bereit sind, Ihre Daten hochzuladen, wechseln Sie zur Registerkarte zum **Vorbereiten von Trainingsdaten**, um Ihren ersten Trainingssatz hinzuzufügen und Daten hochzuladen.  Ein Trainingssatz besteht aus einer Reihe von Audioäußerungen und ihren Zuordnungsskripts, die zum Trainieren eines Stimmmodells verwendet werden. Sie können einen Trainingssatz verwenden, um Ihre Trainingsdaten zu organisieren. Die Überprüfung der Datenbereitschaft erfolgt pro Trainingssatz. Sie können mehrere Datasets in einen Trainingssatz importieren.

Gehen Sie wie folgt vor, um Ihre Trainingsdaten zu erstellen und zu überprüfen.

1. Klicken Sie auf der Registerkarte **Trainingsdaten vorbereiten** auf **Trainingssatz hinzufügen** zum Eingeben von **Name** und **Beschreibung** > **Erstellen**, um einen neuen Trainingssatz hinzuzufügen.

   Wenn der Trainingssatz erfolgreich erstellt wurde, können Sie mit dem Hochladen Ihrer Daten beginnen. 

2. Klicken Sie zum Hochladen von Daten auf **Daten hochladen** > **Datentyp auswählen** > **Daten hochladen** und **Zieltrainingssatz angeben** > Geben Sie **Name** und **Beschreibung** für Ihr Dataset ein > Überprüfen Sie die Einstellungen, und klicken Sie auf **Upload**.

> [!NOTE]
>- Doppelte Audionamen werden aus dem Training entfernt. Achten Sie darauf, dass die ausgewählten Datasets in der ZIP-Datei oder in mehreren ZIP-Dateien nicht die gleichen Audionamen enthalten. Wenn Äußerungs-IDs (entweder in Audio- oder Skriptdateien) doppelt vorhanden sind, werden sie abgelehnt.
>- Wenn Sie Datasets in der vorherigen Version von Speech Studio erstellt haben, müssen Sie einen Trainingssatz für Ihre Datasets angeben, bevor Sie sie verwenden. Andernfalls wird ein Ausrufezeichen an den Datasetnamen angefügt, und das Dataset konnte nicht verwendet werden.

Jedes hochgeladene Dataset muss den Anforderungen für den ausgewählten Datentyp entsprechen. Es ist wichtig, dass Sie Ihre Daten ordnungsgemäß formatieren, bevor sie hochgeladen werden. Dadurch wird sichergestellt, dass die Daten vom Dienst „Benutzerdefinierte neuronale Stimme“ verarbeitet werden. Wechseln Sie zu [Trainingsdaten vorbereiten](how-to-custom-voice-prepare-data.md), und stellen Sie sicher, dass Ihre Daten ordnungsgemäß formatiert sind.

> [!NOTE]
> - Benutzer mit einem Standard-Abonnement (S0) können fünf Datasets gleichzeitig hochladen. Wenn Sie das Limit erreicht wurde, warten Sie, bis der Importvorgang mindestens eines Datasets beendet wurde. Versuchen Sie es anschließend noch mal.
> - Benutzer des kostenlosen Abonnements (F0) können maximal 10 ZIP-Dateien pro Abonnement als Datasets importieren. Bei Benutzern des Standardabonnements (S0) beträgt die Höchstzahl 500.

Datasets werden nach dem Klicken auf **Hochladen** automatisch überprüft. Bei der Datenüberprüfung werden Format, Größe und Samplingrate der Audiodateien geprüft. Sollten Fehler vorliegen, beheben Sie sie, und übermitteln Sie die Daten erneut. 

Nachdem die Daten hochgeladen wurden, können Sie die Details in der Detailansicht des Trainingssatzes überprüfen. Auf der Registerkarte **Übersicht** können Sie die Aussprachebewertungen und den Rauschpegel der einzelnen Datasets genauer überprüfen. Die Aussprachebewertung liegt zwischen 0 und 100. Werte unter 70 deuten in der Regel auf einen Aussprachefehler oder auf einen Fehler bei der Skriptzuordnung hin. Ein starker Akzent kann die Aussprachebewertung verringern und die Qualität der erzeugten digitalen Stimme negativ beeinflussen.

Ein höheres Signal-Rausch-Verhältnis (SNR) deutet auf ein geringeres Rauschen in den Audioaufnahmen hin. Wenn Ihre Aufnahmen in einem professionellen Tonstudio vorgenommen wurden, können Sie SNR-Werte größer als 50 erzielen. Ein SNR-Wert unter 20 kann dazu führen, dass in der generierten Stimme das Rauschen deutlich zu hören ist.

Bei einer niedrigen Aussprachebewertung oder einem geringen SNR-Wert sollten Sie die betroffenen Äußerungen erneut aufnehmen. Wenn die erneute Aufzeichnung nicht möglich ist, erwägen Sie den Ausschluss dieser Äußerungen aus dem Dataset.

Unter **Datendetails** können Sie die Datendetails des Trainingssatzes überprüfen. Befolgen Sie bei typischen Problemen mit den Daten die Anweisungen in der angezeigten Meldung, um sie vor dem Training zu beheben.

Die Probleme sind in drei Typen unterteilt. Beziehen Sie sich auf die folgenden drei Tabellen, um die jeweiligen Fehlertypen zu überprüfen.

Die erste Art von Fehlern, die in der folgenden Tabelle aufgeführt sind, muss manuell behoben werden. Andernfalls werden die Daten mit diesen Fehlern während des Trainings ausgeschlossen. 

| Category | Name | BESCHREIBUNG | Vorschlag |
| --------- | ----------- | ----------- | --------------------------- |
| Skript | Ungültiges Trennzeichen| Diese Skriptzeilen weisen kein gültiges Trennzeichen auf (TAB: {}).| Verwenden Sie TAB, um ID und Inhalt zu trennen.|
| Skript | Ungültige Skript-ID| Das Skript-ID-Format ist ungültig.| Die Skriptzeilen-ID sollte numerisch sein.|
| Skript | Skriptinhalt dupliziert| Der Skriptinhalt von Zeile {} wird mit der Zeile {} dupliziert.| Der Inhalt der Skriptzeile sollte eindeutig sein.|
| Skript | Skriptinhalt zu lang| Der Inhalt der Skriptzeile ist länger als maximal 1.000 Zeichen.| Die Länge des Skriptzeileninhalts sollte weniger als 1.000 Zeichen umfassen.|
| Skript | Skript besitzt keine übereinstimmenden Audiodaten| Die Skriptzeilen-ID enthält keine übereinstimmenden Audiodaten.| Die Skriptzeilen-ID muss mit der Audio-ID übereinstimmen.|
| Skript | Kein gültiges Skript| In diesem Dataset wurde kein gültiges Skript gefunden.| Korrigieren Sie die problematischen Skriptzeilen gemäß der detaillierten Problemliste.|
| Audio | Audiodaten besitzen kein übereinstimmendes Skript| Die Audiodatei stimmt nicht mit der Skript-ID überein.| Der WAV-Dateiname sollte mit der ID in der Skriptdatei übereinstimmen.|
| Audio | Ungültiges Audioformat| Die WAV-Datei weist ein ungültiges Format auf und kann nicht gelesen werden.| Überprüfen Sie das WAV-Dateiformat mit einem Audiotool wie sox.|
| Audio | Niedrige Samplingrate| Die Audiosamplingrate ist niedriger als 16 KHz. | Die WAV-Dateisamplingrate sollte gleich oder höher als 16 KHz sein. |
| Audio | Audiodauer zu lang| Die Audiodauer ist länger als 30 Sekunden.| Teilen Sie lange Audioinhalte in mehrere Dateien auf, um sicherzustellen, dass jede Datei weniger als 15 Sekunden lang ist.|
| Audio | Keine gültigen Audiodaten| In diesem Dataset wurden keine gültigen Audiodaten gefunden.| Korrigieren Sie die problematischen Audiodaten gemäß der detaillierten Problemliste.|

Die zweite Art von Fehlern, die in der folgenden Tabelle aufgeführt sind, wird automatisch behoben, aber es wird empfohlen, die korrigierten Daten zu überprüfen. 

| Category | Name | BESCHREIBUNG | Vorschlag |
| --------- | ----------- | ----------- | --------------------------- |
| Audio | Stereo-Audio | Für das TTS-Modelltraining wird nur ein Kanal in Stereo-Audio verwendet.|     Verwenden Sie „Mono“ bei der TTS-Aufzeichnung oder Datenaufbereitung. Diese Audiodaten werden in Mono konvertiert. Laden Sie das normalisierte Dataset herunter, und überprüfen Sie es.|
| Volume | Lautstärkespitze außerhalb des Bereichs |Die Lautstärkespitze liegt nicht im Bereich von -3 dB (70 % der maximalen Lautstärke) bis -6 dB (50 %). Sie wird jetzt automatisch auf -4 dB (65 %) angepasst.|  Steuern Sie die Lautstärkespitze während der Aufzeichnung oder Datenaufbereitung im richtigen Bereich. Diese Audiodaten werden linear skaliert, um den Spitzenbereich zu entsprechen. Laden Sie das normalisierte Dataset herunter, und überprüfen Sie es.|
|Konflikt | Lange Stille vor dem ersten Wort erkannt | Vor dem ersten Wort wurde eine lange Stille erkannt.| Die Startstille wird auf 200 ms gekürzt. Laden Sie das normalisierte Dataset herunter, und überprüfen Sie es. |
| Konflikt | Lange Stille nach dem letzten Wort erkannt | Nach dem letzten Wort wurde eine lange Stille erkannt. | Die Endstille wird auf 200 ms gekürzt. Laden Sie das normalisierte Dataset herunter, und überprüfen Sie es. |
| Konflikt |Startstille zu kurz | Die Startstille ist kürzer als 100 ms. | Die Startstille wird auf 100 ms erweitert. Laden Sie das normalisierte Dataset herunter, und überprüfen Sie es. |
| Konflikt | Endstille zu kurz | Die Endstille ist kürzer als 100 ms. | Die Endstille wird auf 100 ms erweitert. Laden Sie das normalisierte Dataset herunter, und überprüfen Sie es. |

Wenn die dritte Art von Fehlern, die in der folgenden Tabelle aufgeführt sind, nicht behoben wird, obwohl die Daten mit diesen Fehlern während des Trainings nicht ausgeschlossen werden, wirkt sich dies auf die Qualität des Trainings aus. Für ein qualitativ hochwertigeres Training wird empfohlen, diese Fehler manuell zu beheben. 

| Category | Name | BESCHREIBUNG | Vorschlag |
| --------- | ----------- | ----------- | --------------------------- |
| Skript | Ziffer 0-9 enthalten| Diese Skriptzeilen enthalten die Ziffer 0-9.| Die Skriptzeilen enthalten die Ziffer 0-9. Erweitern Sie sie zu normalisierten Wörtern, und gleichen Sie sie mit Audiodaten ab. Beispiel: „123“ zu „einhundertdreiundzwanzig“.|
| Skript | Wort mit verwirrender Aussprache '{}' | Skript enthält Wort mit verwirrender Aussprache: '{}'.| Erweitern Sie das Wort auf seine tatsächliche Aussprache. Beispielsweise {}.|
| Skript | Zu wenige Frageäußerungen| Frageskriptzeilen machen weniger als 1/6 der gesamten Skriptzeilen aus.| Frageskriptzeilen sollten mindestens 1/6 der Gesamtzeilen ausmachen, damit der Voicefont den Frageton richtig ausdrückt.|
| Skript | Zu wenige Ausrufäußerungen| Ausrufskriptzeilen machen weniger als 1/6 der gesamten Skriptzeilen aus.| Ausrufskriptzeilen sollten mindestens 1/6 der Gesamtzeilen ausmachen, damit der Voicefont den Ausrufton richtig ausdrückt.|
| Audio| Niedrige Samplingrate für neuronale Stimme | Die Audiosamplingrate ist niedriger als 24 KHz.|    Die Samplingrate der WAV-Datei sollte für eine hochwertige neuronale Stimme gleich oder höher als 24 KHz sein.|
| Volume | Gesamtlautstärke zu niedrig | Die Lautstärke von {}-Stichproben ist kleiner als -18 dB (10 % der maximalen Lautstärke).|     Steuern Sie den durchschnittlichen Lautstärkepegel während der Aufzeichnung oder Datenaufbereitung im richtigen Bereich.|
| Volume | Lautstärkeabbruch| Bei {}s wurde ein Lautstärkeabbruch erkannt.| Passen Sie die Aufzeichnungsgeräte an, um einen Lautstärkeabbruch beim Spitzenwert zu vermeiden.|
| Volume | Startstille nicht bereinigt | Die ersten 100 ms der Stille sind nicht bereinigt. Erkennen Sie eine Lautstärke, die über -40 dB liegt (1 % der maximalen Lautstärke).|    Reduzieren Sie den Pegel des Aufzeichnungsrauschens, und belassen Sie die ersten 100 ms als Stille.|
| Volume| Endstille nicht bereinigt| Die letzten 100 ms der Stille sind nicht bereinigt. Erkennen Sie eine Lautstärke, die über -40 dB liegt (1 % der maximalen Lautstärke).|     Reduzieren Sie den Pegel des Aufzeichnungsrauschens, und belassen Sie die letzten 100 ms als Stille.|
| Konflikt | Konflikt zwischen Skript und Audiodaten erkannt| Es besteht ein Konflikt zwischen den Skript- und Audioinhalten. |     Überprüfen Sie die Skript- und Audioinhalte, um sicherzustellen, dass sie mit dem Rauschpegel übereinstimmen und diesen steuern. Reduzieren Sie die Länge der langen Stille, oder teilen Sie sie in mehrere Äußerungen auf.|
| Konflikt | Zusätzliche Audioenergie vor dem ersten Wort erkannt |    Es wurde zusätzliche Audioenergie vor dem ersten Wort erkannt. Dies kann auch daran liegt, dass die Startstille vor dem ersten Wort zu kurz ist.|    Überprüfen Sie die Skript- und Audioinhalte, um sicherzustellen, dass sie mit dem Rauschpegel übereinstimmen und diesen steuern. Lassen Sie auch vor dem ersten Wort eine Stille von 100 ms.|
| Konflikt | Zusätzliche Audioenergie nach dem letzten Wort erkannt| Es wurde zusätzliche Audioenergie nach dem letzten Wort erkannt. Dies kann auch daran liegen, dass die Stille nach dem letzten Wort zu kurz ist.|    Überprüfen Sie die Skript- und Audioinhalte, um sicherzustellen, dass sie mit dem Rauschpegel übereinstimmen und diesen steuern. Lassen Sie auch nach dem letzten Wort eine Stille von 100 ms.|
| Konflikt | Niedriges Signal-Rausch-Verhältnis | Der Pegel des Signal-Rausch-Verhältnisses ist niedriger als {} dB.| Reduzieren Sie den Audiorauschpegel während der Aufzeichnung oder Datenaufbereitung.|
| Konflikt | Fehler beim Erkennen von Sprachinhalten | Fehler bei der Spracherkennung für diese Audiodaten.|  Überprüfen Sie Audio- und Skriptinhalte, um sicherzustellen, dass die Audiodaten gültig sind und mit dem Skript übereinstimmen.|

## <a name="train-your-custom-neural-voice-model"></a>Trainieren Ihres benutzerdefinierten neuronalen Stimmmodells

Nachdem Ihr Dataset überprüft wurde, können Sie damit Ihr benutzerdefiniertes neuronales Stimmmodell erstellen.

1. Klicken Sie auf der Registerkarte **Modell trainieren** auf **Modell trainieren**, um ein Stimmmodell mit den hochgeladenen Daten zu erstellen.

2. Wählen Sie die neuronale Trainingsmethode für Ihr Modell und Ihre Zielsprache aus.

Standardmäßig wird Ihr Stimmmodell in derselben Sprache wie Ihre Trainingsdaten trainiert. Sie können auch eine sekundäre Sprache (Vorschauversion) für Ihr Stimmmodell erstellen.  Überprüfen Sie die Sprachen, die für benutzerdefinierte neuronale Stimmen und sprachübergreifende Features unterstützt werden: [Sprache für die Anpassung](language-support.md#customization).

3. Wählen Sie als nächstes das Dataset aus, das Sie für das Training verwenden möchten, und geben Sie eine Sprecherdatei an.

>[!NOTE]
>- Sie müssen mindestens 300 Äußerungen auswählen, um eine benutzerdefinierte neuronale Stimme zu erstellen.
>- Um eine neuronale Stimme zu trainieren, müssen Sie ein Sprecherprofil mit der Audiozustimmungsdatei des Sprechers angeben, in der dieser der Verwendung seiner Sprachdaten zum Trainieren eines benutzerdefinierten Sprachmodells zustimmt. Das Feature „Benutzerdefinierte neuronale Stimme“ ist mit eingeschränktem Zugriff verfügbar. Stellen Sie sicher, dass Sie die [Anforderungen für verantwortungsvolle KI](/legal/cognitive-services/speech-service/custom-neural-voice/limited-access-custom-neural-voice?context=%2fazure%2fcognitive-services%2fspeech-service%2fcontext%2fcontext) kennen, und [fordern Sie hier den Zugriff an](https://aka.ms/customneural).
>- Auf dieser Seite können Sie auch Ihr Manuskript zum Testen hochladen. Das Testmanuskript muss eine TXT-Datei mit einer Größe von weniger als 1 MB sein. Unterstützte Codierungsformat sind z. B. ANSI/ASCII, UTF-8, UTF-8-BOM, UTF-16-LE oder UTF-16-BE. Jeder Absatz der Äußerung ergibt eine separate Audiodatei. Wenn Sie alle Sätze in einer Audiodatei kombinieren möchten, fügen Sie sie zu einem einzigen Absatz zusammen.

4. Geben Sie dann unter **Name** einen Namen und unter **Beschreibung** eine Beschreibung ein, um dieses Modell identifizieren zu können.

Wählen Sie den Namen sorgfältig aus. Der hier eingegebene Name ist der Name, der zur Festlegung der Stimme in der Sprachsyntheseanforderung als Teil der SSML-Eingabe verwendet wird. Zulässig sind nur Buchstaben, Zahlen und einige wenige Satzzeichen wie -, \_ und (', '). Verwenden Sie unterschiedliche Namen für unterschiedliche neuronale Stimmmodelle.

Im Feld **Beschreibung** werden in der Regel die Namen der Datasets erfasst, die zur Erstellung des Modells verwendet wurden.

5. Überprüfen Sie die Einstellungen, und klicken Sie dann auf **Übermitteln**, um mit dem Trainieren des Modells zu beginnen.

> [!NOTE]
> Doppelte Audionamen werden aus dem Training entfernt. Achten Sie darauf, dass die ausgewählten Datasets in mehreren ZIP-Dateien nicht die gleichen Audionamen enthalten.

In der Tabelle **Modell trainieren** wird ein neuer Eintrag angezeigt, der diesem neu erstellten Modell entspricht. Außerdem zeigt die Tabelle den Status an: „Verarbeitung“, „Erfolgreich“ oder „Fehler“.

Der angezeigte Status gibt Aufschluss über die Konvertierung Ihres Datasets in ein Stimmmodell (wie hier gezeigt).

| State | Bedeutung |
| ----- | ------- |
| Verarbeitung | Ihr Stimmmodell wird erstellt. |
| Erfolgreich | Ihr Stimmmodell wurde erstellt und kann bereitgestellt werden. |
| Fehler | Beim Trainieren Ihres Stimmmodells ist ein Fehler aufgetreten. Dies kann verschiedene Ursachen haben (beispielsweise ein unvorhergesehenes Datenproblem oder ein Netzwerkproblem). |

Die Trainingsdauer hängt davon ab, wie viele Daten Sie trainieren. Das Trainieren einer benutzerdefinierten neuronalen Stimme dauert im Durchschnitt etwa 40 Computestunden. 

> [!NOTE]
> Das Training von benutzerdefinierten neuronalen Stimmen ist nicht kostenlos. Eine Preisübersicht finden Sie [unter diesem Link](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/). Benutzer mit einem Standard-Abonnement (S0) können drei Stimmen gleichzeitig trainieren. Wenn das Limit erreicht wurde, warten Sie, bis der Trainingsvorgang mindestens eines Voicefonts beendet wurde, und versuchen Sie es dann noch mal. 

6. Nachdem Sie das Training des Modells erfolgreich abgeschlossen haben, können Sie die Modelldetails überprüfen.

Bei jedem Training werden 100 Beispielaudiodateien automatisch generiert, um das Modell zu testen. Nach der erfolgreichen Erstellung des Stimmmodells können Sie es vor der Bereitstellung testen.

Die Qualität der Stimme hängt von vielen Faktoren ab, z. B. der Größe des Trainingsdatasets, der Qualität der Aufzeichnung, der Genauigkeit der Transkriptionsdatei sowie davon, wie gut die in den Trainingsdaten aufgezeichnete Stimme zu der Persönlichkeit der für Ihren vorgesehenen Anwendungsfall entworfenen Stimme passt. [Weitere Informationen zu den Funktionen und Beschränkungen dieser Technologie sowie bewährte Methoden zur Verbesserung der Modellqualität](/legal/cognitive-services/speech-service/custom-neural-voice/characteristics-and-limitations-custom-neural-voice?context=%2fazure%2fcognitive-services%2fspeech-service%2fcontext%2fcontext) 

## <a name="create-and-use-a-custom-neural-voice-endpoint"></a>Erstellen und Verwenden eines Endpunkts für eine benutzerdefinierte neuronale Stimme

Nach dem erfolgreichen Erstellen und Testen des Sprachmodells können Sie es in einem benutzerdefinierten Text-to-Speech-Endpunkt bereitstellen. Diesen verwenden Sie anschließend anstelle des üblichen Endpunkts beim Senden von Text-to-Speech-Anforderungen über die REST-API. Der benutzerdefinierte Endpunkt kann nur von dem Abonnement aufgerufen werden, mit dem Sie den Voicefont bereitgestellt haben.

Gehen Sie wie folgt vor, um einen benutzerdefinierten Endpunkt für eine benutzerdefinierte neuronale Stimme zu erstellen.

1. Klicken Sie auf der Registerkarte **Modell bereitstellen** auf **Modelle bereitstellen**. 
2. Geben Sie als nächstes einen **Namen** und eine **Beschreibung** für Ihren benutzerdefinierten Endpunkt ein.
3. Wählen Sie ein Stimmmodell aus, das Sie diesem Endpunkt zuordnen möchten. 
4. Klicken Sie abschließend auf **Bereitstellen**, um Ihren Endpunkt zu erstellen.

Nach dem Klicken auf die Schaltfläche **Bereitstellen** wird in der Endpunkttabelle ein Eintrag für Ihren neuen Endpunkt angezeigt. Das Instanziieren des neuen Endpunkts kann einige Minuten in Anspruch nehmen. Wenn als Bereitstellungsstatus **Erfolgreich** angezeigt wird, kann der Endpunkt verwendet werden.

Sie können Ihren Endpunkt **aussetzen** und **fortsetzen**, wenn Sie ihn nicht immer verwenden. Wenn ein Endpunkt nach dem Aussetzen wieder aktiviert wird, bleibt die Endpunkt-URL unverändert, sodass Sie den Code in Ihren Apps nicht anpassen müssen. 

Sie können den Endpunkt aber auch auf ein neues Modell aktualisieren. Stellen Sie zum Ändern des Modells sicher, dass das neue Modell den gleichen Namen hat wie dasjenige, das Sie aktualisieren möchten. 

> [!NOTE]
>- Benutzer mit einem Standard-Abonnement (S0) können bis zu 50 Endpunkte mit jeweils einer eigenen benutzerdefinierten neuronalen Stimme bereitstellen.
>- Wenn Sie Ihre benutzerdefinierte neuronale Stimme verwenden möchten, müssen Sie den Namen des Stimmmodells angeben sowie den benutzerdefinierten URI direkt in einer HTTP-Anforderung verwenden. Außerdem müssen Sie das gleiche Abonnement verwenden, um die Authentifizierung des TTS-Diensts zu durchlaufen.

Nachdem Sie Ihren Endpunkt bereitgestellt haben, wird sein Name als Link angezeigt. Klicken Sie auf den Link, um spezifische Informationen zu Ihrem Endpunkt anzuzeigen (z. B. Endpunktschlüssel, Endpunkt-URL und Beispielcode).

Der benutzerdefinierte Endpunkt verfügt über dieselben Funktionen wie der Standardendpunkt für Text-to-Speech-Anforderungen.  Weitere Informationen finden Sie unter [Speech SDK](./get-started-text-to-speech.md) oder [REST-API](rest-text-to-speech.md).

Wir stellen auch ein Onlinetool, [Audioinhaltserstellung](https://speech.microsoft.com/audiocontentcreation), zur Verfügung, mit dem Sie die Audioausgabe über eine benutzerfreundliche Benutzeroberfläche optimieren können.

## <a name="next-steps"></a>Nächste Schritte

- [Aufnehmen von Sprachbeispielen](record-custom-voice-samples.md)
- [Text-to-Speech-REST-API](rest-text-to-speech.md)
- [API für lange Audioinhalte](long-audio-api.md)