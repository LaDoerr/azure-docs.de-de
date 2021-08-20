---
title: 'Schnellstart: Konfigurieren und Aktivieren des Defender-IoT-Micro-Agents für Azure RTOS'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie den Defender-IoT-Micro-Agent für den Azure RTOS-Dienst in Ihre Azure IoT Hub-Instanz integrieren und darin aktivieren.
services: defender-for-iot
ms.topic: quickstart
ms.date: 07/01/2021
ms.openlocfilehash: 11705b33efcd594296dea087e0aaf914cb494a0c
ms.sourcegitcommit: a2540262e05ffd4a4b059df0976940d60fabd125
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/01/2021
ms.locfileid: "113138511"
---
# <a name="quickstart-defender-iot-micro-agent-for-azure-rtos"></a>Schnellstart: IoT-Micro-Agent von Defender für Azure RTOS (Vorschau)

Dieser Artikel enthält eine Erläuterung der Voraussetzungen vor den ersten Schritten. Es wird darin erläutert, wie Sie den Defender-IoT-Micro-Agent für den Azure RTOS-Dienst in einer IoT Hub-Instanz aktivieren. Wenn Sie derzeit über keinen IoT-Hub verfügen, lesen Sie die Informationen unter [Erstellen eines IoT-Hubs über das Azure-Portal](../../iot-hub/iot-hub-create-through-portal.md).

## <a name="prerequisites"></a>Voraussetzungen 

### <a name="supported-devices"></a>Unterstützte Geräte

- ST STM32F746G Discovery Kit
- NXP i.MX RT1060 EVK
- Mikrochip SAM E54 Xplained Pro EVK

Laden Sie eine der ZIP-Dateien für das jeweilige Board und Tool (IAR, die Halbleiter-IDE oder den PC) Ihrer Wahl aus der [GitHub-Ressource des Defender-IoT-Micro-Agents für Azure RTOS](https://github.com/azure-rtos/azure-iot-preview/releases) herunter, kompilieren Sie sie, und führen Sie sie aus.

### <a name="azure-resources"></a>Azure-Ressourcen

Die nächste Phase bei den ersten Schritten ist die Vorbereitung Ihrer Azure-Ressourcen. Sie benötigen einen IoT Hub, und wir empfehlen einen Log Analytics-Arbeitsbereich. Für IoT Hub benötigen Sie Ihre IoT Hub-Verbindungszeichenfolge, um eine Verbindung mit Ihrem Gerät herzustellen. 
  
### <a name="iot-hub-connection"></a>IoT Hub-Verbindung

Für die ersten Schritte ist eine IoT Hub-Verbindung ist erforderlich. 

1. Öffnen Sie im Azure-Portal Ihre Instanz von **IoT Hub**.

1. Navigieren Sie zu **IoT-Geräte**.

1. Klicken Sie auf **Erstellen**.

1. Kopieren Sie die IoT-Verbindungszeichenfolge in die [Konfigurationsdatei](how-to-azure-rtos-security-module.md).

Die Verbindungsanmeldeinformationen werden aus der Benutzeranwendungskonfiguration **HOST_NAME**, **DEVICE_ID** und **DEVICE_SYMMETRIC_KEY** übernommen.

Der Defender-IoT-Micro-Agent für Azure RTOS verwendet Azure IoT-Middleware-Verbindungen basierend auf dem **MQTT**-Protokoll.

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie den nächsten Artikel, um die Konfiguration und Anpassung Ihrer Lösung abschließen zu können.

> [!div class="nextstepaction"]
> [Konfigurieren und Anpassen des Defender-IoT-Micro-Agents für Azure RTOS (Vorschau)](how-to-azure-rtos-security-module.md)
