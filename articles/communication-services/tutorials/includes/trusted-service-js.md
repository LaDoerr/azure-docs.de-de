---
title: Vertrauenswürdiger JavaScript-Dienst
description: Dies ist die JavaScript-Version, mit der ein vertrauenswürdiger Dienst für Communication Services erstellt wird.
author: dademath
manager: nimag
services: azure-communication-services
ms.author: ddematheu2
ms.date: 06/30/2021
ms.topic: include
ms.service: azure-communication-services
ms.openlocfilehash: 1c399f05467fe23504b9f204a50484620fcec86e
ms.sourcegitcommit: 98308c4b775a049a4a035ccf60c8b163f86f04ca
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/30/2021
ms.locfileid: "113107294"
---
## <a name="download-code"></a>Herunterladen des Codes

Den fertigen Code für diesen Schnellstart finden Sie auf [GitHub](https://github.com/Azure-Samples/communication-services-javascript-quickstarts/tree/main/trusted-authentication-service).

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Details finden Sie unter [kostenloses Azure-Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Visual Studio Code](https://code.visualstudio.com/) auf einer der [unterstützten Plattformen](https://code.visualstudio.com/docs/supporting/requirements#_platforms)
- [Node.js](https://nodejs.org/): Active LTS- und Maintenance LTS-Versionen (Empfehlung: 10.14.1). Verwenden Sie den Befehl `node --version`, um Ihre Version zu überprüfen.
- [Azure Functions-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) für Visual Studio Code
- Eine aktive Communication Services-Ressource und eine Verbindungszeichenfolge. [Erstellen Sie eine Communication Services-Ressource](../../quickstarts/create-communication-resource.md).

## <a name="overview"></a>Übersicht

:::image type="content" source="../media/trusted-service-architecture.png" alt-text="Diagramm für die Architektur vertrauenswürdiger Dienste":::

In diesem Tutorial erstellen wir eine Azure-Funktion, die als vertrauenswürdiger Tokenbereitstellungsdienst fungieren soll. Sie können dieses Tutorial für einen Bootstrap Ihres Tokenbereitstellungsdiensts nutzen.

Dieser Dienst ist für die Authentifizierung von Benutzern bei Azure Communication Services verantwortlich. Benutzer Ihrer Communication Services-Anwendungen benötigen ein Zugriffstoken (`Access Token`), um an Chatthreads und VoIP-Aufrufen teilnehmen zu können. Die Azure-Funktion kann als vertrauenswürdiger Vermittler zwischen dem Benutzer und Communication Services verwendet werden. Dies ermöglicht es Ihnen, Zugriffstoken bereitzustellen, ohne Ihre Ressourcenverbindungszeichenfolge für Ihre Benutzer verfügbar zu machen.

Weitere Informationen finden Sie in der Dokumentation zur [Client-/Server-Architektur](../../concepts/client-and-server-architecture.md) und zur [Authentifizierung und Autorisierung](../../concepts/authentication.md).

## <a name="setting-up"></a>Einrichten

### <a name="azure-functions-set-up"></a>Einrichten von Azure Functions

Zuerst richten wir die grundlegende Struktur für unsere Azure-Funktion ein. Die Schritt-für-Schritt-Anleitung für die Einrichtung finden Sie hier: [Erstellen einer Funktion mit Visual Studio Code](../../../azure-functions/create-first-function-vs-code-csharp.md?pivots=programming-language-javascript)

Für unsere Azure-Funktion ist die folgende Konfiguration erforderlich:

- Sprache: JavaScript
- Vorlage: HTTP-Trigger
- Autorisierungsstufe: Anonym (dies kann später geändert werden, wenn Sie ein anderes Autorisierungsmodell bevorzugen)
- Funktionsname: Benutzerdefiniert

Nachdem Sie die [Anweisungen für Azure Functions](../../../azure-functions/create-first-function-vs-code-csharp.md?pivots=programming-language-javascript) mit der o. g. Konfiguration ausgeführt haben, verfügen Sie über ein Projekt in Visual Studio Code für die Azure-Funktion mit einer `index.js`-Datei, die die Funktion selbst enthält. Der Code in dieser Datei ist wie folgt:

```javascript

module.exports = async function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');

    const name = (req.query.name || (req.body && req.body.name));
    const responseMessage = name
        ? "Hello, " + name + ". This HTTP triggered function executed successfully."
        : "This HTTP triggered function executed successfully. Pass a name in the query string or in the request body for a personalized response.";

    context.res = {
        // status: 200, /* Defaults to 200 */
        body: responseMessage
    };
}

```

Wir fahren jetzt mit der Installation von Azure Communication Services-Bibliotheken fort.

### <a name="install-communication-services-libraries"></a>Installieren von Communication Services-Bibliotheken

Wir verwenden die `Identity`-Bibliothek, um Benutzerzugriffstoken (`User Access Tokens`) zu generieren.

Verwenden Sie den Befehl `npm install`, um das Azure Communication Services Identity SDK für JavaScript zu installieren.

```console

npm install @azure/communication-identity --save

```

Die Option `--save` listet die Bibliothek als Abhängigkeit in Ihrer Datei **package.json** auf.

Importieren Sie am Anfang der Datei `index.js` die Schnittstelle für `CommunicationIdentityClient`.

```javascript
const { CommunicationIdentityClient } = require('@azure/communication-identity');
```

## <a name="access-token-generation"></a>Generieren von Zugriffstokens

Damit die Azure-Funktion Benutzerzugriffstoken (`User Access Tokens`) generieren kann, müssen wir zunächst die Verbindungszeichenfolge für die Communication Services-Ressource verwenden.

Weitere Informationen zum Abrufen der Verbindungszeichenfolge finden Sie im [Schnellstart zur Ressourcenbereitstellung](../../quickstarts/create-communication-resource.md).

``` javascript
const connectionString = 'INSERT YOUR RESOURCE CONNECTION STRING'
```

Als Nächstes ändern wir die ursprüngliche Funktion, um Benutzerzugriffstoken (`User Access Tokens`) zu generieren.

Benutzerzugriffstoken (`User Access Tokens`) werden generiert, indem ein Benutzer mit der `createUser`-Methode erstellt wird. Sobald der Benutzer erstellt wurde, können wir die `getToken`-Methode verwenden, um ein Token für diesen Benutzer zu generieren, den die Azure-Funktion zurückgibt.

In diesem Beispiel konfigurieren wir den Tokenbereich auf `voip`. Für Ihre Anwendung sind möglicherweise andere Bereiche erforderlich. Erfahren Sie mehr über [Bereiche](../../quickstarts/access-tokens.md).

```javascript
module.exports = async function (context, req) {
    let tokenClient = new CommunicationIdentityClient(connectionString);

    const user = await tokenClient.createUser();

    const userToken = await tokenClient.getToken(user, ["voip"]);

    context.res = {
        body: userToken
    };
}
```

Für vorhandene Kommunikationsbenutzer (`CommunicationUser`) von Communication Services können Sie den Erstellungsschritt überspringen und einfach ein Zugriffstoken generieren. Weitere Informationen finden Sie im [Schnellstart zum Erstellen von Benutzerzugriffstoken](../../quickstarts/access-tokens.md).

## <a name="test-the-azure-function"></a>Testen der Azure-Funktion

Führen Sie die Azure-Funktion mithilfe von `F5` lokal aus. Dadurch wird die Azure-Funktion lokal initialisiert und ist über die folgenden Funktionen zugänglich: `http://localhost:7071/api/FUNCTION_NAME`. Sehen Sie sich die zusätzliche Dokumentation zum [Lokalen Ausführen](../../../azure-functions/create-first-function-vs-code-csharp.md?pivots=programming-language-javascript#run-the-function-locally) an.

Öffnen Sie die URL in Ihrem Browser. Daraufhin wird ein Antworttext mit der Kommunikationsbenutzer-ID, dem Token und dessen Ablaufdatum angezeigt.

:::image type="content" source="../media/trusted-service-sample-response.png" alt-text="Screenshot mit einer Beispielantwort für die erstellte Azure-Funktion":::

## <a name="deploy-the-function-to-azure"></a>Bereitstellen der Funktion in Azure

Wenn Sie Ihre Azure-Funktion bereitstellen möchten, befolgen Sie die [Schritt-für-Schritt-Anweisungen](../../../azure-functions/create-first-function-vs-code-csharp.md?pivots=programming-language-javascript#sign-in-to-azure).

Sie müssen diese Schritte ausführen:
1. Anmelden bei Azure aus Visual Studio
2. Veröffentlichen Sie Ihr Projekt in Ihrem Azure-Konto. Hier müssen Sie ein vorhandenes Abonnement auswählen.
3. Erstellen Sie eine neue Azure Function-Ressource mithilfe des Visual Studio-Assistenten, oder verwenden Sie eine vorhandene Ressource. Eine neue Ressource muss für die gewünschte Region, Runtime und den eindeutigen Bezeichner konfiguriert werden.
4. Warten auf den Abschluss der Bereitstellung
5. Ausführen der Funktion 🎉

## <a name="run-azure-function"></a>Ausführen der Azure-Funktion

Führen Sie die Azure-Funktion mithilfe der URL `http://<function-appn-ame>.azurewebsites.net/api/<function-name>` lokal aus.

Sie finden die URL, indem Sie in Visual Studio Code mit der rechten Maustaste auf die Funktion klicken und die Funktions-URL kopieren.

Hier finden Sie weitere Informationen zum [Ausführen Ihrer Azure-Funktion](../../../azure-functions/create-first-function-vs-code-csharp.md?pivots=programming-language-javascript#run-the-function-in-azure).
