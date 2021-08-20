---
title: Erstellen eines transparenten Gatewaygeräts mit Azure IoT Edge | Microsoft-Dokumentation
description: Verwenden eines Azure IoT Edge-Geräts als transparentes Gateway, das Informationen von nachgeschalteten Geräten verarbeiten kann
author: kgremban
ms.author: kgremban
ms.date: 03/01/2021
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom:
- amqp
- mqtt
ms.openlocfilehash: 76d04b8d55b25c3fd5c96f34ad38088472c504b5
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122346781"
---
# <a name="configure-an-iot-edge-device-to-act-as-a-transparent-gateway"></a>Konfigurieren eines IoT Edge-Geräts als transparentes Gateway

[!INCLUDE [iot-edge-version-201806-or-202011](../../includes/iot-edge-version-201806-or-202011.md)]

Dieser Artikel enthält detaillierte Anweisungen zur Konfiguration eines IoT Edge-Geräts, das als transparentes Gateway für andere Geräte zur Kommunikation mit IoT Hub dient. In diesem Artikel wird mit dem Begriff *IoT Edge-Gateway* auf ein IoT Edge-Gerät verwiesen, das als transparentes Gateway konfiguriert wurde. Weitere Informationen finden Sie unter [Verwendung eines IoT Edge-Geräts als Gateway](./iot-edge-as-gateway.md).

<!-- 1.1 -->
::: moniker range="iotedge-2018-06"

>[!NOTE]
>In IoT Edge, Version 1.1 und älter, kann ein IoT Edge-Gerät einem IoT Edge-Gateway nicht nachgeschaltet werden.
>
>Nachgeschaltete Geräte können keinen Dateiupload verwenden.

::: moniker-end

<!-- 1.2.0 -->
::: moniker range=">=iotedge-2020-11"

>[!NOTE]
>Nachgeschaltete Geräte können keinen Dateiupload verwenden.

::: moniker-end

Es gibt drei allgemeine Schritte zum Einrichten einer erfolgreichen Verbindung mit einem transparenten Gateway. In diesem Artikel wird der erste Schritt behandelt:

1. **Konfigurieren Sie das Gatewaygerät als Server, sodass sich nachgeschaltete Geräte sicher mit ihm verbinden können. Richten Sie das Gateway so ein, dass es Nachrichten von nachgeschalteten Geräten empfängt und sie an das richtige Ziel weiterleitet.**
2. Erstellen Sie eine Geräteidentität für das nachgeschaltete Gerät, damit es sich beim IoT-Hub authentifizieren kann. Konfigurieren Sie das nachgeschaltete Gerät zum Senden von Nachrichten über das Gatewaygerät. Die dazu erforderlichen Schritte finden Sie unter [Authentifizieren eines nachgeschalteten Geräts bei Azure IoT Hub](how-to-authenticate-downstream-device.md).
3. Verbinden Sie das nachgeschaltete Gerät mit dem Gatewaygerät, und beginnen Sie mit dem Senden von Nachrichten. Die dazu erforderlichen Schritte finden Sie unter [Verbinden eines nachgeschalteten Geräts mit einem Azure IoT Edge-Gateway](how-to-connect-downstream-device.md).

Damit ein Gerät als Gateway fungieren kann, muss es sichere Verbindungen mit nachgeschalteten Geräten herstellen können. Mit Azure IoT Edge können Sie mithilfe der Public Key-Infrastruktur sichere Verbindungen zwischen Geräten einrichten. In diesem Fall lassen wir zu, dass ein nachgeschaltetes Gerät eine Verbindung mit einem IoT Edge-Gerät, das als transparentes Gateway fungiert, herstellt. Um eine angemessene Sicherheit zu gewährleisten, sollte das nachgeschaltete Gerät die Identität des Gatewaygeräts bestätigen. Diese Überprüfung der Identität verhindert, dass Ihre Geräte Verbindungen mit potenziell schädlichen Gateways herstellen.

<!-- 1.1 -->
:::moniker range="iotedge-2018-06"
Ein nachgeschaltetes Gerät kann eine beliebige Anwendung oder Plattform sein, deren Identität mit dem [Azure IoT Hub](../iot-hub/index.yml)-Clouddienst erstellt wurde. Diese Anwendungen verwenden häufig das [Azure IoT-Geräte-SDK](../iot-hub/iot-hub-devguide-sdks.md). Ein nachgeschaltetes Gerät kann sogar eine Anwendung sein, die auf dem IoT Edge-Gatewaygerät selbst ausgeführt wird. Allerdings kann ein IoT Edge-Gerät einem IoT Edge-Gateway nicht nachgeschaltet werden.
:::moniker-end
<!-- end 1.1 -->

<!-- 1.2 -->
:::moniker range=">=iotedge-2020-11"
Ein nachgeschaltetes Gerät kann eine beliebige Anwendung oder Plattform sein, deren Identität mit dem [Azure IoT Hub](../iot-hub/index.yml)-Clouddienst erstellt wurde. Diese Anwendungen verwenden häufig das [Azure IoT-Geräte-SDK](../iot-hub/iot-hub-devguide-sdks.md). Ein nachgeschaltetes Gerät kann sogar eine Anwendung sein, die auf dem IoT Edge-Gatewaygerät selbst ausgeführt wird.
:::moniker-end
<!-- end 1.2 -->

Sie können eine beliebige Zertifikatinfrastruktur erstellen, die die für Ihre Gerät-zu-Gateway-Topologie erforderliche Vertrauensstellung ermöglicht. In diesem Artikel wird von der gleichen Zertifikateinrichtung ausgegangen, die Sie auch zum Aktivieren der [X.509-Zertifizierungsstellenzertifikat](../iot-hub/iot-hub-x509ca-overview.md) in IoT Hub verwenden. Hierbei ist ein X.509-Zertifizierungsstellenzertifikat einem bestimmten IoT-Hub zugeordnet (der Stammzertifizierungsstelle von IoT Hub), und es sind eine Reihe von Zertifikaten, die mit dieser Zertifizierungsstelle signiert werden, sowie eine Zertifizierungsstelle für die IoT Edge-Geräte vorhanden.

>[!NOTE]
>Der in diesem Artikel verwendete Begriff *Stammzertifizierungsstellen-Zertifikat* (Stamm-ZS-Zertifikat) bezieht sich auf die oberste Zertifizierungsstelle des öffentlichen Zertifikats der PKI-Vertrauenskette. Es muss nicht unbedingt der Zertifikatstamm einer fremden Zertifizierungsstelle sein. In vielen Fällen ist es tatsächlich nur ein öffentliches Zertifikat einer Zwischenzertifizierungsstelle.

In den folgenden Schritten werden Sie durch den Prozess zum Erstellen der Zertifikate und zum Installieren an den richtigen Orten auf dem Gateway geführt. Sie können mit jedem beliebigen Computer Zertifikate generieren und sie dann auf Ihr IoT Edge-Gerät kopieren.

## <a name="prerequisites"></a>Voraussetzungen

Ein Linux- oder Windows-Gerät, auf dem IoT Edge installiert ist.

Wenn Sie kein Gerät zur Hand haben, können Sie eines in einem virtuellen Azure-Computer erstellen. Führen Sie die Schritte in [Bereitstellen Ihres ersten IoT Edge-Moduls auf einem virtuellen Linux-Gerät](quickstart-linux.md) zum Erstellen eines IoT Hubs, Erstellen eines virtuellen Computers und Konfigurieren der IoT Edge-Runtime aus.

## <a name="set-up-the-device-ca-certificate"></a>Einrichten des Zertifikats der Gerätezertifizierungsstelle

Für alle IoT Edge-Gateways muss ein Zertifikat der Gerätezertifizierungsstelle installiert sein. Der IoT Edge-Sicherheitsdaemon verwendet das Zertifizierungsstellenzertifikat für IoT Edge-Geräte zum Signieren eines Zertifikats der Workloadzertifizierungsstelle, das wiederum ein Serverzertifikat für IoT Edge Hub signiert. Das Gateway legt dem nachgeschalteten Gerät während der Initiierung der Verbindung sein Serverzertifikat vor. Das nachgeschaltete Gerät überprüft, ob das Serverzertifikat Teil einer Zertifikatskette ist, die zum Zertifikat der Stammzertifizierungsstelle führt. Durch diesen Vorgang kann das nachgeschaltete Gerät bestätigen, dass das Gateway aus einer vertrauenswürdigen Quelle stammt. Weitere Informationen finden Sie unter [Grundlegendes zur Verwendung von Zertifikaten durch Azure IoT Edge](iot-edge-certs.md).

![Gatewayzertifikateinrichtung](./media/how-to-create-transparent-gateway/gateway-setup.png)

Das Zertifikat der Stammzertifizierungsstelle und das Zertifikat der Gerätezertifizierungsstelle (mit seinem privaten Schlüssel) müssen auf dem IoT-Edge-Gatewaygerät vorhanden und in der IoT Edge-Konfigurationsdatei konfiguriert sein. Beachten Sie, dass in diesem Fall *Zertifikat der Stammzertifizierungsstelle* die oberste Zertifizierungsstelle für dieses IoT Edge-Szenario ist. Für das Zertifikat der Stammzertifizierungsstelle des Gatewaygeräts und die nachgeschalteten Geräte müssen Sie ein Rollup auf das gleiche Zertifikat der Stammzertifizierungsstelle ausführen.

>[!TIP]
>Der Installationsprozess des Zertifikats der Stammzertifizierungsstelle und des Zertifikats der Gerätezertifizierungsstelle auf einem IoT Edge-Gerät wird auch ausführlicher in [Verwalten von Zertifikaten auf einem IoT Edge-Gerät](how-to-manage-device-certificates.md) erläutert.

Halten Sie die folgenden Dateien bereit:

* Zertifikat der Stammzertifizierungsstelle
* Zertifikat der Gerätezertifizierungsstelle
* Privater Schlüssel des Zertifikats der Gerätezertifizierungsstelle

In Produktionsszenarios sollten diese Dateien mit ihrer eigenen Zertifizierungsstelle generiert werden. Für Entwicklungs- und Testszenarios können Sie Demozertifikate verwenden.

Wenn Sie keine eigene Zertifizierungsstelle haben und Demozertifikate verwenden möchten, folgen Sie den Anleitungen unter [Erstellen von Demozertifikaten zum Testen der Features von IoT Edge-Geräten](how-to-create-test-certificates.md) zum Erstellen Ihrer Dateien. Auf dieser Seite müssen Sie die folgenden Schritte ausführen:

   1. Richten Sie zuerst die Skripts zum Erstellen von Zertifikaten auf Ihrem Gerät ein.
   2. Erstellen Sie ein Stammzertifikat der Zertifizierungsstelle. Am Ende dieser Anleitungen verfügen Sie über eine Zertifikatdatei der Stammzertifizierungsstelle:
      * `<path>/certs/azure-iot-test-only.root.ca.cert.pem`.
   3. Erstellen Sie Zertifizierungsstellenzertifikate für IoT Edge-Geräte. Am Ende dieser Anleitungen verfügen Sie über ein Zertifikat der Gerätezertifizierungsstelle und den dazugehörigen privaten Schlüssel:
      * `<path>/certs/iot-edge-device-<cert name>-full-chain.cert.pem` und
      * `<path>/private/iot-edge-device-<cert name>.key.pem`

Wenn Sie die Zertifikate auf einem anderen Computer erstellt haben, kopieren Sie sie auf Ihr IoT Edge-Gerät. Führen Sie dann die nächsten Schritte aus.

<!-- 1.1 -->
:::moniker range="iotedge-2018-06"

1. Öffnen Sie die Konfigurationsdatei des Sicherheitsdaemons auf Ihrem IoT Edge-Gerät.

   * Windows: `C:\ProgramData\iotedge\config.yaml`
   * Linux: `/etc/iotedge/config.yaml`

1. Suchen Sie den Abschnitt **Zertifikateinstellungen** der Datei. Heben Sie die Auskommentierung der vier Zeilen ab **Zertifikate:** auf, und geben Sie die Datei-URIs für Ihre drei Dateien als Werte für die folgenden Eigenschaften an:
   * **device_ca_cert**: Zertifikat der Gerätezertifizierungsstelle
   * **device_ca_pk**: Privater Schlüssel der Gerätezertifizierungsstelle
   * **trusted_ca_certs**: Zertifikat der Stammzertifizierungsstelle

   Sorgen Sie dafür, dass die Zeile **Zertifikate:** kein führendes Leerzeichen enthält und dass die anderen Zeilen um zwei Leerzeichen eingezogen werden.

1. Speichern und schließen Sie die Datei.

1. Starten Sie IoT Edge neu.
   * Windows: `Restart-Service iotedge`
   * Linux: `sudo systemctl restart iotedge`
:::moniker-end
<!-- end 1.1 -->

<!--1.2 -->
:::moniker range=">=iotedge-2020-11"

1. Öffnen Sie auf Ihrem IoT Edge-Gerät die Konfigurationsdatei `/etc/aziot/config.toml`.

   >[!TIP]
   >Wenn die Konfigurationsdatei auf dem Gerät noch nicht vorhanden ist, verwenden Sie `/etc/aziot/config.toml.edge.template` als Vorlage zum Erstellen einer Datei.

1. Suchen Sie den Parameter `trust_bundle_cert`. Heben Sie die Auskommentierung dieser Zeile auf, und geben Sie den Datei-URI zur Datei mit dem Zertifikat der Stammzertifizierungsstelle auf Ihrem Gerät an.

1. Suchen Sie den Abschnitt `[edge_ca]` (Bereitstellung) der Datei. Heben Sie die Auskommentierung der drei Zeilen in diesem Abschnitt auf, und geben Sie die Datei-URIs zu Ihren Zertifikats- und Schlüsseldateien als Werte für die folgenden Eigenschaften an:
   * **cert**: Zertifikat der Gerätezertifizierungsstelle
   * **pk**: privater Schlüssel des Zertifikats der Gerätezertifizierungsstelle

1. Speichern und schließen Sie die Datei.

1. Übernehmen Sie Ihre Änderungen.

   ```bash
   sudo iotedge config apply
   ```

:::moniker-end
<!-- end 1.2 -->

## <a name="deploy-edgehub-and-route-messages"></a>Bereitstellen von edgeHub und Weiterleiten von Nachrichten

Nachgeschaltete Geräte senden Telemetriedaten und Nachrichten an das Gatewaygerät, wobei das Modul „IoT Edge-Hub“ für das Routing der Informationen an andere Module oder IoT Hub zuständig ist. Sorgen Sie für Folgendes, um Ihr Gatewaygerät für diese Funktion vorzubereiten:

* Das Modul „IoT Edge-Hub“ wird auf dem Gerät bereitgestellt.

  Wenn Sie IoT Edge zum ersten Mal auf einem Gerät installieren, startet nur ein Systemmodul automatisch: der IoT Edge-Agent. Nachdem Sie die erste Bereitstellung für ein Gerät erstellt haben, wird das zweite Systemmodul, der IoT Edge-Hub, ebenfalls gestartet. Wenn das Modul **edgeHub** auf Ihrem Gerät nicht ausgeführt wird, erstellen Sie eine Bereitstellung für das Gerät.

* Für das Modul „IoT Edge-Hub“ wurden Routen zur Verarbeitung eingehender Nachrichten von nachgeschalteten Geräten eingerichtet.

  Für das Gatewaygerät muss eine Route vorhanden sein, damit Nachrichten von nachgeschalteten Geräten verarbeitet werden können. Andernfalls ist eine Verarbeitung nicht möglich. Sie können die Nachrichten an Module auf dem Gatewaygerät oder direkt an IoT Hub senden.

Führen Sie die folgenden Schritte aus, um das Modul „IoT Edge-Hub“ bereitzustellen und mit Routen zur Verarbeitung eingehender Nachrichten von nachgeschalteten Geräten zu konfigurieren:

1. Navigieren Sie im Azure-Portal zu Ihrem IoT Hub.

2. Wechseln Sie zu **IoT Edge**, und wählen Sie das IoT Edge-Gerät aus, das Sie als Gateway verwenden möchten.

3. Wählen Sie **Module festlegen** aus.

4. Auf der Seite **Module** können Sie alle Module hinzufügen, die Sie auf dem Gatewaygerät bereitstellen möchten. Für den Zweck dieses Artikels konzentrieren wir uns auf die Konfiguration und Bereitstellung des Moduls „edgeHub“, das auf dieser Seite nicht explizit festgelegt werden muss.

5. Klicken Sie auf **Weiter: Routen**.

6. Stellen Sie auf der Seite **Routen** sicher, dass es eine Route zum Verarbeiten von Nachrichten von nachgeschalteten Geräten gibt. Beispiel:

   * Eine Route, die alle Nachrichten – ganz gleich, ob von einem Modul oder einem nachgeschalteten Gerät – an IoT Hub sendet:
       * **Name**: `allMessagesToHub`
       * **Wert**: `FROM /messages/* INTO $upstream`

   * Eine Route, die sämtliche Nachrichten von allen nachgeschalteten Geräten an IoT Hub sendet:
      * **Name**: `allDownstreamToHub`
      * **Wert**: `FROM /messages/* WHERE NOT IS_DEFINED ($connectionModuleId) INTO $upstream`

      Diese Route funktioniert, weil Nachrichten von nachgeschalteten Geräten – im Gegensatz zu Nachrichten von IoT Edge Modulen – keine Modul-ID zugeordnet ist. Mithilfe der **WHERE**-Klausel der Route können alle Nachrichten mit dieser Systemeigenschaft herausgefiltert werden.

      Weitere Informationen zum Routing von Nachrichten finden Sie unter [Bereitstellen von Modulen und Einrichten von Routen](./module-composition.md#declare-routes).

7. Nachdem Ihre Route(n) erstellt wurde(n), wählen Sie **Überprüfen + erstellen** aus.

8. Wählen Sie auf der Seite **Überprüfen + erstellen** die Option **Erstellen** aus.

## <a name="open-ports-on-gateway-device"></a>Öffnen von Ports auf dem Gatewaygerät

Standard IoT Edge-Geräte benötigen keine eingehende Verbindung, um zu funktionieren, da die gesamte Kommunikation mit dem IoT Hub über ausgehende Verbindungen erfolgt. Gatewaygeräte unterscheiden sich dadurch, dass sie in der Lage sein müssen, Nachrichten von ihren nachgeschalteten Geräten zu empfangen. Wenn zwischen den nachgeschalteten Geräten und dem Gatewaygerät eine Firewall ist, muss die Kommunikation auch über die Firewall möglich sein.

Damit ein Gatewayszenario funktioniert, muss mindestens eines der unterstützten Protokolle des IoT Edge-Hubs für den eingehenden Datenverkehr von nachgeschalteten Geräten offen sein. Die unterstützten Protokolle sind MQTT, AMQP, HTTPS, MQTT über WebSockets und AMQP über WebSockets.

| Port | Protocol |
| ---- | -------- |
| 8883 | MQTT |
| 5671 | AMQP |
| 443 | HTTPS <br> MQTT+WS <br> AMQP+WS |

## <a name="next-steps"></a>Nächste Schritte

Da nun ein IoT Edge-Gerät als transparentes Gateway eingerichtet ist, müssen Sie Ihre nachgeschalteten Geräte so konfigurieren, dass sie dem Gateway vertrauen und Nachrichten daran senden. Lesen Sie [Authentifizieren eines nachgeschalteten Geräts bei Azure IoT Hub](how-to-authenticate-downstream-device.md), um die nächsten Schritte zum Einrichten Ihres Szenarios eines transparenten Gateways auszuführen.