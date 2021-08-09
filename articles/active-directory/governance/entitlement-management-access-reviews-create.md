---
title: Erstellen einer Zugriffsüberprüfung für ein Zugriffspaket in der Azure AD-Berechtigungsverwaltung
description: Erfahren Sie, wie Sie in Azure Active Directory-Zugriffsüberprüfungen (Vorschauversion) eine Zugriffsüberprüfungsrichtlinie für Zugriffspakete in der Berechtigungsverwaltung erstellen.
services: active-directory
documentationCenter: ''
author: ajburnle
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.subservice: compliance
ms.date: 06/18/2020
ms.author: ajburnle
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: bf34223e44ddfb59a72f98d31f0df5d9a882e0eb
ms.sourcegitcommit: 5da0bf89a039290326033f2aff26249bcac1fe17
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2021
ms.locfileid: "109713592"
---
# <a name="create-an-access-review-of-an-access-package-in-azure-ad-entitlement-management"></a>Erstellen einer Zugriffsüberprüfung für ein Zugriffspaket in der Azure AD-Berechtigungsverwaltung

Um das Risiko eines veralteten Zugriffs zu verringern, sollten Sie regelmäßige Überprüfungen von Benutzern aktivieren, die über aktive Zuweisungen für ein Zugriffspaket in der Azure AD-Berechtigungsverwaltung verfügen. Sie können Überprüfungen aktivieren, wenn Sie ein neues Zugriffspaket erstellen oder ein vorhandenes Zugriffspaket bearbeiten. In diesem Artikel wird beschrieben, wie Sie Zugriffsüberprüfungen für Zugriffspakete aktivieren.

## <a name="prerequisites"></a>Voraussetzungen

Um Überprüfungen von Zugriffspaketen aktivieren zu können, müssen Sie die Voraussetzungen für das Erstellen eines Zugriffspakets erfüllen:
- Azure AD Premium P2
- Globaler Administrator, Identity Governance-Administrator, Benutzeradministrator, Katalogbesitzer oder Zugriffspaket-Manager

Weitere Informationen finden Sie unter [Lizenzanforderungen](entitlement-management-overview.md#license-requirements).


## <a name="create-an-access-review-of-an-access-package"></a>Erstellen einer Zugriffsüberprüfung für ein Zugriffspaket

Sie können Zugriffsüberprüfungen beim [Erstellen eines neuen Zugriffspakets](entitlement-management-access-package-create.md) oder beim [Bearbeiten eines vorhandenen Zugriffspakets](entitlement-management-access-package-lifecycle-policy.md) aktivieren. Führen Sie die folgenden Schritte aus, um Zugriffsüberprüfungen für ein Zugriffspaket zu aktivieren:

1. Öffnen Sie die Registerkarte **Lebenszyklus** für ein Zugriffspaket, und scrollen Sie nach unten zu **Zugriffsüberprüfungen**.

1. Verschieben Sie den Umschalter **Zugriffsüberprüfungen erforderlich** auf **Ja**.

    ![Hinzufügen der Zugriffsüberprüfung](./media/entitlement-management-access-reviews/access-reviews-pane.png)

1. Geben Sie neben **Startet am** das Datum für den Beginn der Überprüfungen an.

1. Legen Sie als Nächstes die **Häufigkeit der Überprüfung** auf **Jährlich**, **Halbjährlich**, **Vierteljährlich** oder **Monatlich** fest.
Mit dieser Einstellung wird festgelegt, wie oft Zugriffsüberprüfungen erfolgen.

1. Legen Sie die **Dauer** fest, um zu definieren, wie viele Tage Prüfer Eingaben für jede Überprüfung der Serie vornehmen können. Sie können beispielsweise eine jährliche Überprüfung planen, die am 1. Januar beginnt und 30 Tage lang zur Überprüfung geöffnet ist, sodass Prüfer bis zum Ende des Monats Zeit für die Bearbeitung haben.

1. Wählen Sie neben **Prüfer** die Option **Selbstüberprüfung** aus, wenn Sie möchten, dass Benutzer ihre eigene Zugriffsüberprüfung durchführen, oder wählen Sie **Bestimmte Prüfer** aus, wenn Sie einen Prüfer angeben möchten.

    ![„Prüfer hinzufügen“ auswählen](./media/entitlement-management-access-reviews/access-reviews-add-reviewer.png)

1. Wenn Sie **Bestimmte Prüfer** ausgewählt haben, geben Sie an, welche Benutzer die Zugriffsüberprüfung durchführen sollen:
    1. Wählen Sie **Prüfer hinzufügen** aus.
    1. Suchen Sie im Bereich **Prüfer auswählen** nach dem/den Benutzer(n), den/die Sie als Prüfer festlegen möchten, und wählen Sie diese(n) aus.
    1. Klicken Sie nach der Auswahl des/der gewünschten Prüfer(s) auf die Schaltfläche **Auswählen**.

    ![Prüfer angeben](./media/entitlement-management-access-reviews/access-reviews-select-reviewer.png)

1. Klicken Sie unten auf der Seite auf **Überprüfen und erstellen**, wenn Sie ein neues Zugriffspaket erstellen, oder klicken Sie auf **Aktualisieren**, wenn Sie ein Zugriffspaket bearbeiten.

## <a name="view-the-status-of-the-access-review"></a>Anzeigen des Status der Zugriffsüberprüfung

Nach dem Startdatum wird eine Zugriffsüberprüfung im Abschnitt **Zugriffsüberprüfungen** aufgeführt. Führen Sie die folgenden Schritte aus, um den Status einer Zugriffsüberprüfung anzuzeigen:

1. Klicken Sie in **Identity Governance** auf **Zugriffspakete**, und wählen Sie dann das Zugriffspaket mit dem Zugriffsüberprüfungsstatus aus, den Sie anzeigen möchten.   

1. Wenn die Zugriffspaketübersicht angezeigt wird, klicken Sie im linken Menü auf **Zugriffsüberprüfungen**.
    
    ![Zugriffsüberprüfungen auswählen](./media/entitlement-management-access-reviews/access-review-status-access-package-overview.png)

1. Es wird eine Liste angezeigt, die alle Richtlinien enthält, denen Zugriffsüberprüfungen zugeordnet sind. Klicken Sie auf die gewünschte Überprüfung, um den zugehörigen Bericht anzuzeigen.

    ![Liste der Zugriffsüberprüfungen](./media/entitlement-management-access-reviews/access-review-status-select-access-reviews.png)
   
1. Im angezeigten Bericht sehen Sie die Anzahl der überprüften Benutzer und die vom Prüfer ausgeführten Aktionen.

    ![Überprüfungsstatus anzeigen](./media/entitlement-management-access-reviews/access-review-status.png)
 

## <a name="access-reviews-email-notifications"></a>E-Mail-Benachrichtigungen über Zugriffsüberprüfungen
Sie können Prüfer festlegen, oder die Benutzer können ihren Zugriff selbst überprüfen. Standardmäßig sendet Azure AD kurz nach dem Start der Überprüfung eine E-Mail an die Prüfer oder an die eine Selbstüberprüfung vornehmenden Benutzer.

Die E-Mail enthält Anweisungen zum Überprüfen des Zugriffs auf Zugriffspakete. Wenn die Überprüfung für Benutzer vorgesehen ist, die den eigenen Zugriff überprüfen sollen, geben Sie die Anweisungen zum Durchführen einer Selbstüberprüfung ihrer Zugriffspakete an.
  
Wenn Sie Gastbenutzer als Prüfer zugewiesen haben und diese ihre Azure AD-Gasteinladung nicht angenommen haben, erhalten diese Benutzer keine E-Mails von Azure AD-Zugriffsüberprüfungen. Die Benutzer müssen zuerst die Einladung annehmen und ein Konto mit Azure AD erstellen, bevor sie die E-Mails empfangen können. 

## <a name="next-steps"></a>Nächste Schritte

- [Überprüfen des Zugriffs auf Zugriffspakete](entitlement-management-access-reviews-review-access.md)
- [Selbstüberprüfung von Zugriffspaketen](entitlement-management-access-reviews-self-review.md)
