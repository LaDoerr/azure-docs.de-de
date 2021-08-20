---
title: Anpassen der Azure IoT Central-Benutzeroberfläche | Microsoft-Dokumentation
description: Informationen zum Anpassen des Designs und der Hilfelinks für Ihre Azure IoT Central-Anwendung
author: dominicbetts
ms.author: dobett
ms.date: 12/06/2019
ms.topic: how-to
ms.service: iot-central
services: iot-central
ms.openlocfilehash: 02c3dde025ccd8bb02b3ba5535a21cacfa8e6dbe
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122339355"
---
# <a name="customize-the-azure-iot-central-ui"></a>Anpassen der Azure IoT Central-Benutzeroberfläche

In diesem Artikel wird beschrieben, wie Sie die Benutzeroberfläche Ihrer Anwendung anpassen können, indem Sie benutzerdefinierte Designs anwenden und die Hilfelinks so ändern, dass sie auf die eigenen benutzerdefinierten Hilferessourcen verweisen. 

Der folgende Screenshot zeigt eine Seite mit dem Standarddesign:

![IoT Central-Standarddesign](./media/howto-customize-ui/standard-ui.png)

Der folgende Screenshot zeigt eine Seite mit einem benutzerdefinierten Screenshot, in dem die benutzerdefinierten Benutzeroberflächenelemente hervorgehoben sind:

![Benutzerdefiniertes IoT Central-Design](./media/howto-customize-ui/themed-ui.png)

## <a name="create-theme"></a>Erstellen eines Designs

Wenn Sie ein benutzerdefiniertes Design erstellen möchten, navigieren Sie im Abschnitt **Verwaltung** zur Seite **Ihre Anwendung anpassen**:

![IoT Central-Designs](./media/howto-customize-ui/themes.png)

Auf dieser Seite können Sie die folgenden Aspekte Ihrer Anwendung anpassen:

### <a name="application-logo"></a>Anwendungslogo

Ein PNG-Bild von maximal 1 MB Größe mit einem transparenten Hintergrund. Dieses Logo wird links auf der Titelleiste für die IoT Central-Anwendung angezeigt.

Wenn Ihr Logobild den Namen Ihrer Anwendung enthält, können Sie den Text des Anwendungsnamens ausblenden. Weitere Informationen finden Sie unter [Verwalten Ihrer Anwendung](howto-administer.md#change-application-name-and-url).

### <a name="browser-icon-favicon"></a>Browsersymbol (Favicon)

Ein PNG-Bild von maximal 32x32 Pixel Größe mit einem transparenten Hintergrund. Ein Webbrowser kann dieses Bild in der Adresszeile, im Verlauf, in Lesezeichen und auf der Registerkarte „Browser“ verwenden.

### <a name="browser-colors"></a>Browserfarben

Sie können die Farbe des Seitenkopfs sowie die Farbe ändern, die für Akzentschaltflächen und andere Hervorhebungen verwendet wird. Verwenden Sie einen hexadezimalen Farbwert von sechs Zeichen im Format `##ff6347`. Weitere Informationen zur Farbschreibweise **HEX-Wert** finden Sie unter [HTML-Farben](https://www.w3schools.com/html/html_colors.asp).

> [!NOTE]
> Über die Seite **Ihre Anwendung anpassen** können Sie jederzeit zu den Standardoptionen zurückkehren.

### <a name="changes-for-operators"></a>Änderungen für Anwendungsoperatoren

Wenn ein Administrator ein benutzerdefiniertes Design erstellt, können Anwendungsoperatoren und andere Benutzer Ihrer Anwendung ein Design in **Einstellungen** nicht mehr auswählen.

## <a name="replace-help-links"></a>Ersetzen von Hilfelinks

Wenn Sie Ihren Anwendungsoperatoren und anderen Benutzern benutzerdefinierte Hilfeinformationen bereitstellen möchten, können Sie die Links im Menü **Hilfe** der Anwendung ändern.

Zum Ändern der Hilfelinks navigieren Sie im Abschnitt **Verwaltung** zur Seite **Hilfe anpassen**:

![Anpassen von IoT Central-Hilfelinks](./media/howto-customize-ui/help-links.png)

Sie können auch neue Einträge zum Hilfemenü hinzufügen und Standardeinträge entfernen:

![Benutzerdefinierte IoT Central-Hilfe](./media/howto-customize-ui/custom-help.png)

> [!NOTE]
> Über die Seite **Hilfe anpassen** können Sie jederzeit zu den Standard-Hilfelinks zurückkehren.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie erfahren haben, wie Sie die Benutzeroberfläche in Ihrer IoT Central-Anwendung anpassen können, sind hier einige empfohlene nächste Schritte:

- [Verwalten Ihrer Anwendung](./howto-administer.md)
- [Hinzufügen von Kacheln zu Ihrem Dashboard](howto-manage-dashboards.md)