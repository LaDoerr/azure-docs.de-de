---
title: Laden von Daten aus Office 365 mithilfe von Azure Data Factory
description: Kopieren von Daten aus Office 365 mithilfe von Azure Data Factory
author: jianleishen
ms.service: data-factory
ms.subservice: data-movement
ms.topic: conceptual
ms.date: 07/05/2021
ms.author: jianleishen
ms.openlocfilehash: 6fa350757ff1d4595c0eaa9c3dffb1c7299fb116
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2021
ms.locfileid: "124835984"
---
# <a name="load-data-from-office-365-by-using-azure-data-factory"></a>Laden von Daten aus Office 365 mithilfe von Azure Data Factory

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

In diesem Artikel wird gezeigt, wie Sie mithilfe von Data Factory _Daten aus Office 365 in Azure Blob Storage laden_. Zum Kopieren von Daten in Azure Data Lake Gen1 oder Gen2 werden ähnliche Schritte ausgeführt. Allgemeine Informationen zum Kopieren von Daten aus Office 365 finden Sie im Artikel zum [Office 365-Connector](connector-office-365.md).

## <a name="create-a-data-factory"></a>Erstellen einer Data Factory

1. Klicken Sie im Menü auf der linken Seite auf **Ressource erstellen** > **Analytics** > **Data Factory**: 
   
   :::image type="content" source="./media/quickstart-create-data-factory-portal/new-azure-data-factory-menu.png" alt-text="Auswahl von Data Factory im Bereich &quot;Neu&quot;":::

2. Geben Sie auf der Seite **Neue Data Factory** die in der folgenden Abbildung gezeigten Werte für die Felder an:
      
   :::image type="content" source="./media/load-office-365-data/new-azure-data-factory.png" alt-text="Seite „Neue Data Factory“":::
 
    * **Name**: Geben Sie einen global eindeutigen Namen für die Azure Data Factory ein. Wenn die Fehlermeldung „Data Factory mit dem Namen *LoadFromOffice365Demo* ist nicht verfügbar“ angezeigt wird, geben Sie einen anderen Namen für die Data Factory ein. Sie können beispielsweise den Namen _**IhrName**_ **LoadFromOffice365Demo** verwenden. Versuchen Sie erneut, die Data Factory zu erstellen. Benennungsregeln für Data Factory-Artefakte finden Sie im Thema [Data Factory – Benennungsregeln](naming-rules.md).
    * **Abonnement**: Wählen Sie Ihr Azure-Abonnement aus, in dem die Data Factory erstellt werden soll. 
    * **Ressourcengruppe**: Wählen Sie eine vorhandene Ressourcengruppe aus der Dropdownliste aus, oder wählen Sie die Option **Neu erstellen** aus, und geben Sie dann den Namen einer Ressourcengruppe ein. Weitere Informationen über Ressourcengruppen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen](../azure-resource-manager/management/overview.md).  
    * **Version**: Wählen Sie **V2** aus.
    * **Standort**: Wählen Sie den Standort für die Data Factory aus. In der Dropdownliste werden nur unterstützte Standorte angezeigt. Die von der Data Factory verwendeten Datenspeicher können sich an anderen Standorten bzw. in anderen Regionen befinden. Diese Datenspeicher umfassen Azure Data Lake Store, Azure Storage, Azure SQL-Datenbank usw.

3. Klicken Sie auf **Erstellen**.
4. Nach Abschluss der Erstellung navigieren Sie zu Ihrer Data Factory. Die Startseite **Data Factory** wird wie in der folgenden Abbildung dargestellt angezeigt:
   
   :::image type="content" source="./media/doc-common-process/data-factory-home-page.png" alt-text="Startseite für Azure Data Factory mit der Kachel „Open Azure Data Factory Studio“":::

5. Klicken Sie auf der Kachel **Open Azure Data Factory Studio** auf **Öffnen**, um die Datenintegration-Anwendung in einer separaten Registerkarte zu starten.

## <a name="create-a-pipeline"></a>Erstellen einer Pipeline

1. Wählen Sie auf der Startseite die Option **Orchestrieren** aus.
 
    :::image type="content" source="./media/doc-common-process/get-started-page.png" alt-text="Screenshot, der die ADF-Startseite zeigt.":::

2. Geben Sie auf der Registerkarte **Allgemein** der Pipeline als **Name** der Pipeline „CopyPipeline“ ein.

3. Verschieben Sie in der Toolbox „Aktivitäten“ in der Kategorie „Move & Transform“ (Verschieben und transformieren) die **Copy**-Aktivität aus der Toolbox auf die Oberfläche des Pipeline-Designers. Geben Sie „CopyFromOffice365ToBlob“ als Aktivitätsnamen an.

### <a name="configure-source"></a>Konfigurieren der Quelle

1. Navigieren Sie zur Pipeline und zur Registerkarte **Quelle**, und klicken Sie auf **+ Neu**, um ein Quelldataset zu erstellen. 

2. Wählen Sie im Fenster „Neues Dataset“ die Option **Office 365** aus, und klicken Sie dann auf **Weiter**.
 
3. Sie befinden sich jetzt auf der Registerkarte zum Konfigurieren der Kopieraktivität. Klicken Sie auf die Schaltfläche **Bearbeiten** neben dem Office 365-Dataset, um die Datenkonfiguration fortzusetzen.

    :::image type="content" source="./media/load-office-365-data/transition-to-edit-dataset.png" alt-text="Konfigurieren des Office 365-Datasets unter „Allgemein“":::
 
4. Eine neue Registerkarte für das Office 365-Dataset wird geöffnet. Geben Sie unten im Eigenschaftenfenster auf der Registerkarte **Allgemein** unter „Name“ den Namen „SourceOffice365Dataset“ ein.
 
5. Wechseln Sie im Eigenschaftenfenster zur Registerkarte **Verbindung**. Klicken Sie neben dem Textfeld „Verknüpfter Dienst“ auf **+ Neu**.

6. Geben Sie im Fenster „New Linked Service“ (Neuer verknüpfter Dienst) als Name „Office365LinkedService“ sowie die Dienstprinzipal-ID und den Dienstprinzipalschlüssel ein. Testen Sie anschließend die Verbindung, und wählen Sie **Erstellen** aus, um den verknüpften Dienst bereitzustellen.

    :::image type="content" source="./media/load-office-365-data/new-office-365-linked-service.png" alt-text="Neuer verknüpfter Office 365-Dienst":::
 
7. Nach der Erstellung des verknüpften Diensts werden wieder die Dataseteinstellungen angezeigt. Klicken Sie neben **Tabelle** auf den Pfeil nach unten, um die Liste der verfügbaren Office 365-Datasets zu erweitern, und wählen Sie dann in der Dropdownliste „BasicDataSet_v0.Message_v0“ aus:

    :::image type="content" source="./media/load-office-365-data/edit-dataset.png" alt-text="Konfigurieren des Office 365-Datasets unter „Tabelle“":::

8. Wechseln Sie nun zurück zur Registerkarte **Pipeline** > **Quelle**, um die Konfiguration zusätzlicher Eigenschaften für die Office 365-Datenextraktion fortzusetzen.  Benutzerbereich und Benutzerbereichsfilter sind optionale Prädikate, die Sie definieren können, um die Daten einzuschränken, die aus Office 365 extrahiert werden sollen. Informationen zum Konfigurieren dieser Einstellungen finden Sie im Abschnitt [Office 365 – Dataset-Eigenschaften](./connector-office-365.md#dataset-properties).

9. Sie müssen einen der Datumsfilter auswählen und Werte für die Start- und Endzeit angeben.

10. Klicken Sie auf die Registerkarte **Schema importieren**, um das Schema für das Nachrichtendataset zu importieren.

    :::image type="content" source="./media/load-office-365-data/edit-source-properties.png" alt-text="Konfigurieren des Office 365-Datasets unter „Schema“":::

### <a name="configure-sink"></a>Konfigurieren der Senke

1. Wechseln Sie zur Pipeline und zur Registerkarte **Senke**, und wählen Sie **+ Neu** aus, um ein Senkendataset zu erstellen.
 
2. Im Fenster „Neues Dataset“ werden beim Kopieren aus Office 365 nur die unterstützten Ziele ausgewählt. Wählen Sie **Azure Blob Storage**, das Binärformat und dann **Weiter** aus.  In diesem Tutorial kopieren Sie Office 365-Daten in Azure Blob Storage.

3. Klicken Sie neben dem Azure Blob Storage-Dataset auf die Schaltfläche **Bearbeiten**, um die Datenkonfiguration fortzusetzen.

4. Geben Sie im Eigenschaftenfenster auf der Registerkarte **Allgemein** unter „Name“ den Namen „OutputBlobDataset“ ein.

5. Wechseln Sie im Eigenschaftenfenster zur Registerkarte **Verbindung**. Klicken Sie neben dem Textfeld „Verknüpfter Dienst“ auf **+ Neu**.

6. Geben Sie im Fenster „New Linked Service“ (Neuer verknüpfter Dienst) als Name „AzureStorageLinkedService“ ein, wählen Sie in der Dropdownliste der Authentifizierungsmethoden die Option „Dienstprinzipal“ aus, und geben Sie den Dienstendpunkt, den Mandanten, die Dienstprinzipal-ID und den Dienstprinzipalschlüssel ein. Wählen Sie anschließend „Speichern“ aus, um den verknüpften Dienst bereitzustellen.  [Hier](connector-azure-blob-storage.md#service-principal-authentication) finden Sie Informationen zum Einrichten der Dienstprinzipalauthentifizierung für Azure Blob Storage.

    :::image type="content" source="./media/load-office-365-data/configure-blob-linked-service.png" alt-text="Neuer verknüpfter Blobdienst":::


## <a name="validate-the-pipeline"></a>Überprüfen der Pipeline

Klicken Sie auf der Symbolleiste auf **Überprüfen**, um die Pipeline zu überprüfen.

Sie können den JSON-Code der Pipeline auch anzeigen, indem Sie oben rechts auf „Code“ klicken.

## <a name="publish-the-pipeline"></a>Veröffentlichen der Pipeline

Wählen Sie auf der oberen Symbolleiste **Publish All** (Alle veröffentlichen). Mit dieser Aktion werden erstellte Entitäten (Datasets und Pipelines) in Data Factory veröffentlicht.

:::image type="content" source="./media/load-office-365-data/publish-changes.png" alt-text="Veröffentlichen von Änderungen"::: 

## <a name="trigger-the-pipeline-manually"></a>Manuelles Auslösen der Pipeline

Wählen Sie in der Symbolleiste die Option **Trigger hinzufügen** und dann **Jetzt auslösen**. Klicken Sie auf der Seite „Pipeline Run“ (Pipelineausführung) auf **Fertig stellen**. 

## <a name="monitor-the-pipeline"></a>Überwachen der Pipeline

Wechseln Sie links zur Registerkarte **Überwachen**. Sie sehen eine Pipelineausführung, die von einem manuellen Trigger ausgelöst wird. Sie können über Links in der Spalte **Aktionen** Aktivitätsdetails anzeigen und die Pipeline erneut ausführen.

:::image type="content" source="./media/load-office-365-data/pipeline-status.png" alt-text="Überwachen der Pipeline"::: 

Klicken Sie in der Spalte „Aktionen“ auf den Link **View Activity Runs** (Aktivitätsausführungen anzeigen), um mit der Pipelineausführung verknüpfte Aktivitätsausführungen anzuzeigen. Da in diesem Beispiel nur eine Aktivität vorhanden ist, wird in der Liste nur ein Eintrag angezeigt. Wenn Sie Details zum Kopiervorgang anzeigen möchten, wählen Sie in der Spalte „Aktionen“ den Link **Details** (Brillensymbol) aus.

:::image type="content" source="./media/load-office-365-data/activity-status.png" alt-text="Überwachen der Aktivität"::: 

Wenn Sie erstmals Daten für diesen Kontext anfordern (also für eine Kombination daraus, auf welche Datentabelle zugegriffen wird, in welches Zielkonto die Daten geladen werden und welche Benutzeridentität die Datenzugriffsanforderung sendet), wird als Status der Kopieraktivität **In Bearbeitung** angezeigt. Nur wenn Sie unter „Aktionen“ auf den Link „Details“ klicken, wird als Status **RequestingConsent** angezeigt.  Ein Mitglied aus der Gruppe der Datenzugriffsgenehmiger muss die Anforderung in Privileged Access Management genehmigen, bevor die Datenextraktion fortgesetzt werden kann.

_Status, in dem Zustimmung angefordert wird:_ 
:::image type="content" source="./media/load-office-365-data/activity-details-request-consent.png" alt-text="Details zur Aktivitätsausführung: Anfordern der Zustimmung"::: 

_Status, in dem Daten extrahiert werden:_

:::image type="content" source="./media/load-office-365-data/activity-details-extract-data.png" alt-text="Details zur Aktivitätsausführung: Extrahieren der Daten"::: 

Nach dem Erteilen der Zustimmung wird die Datenextraktion fortgesetzt, und nach einiger Zeit wird die Pipelineausführung als erfolgreich angezeigt.

:::image type="content" source="./media/load-office-365-data/pipeline-monitoring-succeeded.png" alt-text="Überwachen der Pipeline: Erfolgreich"::: 

Navigieren Sie nun zur Azure Blob Storage-Zielinstanz, und überprüfen Sie, ob die Office 365-Daten im Binärformat extrahiert wurden.

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie den folgenden Artikel, um mehr über die Unterstützung von Azure Synapse Analytics zu erfahren: 

> [!div class="nextstepaction"]
>[Office 365-Connector](connector-office-365.md)