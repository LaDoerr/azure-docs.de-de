---
title: 'Tutorial: REST-Tutorial mit Azure Relay'
description: 'Tutorial: Hier wird beschrieben, wie Sie eine Azure Relay-Hostanwendung erstellen, mit der eine REST-basierte Schnittstelle verfügbar gemacht wird.'
ms.topic: tutorial
ms.custom: devx-track-csharp
ms.date: 06/23/2021
ms.openlocfilehash: ed9068b6105d30d789b848a99325db389b42fec4
ms.sourcegitcommit: d9a2b122a6fb7c406e19e2af30a47643122c04da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2021
ms.locfileid: "114669734"
---
# <a name="tutorial-azure-wcf-relay-rest-tutorial"></a>Tutorial: Tutorial zu Azure WCF Relay mit REST

In diesem Tutorial wird beschrieben, wie Sie eine Azure Relay-Hostanwendung erstellen, die eine REST-basierte Schnittstelle verfügbar macht. REST ermöglicht einem Webclient, z. B. einem Webbrowser, den Zugriff auf die Service Bus-API über HTTP-Anforderungen.

Das Tutorial verwendet das WCF-REST-Programmiermodell (Windows Communication Foundation) zum Erstellen eines REST-Diensts in Azure Relay. Weitere Informationen finden Sie unter [WCF-REST-Programmiermodell](/dotnet/framework/wcf/feature-details/wcf-web-http-programming-model) und [Entwerfen und Implementieren von Diensten](/dotnet/framework/wcf/designing-and-implementing-services).

In diesem Tutorial führen Sie die folgenden Aufgaben durch:

> [!div class="checklist"]
>
> * Installieren der erforderlichen Komponenten für das Tutorial
> * Erstellen eines Relaynamespace
> * Definieren eines REST-basierten WCF-Dienstvertrags
> * Implementieren des REST-basierten WCF-Vertrags
> * Hosten und Ausführen des REST-basierten WCF-Diensts
> * Ausführen und Testen des Diensts

## <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

* Ein Azure-Abonnement. Falls Sie kein Abonnement besitzen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/), bevor Sie beginnen.
* [Visual Studio 2015 oder eine höhere Version.](https://www.visualstudio.com) Für die Beispiele in diesem Tutorial wird Visual Studio 2019 verwendet.
* Azure SDK für .NET. Installieren Sie das SDK über die [SDK-Downloadseite](https://azure.microsoft.com/downloads/).

## <a name="create-a-relay-namespace"></a>Erstellen eines Relaynamespace

Um Relay-Features in Azure verwenden zu können, müssen Sie zuerst einen Dienstnamespace erstellen. Ein Namespace ist ein Bereichscontainer für die Adressierung von Azure-Ressourcen innerhalb Ihrer Anwendung. Führen Sie [diese Anleitung](relay-create-namespace-portal.md) aus, um einen Relaynamespace zu erstellen.

## <a name="define-a-rest-based-wcf-service-contract-to-use-with-azure-relay"></a>Definieren eines REST-basierten WCF-Dienstvertrags für die Verwendung mit Azure Relay

Sie müssen den Vertrag definieren, wenn Sie einen WCF-Dienst im REST-Stil erstellen. Der Vertrag gibt an, welche Vorgänge der Host unterstützt. Ein Dienstvorgang ähnelt einer Webdienstmethode. Definieren Sie einen Vertrag mit einer C++-, C#- oder Visual Basic-Schnittstelle. Jede Methode in der Schnittstelle entspricht einem bestimmten Dienstvorgang. Wenden Sie auf jede Schnittstelle das Attribut [ServiceContractAttribute](/dotnet/api/system.servicemodel.servicecontractattribute) und auf jeden Vorgang das Attribut [OperationContractAttribute](/dotnet/api/system.servicemodel.operationcontractattribute) an. 

> [!TIP]
> Wenn eine Methode in einer Schnittstelle mit [ServiceContractAttribute](/dotnet/api/system.servicemodel.servicecontractattribute) kein Attribut vom Typ [OperationContractAttribute](/dotnet/api/system.servicemodel.operationcontractattribute) besitzt, wird diese Methode nicht verfügbar gemacht. Der für diese Aufgaben verwendete Code wird im Beispiel nach dem Verfahren veranschaulicht.

Der Hauptunterschied zwischen einem WCF-Vertrag und einem Vertrag im REST-Stil ist das Hinzufügen einer Eigenschaft zum Attribut [OperationContractAttribute](/dotnet/api/system.servicemodel.operationcontractattribute): [WebGetAttribute](/dotnet/api/system.servicemodel.web.webgetattribute). Mit dieser Eigenschaft können Sie eine Methode in der Schnittstelle einer Methode auf der anderen Seite der Schnittstelle zuordnen. In diesem Beispiel wird das [WebGetAttribute](/dotnet/api/system.servicemodel.web.webgetattribute)-Attribut verwendet, um eine Methode mit `HTTP GET` zu verknüpfen. Mit diesem Ansatz kann Service Bus Befehle, die an die Schnittstelle gesendet werden, präzise abrufen und interpretieren.

### <a name="to-create-a-contract-with-an-interface"></a>So erstellen Sie einen Vertrag mit einer Schnittstelle

1. Starten Sie Microsoft Visual Studio als Administrator. Klicken Sie hierzu mit der rechten Maustaste auf das Visual Studio-Programmsymbol, und wählen Sie **Als Administrator ausführen** aus.
1. Wählen Sie in Visual Studio **Neues Projekt erstellen** aus.
1. Wählen Sie unter **Neues Projekt erstellen** die Option **Konsolen-App (.NET Framework)** für C# und dann **Weiter** aus.
1. Geben Sie dem Projekt den Namen *ImageListener*. Übernehmen Sie den vorgegebenen **Speicherort**, und wählen Sie anschließend **Erstellen** aus.

   Für ein C#-Projekt erstellt Visual Studio die Datei *Program.cs*. Diese Klasse enthält eine leere `Main()`-Methode, die erforderlich ist, damit ein Konsolenanwendungsprojekt ordnungsgemäß erstellt wird.

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **ImageListener**, und wählen Sie **NuGet-Pakete verwalten** aus.
1. Wählen Sie **Durchsuchen** aus, suchen Sie nach **WindowsAzure.ServiceBus**, und wählen Sie diese Option aus. Wählen Sie **Installieren** aus, und akzeptieren Sie die Nutzungsbedingungen.

    In diesem Schritt wird auf Service Bus und *System.ServiceModel.dll* verwiesen. Mit diesem Paket werden automatisch Verweise auf die Service Bus-Bibliotheken und das WCF-Element `System.ServiceModel` hinzugefügt.

1. Fügen Sie dem Projekt explizit einen Verweis auf `System.ServiceModel.Web.dll` hinzu. Klicken Sie im **Projektmappen-Explorer** unter dem Projektordner mit der rechten Maustaste auf **Verweise**, und wählen Sie **Verweis hinzufügen** aus.
1. Wählen Sie unter **Verweis hinzufügen** die Option **Framework** aus, und geben Sie unter **Suche** den Suchbegriff *System.ServiceModel.Web* ein. Aktivieren Sie das Kontrollkästchen **System.ServiceModel.Web**, und klicken Sie anschließend auf **OK**.

Nehmen Sie als Nächstes die folgenden Codeänderungen am Projekt vor:

1. Fügen Sie am Anfang der Datei *Program.cs* die folgenden `using`-Anweisungen hinzu.

    ```csharp
    using System.ServiceModel;
    using System.ServiceModel.Channels;
    using System.ServiceModel.Web;
    using System.IO;
    ```

    * [System.ServiceModel](/dotnet/api/system.servicemodel) ist der Namespace, der den programmgesteuerten Zugriff auf die grundlegenden Funktionen von WCF ermöglicht. WCF-Relay nutzt viele Objekte und Attribute von WCF, um Dienstverträge zu definieren. Sie verwenden diesen Namespace in den meisten Ihrer Relayanwendungen.
    * [System.ServiceModel.Channels](/dotnet/api/system.servicemodel.channels) trägt zur Definition des Kanals bei. Dies ist das Objekt, über das Sie mit Azure Relay und dem Clientwebbrowser kommunizieren.
    * [System.ServiceModel.Web](/dotnet/api/system.servicemodel.web) enthält die Typen, mit denen Sie webbasierte Anwendungen erstellen können.

1. Benennen Sie den Namespace `ImageListener` in `Microsoft.ServiceBus.Samples` um.

    ```csharp
    namespace Microsoft.ServiceBus.Samples
    {
        ...
    ```

1. Definieren Sie direkt nach der öffnenden geschweiften Klammer der Namespacedeklaration eine neue Schnittstelle mit dem Namen `IImageContract`, und wenden Sie das Attribut `ServiceContractAttribute` mit dem Wert `https://samples.microsoft.com/ServiceModel/Relay/RESTTutorial1` auf die Schnittstelle an. 

    ```csharp
    [ServiceContract(Name = "ImageContract", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/RESTTutorial1")]
    public interface IImageContract
    {
    }
    ```

    Der Namespacewert unterscheidet sich von dem Namespace, den Sie im gesamten Codebereich verwenden. Der Namespacewert ist ein eindeutiger Bezeichner für diesen Vertrag und sollte über Versionsinformationen verfügen. Weitere Informationen finden Sie unter [Dienstversionsverwaltung](/dotnet/framework/wcf/service-versioning). Das explizite Angeben des Namespace verhindert, dass der Standardwert für den Namespace dem Vertragsnamen hinzugefügt wird.

1. Deklarieren Sie innerhalb der `IImageContract`-Benutzeroberfläche eine Methode für den einzelnen Vorgang, den der `IImageContract`-Vertrag in der Schnittstelle verfügbar macht, und wenden Sie das `OperationContract`-Attribut auf die Methode an, die Sie als Teil des öffentlichen Service Bus-Vertrags verfügbar machen möchten.

    ```csharp
    public interface IImageContract
    {
        [OperationContract]
        Stream GetImage();
    }
    ```

1. Fügen Sie im Attribut `OperationContract` den Wert `WebGet` hinzu.

    ```csharp
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }
    ```

   Durch das Hinzufügen des Werts `WebGet` kann der Relaydienst HTTP GET-Anforderungen an `GetImage` weiterleiten und die Rückgabewerte von `GetImage` in eine `HTTP GETRESPONSE`-Antwort übersetzen. Später in diesem Tutorial verwenden Sie einen Webbrowser für den Zugriff auf diese Methode und zum Anzeigen des Bilds im Browser.

1. Direkt nach der `IImageContract`-Definition deklarieren Sie einen Kanal, der von den beiden `IImageContract`- und `IClientChannel`-Schnittstellen erbt.

    ```csharp
    public interface IImageChannel : IImageContract, IClientChannel { }
    ```

   Ein Kanal ist das WCF-Objekt, über das der Dienst und der Client gegenseitig Informationen übergeben. Später erstellen Sie den Kanal in Ihrer Hostanwendung. Azure Relay verwendet dann diesen Kanal, um die HTTP GET-Anforderungen vom Browser an Ihre `GetImage`-Implementierung zu übergeben. Das Relay verwendet den Kanal auch, um den `GetImage`-Rückgabewert in eine `HTTP GETRESPONSE`-Antwort für den Clientbrowser zu übersetzen.

1. Wählen Sie **Erstellen** > **Projektmappe erstellen** aus, um die Korrektheit Ihrer bisherigen Arbeit zu bestätigen.

### <a name="example-that-defines-a-wcf-relay-contract"></a>Beispiel: Definition eines WCF Relay-Vertrags

Das folgende Codebeispiel zeigt eine Basisschnittstelle, die einen WCF-Relayvertrag definiert.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IImageContract", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## <a name="implement-the-rest-based-wcf-service-contract"></a>Implementieren des REST-basierten WCF-Dienstvertrags

Für die Erstellung eines WCF Relay-Diensts im REST-Stil müssen Sie zuerst den Vertrag erstellen, indem Sie eine Schnittstelle verwenden. Der nächste Schritt ist das Implementieren der Schnittstelle. Dieser Vorgang umfasst das Erstellen einer Klasse mit dem Namen `ImageService`, mit der die benutzerdefinierte `IImageContract`-Schnittstelle implementiert wird. Nachdem Sie den Vertrag implementiert haben, konfigurieren Sie die Schnittstelle mit der Datei *App.config*. Die Konfigurationsdatei enthält die erforderlichen Informationen für die Anwendung. Dies sind der Name des Diensts, der Name des Vertrags und der Protokolltyp, der für die Kommunikation mit dem Relaydienst verwendet wird. Der für diese Aufgaben verwendete Code wird im Beispiel nach dem Verfahren veranschaulicht.

Wie bei den vorherigen Schritten gibt es nur wenige Unterschiede zwischen der Implementierung eines Vertrags im REST-Stil und eines WCF Relay-Vertrags.

### <a name="to-implement-a-rest-style-service-bus-contract"></a>So implementieren Sie einen Service Bus-Vertrag im REST-Stil

1. Erstellen Sie eine neue Klasse namens `ImageService` direkt unterhalb der Definition der `IImageContract`-Schnittstelle. Die `ImageService`-Klasse implementiert die `IImageContract`-Schnittstelle.

    ```csharp
    class ImageService : IImageContract
    {
    }
    ```

    Ähnlich wie bei anderen Schnittstellenimplementierungen können Sie die Definition in einer anderen Datei implementieren. In diesem Tutorial findet die Implementierung allerdings in derselben Datei wie die Schnittstellendefinition und die `Main()`-Methode statt.

1. Wenden Sie das Attribut [ServiceBehaviorAttribute](/dotnet/api/system.servicemodel.servicebehaviorattribute) auf die `IImageService`-Klasse an, um anzugeben, dass die Klasse eine Implementierung eines WCF-Vertrags ist.

    ```csharp
    [ServiceBehavior(Name = "ImageService", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
    }
    ```

    Wie bereits erwähnt, ist dieser Namespace kein herkömmlicher Namespace. Er ist Teil der WCF-Architektur, die den Vertrag identifiziert. Weitere Informationen finden Sie unter [Datenvertragsnamen](/dotnet/framework/wcf/feature-details/data-contract-names/).

1. Fügen Sie dem Projekt ein *JPG*-Bild hinzu. Diese Datei ist ein Bild, das der Dienst im empfangenden Browser anzeigt.

   1. Klicken Sie mit der rechten Maustaste auf Ihr Projekt, und wählen Sie **Hinzufügen** aus.
   1. Wählen anschließend **Vorhandenes Element** aus.
   1. Verwenden Sie **Vorhandenes Element hinzufügen**, um zu einer geeigneten JPG-Datei zu navigieren, und wählen Sie anschließend **Hinzufügen** aus. Wählen Sie beim Hinzufügen der Datei in der Dropdownliste neben **Dateiname** die Option **Alle Dateien** aus.

   Im restlichen Teil dieses Tutorials wird davon ausgegangen, dass der Name des Bilds *image.jpg* lautet. Wenn Sie eine andere Datei haben, müssen Sie das Bild umbenennen oder den Code entsprechend ändern.

1. Stellen Sie sicher, dass der ausgeführte Dienst die Bilddatei finden kann. Klicken Sie hierzu im **Projektmappen-Explorer** mit der rechten Maustaste auf die Bilddatei, und wählen Sie anschließend **Eigenschaften** aus. Legen Sie unter **Eigenschaften** den Wert für **In Ausgabeverzeichnis kopieren** auf **Kopieren, wenn neuer** fest.

1. Verwenden Sie die Prozedur unter [So erstellen Sie einen Vertrag mit einer Schnittstelle](#to-create-a-contract-with-an-interface), um dem Projekt einen Verweis auf die Assembly *System.Drawing.dll* hinzuzufügen.

1. Fügen Sie die folgenden zugeordneten `using`-Anweisungen hinzu:

    ```csharp
    using System.Drawing;
    using System.Drawing.Imaging;
    using Microsoft.ServiceBus;
    using Microsoft.ServiceBus.Web;
    ```

1. Fügen Sie in der `ImageService`-Klasse den folgenden Konstruktor hinzu, der die Bitmap lädt und das Senden an den Clientbrowser vorbereitet:

    ```csharp
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";
   
        Image bitmap;
   
        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }
    }
    ```

1. Fügen Sie direkt nach dem vorherigen Code die folgende `GetImage`-Methode in der `ImageService`-Klasse hinzu, um eine HTTP-Nachricht mit dem Bild zurückzugeben.

    ```csharp
    public Stream GetImage()
    {
        MemoryStream stream = new MemoryStream();
        this.bitmap.Save(stream, ImageFormat.Jpeg);
   
        stream.Position = 0;
        WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";
   
        return stream;
    }
    ```

    Diese Implementierung verwendet `MemoryStream` zum Abrufen des Bilds sowie zum Vorbereiten des Streamings an den Browser. Sie startet die Streamposition bei null, deklariert den Streaminhalt als *JPG* und streamt die Informationen.

1. Wählen Sie **Erstellen** > **Projektmappe erstellen** aus.

### <a name="to-define-the-configuration-for-running-the-web-service-on-service-bus"></a>So definieren Sie die Konfiguration zum Ausführen des Webdiensts auf Service Bus

1. Doppelklicken Sie im **Projektmappen-Explorer** auf **App.config**, um die Datei im Visual Studio-Editor zu öffnen.

    Die Datei *App.config* enthält den Dienstnamen, den Endpunkt und die Bindung. Der Endpunkt ist der Ort, der von Azure Relay für Clients und Hosts verfügbar gemacht wird, um die Kommunikation zu ermöglichen. Die Bindung ist der Protokolltyp, der für die Kommunikation verwendet wird. Der Hauptunterschied besteht hier darin, dass der konfigurierte Dienstendpunkt auf eine [WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding)-Bindung verweist.

1. Das XML-Element `<system.serviceModel>` ist ein WCF-Element und definiert mindestens einen Dienst. Hier wird es verwendet, um den Dienstnamen und den Endpunkt zu definieren. Fügen Sie am Ende des `<system.serviceModel>`-Elements – aber immer noch innerhalb von `<system.serviceModel>` – ein `<bindings>`-Element mit dem weiter unten angegebenen Inhalt hinzu:

    ```xml
    <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
            <binding name="default">
                <security relayClientAuthenticationType="None" />
            </binding>
        </webHttpRelayBinding>
    </bindings>
    ```

    Mit diesem Inhalt werden die in der Anwendung verwendeten Bindungen definiert. Sie können mehrere Bindungen definieren, aber in diesem Tutorial definieren Sie nur eine.

    Mit dem obigen Code wird die WCF Relay-Bindung [WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) definiert, für die `relayClientAuthenticationType` auf `None` festgelegt ist. Diese Einstellung gibt an, dass ein Endpunkt mit dieser Bindung keine Clientanmeldeinformationen erfordert.

1. Fügen Sie nach dem `<bindings>`-Element ein `<services>`-Element hinzu. Ähnlich wie Bindungen können Sie mehrere Dienste in einer einzigen Konfigurationsdatei definieren. Für dieses Lernprogramm definieren Sie aber nur einen.

    ```xml
    <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
            <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IImageContract"
                    binding="webHttpRelayBinding"
                    bindingConfiguration="default"
                    behaviorConfiguration="sbTokenProvider"
                    address="" />
        </service>
    </services>
    ```

    Mit diesem Inhalt wird ein Dienst konfiguriert, für den das zuvor definierte `webHttpRelayBinding`-Standardelement verwendet wird. Darüber hinaus verwendet er den standardmäßigen `sbTokenProvider`-Anbieter, der im nächsten Schritt definiert wird.

1. Erstellen Sie nach dem `<services>`-Element ein `<behaviors>`-Element mit dem folgenden Inhalt, und ersetzen Sie `SAS_KEY` durch den SAS-Schlüssel (Shared Access Signature). Informationen zum Abrufen eines SAS-Schlüssels über das [Azure-Portal][Azure portal] finden Sie unter [Abrufen von Anmeldeinformationen für die Verwaltung](service-bus-relay-tutorial.md#get-management-credentials).

    ```xml
    <behaviors>
        <endpointBehaviors>
            <behavior name="sbTokenProvider">
                <transportClientEndpointBehavior>
                    <tokenProvider>
                        <sharedAccessSignature keyName="RootManageSharedAccessKey" key="YOUR_SAS_KEY" />
                    </tokenProvider>
                </transportClientEndpointBehavior>
            </behavior>
            </endpointBehaviors>
            <serviceBehaviors>
                <behavior name="default">
                    <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
                </behavior>
            </serviceBehaviors>
    </behaviors>
    ```

1. Ersetzen Sie in *App.config* im `<appSettings>`-Element den gesamten Wert der Verbindungszeichenfolge durch die Verbindungszeichenfolge, die Sie zuvor aus dem Portal abgerufen haben.

    ```xml
    <appSettings>
       <!-- Service Bus specific app settings for messaging connections -->
       <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_SAS_KEY"/>
    </appSettings>
    ```

1. Wählen Sie **Erstellen** > **Projektmappe erstellen** aus, um die gesamte Lösung zu erstellen.

### <a name="example-that-implements-the-rest-based-wcf-service-contract"></a>Beispiel: Implementierung des REST-basierten WCF-Dienstvertrags

Der folgende Code veranschaulicht die Vertrags- und Dienstimplementierung für einen REST-basierten Dienst, der mit der `WebHttpRelayBinding`-Bindung auf dem Service Bus ausgeführt wird.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{


    [ServiceContract(Name = "ImageContract", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

Das folgende Beispiel zeigt die Datei *App.config*, die dem Dienst zugeordnet ist.

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2"/>
    </startup>
    <system.serviceModel>
        <extensions>
            <!-- In this extension section we are introducing all known service bus extensions. User can remove the ones they don't need. -->
            <behaviorExtensions>
                <add name="connectionStatusBehavior"
                    type="Microsoft.ServiceBus.Configuration.ConnectionStatusElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="transportClientEndpointBehavior"
                    type="Microsoft.ServiceBus.Configuration.TransportClientEndpointBehaviorElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="serviceRegistrySettings"
                    type="Microsoft.ServiceBus.Configuration.ServiceRegistrySettingsElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </behaviorExtensions>
            <bindingElementExtensions>
                <add name="netMessagingTransport"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingTransportExtensionElement, Microsoft.ServiceBus,  Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="tcpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.TcpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpsRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpsRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="onewayRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.RelayedOnewayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingElementExtensions>
            <bindingExtensions>
                <add name="basicHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.BasicHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="webHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WebHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="ws2007HttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WS2007HttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netOnewayRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetOnewayRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netEventRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetEventRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netMessagingBinding"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingExtensions>
        </extensions>
      <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
          <binding name="default">
            <security relayClientAuthenticationType="None" />
          </binding>
        </webHttpRelayBinding>
      </bindings>
      <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
          <endpoint name="RelayEndpoint"
                  contract="Microsoft.ServiceBus.Samples.IImageContract"
                  binding="webHttpRelayBinding"
                  bindingConfiguration="default"
                  behaviorConfiguration="sbTokenProvider"
                  address="" />
        </service>
      </services>
      <behaviors>
        <endpointBehaviors>
          <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
              <tokenProvider>
                <sharedAccessSignature keyName="RootManageSharedAccessKey" key="YOUR_SAS_KEY" />
              </tokenProvider>
            </transportClientEndpointBehavior>
          </behavior>
        </endpointBehaviors>
        <serviceBehaviors>
          <behavior name="default">
            <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
          </behavior>
        </serviceBehaviors>
      </behaviors>
    </system.serviceModel>
    <appSettings>
        <!-- Service Bus specific app settings for messaging connections -->
        <add key="Microsoft.ServiceBus.ConnectionString"
            value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_SAS_KEY>"/>
    </appSettings>
</configuration>
```

## <a name="host-the-rest-based-wcf-service-to-use-azure-relay"></a>Hosten des REST-basierten WCF-Diensts zur Verwendung von Azure Relay

In diesem Abschnitt wird beschrieben, wie ein Webdienst mithilfe einer Konsolenanwendung mit WCF Relay ausgeführt wird. Eine vollständige Auflistung des Codes, der in diesem Abschnitt geschrieben wird, finden Sie in dem Beispiel, das auf die Prozedur folgt.

### <a name="to-create-a-base-address-for-the-service"></a>So erstellen Sie eine Basisadresse für den Dienst

1. Erstellen Sie in der `Main()`-Funktionsdeklaration eine Variable zum Speichern des Namespaces Ihres Projekts. Ersetzen Sie `yourNamespace` durch den Namen des zuvor erstellten Relaynamespaces.

    ```csharp
    string serviceNamespace = "yourNamespace";
    ```

    Service Bus verwendet den Namespace-Namen, um einen eindeutigen URI zu erstellen.

1. Erstellen Sie eine `Uri`-Instanz für die Basisadresse des Diensts, der auf dem Namespace basiert.

    ```csharp
    Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");
    ```

### <a name="to-create-and-configure-the-web-service-host"></a>So erstellen und konfigurieren Sie den Webdiensthost

Erstellen Sie in `Main()` den Webdiensthost mithilfe der URI-Adresse, die Sie weiter oben in diesem Abschnitt erstellt haben.
  
```csharp
WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
```

Der Diensthost ist das WCF-Objekt, das die Hostanwendung instanziiert. In diesem Beispiel wird Folgendes übergeben: der Typ des Hosts (`ImageService`), den Sie erstellen möchten, und die Adresse, unter der Sie die Hostanwendung verfügbar machen möchten.

### <a name="to-run-the-web-service-host"></a>So führen Sie den Webdiensthost aus

1. Fügen Sie in `Main()` die folgende Zeile hinzu, um den Dienst zu öffnen.

    ```csharp
    host.Open();
    ```

    Der Dienst wird jetzt ausgeführt.

1. Zeigen Sie eine Meldung an, die angibt, dass der Dienst ausgeführt wird und wie er beendet wird.

    ```csharp
    Console.WriteLine("Copy the following address into a browser to see the image: ");
    Console.WriteLine(address + "GetImage");
    Console.WriteLine();
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```

1. Schließen Sie danach den Diensthost.

    ```csharp
    host.Close();
    ```

### <a name="example-of-the-service-contract-and-implementation"></a>Beispiel: Dienstvertrag und Implementierung

Das folgende Beispiel schließt den Dienstvertrag und die Implementierung aus früheren Schritten des Lernprogramms ein und hostet den Dienst in einer Konsolenanwendung. Kompilieren Sie den folgenden Code in eine ausführbare Datei mit dem Namen *ImageListener.exe*.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "ImageContract", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string serviceNamespace = "InsertServiceNamespaceHere";
            Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");

            WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
            host.Open();

            Console.WriteLine("Copy the following address into a browser to see the image: ");
            Console.WriteLine(address + "GetImage");
            Console.WriteLine();
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            host.Close();
        }
    }
}
```

## <a name="run-and-test-the-service"></a>Ausführen und Testen des Diensts

Führen Sie nach dem Erstellen der Projektmappe Folgendes aus, um die Anwendung auszuführen:

1. Wählen Sie F5 aus, oder navigieren Sie zum Speicherort der ausführbaren Datei (*ImageListener\bin\Debug\ImageListener.exe*), um den Dienst auszuführen. Behalten Sie die Ausführung der App bei, da sie für den nächsten Schritt benötigt wird.
1. Kopieren Sie die Adresse aus der Eingabeaufforderung in einen Browser, um das Bild anzuzeigen.
1. Wählen Sie nach Abschluss des Vorgangs im Eingabeaufforderungsfenster die EINGABETASTE aus, um die App zu schließen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie jetzt eine Anwendung erstellt haben, die den Azure Relay-Dienst verwendet, finden Sie in den folgenden Artikeln weitere Informationen:

* [Was ist Azure Relay?](relay-what-is-it.md)
* [Verfügbarmachen eines lokalen WCF REST-Diensts für einen externen Client mithilfe von Azure WCF Relay](service-bus-relay-tutorial.md)

[Azure portal]: https://portal.azure.com
