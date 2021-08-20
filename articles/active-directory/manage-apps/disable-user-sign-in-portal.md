---
title: Deaktivieren von Benutzeranmeldungen für eine Unternehmens-App in Azure AD
description: Deaktivieren einer Unternehmens-App in Azure Active Directory, damit sich Benutzer nicht mehr bei der App anmelden können
services: active-directory
author: davidmu1
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: how-to
ms.date: 04/12/2019
ms.author: davidmu
ms.reviewer: alamaral
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7ca695fc0e84f3db3b98c3e25333d58279ad53f1
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122339629"
---
# <a name="disable-user-sign-ins-for-an-enterprise-app-in-azure-active-directory"></a>Deaktivieren von Benutzeranmeldungen für eine Unternehmens-App in Azure Active Directory

In Azure Active Directory (Azure AD) können Sie auf einfache Weise eine Unternehmensanwendung deaktivieren, damit sich keine Benutzer mehr bei der Anwendung anmelden können. Sie müssen über die entsprechenden Berechtigungen zum Verwalten der Unternehmens-App verfügen. Außerdem müssen Sie ein globaler Administrator für das Verzeichnis sein.

## <a name="how-do-i-disable-user-sign-ins"></a>Wie deaktiviere ich Benutzeranmeldungen?

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) über ein Konto an, das als globaler Administrator für das Verzeichnis konfiguriert ist.
1. Wählen Sie **Alle Dienste** aus, geben Sie **Azure Active Directory** in das Textfeld ein, und drücken Sie die **EINGABETASTE**.
1. Wählen Sie im Bereich **Azure Active Directory** -  **_Verzeichnisname_ *_ (also dem Azure AD-Bereich für das Verzeichnis, das Sie verwalten) die Option _* Unternehmensanwendungen** aus.
1. Im Bereich **Unternehmensanwendungen – Alle Anwendungen** wird eine Liste der Apps angezeigt, die Sie verwalten können. Wählen Sie eine App aus.
1. Wählen Sie im Bereich ***App-Name** _ (dem Bereich mit dem Namen der ausgewählten App im Titel) die Option _*Eigenschaften** aus.
1. Wählen Sie im Bereich ***App-Name** _ – _ *Eigenschaften** für die Option **Aktiviert für die Benutzeranmeldung?** die Einstellung **Nein** aus.
1. Klicken Sie auf **Speichern** .

## <a name="use-azure-ad-powershell-to-disable-an-unlisted-app"></a>Deaktivieren einer nicht aufgelisteten App mit Azure AD PowerShell

Wenn Sie die App-ID einer App kennen, die nicht in der Liste der Unternehmens-Apps angezeigt wird (weil Sie z.B. die App gelöscht haben oder der Dienstprinzipal noch nicht erstellt wurde, da die App von Microsoft vorab autorisiert wurde), können Sie den Dienstprinzipal für die App manuell erstellen und die App dann mit einem [AzureAD PowerShell-Cmdlet](/powershell/module/azuread/New-AzureADServicePrincipal) deaktivieren.

```PowerShell
# The AppId of the app to be disabled
$appId = "{AppId}"

# Check if a service principal already exists for the app
$servicePrincipal = Get-AzureADServicePrincipal -Filter "appId eq '$appId'"
if ($servicePrincipal) {
    # Service principal exists already, disable it
    Set-AzureADServicePrincipal -ObjectId $servicePrincipal.ObjectId -AccountEnabled $false
} else {
    # Service principal does not yet exist, create it and disable it at the same time
    $servicePrincipal = New-AzureADServicePrincipal -AppId $appId -AccountEnabled $false
}
```

## <a name="next-steps"></a>Nächste Schritte

* [Alle meine Gruppen anzeigen](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App](assign-user-or-group-access-portal.md)
* [Entfernen einer Benutzer- oder Gruppenzuweisung aus einer Unternehmens-App](./assign-user-or-group-access-portal.md)
* [Ändern des Namens oder Logos einer Unternehmens-App](./add-application-portal-configure.md)
