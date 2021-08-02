---
title: 'Konfigurieren einer GPU für Azure Virtual Desktop (klassisch): Azure'
description: Erfahren Sie, wie Sie durch GPU beschleunigtes Rendering und schnellere Codierung in Azure Virtual Desktop (klassisch) ermöglichen.
author: gundarev
ms.topic: how-to
ms.date: 03/30/2020
ms.author: denisgun
ms.openlocfilehash: 0ae012b1c8084c0f5f518a7e7006d7e5df9cdf4c
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111750063"
---
# <a name="configure-graphics-processing-unit-gpu-acceleration-for-azure-virtual-desktop-classic"></a>Konfigurieren der Beschleunigung durch Graphics Processing Units (GPUs) für Azure Virtual Desktop (klassisch)

>[!IMPORTANT]
>Dieser Inhalt gilt für Azure Virtual Desktop (klassisch). Der Dienst unterstützt keine Azure Virtual Desktop-Objekte in Azure Resource Manager. Wenn Sie Azure Virtual Desktop-Objekte in Azure Resource Manager verwalten möchten, helfen Ihnen die Informationen in [diesem Artikel](../configure-vm-gpu.md) weiter.

Azure Virtual Desktop unterstützt durch GPU beschleunigtes Rendern und Codieren, um die Leistung und Skalierbarkeit von Apps zu erhöhen. Die GPU-Beschleunigung ist besonders für grafikintensive Apps essentiell.

Anhand der Anweisungen in diesem Artikel können Sie eine GPU-optimierte Azure-VM erstellen, ihrem Hostpool hinzufügen und so konfigurieren, dass sie die GPU-Beschleunigung zum Rendern und Codieren verwendet. In diesem Artikel wird vorausgesetzt, dass Sie bereits einen Azure Virtual Desktop-Mandanten konfiguriert haben.

## <a name="select-a-gpu-optimized-azure-virtual-machine-size"></a>Auswählen einer GPU-optimierten VM-Größe

Azure bietet mehrere [GPU-optimierte VM-Größen](../../virtual-machines/sizes-gpu.md). Welche VM-Größe sich am besten für Ihren Hostpool eignet, hängt von mehreren Faktoren ab, z. B. von den Workloads Ihrer App, der gewünschten Benutzerfreundlichkeit und den Kosten. Grundsätzlich bieten größere und leistungsfähigere GPUs eine bessere Benutzerfreundlichkeit bei einer bestimmten Benutzerdichte.

## <a name="create-a-host-pool-provision-your-virtual-machine-and-configure-an-app-group"></a>Erstellen eines Hostpools, Bereitstellen der VM und Konfigurieren einer App-Gruppe

Erstellen Sie einen neuen Hostpool mit der gewählten VM-Größe. Anweisungen dazu finden Sie unter: [Tutorial: Erstellen eines Hostpools mit Azure Marketplace](../create-host-pools-azure-marketplace.md).

Azure Virtual Desktop unterstützt das durch GPU beschleunigte Rendern und Codieren für die folgenden Betriebssysteme:

* Windows 10, Version 1511 und höher
* Windows Server 2016 oder höher

Außerdem müssen Sie eine App-Gruppe konfigurieren oder die Standarddesktop-App-Gruppe „Desktop Application Group“ verwenden, die automatisch erstellt wird, wenn Sie einen neuen Hostpool erstellen. Anweisungen finden Sie unter [Tutorial: Verwalten von App-Gruppen für Azure Virtual Desktop](../manage-app-groups.md).

## <a name="install-supported-graphics-drivers-in-your-virtual-machine"></a>Installieren unterstützter Grafiktreiber auf Ihrer VM

Nach der Bereitstellung müssen Sie die entsprechenden Grafiktreiber installieren, um die GPU-Funktionen von Azure-VMs der N-Serie in Azure Virtual Desktop nutzen zu können. Befolgen Sie die Anweisungen unter [Unterstützte Betriebssysteme und Treiber](../../virtual-machines/sizes-gpu.md#supported-operating-systems-and-drivers), um Treiber vom entsprechenden Grafikanbieter entweder manuell oder mithilfe einer Azure-VM-Erweiterung zu installieren.

Nur von Azure angebotene Treiber werden für Azure Virtual Desktop unterstützt. Außerdem werden für Azure-VMs mit NVIDIA-GPUs nur [NVIDIA GRID-Treiber](../../virtual-machines/windows/n-series-driver-setup.md#nvidia-grid-drivers) für Azure Virtual Desktop unterstützt.

Wenn die Treiber installiert wurden, muss die VM neu gestartet werden. Überprüfen Sie anhand der oben aufgelisteten Schritte, dass die Grafiktreiber erfolgreich installiert wurden.

## <a name="configure-gpu-accelerated-app-rendering"></a>Konfigurieren des durch GPU beschleunigten App-Rendering

Apps und Desktops, die mit Konfigurationen für mehrere Sitzungen ausgeführt werden, werden standardmäßig mit CPUs gerendert und nutzen nicht die verfügbaren GPUs. Konfigurieren Sie eine Gruppenrichtlinie für den Sitzungshost, um durch GPU beschleunigtes Rendering zu aktivieren:

1. Stellen Sie mit einem Konto mit lokalen Administratorrechten eine Verbindung mit dem Desktop der VM her.
2. Öffnen Sie das Startmenü, und geben Sie „gpedit.msc“ ein, um den Gruppenrichtlinien-Editor zu öffnen.
3. Navigieren Sie zu **Computer Configuration** > **Administrative Templates** > **Windows Components** > **Remote Desktop Services** > **Remote Desktop Session Host** > **Remote Session Environment** (Computerkonfiguration > Administrative Vorlagen > Windows-Komponenten > Remotedesktopdienste > Remotedesktop-Sitzungshost > Remotesitzungsumgebung).
4. Wählen Sie die Richtlinie **Standard-Hardwaregrafikadapter für alle Remotedesktopdienste-Sitzungen verwenden** aus, und **aktivieren Sie diese**, um das GPU-Rendering in der Remotesitzung zu aktivieren.

## <a name="configure-gpu-accelerated-frame-encoding"></a>Konfigurieren der durch GPU beschleunigten Framecodierung

Der Remotedesktop codiert alle Grafiken, die von Apps und Desktops gerendert werden für die Übertragung an Remotedesktopclients (unabhängig davon, ob diese mit GPU oder CPU gerendert wurden). Standardmäßig nutzt der Remotedesktop die verfügbaren GPUs nicht für diese Codierung. Konfigurieren Sie eine Gruppenrichtlinie für den Sitzungshost, um durch GPU beschleunigte Framecodierung zu aktivieren. Führen Sie ähnlich wie oben die folgenden Schritte durch:

1. Wählen Sie die Richtlinie **H.264/AVC 444-Grafikmodus für Remotedesktopverbindungen priorisieren** aus, und **aktivieren Sie diese**, um den H.264/AVC 444-Codec in der Remotesitzung zu erzwingen.
2. Wählen Sie die Richtlinie **H.264/AVC-Hardwarecodierung für Remotedesktopverbindungen konfigurieren** aus, und **aktivieren Sie diese**, um die Hardwarecodierung für H.264/AVC in der Remotesitzung zu aktivieren.

    >[!NOTE]
    >Legen Sie in Windows Server 2016 die Option **AVC-Hardwarecodierung bevorzugen** auf **Immer versuchen** fest.

3. Erzwingen Sie ein Gruppenrichtlinienupdate, nachdem Sie die Gruppenrichtlinien bearbeitet haben. Öffnen Sie die Eingabeaufforderung, und geben Sie Folgendes ein:

    ```batch
    gpupdate.exe /force
    ```

4. Melden Sie sich bei der Remotedesktopsitzung ab.

## <a name="verify-gpu-accelerated-app-rendering"></a>Überprüfen des durch GPU beschleunigten App-Renderings

Führen Sie einen der folgenden Schritte durch, um zu überprüfen, dass Ihre Apps die GPUs für das Rendering verwenden:

* Verwenden Sie für Azure-VMs mit NVIDIA-GPUs das Hilfsprogramm `nvidia-smi`, um die GPU-Nutzung beim Ausführen Ihrer Apps zu überprüfen. Dies unter [Überprüfen der Treiberinstallation](../../virtual-machines/windows/n-series-driver-setup.md#verify-driver-installation) beschrieben.
* In unterstützten Betriebssystemversionen können Sie die GPU-Nutzung über den Task-Manager prüfen. Wählen Sie auf der Registerkarte „Leistung“ die GPU aus, um zu prüfen, ob diese von Apps genutzt wird.

## <a name="verify-gpu-accelerated-frame-encoding"></a>Überprüfen der durch GPU beschleunigten Framecodierung

So können Sie überprüfen, ob der Remotedesktop die durch GPU beschleunigte Codierung verwendet:

1. Stellen Sie eine Verbindung mit dem Desktop der VM mithilfe des Azure Virtual Desktop-Clients her.
2. Starten Sie die Ereignisanzeige, und navigieren Sie zum folgenden Knoten: **Applications and Services Logs** > **Microsoft** > **Windows** > **RemoteDesktopServices-RdpCoreCDV** > **Operational** (Anwendungs- und Dienstprotokolle > Microsoft > Windows > RemoteDesktopServices-RdpCoreCDV > Betriebsbereit)
3. Suchen Sie nach der Ereignis-ID 170, um zu überprüfen, ob die durch GPU beschleunigte Codierung verwendet wird. Wenn „Der AVC-Hardware-Encoder ist aktiviert: 1“ angezeigt wird, wird die GPU-Codierung verwendet.
4. Suchen Sie nach der Ereignis-ID 162, um zu überprüfen, ob der AVC 444-Modus aktiviert ist: Wenn „AVC aktiviert: 1, Anfangsprofil: 2048“ angezeigt wird, wird AVC 444 verwendet.

## <a name="next-steps"></a>Nächste Schritte

Anhand der beschriebenen Anweisungen sollten Sie nun die GPU-Beschleunigung auf einem Sitzungshost (einer VM) verwenden können. Für das Verwenden der GPU-Beschleunigung für einen größeren Hostpool können Sie folgende Aspekte in Erwägung ziehen:

* Ziehen Sie die Verwendung einer [VM-Erweiterung](../../virtual-machines/extensions/overview.md) in Betracht, um die Treiberinstallation und Updates für mehrere VMs zu vereinfachen. Verwenden Sie die [NVIDIA-GPU-Treibererweiterung](../../virtual-machines/extensions/hpccompute-gpu-windows.md) für VMs mit NVIDIA-GPUs, und verwenden Sie die AMD-GPU-Treibererweiterung für VMs mit AMD-GPUs.
* Die Verwendung einer Active Directory Domain Services-Gruppenrichtlinie, um die Konfiguration von Gruppenrichtlinien für mehrere VMs zu vereinfachen. Weitere Informationen zur Bereitstellung von Gruppenrichtlinien in der AD DS-Domäne finden Sie unter [Working with Group Policy Objects (Verwenden von Gruppenrichtlinienobjekten)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731212(v=ws.11)).