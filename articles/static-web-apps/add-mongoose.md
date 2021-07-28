---
title: 'Tutorial: Zugreifen auf Daten in Cosmos DB per Mongoose mit Azure Static Web Apps'
description: Es wird beschrieben, wie Sie in Cosmos DB auf Daten zugreifen, indem Sie Mongoose über eine Azure Static Web Apps-API-Funktion nutzen.
author: GeekTrainer
ms.author: chrhar
ms.service: static-web-apps
ms.topic: tutorial
ms.date: 01/25/2021
ms.openlocfilehash: 63bbc978f4927dc6922d92e1f339282f74e4fb4e
ms.sourcegitcommit: 0ce834cd348bb8b28a5f7f612c2807084cde8e8f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2021
ms.locfileid: "109814296"
---
# <a name="tutorial-access-data-in-cosmos-db-using-mongoose-with-azure-static-web-apps"></a>Tutorial: Zugreifen auf Daten in Cosmos DB per Mongoose mit Azure Static Web Apps

[Mongoose](https://mongoosejs.com/) ist der gängigste ODM-Client (Object Document Mapping) für Node.js. Dieser Client ermöglicht Ihnen das Entwerfen einer Datenstruktur und das Erzwingen einer Überprüfung. Mit Mongoose werden alle erforderlichen Tools für die Interaktion mit Datenbanken bereitgestellt, die die Mongoose-API unterstützen. [Cosmos DB](../cosmos-db/mongodb-introduction.md) unterstützt die benötigten Mongoose-APIs und ist als Back-End-Serveroption in Azure verfügbar.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> - Erstellen eines serverlosen Cosmos DB-Kontos
> - Erstellen einer Azure Static Web Apps-Instanz
> - Aktualisieren der Anwendungseinstellungen zum Speichern der Verbindungszeichenfolge

Falls Sie noch nicht über ein Azure-Abonnement verfügen, können Sie ein [kostenloses Testkonto](https://azure.microsoft.com/free/) erstellen.

## <a name="prerequisites"></a>Voraussetzungen

- Ein [Azure-Konto](https://azure.microsoft.com/free/)
- Ein [GitHub-Konto](https://github.com/join)

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

## <a name="create-a-cosmos-db-serverless-database"></a>Erstellen einer serverlosen Cosmos DB-Datenbank

Erstellen Sie als Erstes ein Konto vom Typ [Cosmos DB serverlos](../cosmos-db/serverless.md). Bei Verwendung eines serverlosen Kontos zahlen Sie nur für die Ressourcen, wenn diese auch genutzt werden. Zudem vermeiden Sie die Erstellung einer vollständigen Infrastruktur.

1. Navigieren Sie zu [https://portal.azure.com](https://portal.azure.com).
2. Klicken Sie auf **Ressource erstellen**.
3. Geben Sie im Suchfeld den Suchbegriff **Azure Cosmos DB** ein.
4. Klicken Sie auf **Azure Cosmos DB**.
5. Klicken Sie auf **Erstellen**
6. Konfigurieren Sie Ihr Azure Cosmos DB-Konto mit den folgenden Informationen:
    - Abonnement: Wählen Sie das gewünschte Abonnement aus.
    - Ressource: Klicken Sie auf **Neu erstellen**, und legen Sie den Namen auf **aswa-mongoose** fest.
    - Kontoname: Der Name muss ein eindeutiger Wert sein.
    - API: **Azure Cosmos DB für MongoDB-API**
    - Notebooks (Vorschau): **Aus**
    - Standort: **USA, Westen 2**
    - Kapazitätsmodus: **Serverlos (Vorschau)**
    - Version: **3.6**
    - Verfügbarkeitszonen: **Deaktivieren**
:::image type="content" source="media/add-mongoose/cosmos-db.png" alt-text="Erstellen einer neuen Cosmos DB-Instanz":::
7. Klicken Sie auf **Überprüfen und erstellen**.
8. Klicken Sie auf **Erstellen**

Der Erstellungsvorgang dauert einige Minuten. In einem späteren Schritt kehren Sie noch einmal zur Datenbank zurück, um die Verbindungszeichenfolge zu erfassen.

## <a name="create-a-static-web-app"></a>Erstellen einer statischen Web-App

In diesem Tutorial wird als Hilfestellung für die Erstellung Ihrer Anwendung ein Repository mit GitHub-Vorlagen verwendet.

1. Navigieren Sie zur [Starter-Vorlage](https://github.com/login?return_to=/staticwebdev/mongoose-starter/generate).
2. Wählen Sie den **Besitzer** aus (falls Sie nicht die Organisation Ihres Hauptkontos verwenden).
3. Geben Sie Ihrem Repository den Namen **aswa-mongoose-tutorial**.
4. Klicken Sie auf **Create repository from template** (Repository aus Vorlage erstellen).
5. Wechseln Sie zurück zum [Azure-Portal](https://portal.azure.com).
6. Klicken Sie auf **Ressource erstellen**.
7. Geben Sie im Suchfeld den Suchbegriff **static web apps** ein.
8. Wählen Sie **Static Web Apps** aus.
9. Klicken Sie auf **Erstellen**
10. Konfigurieren Sie Ihre Azure Static Web App-Instanz mit den folgenden Informationen:
    - Abonnement: Wählen Sie dasselbe Abonnement wie oben aus.
    - Ressourcengruppe: Wählen Sie **aswa-mongoose** aus.
    - Name: **aswa-mongoose-tutorial**
    - Region: **USA, Westen 2**
    - Klicken Sie auf **Mit GitHub anmelden**.
    - Klicken Sie in der angezeigten Aufforderung auf **Autorisieren**, um für Azure Static Web Apps die Erstellung der GitHub-Aktion für die Bereitstellung zuzulassen.
    - Organisation: Ihr Kontoname
    - Repository: **aswa-mongoose-tutorial**
    - Branch: **main**
    - Buildvoreinstellungen: Wählen Sie **Benutzerdefiniert** aus.
    - App-Speicherort: **/public**
    - API-Speicherort: **api**
    - Speicherort für App-Artefakte: *Leer lassen*
    :::image type="content" source="media/add-mongoose/azure-static-web-apps.png" alt-text="Ausgefülltes Azure Static Web Apps-Formular":::
11. Klicken Sie auf **Überprüfen und erstellen**.
12. Klicken Sie auf **Erstellen**

Der Erstellungsvorgang dauert einige Minuten. Sie können auf **Zu Ressource wechseln** klicken, nachdem die App bereitgestellt wurde.

## <a name="configure-database-connection-string"></a>Konfigurieren der Datenbankverbindungszeichenfolge

Damit die Web-App mit der Datenbank kommunizieren kann, wird die Datenbankverbindungszeichenfolge als Anwendungseinstellung gespeichert. Sie können das Objekt `process.env` nutzen, um in Node.js auf die Einstellungswerte zuzugreifen.

1. Klicken Sie oben links im Azure-Portal auf **Startseite** (oder navigieren Sie zurück zu [https://portal.azure.com](https://portal.azure.com)).
2. Klicken Sie auf **Ressourcengruppen**.
3. Klicken Sie auf **aswa-mongoose**.
4. Klicken Sie auf den Namen Ihres Datenbankkontos. Der Typ des Kontos lautet **Azure Cosmos DB-Konto**.
5. Klicken Sie unter **Einstellungen** auf **Verbindungszeichenfolge**.
6. Kopieren Sie die Verbindungszeichenfolge, die unter **PRIMÄRE VERBINDUNGSZEICHENFOLGE** angegeben ist.
7. Klicken Sie in der Brotkrümelnavigation auf **aswa-mongoose**.
8. Klicken Sie auf **aswa-mongoose-tutorial**, um zur Websiteinstanz zurückzukehren.
9. Klicken Sie unter **Einstellungen** auf **Konfiguration**.
10. Klicken Sie auf **Hinzufügen,** , und erstellen Sie eine neue Anwendungseinstellung mit den folgenden Werten:
    - Name: **CONNECTION_STRING**
    - Wert: Fügen Sie die weiter oben kopierte Verbindungszeichenfolge ein.
11. Klicken Sie auf **OK**
12. Klicken Sie unten auf der Seite auf **Speichern**.

## <a name="navigate-to-your-site"></a>Navigieren zu Ihrer Website

Sie können die statische Web-App jetzt erkunden.

1. Klicken Sie auf **Übersicht**.
1. Klicken Sie auf die URL, die oben rechts angezeigt wird.
    1. Sie hat in etwa das folgende Format: `https://calm-pond-05fcdb.azurestaticapps.net`.
1. Erstellen Sie eine neue Aufgabe, indem Sie einen Titel eingeben und auf **Aufgabe hinzufügen** klicken.
1. Vergewissern Sie sich, dass die Aufgabe angezeigt wird (dies kann einen Moment dauern).
1. Markieren Sie die Aufgabe als abgeschlossen, indem Sie das **Kontrollkästchen aktivieren**.
1. **Aktualisieren Sie die Seite**, um sich zu vergewissern, dass eine Datenbank verwendet wird.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie diese Anwendung nicht mehr benötigen, löschen Sie die Ressourcengruppe wie folgt:

1. Wechseln Sie zurück zum [Azure-Portal](https://portal.azure.com).
2. Klicken Sie auf **Ressourcengruppen**.
3. Klicken Sie auf **aswa-mongoose**.
4. Klicken Sie auf **Ressourcengruppe löschen**.
5. Geben Sie im Textfeld den Namen **aswa-mongoose** ein.
6. Klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

Im nächsten Artikel erfahren Sie, wie Sie die lokale Entwicklung konfigurieren.
> [!div class="nextstepaction"]
> [Einrichten der lokalen Entwicklung](./local-development.md)
