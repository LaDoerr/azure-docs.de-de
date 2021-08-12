---
title: 'Azure Event Hubs: Verarbeiten von Apache Kafka-Ereignissen'
description: 'Tutorial: Dieser Artikel zeigt, wie Kafka-Ereignisse verarbeitet werden, die mithilfe von Azure Stream Analytics über Event Hubs erfasst werden.'
ms.topic: tutorial
ms.date: 05/10/2021
ms.openlocfilehash: 0a14a4f8a4e82faebe232ac072ebe61e2db427b7
ms.sourcegitcommit: d90cb315dd90af66a247ac91d982ec50dde1c45f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/04/2021
ms.locfileid: "113286371"
---
# <a name="tutorial-process-apache-kafka-for-event-hubs-events-using-stream-analytics"></a>Tutorial: Verarbeiten von Apache Kafka für Event Hubs-Ereignisse mithilfe von Stream Analytics 
Dieser Artikel zeigt, wie Daten an Event Hubs gestreamt und mit Azure Stream Analytics verarbeitet werden können. Die folgenden Schritte werden behandelt: 

1. Erstellen eines Event Hubs-Namespace
2. Erstellen eines Kafka-Clients, der Nachrichten an den Event Hub sendet
3. Erstellen eines Stream Analytics-Auftrags, der Daten aus dem Event Hub in Azure Blob Storage kopiert 

Sie müssen Ihre Protokollclients nicht ändern oder eigene Cluster betreiben, wenn Sie den Kafka-Endpunkt verwenden, der von einem Event Hub verfügbar gemacht wird. Azure Event Hubs unterstützt [Apache Kafka Version 1.0](https://kafka.apache.org/10/documentation.html). und höher. 


## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung benötigen Sie Folgendes:

* Ein Azure-Abonnement. Wenn Sie keins besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) erstellen, bevor Sie beginnen.
* [Java Development Kit (JDK) 1.7 oder höher](/azure/developer/java/fundamentals/java-support-on-azure)
* Ein binäres Maven-Archiv ([Download](https://maven.apache.org/download.cgi)/[Installationsanleitung](https://maven.apache.org/install.html))
* [Git-Client](https://www.git-scm.com/)
* Ein **Azure Storage-Konto** Wenn keine öffentliche IP-Adresse vorhanden ist, [erstellen Sie jetzt eine](../storage/common/storage-account-create.md), ehe Sie fortfahren. Der Stream Analytics-Auftrag in dieser exemplarischen Vorgehensweise speichert die Ausgabedaten in Azure Blob Storage. 


## <a name="create-an-event-hubs-namespace"></a>Erstellen eines Event Hubs-Namespace
Wenn Sie einen Event Hubs-Namespace erstellen, wird der Kafka-Endpunkt für den Namespace automatisch aktiviert. Sie können Ereignisse von Ihren Anwendungen streamen, die das Kafka-Protokoll in Event Hubs verwenden. Befolgen Sie die Schritt-für-Schritt-Anleitung unter [Erstellen eines Event Hubs mithilfe des Azure-Portals](event-hubs-create.md), um einen Event Hubs-Namespace zu erstellen. Wenn Sie einen dedizierten Cluster verwenden, finden Sie weitere Informationen unter [Erstellen eines Namespace und eines Event Hubs in einem dedizierten Cluster](event-hubs-dedicated-cluster-create-portal.md#create-a-namespace-and-event-hub-within-a-cluster).

> [!NOTE]
> Event Hubs für Kafka wird im **Basic-Tarif** nicht unterstützt.

## <a name="send-messages-with-kafka-in-event-hubs"></a>Senden von Nachrichten mit Kafka in Event Hubs

1. Klonen Sie das [Azure Event Hubs für Kafka-Repository](https://github.com/Azure/azure-event-hubs-for-kafka) auf Ihrem Computer.
2. Navigieren Sie zum Ordner `azure-event-hubs-for-kafka/quickstart/java/producer`. 
4. Aktualisieren Sie die Konfigurationsdetails für die Producer in `src/main/resources/producer.config`. Geben Sie den **Namen** und die **Verbindungszeichenfolge** des **Event Hub-Namespace** an. 

    ```xml
    bootstrap.servers={EVENT HUB NAMESPACE}.servicebus.windows.net:9093
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{CONNECTION STRING for EVENT HUB NAMESPACE}";
    ```

5. Navigieren Sie zu `azure-event-hubs-for-kafka/quickstart/java/producer/src/main/java/`, und öffnen Sie die Datei **TestDataReporter.java** in einem Editor Ihrer Wahl. 
6. Kommentieren Sie die folgenden Codezeilen aus:

    ```java
                //final ProducerRecord<Long, String> record = new ProducerRecord<Long, String>(TOPIC, time, "Test Data " + i);
    ```
3. Fügen Sie anstelle des auskommentierten Codes die folgende Codezeile hinzu: 

    ```java
                final ProducerRecord<Long, String> record = new ProducerRecord<Long, String>(TOPIC, time, "{ \"eventData\": \"Test Data " + i + "\" }");            
    ```

    Dieser Code sendet die Ereignisdaten im **JSON**-Format. Wenn Sie die Eingabe für einen Stream Analytics-Auftrag konfigurieren, geben Sie JSON als Format für die Eingabedaten an. 
7. **Führen Sie den Producer aus**, und streamen Sie Daten an Event Hubs. Wechseln Sie auf einem Windows-Computer bei Verwendung einer **Node.js-Eingabeaufforderung** zum Ordner `azure-event-hubs-for-kafka/quickstart/java/producer`, bevor Sie diese Befehle ausführen. 
   
    ```shell
    mvn clean package
    mvn exec:java -Dexec.mainClass="TestProducer"                                    
    ```

## <a name="verify-that-event-hub-receives-the-data"></a>Sicherstellen, dass der Event Hub die Daten empfängt

1. Wählen Sie unter **ENTITÄTEN** die Option **Event Hubs** aus. Bestätigen Sie, dass ein Event Hub mit dem Namen **test** angezeigt wird. 

    ![Event Hub „test“](./media/event-hubs-kafka-stream-analytics/test-event-hub.png)
2. Bestätigen Sie, dass im Event Hub Nachrichten eingehen. 

    ![Event Hub-Nachrichten](./media/event-hubs-kafka-stream-analytics/confirm-event-hub-messages.png)

## <a name="process-event-data-using-a-stream-analytics-job"></a>Verarbeiten von Ereignisdaten mithilfe eines Stream Analytics-Auftrags
In diesem Abschnitt erstellen Sie einen Azure Stream Analytics-Auftrag. Der Kafka-Client sendet Ereignisse an den Event Hub. Sie erstellen einen Stream Analytics-Auftrag, der Ereignisdaten als Eingabe verwendet und diese in Azure Blob Storage ausgibt. Wenn Sie kein **Azure Storage-Konto** haben, müssen Sie eines [erstellen](../storage/common/storage-account-create.md).

Die Abfrage im Stream Analytics-Auftrag durchläuft die Daten, ohne eine Analyse vorzunehmen. Sie können eine Abfrage erstellen, die die Eingabedaten transformiert, um Ausgabedaten in einem anderen Format oder mit gewonnenen Erkenntnissen zu erzeugen.  

### <a name="create-a-stream-analytics-job"></a>Erstellen eines Stream Analytics-Auftrags 

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) auf **+ Ressource erstellen**.
2. Wählen Sie **Analytics** im Menü **Azure Marketplace** und dann **Stream Analytics-Auftrag** aus. 
3. Gehen Sie auf der Seite **Neuer Stream Analytics-Auftrag** wie folgt vor: 
    1. Geben Sie einen **Namen** für den Auftrag ein. 
    2. Wählen Sie Ihr **Abonnement** aus.
    3. Klicken Sie für die **Ressourcengruppe** auf **Neu erstellen**, und geben Sie den Namen ein. Sie können auch eine **vorhandene** Ressourcengruppe verwenden. 
    4. Wählen Sie einen **Speicherort** für den Auftrag aus.
    5. Klicken Sie auf **Erstellen**, um den Auftrag zu erstellen. 

        ![Neuer Stream Analytics-Auftrag](./media/event-hubs-kafka-stream-analytics/new-stream-analytics-job.png)

### <a name="configure-job-input"></a>Konfigurieren einer Auftragseingabe

1. Wählen Sie in der Benachrichtigungsmeldung **Zur Ressource wechseln** aus, um die Seite **Stream Analytics-Auftrag** einzublenden. 
2. Wählen Sie im Abschnitt **AUFTRAGSTOPOLOGIE** im linken Menü **Eingaben** aus.
3. Klicken Sie auf **Datenstromeingabe hinzufügen**, und wählen Sie **Event Hub** aus. 

    ![Hinzufügen eines Event Hubs als Eingabe](./media/event-hubs-kafka-stream-analytics/select-event-hub-input.png)
4. Führen Sie auf der Konfigurationsseite **Event Hub-Eingabe** die folgenden Schritte aus: 

    1. Geben Sie einen **Alias** für die Eingabe an. 
    2. Wählen Sie Ihr Azure-**Abonnement** aus.
    3. Wählen Sie den **Event Hub-Namespace** aus, den Sie zuvor erstellt haben. 
    4. Wählen Sie **test** als **Event Hub** aus. 
    5. Wählen Sie **Speichern** aus. 

        ![Event Hub-Eingabekonfiguration](./media/event-hubs-kafka-stream-analytics/event-hub-input-configuration.png)

### <a name="configure-job-output"></a>Konfigurieren der Auftragsausgabe 

1. Wählen Sie im Abschnitt **AUFTRAGSTOPOLOGIE** im linken Menü **Ausgaben** aus. 
2. Klicken Sie auf der Symbolleiste auf **+ Hinzufügen**, und wählen Sie **Blob Storage** aus.
3. Gehen Sie auf der Seite mit den Blob Storage-Ausgabeeinstellungen folgendermaßen vor: 
    1. Geben Sie einen **Alias** für die Ausgabe an. 
    2. Wählen Sie Ihr Azure- **Abonnement** aus. 
    3. Wählen Sie Ihr **Azure Storage-Konto** aus. 
    4. Geben Sie einen **Namen für den Container** ein, in dem die Ausgabedaten der Stream Analytics-Abfrage gespeichert werden.
    5. Wählen Sie **Speichern** aus.

        ![Blob Storage-Ausgabekonfiguration](./media/event-hubs-kafka-stream-analytics/output-blob-settings.png)
 

### <a name="define-a-query"></a>Definieren einer Abfrage
Nachdem Sie einen Stream Analytics-Auftrag eingerichtet haben, um einen eingehenden Datenstrom zu lesen, ist der nächste Schritt die Erstellung einer Transformation, bei der Daten in Echtzeit analysiert werden. Sie definieren die Transformationsabfrage mit der [Stream Analytics-Abfragesprache](/stream-analytics-query/stream-analytics-query-language-reference). In dieser exemplarischen Vorgehensweise definieren Sie eine Abfrage, die die Daten ohne jegliche Transformation durchläuft.

1. Wählen Sie **Abfrage**.
2. Ersetzen Sie im Abfragefenster `[YourOutputAlias]` durch den Ausgabealias, den Sie zuvor erstellt haben.
3. Ersetzen Sie `[YourInputAlias]` durch den Eingabealias, den Sie zuvor erstellt haben. 
4. Wählen Sie auf der Symbolleiste **Speichern** aus. 

    ![Screenshot: Abfragefenster mit Werten für Eingabe- und Ausgabevariablen](./media/event-hubs-kafka-stream-analytics/query.png)


### <a name="run-the-stream-analytics-job"></a>Ausführen des Stream Analytics-Auftrags

1. Klicken Sie im linken Menü auf **Übersicht**. 
2. Wählen Sie **Starten** aus. 

    ![Startmenü](./media/event-hubs-kafka-stream-analytics/start-menu.png)
1. Klicken Sie auf der Seite **Auftrag starten** auf **Jetzt**. 

    ![Seite „Auftrag starten“](./media/event-hubs-kafka-stream-analytics/start-job-page.png)
1. Warten Sie, bis sich der Status des Auftrags von **Wird gestartet** in **Wird ausgeführt** ändert. 

    ![Auftragsstatus: Wird ausgeführt](./media/event-hubs-kafka-stream-analytics/running.png)

## <a name="test-the-scenario"></a>Prüfen des Szenarios
1. Führen Sie die **Kafka Producer** erneut aus, um Ereignisse an den Event Hub zu senden. 

    ```shell
    mvn exec:java -Dexec.mainClass="TestProducer"                                    
    ```
1. Überprüfen Sie, ob **Ausgabedaten** in **Azure Blob Storage** generiert werden. Sie sehen im Container eine JSON-Datei mit 100 Zeilen, die wie die folgenden Beispielzeilen aussehen: 

    ```
    {"eventData":"Test Data 0","EventProcessedUtcTime":"2018-08-30T03:27:23.1592910Z","PartitionId":0,"EventEnqueuedUtcTime":"2018-08-30T03:27:22.9220000Z"}
    {"eventData":"Test Data 1","EventProcessedUtcTime":"2018-08-30T03:27:23.3936511Z","PartitionId":0,"EventEnqueuedUtcTime":"2018-08-30T03:27:22.9220000Z"}
    {"eventData":"Test Data 2","EventProcessedUtcTime":"2018-08-30T03:27:23.3936511Z","PartitionId":0,"EventEnqueuedUtcTime":"2018-08-30T03:27:22.9220000Z"}
    ```

    Der Azure Stream Analytics-Auftrag hat in diesem Szenario Eingabedaten vom Event Hub empfangen und diese in Azure Blob Storage gespeichert. 



## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie erfahren, wie Daten an Event Hubs gestreamt werden, ohne dass Sie Protokollclients ändern oder eigene Cluster ausführen müssen. Weitere Informationen zu Event Hubs für Apache Kafka finden Sie unter [Apache Kafka-Entwicklerleitfaden für Azure Event Hubs](apache-kafka-developer-guide.md).