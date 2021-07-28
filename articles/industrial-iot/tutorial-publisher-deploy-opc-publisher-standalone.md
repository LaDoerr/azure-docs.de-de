---
title: Bereitstellen von Microsoft OPC Publisher
description: In diesem Tutorial erfahren Sie, wie Sie OPC Publisher im eigenständigen Modus bereitstellen.
author: jehona-m
ms.author: jemorina
ms.service: industrial-iot
ms.topic: tutorial
ms.date: 3/22/2021
ms.openlocfilehash: 80d4ef5fc474ff40807e01b6ecb9014db1ae8f51
ms.sourcegitcommit: df574710c692ba21b0467e3efeff9415d336a7e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2021
ms.locfileid: "110677871"
---
# <a name="tutorial-deploy-the-opc-publisher"></a>Tutorial: Bereitstellen von OPC Publisher

OPC Publisher ist ein vollständig unterstütztes Open-Source-Produkt von Microsoft, das die Lücke zwischen Industrieanlagen und der Microsoft Azure-Cloud schließt. Dazu stellt es eine Verbindung mit OPC UA-fähigen Anlagen oder Industriekonnektivitätssoftware her und veröffentlicht Telemetriedaten in verschiedenen Formaten in [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/), u. a. im Standardformat „IEC62541 OPC UA PubSub“ (ab Version 2.6).

Das Produkt wird als Modul in [Azure IoT Edge](https://azure.microsoft.com/services/iot-edge/) oder als Container in einer einfachen Docker-Umgebung ausgeführt. Da es die [plattformübergreifende .NET-Runtime](/dotnet/core/introduction) nutzt, wird es auch nativ unter Linux und Windows 10 ausgeführt.

In diesem Tutorial lernen Sie, wie die folgenden Aufgaben ausgeführt werden:

> [!div class="checklist"]
> * Bereitstellen von OPC Publisher
> * Ausführen der neuesten veröffentlichten Version von OPC Publisher als Container
> * Angeben von Containererstellungsoptionen im Azure-Portal

Falls Sie noch nicht über ein Azure-Abonnement verfügen, können Sie ein kostenloses Testkonto erstellen.

## <a name="prerequisites"></a>Voraussetzungen

- Eine IoT Hub-Instanz muss erstellt werden.
- Ein IoT Edge-Gerät muss erstellt werden.
- Ein IoT Edge-Gerät muss registriert werden.

## <a name="deploy-the-opc-publisher-from-the-azure-marketplace"></a>Bereitstellen von OPC Publisher über den Azure Marketplace

1. Wählen Sie das zu verwendende Azure-Abonnement aus. Ist kein Azure-Abonnement verfügbar, erstellen Sie eins.
2. Wählen Sie die IoT Hub-Instanz aus, an die OPC Publisher Daten senden soll. Ist keine IoT Hub-Instanz verfügbar, erstellen Sie eine.
3. Wählen Sie das IoT Edge-Gerät aus, auf dem OPC Publisher ausgeführt werden soll (oder geben Sie einen Namen für ein neu zu erstellendes IoT Edge-Gerät ein).
4. Klicken Sie auf „Erstellen“. Daraufhin wird die Seite „Module für Gerät festlegen“ für das ausgewählte IoT Edge-Gerät geöffnet.
5. Klicken Sie auf „OPCPublisher“, um die Seite „IoT Edge-Modul aktualisieren“ von OPC Publisher zu öffnen, und wählen Sie „Optionen für Containererstellung“ aus.
6. Geben Sie basierend auf Ihrer Nutzung von OPC Publisher zusätzliche Containererstellungsoptionen an. Weitere Informationen finden Sie im nächsten Abschnitt.

Alle unterstützten Docker-Images für Docker OPC Publisher sind [hier](https://mcr.microsoft.com/v2/iotedge/opc-publisher/tags/list) aufgeführt. Für Ressourcen ohne OPC UA-Unterstützung haben wir mit führenden Industriekonnektivitätsanbietern zusammengearbeitet und Ihnen dabei geholfen, ihre OPC UA-Adapter Software für Azure IoT Edge zu portieren. Diese Adapter sind im [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps?page=1) verfügbar.

## <a name="specifying-container-create-options-in-the-azure-portal"></a>Angeben von Containererstellungsoptionen im Azure-Portal
Wenn Sie OPC Publisher über das Azure-Portal bereitstellen, können Containererstellungsoptionen auf der Seite „IoT Edge-Modul aktualisieren“ von OPC Publisher angegeben werden. Für diese Erstellungsoptionen muss das JSON-Format verwendet werden. Die OPC Publisher-Befehlszeilenargumente können über den Schlüssel „Cmd“ angegeben werden. Beispiel:
```
"Cmd": [
    "--pf=./pn.json",
    "--aa"
],
```

Hier sehen Sie einen typischen Satz von Containererstellungsoptionen eines IoT Edge-Moduls für OPC Publisher:
```
{
    "Hostname": "opcpublisher",
    "Cmd": [
        "--pf=./pn.json",
        "--aa"
    ],
    "HostConfig": {
        "Binds": [
            "/iiotedge:/appdata"
        ]
    }
}
```

Bei Verwendung dieser Optionen wird von OPC Publisher die Konfigurationsdatei `./pn.json` gelesen. Das Arbeitsverzeichnis von OPC Publisher wird beim Start auf `/appdata` festgelegt. Somit wird von OPC Publisher die Datei `/appdata/pn.json` im zugehörigen Docker-Container gelesen. Die Protokolldatei von OPC Publisher wird in `/appdata` geschrieben, und das (für OPC UA-Zertifikate verwendete) Verzeichnis `CertificateStores` wird ebenfalls in diesem Verzeichnis erstellt. Um diese Dateien im IoT Edge-Hostdateisystem verfügbar zu machen, benötigt die Containerkonfiguration ein Bereitstellungsvolume für die Einbindung. Durch die Bindung `/iiotedge:/appdata` wird das Verzeichnis `/appdata` dem Hostverzeichnis `/iiotedge` zugeordnet. Dieses wird von der IoT Edge-Runtime erstellt, falls es nicht vorhanden ist.
**Ohne dieses Bereitstellungsvolume für die Einbindung gehen alle OPC Publisher-Konfigurationsdateien verloren, wenn der Container neu gestartet wird.**

Eine Verbindung mit einem OPC UA-Server unter Verwendung des Hostnamens ohne einen im Netzwerk konfigurierten DNS-Server kann durch Hinzufügen eines Eintrags vom Typ `ExtraHosts` zum Abschnitt `HostConfig` erreicht werden:

```
"HostConfig": {
    "ExtraHosts": [
        "opctestsvr:192.168.178.26"
    ]
}
```

## <a name="next-steps"></a>Nächste Schritte 
Als Nächstes muss das bereitgestellte OPC Publisher-Edge-Modul konfiguriert werden:

> [!div class="nextstepaction"]
> [Tutorial: Konfigurieren von OPC Publisher](tutorial-publisher-configure-opc-publisher.md)