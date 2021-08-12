---
title: ITSM-Connector in Log Analytics
description: Dieser Artikel bietet eine Übersicht über den ITSM-Connector (IT-Service-Management-Connector) sowie Informationen zur Verwendung dieser Lösung, um die ITSM-Arbeitselemente in Log Analytics zu überwachen und zu verwalten und um etwaige Probleme schnell zu lösen.
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 05/24/2018
ms.custom: references_regions
ms.openlocfilehash: 3097023c4b9d19379f44529615a10c100e794700
ms.sourcegitcommit: 1ee13b62c094a550961498b7a52d0d9f0ae6d9c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2021
ms.locfileid: "109838673"
---
# <a name="connect-azure-to-itsm-tools-by-using-it-service-management-solution"></a>Verbinden von Azure mit ITSM-Tools mithilfe der IT-Service-Management-Lösung

:::image type="icon" source="media/itsmc-overview/itsmc-symbol.png":::

Dieser Artikel bietet Informationen dazu, wie Sie den ITSM-Connector (ITSMC) in Log Analytics konfigurieren, um ITSM-Arbeitselemente zentral zu verwalten.

## <a name="add-it-service-management-connector"></a>Hinzufügen des ITSM-Connectors

Bevor Sie eine Verbindung herstellen können, müssen Sie den ITSM-Connector installieren.

1. Wählen Sie im Azure-Portal **Ressource erstellen** aus:

   ![Screenshot des Menüelements zum Erstellen einer Ressource](media/itsmc-overview/azure-add-new-resource.png)

2. Suchen Sie im Azure Marketplace nach **ITSM-Connector**. Wählen Sie anschließend **Erstellen** aus:

   ![Screenshot der Schaltfläche „Erstellen“ im Azure Marketplace](media/itsmc-overview/add-itsmc-solution.png)

3. Wählen Sie im Abschnitt **LA-Arbeitsbereich** den Log Analytics-Arbeitsbereich aus, in dem Sie den ITSM-Connector installieren möchten.
   > [!NOTE]
   > Die Installation des ITSM-Connectors in Log Analytics-Arbeitsbereichen ist nur in den folgenden Regionen möglich: „USA, Osten“, „USA, Westen 2“, „USA, Süden-Mitte“, „USA, Westen-Mitte“, „US Gov Arizona“, „US Gov Virginia“, „Kanada, Mitte“, „Europa, Westen“, „Vereinigtes Königreich, Süden“, „Asien, Südosten“, „Japan, Osten“, „Indien, Mitte“ und „Australien, Südosten“.

4. Wählen Sie im Abschnitt **Log Analytics-Arbeitsbereich** die Ressourcengruppe aus, in der Sie die Ressource für den ITSM-Connector erstellen möchten:

   ![Screenshot des Abschnitts „Log Analytics-Arbeitsbereich“](media/itsmc-overview/itsmc-solution-workspace.png)
   
   > [!NOTE]
   > Im Rahmen der laufenden Umstellung von der Microsoft Operations Management Suite (OMS) auf Azure Monitor werden OMS-Arbeitsbereiche nun *Log Analytics-Arbeitsbereiche* genannt.

5. Klicken Sie auf **OK**.

Nachdem die Ressource für den ITSM-Connector bereitgestellt wurde, wird rechts oben im Fenster eine Benachrichtigung angezeigt.

## <a name="create-an-itsm-connection"></a>Erstellen einer ITSM-Verbindung

Nach der Installation des ITSM-Connectors müssen Sie das ITSM-Tool für die Verbindung mit dem ITSM-Connector vorbereiten. Wählen Sie basierend auf dem ITSM-Produkt, mit dem Sie eine Verbindung herstellen, einen der folgenden Links aus, um Anweisungen zu erhalten:

- [ServiceNow](./itsmc-connections-servicenow.md)
- [System Center Service Manager](./itsmc-connections-scsm.md)
- [Cherwell](./itsmc-connections-cherwell.md)
- [Provance](./itsmc-connections-provance.md)

Führen Sie nach dem Vorbereiten des ITSM-Tools die folgenden Schritte aus, um eine Verbindung herzustellen

1. Suchen Sie in **Alle Ressourcen** nach **ServiceDesk(*IhrArbeitsbereichsname*)** :

   ![Screenshot der zuletzt verwendeten Ressourcen im Azure-Portal](media/itsmc-definition/create-new-connection-from-resource.png)

1. Wählen Sie im linken Bereich unter **Arbeitsbereichsdatenquellen** die Option **ITSM-Verbindungen** aus:

   ![Screenshot des Menüelements „ITSM-Verbindungen“](media/itsmc-overview/add-new-itsm-connection.png)

1. Wählen Sie **Verbindung hinzufügen** aus.

1. Geben Sie die Verbindungseinstellungen entsprechend dem von Ihnen verwendeten ITSM-Produkt an:

    - [ServiceNow](./itsmc-connections-servicenow.md)
    - [System Center Service Manager](./itsmc-connections-scsm.md)
    - [Cherwell](./itsmc-connections-cherwell.md)
    - [Provance](./itsmc-connections-provance.md)

   > [!NOTE]
   > Standardmäßig aktualisiert der ITSM-Connector die Konfigurationsdaten der Verbindung einmal alle 24 Stunden. Um die Daten Ihrer Verbindung bei von Ihnen vorgenommenen Änderungen oder Vorlagenupdates sofort zu aktualisieren, wählen Sie im Bereich Ihrer Verbindung die Schaltfläche **Synchronisieren** aus:
   >
   > ![Screenshot der Schaltfläche „Synchronisieren“ im Bereich „ITSM-Verbindungen“](media/itsmc-overview/itsmc-connections-refresh.png)

## <a name="create-itsm-work-items-from-azure-alerts"></a>Erstellen von ITSM-Arbeitselementen aus Azure-Warnungen

Nachdem Sie die ITSM-Verbindung hergestellt haben, können Sie im ITSM-Tool Arbeitselemente basierend auf Azure-Warnungen erstellen. Sie verwenden für das Erstellen der Arbeitselemente die ITSM-Aktion in Aktionsgruppen.

Die wiederverwendbaren Aktionsgruppen bieten Ihnen die Möglichkeit, modular Aktionen für Ihre Azure-Warnungen auszulösen. Sie können Aktionsgruppen im Azure-Portal mit Metrikwarnungen, Aktivitätsprotokollwarnungen und Log Analytics-Warnungen verwenden.

> [!NOTE]
> Nachdem Sie die ITSM-Verbindung hergestellt haben, müssen Sie 30 Minuten warten, bis der Synchronisierungsvorgang abgeschlossen ist.

### <a name="define-a-template"></a>Definieren einer Vorlage

Für bestimmte Arbeitselementtypen können Sie im ITSM-Tool Vorlagen definieren. Mithilfe von Vorlagen können Sie Felder definieren, die automatisch entsprechend den festgelegten Werten für eine Aktionsgruppe aufgefüllt werden. Sie können definieren, welche Vorlage als Teil der Aktionsgruppendefinition verwendet werden soll. Informationen zum Erstellen von Vorlagen finden Sie in der ServiceNow-Dokumentation – (hier)[https://docs.servicenow.com/bundle/paris-platform-administration/page/administer/form-administration/task/t_CreateATemplateUsingTheTmplForm.html ].

So erstellen Sie eine Aktionsgruppe

1. Wählen Sie im Azure-Portal **Warnungen** aus.
2. Wählen Sie im Menü oben auf dem Bildschirm **Aktionen verwalten** aus:

    ![Screenshot des Menüelements „Aktionen verwalten“](media/itsmc-overview/action-groups-selection-big.png)

   Das Fenster **Aktionsgruppe erstellen** wird angezeigt.

3. Wählen Sie das **Abonnement** und die **Ressourcengruppe** aus, in dem bzw. der Sie die Aktionsgruppe erstellen möchten. Geben Sie einen **Aktionsgruppennamen** und einen **Anzeigenamen** für die Aktionsgruppe an. Wählen Sie anschließend **Next: Benachrichtigungen**.

    ![Screenshot des Fensters „Aktionsgruppe erstellen“](media/itsmc-overview/action-groups-details.png)

4. Wählen Sie auf der Registerkarte **Benachrichtigungen** die Option **Weiter: Aktionen**.
5. Wählen Sie auf der Registerkarte **Aktionen** den Eintrag **ITSM** in der Liste **Aktionstyp** aus. Geben Sie einen **Namen** für die Aktion an. Wählen Sie die Stiftschaltfläche aus, die für die Option **Details bearbeiten** steht.

    ![Screenshot der Auswahl für die Erstellung einer Aktionsgruppe](media/itsmc-definition/action-group-pen.png)

6. Wählen Sie in der Liste **Abonnement** das Abonnement aus, das den Log Analytics-Arbeitsbereich enthält. Wählen Sie in der Liste **Verbindung** den Namen Ihres ITSM-Connectors aus. Auf diesen folgt der Name Ihres Arbeitsbereichs. Beispiel: *MeinITSM-Connector(MeinArbeitsbereich)* .

7. Wählen Sie einen Typ für das **Arbeitselement** aus.

8. Im letzten Abschnitt der Schnittstelle zum Erstellen der ITSM-Aktionsgruppe können Sie angeben, wie viele Arbeitselemente für die einzelnen Warnungen erstellt werden sollen.

   > [!NOTE]
   > Dieser Abschnitt ist nur für Protokollsuchwarnungen relevant. Für alle anderen Warnungstypen erstellen Sie pro Warnung ein Arbeitselement.

   * Wenn Sie **Incident** oder **Warnung** in der Dropdownliste **Arbeitselement** ausgewählt haben, können Sie für jedes Konfigurationselement einzelne Arbeitselemente erstellen.
    
     ![Screenshot des Bereichs „ITSM-Ticket“ mit Arbeitselement „Incident“](media/itsmc-overview/itsm-action-configuration.png)
    
     * Wenn Sie das Kontrollkästchen **Einzelne Arbeitselemente für jedes Konfigurationselement erstellen** aktivieren, wird für jedes Konfigurationselement jeder Warnung ein neues Arbeitselement erstellt. Da für dieselben betroffenen Konfigurationselemente mehrere Warnungen auftreten, gibt es für jedes Konfigurationselement mehr als ein Arbeitselement.

       So werden beispielsweise für eine Warnung mit drei Konfigurationselementen drei Arbeitselemente erstellt. Für eine Warnung mit einem Konfigurationselement wird ein Arbeitselement erstellt.
        
     * Wenn Sie das Kontrollkästchen **Einzelne Arbeitselemente für jedes Konfigurationselement erstellen** deaktivieren, wird vom ITSM-Connector nur ein Arbeitselement für jede Warnungsregel erstellt und an alle betroffenen Konfigurationselemente angefügt. Ein neues Arbeitselement wird erstellt, falls das vorherige geschlossen wurde.

       >[!NOTE]
       > In diesem Fall werden für einige ausgelöste Warnungen im ITSM-Tool keine neuen Arbeitselemente generiert.

       So wird beispielsweise für eine Warnung mit drei Konfigurationselementen ein Arbeitselement erstellt. Wenn eine Warnung für dieselbe Warnungsregel wie im vorherigen Beispiel ein Konfigurationselement aufweist, wird dieses Konfigurationselement an die Liste der betroffenen Konfigurationselemente im erstellten Arbeitselement angefügt. Für eine Warnung einer anderen Warnungsregel mit einem Konfigurationselement wird ein Arbeitselement erstellt.

   * Wenn Sie den Eintrag **Ereignis** in der Dropdownliste **Arbeitselement** ausgewählt haben, können Sie für jeden Protokolleintrag bzw. für jedes Konfigurationselement einzelne Arbeitselemente erstellen.
    
     ![Screenshot des Bereichs „ITSM-Ticket“ mit Arbeitselement „Ereignis“](media/itsmc-overview/itsm-action-configuration-event.png)

     * Bei Auswahl von **Einzelne Arbeitselemente für jeden Protokolleintrag erstellen (Das Feld für das Konfigurationselement ist nicht ausgefüllt. Dies kann zu einer Menge an Arbeitselementen führen.)** wird ein Arbeitselement pro Zeile in den Suchergebnissen der Abfrage der Protokollsuchwarnungen erstellt. Die Beschreibungseigenschaft in den Nutzdaten des Arbeitselements enthält die Zeile aus den Suchergebnissen.
      
     * Wenn Sie das Kontrollkästchen **Einzelne Arbeitselemente für jedes Konfigurationselement erstellen** aktivieren, wird für jedes Konfigurationselement in jeder Warnung ein neues Arbeitselement erstellt. Jedes Konfigurationselement kann mehr als ein Arbeitselement im ITSM-System aufweisen. Diese Option entspricht dem Aktivieren des Kontrollkästchens, das angezeigt wird, nachdem Sie **Incident** als Arbeitselementtyp ausgewählt haben.
9. Als Teil der Aktionsdefinition können Sie vordefinierte Felder definieren, die konstante Werte als Teil der Nutzlast enthalten. Je nach Arbeitselementtyp gibt es drei Optionen, die als Teil der Nutzlast verwendet werden können:
    * **Keine**: Es wird eine reguläre Nutzlast für ServiceNow ohne zusätzliche vordefinierte Felder und Werte verwendet.
    * **Standardfelder verwenden**: Es wird eine Gruppe von Feldern und Werten verwendet, die automatisch als Teil der Nutzlast an ServiceNow gesendet werden. Diese Felder sind nicht flexibel, und die Werte sind in ServiceNow-Listen definiert.
    * **Gespeicherte Vorlagen aus ServiceNow verwenden**: Es wird eine vordefinierte Gruppe von Feldern und Werten verwendet, die als Teil einer Vorlagendefinition in ServiceNow definiert wurden. Wenn Sie die Vorlage bereits in ServiceNow definiert haben, können Sie sie über die Liste **Vorlagen** verwenden. Andernfalls können Sie die Vorlage in ServiceNow definieren. Ausführliche Informationen dazu finden Sie [hier](#define-a-template).

10. Klicken Sie auf **OK**.

Verwenden Sie beim Erstellen oder Bearbeiten einer Azure-Warnungsregel eine Aktionsgruppe mit einer ITSM-Aktion. Beim Auslösen der Warnung wird das Arbeitselement im ITSM-Tool erstellt bzw. aktualisiert.

> [!NOTE]
> Informationen zu den Preisen für ITSM-Aktionen finden Sie auf der [Seite mit der Preisübersicht](https://azure.microsoft.com/pricing/details/monitor/) für Aktionsgruppen.
>
> Das Feld für Kurzbeschreibungen in der Warnungsregeldefinition ist auf 40 Zeichen begrenzt, wenn Sie es über eine ITSM-Aktion senden.

## <a name="next-steps"></a>Nächste Schritte

* [Beheben von Problemen mit dem ITSM-Controller](./itsmc-resync-servicenow.md)
