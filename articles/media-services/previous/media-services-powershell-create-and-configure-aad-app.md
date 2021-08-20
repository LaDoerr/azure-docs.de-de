---
title: Erstellen einer Azure AD-App mithilfe von PowerShell für den Zugriff auf die Azure Media Services-API | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie mithilfe von PowerShell eine Azure AD-App (Azure Active Directory) erstellen und diese für den Zugriff auf die Azure Media Services-API einrichten.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2021
ms.author: inhenkel
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 47797a071484f01a012af8ff937d5c4deaa93987
ms.sourcegitcommit: e6de87b42dc320a3a2939bf1249020e5508cba94
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/27/2021
ms.locfileid: "114712509"
---
# <a name="use-powershell-to-create-an-azure-ad-app-to-use-with-the-azure-media-services-api"></a>Erstellen einer Azure AD-App mithilfe von PowerShell für die Verwendung mit der Azure Media Services-API

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

[!INCLUDE [v2 deprecation notice](../latest/includes/v2-deprecation-notice.md)]

In diesem Artikel erfahren Sie, wie Sie mithilfe eines PowerShell-Skripts eine Azure AD-Anwendung (Azure Active Directory) und einen Dienstprinzipal erstellen, um auf Azure Media Services-Ressourcen zuzugreifen.  

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto. Beginnen Sie mit einer [kostenlosen Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/), falls Sie kein Konto haben. 
- Media Services-Konto. Weitere Informationen finden Sie unter [Erstellen eines Azure Media Services-Kontos mithilfe des Azure-Portals](media-services-portal-create-account.md).

- Azure PowerShell. Weitere Informationen finden Sie unter [Vorgehensweise zur Verwendung von Azure PowerShell](/powershell/azure/).

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="create-an-azure-ad-app-by-using-powershell"></a>Erstellen einer Azure AD-App mithilfe von PowerShell  

```powershell
Connect-AzAccount
Import-Module Az.Resources
Set-AzContext -SubscriptionId $SubscriptionId
$ServicePrincipal = New-AzADServicePrincipal -DisplayName $ApplicationDisplayName -Password $Password

Get-AzADServicePrincipal -ObjectId $ServicePrincipal.Id 
$NewRole = $null
$Scope = "/subscriptions/your subscription id/resourceGroups/userresourcegroup/providers/microsoft.media/mediaservices/your media account"

$Retries = 0;While ($NewRole -eq $null -and $Retries -le 6)
{
    # Sleep here for a few seconds to allow the service principal application to become active (usually, it will take only a couple of seconds)
    Sleep 15
    New-AzRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Scope | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzRoleAssignment -ServicePrincipalName $ServicePrincipal.ApplicationId -ErrorAction SilentlyContinue
    $Retries++;
}
```

Weitere Informationen finden Sie in den folgenden Artikeln:

- [Erstellen eines Dienstprinzipals für den Zugriff auf Ressourcen mithilfe von Azure PowerShell](../../active-directory/develop/howto-authenticate-service-principal-powershell.md)
- [Hinzufügen oder Entfernen von Azure-Rollenzuweisungen mithilfe von Azure PowerShell](../../role-based-access-control/role-assignments-powershell.md)
- [Vorgehensweise zur manuellen Konfiguration von Daemon-Apps mithilfe von Zertifikaten](https://github.com/azure-samples/active-directory-dotnetcore-daemon-v2)

## <a name="next-steps"></a>Nächste Schritte

Erste Schritte mit dem [Hochladen von Dateien in Ihr Konto](media-services-portal-upload-files.md).
