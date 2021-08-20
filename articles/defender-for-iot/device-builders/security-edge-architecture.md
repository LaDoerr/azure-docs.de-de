---
title: Defender für IoT-Micro-Agent für IoT Edge
description: Grundlegendes zur Architektur und den Funktionen des Azure Defender für IoT-Micro-Agents für IoT Edge.
ms.topic: conceptual
ms.date: 09/09/2020
ms.openlocfilehash: 2e97d31ddf447c072f7bcf8674f29e3ee8086f83
ms.sourcegitcommit: a038863c0a99dfda16133bcb08b172b6b4c86db8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2021
ms.locfileid: "113016377"
---
# <a name="azure-defender-for-iot-edge-defender-iot-micro-agent"></a>Azure Defender für IoT-Micro-Agent für IoT Edge

[Azure IoT Edge](../../iot-edge/index.yml) umfasst leistungsstarke Funktionen zum Verwalten und Ausführen von Geschäftsworkflows am Edge.
Seine Schlüsselrolle in IoT-Umgebungen macht IoT Edge zu einem besonders attraktiven Ziel für böswillige Akteure.

Der Defender für IoT-Micro-Agent stellt eine umfassende Sicherheitslösung für Ihr IoT Edge-Gerät bereit.
Das Defender für IoT-Modul sammelt, aggregiert und analysiert Sicherheitsrohdaten von Ihrem Betriebs- und Containersystem und gibt verwertbare Sicherheitsempfehlungen und -warnungen aus.

Ähnlich wie die Defender für IoT-Sicherheits-Agents für IoT-Geräte ist das Defender für IoT Edge-Modul durch seinen Modulzwilling hochgradig anpassbar.
Weitere Informationen finden Sie unter [Konfigurieren Ihres Agents](how-to-agent-configuration.md).

Der Defender für IoT-Micro-Agent für IoT Edge bietet die folgenden Funktionen:

- Sammeln von Sicherheitsrohereignissen vom zugrunde liegenden Betriebssystem (Linux) und den IoT Edge-Containersystemen

  Weitere Informationen zu verfügbaren Sicherheitsdatensammlern finden Sie unter [Defender für IoT-Agent-Konfiguration](how-to-agent-configuration.md).

- Analyse von IoT Edge-Bereitstellungsmanifesten

- Aggregieren von Sicherheitsrohereignissen in Meldungen, die über [IoT Edge-Hub](../../iot-edge/iot-edge-runtime.md#iot-edge-hub) gesendet werden.

- Entfernen der Konfiguration über den Defender für IoT-Micro-Agent-Zwilling

  Weitere Informationen finden Sie unter [Konfigurieren eines Defender für IoT-Agents](how-to-agent-configuration.md).

Der Defender für IoT-Micro-Agent für IoT Edge wird im privilegierten Modus unter IoT Edge ausgeführt.
Der privilegierte Modus ist erforderlich, damit das Modul das Betriebssystem und andere IoT Edge-Module überwachen kann.

## <a name="module-supported-platforms"></a>Vom Modul unterstützte Plattformen

Der Defender für IoT-Micro-Agent für IoT Edge ist derzeit nur für Linux verfügbar.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie Informationen zur Architektur und den Funktionen des Defender für IoT-Micro-Agents für IoT Edge erhalten.

Um mit der Defender für IoT-Bereitstellung fortzufahren, lesen Sie die folgenden Artikel:

- Bereitstellen des [Defender für IoT-Micro-Agents für IoT Edge](how-to-deploy-edge.md)
- Erfahren Sie, wie Sie den [Defender für IoT-Micro-Agent konfigurieren](how-to-agent-configuration.md).
- Erfahren Sie, wie Sie [den Defender für IoT-Dienst in Ihrer IoT Hub-Instanz aktivieren](quickstart-onboard-iot-hub.md).
- Unter [für IoT – Häufig gestellte Fragen](resources-agent-frequently-asked-questions.md) erfahren Sie mehr über den Dienst.
