---
title: 'Tutorial: Micro-Fulfillment-Center-App-Vorlage | Microsoft-Dokumentation'
description: Ein Tutorial zur Micro-Fulfillment-Center-Anwendungsvorlage für Azure IoT Central.
author: avneet723
ms.author: avneets
ms.service: iot-central
ms.subservice: iot-central-retail
ms.topic: tutorial
ms.date: 01/09/2020
ms.openlocfilehash: d70a15456ecc688b6bc8137223d36164545c0123
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111956102"
---
# <a name="tutorial-deploy-and-walk-through-a-micro-fulfillment-center-application-template"></a>Tutorial: Bereitstellen und Durchlaufen einer Anwendungsvorlage für Micro-Fulfillment-Center

In diesem Tutorial wird mithilfe der Micro-Fulfillment-Center-Anwendungsvorlage für Azure IoT Central eine Einzelhandelslösung erstellt.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Bereitstellen der Anwendungsvorlage
> * Verwenden der Anwendungsvorlage

## <a name="prerequisites"></a>Voraussetzungen
Für diese Tutorialreihe benötigen Sie ein Azure-Abonnement. Optional können Sie eine kostenlose 7-Tage-Testversion verwenden. Wenn Sie kein Azure-Abonnement besitzen, können Sie auf der Seite [Azure-Anmeldeseite](https://aka.ms/createazuresubscription) eines erstellen.

## <a name="create-an-application"></a>Erstellen einer Anwendung 
In diesem Abschnitt erstellen Sie eine neue Azure IoT Central-Anwendung aus einer Vorlage. Sie verwenden diese Anwendung in der gesamten Tutorialreihe, um eine umfassende Lösung zu erstellen.

So erstellen Sie eine neue Azure IoT Central-Anwendung:

1. Navigieren Sie zur [Anwendungs-Manager-Website von Azure IoT Central](https://aka.ms/iotcentral).
1. Wenn Sie über ein Azure-Abonnement verfügen, können Sie sich mit den zugehörigen Anmeldeinformationen anmelden. Melden Sie sich andernfalls mit einem Microsoft-Konto an:

   ![Screenshot: Dialogfeld für die Anmeldung mit einem Microsoft-Konto](./media/tutorial-in-store-analytics-create-app/sign-in.png)

1. Wählen Sie **Neue Anwendung** aus, um mit der Erstellung einer neuen Azure IoT Central-Anwendung zu beginnen.

1. Wählen Sie **Einzelhandel** aus.  Auf der Seite „Einzelhandel“ werden verschiedene Vorlagen für Einzelhandelsanwendungen angezeigt.

So erstellen Sie eine neue Micro-Fulfillment-Center-Anwendung mit Previewfunktionen:  
1. Wählen Sie die Anwendungsvorlage **Micro-Fulfillment-Center** aus. Diese Vorlage enthält Gerätevorlagen für alle im Tutorial verwendeten Geräte. Darüber hinaus stellt die Vorlage ein Operatordashboard zur Überwachung von Bedingungen innerhalb Ihres Fulfillment-Centers sowie zur Überwachung der Bedingungen für Ihre Transportroboter bereit. 

    ![Screenshot: Seite „Eigene IoT-Anwendung erstellen“ in Azure IoT Central](./media/tutorial-micro-fulfillment-center-app/iotc-retail-homepage-mfc.png)
    
1. Wählen Sie optional einen benutzerfreundlichen **Anzeigenamen** aus. Die Anwendungsvorlage basiert auf dem fiktiven Unternehmen Northwind Traders. 

    >[!NOTE]
    >Wenn Sie einen benutzerfreundlichen Anwendungsnamen verwenden, müssen Sie trotzdem einen eindeutigen Wert für die Anwendungs-URL angeben.

1. Falls Sie über ein Azure-Abonnement verfügen, geben Sie Ihr Verzeichnis, Ihr Azure-Abonnement und Ihre Region ein. Andernfalls können Sie eine siebentägige kostenlose Testversion aktivieren und die erforderlichen Kontaktinformationen eingeben.  

1. Klicken Sie auf **Erstellen**.

    ![Screenshot: Seite „Neue Anwendung“ in Azure IoT Central](./media/tutorial-micro-fulfillment-center-app/iotc-retail-create-app-mfc.png)

    ![Screenshot: Abrechnungsinformationen in Azure IoT Central](./media/tutorial-micro-fulfillment-center-app/iotc-retail-create-app-mfc-billing.png)

## <a name="walk-through-the-application"></a>Einführung in die Anwendung 

Nach erfolgreicher Bereitstellung der App-Vorlage wird das **Micro-Fulfillment-Center-Dashboard für Northwind Traders** angezeigt. Northwind Traders ist ein fiktiver Einzelhändler, der über ein in dieser Azure IoT Central-Anwendung verwaltetes Micro-Fulfillment-Center verfügt. Dieses Operatordashboard enthält Informationen und Telemetriedaten zu den Geräten in dieser Vorlage sowie die verfügbaren Befehle, Aufträge und Aktionen. Das Dashboard ist logisch in zwei Abschnitte unterteilt. Auf der linken Seite können Sie die Umgebungsbedingungen innerhalb der Fulfillment-Struktur und auf der rechten Seite den Status eines Transportroboters in Ihrer Einrichtung überwachen.  

Das Dashboard ermöglicht Folgendes:
   * Anzeigen von Gerätetelemetriedaten (etwa die Anzahl von Entnahmen und verarbeiteten Aufträgen) und Eigenschaften (etwa den Struktursystemstatus)  
   * Anzeigen des Grundrisses und der Position der Transportroboter innerhalb der Fulfillment-Struktur
   * Auslösen von Befehlen – etwa zum Zurücksetzen des Steuerungssystems, zum Aktualisieren der Firmwareversion eines Transportroboters oder zum Ändern der Netzwerkkonfiguration

     ![Screenshot: Obere Hälfte des Dashboards des Micro-Fulfillment-Centers von Northwind Traders](./media/tutorial-micro-fulfillment-center-app/mfc-dashboard-1.png)
   * Betrachten eines Beispiels für das Dashboard, über das ein Operator die Bedingungen innerhalb des Fulfillment-Centers überwachen kann 
   * Überwachen der Integrität der Nutzlasten, die auf dem Gatewaygerät innerhalb des Fulfillment-Centers ausgeführt werden    

     ![Screenshot: Untere Hälfte des Dashboards des Micro-Fulfillment-Centers von Northwind Traders](./media/tutorial-micro-fulfillment-center-app/mfc-dashboard-2.png)

## <a name="device-template"></a>Gerätevorlage
Wenn Sie die Registerkarte „Gerätevorlagen“ auswählen, werden für die Vorlage zwei unterschiedliche Gerätetypen angezeigt: 
   * **Robotic Carrier** (Transportroboter): Diese Gerätevorlage stellt die Definition eines funktionierenden Transportroboters dar, der in der Fulfillment-Struktur eingesetzt wird und ordnungsgemäß funktioniert. Wenn Sie die Vorlage auswählen, sehen Sie, dass von dem Roboter Gerätedaten wie Temperatur und Achsenposition sowie Eigenschaften wie etwa der Status des Transportroboters gesendet werden. 
   * **Structure Condition Monitoring** (Überwachung der Strukturbedingungen): Diese Gerätevorlage stellt eine Sammlung von Geräten dar, mit deren Hilfe Sie sowohl Umgebungsbedingungen als auch das Gatewaygerät überwachen können, von dem verschiedene Edgeworkloads für den Betrieb Ihres Fulfillment-Centers gehostet werden. Das Gerät sendet Telemetriedaten wie etwa die Temperatur, die Anzahl von Entnahmen und die Anzahl von Aufträgen. Darüber hinaus sendet es Informationen zum Zustand und zur Integrität der in Ihrer Umgebung ausgeführten Computeworkloads. 

     ![Gerätevorlagen für das Micro-Fulfillment-Center](./media/tutorial-micro-fulfillment-center-app/device-templates.png)

Wenn Sie die Registerkarte „Gerätegruppen“ auswählen, sehen Sie auch, dass für diese Gerätevorlagen automatisch Gerätegruppen erstellt werden.

## <a name="rules"></a>Regeln
Auf der Registerkarte **Regeln** sehen Sie eine Beispielregel, die in der Anwendungsvorlage vorhanden ist, um die Temperaturbedingungen für den Transportroboter zu überwachen. Diese Regel kann verwendet werden, um den Operator zu benachrichtigen, wenn ein bestimmter Roboter in der Einrichtung zu heiß wird und zu Wartungszwecken offline geschaltet werden muss. 

Nutzen Sie die Beispielregel als Orientierungshilfe, um Regeln zu definieren, die besser zu Ihren geschäftlichen Funktionen passen.

![Screenshot: Registerkarte „Regeln“](./media/tutorial-micro-fulfillment-center-app/rules.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie diese Anwendung nicht mehr benötigen, löschen Sie die Anwendungsvorlage. Navigieren Sie zu **Verwaltung** > **Anwendungseinstellungen**, und wählen Sie **Löschen** aus.

![Screenshot: Seite „Anwendungseinstellungen“ für das Micro-Fulfillment-Center](./media/tutorial-micro-fulfillment-center-app/delete.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen:

> [!div class="nextstepaction"]
> [Micro-Fulfillment-Center-Lösungsarchitektur](./architecture-micro-fulfillment-center.md)
