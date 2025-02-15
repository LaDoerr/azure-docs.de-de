---
title: Erste Schritte mit der Azure IoT Hub-Gerätezwillingen (Node) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mithilfe von Azure IoT Hub-Gerätezwillingen Tags hinzufügen und eine IoT Hub-Abfrage verwenden. Sie verwenden die Azure IoT SDKs für Node.js, um die simulierte Geräte-App und eine Dienst-App zu implementieren, die die Tags hinzufügt und die IoT Hub-Abfrage ausführt.
author: fsautomata
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 08/26/2019
ms.author: elioda
ms.custom: mqtt, devx-track-js, devx-track-azurecli
ms.openlocfilehash: bdc1822422f3e8ff7a9ca63b7bcf4627cd0e8470
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122346170"
---
# <a name="get-started-with-device-twins-nodejs"></a>Erste Schritte mit Gerätezwillingen (Node.js)

[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Am Ende dieses Tutorials verfügen Sie über zwei Node.js-Konsolen-Apps:

* **AddTagsAndQuery.js**, eine Node.js-App, die Tags hinzufügt und Gerätezwillinge abfragt.

* **TwinSimulatedDevice.js**, eine Node.js-App, die ein Gerät simuliert, das mit der zuvor erstellten Geräteidentität eine Verbindung zu Ihrem IoT Hub herstellt und seine Verbindungsbedingung meldet.

> [!NOTE]
> Im Artikel [Azure IoT SDKs](iot-hub-devguide-sdks.md) finden Sie Informationen zu den verschiedenen Azure IoT SDKs, mit denen Sie sowohl Geräte- als auch Back-End-Apps erstellen können.
>

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* Node.js, Version 10.0.x oder höher.

* Ein aktives Azure-Konto. (Wenn Sie nicht über ein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen.)

* Stellen Sie sicher, dass der Port 8883 in Ihrer Firewall geöffnet ist. Das Beispielgerät in diesem Artikel verwendet das MQTT-Protokoll, das über Port 8883 kommuniziert. In einigen Netzwerkumgebungen von Unternehmen oder Bildungseinrichtungen ist dieser Port unter Umständen blockiert. Weitere Informationen und Problemumgehungen finden Sie unter [Herstellen einer Verbindung mit IoT Hub (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub).

## <a name="create-an-iot-hub"></a>Erstellen eines IoT-Hubs

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Registrieren eines neuen Geräts beim IoT-Hub

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="get-the-iot-hub-connection-string"></a>Abrufen der IoT-Hub-Verbindungszeichenfolge

[!INCLUDE [iot-hub-howto-twin-shared-access-policy-text](../../includes/iot-hub-howto-twin-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-custom-connection-string](../../includes/iot-hub-include-find-custom-connection-string.md)]

## <a name="create-the-service-app"></a>Erstellen der Dienst-App

In diesem Abschnitt erstellen Sie eine Node.js-Konsolen-App, mit der dem Gerätezwilling, der **myDeviceId** zugeordnet ist, Standortmetadaten hinzugefügt werden. Anschließend werden die Gerätezwillinge abgefragt, die in dem für die Auswahl der Geräte in den USA zuständigen IoT Hub gespeichert sind, und dann diejenigen, die eine Mobilfunkverbindung melden.

1. Erstellen Sie einen neuen leeren Ordner mit dem Namen **addtagsandqueryapp**. Erstellen Sie im Ordner **addtagsandqueryapp** die neue Datei „package.json“, indem Sie an der Eingabeaufforderung den unten angegebenen Befehl ausführen. Der Parameter `--yes` akzeptiert alle Standardwerte.

    ```cmd/sh
    npm init --yes
    ```

2. Führen Sie an der Eingabeaufforderung im Ordner **addtagsandqueryapp** den folgenden Befehl aus, um das Paket **azure-iothub** zu installieren:

    ```cmd/sh
    npm install azure-iothub --save
    ```

3. Erstellen Sie mithilfe eines Text-Editors die neue Datei **AddTagsAndQuery.js** im Ordner **addtagsandqueryapp**.

4. Fügen Sie der Datei **AddTagsAndQuery.js** den folgenden Code hinzu. Ersetzen Sie `{iot hub connection string}` durch die IoT-Hub-Verbindungszeichenfolge, die Sie unter [Abrufen der IoT-Hub-Verbindungszeichenfolge](#get-the-iot-hub-connection-string) kopiert haben.

   ``` javascript
        'use strict';
        var iothub = require('azure-iothub');
        var connectionString = '{iot hub connection string}';
        var registry = iothub.Registry.fromConnectionString(connectionString);

        registry.getTwin('myDeviceId', function(err, twin){
            if (err) {
                console.error(err.constructor.name + ': ' + err.message);
            } else {
                var patch = {
                    tags: {
                        location: {
                            region: 'US',
                            plant: 'Redmond43'
                      }
                    }
                };

                twin.update(patch, function(err) {
                  if (err) {
                    console.error('Could not update twin: ' + err.constructor.name + ': ' + err.message);
                  } else {
                    console.log(twin.deviceId + ' twin updated successfully');
                    queryTwins();
                  }
                });
            }
        });
   ```

    Das **Registry**-Objekt macht alle Methoden verfügbar, die für die Interaktion mit Gerätezwillingen des Diensts erforderlich sind. Mit dem vorherigen Code wird zunächst das **Registry**-Objekt initialisiert und dann der Gerätezwilling für **myDeviceId** abgerufen, und schließlich werden die zugehörigen Tags mit den gewünschten Standortinformationen aktualisiert.

    Nach dem Aktualisieren der Tags wird die Funktion **queryTwins** aufgerufen.

5. Fügen Sie am Ende der Datei **AddTagsAndQuery.js** den folgenden Code ein, um die Funktion **queryTwins** zu implementieren:

   ```javascript
        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log("Devices in Redmond43: " + results.map(function(twin) {return twin.deviceId}).join(','));
                }
            });

            query = registry.createQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log("Devices in Redmond43 using cellular network: " + results.map(function(twin) {return twin.deviceId}).join(','));
                }
            });
        };
   ```

    Mithilfe dieses Codes werden zwei Abfragen ausgeführt: Mit der ersten werden nur die Gerätezwillinge von Geräten in der Anlage **Redmond43** ausgewählt. Mit der zweiten wird die Abfrage so angepasst, dass nur die Geräte ausgewählt werden, die über ein Mobilfunknetz verbunden sind.

    Wenn der Code das **query**-Objekt erstellt, wird die maximale Anzahl der zurückgegebenen Dokumente im zweiten Parameter angegeben. Das **query**-Objekt enthält die boolesche Eigenschaft **hasMoreResults**, mit der Sie die **nextAsTwin**-Methoden mehrmals aufrufen können, um alle Ergebnisse abzurufen. Die **next**-Methode ist für Ergebnisse verfügbar, die keine Gerätezwillinge sind, z.B. die Ergebnisse von Aggregationsabfragen.

6. Führen Sie die Anwendung mit folgendem Befehl aus:

    ```cmd/sh
        node AddTagsAndQuery.js
    ```

   In den Ergebnissen für die Abfrage, mit der alle Geräte in der Anlage **Redmond43** abgefragt werden, sollte ein Gerät angezeigt werden und keines für die Abfrage, mit der die Ergebnisse auf die über ein Mobilfunknetz verbundenen Geräte beschränkt werden.

   ![Anzeige des einen Geräts in den Abfrageergebnissen](media/iot-hub-node-node-twin-getstarted/service1.png)

Im nächsten Abschnitt erstellen Sie eine Geräte-App, mit der die Verbindungsinformationen gemeldet und das Ergebnis der im vorherigen Abschnitt ausgeführten Abfrage geändert wird.

## <a name="create-the-device-app"></a>Erstellen der Geräte-App

In diesem Abschnitt erstellen Sie eine Node.js-Konsolenanwendung, die als **myDeviceId** eine Verbindung mit dem Hub herstellt und dann die gemeldeten Eigenschaften des zugehörigen Gerätezwillings so aktualisiert, dass sie die Informationen über die Verbindung mit einem Mobilfunknetz enthalten.

1. Erstellen Sie einen neuen leeren Ordner mit dem Namen **reportconnectivity**. Erstellen Sie im Ordner **reportconnectivity** die neue Datei „package.json“, indem Sie an der Eingabeaufforderung den unten angegebenen Befehl ausführen. Der Parameter `--yes` akzeptiert alle Standardwerte.

    ```cmd/sh
    npm init --yes
    ```

2. Führen Sie an der Eingabeaufforderung im Ordner **reportconnectivity** den folgenden Befehl aus, um das Paket **azure-iot-device** und das Paket **azure-iot-device-mqtt** zu installieren:

    ```cmd/sh
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

3. Erstellen Sie mithilfe eines Text-Editors die neue Datei **ReportConnectivity.js** im Ordner **reportconnectivity**.

4. Fügen Sie der Datei **ReportConnectivity.js** den folgenden Code hinzu. Ersetzen Sie `{device connection string}` durch die Geräteverbindungszeichenfolge, die Sie beim Erstellen der Geräteidentität **myDeviceId** in [Registrieren eines neuen Geräts im IoT-Hub](#register-a-new-device-in-the-iot-hub) kopiert haben.

    ```javascript
        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;

        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);

        client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');

            client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                var patch = {
                    connectivity: {
                        type: 'cellular'
                    }
                };

                twin.properties.reported.update(patch, function(err) {
                    if (err) {
                        console.error('could not update twin');
                    } else {
                        console.log('twin state reported');
                        process.exit();
                    }
                });
            }
            });
        }
        });
    ```

    Das **Client**-Objekt macht alle Methoden verfügbar, die für die Interaktion mit Gerätezwillingen des Geräts erforderlich sind. Mit diesem Code wird nach dem Initialisieren des **Client**-Objekts der Gerätezwilling für **myDeviceId** abgerufen und die zugehörige gemeldete Eigenschaft mit den Verbindungsinformationen aktualisiert.

5. Führen Sie die Geräte-App aus.

    ```cmd/sh
        node ReportConnectivity.js
    ```

    Die Meldung `twin state reported` sollte angezeigt werden.

6. Da das Gerät nun die Verbindungsinformationen gemeldet hat, sollten diese in beiden Abfragen angezeigt werden. Wechseln Sie zurück in den Ordner **addtagsandqueryapp**, und führen Sie die Abfragen erneut aus:

    ```cmd/sh
        node AddTagsAndQuery.js
    ```

    Nun sollte **myDeviceId** in beiden Abfrageergebnisse angezeigt werden.

    ![Anzeige von „myDeviceId“ in beiden Abfrageergebnissen](media/iot-hub-node-node-twin-getstarted/service2.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie im Azure-Portal einen neuen IoT-Hub konfiguriert und anschließend in der Identitätsregistrierung des IoT-Hubs eine Geräteidentität erstellt. Sie haben Gerätemetadaten aus einer Back-End-App als Tags hinzugefügt und eine simulierte Geräte-App zum Melden von Geräteverbindungsinformationen im Gerätezwilling geschrieben. Sie haben zudem erfahren, wie diese Informationen mithilfe der SQL-ähnlichen IoT Hub-Abfragesprache abgefragt werden können.

Weitere Informationen finden Sie in den folgenden Ressourcen:

* Senden von Telemetriedaten von Geräten: [Tutorial: Erste Schritte mit IoT Hub](../iot-develop/quickstart-send-telemetry-iot-hub.md?pivots=programming-language-nodejs),

* Konfigurieren von Geräten mit den gewünschten Eigenschaften des Gerätezwillings: [Tutorial: Konfigurieren Ihrer Geräte über einen Back-End-Dienst](tutorial-device-twins.md)

* Interaktives Steuern von Geräten (z. B. Einschalten eines Lüfters über eine benutzergesteuerte App): [Schnellstart: Verwenden von direkten Methoden](./quickstart-control-device.md?pivots=programming-language-nodejs).