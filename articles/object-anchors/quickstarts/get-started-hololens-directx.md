---
title: 'Schnellstart: Erstellen einer HoloLens-App mit DirectX'
description: In dieser Schnellstartanleitung wird beschrieben, wie Sie eine HoloLens-App erstellen, indem Sie Object Anchors verwenden.
author: craigktreasure
manager: virivera
ms.author: crtreasu
ms.date: 02/02/2021
ms.topic: quickstart
ms.service: azure-object-anchors
ms.openlocfilehash: 05c131abf23ac060db24b19b6affbeb5e632468b
ms.sourcegitcommit: 190658142b592db528c631a672fdde4692872fd8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112004761"
---
# <a name="quickstart-create-a-hololens-app-with-azure-object-anchors-in-cwinrt-and-directx"></a>Schnellstart: Erstellen einer HoloLens-App mit Azure Object Anchors in C++/WinRT und DirectX

In dieser Schnellstartanleitung wird beschrieben, wie Sie mit [Azure Object Anchors](../overview.md) eine HoloLens-App in C++/WinRT und DirectX erstellen. Azure Object Anchors ist ein verwalteter Clouddienst, der 3D-Ressourcen in KI-Modelle konvertiert, die objektbezogene Mixed Reality-Erlebnisse für HoloLens ermöglichen. Wenn Sie fertig sind, verfügen Sie über eine HoloLens-App, die ein Objekt und seine Position in einer Holographic DirectX 11-Anwendung (Universal Windows) erkennen kann.

Sie lernen Folgendes:

> [!div class="checklist"]
> * Erstellen und Querladen einer HoloLens-Anwendung
> * Erkennen eines Objekts und Visualisieren seines Modells

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie für diese Schnellstartanleitung sicher, dass Sie über Folgendes verfügen:

* Ein physisches Objekt in Ihrer Umgebung und das zugehörige 3D-Modell (CAD oder gescannt)
* Einen Windows-Computer, auf dem folgende Komponenten installiert sind:
  * <a href="https://git-scm.com" target="_blank">Git für Windows</a>
  * <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2019</a> mit der Workload **Entwicklung für die universelle Windows-Plattform** und der Komponente **Windows 10 SDK (10.0.18362.0 oder höher)**
* Ein HoloLens 2-Gerät, das aktuell und für das der [Entwicklermodus](/windows/mixed-reality/using-visual-studio#enabling-developer-mode) aktiviert ist.
  * Öffnen Sie zum Aktualisieren auf das neueste HoloLens-Release die App **Einstellungen**, navigieren Sie zu **Update und Sicherheit**, und wählen Sie dann **Nach Updates suchen** aus.

## <a name="open-the-sample-project"></a>Öffnen des Beispielprojekts

[!INCLUDE [Clone Sample Repo](../../../includes/object-anchors-clone-sample-repository.md)]

Öffnen Sie `quickstarts/apps/directx/DirectXAoaSampleApp.sln` in Visual Studio.

Ändern Sie **Projektmappenkonfiguration** in **Release** und **Solution Platform** (Projektmappenplattform) in **ARM64**, und wählen Sie in den Optionen für das Bereitstellungsziel die Option **Gerät** aus. Erstellen Sie dann das Projekt **AoaSampleApp**, indem Sie mit der rechten Maustaste auf das Projekt klicken und dann **Erstellen** auswählen.

:::image type="content" source="./media/vs-deploy-to-device.png" alt-text="Konfigurieren des Visual Studio-Projekts für die Bereitstellung":::

## <a name="deploy-the-app-to-hololens"></a>Bereitstellen der App für HoloLens

Nachdem Sie das Beispielprojekt erfolgreich kompiliert haben, können Sie die App für HoloLens bereitstellen.

Schalten Sie das HoloLens-Gerät ein, melden Sie sich an, und stellen Sie dafür per USB-Kabel eine Verbindung mit dem PC her. Stellen Sie sicher, dass **Gerät** das ausgewählte Bereitstellungsziel ist (siehe oben).

Klicken Sie mit der rechten Maustaste auf das Projekt **AoaSampleApp**, und klicken Sie dann im Popupmenü auf **Bereitstellen**, um die App zu installieren. Wenn im **Ausgabefenster** von Visual Studio kein Fehler angezeigt wird, wird die App auf dem HoloLens-Gerät installiert.

:::image type="content" source="./media/vs-deploy-app.png" alt-text="Bereitstellen der App für HoloLens":::

Vor dem Starten der App müssen Sie ein Objektmodell hochladen. Befolgen Sie die Anweisungen im Abschnitt **Erfassen des Objektmodells und Erkennen seiner Instanz** weiter unten.

Um die App zu starten und zu debuggen, wählen Sie **Debuggen > Debuggen starten** aus. Wählen Sie zum Beenden der App entweder **Debuggen beenden** aus, oder drücken Sie **UMSCHALT+F5**.

## <a name="ingest-object-model-and-detect-its-instance"></a>Erfassen des Objektmodells und Erkennen seiner Instanz

Sie müssen ein Objektmodell erstellen, um die Beispiel-App ausführen zu können. Angenommen, Sie haben bereits entweder ein CAD- oder ein gescanntes 3D-Gittermodell eines Objekts in Ihrem Raum. Informationen zum Erstellen eines Modells finden Sie unter [Schnellstart: Erfassen eines 3D-Modells](./get-started-model-conversion.md).

Laden Sie dieses Modell (**chair.ou** in unserem Fall) auf Ihren Computer herunter. Wählen Sie dann im HoloLens-Geräteportal **System > Datei-Explorer > LocalAppData > AoaSampleApp > LocalState** und **Durchsuchen...** aus. Wählen Sie dann Ihre Modelldatei (z. B. **chair.ou**) und **Hochladen** aus. Die Modelldatei sollte anschließend im lokalen Cache angezeigt werden.

:::image type="content" source="../../../includes/media/object-anchors-quickstarts/portal-upload-model.png" alt-text="Portalupload des Modells":::

Starten Sie die App **AoaSampleApp** in HoloLens (wenn sie bereits geöffnet war, schließen Sie sie, und öffnen Sie sie dann erneut). Treten Sie nahe (innerhalb von 2 Metern Entfernung) an das Zielobjekt (Stuhl) heran, und scannen Sie es, indem Sie es aus mehreren Perspektiven betrachten. Sie sollten einen rosafarbenen Begrenzungsrahmen um das Objekt mit einigen gelben Punkten in der Nähe der Oberfläche des Objekts sehen, wodurch angezeigt wird, dass es erkannt wurde.

:::image type="content" source="./media/chair-detection.png" alt-text="Stuhlerkennung":::

Abbildung: Ein erkannter Stuhl, gerendert mit seinem Begrenzungsrahmen (rosa), der Punktwolke (gelb) und einem Suchbereich (großer gelber Bereich).

Sie können in der App einen Suchraum für das Objekt definieren, indem Sie mit der rechten oder linken Hand in der Luft mit dem Finger klicken. Der Suchraum wechselt zwischen einer Kugel mit einem Radius von 2 Metern, einem 4 m^3 großen Begrenzungsrahmen und einer Frustum-Ansicht. Bei größeren Objekten (z. B. Autos) ist es in der Regel am besten, die Auswahl des Sichtkegels (Frustum) zu verwenden, während Sie in einer Ecke des Objekts in etwa 2 Meter Entfernung stehen.
Jedes Mal, wenn sich der Suchbereich ändert, entfernt die App aktuell nachverfolgte Instanzen und versucht dann, sie im neuen Suchbereich erneut zu finden.

Diese App kann mehrere Objekte gleichzeitig nachverfolgen. Laden Sie zu diesem Zweck mehrere Modelle in den Ordner **LocalState** hoch, und legen Sie einen Suchbereich fest, der alle Zielobjekte abdeckt. Es kann länger dauern, bis mehrere Objekte erkannt und nachverfolgt werden.

Die App richtet ein 3D-Modell genau an seinem physischen Pendant aus. Ein Benutzer kann mit der linken Hand in der Luft tippen, um den hochpräzisen Nachverfolgungsmodus zu aktivieren, der eine genauere Position berechnet. Dies ist noch eine experimentelle Funktion, die mehr Systemressourcen verbraucht und zu einem höheren Jitter in der geschätzten Position führen kann. Tippen Sie erneut mit der linken Hand in der Luft, um zurück in den normalen Nachverfolgungsmodus zu wechseln.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Schnellstart: Erfassen eines 3D-Modells](./get-started-model-conversion.md)

> [!div class="nextstepaction"]
> [Konzepte: SDK-Übersicht](../concepts/sdk-overview.md)

> [!div class="nextstepaction"]
> [Häufig gestellte Fragen](../faq.md)

> [!div class="nextstepaction"]
> [SDK: Konvertierung](/dotnet/api/overview/azure/mixedreality.objectanchors.conversion-readme-pre)
