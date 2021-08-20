---
title: Datei einfügen
description: Datei einfügen
services: azure-communication-services
author: tomaschladek
manager: nmurav
ms.service: azure-communication-services
ms.subservice: azure-communication-services
ms.date: 06/30/2021
ms.topic: include
ms.custom: include file
ms.author: tchladek
ms.openlocfilehash: 70e49ef548515e842b3838c892570622c8fe950f
ms.sourcegitcommit: d2738669a74cda866fd8647cb9c0735602642939
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2021
ms.locfileid: "113659380"
---
> [!NOTE]
> Den fertigen Code für diesen Schnellstart finden Sie auf [GitHub](https://github.com/Azure-Samples/communication-services-dotnet-quickstarts/tree/main/AccessTokensQuickstart).

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Die aktuelle Version des [.NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core) für Ihr Betriebssystem.
- Eine aktive Communication Services-Ressource und eine Verbindungszeichenfolge. [Erstellen Sie eine Communication Services-Ressource.](../create-communication-resource.md)

## <a name="setting-up"></a>Einrichten

### <a name="create-a-new-c-application"></a>Erstellen einer neuen C#-Anwendung

Verwenden Sie in einem Konsolenfenster (z. B. cmd, PowerShell oder Bash) den Befehl `dotnet new` zum Erstellen einer neuen Konsolen-App mit dem Namen `AccessTokensQuickstart`. Dieser Befehl erstellt ein einfaches „Hallo Welt“-C#-Projekt mit einer einzigen Quelldatei: **Program.cs**.

```console
dotnet new console -o AccessTokensQuickstart
```

Wechseln Sie zum neu erstellten App-Ordner, und verwenden Sie den Befehl `dotnet build`, um Ihre Anwendung zu kompilieren.

```console
cd AccessTokensQuickstart
dotnet build
```

### <a name="install-the-package"></a>Installieren des Pakets

Installieren Sie im Anwendungsverzeichnis mithilfe des Befehls `dotnet add package` die Azure Communication Services-Identitätsbibliothek für das .NET-Paket.

```console
dotnet add package Azure.Communication.Identity --version 1.0.0
```

### <a name="set-up-the-app-framework"></a>Einrichten des App-Frameworks

Über das Projektverzeichnis:

1. Öffnen Sie die Datei **Program.cs** in einem Text-Editor.
1. Fügen Sie eine Anweisung vom Typ `using` hinzu, um den Namespace `Azure.Communication.Identity` einzuschließen.
1. Aktualisieren der `Main`-Methodendeklaration zur Unterstützung von asynchronem Code

Verwenden Sie zum Einstieg den folgenden Code:

```csharp
using System;
using Azure;
using Azure.Core;
using Azure.Communication.Identity;

namespace AccessTokensQuickstart
{
    class Program
    {
        static async System.Threading.Tasks.Task Main(string[] args)
        {
            Console.WriteLine("Azure Communication Services - Access Tokens Quickstart");

            // Quickstart code goes here
        }
    }
}
```
## <a name="authenticate-the-client"></a>Authentifizieren des Clients

Initialisieren Sie einen Kommunikationsidentitätsclient (`CommunicationIdentityClient`) mit Ihrer Verbindungszeichenfolge. Im folgenden Code wird die Verbindungszeichenfolge für die Ressource aus einer Umgebungsvariablen namens `COMMUNICATION_SERVICES_CONNECTION_STRING` abgerufen. Informationen zur [Verwaltung der Verbindungszeichenfolge Ihrer Ressource](../create-communication-resource.md#store-your-connection-string).

Fügen Sie der `Main`-Methode folgenden Code hinzu:

```csharp
// This code demonstrates how to fetch your connection string
// from an environment variable.
string connectionString = Environment.GetEnvironmentVariable("COMMUNICATION_SERVICES_CONNECTION_STRING");
var client = new CommunicationIdentityClient(connectionString);
```

Alternativ dazu können Sie auch den Endpunkt und den Zugriffsschlüssel trennen.
```csharp
// This code demonstrates how to fetch your endpoint and access key
// from an environment variable.
string endpoint = Environment.GetEnvironmentVariable("COMMUNICATION_SERVICES_ENDPOINT");
string accessKey = Environment.GetEnvironmentVariable("COMMUNICATION_SERVICES_ACCESSKEY");
var client = new CommunicationIdentityClient(new Uri(endpoint), new AzureKeyCredential(accessKey));
```

Wenn Sie eine Azure Active Directory(AD)-Anwendung eingerichtet haben, finden Sie weitere Informationen unter [Verwenden von Dienstprinzipalen](../identity/service-principal.md). Sie können sich auch bei AD authentifizieren.
```csharp
TokenCredential tokenCredential = new DefaultAzureCredential();
var client = new CommunicationIdentityClient(new Uri(endpoint), tokenCredential);
```

## <a name="create-an-identity"></a>Erstellen einer Identität

Von Azure Communication Services wird ein einfaches Identitätsverzeichnis gepflegt. Verwenden Sie die `createUser`-Methode, um einen neuen Eintrag im Verzeichnis mit einer eindeutigen empfangenen `Id`.Store-Identität mit einer Zuordnung zu den Benutzern Ihrer Anwendung zu erstellen. Beispielsweise, indem Sie sie in der Datenbank des Anwendungsservers speichern. Die Identität ist später erforderlich, um Zugriffstoken auszustellen.

```csharp
var identityResponse = await client.CreateUserAsync();
var identity = identityResponse.Value;
Console.WriteLine($"\nCreated an identity with ID: {identity.Id}");
```

## <a name="issue-identity-access-tokens"></a>Ausstellen von Identitätszugriffstoken

Verwenden Sie die Methode `GetToken`, um ein Zugriffstoken für eine bereits vorhandene Communication Services-Identität auszustellen. Der Parameter `scopes` definiert einen Satz primitiver Elemente, die dieses Zugriffstoken autorisieren. Weitere Informationen finden Sie in der [Liste der unterstützten Aktionen](../../concepts/authentication.md). Eine neue Instanz des Parameters `communicationUser` kann basierend auf der Zeichenfolgendarstellung der Azure Communication Service-Identität generiert werden.

```csharp
// Issue an access token with the "voip" scope for an identity
var tokenResponse = await client.GetTokenAsync(identity, scopes: new [] { CommunicationTokenScope.VoIP });
var token =  tokenResponse.Value.Token;
var expiresOn = tokenResponse.Value.ExpiresOn;
Console.WriteLine($"\nIssued an access token with 'voip' scope that expires at {expiresOn}:");
Console.WriteLine(token);
```

Zugriffstoken sind kurzlebige Anmeldeinformationen, die erneut ausgestellt werden müssen. Wenn dies nicht der Fall ist, kann die Benutzerumgebung Ihrer Anwendung unterbrochen werden. Die Antworteigenschaft `expiresOn` gibt die Lebensdauer des Zugriffstokens an.

## <a name="create-an-identity-and-issue-an-access-token-within-the-same-request"></a>Erstellen einer Identität und Ausstellen eines Zugriffstokens innerhalb derselben Anforderung

Verwenden Sie die `CreateUserAndTokenAsync`-Methode, um eine Communication Services-Identität zu erstellen und ein Zugriffstoken dafür auszustellen. Der Parameter `scopes` definiert einen Satz primitiver Elemente, die dieses Zugriffstoken autorisieren. Weitere Informationen finden Sie in der [Liste der unterstützten Aktionen](../../concepts/authentication.md).

```csharp
// Issue an identity and an access token with the "voip" scope for the new identity
var identityAndTokenResponse = await client.CreateUserAndTokenAsync(scopes: new[] { CommunicationTokenScope.VoIP });
var identity = identityAndTokenResponse.Value.User;
var token = identityAndTokenResponse.Value.AccessToken.Token;
var expiresOn = identityAndTokenResponse.Value.AccessToken.ExpiresOn;

Console.WriteLine($"\nCreated an identity with ID: {identity.Id}");
Console.WriteLine($"\nIssued an access token with 'voip' scope that expires at {expiresOn}:");
Console.WriteLine(token);
```

## <a name="refresh-access-tokens"></a>Zugriffstoken für die Aktualisierung

Um ein Zugriffstoken zu aktualisieren, übergeben Sie eine Instanz des `CommunicationUserIdentifier`-Objekts an `GetTokenAsync`. Wenn Sie diese `Id` gespeichert haben und einen neuen `CommunicationUserIdentifier` erstellen müssen, können Sie zu diesem Zweck die gespeicherte `Id` wie folgt an den `CommunicationUserIdentifier`-Konstruktor übergeben:

```csharp
var identityToRefresh = new CommunicationUserIdentifier(identity.Id);
var tokenResponse = await client.GetTokenAsync(identityToRefresh, scopes: new [] { CommunicationTokenScope.VoIP });
```

## <a name="revoke-access-tokens"></a>Widerrufen von Zugriffstoken

In einigen Fällen können Sie Zugriffstoken explizit widerrufen. Beispielsweise dann, wenn der Benutzer einer Anwendung das Kennwort ändert, das für die Authentifizierung beim Dienst verwendet wird. Die Methode `RevokeTokensAsync` erklärt alle aktiven Zugriffstoken für ungültig, die für die Identität ausgestellt wurden.

```csharp
await client.RevokeTokensAsync(identity);
Console.WriteLine($"\nSuccessfully revoked all access tokens for identity with ID: {identity.Id}");
```

## <a name="delete-an-identity"></a>Löschen einer Identität

Wird eine Identität gelöscht, werden alle aktiven Zugriffstoken widerrufen, und für die Identitäten können keine Zugriffstoken mehr ausgestellt werden. Außerdem werden alle gespeicherten Inhalte entfernt, die der Identität zugeordnet sind.

```csharp
await client.DeleteUserAsync(identity);
Console.WriteLine($"\nDeleted the identity with ID: {identity.Id}");
```

## <a name="run-the-code"></a>Ausführen des Codes

Führen Sie die Anwendung mit dem Befehl `dotnet run` aus dem Anwendungsverzeichnis aus.

```console
dotnet run
```
