---
author: mikben
ms.service: azure-communication-services
ms.topic: include
ms.date: 03/10/2021
ms.author: mikben
ms.openlocfilehash: 7589190f29e7d85bfb49a868dde9b261882d28b0
ms.sourcegitcommit: b11257b15f7f16ed01b9a78c471debb81c30f20c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2021
ms.locfileid: "111593555"
---
In dieser Schnellstartanleitung erfahren Sie, wie Sie einen Anruf mithilfe des Calling SDK von Azure Communication Services für Windows einleiten.

> [!NOTE]
> Den fertigen Code für diesen Schnellstart finden Sie auf [GitHub](https://github.com/Azure-Samples/communication-services-dotnet-quickstarts/tree/main/VoiceCalling).

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 
- Installieren Sie [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) mit der Workload „Entwicklung für die universelle Windows-Plattform“. 
- Eine bereitgestellte Communication Services-Ressource. [Erstellen einer Communication Services-Ressource](../../../create-communication-resource.md)
- Ein [Benutzerzugriffstoken](../../../access-tokens.md) für Ihren Azure Communication Service

## <a name="setting-up"></a>Einrichten

### <a name="creating-the-project"></a>Erstellen des Projekts

Erstellen Sie in Visual Studio ein neues Projekt mit der Vorlage **Leere App (Universelle Windows-App)** , um eine einseitige UWP-App (Universelle Windows-Plattform) einzurichten.

:::image type="content" source="../../media/windows/create-a-new-project.png" alt-text="Screenshot des Fensters „Neues Projekt“ in Visual Studio.":::

### <a name="install-the-package"></a>Installieren des Pakets

Klicken Sie mit der rechten Maustaste auf Ihr Projekt, und wechseln Sie zu `Manage Nuget Packages`, um `Azure.Communication.Calling` zu installieren. 

### <a name="request-access"></a>Anfordern des Zugriffs

Wechseln Sie zu `Package.appxmanifest`, und klicken Sie auf `Capabilities`.
Aktivieren Sie `Internet (Client & Server)`, um ein- und ausgehenden Zugriff auf das Internet zu erhalten. Aktivieren Sie `Microphone`, um auf den Audiofeed des Mikrofons zuzugreifen. 

:::image type="content" source="../../media/windows/request-access.png" alt-text="Screenshot: Anfordern des Zugriffs auf das Internet und das Mikrofon in Visual Studio":::

### <a name="set-up-the-app-framework"></a>Einrichten des App-Frameworks

Wir müssen ein einfaches Layout konfigurieren, um unsere Logik anzufügen. Um einen ausgehenden Anruf zu tätigen, benötigen wir ein `TextBox`-Element, um die Benutzer-ID des Angerufenen bereitzustellen. Außerdem benötigen wir die Schaltflächen `Start Call` und `Hang Up`. Öffnen Sie das `MainPage.xaml`-Element Ihres Projekts, und fügen Sie Ihrer `Page` den Knoten `StackPanel` hinzu: 

```C#
<Page
    x:Class="CallingQuickstart.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:CallingQuickstart"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <StackPanel>
        <TextBox Text="Who would you like to call?" TextWrapping="Wrap" x:Name="CalleeTextBox" Margin="10,10,10,10"></TextBox>
        <Button Content="Start Call" Click="CallButton_ClickAsync" x:Name="CallButton" Margin="10,10,10,10"></Button>
        <Button Content="Hang Up" Click="HangupButton_Click" x:Name="HangupButton" Margin="10,10,10,10"></Button>
    </StackPanel>
</Page>
```

Öffnen Sie `MainPage.xaml.cs`, und ersetzen Sie den Inhalt durch folgende Implementierung: 
```C#
using System;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;

using Azure.Communication;
using Azure.Communication.Calling;

namespace CallingQuickstart
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            this.InitCallAgent();
        }
        
        private async void InitCallAgent()
        {
            // Create Call Client and initialize Call Agent
        }
        
        private async void CallButton_ClickAsync(object sender, RoutedEventArgs e)
        {
            // Start call
        }

        private async void HangupButton_Click(object sender, RoutedEventArgs e)
        {
            // End the current call
        }

        CallClient call_client_;
        CallAgent call_agent_;
        Call call_;
    }
}
```

## <a name="object-model"></a>Objektmodell

Die folgenden Klassen und Schnittstellen befassen sich mit einigen der wichtigsten Features des Azure Communication Services Calling SDK:

| Name                                  | Beschreibung                                                  |
| ------------------------------------- | ------------------------------------------------------------ |
| CallClient | „CallClient“ ist der Haupteinstiegspunkt des Calling SDK.|
| CallAgent | CallAgent dient zum Starten und Verwalten von Anrufen. |
| CommunicationTokenCredential | „CommunicationTokenCredential“ dient als tokengestützte Anmeldeinformation zum Instanziieren von „CallAgent“.| 
| CommunicationUserIdentifier | „CommunicationUserIdentifier“ wird zur Darstellung der Identität des Benutzers verwendet, wie u. a. die folgenden: CommunicationUserIdentifier/PhoneNumberIdentifier/CallingApplication. |

## <a name="authenticate-the-client"></a>Authentifizieren des Clients

Initialisieren Sie eine `CallAgent`-Instanz mit einem Benutzerzugriffstoken, das es uns ermöglicht, Anrufe zu tätigen und zu empfangen. Fügen Sie der Funktion `InitCallAgent` den folgenden Code hinzu: 

```C#
CommunicationTokenCredential token_credential = new CommunicationTokenCredential("<USER_ACCESS_TOKEN>");
call_client_ = new CallClient();

CallAgentOptions callAgentOptions = new CallAgentOptions()
{
    DisplayName = "<YOUR_DISPLAY_NAME>"
};
call_agent_ = await call_client_.CreateCallAgent(token_credential, callAgentOptions);
```

## <a name="start-a-call"></a>Beginnen eines Anrufs

Fügen Sie die Implementierung zu `CallButton_ClickAsync` hinzu, um einen Aufruf mit dem erstellten `call_agent` einzuleiten. 

```C#
StartCallOptions startCallOptions = new StartCallOptions();
ICommunicationIdentifier[] callees = new ICommunicationIdentifier[1]
{
    new CommunicationUserIdentifier(CalleeTextBox.Text)
};
call_ = await call_agent_.StartCallAsync(callees, startCallOptions);
```

## <a name="end-a-call"></a>Beenden eines Anrufs

Beenden Sie den aktuellen Anruf, nachdem auf die Schaltfläche `Hang Up` geklickt wurde. 

```C#
private async void HangupButton_Click(object sender, RoutedEventArgs e)
{
    await call_.HangUpAsync(new HangUpOptions());
}
```

## <a name="run-the-code"></a>Ausführen des Codes

Sie können den Build in Visual Studio erstellen und den Code ausführen. Beachten Sie, dass die Lösungsplattformen `ARM64`, `x64` und `x86` unterstützt werden. 

Sie können einen ausgehenden Anruf tätigen, indem Sie eine Benutzer-ID im Textfeld eingeben und auf die Schaltfläche `Start Call` klicken. Wenn Sie `8:echo123` anrufen, werden Sie mit einem Echobot verbunden. Dies eignet sich hervorragend für die ersten Schritte und zum Überprüfen, ob Ihre Audiogeräte funktionieren.

:::image type="content" source="../../media/windows/run-the-app.png" alt-text="Screenshot: Ausführen der Schnellstart-App":::

## <a name="sample-code"></a>Beispielcode

Sie können die Beispiel-App von [GitHub](https://github.com/Azure-Samples/communication-services-dotnet-quickstarts/tree/main/VoiceCalling) herunterladen.
