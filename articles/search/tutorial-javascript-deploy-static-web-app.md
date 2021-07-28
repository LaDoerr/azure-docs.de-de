---
title: 'JavaScript-Tutorial: Bereitstellen der Website mit Suchunterstützung'
titleSuffix: Azure Cognitive Search
description: Hier erfahren Sie, wie Sie die Website mit Suchunterstützung als statische Azure-Web-App bereitstellen.
manager: nitinme
author: diberry
ms.author: diberry
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 03/18/2021
ms.custom: devx-track-js
ms.devlang: javascript
ms.openlocfilehash: 021f7aeda86f5ff9f11eb2991a74c6ad37a203e6
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110469781"
---
# <a name="3---deploy-the-search-enabled-website"></a>3\. Bereitstellen der Website mit Suchunterstützung

Hier erfahren Sie, wie Sie die Website mit Suchunterstützung als statische Azure-Web-App bereitstellen. Diese Bereitstellung umfasst sowohl die React-App als auch die Funktions-App.  

Die statische Web-App pullt Informationen und Dateien für die Bereitstellung unter Verwendung Ihrer Fork des Beispielrepositorys aus GitHub.  

## <a name="create-a-static-web-app-in-visual-studio-code"></a>Erstellen einer statischen Web-App in Visual Studio Code

1. Wählen Sie über die Aktivitätsleiste die Option **Azure** und anschließend über die Seitenleiste die Option **Static Web Apps** aus. 

1. Wenn in VS Code ein Popupfenster angezeigt wird, in dem Sie gefragt werden, über welche Verzweigung Sie bereitstellen möchten, wählen Sie die Standardverzweigung aus, in der Regel **master** oder **main**. 

    Diese Einstellung bedeutet, dass nur Änderungen, die Sie für diese Verzweigung committen, in Ihrer statischen Web-App bereitgestellt werden. 

1. Wenn ein Popupfenster angezeigt wird, in dem Sie aufgefordert werden, Ihre Änderungen zu committen, tun Sie dies nicht. Für die Geheimnisse aus dem Massenimportschritt darf kein Commit für das Repository ausgeführt werden. 

    Um die Änderungen zurückzusetzen, wählen Sie in VS Code in der Aktivitätsleiste das Symbol für Quellcodeverwaltung aus. Wählen Sie dann jede geänderte Datei in der Liste „Änderungen“ aus, und wählen Sie das Symbol für **Änderungen verwerfen** aus.

1. Klicken Sie mit der rechten Maustaste auf den Abonnementnamen, und wählen Sie anschließend **Create Static Web App (Advanced)** (Statische Web-App erstellen (erweitert)) aus.    

    :::image type="content" source="media/tutorial-javascript-create-load-index/visual-studio-code-create-static-web-app-resource-advanced.png" alt-text="Klicken Sie mit der rechten Maustaste auf den Abonnementnamen, und wählen Sie anschließend **Create Static Web App (Advanced)** (Statische Web-App erstellen (erweitert)) aus.":::

1. Geben Sie gemäß der Eingabeaufforderungen die folgenden Informationen an:

    |Prompt|EINGABETASTE|
    |--|--|
    |„Enter the name for the new Static Web App.“ (Geben Sie den Namen für die neue statische Web-App ein.)|Erstellen Sie einen eindeutigen Namen für Ihre Ressource. Dazu können Sie etwa dem Repositorynamen Ihren eigenen Namen voranstellen (Beispiel: `joansmith-azure-search-javascript-samples`). |
    |„Select a resource group for new resources.“ (Wählen Sie eine Ressourcengruppe für neue Ressourcen aus.)|Verwenden Sie die Ressourcengruppe, die Sie für dieses Tutorial erstellt haben.|
    |SKU auswählen| Wählen Sie die kostenlose SKU für dieses Tutorial aus.|
    |„Choose build preset to configure default project structure.“ (Wählen Sie eine Buildvoreinstellung aus, um die Standardprojektstruktur zu konfigurieren.)|Wählen Sie **Benutzerdefiniert** aus.|
    |„Select the location of your application code“ (Wählen Sie den Speicherort Ihres Anwendungscodes aus.)|`search-website`<br><br>Dies ist der Pfad vom Stamm des Repositorys zu Ihrer Azure Static-Web-App. |
    |Wählen Sie den Speicherort Ihres Azure-Funktionscodes aus.|`search-website/api`<br><br>Dies ist der Pfad vom Stamm des Repositorys zu Ihrer Azure Function-App. |
    |„Enter the path of your build output...“ (Geben Sie den Pfad Ihrer Buildausgabe ein...)|`build`<br><br>Dies ist der Pfad von Ihrer Azure Static-Web-App zu Ihren generierten Dateien.|
    |„Select a location for new resources.“ (Wählen Sie einen Standort für neue Ressourcen aus.)|Wählen Sie eine Region in Ihrer Nähe aus.|

1. Die Ressource wird erstellt. Wählen Sie in den Benachrichtigungen die Option **Open Actions in GitHub** (Aktionen in GitHub öffnen) aus. Dadurch wird ein Browserfenster mit Ihrem geforkten Repository geöffnet. 

    Der Liste mit den Aktionen können Sie entnehmen, dass Ihre App (sowohl Client als auch Funktionen) erfolgreich in Ihre statische Azure-Web-App gepusht wurde. 

    Warten Sie, bis der Bildvorgang und die Bereitstellung abgeschlossen sind, bevor Sie fortfahren. Das kann ein bis zwei Minuten dauern.

## <a name="get-cognitive-search-query-key-in-visual-studio-code"></a>Abrufen des Cognitive Search-Abfrageschlüssels in Visual Studio Code

1. Öffnen Sie in Visual Studio Code die [Aktivitätsleiste](https://code.visualstudio.com/docs/getstarted/userinterface), und wählen Sie das Azure-Symbol aus. 

1. Wählen Sie auf der Seitenleiste im Bereich **Azure: Cognitive Search** Ihr Azure-Abonnement aus. Klicken Sie dann mit der rechten Maustaste auf Ihre Suchressource, und wählen Sie **Copy Query Key** (Abfrageschlüssel kopieren) aus. 

    :::image type="content" source="./media/tutorial-javascript-create-load-index/visual-studio-code-copy-query-key.png" alt-text="Wählen Sie auf der Seitenleiste im Bereich **Azure: Cognitive Search** Ihr Azure-Abonnement aus. Klicken Sie dann mit der rechten Maustaste auf Ihre Suchressource, und wählen Sie **Copy Query Key** (Abfrageschlüssel kopieren) aus.":::

1. Speichern Sie diesen Abfrageschlüssel. Er wird im nächsten Abschnitt benötigt. Der Abfrageschlüssel kann zum Abfragen Ihres Index verwendet werden. 

## <a name="add-configuration-settings-in-azure-portal"></a>Hinzufügen von Konfigurationseinstellungen im Azure-Portal

Von der Azure-Funktions-App werden keine Suchdaten zurückgegeben, solange die Suchgeheimnisse nicht in den Einstellungen angegeben wurden. 

1. Wählen Sie auf der Aktivitätsleiste **Azure** aus. 
1. Klicken Sie mit der rechten Maustaste auf Ihre statische Web-App-Ressource, und wählen Sie dann **Im Portal öffnen** aus.

    :::image type="content" source="media/tutorial-javascript-static-web-app/open-static-web-app-in-azure-portal.png" alt-text="Rechtsklicken auf die statische JavaScript-Web-App-Ressource und Auswählen von „Im Portal öffnen“":::

1. Wählen Sie **Konfiguration** und dann **+ Hinzufügen** aus.

    :::image type="content" source="media/tutorial-javascript-static-web-app/add-new-application-setting-to-static-web-app-in-portal.png" alt-text="Auswählen von „Konfiguration“ und „Hinzufügen“ für Ihre JavaScript-App":::

1. Fügen Sie die folgenden Einstellungen hinzu:

    |Einstellung|Ihr Suchressourcenwert|
    |--|--|
    |SearchApiKey|Ihr Suchabfrageschlüssel|
    |SearchServiceName|Der Name Ihrer Suchressource|
    |SearchIndexName|`good-books`|
    |SearchFacets|`authors*,language_code`|

    Azure Cognitive Search erfordert zum Filtern von Auflistungen eine andere Syntax als für Zeichenfolgen. Fügen Sie nach einem Feldnamen `*` hinzu, um anzugeben, dass das Feld vom Typ `Collection(Edm.String)` ist. Dadurch kann die Azure-Funktion Filter ordnungsgemäß zu Abfragen hinzufügen.

1. Klicken Sie auf **Speichern**, um die Einstellungen zu speichern. 

    :::image type="content" source="media/tutorial-javascript-static-web-app/save-new-application-setting-to-static-web-app-in-portal.png" alt-text="Auswählen von „Speichern“, um die Einstellungen zu speichern":::

1. Kehren Sie zu VS Code zurück. 
1. Aktualisieren Sie Ihre statische Web-App, um die Anwendungseinstellungen der statischen Web-App anzuzeigen. 

    :::image type="content" source="media/tutorial-javascript-static-web-app/visual-studio-code-extension-fresh-resource.png" alt-text="Aktualisieren der statischen Web-App, um die Anwendungseinstellungen der statischen Web-App anzuzeigen":::

## <a name="use-search-in-your-static-web-app"></a>Verwenden der Suche in Ihrer statischen Web-App

1. Öffnen Sie in Visual Studio Code die [Aktivitätsleiste](https://code.visualstudio.com/docs/getstarted/userinterface), und wählen Sie das Azure-Symbol aus.
1. Klicken Sie auf der Seitenleiste im Bereich `Static web apps` **mit der rechten Maustaste auf Ihr Azure-Abonnement**, und suchen Sie nach der statischen Web-App, die Sie für dieses Tutorial erstellt haben.
1. Klicken Sie mit der rechten Maustaste auf den Namen Ihrer statischen Web-App, und wählen Sie **Browse site** (Website durchsuchen) aus.
    
    :::image type="content" source="media/tutorial-javascript-create-load-index/visual-studio-code-browse-static-web-app.png" alt-text="Klicken Sie mit der rechten Maustaste auf den Namen Ihrer statischen Web-App, und wählen Sie **Browse site** (Website durchsuchen) aus.":::    

1. Wählen Sie im Popupdialogfeld die Option **Öffnen** aus.
1. Geben Sie auf der Suchleiste der Website _langsam_ eine Suchabfrage wie `code` ein, damit das Vorschlagfeature Buchtitel vorschlagen kann. Wählen Sie einen Vorschlag aus, oder fahren Sie mit der Eingabe Ihrer eigenen Abfrage fort. Drücken Sie die EINGABETASTE, wenn Sie mit der Eingabe Ihrer Suchabfrage fertig sind. 
1. Sehen Sie sich die Ergebnisse an, und wählen Sie eines der Bücher aus, um weitere Details anzuzeigen. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe, um die in diesem Tutorial erstellten Ressourcen zu bereinigen.

1. Öffnen Sie in Visual Studio Code die [Aktivitätsleiste](https://code.visualstudio.com/docs/getstarted/userinterface), und wählen Sie das Azure-Symbol aus. 

1. Klicken Sie auf der Seitenleiste im Bereich `Resource Groups` **mit der rechten Maustaste auf Ihr Azure-Abonnement**, und suchen Sie nach der Ressourcengruppe, die Sie für dieses Tutorial erstellt haben.
1. Klicken Sie mit der rechten Maustaste auf den Namen der Ressourcengruppe, und wählen Sie **Löschen** aus.
    Dadurch werden sowohl die Suchressource als auch die Ressourcen der statischen Web-App gelöscht.
1. Wenn Sie den GitHub-Fork des Beispiels nicht mehr benötigen, können Sie ihn auf GitHub löschen. Navigieren Sie zu den **Einstellungen** Ihres Forks, und löschen Sie ihn. 


## <a name="next-steps"></a>Nächste Schritte

* [Machen Sie sich mit der Suchintegration für die Website mit Suchunterstützung vertraut.](tutorial-javascript-search-query-integration.md)
