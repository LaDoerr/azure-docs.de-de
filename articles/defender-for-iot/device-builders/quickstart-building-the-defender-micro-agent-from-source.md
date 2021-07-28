---
title: 'Schnellstart: Erstellen des Micro-Agent für Defender auf der Grundlage von Quellcode (Vorschau)'
description: In dieser Schnellstartanleitung erfahren Sie mehr über den Micro-Agent, der eine Infrastruktur beinhaltet, die Sie zum Anpassen Ihrer Distribution verwenden können.
ms.date: 05/10/2021
ms.topic: quickstart
ms.openlocfilehash: b52c985619e317421a1597fee77911d9bc1e81d2
ms.sourcegitcommit: a038863c0a99dfda16133bcb08b172b6b4c86db8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2021
ms.locfileid: "113011176"
---
# <a name="quickstart-build-the-defender-micro-agent-from-source-code-preview"></a>Schnellstart: Erstellen des Micro-Agent für Defender auf der Grundlage von Quellcode (Vorschau)

Der Micro-Agent beinhaltet eine Infrastruktur, die Sie zum Anpassen Ihrer Distribution verwenden können. Eine Liste der verfügbaren Konfigurationsparameter finden Sie in der Datei `configs/LINUX_BASE.conf`.

Ändern Sie für eine einzelne Distribution die Basisdatei (`.conf`). 

Wenn Sie mehrere Distributionen benötigen, können Sie die Basiskonfiguration als Grundlage verwenden und deren Werte überschreiben. 

So überschreiben Sie die Werte:

1. Erstellen Sie eine neue Datei vom Typ `.dist`.

1. Fügen Sie ganz oben `CONF_DEFINE_BASE(${g_plat_config_path} LINUX_BASE.conf)` hinzu.
 
1. Definieren Sie neue Werte für alles, was Sie benötigen. Beispiel: 

    `set(ASC_LOW_PRIORITY_INTERVAL 60*60*24)` 

1. Versehen Sie die Datei vom Typ `.dist` bei der Erstellung mit einem Verweis. Beispiel: 

    `cmake -DCMAKE_BUILD_TYPE=Debug -Dlog_level=DEBUG -Dlog_level_cmdline:BOOL=ON -DIOT_SECURITY_MODULE_DIST_TARGET=UBUNTU1804 ..` 

## <a name="prerequisites"></a>Voraussetzungen

1. Fordern Sie bei Ihrem Konto-Manager Zugriff auf den Quellcode von Defender für IoT an.
 
1. Klonen oder extrahieren Sie den Quellcode in einem Ordner auf dem Datenträger.

1. Navigieren Sie zu diesem Verzeichnis.

1. Pullen Sie die Submodule mithilfe des folgenden Codes:

    ```bash
    git submodule update --init
    ```
    
1. Pullen Sie als Nächstes die Submodule für das Azure IoT SDK mithilfe des folgenden Codes: 

    ```bash
    git -C deps/azure-iot-sdk-c/ submodule update –init
    ```
 

1. Fügen Sie eine Ausführungsberechtigung hinzu, und führen Sie das Setupskript für die Entwicklerumgebung aus:

    ```bash
    chmod +x scripts/install_development_environment.sh && ./scripts/install_development_environment.sh 
    ```

1. Erstellen Sie ein Verzeichnis für die Buildausgaben: 

    ```bash
    mkdir cmake 
    ```

1. Optional: Laden Sie das Tool [VS Code](https://code.visualstudio.com/download ) herunter, und installieren Sie es. 

1. Optional: Installieren Sie die [C/C++-Erweiterung](https://code.visualstudio.com/docs/languages/cpp ) für VS Code.- Keine

## <a name="baseline-configuration-signing"></a>Signieren der Baselinekonfiguration 

Der Agent überprüft standardmäßig die Authentizität von Konfigurationsdateien, die auf dem Datenträger platziert werden, um Manipulationen entgegenzuwirken.

Dieser Prozess kann durch Definieren des Präprozessorflags `ASC_BASELINE_CONF_SIGN_CHECK_DISABLE` beendet werden.

Für Produktionsumgebungen sollte die Signaturüberprüfung nicht deaktiviert werden. 

Sollten Sie für Produktionsszenarien eine andere Konfiguration benötigen, wenden Sie sich an das Team von Defender für IoT. 

## <a name="building-the-defender-iot-micro-agent"></a>Erstellen des Micro-Agents für Defender für IoT 

1. Öffnen Sie das Verzeichnis mit VS Code. 

1. Navigieren Sie zum Verzeichnis `cmake`. 

1. Führen Sie den folgenden Befehl aus: 

    ```bash
    cmake -DCMAKE_BUILD_TYPE=Debug -Dlog_level=DEBUG -Dlog_level_cmdline:BOOL=ON -DIOT_SECURITY_MODULE_DIST_TARGET<the appropriate distro configuration file name> .. 
    
    cmake --build . -- -j${env:NPROC}
    ```

    Beispiel: 

    ```bash
    cmake -DCMAKE_BUILD_TYPE=Debug -Dlog_level=DEBUG -Dlog_level_cmdline:BOOL=ON -DIOT_SECURITY_MODULE_DIST_TARGETUBUNTU1804 ..
    
    cmake --build . -- -j${env:NPROC}
    ```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Schnellstart: Konfigurieren Ihrer Lösung mit Azure Defender für IoT](quickstart-configure-your-solution.md)
