---
title: Tutorial zum Aktivieren eines Azure Stack Edge Pro-Geräts mit GPU im Azure-Portal | Microsoft-Dokumentation
description: Im Tutorial zur Bereitstellung von Azure Stack Edge Pro mit GPU erfahren Sie, wie Sie Ihr physisches Gerät aktivieren.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 07/14/2021
ms.author: alkohli
ms.openlocfilehash: fe1397b2853e95af715e4feb8423f7db3cf548f0
ms.sourcegitcommit: 192444210a0bd040008ef01babd140b23a95541b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/15/2021
ms.locfileid: "114219966"
---
# <a name="tutorial-activate-azure-stack-edge-pro-with-gpu"></a>Tutorial: Aktivieren von Azure Stack Edge Pro mit GPU

In diesem Tutorial wird erläutert, wie Sie Ihr Azure Stack Edge Pro-Gerät mit eingebauter GPU über die lokale Webbenutzeroberfläche aktivieren.

Der Aktivierungsvorgang dauert ungefähr 5 Minuten.

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Voraussetzungen
> * Aktivieren des physischen Geräts

## <a name="prerequisites"></a>Voraussetzungen

Überprüfen Sie Folgendes, bevor Sie ein Azure Stack Edge Pro-Gerät mit GPU konfigurieren und einrichten:

* Für Ihr physisches Gerät: 
    
    - Sie haben Ihr physisches Gerät gemäß der Anleitung unter [Installieren von Azure Stack Edge Pro](azure-stack-edge-gpu-deploy-install.md) installiert.
    - Sie haben die Netzwerk- und Computenetzwerkeinstellungen gemäß [Konfigurieren von Netzwerk, Computenetzwerk und Webproxy](azure-stack-edge-gpu-deploy-configure-network-compute-web-proxy.md) konfiguriert.
    - Sie haben Ihre eigenen Gerätezertifikate hochgeladen oder auf Ihrem Gerät generiert, sofern Sie den Gerätenamen oder die DNS-Domäne auf der Seite **Gerät** geändert haben. Falls Sie diesen Schritt nicht durchgeführt haben, wird während der Aktivierung des Geräts ein Fehler angezeigt, und die Aktivierung wird blockiert. Weitere Informationen finden Sie unter [Konfigurieren von Zertifikaten](azure-stack-edge-gpu-deploy-configure-certificates.md).
    
* Sie verfügen über den Aktivierungsschlüssel aus dem Azure Stack Edge-Dienst, den Sie zum Verwalten des Azure Stack Edge Pro-Geräts erstellt haben. Weitere Informationen finden Sie unter [Vorbereiten der Bereitstellung von Azure Stack Edge Pro](azure-stack-edge-gpu-deploy-prep.md).


## <a name="activate-the-device"></a>Aktivieren des Geräts

1. Navigieren Sie auf der lokalen Webbenutzeroberfläche des Geräts zur Seite **Erste Schritte**.
2. Wählen Sie auf der Kachel **Aktivierung** die Option **Aktivieren** aus. 

    ![Seite „Clouddetails“ auf der lokalen Webbenutzeroberfläche](./media/azure-stack-edge-gpu-deploy-activate/activate-1.png)
    
3. Geben Sie im Bereich **Aktivieren** den **Aktivierungsschlüssel** ein, den Sie unter [Aktivierungsschlüssel für Azure Stack Edge Pro abrufen](azure-stack-edge-gpu-deploy-prep.md#get-the-activation-key) abgerufen haben.

4. Wählen Sie **Übernehmen**.

    ![Seite „Clouddetails“ auf der lokalen Webbenutzeroberfläche 2](./media/azure-stack-edge-gpu-deploy-activate/activate-2.png)


5. Zunächst wird das Gerät aktiviert. Sie werden dann aufgefordert, die Schlüsseldatei herunterzuladen.
    
    ![Seite „Clouddetails“ auf der lokalen Webbenutzeroberfläche 3](./media/azure-stack-edge-gpu-deploy-activate/activate-3.png)
    
    Wählen Sie **Herunterladen und fortfahren** aus, und speichern Sie die Datei *device-serial-no.json* an einem sicheren Speicherort außerhalb des Geräts. **Diese Schlüsseldatei enthält die Wiederherstellungsschlüssel für den Betriebssystem-Datenträger und die anderen Datenträger auf Ihrem Gerät**. Diese Schlüssel werden möglicherweise für eine zukünftige Systemwiederherstellung benötigt.

    Nachfolgend ist der Inhalt der *json*-Datei gezeigt:

        
    ```json
    {
      "Id": "<Device ID>",
      "DataVolumeBitLockerExternalKeys": {
        "hcsinternal": "<BitLocker key for data disk>",
        "hcsdata": "<BitLocker key for data disk>"
      },
      "SystemVolumeBitLockerRecoveryKey": "<BitLocker key for system volume>",
      "ServiceEncryptionKey": "<Azure service encryption key>"
    }
    ```
        
 
    In der folgenden Tabelle sind die verschiedenen Schlüssel erläutert:
    
    |Feld  |BESCHREIBUNG  |
    |---------|---------|
    |`Id`    | Die ID des Geräts.        |
    |`DataVolumeBitLockerExternalKeys`|Die BitLocker-Schlüssel für die Datenträger, die zur Wiederherstellung der lokalen Daten auf Ihrem Gerät verwendet werden.|
    |`SystemVolumeBitLockerRecoveryKey`| Der BitLocker-Schlüssel für das Systemvolume. Dieser Schlüssel hilft bei der Wiederherstellung der Systemkonfiguration und der Systemdaten für Ihr Gerät. |
    |`ServiceEncryptionKey`| Dieser Schlüssel schützt die durch den Azure-Dienst übertragenen Daten. Dieser Schlüssel stellt sicher, dass eine Kompromittierung des Azure-Diensts nicht zu einer Kompromittierung gespeicherter Informationen führt. |

6. Wechseln Sie zur Seite **Übersicht**. Der Gerätestatus muss als **Aktiviert** angezeigt werden.

    ![Seite „Clouddetails“ auf der lokalen Webbenutzeroberfläche 4](./media/azure-stack-edge-gpu-deploy-activate/activate-4.png)
 
Die Aktivierung des Geräts ist abgeschlossen. Sie können nun Freigaben auf Ihrem Gerät hinzufügen.

Wenn während der Aktivierung Probleme auftreten, gehen Sie zu [Beheben von Aktivierungsproblemen auf Ihrem Azure Stack Edge Pro-GPU-Gerät](azure-stack-edge-gpu-troubleshoot-activation.md#activation-errors).



## <a name="deploy-workloads"></a>Bereitstellen von Workloads

Nachdem Sie das Gerät aktiviert haben, besteht der nächste Schritt im Bereitstellen der Workloads.

- Informationen über das Bereitstellen von VM-Workloads finden Sie unter [Was sind VMs in Azure Stack Edge?](azure-stack-edge-gpu-virtual-machine-overview.md) und in der zugehörigen Dokumentation zur VM-Bereitstellung.
- So stellen Sie Netzwerkfunktionen als verwaltete Anwendungen zur Verfügung:
    - Stellen Sie sicher, dass Sie eine Geräteressource für den Azure-Netzwerkfunktionsmanager (NFM) erstellen, die mit der Azure Stack Edge-Ressource verknüpft ist. Die Geräteressource aggregiert alle Netzwerkfunktionen, die auf dem Azure Stack Edge-Gerät bereitgestellt werden. Ausführliche Anweisungen finden Sie im [Tutorial: Erstellen einer Netzwerkfunktionsmanager-Geräteressource (Vorschauversion)](../network-function-manager/create-device.md). 
    - Anschließend können Sie den Netzwerkfunktionsmanager in Übereinstimmung mit den Anweisungen im [Tutorial: Bereitstellen von Netzwerkfunktionen in Azure Stack Edge (Vorschauversion)](../network-function-manager/deploy-functions.md) bereitstellen.
- So stellen Sie IoT Edge- und Kubernetes-Workloads bereit:
    - Zunächst müssen Sie Compute wie im [Tutorial: Konfigurieren von Compute auf einem Azure Stack Edge Pro GPU-Gerät](azure-stack-edge-gpu-deploy-configure-compute.md) beschrieben konfigurieren. In diesem Schritt wird ein Kubernetes-Cluster erstellt, der als Hostingplattform für IoT Edge auf Ihrem Gerät fungiert. 
    - Nachdem ein Kubernetes-Cluster auf Ihrem Azure Stack Edge-Gerät erstellt wurde, können Sie die Anwendungsworkloads auf diesem Cluster mittels einer der folgenden Methoden bereitstellen:

        - Nativer Zugriff über `kubectl`
        - IoT Edge
        - Azure Arc
        
        Weitere Informationen über die Bereitstellung von Workloads finden Sie unter [Verwalten von Kubernetes-Workloads auf Ihrem Azure Stack Edge-Gerät](azure-stack-edge-gpu-kubernetes-workload-management.md).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Voraussetzungen
> * Aktivieren des physischen Geräts

Informationen zum Übertragen von Daten mit Ihrem Azure Stack Edge Pro-Gerät finden Sie im folgenden Artikel:

> [!div class="nextstepaction"]
> [Übertragen von Daten mit Azure Stack Edge Pro](./azure-stack-edge-gpu-deploy-add-shares.md)