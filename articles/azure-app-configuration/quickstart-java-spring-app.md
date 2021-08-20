---
title: 'Schnellstart: Verwenden von Azure App Configuration'
description: In dieser Schnellanleitung erstellen Sie mit Azure App Configuration eine Java Spring-App, um die Speicherung und Verwaltung von Anwendungseinstellungen getrennt von Ihrem Code zu zentralisieren.
services: azure-app-configuration
documentationcenter: ''
author: AlexandraKemperMS
editor: ''
ms.service: azure-app-configuration
ms.topic: quickstart
ms.date: 04/18/2020
ms.custom: devx-track-java
ms.author: alkemper
ms.openlocfilehash: 301aab272d719bb89124f83d0dde0c616c37e031
ms.sourcegitcommit: 7d63ce88bfe8188b1ae70c3d006a29068d066287
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2021
ms.locfileid: "114450530"
---
# <a name="quickstart-create-a-java-spring-app-with-azure-app-configuration"></a>Schnellstart: Erstellen einer Java Spring-App mit Azure App Configuration

In dieser Schnellanleitung integrieren Sie Azure App Configuration in eine Java Spring-App, um die Speicherung und Verwaltung von Anwendungseinstellungen getrennt von Ihrem Code zu zentralisieren.

## <a name="prerequisites"></a>Voraussetzungen

- Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/)
- Ein unterstütztes [Java Development Kit (JDK)](/java/azure/jdk) mit Version 8.
- [Apache Maven](https://maven.apache.org/download.cgi) Version 3.0 oder höher

## <a name="create-an-app-configuration-store"></a>Erstellen eines App Configuration-Speichers

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

7. Wählen Sie **Konfigurations-Explorer** >  **+ Erstellen** > **Schlüssel-Wert** aus, um die folgenden Schlüssel-Wert-Paare hinzuzufügen:

    | Schlüssel | Wert |
    |---|---|
    | /application/config.message | Hallo |

    Lassen Sie **Bezeichnung** und **Inhaltstyp** vorerst leer.

8. Wählen Sie **Übernehmen**.

## <a name="create-a-spring-boot-app"></a>Erstellen einer Spring Boot-App

Verwenden Sie [Spring Initializr](https://start.spring.io/), um ein neues Spring Boot-Projekt zu erstellen.

1. Navigieren Sie zu <https://start.spring.io/>.

1. Verwenden Sie die folgenden Optionen:

   - Generieren Sie ein **Maven**-Projekt mit **Java**.
   - Geben Sie eine **Spring Boot**-Version ab 2.0 an.
   - Geben Sie Namen für die **Gruppe** und das **Artefakt** für Ihre Anwendung an.
   - Fügen Sie die Abhängigkeit **Spring Web** hinzu.

1. Wählen Sie nach Angabe der vorherigen Optionen die Option **Projekt generieren** aus. Laden Sie das Projekt nach entsprechender Aufforderung unter einem Pfad auf dem lokalen Computer herunter.

## <a name="connect-to-an-app-configuration-store"></a>Herstellen einer Verbindung mit einem App Configuration-Speicher

1. Nachdem Sie die Dateien auf Ihrem lokalen System extrahiert haben, kann Ihre einfache Spring Boot-Anwendung bearbeitet werden. Suchen Sie im Stammverzeichnis Ihrer App nach der Datei *pom.xml*.

1. Öffnen Sie die Datei *pom.xml* in einem Text-Editor, und fügen Sie Spring Cloud Azure Config Starter der Liste mit den Abhängigkeiten (`<dependencies>`) hinzu:

    **Spring Boot 2.4**

    ```xml
    <dependency>
        <groupId>com.azure.spring</groupId>
        <artifactId>azure-spring-cloud-appconfiguration-config</artifactId>
        <version>2.0.0</version>
    </dependency>
    ```

   > [!NOTE]
   > Wenn Sie Support für eine ältere Version von Spring Boot benötigen, sehen Sie sich unsere [alte Bibliothek](https://github.com/Azure/azure-sdk-for-java/blob/spring-cloud-starter-azure-appconfiguration-config_1.2.9/sdk/appconfiguration/spring-cloud-starter-azure-appconfiguration-config/README.md) an.

1. Erstellen Sie eine neue Java-Datei mit dem Namen *MessageProperties.java* im Paketverzeichnis Ihrer App. Fügen Sie die folgenden Zeilen hinzu:

    ```java
    package com.example.demo;

    import org.springframework.boot.context.properties.ConfigurationProperties;

    @ConfigurationProperties(prefix = "config")
    public class MessageProperties {
        private String message;

        public String getMessage() {
            return message;
        }

        public void setMessage(String message) {
            this.message = message;
        }
    }
    ```

1. Erstellen Sie im Paketverzeichnis Ihrer App eine neue Java-Datei mit dem Namen *HelloController.java*. Fügen Sie die folgenden Zeilen hinzu:

    ```java
    package com.example.demo;

    import org.springframework.web.bind.annotation.GetMapping;
    import org.springframework.web.bind.annotation.RestController;

    @RestController
    public class HelloController {
        private final MessageProperties properties;

        public HelloController(MessageProperties properties) {
            this.properties = properties;
        }

        @GetMapping
        public String getMessage() {
            return "Message: " + properties.getMessage();
        }
    }
    ```

1. Öffnen Sie die Java-Hauptanwendungsdatei, und fügen Sie `@EnableConfigurationProperties` hinzu, um diese Funktion zu aktivieren.

    ```java
    import org.springframework.boot.context.properties.EnableConfigurationProperties;

    @SpringBootApplication
    @EnableConfigurationProperties(MessageProperties.class)
    public class DemoApplication {
        public static void main(String[] args) {
            SpringApplication.run(DemoApplication.class, args);
        }
    }
    ```

1. Erstellen Sie eine neue Datei mit der Bezeichnung `bootstrap.properties` unter dem Verzeichnis „resources“ der App, und fügen Sie die folgenden Zeilen in der Datei ein. Ersetzen Sie die Beispielwerte durch die entsprechenden Eigenschaften für Ihren App Configuration-Speicher.

    ```CLI
    spring.cloud.azure.appconfiguration.stores[0].connection-string= ${APP_CONFIGURATION_CONNECTION_STRING}
    ```

1. Legen Sie eine Umgebungsvariable mit dem Namen **APP_CONFIGURATION_CONNECTION_STRING** auf den Zugriffsschlüssel für Ihren App Configuration-Speicher fest. Führen Sie an der Befehlszeile den folgenden Befehl aus, und starten Sie die Eingabeaufforderung neu, damit die Änderung wirksam wird:

    ```cmd
    setx APP_CONFIGURATION_CONNECTION_STRING "connection-string-of-your-app-configuration-store"
    ```

    Führen Sie bei Verwendung von Windows PowerShell den folgenden Befehl aus:

    ```azurepowershell
    $Env:APP_CONFIGURATION_CONNECTION_STRING = "connection-string-of-your-app-configuration-store"
    ```

    Führen Sie bei Verwendung von macOS oder Linux den folgenden Befehl aus:

    ```cmd
    export APP_CONFIGURATION_CONNECTION_STRING='connection-string-of-your-app-configuration-store'
    ```

## <a name="build-and-run-the-app-locally"></a>Lokales Erstellen und Ausführen der App

1. Erstellen Sie Ihre Spring Boot-Anwendung mit Maven, und führen Sie sie aus. Beispiel:

    ```cmd
    mvn clean package
    mvn spring-boot:run
    ```

2. Nachdem Ihre Anwendung ausgeführt wird, testen Sie sie mit *cURL*. Beispiel:

      ```cmd
      curl -X GET http://localhost:8080/
      ```

    Es wird die Nachricht angezeigt, die Sie im App Configuration-Speicher eingegeben haben.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie einen neuen App Configuration-Speicher erstellt und mit einer Java Spring-App verwendet. Weitere Informationen finden Sie unter [Spring in Azure](/java/azure/spring-framework/). Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Ihre Java Spring-App für das dynamische Aktualisieren der Konfigurationseinstellungen konfigurieren.

> [!div class="nextstepaction"]
> [Tutorial: Verwenden der dynamischen Konfiguration in einer .NET Framework-App](./enable-dynamic-configuration-java-spring-app.md)
