---
title: Azure Stack Edge Pro-FPGA-Computeverwaltung
description: Hier wird beschrieben, wie Sie über das Azure-Portal auf Ihrem Azure Stack Edge Pro-FPGA-Gerät die Edgecomputingeinstellungen wie Trigger und Module verwalten, die Computekonfiguration anzeigen und die Konfiguration entfernen.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 01/06/2021
ms.author: alkohli
ms.openlocfilehash: 80d19909cc4a2fe843adbf19eed0f8316d64f6db
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110460699"
---
# <a name="manage-compute-on-your-azure-stack-edge-pro-fpga"></a>Verwalten von Computeressourcen auf Ihrem Azure Stack Edge Pro-FPGA-Gerät

In diesem Artikel wird beschrieben, wie Sie Computeressourcen auf Ihrem Azure Stack Edge Pro-FPGA-Gerät verwalten. Die Computekonfiguration kann über das Azure-Portal oder über die lokale Webbenutzeroberfläche verwaltet werden. Verwenden Sie das Azure-Portal, um Module, Trigger und die Computekonfiguration zu verwalten, und die lokale Webbenutzeroberfläche, um Computeeinstellungen zu verwalten.

In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Verwalten von Triggern
> * Verwalten der Computekonfiguration


## <a name="manage-triggers"></a>Verwalten von Triggern

Ein Ereignis ist etwas, das in Ihrer Cloudumgebung oder auf Ihrem Gerät passiert und ggf. eine Aktion erfordert. Die Erstellung einer Datei in einer Freigabe ist beispielsweise ein Ereignis. Die Ereignisse werden von Triggern ausgelöst. Für Ihr Azure Stack Edge Pro-FPGA-Gerät können Trigger verwendet werden, um auf Dateiereignisse zu reagieren oder einem Zeitplan zu folgen.

- **Datei:** Mit diesen Triggern kann auf Dateiereignisse reagiert werden (etwa auf die Erstellung oder Änderung einer Datei).
- **Scheduled**: Mit diesen Triggern kann ein Zeitplan mit Startzeit (Datum/Uhrzeit) und Wiederholungsintervall implementiert werden.


### <a name="add-a-trigger"></a>Hinzufügen eines Triggers

Gehen Sie im Azure-Portal wie folgt vor, um einen Trigger zu erstellen:

1. Navigieren Sie im Azure-Portal zu Ihrer Azure Stack Edge-Ressource und anschließend zu **IoT Edge > Trigger**. Wählen Sie auf der Befehlsleiste die Option **+ Trigger hinzufügen** aus.

    ![Auswählen von „Trigger hinzufügen“](media/azure-stack-edge-manage-compute/add-trigger-1.png)

2. Geben Sie auf dem Blatt **Trigger hinzufügen** einen eindeutigen Namen für den Trigger an.
    
    <!--Trigger names can only contain numbers, lowercase letters, and hyphens. The share name must be between 3 and 63 characters long and begin with a letter or a number. Each hyphen must be preceded and followed by a non-hyphen character.-->

3. Wählen Sie unter **Typ** einen Typ für den Trigger aus. Wenn der Trigger als Reaktion auf ein Dateiereignis verwendet werden soll, wählen Sie **Datei** aus. Wenn der Trigger zu einer bestimmten Zeit gestartet und mit einem bestimmten Wiederholungsintervall ausgeführt werden soll, wählen Sie **Geplant** aus. Die daraufhin angezeigten Optionen hängen von der getroffenen Auswahl ab.

    - **Dateitrigger:** Wählen Sie in der Dropdownliste eine eingebundene Freigabe aus. Wird in dieser Freigabe ein Ereignis ausgelöst, ruft der Trigger eine Azure-Funktion auf.

        ![Hinzufügen einer SMB-Freigabe](media/azure-stack-edge-manage-compute/add-file-trigger.png)

    - **Geplanter Trigger:** Geben Sie die Startzeit (Datum/Uhrzeit) und das Wiederholungsintervall in Stunden, Minuten oder Sekunden an. Geben Sie außerdem den Namen für ein Thema ein. Mit einem Thema können Sie den Trigger flexibel an ein auf dem Gerät bereitgestelltes Modul weiterleiten.

        Beispiel für eine Routenzeichenfolge: `"route3&quot;: &quot;FROM /* WHERE topic = 'topicname' INTO BrokeredEndpoint(&quot;modules/modulename/inputs/input1")"`.

        ![Hinzufügen einer NFS-Freigabe](media/azure-stack-edge-manage-compute/add-scheduled-trigger.png)

4. Wählen Sie **Hinzufügen** aus, um den Trigger zu erstellen. Eine Benachrichtigung informiert darüber, dass der Trigger erstellt wird. Nach Abschluss der Triggererstellung wird das Blatt mit dem neuen Trigger aktualisiert.
 
    ![Aktualisierte Triggerliste](media/azure-stack-edge-manage-compute/add-trigger-2.png)

### <a name="delete-a-trigger"></a>Löschen eines Triggers

Gehen Sie im Azure-Portal wie folgt vor, um einen Trigger zu löschen:

1. Wählen Sie in der Triggerliste den zu löschenden Trigger aus.

    ![Trigger auswählen](media/azure-stack-edge-manage-compute/delete-trigger-1.png)

2. Klicken Sie mit der rechten Maustaste darauf, und wählen Sie **Löschen** aus.

    ![Auswählen von „Löschen“](media/azure-stack-edge-manage-compute/delete-trigger-2.png)

3. Wenn Sie zur Bestätigung aufgefordert werden, klicken Sie auf **Ja**.

    ![Bestätigen des Löschens](media/azure-stack-edge-manage-compute/delete-trigger-3.png)

Die Triggerliste wird nach dem Löschvorgang entsprechend aktualisiert.

## <a name="manage-compute-configuration"></a>Verwalten der Computekonfiguration

Im Azure-Portal können Sie die Computekonfiguration anzeigen, eine vorhandene Computekonfiguration entfernen oder die Computekonfiguration aktualisieren, um Zugriffsschlüssel für das IoT-Gerät und das IoT Edge-Gerät für Ihr Azure Stack Edge Pro-FPGA zu aktualisieren.

### <a name="view-compute-configuration"></a>Anzeigen der Computekonfiguration

Gehen Sie im Azure-Portal wie folgt vor, um die Computekonfiguration für Ihr Gerät anzuzeigen:

1. Navigieren Sie im Azure-Portal zu Ihrer Azure Stack Edge-Ressource und dann zu **IoT Edge > Übersicht**. 

    ![Auswählen von „View compute“ (Compute anzeigen)](media/azure-stack-edge-manage-compute/view-compute-1.png)

2. Navigieren Sie zur Seite **Eigenschaften**. Notieren Sie sich die Computekonfiguration auf Ihrem Gerät. Wenn Sie Computeeinstellungen konfiguriert haben, haben Sie eine IoT Hub-Ressource erstellt. Unter dieser IoT Hub-Ressource sind ein IoT-Gerät und ein IoT Edge-Gerät konfiguriert. Auf dem IoT Edge-Gerät wird nur die Ausführung der Linux-Module unterstützt.

    ![Anzeigen der Konfiguration](media/azure-stack-edge-manage-compute/view-compute-2.png)


### <a name="remove-compute-configuration"></a>Entfernen der Computekonfiguration

Gehen Sie im Azure-Portal wie folgt vor, um die vorhandene Edgecomputingkonfiguration für Ihr Gerät zu entfernen:

1. Navigieren Sie im Azure-Portal zu Ihrer Azure Stack Edge-Ressource und dann zu **IoT Edge > Übersicht**. Wählen Sie auf der Befehlsleiste die Option **Entfernen** aus.

    ![Auswählen von „Compute entfernen“](media/azure-stack-edge-manage-compute/remove-compute-1.png)

2. Wenn Sie die Computekonfiguration entfernen, müssen Sie Ihr Gerät neu konfigurieren, um Compute wieder verwenden zu können. Wählen Sie **Ja** aus, wenn Sie zur Bestätigung aufgefordert werden.

    ![Auswählen von „Compute entfernen“ 2](media/azure-stack-edge-manage-compute/remove-compute-2.png)

### <a name="sync-up-iot-device-and-iot-edge-device-access-keys"></a>Synchronisieren der Zugriffsschlüssel von IoT-Gerät und IoT Edge-Gerät

Wenn Sie Compute auf Ihrem Azure Stack Edge Pro-FPGA konfigurieren, werden ein IoT-Gerät und ein IoT Edge-Gerät erstellt. Diesen Geräten werden automatisch symmetrische Zugriffsschlüssel zugewiesen. Diese Schlüssel werden aus Sicherheitsgründen regelmäßig über den IoT Hub-Dienst rotiert.

Um die Schlüssel zu rotieren, können Sie zu dem IoT Hub-Dienst wechseln, den Sie erstellt haben, und das IoT-Gerät oder das IoT Edge-Gerät auswählen. Jedes Gerät verfügt über einen primären und einen sekundären Zugriffsschlüssel. Weisen Sie den primären Zugriffsschlüssel dem sekundären Zugriffsschlüssel zu, und generieren Sie dann den primären Zugriffsschlüssel neu.

Wenn die Schlüssel Ihres IoT-Geräts und Ihres IoT Edge-Geräts rotiert wurden, müssen Sie die Konfiguration auf Ihrem Azure Stack Edge Pro-FPGA aktualisieren, um die neuesten Zugriffsschlüssel zu erhalten. Durch die Synchronisierung wird sichergestellt, dass Ihr Gerät über die neuesten Schlüssel für Ihr IoT-Gerät und Ihr IoT Edge-Gerät verfügt. Das Azure Stack Edge Pro-FPGA verwendet nur die primären Zugriffsschlüssel.

Gehen Sie im Azure-Portal wie folgt vor, um die Zugriffsschlüssel für Ihr Gerät zu synchronisieren:

1. Navigieren Sie im Azure-Portal zu Ihrer Azure Stack Edge-Ressource und dann zu **IoT Edge > Übersicht**. Wählen Sie auf der Befehlsleiste die Option **Konfiguration aktualisieren** aus.

    ![Auswählen von „Konfiguration aktualisieren“](media/azure-stack-edge-manage-compute/refresh-configuration-1.png)

2. Wählen Sie **Ja** aus, wenn Sie zur Bestätigung aufgefordert werden.

     ![Auswählen von „Ja“ bei entsprechender Aufforderung](media/azure-stack-edge-manage-compute/refresh-configuration-2.png)

3. Schließen Sie das Dialogfeld, wenn die Synchronisierung abgeschlossen ist.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum [Verwalten von Edgecomputingnetzwerken über das Azure-Portal](azure-stack-edge-extend-compute-access-modules.md).
