---
title: Azure Media Services – Übersicht | Microsoft-Dokumentation
description: Microsoft Azure Media Services ist eine erweiterbare, cloudbasierte Plattform, die Entwicklern das Erstellen von skalierbaren Medienverwaltungslösungen und Bereitstellungsanwendungen ermöglicht. Dieser Artikel bietet eine Übersicht über Azure Media Services.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/10/2021
ms.author: inhenkel
ms.openlocfilehash: 1e394c29d1f1a707b30b8d04568ff92421e2916f
ms.sourcegitcommit: e6de87b42dc320a3a2939bf1249020e5508cba94
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/27/2021
ms.locfileid: "114712726"
---
# <a name="azure-media-services-overview"></a>Azure Media Services – Übersicht

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

[!INCLUDE [v2 deprecation notice](../latest/includes/v2-deprecation-notice.md)]

Microsoft Azure Media Services (AMS) ist eine erweiterbare, cloudbasierte Plattform, die Entwicklern das Erstellen von skalierbaren Medienverwaltungslösungen und Bereitstellungsanwendungen ermöglicht. Media Services basiert auf REST-APIs, mit denen Sie auf sichere Weise Video- oder Audioinhalte hochladen, speichern, codieren und verpacken können – sowohl für eine bedarfsgesteuerte als auch für eine auf Livestreaming basierende Bereitstellung auf verschiedenen Clients (z.B. TV, PC und mobile Geräte).

Sie können mithilfe von Media Services vollständige End-to-End-Workflows erstellen. Es ist auch möglich, Drittanbieterkomponenten für einige Elemente Ihres Workflows zu nutzen. Beispielsweise können Sie die Codierung mit einem Encoder eines Drittanbieters durchführen. Anschließend sorgen Sie mit Media Services für Upload, Schutz, Paketierung und Bereitstellung. Sie können Ihre Inhalte live streamen oder sie bei Bedarf bereitstellen. 


## <a name="compliance-privacy-and-security"></a>Compliance, Datenschutz und Sicherheit

Wichtig: Ihre Nutzung von Azure Media Services darf nicht gegen geltende Gesetze verstoßen, und weder Media Services noch ein anderer Azure-Dienst darf auf eine Weise verwendet werden, die die Rechte Dritter verletzt oder für Dritte schädlich ist.

Vor dem Upload eines Videos oder Bilds in Media Services müssen Sie über alle entsprechenden Berechtigungen zur Verwendung des Videos/Bilds verfügen, einschließlich – sofern gesetzlich erforderlich – alle notwendigen Zustimmungen von Einzelpersonen (falls vorhanden), die auf dem Video/Bild zu sehen sind, zur Nutzung, Verarbeitung und Speicherung ihrer Daten in Media Services und Azure. Je nach Rechtsprechung können besondere rechtliche Anforderungen für die Sammlung, Onlineverarbeitung und Speicherung bestimmter Datenkategorien, z. B. biometrische Daten, gelten. Stellen Sie Konformität mit allen rechtlichen Anforderungen sicher, die möglicherweise für Sie gelten, bevor Sie Media Services und Azure für die Verarbeitung und Speicherung von Daten verwenden, die bestimmten rechtlichen Anforderungen unterliegen.

Informationen zu Compliance, Datenschutz und Sicherheit in Media Services finden Sie im [Trust Center](https://www.microsoft.com/trust-center/?rtc=1) von Microsoft. Informationen zu den Datenschutzauflagen und zur Behandlung Ihrer Daten sowie Datenaufbewahrungsmethoden durch Microsoft, einschließlich Informationen zur Löschung Ihrer Daten, finden Sie in den [Datenschutzbestimmungen](https://privacy.microsoft.com/PrivacyStatement), in den [Lizenzbedingungen für Onlinedienste](https://www.microsoft.com/licensing/product-licensing/products?rtc=1) (Online Services Terms, OST) und im [Nachtrag zur Datenverarbeitung](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=67) (Data Processing Addendum, DPA). Wenn Sie Media Services nutzen, erklären Sie sich damit einverstanden, dass Sie OST, DPA und den Datenschutzbestimmungen unterliegen.
 
## <a name="prerequisites"></a>Voraussetzungen

Um mit Azure Media Services loszulegen, sollten Sie Folgendes haben:

* Ein Azure-Konto. Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Einen Monat kostenlos testen](https://azure.microsoft.com).
* Ein Azure Media Services-Konto. Weitere Informationen finden Sie unter [Erstellen von Konten](media-services-portal-create-account.md).
* (Optional) Eine eingerichtete Entwicklungsumgebung. Wählen Sie .NET oder REST-API für Ihre Entwicklungsumgebung. Weitere Informationen finden Sie unter [Einrichten der Umgebung](media-services-dotnet-how-to-use.md).

    Erfahren Sie außerdem, wie Sie programmgesteuert eine [Verbindung mit AMS-API](media-services-use-aad-auth-to-access-ams-api.md) herstellen können.
* Einen Standard- oder Premium-Streamingendpunkt im Status „Gestartet“.  Weitere Informationen finden Sie unter [Verwalten von Streamingendpunkten](media-services-portal-manage-streaming-endpoints.md).

## <a name="sdks-and-tools"></a>SDKs und Tools

Zum Entwickeln von Media Services-Lösungen können Sie folgende Komponenten verwenden:

* [Media Services-REST-API](/rest/api/media/operations/azure-media-services-rest-api-reference)
* Eines der verfügbaren Client-SDKs:
    * Azure Media Services SDK für .NET
    
        * [NuGet-Paket](https://www.nuget.org/packages/windowsazure.mediaservices/)
        * [GitHub-Quellcode](https://github.com/Azure/azure-sdk-for-media-services)
    * [Azure SDK für Java](https://github.com/Azure/azure-sdk-for-java)
    * [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php)
    * [Azure Media Services für Node.js](https://github.com/michelle-becker/node-ams-sdk/blob/master/lib/request.js) (Dies ist eine nicht von Microsoft stammende Version des Node.js SDK. Sie wird von einer Community verwaltet und bietet derzeit keine 100 %-ige Abdeckung der AMS-APIs).
* Vorhandene Tools:
    * [Azure portal](https://portal.azure.com/)
    * [Azure Media Services-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (Azure Media Services Explorer [AMSE] ist eine WinForms-/C#-Anwendung für Windows)

> [!NOTE]
> Informationen zum Abrufen der aktuellen Version des Java SDK sowie zu den ersten Schritten beim Entwickeln mit Java finden Sie unter [Erste Schritte mit dem Java-Client-SDK für Azure Media Services](./media-services-java-how-to-use.md). <br/>
> Wenn Sie das aktuelle PHP SDK für Media Services herunterladen möchten, suchen Sie im [Packagist-Repository](https://packagist.org/packages/microsoft/windowsazure#v0.5.7) nach Version 0.5.7 des Microsoft-/WindowAzure-Pakets.  

## <a name="code-samples"></a>Codebeispiele

Im Katalog mit **Azure-Codebeispielen** stehen mehrere Codebeispiele zur Verfügung: [Azure Media Services-Codebeispiele](https://azure.microsoft.com/resources/samples/?service=media-services&sort=0).

## <a name="concepts"></a>Konzepte

Azure Media Services-Konzepte finden Sie unter [Konzepte](media-services-concepts.md).

## <a name="supported-scenarios-and-availability-of-media-services-across-data-centers"></a>Unterstützte Szenarien und rechenzentrumsübergreifende Verfügbarkeit von Media Services

Weitere Informationen zu gängigen Azure-Szenarien finden Sie unter [Szenarien und datencenterübergreifende Verfügbarkeit von Media Services-Features](scenarios-and-availability.md).
Weitere Informationen zur regionalen Verfügbarkeit finden Sie unter [Regionale Verfügbarkeit von Azure Media Services](availability-regions-v-2.md).

## <a name="service-level-agreement-sla"></a>Vereinbarung zum Servicelevel (SLA)

Weitere Informationen finden Sie im [Microsoft Azure-SLA](https://azure.microsoft.com/support/legal/sla/).

## <a name="support"></a>Support

[Azure-Support](https://azure.microsoft.com/support/options/) bietet Supportoptionen für Azure, Media Services eingeschlossen.

## <a name="provide-feedback"></a>Feedback geben

[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
