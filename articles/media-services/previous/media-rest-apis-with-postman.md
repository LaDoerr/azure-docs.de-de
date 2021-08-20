---
title: Konfigurieren von Postman für Azure Media Services-REST-API-Aufrufe
description: In diesem Artikel wird beschrieben, wie Sie Postman für Media Services-REST-API-Aufrufe konfigurieren.
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
ms.openlocfilehash: 8dd6d25d1535bdc09ad7559c059424fcb531757f
ms.sourcegitcommit: e6de87b42dc320a3a2939bf1249020e5508cba94
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/27/2021
ms.locfileid: "114706600"
---
# <a name="configure-postman-for-media-services-v2-rest-api-calls"></a>Konfigurieren von Postman für Media Services v2 REST-API-Aufrufe

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

[!INCLUDE [v2 deprecation notice](../latest/includes/v2-deprecation-notice.md)]

Dieses Tutorial zeigt Ihnen, wie Sie **Postman** so konfigurieren, dass es für den Aufruf von Azure Media Services-REST-APIs verwendet werden kann. Das Tutorial zeigt, wie Umgebungs- und Sammlungsdateien in **Postman** importiert werden. Die Sammlung enthält gruppierte Definitionen von HTTP-Anforderungen, die Azure Media Services-REST-APIs aufrufen. Die Umgebungsdatei enthält Variablen, die von der Sammlung verwendet werden.

Diese Umgebung und Sammlung werden in Artikeln verwendet, die zeigen, wie verschiedene Aufgaben mit den Azure Media Services-REST-APIs durchgeführt werden können.

## <a name="prerequisites"></a>Voraussetzungen

- Installieren Sie den REST-Client von [Postman](https://www.getpostman.com/), um die REST-APIs auszuführen, die in einigen der AMS REST-Tutorials gezeigt werden. 

    Wir verwenden **Postman**, aber jedes REST-Tool wäre geeignet. Andere Alternativen sind: **Visual Studio Code** mit dem REST-Plug-In oder **Telerik Fiddler**. 

## <a name="configure-the-environment"></a>Konfigurieren der Umgebung 

1. Erstellen Sie eine JSON-Datei, die die Umgebungsvariablen enthält, die in AMS-Tutorials verwendet werden. Benennen Sie die Datei (z.B. **AzureMediaServices.postman_environment.json**). Öffnen Sie die Datei, und fügen Sie den Code ein, der die Postman-Umgebung über [diese Codeliste](postman-environment.md) definiert. 
2. Öffnen Sie **Postman**.
3. Wählen Sie auf der rechten Seite des Bildschirms die Option **Manage environment** (Umgebung verwalten) aus.

    ![Screenshot der ausgewählten Option „Umgebung verwalten“.](./media/media-services-rest-upload-files/postman-create-env.png)
4. Klicken Sie im Dialogfeld **Manage environment** (Umgebung verwalten) auf **Import**.
5. Navigieren Sie zur Datei **AzureMediaServices.postman_environment.json** und wählen Sie sie aus.
6. Die **AzureMedia**-Umgebung wird hinzugefügt.
7. Schließen Sie das Dialogfeld.
8. Wählen Sie die **AzureMedia**-Umgebung aus.

    ![Screenshot der ausgewählten AzureMedia-Umgebung.](./media/media-services-rest-upload-files/postman-choose-env.png)

## <a name="configure-the-collection"></a>Konfigurieren der Sammlung

1. Erstellen Sie eine JSON-Datei, die die Sammlung **Postman** mit allen Vorgängen enthält, die zum Hochladen einer Datei in Media Services erforderlich sind. Benennen Sie die Datei (z.B. **AzureMediaServicesOperations.postman_collection.json**). Öffnen Sie die Datei, und fügen Sie den Code ein, der die **Postman**-Sammlung über [diese Codeliste](postman-collection.md) definiert.
2. Klicken Sie auf **Import**, um die Sammlungsdatei zu importieren.
3. Wählen Sie die Datei **AzureMediaServicesOperations.postman_collection.json** aus.

    ![Screenshot des Dialogfelds „Importieren“ mit ausgewählter Option „Dateien wählen“.](./media/media-services-rest-upload-files/postman-import-collection.png)

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den Artikel [Hochladen von Medienobjekten](media-services-rest-upload-files.md) an.  
