---
title: DRM-Verschlüsselung und Lizenzbereitstellung für Media Services
description: Hier erfahren Sie, wie Sie mit der dynamischen DRM-Verschlüsselung und dem Lizenzbereitstellungsdienst Streams bereitstellen, die mit Microsoft PlayReady-, Google Widevine- oder Apple FairPlay-Lizenzen verschlüsselt sind.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/25/2021
ms.author: inhenkel
ms.custom: seodec18
ms.openlocfilehash: ac364950b78aeb61bd74fcc918a4dae2a31a6ffa
ms.sourcegitcommit: 63f3fc5791f9393f8f242e2fb4cce9faf78f4f07
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2021
ms.locfileid: "114690312"
---
# <a name="tutorial-use-drm-dynamic-encryption-and-license-delivery-service"></a>Tutorial: Verwenden der dynamischen DRM-Verschlüsselung und des Lizenzbereitstellungsdiensts

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

> [!NOTE]
> Obwohl in diesem Tutorial die [.NET SDK](/dotnet/api/microsoft.azure.management.media.models.liveevent)-Beispiele verwendet werden, sind die allgemeinen Schritte für die [REST-API](/rest/api/media/liveevents), die [CLI](/cli/azure/ams/live-event) oder für andere unterstützte [SDKs](media-services-apis-overview.md#sdks) dieselben.

Mithilfe von Azure Media Services können Sie Streams mit Microsoft PlayReady-, Google Widevine- und Apple FairPlay-Lizenzen verschlüsselt übermitteln. Ausführliche Erläuterungen finden Sie unter [Inhaltsschutz mit dynamischer Verschlüsselung](drm-content-protection-concept.md).

Außerdem bietet Media Services einen Dienst zum Übermitteln von PlayReady-, Widevine- und FairPlay-DMR-Lizenzen. Wenn ein Benutzer die mit DRM geschützten Inhalte anfordert, fordert die Player-App eine Lizenz vom Media Services-Lizenzdienst an. Wenn die Player-App autorisiert wurde, gibt der Media Services-Lizenzdienst eine Lizenz für den Player aus. Eine Lizenz enthält den Entschlüsselungsschlüssel, der vom Clientplayer zum Entschlüsseln und Streamen des Inhalts verwendet werden kann.

Dieser Artikel basiert auf dem Beispiel für [Verschlüsseln mit DRM](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/main/AMSV3Tutorials/EncryptWithDRM).

Das in diesem Artikel beschriebene Beispiel führt zu folgendem Ergebnis:

![AMS mit DRM-Schutz (Video) in Azure Media Player](./media/drm-protect-with-drm-tutorial/ams_player.png)

Dieses Tutorial veranschaulicht folgende Vorgehensweisen:

> [!div class="checklist"]
> * Erstellen einer Codierungstransformation.
> * Legen Sie den Signaturschlüssel fest, der für die Überprüfung des Tokens verwendet wird.
> * Festlegen der Anforderungen für die Richtlinie für Inhaltsschlüssel.
> * Erstellen eines Streaminglocators mit der angegebenen Streamingrichtlinie.
> * Erstellen einer URL zum Wiedergeben der Datei.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* Lesen Sie den Artikel [Übersicht über den Inhaltsschutz](drm-content-protection-concept.md).
* Lesen Sie die Informationen unter [Entwurf eines Multi-DRM-Inhaltsschutzsystems mit Zugriffssteuerung](architecture-design-multi-drm-system.md).
* Installieren Sie Visual Studio Code oder Visual Studio.
* Erstellen Sie ein Azure Media Services-Konto, wie in [dieser Schnellstartanleitung](./account-create-how-to.md) beschrieben.
* Rufen Sie die zur Verwendung von Media Services-APIs erforderlichen Anmeldeinformationen ab (siehe [Zugreifen auf APIs](./access-api-howto.md)).
* Legen Sie die entsprechenden Werte in der App-Konfigurationsdatei („appsettings.json“ oder ENV-Datei) fest.

## <a name="download-the-code-and-configure-the-sample"></a>Herunterladen des Codes und Konfigurieren des Beispiels

Klonen Sie ein GitHub-Repository, das das vollständige in diesem Artikel beschriebene .NET-Beispiel enthält, mit dem folgenden Befehl auf Ihrem Computer:

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git
 ```
 
Das Beispiel „Encrypt with DRM“ befindet sich im Ordner [EncryptWithDRM](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/main/AMSV3Tutorials/EncryptWithDRM).

[!INCLUDE [appsettings or .env file](./includes/note-appsettings-or-env-file.md)]

> [!NOTE]
> Mit dem Beispiel werden bei jeder Ausführung der App eindeutige Ressourcen erstellt. Normalerweise verwenden Sie vorhandene Ressourcen wie Transformationen und Richtlinien nochmal (wenn vorhandene Ressourcen über erforderliche Konfigurationen verfügen).

### <a name="start-using-media-services-apis-with-the-net-sdk"></a>Beginnen mit der Verwendung von Media Services-APIs mit dem .NET SDK

Um mit der Verwendung von Media Services-APIs in .NET zu beginnen, müssen Sie ein `AzureMediaServicesClient`-Objekt erstellen. Zum Erstellen des Objekts müssen Sie Anmeldeinformationen für den Client bereitstellen, damit dieser per Azure Active Directory eine Verbindung mit Azure herstellen kann. Eine weitere Option ist die interaktive Authentifizierung, die in `GetCredentialsInteractiveAuthAsync` implementiert ist.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/Common_Utils/Authentication.cs#CreateMediaServicesClientAsync)]

Im Code, den Sie am Anfang des Artikels geklont haben, wird mit der Funktion `GetCredentialsAsync` das Objekt `ServiceClientCredentials` erstellt. Dies erfolgt basierend auf den Anmeldeinformationen in der lokalen Konfigurationsdatei (*appsettings.json*) oder über die Datei *.env* mit den Umgebungsvariablen im Stammverzeichnis des Repositorys.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/Common_Utils/Authentication.cs#GetCredentialsAsync)]

Bei der interaktiven Authentifizierung wird durch die Funktion `GetCredentialsInteractiveAuthAsync` das Objekt `ServiceClientCredentials` erstellt, und zwar basierend auf einer interaktiven Authentifizierung und den Verbindungsparametern, die in der lokalen Konfigurationsdatei (*appsettings.json*) oder über die Umgebungsvariablendatei ( *.env*) im Stammverzeichnis des Repositorys angegeben sind. In diesem Fall sind AADCLIENTID und AADSECRET in der Konfigurations- oder Umgebungsvariablendatei nicht erforderlich.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/Common_Utils/Authentication.cs#GetCredentialsInteractiveAuthAsync)]

## <a name="create-an-output-asset"></a>Erstellen eines Ausgabemedienobjekts  

Das [Ausgabeobjekt](assets-concept.md) speichert das Ergebnis Ihres Codierungsauftrags.  

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#CreateOutputAsset)]

## <a name="get-or-create-an-encoding-transform"></a>Abrufen oder Erstellen einer Codierungstransformation

Beim Erstellen einer neuen [Transformations](transform-jobs-concept.md)instanz müssen Sie angeben, was als Ausgabe generiert werden soll. Der erforderliche Parameter ist ein `transformOutput`-Objekt, wie im folgenden Code gezeigt. Jedes TransformOutput-Objekt enthält eine **Voreinstellung**. Die Voreinstellung beschreibt die Schrittanleitungen von Vorgängen zur Video- und/oder Audioverarbeitung, die verwendet werden sollen, um das gewünschte TransformOutput-Objekt zu generieren. Das in diesem Artikel beschriebene Beispiel verwendet eine integrierte Voreinstellung namens **AdaptiveStreaming**. Die Voreinstellung codiert das Eingabevideo in eine automatisch generierte Bitratenkette (Paare aus Bitrate und Auflösung) auf Basis der Eingabeauflösung und -bitrate und generiert ISO MP4-Dateien mit H.264-Video und AAC-Audio entsprechend jedem Paar aus Bitrate und Auflösung. 

Vor dem Erstellen einer neuen **Transformation** sollten Sie zunächst mit der **Get**-Methode überprüfen, ob eine solche bereits vorhanden ist. Der folgende Code veranschaulicht dies.  In Media Services v3 geben **Get**-Methoden für Entitäten **null** zurück, wenn die Entität nicht vorhanden ist (eine Überprüfung des Namens ohne Unterscheidung zwischen Groß-/Kleinschreibung ).

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#EnsureTransformExists)]

## <a name="submit-job"></a>Auftrag übermitteln

Wie bereits erwähnt, ist das **Transformations** objekt die Anleitung und ein [Auftrag](transform-jobs-concept.md) die tatsächliche Anforderung an Media Services, diese **Transformation** auf ein bestimmtes Eingabevideo oder einen Audioinhalt anzuwenden. Der **Auftrag** gibt Informationen wie den Speicherort des Eingabevideos und den Speicherort für die Ausgabe an.

In diesem Tutorial wird die Auftragseingabe basierend auf einer Datei erstellt, die direkt aus einer [HTTPS-Quell-URL](job-input-from-http-how-to.md) erfasst wird.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#SubmitJob)]

## <a name="wait-for-the-job-to-complete"></a>Warten auf den Abschluss des Auftrags

Die Auftragsausführung dauert einige Zeit. Nach Abschluss des Auftrags möchten Sie eine Benachrichtigung erhalten. Das Codebeispiel unten zeigt, wie der Status des **Auftrags** vom Dienst abgerufen wird. Der Abruf gehört aufgrund potenzieller Latenzzeiten nicht zu den empfohlenen Best Practices für Produktionsanwendungen. Der Abruf kann gedrosselt werden, wenn er für ein Konto im Übermaß verwendet wird. Entwickler sollten stattdessen Event Grid verwenden. Weitere Informationen finden Sie unter [Routing von Ereignissen an einen benutzerdefinierten Webendpunkt](monitoring/job-state-events-cli-how-to.md).

Der **Auftrag** durchläuft in der Regel die folgenden Zustände: **Geplant**, **In Warteschlange**, **Wird verarbeitet**, **Abgeschlossen** (Endzustand). Wenn für den Auftrag ein Fehler aufgetreten ist, wird der Zustand **Fehler** angezeigt. Wird der Auftrag gerade abgebrochen, befindet er sich im Zustand **Vorgang wird abgebrochen**. Nach Abschluss des Vorgangs lautet der Status **Abgebrochen**.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#WaitForJobToFinish)]

## <a name="create-a-content-key-policy"></a>Erstellen einer Richtlinie für Inhaltsschlüssel

Ein Inhaltsschlüssel ermöglicht den sicheren Zugriff auf Ihre Medienobjekte. Sie müssen eine [Richtlinie für Inhaltsschlüssel](drm-content-key-policy-concept.md) erstellen, wenn Sie Ihre Inhalte per DRM verschlüsseln. Die Richtlinie konfiguriert die Übermittlung des Inhaltsschlüssels an Endclients. Der Inhaltsschlüssel ist einem Streaminglocator zugeordnet. Media Services bietet außerdem den Schlüsselbereitstellungsdienst, der Verschlüsselungsschlüssel und Lizenzen an autorisierte Benutzer übermittelt.

Sie müssen die Anforderungen (Einschränkungen) für die **Richtlinie für Inhaltsschlüssel** festlegen, die erfüllt sein müssen, damit Schlüssel mit der angegebenen Konfiguration bereitgestellt werden. In diesem Beispiel werden die folgenden Konfigurationen und Anforderungen festgelegt:

* Konfiguration

    Die [PlayReady](drm-playready-license-template-concept.md)- und [Widevine](drm-widevine-license-template-concept.md)-Lizenzen werden so konfiguriert, dass sie vom Media Services-Lizenzbereitstellungsdienst bereitgestellt werden können. Obwohl in dieser Beispiel-App die [FairPlay](drm-fairplay-license-overview.md)-Lizenz nicht konfiguriert wird, enthält sie eine Methode, die Sie zum Konfigurieren von FairPlay verwenden können. Sie können die FairPlay-Konfiguration als weitere Option hinzufügen.

* Einschränkung

    Die App legt eine Einschränkung des JWT-Tokentyps (JSON Web Token) in der Richtlinie fest.

Wenn ein Stream von einem Player angefordert wird, verwendet Media Services den angegebenen Schlüssel, um Ihren Inhalt dynamisch zu verschlüsseln. Um den Stream zu entschlüsseln, fordert der Player den Schlüssel vom Schlüsselübermittlungsdienst an. Um zu ermitteln, ob der Benutzer berechtigt ist, den Schlüssel zu erhalten, wertet der Dienst die Richtlinie für Inhaltsschlüssel aus, die Sie für den Schlüssel angegeben haben.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#GetOrCreateContentKeyPolicy)]

## <a name="create-a-streaming-locator"></a>Erstellen eines Streaminglocators

Nachdem die Codierung abgeschlossen ist und die Richtlinie für den Inhaltsschlüssel festgelegt wurde, besteht der nächste Schritt darin, das Video im Ausgabeobjekt Clients für die Wiedergabe zur Verfügung zu stellen. Sie stellen das Video in zwei Schritten zur Verfügung:

1. Erstellen eines [Streaminglocators](stream-streaming-locators-concept.md)
2. Erstellen der Streaming-URLs, die von Clients verwendet werden können

Der Vorgang des Erstellens eines **StreamingLocator** wird als „Veröffentlichen“ bezeichnet. Standardmäßig ist der **Streaminglocator** sofort nach den API-Aufrufen gültig. Wenn Sie keine optionale Start- und Endzeit eingestellt haben, dann bleibt der Streaminglocator bis zu seiner Löschung bestehen.

Beim Erstellen eines **Streaminglocators** müssen Sie den gewünschten Wert für `StreamingPolicyName` angeben. In diesem Tutorial verwenden wir eine der vordefinierten Streamingrichtlinien, die Azure Media Services mitteilt, wie der Inhalt für das Streaming veröffentlicht werden soll. In diesem Beispiel wird „StreamingLocator.StreamingPolicyName“ auf die Richtlinie „Predefined_MultiDrmCencStreaming“ festgelegt. Die PlayReady- und Widevine-Verschlüsselungen werden angewendet, und der Schlüssel wird basierend auf den konfigurierten DRM-Lizenzen an den Wiedergabeclient übermittelt. Wenn Sie den Stream auch mit CBCS (FairPlay) verschlüsseln möchten, verwenden Sie „Predefined_MultiDrmStreaming“.

> [!IMPORTANT]
> Wenn Sie eine benutzerdefinierte [Streamingrichtlinie](stream-streaming-policy-concept.md) verwenden, sollten Sie eine begrenzte Sammlung solcher Richtlinien für Ihr Media Services-Konto erstellen und diese für Ihre Streaminglocators wiederverwenden, wenn dieselben Verschlüsselungsoptionen und Protokolle benötigt werden. Ihr Media Services-Konto weist ein Kontingent für die Anzahl von StreamingPolicy-Einträgen auf. Sie sollten nicht für jeden Streaminglocator eine neue StreamingPolicy erstellen.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#CreateStreamingLocator)]

## <a name="get-a-test-token"></a>Abrufen eines Testtokens

In diesem Tutorial wird für die Richtlinie für den Inhaltsschlüssel eine Tokeneinschränkung angegeben. Eine durch Token eingeschränkte Richtlinie gilt nur zusammen mit einem Token, das von einem Sicherheitstokendienst (Security Token Service, STS) ausgestellt wurde. Media Services unterstützt Token in den [JWT](/previous-versions/azure/azure-services/gg185950(v=azure.100)#BKMK_3)-Formaten. Ein solches wird auch im Beispiel konfiguriert.

In ContentKeyPolicy wird ContentKeyIdentifierClaim verwendet, d.h., das an den Schlüsselbereitstellungsdienst übergebene Token muss den Bezeichner von ContentKey enthalten. In diesem Beispiel geben wir beim Erstellen des Streaminglocators keinen Inhaltsschlüssel an, dieser wird im System zufällig erstellt. Die ContentKeyId muss abgerufen werden, die in den Anspruch ContentKeyIdentifierClaim eingefügt werden muss, um das Testtoken zu generieren.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#GetToken)]

## <a name="build-a-streaming-url"></a>Erstellen einer Streaming-URL

Nachdem der [StreamingLocator](/rest/api/media/streaminglocators) erstellt wurde, können Sie die Streaming-URLs abrufen. Um eine URL zu erstellen, müssen Sie den [StreamingEndpoint](/rest/api/media/streamingendpoints)-Hostnamen und den **Streaminglocator**-Pfad miteinander verketten. In diesem Beispiel wird der *standardmäßige* **Streamingendpunkt** verwendet. Beim ersten Erstellen eines Media Services-Kontos weist dieser *standardmäßige* **Streamingendpunkt** den Status „Beendet“ auf. Sie müssen daher **Start** aufrufen.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#GetMPEGStreamingUrl)]

Wenn Sie die App ausführen, wird der folgende Bildschirm angezeigt:

![Schützen mit DRM](./media/drm-protect-with-drm-tutorial/playready_encrypted_url.png)

Sie können einen Browser öffnen und die resultierende URL einfügen, um die Azure Media Player-Demoseite mit der URL und dem bereits ausgefüllten Token zu starten.

## <a name="clean-up-resources-in-your-media-services-account"></a>Bereinigen von Ressourcen in Ihrem Media Services-Konto

Im Allgemeinen sollten Sie alles mit Ausnahme der Objekte bereinigen, die Sie wiederverwenden möchten. (In der Regel werden Transformationen, Streaminglocators usw. wiederverwendet.) Wenn Sie Ihr Konto nach dem Experimentieren bereinigen, sollten Sie alle Ressourcen löschen, die Sie nicht wiederverwenden möchten. Der folgende Code beispielsweise löscht den Auftrag, die erstellten Assets und die Inhaltsschlüsselrichtlinie:

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#CleanUp)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie keine Ressourcen in Ihrer Ressourcengruppe mehr benötigen, einschließlich der Media Services und Speicherkonten, die Sie für dieses Tutorial erstellt haben, löschen Sie die zuvor erstellte Ressourcengruppe.

Führen Sie den folgenden CLI-Befehl aus:

```azurecli
az group delete --name amsResourceGroup
```

## <a name="additional-notes"></a>Zusätzliche Hinweise

* Widevine ist ein von Google Inc. bereitgestellter Dienst, der den Vertragsbedingungen und der Datenschutzrichtlinie von Google, Inc. unterliegt.

## <a name="ask-questions-give-feedback-get-updates"></a>Fragen stellen, Feedback geben, Updates abrufen

Im Artikel [Azure Media Services-Community](media-services-community.md) finden Sie verschiedene Möglichkeiten, Fragen zu stellen, Feedback zu geben und Updates zu Media Services zu bekommen.

## <a name="next-steps"></a>Nächste Schritte

Auschecken

> [!div class="nextstepaction"]
> [Verwenden der dynamischen AES-128-Verschlüsselung und des Schlüsselübermittlungsdiensts](drm-playready-license-template-concept.md)
