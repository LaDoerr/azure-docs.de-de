---
title: 'Tutorial: Erstellen einer Java-App zum Erstellen eines Cassandra-API-Kontos in Azure Cosmos DB'
description: Dieses Tutorial zeigt, wie Sie ein Cassandra-API-Konto erstellen, eine Datenbank (auch als „Keyspace“ bezeichnet) hinzufügen und mithilfe einer Java-App eine Tabelle zu diesem Konto hinzufügen.
author: TheovanKraay
ms.author: thvankra
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: tutorial
ms.date: 12/06/2018
ms.custom: seodec18, devx-track-java
ms.openlocfilehash: b17734e83a6a096aedfdfa6aa1485cbddc6642f0
ms.sourcegitcommit: 82d82642daa5c452a39c3b3d57cd849c06df21b0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2021
ms.locfileid: "113355124"
---
# <a name="tutorial-create-a-cassandra-api-account-in-azure-cosmos-db-by-using-a-java-application-to-store-keyvalue-data"></a>Tutorial: Erstellen eines Cassandra-API-Kontos in Azure Cosmos DB durch die Verwendung einer Java-Anwendung zum Speichern von Schlüssel/Wert-Daten
[!INCLUDE[appliesto-cassandra-api](includes/appliesto-cassandra-api.md)]

Als Entwickler besitzen Sie möglicherweise Anwendungen, die Schlüssel/Wert-Paare verwenden. Sie können ein Cassandra-API-Konto in Azure Cosmos DB zum Speichern der Schlüssel/Wert-Daten verwenden. In diesem Tutorial wird beschrieben, wie mithilfe einer Java-Anwendung ein Cassandra-API-Konto in Azure Cosmos DB erstellt und eine Datenbank (auch als „Keyspace“ bezeichnet) sowie eine Tabelle hinzugefügt wird. Die Java-Anwendung verwendet den [Java-Treiber](https://github.com/datastax/java-driver), um eine Benutzerdatenbank zu erstellen, die Details wie Benutzer-ID, Benutzernamen und Stadt des Benutzers enthält.  

Dieses Tutorial enthält die folgenden Aufgaben:

> [!div class="checklist"]
> * Erstellen eines Cassandra-Datenbankkontos
> * Abrufen der Kontoverbindungszeichenfolge
> * Erstellen eines Maven-Projekts und der Abhängigkeiten
> * Hinzufügen einer Datenbank und Tabelle
> * Ausführen der App

## <a name="prerequisites"></a>Voraussetzungen 

* Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) erstellen, bevor Sie beginnen. 

* Beziehen Sie die aktuelle Version des [Java Development Kit (JDK)](/java/azure/jdk/). 

* Führen Sie den [Download](https://maven.apache.org/download.cgi) und die [Installation](https://maven.apache.org/install.html) des binären [Maven](https://maven.apache.org/)-Archivs durch. 
  - Unter Ubuntu können Sie `apt-get install maven` ausführen, um Maven zu installieren. 

## <a name="create-a-database-account"></a>Erstellen eines Datenbankkontos 

[!INCLUDE [cosmos-db-create-dbaccount-cassandra](includes/cosmos-db-create-dbaccount-cassandra.md)]

## <a name="get-the-connection-details-of-your-account"></a>Abrufen der Verbindungsdetails zu Ihrem Konto  

Rufen Sie die Informationen der Verbindungszeichenfolge über das Azure-Portal ab, und fügen Sie sie in die Java-Konfigurationsdatei ein. Die Angabe der Verbindungszeichenfolge ermöglicht Ihrer App die Kommunikation mit Ihrer gehosteten Datenbank. 

1. Wechseln Sie vom [Azure-Portal](https://portal.azure.com/) zu Ihrem Azure Cosmos-Konto. 

2. Öffnen Sie den Bereich **Verbindungszeichenfolge**.  

3. Kopieren Sie die Werte von **KONTAKTPUNKT**, **PORT**, **BENUTZERNAME**, und **PRIMÄRES KENNWORT**. Diese werden Sie in den nächsten Schritten benötigen.

## <a name="create-the-project-and-the-dependencies"></a>Erstellen des Projekts und der Abhängigkeiten 

Das Java-Beispielprojekt, das Sie in diesem Artikel verwenden, wird in GitHub gehostet. Sie können die Schritte in diesem Dokument ausführen oder das Beispiel aus dem Repository [azure-cosmos-db-cassandra-java-getting-started](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started) herunterladen. 

Aktualisieren Sie nach dem Herunterladen der Dateien, die Informationen zur Verbindungszeichenfolge in der Datei `java-examples\src\main\resources\config.properties`, und führen Sie sie aus.  

```java
cassandra_host=<FILLME_with_CONTACT POINT> 
cassandra_port = 10350 
cassandra_username=<FILLME_with_USERNAME> 
cassandra_password=<FILLME_with_PRIMARY PASSWORD> 
```

Verwenden Sie die folgenden Schritte, um das Beispiel von Grund auf neu zu erstellen: 

1. Erstellen Sie über das Terminal oder die Eingabeaufforderung ein neues Maven-Projekt namens „Cassandra-demo“. 

   ```bash
   mvn archetype:generate -DgroupId=com.azure.cosmosdb.cassandra -DartifactId=cassandra-demo -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false 
   ```
 
2. Suchen Sie den Ordner `cassandra-demo`. Öffnen Sie in einem Text-Editor die generierte Datei `pom.xml`. 

   Fügen Sie die Cassandra-Abhängigkeiten hinzu, und erstellen Sie die für Ihr Projekt erforderlichen Plug-Ins, die in der Datei [pom.xml](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/main/pom.xml) gezeigt werden.  

3. Erstellen Sie im Ordner `cassandra-demo\src\main` einen neuen Ordner namens `resources`.  Fügen Sie im Ressourcenordner die Dateien „config.properties“ und „log4j.properties“ hinzu:

   - Die Datei [config.properties](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/main/src/main/resources/config.properties) speichert den Verbindungsendpunkt und die Schlüsselwerte des Cassandra-API-Kontos. 
   
   - Die Datei [log4j.properties](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/main/src/main/resources/log4j.properties) definiert die Ebene der Protokollierung, die für die Interaktion mit der Cassandra-API erforderlich ist.  

4. Navigieren Sie zum Ordner `src/main/java/com/azure/cosmosdb/cassandra/`. Erstellen Sie im Cassandra-Ordner einen weiteren Ordner mit dem Namen `utils`. Der neue Ordner speichert die Hilfsprogrammklassen, die zum Herstellen einer Verbindung mit der Cassandra-API-Konto erforderlich sind. 

   Fügen Sie die Klasse [CassandraUtils](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/main/src/main/java/com/azure/cosmosdb/cassandra/util/CassandraUtils.java) hinzu, um den Cluster zu erstellen und Cassandra-Sitzungen zu öffnen und zu schließen. Der Cluster stellt eine Verbindung mit dem Cassandra-API-Konto in Azure Cosmos DB her und gibt eine Sitzung für den Zugriff zurück. Lesen Sie mithilfe der Klasse [Configurations](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/main/src/main/java/com/azure/cosmosdb/cassandra/util/Configurations.java) Informationen zur Verbindungszeichenfolge aus der Datei „config.properties“. 

5. Das Java-Beispiel erstellt eine Datenbank mit Benutzerinformationen wie Benutzernamen, Benutzer-ID und Stadt des Benutzers. Sie müssen GET- und SET-Methoden definieren, um auf die Benutzerdetails in der main-Funktion zuzugreifen.
 
   Erstellen Sie eine [User.java](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/main/src/main/java/com/azure/cosmosdb/cassandra/examples/UserProfile.java)-Klasse im Ordner `src/main/java/com/azure/cosmosdb/cassandra/` mit GET- und SET-Methoden. 

## <a name="add-a-database-and-a-table"></a>Hinzufügen einer Datenbank und Tabelle  

In diesem Abschnitt wird beschrieben, wie Sie eine Datenbank (Keyspace) und eine Tabelle mit CQL hinzufügen.

1. Erstellen Sie im Ordner `src\main\java\com\azure\cosmosdb\cassandra` einen neuen Ordner namens `repository`. 

2. Erstellen Sie die Java-Klasse `UserRepository`, und fügen Sie den folgenden Code hinzu: 

   ```java
   package com.azure.cosmosdb.cassandra.repository; 
   import java.util.List; 
   import com.datastax.driver.core.BoundStatement; 
   import com.datastax.driver.core.PreparedStatement; 
   import com.datastax.driver.core.Row; 
   import com.datastax.driver.core.Session; 
   import org.slf4j.Logger; 
   import org.slf4j.LoggerFactory; 
   
   /** 
    * Create a Cassandra session 
    */ 
   public class UserRepository { 
   
       private static final Logger LOGGER = LoggerFactory.getLogger(UserRepository.class); 
       private Session session; 
       public UserRepository(Session session) { 
           this.session = session; 
       } 
   
       /** 
       * Create keyspace uprofile in cassandra DB 
        */ 
   
       public void createKeyspace() { 
            final String query = "CREATE KEYSPACE IF NOT EXISTS uprofile WITH REPLICATION = { 'class' : 'NetworkTopologyStrategy', 'datacenter1' : 1 }"; 
           session.execute(query); 
           LOGGER.info("Created keyspace 'uprofile'"); 
       } 
   
       /** 
        * Create user table in cassandra DB 
        */ 
   
       public void createTable() { 
           final String query = "CREATE TABLE IF NOT EXISTS uprofile.user (user_id int PRIMARY KEY, user_name text, user_bcity text)"; 
           session.execute(query); 
           LOGGER.info("Created table 'user'"); 
       } 
   } 
   ```

3. Suchen Sie den Ordner `src\main\java\com\azure\cosmosdb\cassandra`, und erstellen Sie einen neuen Unterordner namens `examples`.

4. Erstellen Sie die Java-Klasse `UserProfile`. Diese Klasse enthält die main-Methode, die die Methoden createKeyspace und createTable aufruft, die Sie zuvor definiert haben: 

   ```java
   package com.azure.cosmosdb.cassandra.examples; 
   import java.io.IOException; 
   import com.azure.cosmosdb.cassandra.repository.UserRepository; 
   import com.azure.cosmosdb.cassandra.util.CassandraUtils; 
   import com.datastax.driver.core.PreparedStatement; 
   import com.datastax.driver.core.Session; 
   import org.slf4j.Logger; 
   import org.slf4j.LoggerFactory; 
   
   /** 
    * Example class which will demonstrate following operations on Cassandra Database on CosmosDB 
    * - Create Keyspace 
    * - Create Table 
    * - Insert Rows 
    * - Select all data from a table 
    * - Select a row from a table 
    */ 
   
   public class UserProfile { 
   
       private static final Logger LOGGER = LoggerFactory.getLogger(UserProfile.class); 
       public static void main(String[] s) throws Exception { 
           CassandraUtils utils = new CassandraUtils(); 
           Session cassandraSession = utils.getSession(); 
   
           try { 
               UserRepository repository = new UserRepository(cassandraSession); 
               //Create keyspace in cassandra database 
               repository.createKeyspace(); 
               //Create table in cassandra database 
               repository.createTable(); 
   
           } finally { 
               utils.close(); 
               LOGGER.info("Please delete your table after verifying the presence of the data in portal or from CQL"); 
           } 
       } 
   } 
   ```
 
## <a name="run-the-app"></a>Ausführen der App 

1. Öffnen Sie eine Eingabeaufforderung oder ein Terminalfenster. Fügen Sie folgenden Codeblock ein. 

   Dieser Code ändert das Verzeichnis (cd) in den Ordnerpfad, in dem das Projekt erstellt wurde. Anschließend führt er den Befehl `mvn clean install` zum Generieren der Datei `cosmosdb-cassandra-examples.jar` im Zielordner aus. Abschließend führt er die Java-Anwendung aus.

   ```bash
   cd cassandra-demo

   mvn clean install 

   java -cp target/cosmosdb-cassandra-examples.jar com.azure.cosmosdb.cassandra.examples.UserProfile 
   ```

   Im Terminalfenster werden Benachrichtigungen angezeigt, dass der Keyspace und die Tabelle erstellt wurden. 
   
2. Öffnen Sie im Azure-Portal nun den **Daten-Explorer**, um sicherzustellen, dass der Keyspace und die Tabelle erstellt wurden.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie ein Cassandra-API-Konto in Azure Cosmos DB, eine Datenbank und eine Tabelle mithilfe einer Java-Anwendung erstellen. Sie können jetzt mit dem nächsten Artikel fortfahren:

> [!div class="nextstepaction"]
> [Beispieldaten in die Cassandra-API-Tabelle laden](cassandra-api-load-data.md)
