---
title: 'Schnellstart: Erstellen einer HoloLens-App mit Unity'
description: In diesem Schnellstart wird beschrieben, wie Sie eine HoloLens-Unity-App erstellen, indem Sie Object Anchors verwenden.
author: craigktreasure
manager: virivera
ms.author: crtreasu
ms.date: 03/02/2021
ms.topic: quickstart
ms.service: azure-object-anchors
ms.openlocfilehash: 2c96f1392228c4fcce65e05711b70a6ce689592a
ms.sourcegitcommit: 190658142b592db528c631a672fdde4692872fd8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112006891"
---
# <a name="quickstart-create-a-hololens-app-with-azure-object-anchors-in-unity"></a>Schnellstart: Erstellen einer HoloLens-App mit Azure Object Anchors in Unity

In diesem Schnellstart erstellen Sie eine Unity HoloLens-App, die [Azure Object Anchors](../overview.md) verwendet. Azure Object Anchors ist ein verwalteter Clouddienst, der 3D-Ressourcen in KI-Modelle konvertiert, die objektbezogene Mixed Reality-Erlebnisse für HoloLens ermöglichen. Wenn Sie fertig sind, verfügen Sie über eine HoloLens-App, die mit Unity erstellt wurde und Objekte in der physischen Welt erkennen kann.

Sie lernen Folgendes:

> [!div class="checklist"]
> * Vorbereiten von Unity-Buildeinstellungen
> * Exportieren des HoloLens-Visual Studio-Projekts
> * Bereitstellen und Ausführen der App auf einem HoloLens 2-Gerät.

[!INCLUDE [Unity quickstart prerequisites](../../../includes/object-anchors-quickstart-unity-prerequisites.md)]

[!INCLUDE [Unity device setup](../../../includes/object-anchors-quickstart-unity-device-setup.md)]

## <a name="open-the-sample-project"></a>Öffnen des Beispielprojekts

[!INCLUDE [Clone Sample Repo](../../../includes/object-anchors-clone-sample-repository.md)]

[!INCLUDE [Download Unity Package](../../../includes/object-anchors-quickstart-unity-download-package.md)]

Öffnen Sie das Projekt `quickstarts/apps/unity/basic` in Unity.

[!INCLUDE [Import Unity Package](../../../includes/object-anchors-quickstart-unity-import-package.md)]

[!INCLUDE [Unity build sample scene 1](../../../includes/object-anchors-quickstart-unity-build-sample-scene-1.md)]

[!INCLUDE [Unity build sample scene 2](../../../includes/object-anchors-quickstart-unity-build-sample-scene-2.md)]

[!INCLUDE [Unity build and deploy](../../../includes/object-anchors-quickstart-unity-build-deploy.md)]

### <a name="run-the-sample-app"></a>Ausführen der Beispiel-App

Schalten Sie das Gerät ein, wählen Sie **Alle Apps** aus, suchen Sie nach der App, und starten Sie sie dann. Nach dem Unity-Begrüßungsbildschirm wird eine Meldung angezeigt, die besagt, dass Object Observer initialisiert wurde. Allerdings müssen Sie der App Ihr Modell hinzufügen.

[!INCLUDE [Unity setup Windows Device Portal](../../../includes/object-anchors-quickstart-unity-setup-device-portal.md)]

[!INCLUDE [Unity upload your model](../../../includes/object-anchors-quickstart-unity-upload-model.md)]

Die App sucht nach Objekten im aktuellen Feld der Ansicht und verfolgt sie nach der Erkennung nach. Eine Instanz wird entfernt, wenn sie sechs Meter vom Standort des Benutzers entfernt ist. Der Debugtext zeigt Details zu einer Instanz an, z. B. die ID, den aktualisierten Zeitstempel und die Oberflächenabdeckung.

[!INCLUDE [Unity troubleshooting](../../../includes/object-anchors-quickstart-unity-troubleshooting.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Unity HoloLens mit MRTK](get-started-unity-hololens-mrtk.md)

> [!div class="nextstepaction"]
> [Konzepte: SDK-Übersicht](../concepts/sdk-overview.md)

> [!div class="nextstepaction"]
> [Häufig gestellte Fragen](../faq.md)

> [!div class="nextstepaction"]
> [Konvertierungs-SDK](/dotnet/api/overview/azure/mixedreality.objectanchors.conversion-readme-pre)
