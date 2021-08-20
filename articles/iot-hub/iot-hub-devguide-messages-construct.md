---
title: Grundlegendes zum Azure IoT Hub-Nachrichtenformat | Microsoft-Dokumentation
description: 'Entwicklerhandbuch: Beschreibung des Formats und des erwarteten Inhalts von IoT Hub-Nachrichten.'
author: ash2017
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/01/2021
ms.author: asrastog
ms.custom:
- 'Role: Cloud Development'
- 'Role: IoT Device'
ms.openlocfilehash: 40cbc1c5046c944a8915f1db38805cacb95b2889
ms.sourcegitcommit: 8b7d16fefcf3d024a72119b233733cb3e962d6d9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2021
ms.locfileid: "114294779"
---
# <a name="create-and-read-iot-hub-messages"></a>Erstellen und Lesen von IoT Hub-Nachrichten

Um eine nahtlose Interoperabilität zwischen Protokollen zu gewährleisten, definiert IoT Hub ein gemeinsames Nachrichtenformat für alle geräteseitigen Protokolle. Dieses Nachrichtenformat wird sowohl für [D2C-Routing](iot-hub-devguide-messages-d2c.md)- als auch für [C2D](iot-hub-devguide-messages-c2d.md)-Nachrichten verwendet. 

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

IoT Hub implementiert das D2C-Messaging anhand eines Streaming-Messagingmusters. Die D2C-Nachrichten von IoT Hub sind eher mit [Event Hubs](../event-hubs/index.yml)-*Ereignissen* vergleichbar als mit [Service Bus](../service-bus-messaging/index.yml)-*Nachrichten*, da der Dienst von einem größeren Volumen von Ereignissen durchlaufen wird, die von mehreren Lesern gelesen werden können.

Eine IoT Hub-Nachricht enthält Folgendes:

* Einen vordefinierten Satz von *Systemeigenschaften* wie unten aufgeführt.

* Einen Satz an *Anwendungseigenschaften*. Ein Wörterbuch mit Zeichenfolgeneigenschaften. Die Anwendung kann diese definieren und darauf zugreifen, ohne den Nachrichtentext deserialisieren zu müssen. IoT Hub ändert diese Eigenschaften nie.

* Ein nicht lesbarer binärer Textkörper.

Beim Senden von D2C-Nachrichten mit dem HTTPS-Protokoll sowie beim Senden von C2D-Nachrichten dürfen Eigenschaftennamen und Eigenschaftswerte nur alphanumerische ASCII-Zeichen sowie die Zeichen ``{'!', '#', '$', '%, '&', ''', '*', '+', '-', '.', '^', '_', '`', '|', '~'}`` enthalten.

D2C-Messaging mit IoT Hub weist folgende Merkmale auf:

* D2C-Nachrichten sind dauerhafter Art und werden bis zu sieben Tage lang auf dem Standardendpunkt der IoT Hub-Instanz (**messages/events**) aufbewahrt.

* D2C-Nachrichten dürfen maximal 256 KB groß sein. Sie können in Batches gruppiert werden, um den Sendevorgang zu optimieren. Die Batches können maximal 256 KB groß sein.

* IoT Hub erlaubt keine beliebige Partitionierung. D2C-Nachrichten werden gemäß ihrer ursprünglichen **deviceId** partitioniert.

* Wie in [Verwalten des Zugriffs auf IoT Hub](iot-hub-devguide-security.md) erläutert, ermöglicht IoT Hub Authentifizierung und Zugriffssteuerung auf Gerätebasis.

* Sie können Nachrichten mit Informationen stempeln, die in die Anwendungseigenschaften aufgenommen. Weitere Informationen finden Sie unter [Nachrichtenergänzungen](iot-hub-message-enrichments-overview.md).

Weitere Informationen zum Codieren und Decodieren von Nachrichten, die über verschiedene Protokollen gesendet werden, finden Sie unter [Azure IoT SDKs](iot-hub-devguide-sdks.md).

## <a name="system-properties-of-d2c-iot-hub-messages"></a>Systemeigenschaften von **D2C**-IoT Hub-Nachrichten

| Eigenschaft | BESCHREIBUNG  |Kann der Benutzer festgelegt werden?|Schlüsselwort für </br>Routingabfrage|
| --- | --- | --- | --- |
| message-id |Eine vom Benutzer festgelegte Kennung für die Nachricht; wird für Anforderung-Antwort-Muster verwendet. Format: Eine Zeichenfolge mit Berücksichtigung von Klein-/Großschreibung (bis zu 128 Zeichen lang), die aus alphanumerischen ASCII-Zeichen (7 Bit) + `{'-', ':', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`besteht.  | Ja | messageId |
| iothub-enqueuedtime |Datum und Uhrzeit des Empfangs der [D2C](iot-hub-devguide-d2c-guidance.md)-Nachricht durch IoT Hub. | Nein | enqueuedTime |
| user-id |Eine ID zum Festlegen des Ursprungs von Nachrichten. Wenn IoT Hub Nachrichten generiert, wird diese Eigenschaft auf `{iot hub name}`festgelegt. | Ja | userId |
| iothub-connection-device-id |Eine ID, die von IoT Hub für D2C-Nachrichten festgelegt wird. Diese Eigenschaft enthält die **deviceId** des Geräts, das die Nachricht sendet. | Nein | connectionDeviceId |
| iothub-connection-module-id |Eine ID, die von IoT Hub für D2C-Nachrichten festgelegt wird. Sie enthält die **moduleId** des Geräts, das die Nachricht sendet. | Nein | connectionModuleId |
| iothub-connection-auth-generation-id |Eine ID, die von IoT Hub für D2C-Nachrichten festgelegt wird. Sie enthält die **connectionDeviceGenerationId** (gemäß [Geräteidentitätseigenschaften](iot-hub-devguide-identity-registry.md#device-identity-properties)) des Geräts, das die Nachricht gesendet hat. | Nein |connectionDeviceGenerationId |
| iothub-connection-auth-method |Eine von IoT Hub für D2C-Nachrichten festgelegte Authentifizierungsmethode. Diese Eigenschaft enthält Informationen zu der Methode, die zum Authentifizieren des Geräts verwendet wird, das die Nachricht sendet.| Nein | connectionAuthMethod |
| dt-dataschema | Dieser Wert wird von IoT Hub für Gerät-zu-Cloud-Nachrichten festgelegt. Er enthält die in der Geräteverbindung festgelegte Gerätemodell-ID. | Nein | $dt-dataschema |
| dt-subject | Der Name der Komponente, die die Gerät-zu-Cloud-Nachrichten sendet. | Ja | $dt-subject |

## <a name="application-properties-of-d2c-iot-hub-messages"></a>Anwendungseigenschaften von **D2C**-IoT Hub-Nachrichten

Anwendungseigenschaften werden häufig verwendet, um mithilfe der Eigenschaft `iothub-creation-time-utc` einen Zeitstempel vom Gerät zu senden. So wird aufgezeichnet, wann die Nachricht vom Gerät gesendet wurde. Das Format dieses Zeitstempels muss UTC ohne Zeitzoneninformationen sein. Beispielsweise ist `2021-04-21T11:30:16Z` zulässig, aber `2021-04-21T11:30:16-07:00` ist unzulässig:

```json
{
  "applicationId":"5782ed70-b703-4f13-bda3-1f5f0f5c678e",
  "messageSource":"telemetry",
  "deviceId":"sample-device-01",
  "schema":"default@v1",
  "templateId":"urn:modelDefinition:mkuyqxzgea:e14m1ukpn",
  "enqueuedTime":"2021-01-29T16:45:39.143Z",
  "telemetry":{
    "temperature":8.341033560421833
  },
  "messageProperties":{
    "iothub-creation-time-utc":"2021-01-29T16:45:39.021Z"
  },
  "enrichments":{}
}
```

## <a name="system-properties-of-c2d-iot-hub-messages"></a>Systemeigenschaften von **C2D**-IoT Hub-Nachrichten

| Eigenschaft | BESCHREIBUNG  |Kann der Benutzer festgelegt werden?|
| --- | --- | --- |
| message-id |Eine vom Benutzer festgelegte Kennung für die Nachricht; wird für Anforderung-Antwort-Muster verwendet. Format: Eine Zeichenfolge mit Berücksichtigung von Klein-/Großschreibung (bis zu 128 Zeichen lang), die aus alphanumerischen ASCII-Zeichen (7 Bit) + `{'-', ':', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`besteht.  |Ja|
| sequence-number |Eine Nummer (für jede Gerätewarteschlange eindeutig), die jeder C2D-Nachricht von IoT Hub zugewiesen wird |Nein|
| zu |Ein Ziel, das in [C2D](iot-hub-devguide-c2d-guidance.md) -Nachrichten angegeben wird. |Nein|
| absolute-expiry-time |Datum und Uhrzeit des Nachrichtenablaufs. |Ja| 
| correlation-id |Eine Zeichenfolgeneigenschaft in einer Antwortnachricht, die normalerweise die Nachrichten-ID der Anforderung im Anforderung-Antwort-Muster enthält. |Ja|
| user-id |Eine ID zum Festlegen des Ursprungs von Nachrichten. Wenn IoT Hub Nachrichten generiert, wird diese Eigenschaft auf `{iot hub name}`festgelegt. |Ja|
| iothub-ack |Ein Feedbacknachrichtengenerator. Diese Eigenschaft wird in C2D-Nachrichten verwendet, um IoT Hub anzuweisen, als Ergebnis der Nachrichtenverarbeitung durch das Gerät Feedbacknachrichten zu generieren. Mögliche Werte: **Kein** (Standardeinstellung): Es wird keine Feedbacknachricht generiert. **Positiv**: Es wird eine Feedbacknachricht empfangen, wenn die Nachricht abgeschlossen wurde. **Negativ**: Es wird eine Feedbacknachricht empfangen, wenn die Nachricht ohne vollständige Verarbeitung durch das Gerät abgelaufen ist (oder die maximale Anzahl von Zustellversuchen erreicht wurde). **Voll**: Feedback wird sowohl bei erfolgreicher als auch nicht erfolgreicher Nachrichtenverarbeitung generiert. |Ja|

### <a name="system-property-names"></a>Namen von Systemeigenschaften

Die Namen von Systemeigenschaften variieren je nach dem Endpunkt, an den die Nachrichten weitergeleitet werden. Einzelheiten zu diesen Namen finden Sie in der nachstehenden Tabelle.

|Name der Systemeigenschaft|Event Hubs|Azure Storage|Service Bus|Event Grid|
|--------------------|----------|-------------|-----------|----------|
|Meldungs-ID|message-id|messageId|MessageId|message-id|
|Benutzer-ID|user-id|userId|UserId|user-id|
|Verbindungsgeräte-ID|iothub-connection-device-id| connectionDeviceId|iothub-connection-device-id|iothub-connection-device-id|
|Verbindungsmodul-ID|iothub-connection-module-id|connectionModuleId|iothub-connection-module-id|iothub-connection-module-id|
|ID der Verbindungsauthentifizierungsgenerierung|iothub-connection-auth-generation-id|connectionDeviceGenerationId| iothub-connection-auth-generation-id|iothub-connection-auth-generation-id|
|Verbindungsauthentifizierungsmethode|iothub-connection-auth-method|connectionAuthMethod|iothub-connection-auth-method|iothub-connection-auth-method|
|contentType|content-type|contentType|ContentType|iothub-content-type|
|contentEncoding|content-encoding|contentEncoding|ContentEncoding|iothub-content-encoding|
|iothub-enqueuedtime|iothub-enqueuedtime|enqueuedTime| – |iothub-enqueuedtime|
|CorrelationId|correlation-id|correlationId|CorrelationId|correlation-id|
|dt-dataschema|dt-dataschema|dt-dataschema|dt-dataschema|dt-dataschema|
|dt-subject|dt-subject|dt-subject|dt-subject|dt-subject|

## <a name="message-size"></a>Nachrichtengröße

IoT Hub misst die Nachrichtengröße auf „protokollagnostische“ Weise, indem nur die tatsächliche Nutzlast berücksichtigt wird. Die Größe in Byte wird als Summe der folgenden Werte berechnet:

* Text in Byte
* Größe aller Werte der Nachrichtensystemeigenschaften in Byte
* Größe aller Benutzereigenschaftsnamen und -werte in Byte

Die Eigenschaftennamen und -werte sind auf ASCII-Zeichen beschränkt, sodass die Länge der Zeichenfolgen der Größe in Byte entspricht.

## <a name="anti-spoofing-properties"></a>Eigenschaften zum Schutz vor Spoofing

Um ein Gerätespoofing beim D2C-Messaging zu verhindern, versieht IoT Hub alle Nachrichten mit den folgenden Eigenschaften:

* **iothub-connection-device-id**
* **iothub-connection-auth-generation-id**
* **iothub-connection-auth-method**

Die ersten beiden Eigenschaften enthalten die Werte für **deviceId** und **generationId** des ursprünglichen Geräts, wie unter [Geräteidentitätseigenschaften](iot-hub-devguide-identity-registry.md#device-identity-properties) beschrieben.

Die Eigenschaft **iothub-connection-auth-method** enthält ein serialisiertes JSON-Objekt mit folgenden Eigenschaften:

```json
{
  "scope": "{ hub | device }",
  "type": "{ symkey | sas | x509 }",
  "issuer": "iothub"
}
```

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu Größenbeschränkungen für Nachrichten in IoT Hub finden Sie unter [IoT Hub-Kontingente und -Drosselung](iot-hub-devguide-quotas-throttling.md).

* Informationen zum Erstellen und Lesen von IoT Hub-Nachrichten in verschiedenen Programmiersprachen finden Sie in den [Schnellstartanleitungen](../iot-develop/quickstart-send-telemetry-iot-hub.md?pivots=programming-language-nodejs).