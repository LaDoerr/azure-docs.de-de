---
title: Konfigurieren des Rollenanspruchs für Azure AD-Unternehmens-Apps | Azure
titleSuffix: Microsoft identity platform
description: Erfahren Sie, wie Sie im SAML-Token ausgestellte Rollenansprüche für Unternehmensanwendungen in Azure Active Directory konfigurieren.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev
ms.workload: identity
ms.topic: how-to
ms.date: 02/15/2021
ms.author: jeedes
ms.openlocfilehash: 002304f06e97aa04b3c15548e524c931c69ec6d6
ms.sourcegitcommit: 03f0db2e8d91219cf88852c1e500ae86552d8249
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2021
ms.locfileid: "123031733"
---
# <a name="configure-the-role-claim-issued-in-the-saml-token-for-enterprise-applications"></a>Konfigurieren von im SAML-Token ausgestellten Rollenansprüchen für Unternehmensanwendungen

Mit Azure Active Directory (Azure AD) können Sie den Anspruchstyp für den Rollenanspruch in dem Antworttoken anpassen, das Sie nach dem Autorisieren der App erhalten.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure AD-Abonnement mit eingerichtetem Verzeichnis.
- Ein Abonnement, für das einmaliges Anmelden (Single Sign-On, SSO) aktiviert ist. Sie müssen SSO mit Ihrer Anwendung konfigurieren.

> [!NOTE]
> In diesem Artikel wird erläutert, wie Sie mithilfe von APIs in Azure AD Anwendungsrollen für den Dienstprinzipal erstellen, aktualisieren und löschen. Wenn Sie die neue Benutzeroberfläche für App-Rollen verwenden möchten, finden Sie [hier](./howto-add-app-roles-in-azure-ad-apps.md) weitere Informationen.

## <a name="when-to-use-this-feature"></a>Verwendung dieses Features

Verwenden Sie dieses Feature, wenn die Anwendung erwartet, dass in der von Azure AD zurückgegebenen SAML-Antworten benutzerdefinierte Rollen übergeben werden. Sie können nach Bedarf beliebig viele Rollen hinzufügen.

## <a name="create-roles-for-an-application"></a>Erstellen von Rollen für eine Anwendung

1. Klicken Sie im linken Bereich des <a href="https://portal.azure.com/" target="_blank">Azure-Portals</a> auf das Symbol **Azure Active Directory**.

    ![Azure Active Directory-Symbol][1]

2. Wählen Sie **Unternehmensanwendungen**. Wählen Sie anschließend **Alle Anwendungen**.

    ![Bereich für Unternehmensanwendungen][2]

3. Wählen Sie oben im Dialogfeld die Schaltfläche **Neue Anwendung** aus, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld den Namen Ihrer Anwendung ein, und wählen Sie Ihre Anwendung im Ergebnisbereich aus. Wählen Sie die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Anwendung in der Ergebnisliste](./media/active-directory-enterprise-app-role-management/tutorial_app_addfromgallery.png)

5. Wechseln Sie nach dem Hinzufügen der Anwendung zur Seite **Eigenschaften**, und kopieren Sie die Objekt-ID.

    ![Eigenschaftenseite](./media/active-directory-enterprise-app-role-management/tutorial_app_properties.png)

6. Öffnen Sie den [Microsoft Graph-Tester](https://developer.microsoft.com/graph/graph-explorer) in einem anderen Fenster, und führen Sie die folgenden Schritte aus:

    1. Melden Sie sich an der Graph-Tester-Website mit den Anmeldeinformationen des globalen Administrators bzw. Co-Admins für Ihren Mandanten an.

    1. Sie müssen über ausreichende Berechtigungen zum Erstellen der Rollen verfügen. Wählen Sie **Berechtigungen ändern**, um die Berechtigungen abzurufen.

        ![Schaltfläche „Berechtigungen ändern“](./media/active-directory-enterprise-app-role-management/graph-explorer-new9.png)

        > [!NOTE]
        > Die Rollen „Cloud-App-Administrator“ und „App-Administrator“ funktionieren in diesem Szenario nicht, da für das Lesen und Schreiben im Verzeichnis globale Administratorberechtigungen erforderlich sind.

    1. Wählen Sie in der Liste die folgenden Berechtigungen aus (sofern Sie noch nicht darüber verfügen), und wählen Sie **Berechtigungen ändern**.

        ![Liste mit Berechtigungen und Schaltfläche „Berechtigungen ändern“](./media/active-directory-enterprise-app-role-management/graph-explorer-new10.png)

    1. Erteilen Sie die Zustimmung. Sie werden wieder am System angemeldet.

    1. Ändern Sie die Version in **Beta**, und rufen Sie mithilfe der folgenden Abfrage die Liste mit den Dienstprinzipalen von Ihrem Mandanten ab:

        `https://graph.microsoft.com/beta/servicePrincipals`

        Halten Sie sich an das folgende Muster, wenn Sie mehrere Verzeichnisse verwenden: `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`

        ![Graph-Tester-Dialogfeld mit der Abfrage zum Abrufen von Dienstprinzipalen](./media/active-directory-enterprise-app-role-management/graph-explorer-new1.png)

    1. Wählen Sie in der Liste mit den abgerufenen Dienstprinzipalen den Dienstprinzipal aus, den Sie ändern möchten. Sie können auch mit STRG+F in den aufgeführten Dienstprinzipalen nach der Anwendung suchen. Suchen Sie nach der Objekt-ID, die Sie von der Seite **Eigenschaften** kopiert haben, und verwenden Sie die folgende Abfrage, um den Dienstprinzipal abzurufen:

        `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`

        ![Abfrage zum Abrufen des Dienstprinzipals, den Sie ändern müssen](./media/active-directory-enterprise-app-role-management/graph-explorer-new2.png)

    1. Extrahieren Sie die **appRoles**-Eigenschaft aus dem Dienstprinzipalobjekt.

        ![Details zur appRoles-Eigenschaft](./media/active-directory-enterprise-app-role-management/graph-explorer-new3.png)

        Bei Verwendung der benutzerdefinierten App (nicht der Azure Marketplace-App) werden zwei Standardrollen angezeigt: „user“ und „msiam_access“. Für die Marketplace-App ist „msiam_access“ die einzige Standardrolle. Sie müssen keine Änderungen in den Standardrollen vornehmen.

    1. Generieren Sie neue Rollen für Ihre Anwendung.

        Der folgende JSON-Code ist ein Beispiel für das **appRoles**-Objekt. Erstellen Sie ein ähnliches Objekt, um die gewünschten Rollen für Ihre Anwendung hinzuzufügen.

        ```json
        {
          "appRoles": [
            {
               "allowedMemberTypes": [
                  "User"
                ],
                "description": "msiam_access",
                "displayName": "msiam_access",
                "id": "b9632174-c057-4f7e-951b-be3adc52bfe6",
                "isEnabled": true,
                "origin": "Application",
                "value": null
            },
            {
                "allowedMemberTypes": [
                    "User"
                ],
                "description": "Administrators Only",
                "displayName": "Admin",
                "id": "4f8f8640-f081-492d-97a0-caf24e9bc134",
                "isEnabled": true,
                "origin": "ServicePrincipal",
                "value": "Administrator"
            }
         ]
        }
        ```

        Neue Rollen können nur nach der Rolle „msiam_access“ für den Patchvorgang hinzugefügt werden. Darüber hinaus können Sie Ihrer Organisation je nach Bedarf beliebig viele Rollen hinzufügen. Azure AD sendet den Wert dieser Rollen als Anspruchswert in der SAML-Antwort. Verwenden Sie zum Generieren der GUID-Werte für die ID von neuen Rollen Webtools wie [dieses](https://www.guidgenerator.com/).

    1. Wechseln Sie wieder zum Graph-Tester, und ändern Sie die Methode von **GET** in **PATCH**. Patchen Sie das Dienstprinzipalobjekt, damit es über die gewünschten Rollen verfügt, indem Sie die **appRoles**-Eigenschaft (siehe vorheriges Beispiel) aktualisieren. Wählen Sie die Option **Abfrage ausführen**, um den Patchvorgang auszuführen. Die Erstellung der Rolle wird in einer Erfolgsmeldung bestätigt.

        ![Patchvorgang mit Erfolgsmeldung](./media/active-directory-enterprise-app-role-management/graph-explorer-new11.png)

1. Nachdem der Dienstprinzipal mit weiteren Rollen gepatcht wurde, können Sie den jeweiligen Rollen Benutzer zuweisen. Sie können die Benutzer zuweisen, indem Sie im Portal zur Anwendung navigieren. Wählen Sie die Registerkarte **Benutzer und Gruppen**. Auf dieser Registerkarte werden alle Benutzer und Gruppen angezeigt, die der App bereits zugewiesen sind. Sie können für die neuen Rollen neue Benutzer hinzufügen. Außerdem können Sie einen vorhandenen Benutzer auswählen und dann die Option **Bearbeiten** verwenden, um die Rolle zu ändern.

    ![Registerkarte „Benutzer und Gruppen“](./media/active-directory-enterprise-app-role-management/graph-explorer-new5.png)

    Um die Rolle einem Benutzer zuzuweisen, wählen Sie die neue Rolle aus und wählen dann unten auf der Seite die Schaltfläche **Zuweisen**.

    ![Bereich „Zuweisung bearbeiten“ und Bereich „Rolle auswählen“](./media/active-directory-enterprise-app-role-management/graph-explorer-new6.png)

    
    Aktualisieren Sie Ihre Sitzung im Azure-Portal, um neue Rollen anzuzeigen.

1. Aktualisieren Sie die Tabelle **Attribute**, um eine benutzerdefinierte Zuordnung des Rollenanspruchs zu definieren.

1. Führen Sie im Dialogfeld **Benutzerattribute** im Abschnitt **Benutzeransprüche** die folgenden Schritte aus, um das SAML-Tokenattribut wie in der folgenden Tabelle gezeigt hinzuzufügen:

    | Attributname | Attributwert |
    | -------------- | ----------------|
    | Rollenname  | user.assignedroles |

    Wenn der Anspruchswert der Rolle NULL ist, sendet Azure AD diesen Wert nicht im Token. Dies ist der entwurfsgemäße Standard.

    1. Klicken Sie auf das Symbol **Bearbeiten**, um das Dialogfeld **Benutzerattribute und Ansprüche** zu öffnen.

        ![Screenshot: Symbol „Bearbeiten“, das zum Öffnen des Dialogfelds „Benutzerattribute und Ansprüche“ verwendet wird](./media/active-directory-enterprise-app-role-management/editattribute.png)

    1. Fügen Sie im Dialogfeld **Benutzeransprüche verwalten** das SAML-Tokenattribut hinzu, indem Sie auf **Neuen Anspruch hinzufügen** klicken.

        ![Schaltfläche „Attribut hinzufügen“](./media/active-directory-enterprise-app-role-management/tutorial_attribute_04.png)

        ![Bereich „Attribut bearbeiten“](./media/active-directory-enterprise-app-role-management/tutorial_attribute_05.png)

    1. Geben Sie im Feld **Name** den gewünschten Attributnamen ein. In diesem Beispiel wird **Rollenname** als Anspruchsname verwendet.

    1. Lassen Sie das Feld **Namespace** leer.

    1. Geben Sie in der Liste **Quellattribut** den für diese Zeile angezeigten Attributwert ein.

    1. Wählen Sie **Speichern** aus.

10. Melden Sie sich am [Zugriffsbereich](https://myapps.microsoft.com) an, und wählen Sie Ihre Anwendungskachel aus, um für die Anwendung das einmalige Anmelden zu testen, das von einem Identitätsanbieter initiiert wird. Im SAML-Token sollten alle zugewiesenen Rollen für den Benutzer mit dem von Ihnen festgelegten Anspruchsnamen angezeigt werden.

## <a name="update-an-existing-role"></a>Aktualisieren einer vorhandenen Rolle

Führen Sie die folgenden Schritte aus, um eine vorhandene Rolle zu aktualisieren:

1. Öffnen Sie den [Microsoft Graph-Tester](https://developer.microsoft.com/graph/graph-explorer).

1. Melden Sie sich an der Graph-Tester-Website mit den Anmeldeinformationen des globalen Administrators bzw. Co-Admins für Ihren Mandanten an.

1. Ändern Sie die Version in **Beta**, und rufen Sie mithilfe der folgenden Abfrage die Liste mit den Dienstprinzipalen von Ihrem Mandanten ab:

    `https://graph.microsoft.com/beta/servicePrincipals`

    Halten Sie sich an das folgende Muster, wenn Sie mehrere Verzeichnisse verwenden: `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`

    ![Graph-Tester-Dialogfeld mit der Abfrage zum Abrufen von Dienstprinzipalen](./media/active-directory-enterprise-app-role-management/graph-explorer-new1.png)

1. Wählen Sie in der Liste mit den abgerufenen Dienstprinzipalen den Dienstprinzipal aus, den Sie ändern möchten. Sie können auch mit STRG+F in den aufgeführten Dienstprinzipalen nach der Anwendung suchen. Suchen Sie nach der Objekt-ID, die Sie von der Seite **Eigenschaften** kopiert haben, und verwenden Sie die folgende Abfrage, um den Dienstprinzipal abzurufen:

    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`

    ![Abfrage zum Abrufen des Dienstprinzipals, den Sie ändern müssen](./media/active-directory-enterprise-app-role-management/graph-explorer-new2.png)

1. Extrahieren Sie die **appRoles**-Eigenschaft aus dem Dienstprinzipalobjekt.

    ![Details zur appRoles-Eigenschaft](./media/active-directory-enterprise-app-role-management/graph-explorer-new3.png)

1. Verwenden Sie die folgenden Schritte, um die vorhandene Rolle zu aktualisieren.

    ![Anforderungstext für „PATCH“ mit Hervorhebung von „description“ und „displayname“](./media/active-directory-enterprise-app-role-management/graph-explorer-patchupdate.png)

    1. Ändern Sie die Methode von **GET** in **PATCH**.

    1. Kopieren Sie die vorhandenen Rollen, und fügen Sie sie unter **Request Body** (Anforderungstext) ein.

    1. Aktualisieren Sie den Wert einer Rolle, indem Sie die Rollenbeschreibung, den Rollenwert oder den Anzeigenamen der Rolle je nach Bedarf ändern.

    1. Wählen Sie **Abfrage ausführen**, nachdem Sie alle erforderlichen Rollen aktualisiert haben.

## <a name="delete-an-existing-role"></a>Löschen einer vorhandenen Rolle

Führen Sie die folgenden Schritte aus, um eine vorhandene Rolle zu löschen:

1. Öffnen Sie den [Microsoft Graph-Tester](https://developer.microsoft.com/graph/graph-explorer) in einem anderen Fenster.

1. Melden Sie sich an der Graph-Tester-Website mit den Anmeldeinformationen des globalen Administrators bzw. Co-Admins für Ihren Mandanten an.

1. Ändern Sie die Version in **Beta**, und rufen Sie mithilfe der folgenden Abfrage die Liste mit den Dienstprinzipalen von Ihrem Mandanten ab:

    `https://graph.microsoft.com/beta/servicePrincipals`

    Halten Sie sich an das folgende Muster, wenn Sie mehrere Verzeichnisse verwenden: `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`

    ![Graph-Tester-Dialogfeld mit der Abfrage zum Abrufen der Liste mit den Dienstprinzipalen](./media/active-directory-enterprise-app-role-management/graph-explorer-new1.png)

1. Wählen Sie in der Liste mit den abgerufenen Dienstprinzipalen den Dienstprinzipal aus, den Sie ändern möchten. Sie können auch mit STRG+F in den aufgeführten Dienstprinzipalen nach der Anwendung suchen. Suchen Sie nach der Objekt-ID, die Sie von der Seite **Eigenschaften** kopiert haben, und verwenden Sie die folgende Abfrage, um den Dienstprinzipal abzurufen:

    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`

    ![Abfrage zum Abrufen des Dienstprinzipals, den Sie ändern müssen](./media/active-directory-enterprise-app-role-management/graph-explorer-new2.png)

1. Extrahieren Sie die **appRoles**-Eigenschaft aus dem Dienstprinzipalobjekt.

    ![Details zur appRoles-Eigenschaft aus dem Dienstprinzipalobjekt](./media/active-directory-enterprise-app-role-management/graph-explorer-new7.png)

1. Verwenden Sie die folgenden Schritte, um die vorhandene Rolle zu löschen.

    ![Anforderungstext für „PATCH“ mit Festlegung von „IsEnabled“ auf „false“](./media/active-directory-enterprise-app-role-management/graph-explorer-new8.png)

    1. Ändern Sie die Methode von **GET** in **PATCH**.

    1. Kopieren Sie die vorhandenen Rollen aus der Anwendung, und fügen Sie sie unter **Request Body** (Anforderungstext) ein.

    1. Legen Sie den Wert **IsEnabled** für die Rolle, die Sie löschen möchten, auf **false** fest.

    1. Wählen Sie **Run Query** (Abfrage ausführen) aus.

    Stellen Sie sicher, dass Sie über die Rolle „msiam_access“ verfügen und die ID in der generierten Rolle übereinstimmt.

1. Löschen Sie diesen Rollenblock aus dem Abschnitt **appRoles**, nachdem die Rolle deaktiviert wurde. Behalten Sie **PATCH** als Methode bei, und wählen Sie **Abfrage ausführen**.

1. Nachdem die Abfrage ausgeführt wurde, wird die Rolle gelöscht.

    Die Rolle muss deaktiviert werden, bevor sie entfernt werden kann.

## <a name="next-steps"></a>Nächste Schritte

Informationen zu den weiteren Schritten finden Sie in der [App-Dokumentation](../saas-apps/tutorial-list.md).

<!--Image references-->

[1]: ./media/active-directory-enterprise-app-role-management/tutorial_general_01.png
[2]: ./media/active-directory-enterprise-app-role-management/tutorial_general_02.png
[3]: ./media/active-directory-enterprise-app-role-management/tutorial_general_03.png
[4]: ./media/active-directory-enterprise-app-role-management/tutorial_general_04.png