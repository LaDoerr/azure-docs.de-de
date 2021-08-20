---
title: 'Schnellstart: Einrichten des SAML-basierten einmaligen Anmeldens für eine Anwendung'
titleSuffix: Azure AD
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie SAML-basiertes einmaliges Anmelden (Single Sign-On, SSO) für eine Anwendung in Ihrem Azure Active Directory-Mandanten (Azure AD-Mandanten) einrichten.
services: active-directory
author: davidmu1
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: quickstart
ms.workload: identity
ms.date: 07/23/2020
ms.author: davidmu
ms.reviewer: ergleenl
ms.openlocfilehash: cded1a874c48cab43e24a19acc6707aa8de7d1e7
ms.sourcegitcommit: d9a2b122a6fb7c406e19e2af30a47643122c04da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2021
ms.locfileid: "114669401"
---
# <a name="quickstart-set-up-saml-based-single-sign-on-for-an-application"></a>Schnellstart: Einrichten des SAML-basierten einmaligen Anmeldens für eine Anwendung

Beginnen Sie mit vereinfachten Benutzeranmeldungen, indem Sie einmaliges Anmelden (Single Sign-On, SSO) für eine Anwendung einrichten, die Sie Ihrem Azure Active Directory-Mandanten (Azure AD-Mandanten) hinzugefügt haben. Nachdem Sie SSO eingerichtet haben, können sich Ihre Benutzer mit ihren Azure AD-Anmeldeinformationen bei einer Anwendung anmelden. SSO ist in der kostenlosen Edition von Azure AD enthalten.

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen Folgendes, um SSO für eine Anwendung einzurichten, die Sie Ihrem Azure AD-Mandanten hinzugefügt haben:

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Eine der folgenden Rollen: Globaler Administrator, Cloudanwendungsadministrator, Anwendungsadministrator oder Besitzer des Dienstprinzipals.
- Eine Anwendung, die SSO unterstützt und bereits vorkonfiguriert und dem Azure AD-Katalog hinzugefügt wurde. Bei den meisten Apps kann Azure AD für SSO verwendet werden. Die Apps im Azure AD-Katalog sind vorkonfiguriert. Falls Ihre App nicht aufgelistet oder eine benutzerdefinierte App ist, können Sie sie trotzdem mit Azure AD verwenden. Sehen Sie sich die Tutorials und die restliche Dokumentation im Inhaltsverzeichnis an. In dieser Schnellstartanleitung liegt der Schwerpunkt auf Apps, die von den App-Entwicklern für SSO vorkonfiguriert und dem Azure AD-Katalog hinzugefügt wurden.
- Optional: Gehen Sie den Schnellstart [Anzeigen Ihrer Apps](view-applications-portal.md) durch.
- Optional: Gehen Sie den Schnellstart [Hinzufügen einer App](add-application-portal.md) durch.
- Optional: Gehen Sie den Schnellstart [Konfigurieren einer App](add-application-portal-configure.md) durch.
- Optional: Arbeiten Sie den Artikel [Zuweisen von Benutzern zu einer App](add-application-portal-assign-users.md) durch.

>[!IMPORTANT]
>Verwenden Sie zum Testen der in dieser Schnellstartanleitung aufgeführten Schritte keine Produktionsumgebung.

## <a name="enable-single-sign-on-for-an-app"></a>Aktivieren des einmaligen Anmeldens für eine App

Nachdem Sie Ihrem Azure AD-Mandanten eine Anwendung hinzugefügt haben, wird die Übersichtsseite angezeigt. Wenn Sie eine Anwendung konfigurieren, die bereits hinzugefügt wurde, lesen Sie die erste Schnellstartanleitung. Darin wird beschrieben, wie Sie die Ihrem Mandanten hinzugefügten Anwendungen anzeigen können.

Richten Sie einmaliges Anmelden wie folgt für eine Anwendung ein:

1. Wählen Sie im Azure AD-Portal die Option **Unternehmensanwendungen** aus. Suchen Sie dann nach der Anwendung, für die Sie einmaliges Anmelden einrichten möchten, und wählen Sie diese aus.
1. Wählen Sie im Abschnitt **Verwalten** die Option **Einmaliges Anmelden** aus, um den Bereich **Einmaliges Anmelden** zur Bearbeitung zu öffnen.

    > [!IMPORTANT]
    > Wenn die App den OIDC-Standard (OpenID Connect) für einmaliges Anmelden nutzt, wird in der Navigation keine Option für einmaliges Anmelden angezeigt. Weitere Informationen zum Einrichten dieser Option finden Sie in der Schnellstartanleitung für OIDC-basiertes einmaliges Anmelden.

1. Wählen Sie **SAML** aus, um die Seite für die SSO-Konfiguration zu öffnen. In diesem Beispiel ist GitHub die Anwendung, für die wir SSO konfigurieren. Nachdem Sie GitHub eingerichtet haben, können sich Ihre Benutzer mit ihren Anmeldeinformationen von Ihrem Azure AD-Mandanten bei GitHub anmelden.

    :::image type="content" source="media/add-application-portal-setup-sso/github-sso.png" alt-text="Screenshot der Seite zum Konfigurieren des einmaligen Anmeldens für GitHub":::

1. Der Prozess zum Konfigurieren einer Anwendung für die Verwendung von Azure AD für SAML-basiertes SSO variiert je nach Anwendung. Es ist ein Link zur Anleitung für GitHub vorhanden. Anleitungen für andere Apps finden Sie unter [Tutorials zur Integration von SaaS-Anwendungen in Azure Active Directory](/azure/active-directory/saas-apps/).
1. Befolgen Sie die Anleitung zum Einrichten von SSO für die Anwendung. Viele Anwendungen verfügen über spezifische Abonnementanforderungen für die SSO-Funktionen. Beispielsweise ist für GitHub ein Enterprise-Abonnement erforderlich.
    > [!TIP]
    > Weitere Informationen zu den SAML-Konfigurationsoptionen finden Sie unter [Konfigurieren des SAML-basierten einmaligen Anmeldens](configure-saml-single-sign-on.md).

> [!TIP]
> Sie können die App-Verwaltung mithilfe der Graph-API automatisieren. Informationen hierzu finden Sie unter [Automatisieren einer SAML-basierten SSO-App-Konfiguration mit Microsoft Graph-API](/graph/application-saml-sso-configure-api).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie diese Schnellstartserie abgeschlossen haben, sollten Sie die App löschen, um Ihren Testmandanten zu bereinigen. Das Löschen der App wird in der letzten Schnellstartanleitung dieser Reihe behandelt. Informationen finden Sie unter [Schnellstart: Löschen einer Anwendung von Ihrem Azure AD-Mandanten (Azure Active Directory)](delete-application-portal.md).

## <a name="next-steps"></a>Nächste Schritte

Im nächsten Artikel erfahren Sie, wie Sie eine App löschen:
> [!div class="nextstepaction"]
> [Löschen einer App](delete-application-portal.md)
