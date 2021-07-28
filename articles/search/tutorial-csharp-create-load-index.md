---
title: '.NET-Tutorial: Hinzufügen von Suchfunktionen zu Web-Apps'
titleSuffix: Azure Cognitive Search
description: Erstellen Sie einen Index, und importieren Sie CSV-Daten mit .NET in den Suchindex.
manager: nitinme
author: diberry
ms.author: diberry
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 05/21/2021
ms.custom: devx-track-csharp
ms.devlang: dotnet
ms.openlocfilehash: 7822f86b52bf3b8fce9ee144797d8a9b83d90a9b
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110469850"
---
# <a name="2---create-and-load-search-index-with-net"></a>2: Erstellen und Laden des Suchindexes mit .NET

Setzen Sie die Erstellung der Website mit Suchunterstützung fort, indem Sie folgende Schritte ausführen:
* Erstellen einer Suchressource mit der VS Code-Erweiterung
* Erstellen eines neuen Indexes und Importieren von Daten mit .NET mithilfe des Beispielskripts und des Azure SDK [Azure.Search.Documents](https://www.nuget.org/packages/Azure.Search.Documents/)

## <a name="create-an-azure-search-resource"></a>Erstellen einer Azure Search-Ressource 

Erstellen Sie mit der [Azure Cognitive Search](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurecognitivesearch)-Erweiterung für Visual Studio Code eine neue Suchressource.

1. Öffnen Sie in Visual Studio Code die [Aktivitätsleiste](https://code.visualstudio.com/docs/getstarted/userinterface), und wählen Sie das Azure-Symbol aus. 

1. Klicken Sie auf der Seitenleiste im Bereich `Azure: Cognitive Search` **mit der rechten Maustaste auf Ihr Azure-Abonnement**, und wählen Sie **Create new search service** (Neuen Suchdienst erstellen) aus.

    :::image type="content" source="./media/tutorial-javascript-create-load-index/visual-studio-code-create-search-resource.png" alt-text="Rechtsklicken auf das Azure-Abonnement auf der Seitenleiste im Bereich **Azure: Cognitive Search** und Auswählen von **Create new search service** (Neuen Suchdienst erstellen)":::

1. Geben Sie gemäß der Eingabeaufforderungen die folgenden Informationen an:

    |Prompt|EINGABETASTE|
    |--|--|
    |Geben Sie einen global eindeutigen Namen für den neuen Suchdienst ein.|**Notieren Sie sich diesen Namen.** Dieser Ressourcenname wird Teil des Ressourcenendpunkts.|
    |Auswählen einer Ressourcengruppe für neue Ressourcen|Verwenden Sie die Ressourcengruppe, die Sie für dieses Tutorial erstellt haben.|
    |Wählen Sie die SKU für Ihren Suchdienst aus.|Wählen Sie in diesem Tutorial die Option **Free** aus. Sie können einen SKU-Tarif nicht mehr ändern, nachdem der Dienst erstellt wurde.|
    |Wählen Sie einen Standort für neue Ressourcen aus.|Wählen Sie eine Region in Ihrer Nähe aus.|

1. Nach Ihrer Eingabe wird die neue Suchressource erstellt. 

## <a name="get-your-search-resource-admin-key"></a>Abrufen des Administratorschlüssels für die Suchressource

Rufen Sie den Administratorschlüssel für die Suchressource mit der Visual Studio Code-Erweiterung ab. 

1. Klicken Sie in Visual Studio Code auf der Seitenleiste mit der rechten Maustaste auf die Suchressource, und wählen Sie **Copy Admin Key** (Administratorschlüssel kopieren) aus.

    :::image type="content" source="./media/tutorial-javascript-create-load-index/visual-studio-code-copy-admin-key.png" alt-text="Rechtsklicken auf die Suchressource auf der Seitenleiste und Auswählen von **Copy Admin Key** (Administratorschlüssel kopieren)":::

1. Notieren Sie sich diesen Administratorschlüssel. Sie benötigen ihn in [einem späteren Abschnitt](#prepare-the-bulk-import-script-for-search). 

## <a name="prepare-the-bulk-import-script-for-search"></a>Vorbereiten des Massenimportskripts für die Suche

Das Skript verwendet das Azure SDK für Cognitive Search:

* [NuGet-Paket „Azure.Search.Documents“](https://www.nuget.org/packages/Azure.Search.Documents/)
* [Referenzdokumentation](/dotnet/api/overview/azure/search)

1. Öffnen Sie in Visual Studio Code die Datei `Program.cs` im Unterverzeichnis `search-website/bulk-insert`, und ersetzen Sie die folgenden Variablen durch Ihre eigenen Werte, um sich mit dem Azure Search SDK zu authentifizieren:

    * YOUR-SEARCH-RESOURCE-NAME
    * YOUR-SEARCH-ADMIN-KEY

    :::code language="csharp" source="~/azure-search-dotnet-samples/search-website/bulk-insert/Program.cs" highlight="16-19" :::

1. Öffnen Sie in Visual Studio Code ein integriertes Terminal für das Unterverzeichnis `search-website/bulk-insert` des Projektverzeichnisses, und führen Sie den folgenden Befehl aus, um die Abhängigkeiten zu installieren: 

    ```bash
    dotnet restore
    ```

## <a name="run-the-bulk-import-script-for-search"></a>Ausführen des Massenimportskripts für die Suche

1. Verwenden Sie weiterhin das integrierte Terminal in Visual Studio für das Unterverzeichnis `search-website/bulk-insert` des Projektverzeichnisses, um den folgenden Bash-Befehl zum Ausführen des Skripts `Program.cs` auszuführen:

    ```bash
    dotnet run
    ```

1. Während der Codeausführung wird in der Konsole der Status angezeigt. 
1. Nach Abschluss des Uploads lautet die letzte in der Konsole ausgegebene Meldung „Finished bulk inserting book data“ (Masseneinfügen von Buchdaten abgeschlossen).

## <a name="review-the-new-search-index"></a>Überprüfen des neuen Suchindexes

Nach Abschluss des Uploads kann der Suchindex verwendet werden. Überprüfen Sie den neuen Suchindex.

1. Öffnen Sie in Visual Studio Code die Azure Cognitive Search-Erweiterung, und wählen Sie Ihre Suchressource aus.  

    :::image type="content" source="media/tutorial-javascript-create-load-index/visual-studio-code-search-extension-view-resource.png" alt-text="Öffnen der Azure Cognitive Search-Erweiterung in Visual Studio Code und Auswählen der Suchressource":::

1. Erweitern Sie „Indizes“ > „Dokumente“ > `good-books`, und wählen Sie dann ein Dokument aus, um alle dokumentspezifischen Daten anzuzeigen.
 
    :::image type="content" source="media/tutorial-javascript-create-load-index/visual-studio-code-search-extension-view-docs.png" lightbox="media/tutorial-javascript-create-load-index/visual-studio-code-search-extension-view-docs.png" alt-text="Erweitern von „Indizes“ und „good-books“ und Auswählen eines Dokuments":::

## <a name="copy-your-search-resource-name"></a>Kopieren des Namens Ihrer Suchressource

Notieren Sie sich den **Namen der Suchressource**. Sie benötigen ihn, um die Azure Functions-App mit Ihrer Suchressource zu verbinden. 

> [!CAUTION]
> Möglicherweise möchten Sie den Administratorschlüssel für die Suche in der Azure Functions-Instanz verwenden. Dadurch wird jedoch nicht das Prinzip der geringsten Berechtigungen befolgt. Die Azure Functions-Instanz verwendet den Abfrageschlüssel, um das Prinzip der geringsten Berechtigungen einzuhalten. 

## <a name="rollback-bulk-import-file-changes"></a>Rollback von Massenimport-Dateiänderungen

Verwenden Sie den folgenden git-Befehl im integrierten Visual Studio Code-Terminal im Verzeichnis `bulk-insert`, um ein Rollback der Änderungen auszuführen. Sie werden nicht benötigt, um das Tutorial fortzusetzen, und Sie sollten diese Geheimnisse nicht speichern oder in Ihr Repository pushen. 

```git
git checkout .
```

## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen Ihrer statischen Web-App](tutorial-csharp-deploy-static-web-app.md)