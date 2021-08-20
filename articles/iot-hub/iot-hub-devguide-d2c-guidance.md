---
title: Azure IoT Hub – D2C-Optionen (Gerät-zu-Cloud) | Microsoft Docs
description: Entwicklerhandbuch – Leitfaden, der angibt, wann D2C-Nachrichten, gemeldete Eigenschaften oder Dateiupload für die C2D-Kommunikation verwendet werden sollen.
author: wesmc7777
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.custom:
- amqp
- mqtt
- 'Role: Cloud Development'
- 'Role: IoT Device'
ms.openlocfilehash: 9d48a6b7478de1015a97d3f91ce08398de3d6c1c
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122355005"
---
# <a name="device-to-cloud-communications-guidance"></a>Leitfaden zur D2C-Kommunikation

Beim Senden von Informationen von der Geräte-App an das Lösungs-Back-End stehen in IoT Hub drei Optionen zur Verfügung:

* [Gerät-zu-Cloud-Nachrichten](iot-hub-devguide-messages-d2c.md) für Time Series-Telemetrie und Warnungen.

* [Gemeldete Eigenschaften des Gerätezwillings](iot-hub-devguide-device-twins.md) für die Meldung von Gerätestatusinformationen, z.B. verfügbare Funktionen, Bedingungen oder Status von Workflows mit langer Laufzeit. z.B. Konfigurations- und Softwareupdates.

* [Dateiuploads](iot-hub-devguide-file-upload.md) für Mediendateien und große Telemetriebatches, die von zeitweise verbundenen Geräten hochgeladen oder komprimiert werden, um Bandbreite zu sparen.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Hier finden Sie einen detaillierten Vergleich verschiedener Optionen für die D2C-Kommunikation.

| Faktor | D2C-Nachrichten | Gemeldete Eigenschaften des Gerätezwillings | Dateiuploads |
| ---- | ------- | ---------- | ---- |
| Szenario | Telemetrie-Zeitreihen und -Warnungen, Beispiel: Sendung von 256-KB-Sensordatenbatches alle 5 Minuten | Verfügbare Funktionen und Bedingungen, z.B. der aktuelle Gerätekonnektivitätsmodus wie Mobilfunk oder WLAN. Synchronisierung von Workflows mit langer Laufzeit, z.B. Konfiguration und Softwareupdates. | Mediendateien. Große (normalerweise komprimierte) Telemetriebatches. |
| Speichern und Abrufen | Temporäre Speicherung durch IoT Hub, bis zu 7 Tage. Nur sequenzielles Lesen. | Von IoT Hub im Gerätezwilling gespeichert. Abrufbar mithilfe der [IoT Hub-Abfragesprache](iot-hub-devguide-query-language.md). | Speicherung im vom Benutzer bereitgestellten Azure Storage-Konto. |
| Size | Nachrichten bis zu 256 KB | Die Maximalgröße gemeldeter Eigenschaften beträgt 32 KB. | Maximale von Azure Blob Storage unterstützte Dateigröße. |
| Häufigkeit | Hoch. Weitere Informationen finden Sie unter [IoT Hub-Grenzwerte](iot-hub-devguide-quotas-throttling.md). | Mittel. Weitere Informationen finden Sie unter [IoT Hub-Grenzwerte](iot-hub-devguide-quotas-throttling.md). | Niedrig. Weitere Informationen finden Sie unter [IoT Hub-Grenzwerte](iot-hub-devguide-quotas-throttling.md). |
| Protocol | Mit allen Protokollen verfügbar. | Mit MQTT oder AMQP verfügbar. | Mit jedem Protokoll verfügbar, auf dem Gerät ist jedoch HTTPS erforderlich. |

Eine Anwendung muss möglicherweise Informationen sowohl als Telemetriezeitreihen als auch als Warnung senden und diese außerdem im Gerätezwilling zur Verfügung stellen. In diesem Szenario können Sie eine der folgenden Optionen auswählen:

* Die Geräte-App sendet eine D2C-Nachricht und meldet eine Eigenschaftsänderung.
* Das Lösungs-Back-End speichert die Informationen beim Empfang der Nachricht in den Tags des Gerätezwillings.

Da D2C-Nachrichten einen viel höheren Durchsatz zulassen als Gerätezwillingsupdates, ist es in einigen Fällen ratsam, das Gerätezwillingsupdate nicht für jede D2C-Nachricht durchzuführen.
