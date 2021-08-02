---
title: Bereitstellen benutzerdefinierter Richtlinien mit Azure Pipelines
titleSuffix: Azure AD B2C
description: Hier erfahren Sie, wie Sie in Azure AD B2C mithilfe von Azure Pipelines benutzerdefinierte Richtlinien in einer CI/CD-Pipeline bereitstellen.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 06/01/2021
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 470fcebf33e995d4c81d916970d80015b8f7c8f6
ms.sourcegitcommit: 7f59e3b79a12395d37d569c250285a15df7a1077
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/02/2021
ms.locfileid: "110783749"
---
# <a name="deploy-custom-policies-with-azure-pipelines"></a>Bereitstellen benutzerdefinierter Richtlinien mit Azure Pipelines

[Azure Pipelines](/azure/devops/pipelines) kombiniert Continuous Integration (CI) und Continuous Delivery (CD), um den Code kontinuierlich und konsistent zu testen und zu erstellen und auf einem beliebigen Ziel bereitzustellen. In diesem Artikel wird beschrieben, wie Sie den Bereitstellungsprozess der [benutzerdefinierten Richtlinien](user-flow-overview.md) in Azure Active Directory B2C (Azure AD B2C) mithilfe von Azure Pipelines automatisieren.

> [!IMPORTANT]
> Bei der Verwaltung von benutzerdefinierten Richtlinien in Azure AD B2C mit Azure Pipelines werden derzeit **Vorschau**-Vorgänge verwendet, die im `/beta`-Endpunkt der Microsoft Graph-API zur Verfügung stehen. Die Verwendung dieser APIs in Produktionsanwendungen wird nicht unterstützt. Weitere Informationen finden Sie in der [Microsoft Graph-REST-API-Beta-Endpunkt-Referenz](/graph/api/overview?toc=.%2fref%2ftoc.json&view=graph-rest-beta&preserve-view=true).

## <a name="prerequisites"></a>Voraussetzungen

* Führen Sie die unter [Erste Schritte mit benutzerdefinierten Richtlinien in Azure Active Directory B2C](tutorial-create-user-flows.md) beschriebenen Schritte aus.
* Wenn Sie noch keine DevOps-Organisation erstellt haben, erstellen Sie eine, indem Sie die Anweisungen unter [Registrieren, Anmelden bei Azure DevOps](/azure/devops/user-guide/sign-up-invite-teammates) befolgen.  

## <a name="register-an-application-for-management-tasks"></a>Registrieren einer Anwendung für Verwaltungstasks

Sie verwenden das PowerShell-Skript, um die Azure AD B2C-Richtlinien bereitzustellen. Bevor das PowerShell-Skript mit der [Microsoft Graph-API](microsoft-graph-operations.md) interagieren kann, erstellen Sie eine Anwendungsregistrierung in Ihrem Azure AD B2C-Mandanten. Wenn dies noch nicht erfolgt ist, [registrieren Sie eine Microsoft Graph-Anwendung](microsoft-graph-get-started.md).

Damit das PowerShell-Skript auf Daten in Microsoft Graph zugreifen kann, erteilen Sie der registrierten Anwendung die entsprechenden [Anwendungsberechtigungen](/graph/permissions-reference). In den **API-Berechtigungen** der App-Registrierung unter **Microsoft Graph** > **Richtlinie** >  wurde die Berechtigung **Policy.ReadWrite.TrustFramework** erteilt.

## <a name="configure-an-azure-repo"></a>Konfigurieren eines Azure-Repository

Nach dem Registrieren einer Microsoft Graph-Anwendung können Sie ein Repository für Ihre Richtliniendateien konfigurieren.

1. Melden Sie sich bei Ihrer [Azure DevOps-Organisation](https://azure.microsoft.com/services/devops/) an.
1. [Erstellen Sie ein neues Projekt][devops-create-project], oder wählen Sie ein vorhandenes Projekt aus.
1. Navigieren Sie in Ihrem Projekt zu **Repos**, und wählen Sie **Dateien** aus. 
1. Wählen Sie ein vorhandenes Repository aus, oder erstellen Sie ein Repository.
1. Erstellen Sie im Stammverzeichnis Ihres Repositorys einen Ordner mit dem Namen `B2CAssets`. Fügen Sie Ihre benutzerdefinierten Azure AD B2C-Richtliniendateien dem Ordner *B2CAssets* hinzu.
1. Erstellen Sie im Stammverzeichnis Ihres Repositorys einen Ordner mit dem Namen `Scripts`. Erstellen Sie eine PowerShell-Datei *DeployToB2C.ps1*. Fügen Sie das folgende PowerShell-Skript in die Datei *DeployToB2C.ps1* ein. 
1. **Committen** und **pushen** Sie die Änderungen.

Das folgende Skript erhält ein Zugriffstoken von Azure AD. Mit dem Token ruft das Skript die Microsoft Graph-API auf, um die Richtlinien in den Ordner *B2CAssets* hochzuladen. Sie können auch den Inhalt der Richtlinie ändern, bevor Sie sie hochladen. Ersetzen Sie z. B. `tenant-name.onmicrosoft.com` durch den Namen Ihres Mandanten.

```PowerShell
[Cmdletbinding()]
Param(
    [Parameter(Mandatory = $true)][string]$ClientID,
    [Parameter(Mandatory = $true)][string]$ClientSecret,
    [Parameter(Mandatory = $true)][string]$TenantId,
    [Parameter(Mandatory = $true)][string]$PolicyId,
    [Parameter(Mandatory = $true)][string]$PathToFile
)

try {
    $body = @{grant_type = "client_credentials"; scope = "https://graph.microsoft.com/.default"; client_id = $ClientID; client_secret = $ClientSecret }

    $response = Invoke-RestMethod -Uri https://login.microsoftonline.com/$TenantId/oauth2/v2.0/token -Method Post -Body $body
    $token = $response.access_token

    $headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]"
    $headers.Add("Content-Type", 'application/xml')
    $headers.Add("Authorization", 'Bearer ' + $token)

    $graphuri = 'https://graph.microsoft.com/beta/trustframework/policies/' + $PolicyId + '/$value'
    $policycontent = Get-Content $PathToFile

    # Optional: Change the content of the policy. For example, replace the tenant-name with your tenant name.
    # $policycontent = $policycontent.Replace("your-tenant.onmicrosoft.com", "contoso.onmicrosoft.com")     

    $response = Invoke-RestMethod -Uri $graphuri -Method Put -Body $policycontent -Headers $headers

    Write-Host "Policy" $PolicyId "uploaded successfully."
}
catch {
    Write-Host "StatusCode:" $_.Exception.Response.StatusCode.value__

    $_

    $streamReader = [System.IO.StreamReader]::new($_.Exception.Response.GetResponseStream())
    $streamReader.BaseStream.Position = 0
    $streamReader.DiscardBufferedData()
    $errResp = $streamReader.ReadToEnd()
    $streamReader.Close()

    $ErrResp

    exit 1
}

exit 0
```

## <a name="configure-azure-pipelines"></a>Konfigurieren von Azure Pipelines

Nachdem Sie Ihr Repository initialisiert und mit Ihren benutzerdefinierten Richtliniendateien aufgefüllt haben, können Sie die Freigabepipeline einrichten. Führen Sie die folgenden Schritte aus, um eine Pipeline zu erstellen:

1. Wählen Sie in Ihrem Projekt **Pipelines** > **Freigaben** > **Neue Pipeline** aus.
1. Wählen Sie unter **Vorlage auswählen** die Option **Leerer Auftrag** und dann **Anwenden** aus.
1. Geben Sie einen **Phasennamen** (z. B. *DeployCustomPolicies*) ein, und schließen Sie dann den Bereich.
1. Wählen Sie **Artefakt hinzufügen** aus, und wählen Sie unter **Quelltyp** die Option **Azure-Repository** aus.
    1. Wählen Sie unter **Projekt** Ihr Projekt aus.
    1. Wählen Sie die **Quelle (Repository)** aus, die den Ordner *Skripts* enthält.
    1. Wählen Sie einen **Standardbranch** aus, z. B. *master*.
    1. Übernehmen Sie unter **Standardversion** die Einstellung *Neuestes Element aus dem Standardbranch*.
    1. Geben Sie für das Repository einen **Quellalias** ein, zum Beispiel *policyRepo*. 
1. Wählen Sie **Hinzufügen** aus.
1. Benennen Sie die Pipeline um, und geben Sie ihr einen aussagekräftigen Namen, zum Beispiel *Deploy Custom Policy Pipeline*.
1. Wählen Sie zum Speichern der Pipelinekonfiguration **Speichern** aus.

### <a name="configure-pipeline-variables"></a>Konfigurieren von Pipelinevariablen

Die Pipelinevariablen stellen eine bequeme Möglichkeit dar, wichtige Bits von Daten in verschiedene Teile der Pipeline zu übernehmen. Die folgenden Variablen enthalten Informationen zu Ihrer Azure AD B2C-Umgebung.

| Name | Wert |
| ---- | ----- |
| `clientId` | **Anwendungs-ID (Client)** der zuvor von Ihnen registrierten Anwendung. |
| `clientSecret` | Wert des **geheimen Clientschlüssels**, den Sie zuvor erstellt haben. <br /> Ändern Sie den Variablentyp in **Geheimnis** (wählen Sie das Schlosssymbol aus). |
| `tenantId` | `your-b2c-tenant.onmicrosoft.com`, wobei *your-b2c-tenant* dem Namen Ihres Azure AD B2C-Mandanten entspricht. |

Führen Sie die folgenden Schritte aus, um Pipelinevariablen hinzuzufügen:

1. Wählen Sie in Ihrer Pipeline die Registerkarte **Variablen** aus.
1. Fügen Sie unter **Pipelinevariablen** die obige Variable mit ihren Werten hinzu.
1. Wählen Sie **Speichern** aus, um die Variablen zu speichern.

### <a name="add-pipeline-tasks"></a>Hinzufügen von Pipelinetasks

Eine Pipelineaufgabe ist ein vorgepacktes Skript, das eine Aktion ausführt. Fügen Sie eine Aufgabe hinzu, die das PowerShell-Skript *DeployToB2C.ps1* aufruft.

1. Wählen Sie in der Pipeline, die Sie erstellt haben, die Registerkarte **Aufgaben** aus.
1. Wählen Sie **Agent-Auftrag** und dann das Pluszeichen ( **+** ) aus, um dem Agent-Auftrag einen Task hinzuzufügen.
1. Suchen Sie nach dem Eintrag **PowerShell**, und wählen Sie ihn aus. Wählen Sie nicht „Azure PowerShell“, „PowerShell auf Zielcomputern“ oder einen anderen Eintrag im Zusammenhang mit PowerShell aus.
1. Wählen Sie den neu hinzugefügten Task **PowerShell-Skript** aus.
1. Geben Sie für den Task „PowerShell-Skript“ die folgenden Werte ein:
    * **Taskversion**: 2.*
    * **Anzeigename**: Der Name der Richtlinie, die dieser Task hochladen soll, zum Beispiel *B2C_1A_TrustFrameworkBase*.
    * **Typ:** Dateipfad
    * **Skriptpfad:** Wählen Sie die Auslassungspunkte (**_..._* _) aus, navigieren Sie zum Ordner _Skripts*, und wählen Sie dann die Datei *DeployToB2C.ps1* aus.
    * **Argumente**:

        Geben Sie als **Argumente** die folgenden Werte ein. Ersetzen Sie `{alias-name}` durch den Alias, den Sie im vorherigen Abschnitt angegeben haben. Ersetzen Sie `{policy-id}` durch den Richtliniennamen. Ersetzen Sie `{policy-file-name}` durch den Namen der Richtliniendatei.

        Die erste Richtlinie, die Sie hochladen, muss *TrustFrameworkBase.xml* sein.

        ```PowerShell
        -ClientID $(clientId) -ClientSecret $(clientSecret) -TenantId $(tenantId) -PolicyId {policy-id} -PathToFile $(System.DefaultWorkingDirectory)/{alias-name}/B2CAssets/{policy-file-name}
        ```

        `PolicyId` ist ein Wert, der sich am Anfang jeder XML-Richtliniendatei im TrustFrameworkPolicy-Knoten befindet. Beispiel: `PolicyId` in der folgenden XML-Richtliniendatei lautet *B2C_1A_TrustFrameworkBase*:

        ```xml
        <TrustFrameworkPolicy
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xmlns:xsd="http://www.w3.org/2001/XMLSchema"
        xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06"
        PolicySchemaVersion="0.3.0.0"
        TenantId="your-tenant.onmicrosoft.com"
        PolicyId= "B2C_1A_TrustFrameworkBase"
        PublicPolicyUri="http://your-tenant.onmicrosoft.com/B2C_1A_TrustFrameworkBase">
        ```

        Ihre endgültigen Argumente sollten nun wie im folgenden Beispiel gezeigt aussehen:

        ```PowerShell
        -ClientID $(clientId) -ClientSecret $(clientSecret) -TenantId $(tenantId) -PolicyId B2C_1A_TrustFrameworkBase -PathToFile $(System.DefaultWorkingDirectory)/policyRepo/B2CAssets/TrustFrameworkBase.xml
        ```

1. Wählen Sie **Speichern** aus, um den Agent-Auftrag zu speichern.

## <a name="test-your-pipeline"></a>Testen Ihrer Pipeline

So testen Sie Ihre Pipeline

1. Wählen Sie **Pipelines** und anschließend **Freigaben** aus.
1. Wählen Sie die zuvor erstellte Pipeline (z. B. *DeployCustomPolicies*) aus.
1. Wählen Sie **Freigabe erstellen** und dann **Erstellen** aus, um die Freigabe in die Warteschlange einzureihen.

Es sollte ein Benachrichtigungsbanner mit dem Hinweis angezeigt werden, dass eine Freigabe in die Warteschlange gestellt wurde. Um deren Status anzuzeigen, wählen Sie den Link im Benachrichtigungsbanner oder die Freigabe in der Liste auf der Registerkarte **Freigaben** aus.

## <a name="add-more-pipeline-tasks"></a>Hinzufügen weiterer Pipelinetasks

Wiederholen Sie die [vorherigen Schritte](#add-pipeline-tasks) für jede der benutzerdefinierten Richtliniendateien, um die restlichen Richtlinien bereitzustellen.

Stellen Sie beim Ausführen der Agents und beim Hochladen der Richtliniendateien unbedingt die richtige Reihenfolge sicher:

1. *TrustFrameworkBase.xml*
1. *TrustFrameworkExtensions.xml*
1. *SignUpOrSignin.xml*
1. *ProfileEdit.xml*
1. *PasswordReset.xml*

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen:

* [Dienst-zu-Dienst-Aufrufe mit Clientanmeldeinformationen](../active-directory/develop/v2-oauth2-client-creds-grant-flow.md)
* [Azure DevOps Services](/azure/devops/user-guide/)

<!-- LINKS - External -->
[devops]: /azure/devops/
[devops-create-project]:  /azure/devops/organizations/projects/create-project
[devops-pipelines]: /azure/devops/pipelines
