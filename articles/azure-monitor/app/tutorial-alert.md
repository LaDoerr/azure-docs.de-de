---
title: Senden von Warnungen aus Application Insights nach SQL | Microsoft-Dokumentation
description: Tutorial zum Senden von Warnungen als Reaktion auf Fehler in der Anwendung mithilfe von Azure Application Insights
ms.topic: tutorial
ms.date: 04/10/2019
ms.custom: mvc
ms.openlocfilehash: 52510fa531ba43a9e2bb8b7d915847cd551c520a
ms.sourcegitcommit: 147910fb817d93e0e53a36bb8d476207a2dd9e5e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2021
ms.locfileid: "130129584"
---
# <a name="monitor-and-alert-on-application-health-with-azure-application-insights"></a>Überwachen der Anwendungsintegrität und Senden entsprechender Warnungen mit Azure Application Insights

Mit Azure Application Insights können Sie Ihre Anwendung überwachen und Warnungen erhalten, wenn sie nicht verfügbar ist sowie wenn Fehler oder Leistungsprobleme auftreten.  Dieses Tutorial führt Sie durch den Prozess zum Erstellen von Tests, um die Verfügbarkeit einer Anwendung kontinuierlich zu überprüfen.

Folgendes wird vermittelt:

> [!div class="checklist"]
> * Erstellen eines Verfügbarkeitstests zur kontinuierlichen Überprüfung der Anwendungsreaktion
> * Senden von E-Mails an Administratoren, wenn ein Problem auftritt

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

Erstellen Sie eine [Application Insights-Ressource](../app/create-new-resource.md).

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="create-availability-test"></a>Erstellen eines Verfügbarkeitstests

Verfügbarkeitstests in Application Insights ermöglichen Ihnen das automatische Testen Ihrer Anwendung an verschiedenen Standorten auf der ganzen Welt.   In diesem Tutorial führen Sie einen einfachen URL-Test durch, um sicherzustellen, dass Ihre Webanwendung verfügbar ist.  Sie könnten auch eine vollständige exemplarische Vorgehensweise erstellen, um ihren Betrieb im Detail zu testen. 

1. Klicken Sie auf **Application Insights**, und wählen Sie anschließend Ihr Abonnement aus.  

2. Wählen Sie im Menü **Untersuchen** die Option **Verfügbarkeit** aus, und klicken Sie dann auf **Test erstellen**.

    ![Verfügbarkeitstest hinzufügen](media/tutorial-alert/add-test-001.png)

3. Geben Sie einen Namen für den Test ein, und behalten Sie die anderen Standardwerte bei.  Bei dieser Auswahl werden alle 5 Minuten Anforderungen der Anwendungs-URL an fünf verschiedenen geografischen Standorten ausgelöst.

4. Wählen Sie **Warnungen** aus, um die Dropdownliste **Warnungen** zu öffnen, in der Sie detailliert festlegen können, wie bei einem Fehlschlagen des Tests reagiert wird. Wählen Sie **Nahezu in Echtzeit**, und legen Sie den Status auf **Aktiviert** fest.

    Geben Sie eine E-Mail-Adresse ein, an die eine Nachricht gesendet wird, wenn die Warnungskriterien erfüllt sind.  Geben Sie optional die Adresse eines Webhooks ein, der aufgerufen wird, wenn die Warnungskriterien erfüllt sind.

    ![Test erstellen](media/tutorial-alert/create-test-001.png)

5. Kehren Sie zum Testbereich zurück, klicken Sie auf (...), und bearbeiten Sie die Warnung, um die Konfiguration für Ihre in nahezu Echtzeit erfolgende Warnung einzugeben.

    ![Warnung bearbeiten](media/tutorial-alert/edit-alert-001.png)

6. Legen Sie „Fehlgeschlagene Standorte“ auf einen Wert größer oder gleich 3 fest. Erstellen Sie eine [Aktionsgruppe](../alerts/action-groups.md), um zu konfigurieren, wer benachrichtigt wird, wenn Ihr Warnungsschwellenwert überschritten wird.

    ![Benutzeroberfläche zum Speichern der Warnung](media/tutorial-alert/save-alert-001.png)

7. Nachdem Sie Ihre Warnung konfiguriert haben, klicken Sie auf einen Testnamen, um Details zu den einzelnen Standorten anzuzeigen. Tests können sowohl im Linien- als auch Punktdiagrammformat dargestellt werden, um den Erfolg bzw. Misserfolg für eine bestimmte Zeitspanne zu visualisieren.

    ![Testdetails](media/tutorial-alert/test-details-001.png)

8. Sie können einen bestimmten Test detailliert untersuchen, indem Sie im Punktdiagramm auf den zugehörigen Punkt klicken. Dadurch wird eine Detailansicht der gesamten Transaktion eingeblendet. Das folgende Beispiel zeigt die Details für eine fehlgeschlagene Anforderung.

    ![Testergebnis](media/tutorial-alert/test-result-001.png)
  
## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich nun mit dem Erstellen von Warnungen bei Problemen vertraut gemacht haben, erfahren Sie im nächsten Tutorial, wie die Interaktion von Benutzern mit Ihrer Anwendung analysiert wird.

> [!div class="nextstepaction"]
> [Grundlegendes zu Benutzern](./tutorial-users.md)

