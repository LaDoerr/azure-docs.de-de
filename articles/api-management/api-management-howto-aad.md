---
title: Autorisieren von Entwicklerkonten mithilfe von Azure Active Directory
titleSuffix: Azure API Management
description: Erfahren Sie, wie Benutzer mithilfe von Azure Active Directory in Azure API Management autorisiert werden.
services: api-management
documentationcenter: API Management
author: miaojiang
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 03/22/2021
ms.author: apimpm
ms.openlocfilehash: 37a0ac51f5cbc7d3eaa98027b5e8568dfcf208ee
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111747705"
---
# <a name="authorize-developer-accounts-by-using-azure-active-directory-in-azure-api-management"></a>Autorisieren von Entwicklerkonten mithilfe von Azure Active Directory in Azure API Management

Dieser Artikel zeigt, wie Sie den Zugriff auf das Entwicklerportal für Benutzer aus Azure Active Directory (Azure AD) aktivieren. Außerdem erfahren Sie, wie Sie Azure AD-Benutzergruppen verwalten, indem Sie externe Gruppen hinzufügen, die die Benutzer enthalten.

## <a name="prerequisites"></a>Voraussetzungen

- Bearbeiten Sie den folgenden Schnellstart: [Erstellen einer neuen Azure API Management-Dienstinstanz](get-started-create-service-instance.md)
- Importieren und veröffentlichen Sie eine Azure API Management-Instanz. Weitere Informationen finden Sie unter [Importieren und Veröffentlichen](import-and-publish.md).

[!INCLUDE [premium-dev-standard.md](../../includes/api-management-availability-premium-dev-standard.md)]

## <a name="authorize-developer-accounts-by-using-azure-ad"></a>Autorisieren von Entwicklerkonten mithilfe von Azure AD

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 
2. Select ![Pfeilsymbol](./media/api-management-howto-aad/arrow.png).
3. Geben Sie im Suchfeld **api** ein.
4. Wählen Sie **API Management-Dienste** aus.
5. Wählen Sie Ihre API Management-Dienstinstanz aus.
6. Wählen Sie unter **Entwicklerportal** die Option **Identitäten** aus.
7. Wählen Sie oben **+ Hinzufügen** aus.

    Auf der rechten Seite wird der Bereich **Identitätsanbieter hinzufügen** geöffnet.
8. Wählen Sie für **Anbietertyp** die Option **Azure Active Directory** aus.

    Im Bereich werden Steuerelemente angezeigt, mit denen Sie weitere erforderliche Informationen eingeben können. Zu den Steuerelementen gehören **Client-ID** und **Clientgeheimnis**. (Sie erhalten weiter unten in diesem Artikel Informationen zu diesen Steuerelementen.)
9. Notieren Sie sich den Inhalt der **Umleitungs-URL**.
    

    :::image type="content" source="media/api-management-howto-aad/api-management-with-aad001.png" alt-text="Identitätsanbieter im Azure-Portal hinzufügen":::
    > [!NOTE]
    > Es gibt zwei Umleitungs-URLs:<br/>
    > **Umleitungs-URL**-Punkten zum neuesten Entwicklerportal der API Management.<br/>
    > **Umleitungs-URL (veraltet Portal)** : verweist auf das als veraltet markierte Entwicklerportal von API Management.
    >
    > Es wird empfohlen, die neueste Umleitungs-URL des Entwicklerportals zu verwenden.
   
10. Öffnen Sie in Ihrem Browser eine weitere Registerkarte. 
11. Navigieren Sie im Microsoft Azure-Portal zu [App-Registrierungen](https://go.microsoft.com/fwlink/?linkid=2083908), um eine App in Active Directory Domain Services zu registrieren.
12. Wählen Sie unter **Verwalten** die Option **App-Registrierungen** aus.
13. Wählen Sie **Neue Registrierung** aus. Legen Sie auf der Seite **Anwendung registrieren** folgende Werte fest:
    
    * Legen Sie für **Name** einen aussagekräftigen Namen fest, z. B. *Entwicklerportal*.
    * Legen Sie **Unterstützte Kontotypen** auf **Nur Konten in diesem Organisationsverzeichnis** fest. 
    * Legen Sie **Umleitungs-URI** auf den Wert aus Schritt 9 fest. 
    * Klicken Sie auf **Registrieren**. 

14.  Kopieren Sie nach der Registrierung der Anwendung die **Anwendungs-ID (Client)** von der Seite **Übersicht**. 
15. Navigieren Sie zurück zur API Management-Instanz. Fügen Sie den Wert **Anwendungs-ID (Client)** im Fenster **Identitätsanbieter hinzufügen** in das Feld **Client-ID** ein.
16. Wechseln Sie zur Konfiguration von Azure AD zurück, und wählen Sie **Certificates & secrets** (Zertifikate und Geheimnisse) unter **Verwalten** aus. Klicken Sie auf **Neuer geheimer Clientschlüssel**. Geben Sie für **Beschreibung** einen Wert ein, wählen Sie eine Option für **Gültig bis** aus, und klicken Sie auf **Hinzufügen**. Kopieren Sie den Wert für den geheimen Clientschlüssel, bevor Sie die Seite verlassen. Sie benötigen diese Angaben im nächsten Schritt. 
17. Klicken Sie unter **Verwalten** auf **Authentifizierung**, und legen Sie **ID-Token** unter **Implizite Genehmigung** fest.
18. Wechseln Sie zur API Management-Instanz zurück, und fügen Sie den geheimen Clientschlüssel in das Feld **Geheimer Clientschlüssel** ein.

    > [!IMPORTANT]
    > Aktualisieren Sie unbedingt den **geheimen Clientschlüssel**, bevor der Schlüssel abläuft. 
    >  
    >

19. Das Fenster **Identitätsanbieter hinzufügen** enthält auch das Textfeld **Zulässige Mandanten**. Geben Sie die in diesem Feld die Domänen der Azure AD-Instanzen ein, für die Sie den APIs der API Management-Dienstinstanz Zugriff gewähren möchten. Sie können mehrere Domänen durch neue Zeilen, Leerzeichen oder Kommas trennen.

    > [!NOTE]
    > Sie können im Abschnitt **Zulässige Mandanten** mehrere Domänen angeben. Bevor sich Benutzer aus einer anderen Domäne als der ursprünglichen Domäne (der Domäne, in der die Anwendung registriert wurde) anmelden können, muss ein globaler Administrator dieser anderen Domäne der Anwendung Berechtigungen für den Verzeichnisdatenzugriff erteilen. Sie müssen als globaler Administrator folgendermaßen vorgehen, um Berechtigungen zu erteilen: a) Wechseln Sie zu `https://<URL of your developer portal>/aadadminconsent` (z. B. https://contoso.portal.azure-api.net/aadadminconsent).
    > b. Geben Sie den Domänennamen des Azure AD-Mandanten ein, auf den Zugriff erteilt werden soll.
    > c. Klicken Sie auf **Submit** (Senden). 

20.  Nachdem Sie die gewünschte Konfiguration angegeben haben, wählen Sie **Hinzufügen** aus.

Nachdem die Änderungen gespeichert wurden, können sich Benutzer der angegebenen Azure AD-Instanz beim Entwicklerportal anmelden, indem sie die unter [Anmelden beim Entwicklerportal mit einem Azure AD-Konto](#log_in_to_dev_portal) beschriebenen Schritte ausführen.

## <a name="add-an-external-azure-ad-group"></a>Hinzufügen einer externen Azure AD-Gruppe

Nachdem Sie den Zugriff für Benutzer in einem Azure AD-Mandanten aktiviert haben, können Sie Azure AD-Gruppen in API Management hinzufügen. Als Ergebnis können Sie die Produktsichtbarkeit mithilfe von Azure AD-Gruppen steuern.

Wenn Sie in APIM eine externe Azure AD-Gruppe hinzufügen möchten, müssen Sie zunächst den vorherigen Abschnitt durchlaufen. Standardmäßig hat die Anwendung, die Sie registriert haben, Zugriff auf die Microsoft Graph-API mit der erforderlichen delegierten Berechtigung `User.Read`. Sie müssen der Anwendung auch Zugriff auf die Microsoft Graph-API und die Azure Active Directory Graph-API mit der Anwendungsberechtigung `Directory.Read.All` gewähren, indem Sie die folgenden Schritte ausführen: 

1. Wechseln Sie zurück zu Ihrer App-Registrierung, die im vorherigen Abschnitt erstellt wurde.
2. Wählen Sie **API-Berechtigungen** und anschließend **Berechtigung hinzufügen** aus. 
3. Wählen Sie im Bereich **API-Berechtigungen anfordern**  die Registerkarte **Microsoft-APIs** und dann die Kachel **Microsoft Graph** aus. Wählen Sie **Anwendungsberechtigungen** aus und suchen Sie nach **Verzeichnis**. Wählen Sie die Berechtigung **Directory.Read.All** und dann unten im Bereich **Berechtigungen hinzufügen** aus.
4. Wählen Sie **Berechtigung hinzufügen** aus. 
5. Wählen Sie im Bereich **API-Berechtigungen anfordern**  die Registerkarte **Microsoft-APIs** aus, scrollen Sie nach unten und wählen Sie dann die Kachel **Azure Active Directory Graph** aus. Wählen Sie **Anwendungsberechtigungen** aus und suchen Sie nach **Verzeichnis**. Wählen Sie die Berechtigung **Directory.Read.All** und dann **Berechtigungen hinzufügen** aus. 
6. Wählen Sie **Administratoeinwilligung für {tenantname} erteilen** aus, damit Sie allen Benutzern in diesem Verzeichnis den Zugriff gewähren. 

Jetzt können Sie externe Azure AD-Gruppen über die Registerkarte **Gruppen** Ihrer API Management-Instanz hinzufügen.

1. Wählen Sie die Registerkarte **Gruppen** .
2. Klicken Sie auf die Schaltfläche **AAD-Gruppe hinzufügen**.

   ![Schaltfläche „AAD-Gruppe hinzufügen“](./media/api-management-howto-aad/api-management-with-aad008.png)
3. Wählen Sie die Gruppe aus, die Sie hinzufügen möchten.
4. Klicken Sie auf die Schaltfläche **Auswählen**.

Nachdem Sie eine externe Azure AD-Gruppe hinzugefügt haben, können Sie ihre Eigenschaften überprüfen und konfigurieren. Wählen Sie den Namen der Gruppe auf der Registerkarte **Gruppen** aus. Von hier aus können Sie Informationen wie z.B. **Name** und **Beschreibung** der Gruppe bearbeiten.
 
Benutzer aus der konfigurierten Azure AD-Instanz können sich jetzt beim Entwicklerportal anmelden. Sie können beliebige Gruppen anzeigen und abonnieren, die für sie sichtbar sind.

## <a name="developer-portal---add-azure-ad-account-authentication"></a><a id="log_in_to_dev_portal"></a> Entwicklerportal – Hinzufügen der Azure AD-Kontoauthentifizierung

Im Entwicklerportal ist die Anmeldung bei AAD über das Widget **Schaltfläche „Anmelden“: OAuth** möglich. Das Widget ist auf der Anmeldeseite des standardmäßigen Entwicklerportals bereits integriert.

Obwohl ein neues Konto automatisch erstellt wird, wenn sich ein neuer Benutzer bei AAD anmeldet, können Sie das gleiche Widget auf der Anmeldeseite hinzufügen.

Das Widget **Registrierungsformular: OAuth** stellt ein Formular dar, das zum Registrieren bei OAuth verwendet wird.

> [!IMPORTANT]
> Sie müssen [das Portal](api-management-howto-developer-portal-customize.md#publish) erneut veröffentlichen, damit die AAD-Änderungen wirksam werden.

## <a name="legacy-developer-portal---how-to-sign-in-with-azure-ad"></a>Altes Entwicklerportal: Anmelden mit Azure AD

[!INCLUDE [api-management-portal-legacy.md](../../includes/api-management-portal-legacy.md)]

So melden Sie sich mit einem in den vorherigen Abschnitten konfigurierten Azure AD-Konto beim Entwicklerportal an

1. Öffnen Sie ein neues Browserfenster, indem Sie die Anmelde-URL aus der Active Directory-Anwendungskonfiguration verwenden, und wählen Sie **Azure Active Directory** aus.

   ![Anmeldeseite][api-management-dev-portal-signin]

1. Geben Sie die Anmeldeinformationen eines Azure AD-Benutzers ein, und wählen Sie **Anmelden** aus.

   ![Anmeldung mit Benutzername und Kennwort][api-management-aad-signin]

1. Es wird möglicherweise ein Registrierungsformular angezeigt, wenn weitere Informationen erforderlich sind. Vervollständigen Sie das Registrierungsformular, und wählen Sie **Registrieren**.

   ![Schaltfläche „Anmelden“ auf dem Registrierungsformular][api-management-complete-registration]

Sie sind jetzt beim Entwicklerportal für Ihre API Management-Dienstinstanz angemeldet.

![Entwicklerportal nach Abschluss der Registrierung][api-management-registration-complete]

[api-management-dev-portal-signin]: ./media/api-management-howto-aad/api-management-dev-portal-signin.png
[api-management-aad-signin]: ./media/api-management-howto-aad/api-management-aad-signin.png
[api-management-complete-registration]: ./media/api-management-howto-aad/api-management-complete-registration.png
[api-management-registration-complete]: ./media/api-management-howto-aad/api-management-registration-complete.png

[How to add operations to an API]: ./mock-api-responses.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: get-started-create-service-instance.md
[API Management policy reference]: ./api-management-policies.md
[Caching policies]: ./api-management-policies.md#caching-policies
[Create an API Management service instance]: get-started-create-service-instance.md

[https://oauth.net/2/]: https://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

[Sign in to the developer portal by using an Azure AD account]: #Sign-in-to-the-developer-portal-by-using-an-Azure-AD-account
