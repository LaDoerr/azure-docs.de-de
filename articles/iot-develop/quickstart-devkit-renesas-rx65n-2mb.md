---
title: 'Schnellstart: Verbinden eines Renesas RX65N-2MB mit Azure IoT Central'
description: Mithilfe von eingebetteter Azure RTOS-Software können Sie ein Renesas RX65N-2MB-Gerät mit Azure IoT verbinden und Telemetriedaten senden.
author: timlt
ms.author: timlt
ms.service: iot-develop
ms.devlang: c
ms.topic: quickstart
ms.date: 06/04/2021
ms.openlocfilehash: cf8f9a8e912c0f75abd4466b9efe532b2dbf622a
ms.sourcegitcommit: f9e368733d7fca2877d9013ae73a8a63911cb88f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111903028"
---
# <a name="quickstart-connect-a-renesas-starter-kit-for-rx65n-2mb-to-iot-central"></a>Schnellstart: Verbinden eines Renesas Starter Kit+ für RX65N-2MB mit IoT Central

**Gilt für**: [Entwicklung von eingebetteten Geräten](about-iot-develop.md#embedded-device-development)<br>
**Gesamtdauer**: 30 Minuten

[![Code durchsuchen](media/common/browse-code.svg)](https://github.com/azure-rtos/getting-started/tree/master/Renesas/RSK_RX65N_2MB)

In diesem Schnellstart verwenden Sie Azure RTOS, um das Renesas Starter Kit+ für RX65N-2MB (nachfolgend als „Renesas RX65N“ bezeichnet) mit Azure IoT zu verbinden.

Sie führen die folgenden Aufgaben aus:

* Installieren von eingebetteten Entwicklungstools für die Programmierung eines Renesas RX65N in C
* Erstellen eines Images und sein Einspielen per Flashvorgang auf dem Renesas RX65N
* Verwenden von Azure IoT Central zum Erstellen von Cloudkomponenten, Anzeigen von Eigenschaften und Gerätetelemetriedaten und Aufrufen von direkten Befehlen

## <a name="prerequisites"></a>Voraussetzungen

* PC mit Microsoft Windows 10
* [Git](https://git-scm.com/downloads) zum Klonen des Repositorys
* Hardware

    * Das [Renesas Starter Kit+ für RX65N-2MB](https://www.renesas.com/products/microcontrollers-microprocessors/rx-32-bit-performance-efficiency-mcus/rx65n-2mb-starter-kit-plus-renesas-starter-kit-rx65n-2mb) (Renesas RX65N)
    * Der [Renesas E2-Emulator Lite](https://www.renesas.com/software-tool/e2-emulator-lite-rte0t0002lkce00000r)
    * Kabel: 2 USB 2.0 A-Stecker zu Mini-USB-Stecker
    * Die enthaltene 5 V-Stromversorgung
    * Ethernetkabel
    * Kabelgebundener Ethernet-Zugriff

## <a name="prepare-the-development-environment"></a>Vorbereiten der Entwicklungsumgebung

Zum Einrichten Ihrer Entwicklungsumgebung klonen Sie zuerst ein GitHub-Repository, in dem alle für den Schnellstart benötigten Ressourcen enthalten sind. Anschließend installieren Sie einige Programmiertools.

### <a name="clone-the-repo-for-the-quickstart"></a>Klonen des Repositorys für den Schnellstart

Klonen Sie das folgende Repository, um den gesamten Beispielgerätecode, die Setupskripts und die Offlineversionen der Dokumentation herunterzuladen. Wenn Sie dieses Repository bereits bei einem anderen Schnellstart geklont haben, müssen Sie diesen Schritt nicht erneut ausführen.

Führen Sie den folgenden Befehl aus, um das Repository zu klonen:

```shell
git clone --recursive https://github.com/azure-rtos/getting-started.git
```

### <a name="install-the-tools"></a>Installieren der Tools

Das geklonte Repository enthält ein Setupskript, mit dem die erforderlichen Tools installiert und konfiguriert werden. Wenn Sie diese Tools in einem anderen eingebetteten Geräteschnellstart installiert haben, müssen Sie diesen Schritt nicht erneut ausführen.

> [!NOTE]
> Mit dem Setupskript werden die folgenden Tools installiert:
> * [CMake](https://cmake.org): Buildvorgang
> * [RX GCC](http://gcc-renesas.com/downloads/get.php?f=rx/8.3.0.202004-gnurx/gcc-8.3.0.202004-GNURX-ELF.exe): Kompiliervorgang
> * [Termite](https://www.compuphase.com/software_termite.htm): Überwachung der Ausgabe des seriellen Anschlusses für verbundene Geräte

So installieren Sie die Tools

1. Navigieren Sie im Datei-Explorer zum folgenden Pfad im Repository, und führen Sie das Setupskript mit dem Namen *get-toolchain.bat* aus:

    *getting-started\tools\get-toolchain.bat*

1. Fügen Sie den RX-Compiler dem Windows-Pfad hinzu:

    *%USERPROFILE%\AppData\Roaming\GCC for Renesas RX 8.3.0.202004-GNURX-ELF\rx-elf\rx-elf\bin*

1. Öffnen Sie nach der Installation ein neues Konsolenfenster, um die vom Setupskript vorgenommenen Konfigurationsänderungen zu überprüfen. Verwenden Sie diese Konsole zur Ausführung der restlichen Programmieraufgaben im Schnellstart. Sie können die Windows-Eingabeaufforderung, PowerShell oder Git Bash für Windows verwenden.
1. Führen Sie die folgenden Befehle aus, um zu bestätigen, dass CMake, Version 3.14 oder höher, installiert und der RX-Compilerpfad ordnungsgemäß eingerichtet wurde.

    ```shell
    cmake --version
    rx-elf-gcc
    ```
So installieren Sie die restlichen Tools:

* Installieren Sie [Renesas Flash Programmer](https://www.renesas.com/software-tool/renesas-flash-programmer-programming-gui). Der Renesas Flash-Programmierer enthält die Treiber und Tools, die zum Einspielen von Renesas RX65N per Flashvorgang über Renesas E2 Lite erforderlich sind.

[!INCLUDE [iot-develop-embedded-create-central-app-with-device](../../includes/iot-develop-embedded-create-central-app-with-device.md)]

## <a name="prepare-the-device"></a>Vorbereiten des Geräts

Zum Verbinden des Renesas RX65N mit Azure ändern Sie eine Konfigurationsdatei für WLAN- und Azure IoT-Einstellungen, erstellen das Image neu und spielen es per Flashvorgang auf dem Gerät ein.

### <a name="add-configuration"></a>Hinzufügen einer Konfiguration

1. Öffnen Sie die folgende Datei in einem Text-Editor:

    *getting-started\Renesas\RSK_RX65N_2MB\app\azure_config.h*

1. Legen Sie die Konstanten aus den Azure IoT-Geräteinformationen auf die Werte fest, die Sie nach dem Erstellen der Azure-Ressourcen gespeichert haben.

    |Konstantenname|Wert|
    |-------------|-----|
    |`IOT_DPS_ID_SCOPE` |{*Ihr Wert für den ID-Bereich*}|
    |`IOT_DPS_REGISTRATION_ID` |{*Ihr Wert für die Geräte-ID*}|
    |`IOT_DEVICE_SAS_KEY` |{*Ihr Wert für den Primärschlüssel*}|

1. Speichern und schließen Sie die Datei.

### <a name="build-the-image"></a>Erstellen des Image

1. Führen Sie in Ihrer Konsole oder im Datei-Explorer das Skript *rebuild.bat* unter dem folgenden Pfad aus, um das Image zu erstellen:

    *getting-started\Renesas\RSK_RX65N_2MB\tools\rebuild.bat*

2. Vergewissern Sie sich nach Abschluss des Buildvorgangs, dass die Binärdatei unter dem folgenden Pfad erstellt wurde:

    *getting-started\Renesas\RSK_RX65N_2MB\build\app\rx65n_azure_iot.hex*

### <a name="connect-the-device"></a>Verbinden des Geräts

> [!NOTE]
> Weitere Informationen zum Einrichten von Renesas RX65N und zu den ersten Schritten damit finden Sie unter [Renesas Starter Kit+ für RX65N-2MB – Schnellstart](https://www.renesas.com/document/man/e2studio-renesas-starter-kit-rx65n-2mb-quick-start-guide).

1. Führen Sie die folgenden Schritte aus, indem Sie die folgende Abbildung als Referenz verwenden.
    
    :::image type="content" source="media/quickstart-devkit-renesas-rx65n-2mb/renesas-rx65n.jpg" alt-text="Suchen von „Reset“, „Power“, „Ethernet“, „USB“ und „E1/E2Lite“ auf dem Renesas RX65N-Board":::

1. Verbinden Sie den **Netzeingang** auf dem Renesas RX65N über die 5 V-Stromversorgung mit einer Steckdose.

1. Verbinden Sie das **Ethernet** auf dem Renesas RX65N über das Ethernet-Kabel mit Ihrem Router.

1. Schließen Sie das **USB Serial** auf dem Renesas RX65N mit dem ersten Mini-USB-Kabel an Ihren Computer an.

1. Schließen Sie das **E2 Lite USB Serial** auf dem Renesas E2 Lite mit dem zweiten Mini-USB-Kabel an Ihren Computer an.

1. Verbinden Sie das **E1/E2Lite** auf dem Renesas RX65N mithilfe des bereitgestellten Flachbandkabels mit Renesas E2 Lite.

### <a name="flash-the-image"></a>Einspielen des Images

1. Starten Sie die Anwendung *Renesas Flash Programmer* über das Startmenü.

2. Wählen Sie im Menü *Datei* die Option *Neues Projekt...* aus, und geben Sie die folgenden Einstellungen ein:
    * **Microcontroller**: RX65x
    * **Projektname**: RX65N
    * **Tool**: E2-Emulator Lite

    :::image type="content" source="media/quickstart-devkit-renesas-rx65n-2mb/rfp-new.png" alt-text="Screenshot: Renesas Flash Programmer, „Neues Projekt“":::

3. Wählen Sie die Schaltfläche *Tooldetails* aus, und navigieren Sie zur Registerkarte *Einstellungen zurücksetzen*.

4. Wählen Sie *Pin als Hi-Z zurücksetzen* aus, und klicken Sie auf die Schaltfläche *OK*.

    :::image type="content" source="media/quickstart-devkit-renesas-rx65n-2mb/rfp-reset.png" alt-text="Screenshot: Renesas Flash Programmer, „Einstellungen zurücksetzen“":::

5. Klicken Sie auf die Schaltfläche *Verbinden*, aktivieren Sie bei der entsprechenden Aufforderung das Kontrollkästchen *Automatische Authentifizierung*, und klicken Sie auf *OK*.

    :::image type="content" source="media/quickstart-devkit-renesas-rx65n-2mb/rfp-auth.png" alt-text="Screenshot: Renesas Flash Programmer, „Authentifizierung“":::

6. Wählen Sie die Schaltfläche *Durchsuchen...* aus, und suchen Sie die im vorherigen Abschnitt erstellte Datei *rx65n_azure_iot.hex*.

7. Klicken Sie auf *Start*, um mit dem Einspielen per Flashvorgang zu beginnen. Dieser Vorgang dauert ca. 10 Sekunden.

### <a name="confirm-device-connection-details"></a>Bestätigen der Details zur Geräteverbindung

Sie können die **Termite**-App verwenden, um die Kommunikation zu überwachen und sich zu vergewissern, dass Ihr Gerät richtig eingerichtet wurde.
> [!TIP]
> Wenn beim Initialisieren oder Verbinden Ihres Geräts nach dem Einspielen per Flashvorgang Probleme auftreten, lesen Sie [Problembehandlung](https://github.com/azure-rtos/getting-started/blob/master/docs/troubleshooting.md).

1. Starten Sie **Termite**.
1. Wählen Sie **Settings** aus.
1. Überprüfen Sie im Dialogfeld **Serial port settings** (Einstellungen für den seriellen Anschluss) die folgenden Einstellungen, und ändern Sie sie bei Bedarf:
    * **Baud rate** (Baudrate): 115.200
    * **Port**: (Anschluss) Der Anschluss, mit dem Renesas RX65N verbunden ist. Falls die Dropdownliste mehrere Anschlussoptionen enthält, können Sie hier den richtigen Anschluss auswählen. Öffnen Sie den **Geräte-Manager** von Windows, und ermitteln Sie unter **Anschlüsse**, welcher Anschluss verwendet wird.

    :::image type="content" source="media/quickstart-devkit-renesas-rx65n-2mb/termite-settings.png" alt-text="Screenshot: Einstellungen für serielle Anschlüsse in der Termite-App":::

1. Wählen Sie „OK“ aus.
1. Drücken Sie an Ihrem Gerät die Taste **Reset**.
1. Überprüfen Sie in der **Termite**-App die folgenden Prüfpunktwerte, um sich zu vergewissern, dass das Gerät initialisiert und mit Azure IoT verbunden wurde.

    ```output
    Starting Azure thread

    Initializing DHCP
        IP address: 10.0.0.81
        Mask: 255.255.255.0
        Gateway: 10.0.0.1
    SUCCESS: DHCP initialized

    Initializing DNS client
        DNS address: 10.0.0.1
    SUCCESS: DNS client initialized

    Initializing SNTP client
        SNTP server 0.pool.ntp.org
        SNTP IP address: 104.194.242.237
        SNTP time update: May 28, 2021 22:53:27.54 UTC 
    SUCCESS: SNTP initialized

    Initializing Azure IoT DPS client
        DPS endpoint: global.azure-devices-provisioning.net
        DPS ID scope: ***
        Registration ID: mydevice
    SUCCESS: Azure IoT DPS client initialized

    Initializing Azure IoT Hub client
        Hub hostname: ***.azure-devices.net
        Device id: mydevice
        Model id: dtmi:azurertos:devkit:gsg;1
    Connected to IoT Hub
    SUCCESS: Azure IoT Hub client initialized
    ```

Lassen Sie Termite geöffnet, damit Sie die Geräteausgabe während der folgenden Schritte verfolgen können.

## <a name="verify-the-device-status"></a>Überprüfen des Gerätestatus

Zeigen Sie den Gerätestatus im IoT Central-Portal wie folgt an:
1. Wählen Sie im Anwendungsdashboard im seitlichen Navigationsmenü die Option **Geräte** aus.
1. Vergewissern Sie sich, dass der **Gerätestatus** auf **Bereitgestellt** aktualisiert wurde.
1. Vergewissern Sie sich, dass die Anzeige unter **Gerätevorlage** auf **Leitfaden für erste Schritte** aktualisiert wurde.

    :::image type="content" source="media/quickstart-devkit-renesas-rx65n-2mb/iot-central-device-view-status.png" alt-text="Screenshot: Gerätestatus in IoT Central":::

## <a name="view-telemetry"></a>Anzeigen von Telemetriedaten

Mit IoT Central können Sie den Weg der Telemetriedaten von Ihrem Gerät in die Cloud anzeigen.

Zeigen Sie die Telemetriedaten im IoT Central-Portal wie folgt an:

1. Wählen Sie im Anwendungsdashboard im seitlichen Navigationsmenü die Option **Geräte** aus.
1. Wählen Sie das Gerät in der Geräteliste aus.
1. Zeigen Sie die Telemetriedaten auf der Registerkarte **Übersicht** an, während das Gerät Nachrichten an die Cloud sendet.

    :::image type="content" source="media/quickstart-devkit-renesas-rx65n-2mb/iot-central-device-telemetry.png" alt-text="Screenshot: Gerätetelemetrie in IoT Central":::

    > [!NOTE]
    > Sie können die Telemetriedaten auch auf dem Gerät überwachen, indem Sie die Termite-App verwenden.

## <a name="call-a-direct-method-on-the-device"></a>Aufrufen einer direkten Methode auf dem Gerät

Sie können auch mithilfe von IoT Central eine direkte Methode aufrufen, die Sie auf Ihrem Gerät implementiert haben. Direkte Methoden haben einen Namen und können optional über JSON-Nutzdaten, eine konfigurierbare Verbindung und ein Methodentimeout verfügen. In diesem Abschnitt rufen Sie eine Methode auf, mit der Sie eine LED ein- oder ausschalten können.

Rufen Sie eine Methode wie folgt im IoT Central-Portal auf:

1. Wählen Sie auf der Geräteseite die Registerkarte **Befehl** aus.
1. Wählen Sie in der Dropdownliste **Status** die Option **True** und dann **Ausführen** aus.  Die LED sollte aufleuchten.

    :::image type="content" source="media/quickstart-devkit-renesas-rx65n-2mb/iot-central-invoke-method.png" alt-text="Screenshot: Aufrufen einer direkten Methode auf einem Gerät in IoT Central":::

1. Wählen Sie in der Dropdownliste **Status** die Option **False** und dann **Ausführen** aus. Die LED sollte nicht mehr aufleuchten.

## <a name="view-device-information"></a>Anzeigen von Geräteinformationen

Sie können die Geräteinformationen in IoT Central anzeigen.

Wählen Sie auf der Geräteseite die Registerkarte **Info** aus.

:::image type="content" source="media/quickstart-devkit-renesas-rx65n-2mb/iot-central-device-about.png" alt-text="Screenshot: Geräteinformationen in IoT Central":::

## <a name="troubleshoot"></a>Problembehandlung

Wenn beim Erstellen des Gerätecodes, beim Einspielen per Flashvorgang auf dem Gerät oder beim Herstellen einer Verbindung Probleme auftreten, lesen Sie [Problembehandlung](https://github.com/azure-rtos/getting-started/blob/master/docs/troubleshooting.md).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die in diesem Schnellstart erstellten Azure-Ressourcen nicht mehr benötigen, können Sie sie über das IoT Central-Portal löschen.

Entfernen Sie die gesamte Azure IoT Central-Beispielanwendung und alle zugehörigen Geräte und Ressourcen wie folgt:
1. Wählen Sie **Verwaltung** > **Ihre Anwendung** aus.
1. Klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie ein benutzerdefiniertes Image mit Azure RTOS-Beispielcode erstellt und es anschließend per Flashvorgang auf dem Renesas RX65N-Gerät eingespielt. Außerdem haben Sie über das IoT Central-Portal Azure-Ressourcen erstellt, Renesas RX65N auf sichere Weise mit Azure verbunden, die Telemetriedaten angezeigt und Nachrichten gesendet.

Lesen Sie als Nächstes die folgenden Artikel, um mehr darüber zu erfahren, wie Geräte mithilfe der IoT-Geräte-SDKs mit Azure IoT verbunden werden. 

> [!div class="nextstepaction"]
> [Verbinden eines simulierten Geräts mit IoT Central](quickstart-send-telemetry-central.md)
> [!div class="nextstepaction"]
> [Verbinden eines simulierten Geräts mit IoT Hub](quickstart-send-telemetry-iot-hub.md)

> [!IMPORTANT]
> Mit Azure RTOS werden für OEMs Komponenten bereitgestellt, mit denen diese die Kommunikation schützen, Code erstellen und die Datenisolation sicherstellen können, indem sie die zugrunde liegenden MCU/MPU-Mechanismen für den Hardwareschutz verwenden. Letztendlich ist aber jeder OEM selbst dafür verantwortlich sicherzustellen, dass sein Gerät die sich ständig weiter entwickelnden Sicherheitsanforderungen erfüllt.

