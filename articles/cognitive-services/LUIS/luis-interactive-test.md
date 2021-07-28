---
title: Testen von Apps im LUIS-Portal
description: Verwenden Sie LUIS (Language Understanding Intelligent Service), um Ihre Anwendung fortlaufend zu optimieren und ihr Sprachverständnis zu verbessern.
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.author: aahi
author: aahill
ms.manager: nitinme
ms.date: 06/01/2021
ms.openlocfilehash: d5263f85fa8cc2a9f1b55da32b4aa1418e304347
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111954264"
---
# <a name="test-your-luis-app-in-the-luis-portal"></a>Testen Ihrer LUIS-App im LUIS-Portal


Beim Testen stellen Sie LUIS Beispieläußerungen bereit und erhalten eine Antwort von Absichten und Entitäten, die LUIS erkannt hat. Sie können LUIS interaktiv mit jeweils einer einzelnen Äußerung testen, oder Sie können einen Batch von Äußerungen angeben. Beim Testen können Sie die Vorhersageantwort des aktuell aktiven Modells mit der Vorhersageantwort des veröffentlichten Modells vergleichen. 


Das Testen einer App ist ein iterativer Vorgang. Testen Sie Ihre LUIS-App nach dem Trainieren mit Beispieläußerungen, um festzustellen, ob die Absichten und Entitäten ordnungsgemäß erkannt werden. Ist das nicht der Fall, aktualisieren Sie die LUIS-App, und trainieren und testen Sie sie erneut.

<!-- anchors for H2 name changes -->
<a name="train-your-app"></a>
<a name="test-your-app"></a>
<a name="access-the-test-page"></a>
<a name="luis-interactive-testing"></a>

## <a name="interactive-testing"></a>Interaktives Testen

Interaktive Tests werden im **Testbereich** des LUIS-Portals durchgeführt. Sie können eine Äußerung eingeben, um zu ermitteln, wie Absichten und Entitäten identifiziert und bewertet werden. Wenn LUIS die Absichten und Entitäten für eine Äußerung im Testbereich nicht wie erwartet vorhersagt, kopieren Sie sie als neue Äußerung auf die Seite **Absicht**. Bezeichnen Sie dann die Teile dieser Äußerung nach Entitäten, und trainieren Sie LUIS. 

Wenn Sie mehrere Äußerungen gleichzeitig testen, finden Sie weitere Informationen unter [Batchtests](./luis-how-to-batch-test.md). Weitere Informationen zu Vorhersageergebnissen finden Sie im Artikel [Vorhersageergebnisse](luis-concept-prediction-score.md).

Sie können den Test mithilfe des [Endpunkts](luis-glossary.md#endpoint) durchführen. Dabei können Sie maximal zwei Versionen der App einsetzen. Die Haupt- oder Liveversion Ihrer App wird dabei als **Produktionsendpunkt** festgelegt, und eine zweite Version wird auf dem **Stagingendpunkt** hinzugefügt. Mit diesem Ansatz erhalten Sie drei Versionen einer Äußerung: das aktuelle Modell im Testbereich des [LUIS](luis-reference-regions.md)-Portals und die beiden Versionen auf zwei unterschiedlichen Endpunkten. 

Alle Endpunkttests werden auf Ihr Nutzungskontingent angerechnet. 

## <a name="logging"></a>Protokollierung

LUIS speichert alle protokollierten Äußerungen im Abfrageprotokoll, das im LUIS-Portal auf der Seite mit der **Apps**-Liste sowie über die [Erstellungs-APIs](https://go.microsoft.com/fwlink/?linkid=2092087) für LUIS zum Download bereitsteht. 

Wenn Sie Tests an einem Endpunkt durchführen und nicht möchten, dass die Äußerung protokolliert wird, müssen Sie die Konfiguration `logging=false` für Abfragezeichenfolgen verwenden.

Alle Äußerungen, bei denen LUIS unsicher ist, werden auf der Seite **[Überprüfen von Endpunktäußerungen](luis-how-to-review-endpoint-utterances.md)** des [LUIS](luis-reference-regions.md)-Portals aufgeführt.  

## <a name="test-an-utterance"></a>Testen einer Äußerung

> [!NOTE]
> Denken Sie daran, LUIS zu [trainieren](luis-how-to-train.md), nachdem Sie Änderungen am Modell vorgenommen haben. Änderungen an der LUIS-App stehen erst für Tests zur Verfügung, nachdem die App trainiert wurde.
> 1. Melden Sie sich beim LUIS-Portal an, und wählen Sie Ihr Abonnement und Ihre Erstellungsressource aus, um die Apps anzuzeigen, die dieser Erstellungsressource zugewiesen sind.
> 2. Öffnen Sie Ihre App, indem Sie auf der Seite Meine Apps den Namen der App auswählen.
> 3. Um die aktuellste Version der aktiven App zu testen, wählen Sie vor dem Testen im oberen Menü Train (Trainieren) aus.

Die Testäußerung sollte nicht exakt mit einer der Beispieläußerungen in der App übereinstimmen. Die Testäußerung sollte die Wortwahl, Satzlänge und Entitätsverwendung aufweisen, die Sie von einem Benutzer erwarten.

1. Melden Sie sich beim [LUIS-Portal](https://www.luis.ai) an, und wählen Sie Ihr **Abonnement** und Ihre **Erstellungsressource** aus, um die Apps anzuzeigen, die dieser Erstellungsressource zugewiesen sind.
1. Öffnen Sie Ihre App, indem Sie auf der Seite **Meine Apps** den Namen der App auswählen.

1. Um auf den Einblendbereich **Test** zuzugreifen, wählen Sie im oberen Bereich Ihrer Anwendung **Test** aus.

    > [!div class="mx-imgBorder"]
    > ![Seite „Train & Test App“](./media/luis-how-to-interactive-test/test.png) (App trainieren und testen)

1. Geben Sie eine Äußerung in das Textfeld ein, und drücken Sie die EINGABETASTE. Sie können im **Test** beliebig viele Äußerungen eingeben, aber immer nur einzeln.

1. Die Äußerung, die am höchsten bewertete Absicht und ihre Bewertung werden der Liste der Äußerungen unter dem Textfeld hinzugefügt.

    > [!div class="mx-imgBorder"]
    > ![Erkennen einer falschen Absicht in interaktiven Tests](./media/luis-how-to-interactive-test/test-weather-1.png)

## <a name="inspect-the-prediction"></a>Untersuchen der Vorhersage

Sie überprüfen die Details des Testergebnisses im Bereich **Überprüfen**.

1. Wählen Sie bei geöffnetem Einblendbereich **Test** die Option **Überprüfen** für eine Äußerung aus, die verglichen werden soll.

    > [!div class="mx-imgBorder"]
    > ![Auswählen der Schaltfläche „Überprüfen“, um weitere Details zu den Testergebnissen anzuzeigen](./media/luis-how-to-interactive-test/inspect.png)

1. Der Bereich **Überprüfung** wird angezeigt. Dieser Bereich enthält die Absicht mit der höchsten Bewertung sowie alle identifizierten Entitäten. Die Vorhersage der ausgewählten Äußerung wird im Bereich angezeigt.

    > [!div class="mx-imgBorder"]
    > ![Ausschnitt aus einem Screenshot des Testüberprüfungsbereichs](./media/luis-how-to-interactive-test/inspect-panel.png)

## <a name="add-to-example-utterances"></a>Hinzufügen zu Beispieläußerungen

Im Überprüfungsbereich können Sie die Testäußerung einer Absicht hinzufügen, indem Sie **Add to example utterances** (Zu Beispieläußerungen hinzufügen) auswählen.

## <a name="disable-required-features"></a>Deaktivieren erforderlicher Features

So können Sie feststellen, ob die trainierte App Ihre Entitäten basierend auf den erforderlichen Features richtig vorhersagt. Die Standardeinstellung besteht in der Anwendung des Features bei Bedarf während der Vorhersage. Mit diesem Umschalter können Sie anzeigen, wie die Vorhersage lauten würde, wenn das Feature der Unterentität nicht erforderlich wäre.

### <a name="when-to-disable-required-features"></a>Wann sollte man erforderliche Features deaktivieren

Die trainierte App kann eine durch maschinelles Lernen erworbene Entität auf Grundlage eines der folgenden Aspekte falsch vorhersagen:
* Fehlerhafte Bezeichnung von Beispieläußerungen.
* Das erforderliche Feature stimmt nicht mit dem Text überein.

Eine Beispiel ist eine durch maschinelles Lernen erworbene Entität mit den Namen einer Person als Unterentität.

:::image type="content" source="media/luis-how-to-interactive-test/disable-required-feature.png" alt-text="Screenshot des durch maschinelles Lernen erworbenen Entitätsschemas im LUIS-Portal mit erforderlichem Feature":::

Eine Beispieläußerung für diese durch maschinelles Lernen erworbene Entität ist: `Assign Bob Jones to work on the new security feature`.

Die Extraktion sollte `security feature` als Ticketbeschreibung und `Bob Jones` als Techniker sein, beides Unterentitäten der Entität `Assign ticket`.

Um der Unterentität bei einer erfolgreichen Vorhersage zu helfen, fügen Sie die vorgefertigte Entität[PersonName](luis-reference-prebuilt-person.md) der Unterentität `engineer` als Feature hinzu. Wenn Sie das Feature erforderlich machen, bedeutet dies, dass die Unterentität nur extrahiert wird, wenn die vorgefertigte Entität „PersonName“ für den Text vorhergesagt wird. Dies bedeutet, dass ein beliebiger Name im Text, der nicht mit der Unterentität „PersonName“ vorhergesagt wird, nicht als bezeichnete Unterentität `engineer` zurückgegeben wird.

Wenn Sie den interaktiven Testbereich verwenden und feststellen, dass eine untergeordnete Entität mit einem erforderlichen Feature nicht vorhersagt, schalten Sie diese Einstellung um, um festzustellen, ob die Unterentität vorhergesagt würde, wenn das Feature nicht erforderlich wäre. Die Unterentität kann wegen der korrekten Bezeichnung von Beispieläußerungen möglicherweise ohne das erforderliche Feature ordnungsgemäß vorhergesagt werden.

## <a name="view-sentiment-results"></a>Anzeigen von Standpunktergebnissen

Wenn die **Standpunktanalyse** auf der Seite **[Publish](luis-how-to-publish-app.md#enable-sentiment-analysis)** (Veröffentlichen) konfiguriert wird, schließen die Testergebnisse den in der Äußerung gefundenen Standpunkt ein.

## <a name="correct-matched-patterns-intent"></a>Korrigieren der Absicht des übereinstimmenden Musters

Wenn Sie [Muster](luis-concept-patterns.md) verwenden und die Äußerung mit einem Muster übereinstimmte, jedoch die falsche Absicht vorhergesagt wurde, wählen Sie den Link **Bearbeiten** für das Muster und dann die richtige Absicht aus.

## <a name="compare-with-published-version"></a>Vergleichen mit der veröffentlichten Version

Sie können die aktive Version Ihrer App mit der veröffentlichten [Endpunktversion](luis-glossary.md#endpoint) testen. Wählen Sie im Bereich **Überprüfen** die Option **Compare with published** (Mit veröffentlichtem Element vergleichen) aus. Tests mit dem veröffentlichten Modell werden mit dem Kontingent Ihres Azure-Abonnements verrechnet.

> [!div class="mx-imgBorder"]
> ![Vergleichen mit veröffentlichtem Element](./media/luis-how-to-interactive-test/inspect-panel-compare.png)

## <a name="view-endpoint-json-in-test-panel"></a>Anzeigen des Endpunkt-JSON-Codes im Testbereich
Sie können den für den Vergleich zurückgegebenen JSON-Code des Endpunkts anzeigen, indem Sie die Ansicht **JSON anzeigen** auswählen.

> [!div class="mx-imgBorder"]
> ![Veröffentlichte JSON-Antwort](./media/luis-how-to-interactive-test/inspect-panel-compare-json.png)

## <a name="additional-settings-in-test-panel"></a>Zusätzliche Einstellungen im Testbereich

### <a name="luis-endpoint"></a>LUIS-Endpunkt

Wenn Sie mehrere LUIS-Endpunkte haben, können Sie den für Tests verwendeten Endpunkt über den Link **Zusätzliche Einstellungen** im Bereich „Veröffentlicht“ des Tests ändern. Wenn Sie nicht sicher sind, welchen Endpunkt Sie verwenden sollten, wählen Sie den Standard **Starter_Key** aus.

> [!div class="mx-imgBorder"]
> ![Testbereich mit hervorgehobenem Link „Zusätzliche Einstellungen“](media/luis-how-to-interactive-test/additional-settings-v3-settings.png)


## <a name="batch-testing"></a>Testen in Batches
Weitere Informationen finden Sie in den [Konzepten](./luis-how-to-batch-test.md) zu Batchtests und im Artikel zum [Testen eines Batches von Äußerungen](luis-how-to-batch-test.md).

## <a name="next-steps"></a>Nächste Schritte

Wenn sich in Tests herausstellt, dass Ihre LUIS-App nicht die richtigen Absichten und Entitäten erkennt, können Sie die Genauigkeit Ihrer LUIS-App verbessern, indem Sie aktiv weitere Äußerungen bezeichnen oder Features hinzufügen.

* [Bezeichnen von vorgeschlagenen Äußerungen mit LUIS](luis-how-to-review-endpoint-utterances.md)
* [Verwenden von Features zum Verbessern der Leistung Ihrer LUIS-App](luis-how-to-add-features.md)
* [Bewährte Methoden für SQL Data Warehouse](luis-concept-best-practices.md)