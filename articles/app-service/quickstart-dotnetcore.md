---
title: 'Schnellstart: Bereitstellen einer ASP.NET-Web-App'
description: Hier erfahren Sie, wie Sie Web-Apps in Azure App Service ausführen, indem Sie Ihre erste ASP.NET-App bereitstellen.
ms.assetid: b1e6bd58-48d1-4007-9d6c-53fd6db061e3
ms.topic: quickstart
ms.date: 06/08/2021
ms.custom: devx-track-csharp, mvc, devcenter, vs-azure, seodec18, contperf-fy21q1
zone_pivot_groups: app-service-ide
adobe-target: true
adobe-target-activity: DocsExp–386541–A/B–Enhanced-Readability-Quickstarts–2.19.2021
adobe-target-experience: Experience B
adobe-target-content: ./quickstart-dotnetcore-uiex
ms.openlocfilehash: 3b3abdf40d5aa9d56421361237432ddf08c0c016
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111746535"
---
<!-- NOTES:

I'm a .NET developer who wants to deploy my web app to App Service. I may develop apps with
Visual Studio, Visual Studio for Mac, Visual Studio Code, or the .NET SDK/CLI. This article
should be able to guide .NET devs, whether they're app is .NET Core, .NET, or .NET Framework.

As a .NET developer, when choosing an IDE and .NET TFM - you map to various OS requirements.
For example, if you choose Visual Studio - you're developing the app on Windows, but you can still
target cross-platform with .NET Core 3.1 or .NET 5.0.

| .NET / IDE         | Visual Studio | Visual Studio for Mac | Visual Studio Code | Command line   |
|--------------------|---------------|-----------------------|--------------------|----------------|
| .NET Core 3.1      | Windows       | macOS                 | Cross-platform     | Cross-platform |
| .NET 5.0           | Windows       | macOS                 | Cross-platform     | Cross-platform |
| .NET Framework 4.8 | Windows       | N/A                   | Windows            | Windows        |

-->

# <a name="quickstart-deploy-an-aspnet-web-app"></a>Schnellstart: Bereitstellen einer ASP.NET-Web-App

In dieser Schnellstartanleitung wird beschrieben, wie Sie Ihre erste ASP.NET-Web-App erstellen und für [Azure App Service](overview.md) bereitstellen. App Service unterstützt verschiedene Versionen von .NET-Apps und bietet einen hochgradig skalierbaren Webhostingdienst mit Self-Patching. ASP.net Web-Apps sind plattformübergreifend und können unter Linux oder Windows gehostet werden. Am Ende verfügen Sie über eine Azure-Ressourcengruppe, die einen App Service-Hostingplan und eine App Service-Instanz mit einer bereitgestellten Webanwendung umfasst.

## <a name="prerequisites"></a>Voraussetzungen

:::zone target="docs" pivot="development-environment-vs"

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/dotnet).
- <a href="https://www.visualstudio.com/downloads" target="_blank">Visual Studio 2019</a> mit der Workload **ASP.NET- und Webentwicklung**

    Sie haben Visual Studio 2019 bereits installiert:

    - Installieren Sie in Visual Studio die neuesten Updates, indem Sie **Hilfe** > **Nach Updates suchen** auswählen.
    - Fügen Sie die Workload hinzu. Wählen Sie dazu **Tools** > **Tools und Features abrufen** aus.

:::zone-end

:::zone target="docs" pivot="development-environment-vscode"

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/dotnet).
- <a href="https://www.visualstudio.com/downloads" target="_blank">Visual Studio Code</a>
- Die Erweiterung für <a href="https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack" target="_blank">Azure-Tools</a>.

### <a name="net-core-31"></a>[.NET Core 3.1](#tab/netcore31)

<a href="https://dotnet.microsoft.com/download/dotnet-core/3.1" target="_blank">Installation des aktuellen .NET Core 3.1 SDK</a>

### <a name="net-50"></a>[.NET 5.0](#tab/net50)

<a href="https://dotnet.microsoft.com/download/dotnet/5.0" target="_blank">Installieren Sie das aktuelle .NET 5.0 SDK. </a>

### <a name="net-framework-48"></a>[.NET Framework 4.8](#tab/netframework48)

<a href="https://dotnet.microsoft.com/download/dotnet-framework/net48" target="_blank">Installieren Sie das .NET Framework 4.8 Developer Pack.</a>

> [!NOTE]
> Visual Studio Code ist plattformübergreifend, .NET Framework jedoch nicht. Wenn Sie .NET Framework-Apps mit Visual Studio Code entwickeln, empfiehlt es sich, einen Windows-Computer zu verwenden, um die Buildabhängigkeiten zu erfüllen.

---

:::zone-end

<!-- markdownlint-disable MD044 -->
:::zone target="docs" pivot="development-environment-cli"
<!-- markdownlint-enable MD044 -->

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/dotnet).
- Die <a href="/cli/azure/install-azure-cli" target="_blank">Azure CLI</a>
- Das .NET SDK (einschließlich Runtime und CLI).

### <a name="net-core-31"></a>[.NET Core 3.1](#tab/netcore31)

<a href="https://dotnet.microsoft.com/download/dotnet-core/3.1" target="_blank">Installation des aktuellen .NET Core 3.1 SDK</a>

### <a name="net-50"></a>[.NET 5.0](#tab/net50)

<a href="https://dotnet.microsoft.com/download/dotnet/5.0" target="_blank">Installieren Sie das aktuelle .NET 5.0 SDK. </a>

### <a name="net-framework-48"></a>[.NET Framework 4.8](#tab/netframework48)

<a href="https://dotnet.microsoft.com/download/dotnet/5.0" target="_blank"> Installieren Sie das neueste .NET 5.0 SDK </a> und <a href="https://dotnet.microsoft.com/download/dotnet-framework/net48" target="_blank"> das .NET Framework 4.8 Developer Pack.</a>

> [!NOTE]
> [.NET CLI](/dotnet/core/tools) ist plattformübergreifend, .NET Framework jedoch nicht. Wenn Sie .NET Framework-Apps mit .NET CLI entwickeln, empfiehlt es sich, einen Windows-Computer zu verwenden, um die Buildabhängigkeiten zu erfüllen.

---

:::zone-end

## <a name="create-an-aspnet-web-app"></a>Erstellen einer ASP.NET-Web-App

> [!TIP]
> .NET Core 3,1 ist die aktuelle LTS-Version (Long-Term Support) von .NET. Weitere Informationen finden Sie in der [Supportrichtlinie](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).

:::zone target="docs" pivot="development-environment-vs"

### <a name="net-core-31"></a>[.NET Core 3.1](#tab/netcore31)

1. Öffnen Sie Visual Studio, und wählen Sie **Neues Projekt erstellen** aus.
1. Suchen Sie unter **Neues Projekt erstellen** die Option **ASP.NET Core-Web-App**, und wählen Sie sie aus. Wählen Sie anschließend **Weiter** aus.
1. Geben Sie der Anwendung unter **Neues Projekt konfigurieren** den Namen _MyFirstAzureWebApp_, und wählen Sie dann **Weiter** aus.

   :::image type="content" source="media/quickstart-dotnet/configure-webapp-net.png" alt-text="Konfigurieren der ASP.NET Core 3.1 Web-App" border="true":::

1. Wählen Sie **.NET Core 3.1 (langfristige Unterstützung)** aus.
1. Stellen Sie sicher, dass der **Authentifizierungstyp**  auf **Keine** festgelegt ist. Wählen Sie **Erstellen** aus.

   :::image type="content" source="media/quickstart-dotnet/vs-additional-info-netcoreapp31.png" alt-text="Visual Studio: Wählen Sie .NET Core 3.1 und als Authentifizierungstyp „Keine“ aus." border="true":::

1. Wählen Sie im Visual Studio-Menü **Debuggen** > **Starten ohne Debugging** aus, um die Web-App lokal auszuführen.

   :::image type="content" source="media/quickstart-dotnet/local-webapp-net.png" alt-text="Visual Studio: .NET Core 3.1 lokal durchsuchen" lightbox="media/quickstart-dotnet/local-webapp-net.png" border="true":::

### <a name="net-50"></a>[.NET 5.0](#tab/net50)

1. Öffnen Sie Visual Studio, und wählen Sie **Neues Projekt erstellen** aus.
1. Suchen Sie unter **Neues Projekt erstellen** die Option **ASP.NET Core-Web-App**, und wählen Sie sie aus. Wählen Sie anschließend **Weiter** aus.
1. Geben Sie der Anwendung unter **Neues Projekt konfigurieren** den Namen _MyFirstAzureWebApp_, und wählen Sie dann **Weiter** aus.

   :::image type="content" source="media/quickstart-dotnet/configure-webapp-net.png" alt-text="Visual Studio: Konfigurieren Sie die ASP.NET 5.0 Web-App." border="true":::

1. Wählen Sie **.NET Core 5.0 (aktuell)** aus.
1. Stellen Sie sicher, dass der **Authentifizierungstyp**  auf **Keine** festgelegt ist. Wählen Sie **Erstellen** aus.

   :::image type="content" source="media/quickstart-dotnet/vs-additional-info-net50.png" alt-text="Visual Studio: zusätzliche Informationen bei der Auswahl von .NET Core 5.0." border="true":::

1. Wählen Sie im Visual Studio-Menü **Debuggen** > **Starten ohne Debugging** aus, um die Web-App lokal auszuführen.

   :::image type="content" source="media/quickstart-dotnet/local-webapp-net.png" alt-text="Visual Studio-ASP.NET Core 5.0 wird lokal ausgeführt." lightbox="media/quickstart-dotnet/local-webapp-net.png" border="true":::

### <a name="net-framework-48"></a>[.NET Framework 4.8](#tab/netframework48)

1. Öffnen Sie Visual Studio, und wählen Sie **Neues Projekt erstellen** aus.
1. Suchen Sie unter **Neues Projekt erstellen** die Option **ASP.NET-Webanwendung (.NET Framework)** , und wählen Sie sie aus. Wählen Sie anschließend **Weiter** aus.
1. Geben Sie der Anwendung unter **Neues Projekt konfigurieren** den Namen _MyFirstAzureWebApp_, und wählen Sie dann **Erstellen** aus.

   :::image type="content" source="media/quickstart-dotnet/configure-webapp-netframework48.png" alt-text="Visual Studio: Konfigurieren Sie die ASP.NET 4.8 Web-App." border="true":::

1. Wählen Sie die Vorlage **MVC** aus.
1. Stellen Sie sicher, dass **Authentifizierung** auf **Keine Authentifizierung** festgelegt ist. Wählen Sie **Erstellen** aus.

   :::image type="content" source="media/quickstart-dotnet/vs-mvc-no-auth-netframework48.png" alt-text="Visual Studio: Wählen Sie die MVC-Vorlage aus." border="true":::

1. Wählen Sie im Visual Studio-Menü **Debuggen** > **Starten ohne Debugging** aus, um die Web-App lokal auszuführen.

   :::image type="content" source="media/quickstart-dotnet/vs-local-webapp-netframework48.png" alt-text="Visual Studio ASP.NET Framework 4.8 wird lokal ausgeführt." lightbox="media/quickstart-dotnet/vs-local-webapp-netframework48.png" border="true":::

---

:::zone-end

:::zone target="docs" pivot="development-environment-vscode"

Erstellen Sie einen neuen Ordner mit dem Namen _MyFirstAzureWebApp_, und öffnen Sie ihn in Visual Studio Code. Öffnen Sie das <a href="https://code.visualstudio.com/docs/editor/integrated-terminal" target="_blank">Terminal</a>-Fenster, und erstellen Sie mit dem Befehl [`dotnet new webapp`](/dotnet/core/tools/dotnet-new#web-options) eine neue .NET Web-App.

### <a name="net-core-31"></a>[.NET Core 3.1](#tab/netcore31)

```dotnetcli
dotnet new webapp -f netcoreapp3.1
```

### <a name="net-50"></a>[.NET 5.0](#tab/net50)

```dotnetcli
dotnet new webapp -f net5.0
```

### <a name="net-framework-48"></a>[.NET Framework 4.8](#tab/netframework48)

```dotnetcli
dotnet new webapp --target-framework-override net48
```

> [!IMPORTANT]
> Das `--target-framework-override`-Flag ist ein Freitextersatz für den Zielframeworkmoniker (TFM) für das Projekt und stellt *keine Garantie* dafür dar, dass die unterstützende Vorlage vorhanden ist oder kompiliert wird. Sie können .NET Framework Apps nur unter Windows erstellen und ausführen.

---

Führen Sie die Anwendung über das **Terminal** in Visual Studio Code mit dem Befehl [`dotnet run`](/dotnet/core/tools/dotnet-run) lokal aus.

```dotnetcli
dotnet run
```

Öffnen Sie einen Webbrowser, und navigieren Sie zu der App auf `https://localhost:5001`.


### <a name="net-core-31"></a>[.NET Core 3.1](#tab/netcore31)

Die Vorlage für die ASP.NET Core 3.1-Web-App wird auf der Seite angezeigt.

:::image type="content" source="media/quickstart-dotnet/local-webapp-net.png" alt-text="Visual Studio Code: Führen Sie .NET Core 3.1 lokal im Browser aus." lightbox="media/quickstart-dotnet/local-webapp-net.png" border="true":::

### <a name="net-50"></a>[.NET 5.0](#tab/net50)

Die Vorlage für die ASP.NET Core 5.0-Web-App wird auf der Seite angezeigt.

:::image type="content" source="media/quickstart-dotnet/local-webapp-net.png" alt-text="Visual Studio Code: Führen Sie .NET 5.0 lokal im Browser aus." lightbox="media/quickstart-dotnet/local-webapp-net.png" border="true":::

### <a name="net-framework-48"></a>[.NET Framework 4.8](#tab/netframework48)

Die Vorlage für die ASP.NET Framework 4.8-Web-App wird auf der Seite angezeigt.

:::image type="content" source="media/quickstart-dotnet/local-webapp-net48.png" alt-text="Visual Studio Code: Führen Sie .NET 4.8 lokal im Browser aus." lightbox="media/quickstart-dotnet/local-webapp-net48.png" border="true":::

---

:::zone-end

<!-- markdownlint-disable MD044 -->
:::zone target="docs" pivot="development-environment-cli"
<!-- markdownlint-enable MD044 -->

Öffnen Sie auf Ihrem Computer in einem Arbeitsverzeichnis ein Terminalfenster. Erstellen Sie mit dem Befehl [`dotnet new webapp`](/dotnet/core/tools/dotnet-new#web-options) eine neue .NET-Web-App, und ändern Sie dann die Verzeichnisse in der neu erstellten App.

### <a name="net-core-31"></a>[.NET Core 3.1](#tab/netcore31)

```dotnetcli
dotnet new webapp -n MyFirstAzureWebApp -f netcoreapp3.1 && cd MyFirstAzureWebApp
```

### <a name="net-50"></a>[.NET 5.0](#tab/net50)

```dotnetcli
dotnet new webapp -n MyFirstAzureWebApp -f net5.0 && cd MyFirstAzureWebApp
```

### <a name="net-framework-48"></a>[.NET Framework 4.8](#tab/netframework48)

```dotnetcli
dotnet new webapp -n MyFirstAzureWebApp --target-framework-override net48 && cd MyFirstAzureWebApp
```

> [!IMPORTANT]
> Das `--target-framework-override`-Flag ist ein Freitextersatz für den Zielframeworkmoniker (TFM) für das Projekt und stellt *keine Garantie* dafür dar, dass die unterstützende Vorlage vorhanden ist oder kompiliert wird. Sie können .NET Framework Apps nur unter Windows erstellen.

---

Führen Sie die Anwendung aus derselben Terminalsitzung mithilfe des Befehls [`dotnet run`](/dotnet/core/tools/dotnet-run) lokal aus.

```dotnetcli
dotnet run
```

Öffnen Sie einen Webbrowser, und navigieren Sie zu der App auf `https://localhost:5001`.

### <a name="net-core-31"></a>[.NET Core 3.1](#tab/netcore31)

Die Vorlage für die ASP.NET Core 3.1-Web-App wird auf der Seite angezeigt.

:::image type="content" source="media/quickstart-dotnet/local-webapp-net.png" alt-text="Visual Studio Code: ASP.NET Core 3.1 im lokalen Browser." lightbox="media/quickstart-dotnet/local-webapp-net.png" border="true":::

### <a name="net-50"></a>[.NET 5.0](#tab/net50)

Die Vorlage für die ASP.NET Core 5.0-Web-App wird auf der Seite angezeigt.

:::image type="content" source="media/quickstart-dotnet/local-webapp-net.png" alt-text="Visual Studio Code: ASP.NET Core 5.0 im lokalen Browser." lightbox="media/quickstart-dotnet/local-webapp-net.png" border="true":::

### <a name="net-framework-48"></a>[.NET Framework 4.8](#tab/netframework48)

Die Vorlage für die ASP.NET Framework 4.8-Web-App wird auf der Seite angezeigt.

:::image type="content" source="media/quickstart-dotnet/local-webapp-net48.png" alt-text="Visual Studio Code: ASP.NET Framework 4.8 im lokalen Browser." lightbox="media/quickstart-dotnet/local-webapp-net48.png" border="true":::

---

:::zone-end

## <a name="publish-your-web-app"></a>Veröffentlichen Ihrer Web-App

Zum Veröffentlichen Ihrer Web-App müssen Sie zuerst eine neue App Service-Instanz erstellen und konfigurieren, auf der Sie Ihre App veröffentlichen können.

Erstellen Sie bei der Einrichtung der App Service-Instanz Folgendes:

- Eine neue [Ressourcengruppe](../azure-resource-manager/management/overview.md#terminology), die alle Azure-Ressourcen für den Dienst enthalten soll.
- Einen neuen [Hostingplan](overview-hosting-plans.md), mit dem der Standort, die Größe und die Funktionen der Webserverfarm zum Hosten Ihrer App angegeben werden.

Führen Sie die folgenden Schritte aus, um Ihre App Service-Instanz zu erstellen und Ihre Web-App zu veröffentlichen:

:::zone target="docs" pivot="development-environment-vs"

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **MyFirstAzureWebApp**, und wählen Sie **Veröffentlichen** aus.
1. Wählen Sie unter **Veröffentlichen** die Option **Azure** aus, und klicken Sie dann auf **Weiter**.

    :::image type="content" source="media/quickstart-dotnet/vs-publish-target-Azure.png" alt-text="Visual Studio: Veröffentlichen Sie die Web-App und zielen Sie dabei auf Azure." border="true":::

1. Ihre Optionen hängen davon ab, ob Sie bereits bei Azure angemeldet sind und ob Sie über ein Visual Studio-Konto verfügen, das mit einem Azure-Konto verknüpft ist. Wählen Sie entweder **Konto hinzufügen** oder **Anmelden** aus, um sich bei Ihrem Azure-Abonnement anzumelden. Wenn Sie bereits angemeldet sind, wählen Sie das gewünschte Konto aus.

    :::image type="content" source="media/quickstart-dotnetcore/sign-in-Azure-vs2019.png" border="true" alt-text="Visual Studio: Wählen Sie das Dialogfeld „Anmelden bei Azure“ aus.":::

1. Wählen Sie als **Spezifisches Ziel** entweder **Azure App Service (Linux)** oder **Azure App Service (Windows)** aus.

    > [!IMPORTANT]
    > Wenn Sie ASP.NET Framework 4.8 als Ziel auswählen, verwenden Sie **Azure App Service (Windows)** .

1. Klicken Sie rechts von **App Service-Instanzen** auf **+** .

    :::image type="content" source="media/quickstart-dotnetcore/publish-new-app-service.png" border="true" alt-text="Visual Studio: Dialogfeld „Neue App Service-App“.":::

1. Akzeptieren Sie unter **Abonnement** das angegebene Abonnement, oder wählen Sie in der Dropdownliste ein neues Abonnement aus.
1. Wählen Sie unter **Ressourcengruppe** die Option **Neu** aus. Geben Sie unter **Name der neuen Ressourcengruppe** den Namen *myResourceGroup* ein, und wählen Sie **OK** aus.
1. Wählen Sie unter **Hostingplan** die Option **Neu** aus.
1. Geben Sie im Dialogfeld **Hostingplan: Neu erstellen** die Werte ein, die in der folgenden Tabelle angegeben sind:

    | Einstellung          | Vorgeschlagener Wert          | BESCHREIBUNG                                                           |
    |------------------|--------------------------|-----------------------------------------------------------------------|
    | **Hostingplan** | *MyFirstAzureWebAppPlan* | Name des App Service-Plans.                                         |
    | **Location**     | *Europa, Westen*            | Das Rechenzentrum, in dem die Web-App gehostet wird.                           |
    | **Größe**         | *Free*                   | Der [Tarif][app-service-pricing-tier] bestimmt die Hostingfeatures. |

    :::image type="content" source="media/quickstart-dotnetcore/create-new-hosting-plan-vs2019.png" border="true" alt-text="Erstellen eines neuen Hostingplans":::

1. Geben Sie unter **Name** einen eindeutigen App-Namen ein, der nur aus den folgenden zulässigen Zeichen besteht: `a-z`, `A-Z`, `0-9` und `-`. Sie können den automatisch generierten eindeutigen Namen übernehmen. Die URL der Web-App lautet `http://<app-name>.azurewebsites.net`, wobei `<app-name>` der Name Ihrer App ist.
1. Wählen Sie **Erstellen** aus, um die Azure-Ressourcen zu erstellen.

    :::image type="content" source="media/quickstart-dotnetcore/web-app-name-vs2019.png" border="true" alt-text="Visual Studio: Dialogfeld zum Erstellen von App-Ressourcen.":::

   Nach dem Abschluss des Assistenten werden die Azure-Ressourcen für Sie erstellt, und Sie können sie veröffentlichen.

1. Wählen Sie **Fertig stellen** aus, um den Assistenten zu schließen.
1. Wählen Sie auf der Seite **Veröffentlichen** die Option **Veröffentlichen** aus. Visual Studio erstellt, packt und veröffentlicht die App in Azure und startet sie anschließend im Standardbrowser.

    ### <a name="net-core-31"></a>[.NET Core 3.1](#tab/netcore31)

    Die ASP.NET Core 3.1-Web-App wird auf der Seite angezeigt.

    :::image type="content" source="media/quickstart-dotnet/Azure-webapp-net.png" lightbox="media/quickstart-dotnet/Azure-webapp-net.png" border="true" alt-text="Visual Studio: ASP.NET Core 3.1-Web-App in Azure.":::

    ### <a name="net-50"></a>[.NET 5.0](#tab/net50)

    Die ASP.NET Core 5.0-Web-App wird auf der Seite angezeigt.

    :::image type="content" source="media/quickstart-dotnet/Azure-webapp-net.png" lightbox="media/quickstart-dotnet/Azure-webapp-net.png" border="true" alt-text="Visual Studio: ASP.NET Core 5.0-Web-App in Azure.":::

    ### <a name="net-framework-48"></a>[.NET Framework 4.8](#tab/netframework48)

    Die ASP.NET Framework 4.8-Web-App wird auf der Seite angezeigt.

    :::image type="content" source="media/quickstart-dotnet/vs-Azure-webapp-net48.png" lightbox="media/quickstart-dotnet/vs-Azure-webapp-net48.png" border="true" alt-text="Visual Studio: ASP.NET Framework 4.8-Web-App in Azure.":::

    ---

:::zone-end

:::zone target="docs" pivot="development-environment-vscode"

Vorgehensweise zum Bereitstellen Ihrer Web-App mithilfe der Visual Studio Azure-Tools-Erweiterung bereit:

1. Öffnen Sie in Visual Studio Code die [**Befehlspalette**](https://code.visualstudio.com/docs/getstarted/userinterface#_command-palette) <kbd>STRG</kbd>++<kbd>UMSCHALT</kbd>++<kbd>P</kbd>.
1. Suchen Sie „Azure App Service: „In Web-App bereitstellen“ aus.
1. Gehen Sie bei den Aufforderungen wie folgt vor:

    - Wählen Sie *MyFirstAzureWebApp* als Ordner für die Bereitstellung aus.
    - Wählen Sie nach Aufforderung die Option **Konfiguration hinzufügen** aus.
    - Melden Sie sich nach Aufforderung bei Ihrem vorhandenen Azure-Konto an.

    :::image type="content" source="media/quickstart-dotnet/vscode-sign-in-to-Azure.png" alt-text="Visual Studio Code: Anmelden bei Azure" border="true":::

    - Wählen Sie Ihr **Abonnement** aus.
    - Wählen Sie **Neue Web-App erstellen... Erweitert** aus.
    - Verwenden Sie bei **Geben Sie einen global eindeutigen Namen ein** einen Namen, der in ganz Azure eindeutig ist (*zulässig sind die Zeichen `a-z`, `0-9` sowie `-`* ). Ein bewährtes Muster ist eine Kombination aus Ihrem Firmennamen und einer App-ID.
    - Wählen Sie **Neue Ressourcengruppe erstellen** aus, und geben Sie einen Namen ein, z. B. `myResourceGroup`.
    - Wenn Sie zur **Auswahl eines Laufzeitstapels** aufgefordert werden:
      - Wählen Sie für *.NET Core 3.1* die Option **.NET Core 3.1 (LTS)** aus
      - Wählen Sie für *.NET 5.0* die Option **.NET 5** aus
      - Wählen Sie für *.NET Framework 4.8* die Option **ASP.NET V4.8** aus
    - Wählen Sie ein Betriebssystem (Windows oder Linux) aus.
        - Bei *.NET Framework 4.8* wird Windows implizit ausgewählt.
    - Klicken Sie auf **Einen neuen App Services-Plan erstellen**, geben Sie einen Namen an, und wählen Sie dann den [Tarif][app-service-pricing-tier] **F1 Free** aus.
    - Wählen Sie für die Application Insights-Ressource die Option **Vorerst überspringen** aus.
    - Wählen Sie einen Ort in Ihrer Nähe aus.

1. Wenn die Veröffentlichung abgeschlossen ist, wählen Sie in der Benachrichtigung **Website durchsuchen** aus, und wählen Sie bei entsprechender Aufforderung **Öffnen**.

    ### <a name="net-core-31"></a>[.NET Core 3.1](#tab/netcore31)

    Die ASP.NET Core 3.1-Web-App wird auf der Seite angezeigt.

    :::image type="content" source="media/quickstart-dotnet/Azure-webapp-net.png" lightbox="media/quickstart-dotnet/Azure-webapp-net.png" border="true" alt-text="Visual Studio Code: ASP.NET Core 3.1-Web-App in Azure.":::

    ### <a name="net-50"></a>[.NET 5.0](#tab/net50)

    Die ASP.NET Core 5.0-Web-App wird auf der Seite angezeigt.

    :::image type="content" source="media/quickstart-dotnet/Azure-webapp-net.png" lightbox="media/quickstart-dotnet/Azure-webapp-net.png" border="true" alt-text="Visual Studio Code: ASP.NET Core 5.0-Web-App in Azure.":::

    ### <a name="net-framework-48"></a>[.NET Framework 4.8](#tab/netframework48)

    Die ASP.NET Framework 4.8-Web-App wird auf der Seite angezeigt.

    :::image type="content" source="media/quickstart-dotnet/Azure-webapp-net48.png" lightbox="media/quickstart-dotnet/vs-Azure-webapp-net48.png" border="true" alt-text="Visual Studio Code: ASP.NET Framework 4.8-Web-App in Azure.":::

    ---

:::zone-end

<!-- markdownlint-disable MD044 -->
:::zone target="docs" pivot="development-environment-cli"
<!-- markdownlint-enable MD044 -->

Stellen Sie den Code mit dem Befehl [`az webapp up`](/cli/azure/webapp#az_webapp_up) in Ihrem lokalen Verzeichnis *MyFirstAzureWebApp* bereit:

```azurecli
az webapp up --sku F1 --name <app-name> --os-type <os>
```

- Wenn der Befehl `az` nicht erkannt wird, stellen Sie sicher, dass Sie die Azure CLI gemäß der Beschreibung im Abschnitt [Voraussetzungen](#prerequisites) installiert haben.
- Ersetzen Sie `<app-name>` durch einen Namen, der innerhalb von Azure eindeutig ist (*gültige Zeichen: `a-z`, `0-9` und `-`* ). Ein bewährtes Muster ist eine Kombination aus Ihrem Firmennamen und einer App-ID.
- Mit dem Argument `--sku F1` wird die Web-App im [Tarif][app-service-pricing-tier] **Free** erstellt. Lassen Sie dieses Argument weg, um einen schnelleren Premium-Tarif zu verwenden. Dieser verursacht jedoch stündlich Kosten.
- Ersetzen Sie `<os>` entweder durch `linux` oder durch `windows`. Sie müssen `windows` verwenden, wenn Sie *ASP.NET Framework 4.8* als Ziel verwenden.
- Optional können Sie das Argument `--location <location-name>` einfügen, wobei `<location-name>` eine verfügbare Azure-Region ist. Sie können eine Liste der zulässigen Regionen für Ihr Azure-Konto abrufen, indem Sie den Befehl [`az account list-locations`](/cli/azure/appservice#az_appservice_list_locations) ausführen.

Die Ausführung dieses Befehls kann einige Minuten in Anspruch nehmen. Bei der Ausführung werden Meldungen zum Erstellen der Ressourcengruppe, des App Service-Plans und der Hosting-App und zur Konfiguration der Protokollierung angezeigt, und anschließend erfolgt die ZIP-Bereitstellung. Danach wird eine Meldung mit der URL der APP ausgegeben:

```azurecli
You can launch the app at http://<app-name>.azurewebsites.net
```

Öffnen Sie einen Webbrowser, und navigieren Sie zu der URL.

### <a name="net-core-31"></a>[.NET Core 3.1](#tab/netcore31)

Die ASP.NET Core 3.1-Web-App wird auf der Seite angezeigt.

:::image type="content" source="media/quickstart-dotnet/Azure-webapp-net.png" lightbox="media/quickstart-dotnet/Azure-webapp-net.png" border="true" alt-text="CLI: ASP.NET Core 3.1-Web-App in Azure.":::

### <a name="net-50"></a>[.NET 5.0](#tab/net50)

Die ASP.NET Core 5.0-Web-App wird auf der Seite angezeigt.

:::image type="content" source="media/quickstart-dotnet/Azure-webapp-net.png" lightbox="media/quickstart-dotnet/Azure-webapp-net.png" border="true" alt-text="CLI: ASP.NET Core 5.0-Web-App in Azure.":::

### <a name="net-framework-48"></a>[.NET Framework 4.8](#tab/netframework48)

Die ASP.NET Framework 4.8-Web-App wird auf der Seite angezeigt.

:::image type="content" source="media/quickstart-dotnet/Azure-webapp-net48.png" lightbox="media/quickstart-dotnet/Azure-webapp-net48.png" border="true" alt-text="CLI: ASP.NET Framework 4.8-Web-App in Azure.":::

---

:::zone-end

## <a name="update-the-app-and-redeploy"></a>Aktualisieren der App und erneutes Bereitstellen

Führen Sie die folgenden Schritte aus, um Ihre Web-App zu aktualisieren und erneut bereitzustellen:

:::zone target="docs" pivot="development-environment-vs"

1. Öffnen Sie im **Projektmappen-Explorer** unter Ihrem Projekt die Datei *Index.cshtml*.
1. Ersetzen Sie das erste `<div>`-Element durch den folgenden Code:

    ```razor
    <div class="jumbotron">
        <h1>.NET 💜 Azure</h1>
        <p class="lead">Example .NET app to Azure App Service.</p>
    </div>
    ```

   Speichern Sie die Änderungen.

1. Klicken Sie zur erneuten Bereitstellung in Azure im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **MyFirstAzureWebApp**, und wählen Sie **Veröffentlichen** aus.
1. Wählen Sie auf der Zusammenfassungsseite **Veröffentlichen** die Option **Veröffentlichen** aus.

    Nach Abschluss der Veröffentlichung wird in Visual Studio ein Browser mit der URL der Web-App gestartet.

    ### <a name="net-core-31"></a>[.NET Core 3.1](#tab/netcore31)

    Die aktualisierte ASP.NET Core 3.1-Web-App wird auf der Seite angezeigt.

    :::image type="content" source="media/quickstart-dotnet/updated-Azure-webapp-net.png" lightbox="media/quickstart-dotnet/updated-Azure-webapp-net.png" border="true" alt-text="Visual Studio: Aktualisierte ASP.NET Core 3.1-Web-App in Azure.":::

    ### <a name="net-50"></a>[.NET 5.0](#tab/net50)

    Die aktualisierte ASP.NET Core 5.0-Web-App wird auf der Seite angezeigt.

    :::image type="content" source="media/quickstart-dotnet/updated-Azure-webapp-net.png" lightbox="media/quickstart-dotnet/updated-Azure-webapp-net.png" border="true" alt-text="Visual Studio: Aktualisierte ASP.NET Core 5.0-Web-App in Azure.":::

    ### <a name="net-framework-48"></a>[.NET Framework 4.8](#tab/netframework48)

    Die aktualisierte ASP.NET Framework 4.8-Web-App wird auf der Seite angezeigt.

    :::image type="content" source="media/quickstart-dotnet/vs-updated-Azure-webapp-net48.png" lightbox="media/quickstart-dotnet/vs-updated-Azure-webapp-net48.png" border="true" alt-text="Visual Studio: Aktualisierte ASP.NET Framework 4.8-Web-App in Azure.":::

    ---

:::zone-end

:::zone target="docs" pivot="development-environment-vscode"

1. Öffnen Sie *Index.cshtml*.
1. Ersetzen Sie das erste `<div>`-Element durch den folgenden Code:

    ```razor
    <div class="jumbotron">
        <h1>.NET 💜 Azure</h1>
        <p class="lead">Example .NET app to Azure App Service.</p>
    </div>
    ```

   Speichern Sie die Änderungen.

1. Öffnen Sie die **Seitenleiste** in Visual Studio Code, und wählen Sie das **Azure**-Symbol aus, um die zugehörigen Optionen anzuzeigen.
1. Erweitern Sie unter dem **App Service**-Knoten Ihr Abonnement, und klicken Sie mit der rechten Maustaste auf **MyFirstAzureWebApp**.
1. Wählen Sie die Option **In Web-App bereitstellen** aus.
1. Wählen Sie **Bereitstellen**, wenn Sie dazu aufgefordert werden.
1. Wenn die Veröffentlichung abgeschlossen ist, wählen Sie in der Benachrichtigung **Website durchsuchen** aus, und wählen Sie bei entsprechender Aufforderung **Öffnen**.

    ### <a name="net-core-31"></a>[.NET Core 3.1](#tab/netcore31)

    Die aktualisierte ASP.NET Core 3.1-Web-App wird auf der Seite angezeigt.

    :::image type="content" source="media/quickstart-dotnet/updated-Azure-webapp-net.png" lightbox="media/quickstart-dotnet/updated-Azure-webapp-net.png" border="true" alt-text="Visual Studio Code: Aktualisierte ASP.NET Core 3.1-Web-App in Azure.":::

    ### <a name="net-50"></a>[.NET 5.0](#tab/net50)

    Die aktualisierte ASP.NET Core 5.0-Web-App wird auf der Seite angezeigt.

    :::image type="content" source="media/quickstart-dotnet/updated-Azure-webapp-net.png" lightbox="media/quickstart-dotnet/updated-Azure-webapp-net.png" border="true" alt-text="Visual Studio Code: Aktualisierte ASP.NET Core 5.0-Web-App in Azure.":::

    ### <a name="net-framework-48"></a>[.NET Framework 4.8](#tab/netframework48)

    Die aktualisierte ASP.NET Framework 4.8-Web-App wird auf der Seite angezeigt.

    :::image type="content" source="media/quickstart-dotnet/updated-Azure-webapp-net48.png" lightbox="media/quickstart-dotnet/updated-Azure-webapp-net48.png" border="true" alt-text="Visual Studio Code: Aktualisierte ASP.NET Framework 4.8-Web-App in Azure.":::

    ---

:::zone-end

<!-- markdownlint-disable MD044 -->
:::zone target="docs" pivot="development-environment-cli"
<!-- markdownlint-enable MD044 -->

Öffnen Sie im lokalen Verzeichnis die Datei *Index.cshtml*. Ersetzen Sie das erste `<div>`-Element:

```razor
<div class="jumbotron">
    <h1>.NET 💜 Azure</h1>
    <p class="lead">Example .NET app to Azure App Service.</p>
</div>
```

Speichern Sie Ihre Änderungen, und stellen Sie die App dann mit dem Befehl `az webapp up` erneut bereit:

### <a name="net-core-31"></a>[.NET Core 3.1](#tab/netcore31)

ASP.NET Core 3.1 ist plattformübergreifend. Ersetzen Sie basierend auf Ihrer vorherigen Bereitstellung `<os>` durch `linux` oder `windows`.

```azurecli
az webapp up --os-type <os>
```

### <a name="net-50"></a>[.NET 5.0](#tab/net50)

ASP.NET Core 5.0 ist plattformübergreifend. Ersetzen Sie basierend auf Ihrer vorherigen Bereitstellung `<os>` durch `linux` oder `windows`.

```azurecli
az webapp up --os-type <os>
```

### <a name="net-framework-48"></a>[.NET Framework 4.8](#tab/netframework48)

ASP.NET Framework 4.8 weist Framework-Abhängigkeiten auf und muss unter Windows gehostet werden.

```azurecli
az webapp up --os-type windows
```

> [!TIP]
> Wenn Sie Ihre .NET-Apps unter Linux hosten möchten, sollten Sie eine Migration von [ASP.NET Framework zu ASP.NET Core](/aspnet/core/migration/proper-to-2x)in Erwägung ziehen.

---

In diesem Befehl werden lokal zwischengespeicherte Werte aus der Datei *.azure/config* verwendet (einschließlich App-Name, Ressourcengruppe und App Service-Plan).

Wechseln Sie nach Abschluss der Bereitstellung wieder zu dem Browserfenster, das im Schritt **Navigieren zur App** geöffnet wurde, und wählen Sie die Option „Aktualisieren“ aus.

### <a name="net-core-31"></a>[.NET Core 3.1](#tab/netcore31)

Die aktualisierte ASP.NET Core 3.1-Web-App wird auf der Seite angezeigt.

:::image type="content" source="media/quickstart-dotnet/updated-Azure-webapp-net.png" lightbox="media/quickstart-dotnet/updated-Azure-webapp-net.png" border="true" alt-text="CLI: Aktualisierte ASP.NET Core 3.1-Web-App in Azure.":::

### <a name="net-50"></a>[.NET 5.0](#tab/net50)

Die aktualisierte ASP.NET Core 5.0-Web-App wird auf der Seite angezeigt.

:::image type="content" source="media/quickstart-dotnet/updated-Azure-webapp-net.png" lightbox="media/quickstart-dotnet/updated-Azure-webapp-net.png" border="true" alt-text="CLI: Aktualisierte ASP.NET Core 5.0-Web-App in Azure.":::

### <a name="net-framework-48"></a>[.NET Framework 4.8](#tab/netframework48)

Die aktualisierte ASP.NET Framework 4.8-Web-App wird auf der Seite angezeigt.

:::image type="content" source="media/quickstart-dotnet/updated-Azure-webapp-net48.png" lightbox="media/quickstart-dotnet/updated-Azure-webapp-net48.png" border="true" alt-text="CLI: Aktualisierte ASP.NET Framework 4.8-Web-App in Azure.":::

---

:::zone-end

## <a name="manage-the-azure-app"></a>Verwalten der Azure-App

Wechseln Sie zum Verwalten Ihrer Web-App zum [Azure-Portal](https://portal.azure.com), und suchen Sie nach **App Services**. Wählen Sie diese Option anschließend aus.

:::image type="content" source="media/quickstart-dotnetcore/app-services.png" alt-text="Azure-Portal: Wählen Sie die Option „Azure App Services“ aus.":::

Wählen Sie auf der Seite **App Services** den Namen Ihrer Web-App aus.

:::image type="content" source="./media/quickstart-dotnetcore/select-app-service.png" alt-text="Azure-Portal: Seite „App Services“ mit einer ausgewählten Beispiel-Web-App.":::

Die Seite **Übersicht** für Ihre Web-App enthält Optionen für die grundlegende Verwaltung, z. B. Durchsuchen, Beenden, Starten, Neustarten und Löschen. Im linken Menü können Sie auf weitere Seiten für die Konfiguration Ihrer App zugreifen.

:::image type="content" source="media/quickstart-dotnetcore/web-app-overview-page.png" alt-text="Azure-Portal: Übersichtsseite „Azure App Service“.":::

<!--
## Clean up resources - H2 added from the next three includes
-->
:::zone target="docs" pivot="development-environment-vs"
[!INCLUDE [Clean-up Portal web app resources](../../includes/clean-up-section-portal-web-app.md)]
:::zone-end

:::zone target="docs" pivot="development-environment-vscode"
[!INCLUDE [Clean-up Portal web app resources](../../includes/clean-up-section-portal-web-app.md)]
:::zone-end

<!-- markdownlint-disable MD044 -->
:::zone target="docs" pivot="development-environment-cli"
<!-- markdownlint-enable MD044 -->
[!INCLUDE [Clean-up CLI resources](../../includes/cli-samples-clean-up.md)]
:::zone-end

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie eine ASP.NET-Web-App auf Azure App Service bereitgestellt.

### <a name="net-core-31"></a>[.NET Core 3.1](#tab/netcore31)

Fahren Sie mit dem nächsten Artikel fort, um sich darüber zu informieren, wie Sie eine .NET Core-App erstellen und dafür eine Verbindung mit einer SQL-Datenbank herstellen:

> [!div class="nextstepaction"]
> [Tutorial: ASP.NET Core-App mit SQL-Datenbank](tutorial-dotnetcore-sqldb-app.md)

> [!div class="nextstepaction"]
> [Konfigurieren der ASP.NET Core 3.1-App](configure-language-dotnetcore.md)

### <a name="net-50"></a>[.NET 5.0](#tab/net50)

Fahren Sie mit dem nächsten Artikel fort, um sich darüber zu informieren, wie Sie eine .NET Core-App erstellen und dafür eine Verbindung mit einer SQL-Datenbank herstellen:

> [!div class="nextstepaction"]
> [Tutorial: ASP.NET Core-App mit SQL-Datenbank](tutorial-dotnetcore-sqldb-app.md)

> [!div class="nextstepaction"]
> [Konfigurieren der ASP.NET Core 5.0-App](configure-language-dotnetcore.md)

### <a name="net-framework-48"></a>[.NET Framework 4.8](#tab/netframework48)

Fahren Sie mit dem nächsten Artikel fort, um sich darüber zu informieren, wie Sie eine .NET Framework-App erstellen und dafür eine Verbindung mit einer SQL-Datenbank herstellen:

> [!div class="nextstepaction"]
> [Tutorial: ASP.NET-App mit SQL-Datenbank](app-service-web-tutorial-dotnet-sqldatabase.md)

> [!div class="nextstepaction"]
> [Konfigurieren einer ASP.NET Framework-App](configure-language-dotnet-framework.md)

---

[app-service-pricing-tier]: https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
