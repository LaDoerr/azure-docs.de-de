---
title: 'Azure IoT Hub Device Provisioning Service: Gerätekonzepte'
description: Dieser Artikel beschreibt Konzepte für die erneute Bereitstellung von Geräten für Azure IoT Hub Device Provisioning Service (DPS).
author: wesmc7777
ms.author: wesmc
ms.date: 04/16/2021
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
ms.openlocfilehash: 29d7547404e4eaf107734bacfbfa43a0aef19f64
ms.sourcegitcommit: 7d63ce88bfe8188b1ae70c3d006a29068d066287
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2021
ms.locfileid: "114466884"
---
# <a name="iot-hub-device-reprovisioning-concepts"></a>IoT Hub Device-Konzepte für die erneute Bereitstellung

Im Lauf des Lebenszyklus einer IoT-Lösung kommt es häufig vor, dass Geräte von einem IoT-Hub zu einem anderen verlagert werden. Folgende Szenarien können der Grund für eine solche Verlagerung sein:

* **Geolocation oder Geolatenz**: Wenn ein Gerät von einem Standort zu einem anderen verlagert wird, lässt sich die Netzwerklatenz verbessern, indem das Gerät zu einem näher gelegenen IoT-Hub migriert wird.

* **Mehrinstanzenfähigkeit**: Ein Gerät wird innerhalb der gleichen IoT-Lösung einem neuen Kunden oder Kundenstandort zugewiesen. Die Dienstbereitstellung für diesen neuen Kunden erfolgt möglicherweise über einen anderen IoT-Hub.

* **Änderung einer Lösung**: Ein Gerät wird in eine neue oder aktualisierte IoT-Lösung verlagert. Aufgrund der Neuzuweisung muss das Gerät möglicherweise mit einem neuen IoT-Hub kommunizieren, der mit anderen Back-End-Komponenten verbunden ist.

* **Quarantäne**: Dieses Szenario ähnelt der Änderung einer Lösung. Ein Gerät, das Fehlfunktionen aufweist, eine Sicherheitslücke darstellt oder veraltet ist, kann einem IoT-Hub zugewiesen werden, der ausschließlich Updates ausführen kann, um die Compliance wiederherzustellen. Sobald das Gerät wieder ordnungsgemäß funktioniert, wird es wieder zu seinem primären Hub migriert.

Die Unterstützung für die erneute Bereitstellung im Device Provisioning Service erfüllt diese Anforderungen. Auf Basis der Richtlinie für die erneute Bereitstellung, die im Registrierungseintrag des Geräts konfiguriert ist, können Geräte neuen IoT-Hubs automatisch erneut zugewiesen werden.

## <a name="device-state-data"></a>Daten zum Gerätezustand

Die Daten zum Gerätestatus bestehen aus dem [Gerätezwilling](../iot-hub/iot-hub-devguide-device-twins.md) und den Gerätefunktionen. Diese Daten werden in der Device Provisioning Service-Instanz und dem IoT-Hub gespeichert, dem das Gerät zugewiesen ist.

![Schaubild der Bereitstellung mithilfe des Diensts für die Gerätebereitstellung](./media/concepts-device-reprovisioning/dps-provisioning.png)

Wenn ein Gerät erstmals mit einer Device Provisioning Service-Instanz bereitgestellt wird, werden folgende Schritte ausgeführt:

1. Das Gerät sendet eine Bereitstellungsanforderung an eine Device Provisioning Service-Instanz. Die Dienstinstanz authentifiziert die Geräteidentität anhand eines Registrierungseintrags und erstellt die anfängliche Konfiguration der Gerätezustandsdaten. Die Dienstinstanz weist das Gerät basierend auf der Registrierungskonfiguration einem IoT-Hub zu und gibt diese IoT-Hub-Zuweisung an das Gerät zurück.

2. Die Provisioning Service-Instanz sendet eine Kopie aller anfänglichen Gerätezustandsdaten an den zugewiesenen IoT-Hub. Das Gerät stellt eine Verbindung mit dem zugewiesenen IoT-Hub her, und beginnt mit dem Betrieb.

Im Lauf der Zeit können die Gerätezustandsdaten im IoT-Hub durch [Gerätevorgänge](../iot-hub/iot-hub-devguide-device-twins.md#device-operations) und [Back-End-Vorgänge](../iot-hub/iot-hub-devguide-device-twins.md#back-end-operations) aktualisiert werden. Die anfänglichen Informationen zum Gerätestatus, die in der Device Provisioning Service-Instanz gespeichert sind, bleiben davon unberührt. Diese unveränderten Gerätezustandsdaten stellen die anfängliche Konfiguration dar.

![Bereitstellen mit dem Device Provisioning Service](./media/concepts-device-reprovisioning/dps-provisioning-2.png)

Je nach Szenario ist es bei der Verlagerung von Geräten von einem IoT-Hub zu einem anderen möglicherweise erforderlich, den im vorherigen IoT-Hub aktualisierten Gerätezustand zum neuen IoT-Hub zu migrieren. Diese Migration wird durch die Richtlinien für die erneute Bereitstellung im Device Provisioning Service unterstützt.

## <a name="reprovisioning-policies"></a>Richtlinien für die erneute Bereitstellung

Je nach Szenario sendet ein Gerät in der Regel beim Neustart eine Anforderung an eine Provisioning Service-Instanz. Außerdem wird eine Methode zum manuellen Auslösen der Bereitstellung nach Bedarf unterstützt. Die in einem Registrierungseintrag befindliche Richtlinie für die erneute Bereitstellung bestimmt, wie die Device Provisioning Service-Instanz diese Bereitstellungsanforderungen verarbeitet. Die Richtlinie bestimmt auch, ob Gerätestatusdaten während der erneuten Bereitstellung migriert werden sollen. Für einzelne Registrierungen und Registrierungsgruppen sind die gleichen Richtlinien verfügbar:

* **Gerät erneut bereitstellen und Daten migrieren**: Dies ist die Standardrichtlinie für neue Registrierungseinträge. Diese Richtlinie wird angewendet, wenn Geräte, die dem Registrierungseintrag zugeordnet sind, eine neue Anforderung senden (1). Je nach Konfiguration des Registrierungseintrags wird das Gerät möglicherweise einem anderen IoT-Hub zugewiesen. Wenn das Gerät den IoT-Hub wechselt, wird die Geräteregistrierung für den ursprünglichen IoT-Hub entfernt. Die aktualisierten Gerätezustandsinformationen von diesem ursprünglichen IoT-Hub werden zum neuen IoT-Hub migriert (2). Während der Migration wird der Gerätestatus als **Wird zugewiesen** gemeldet.

    ![Diese Abbildung zeigt, dass eine Richtlinie angewendet wird, wenn Geräte, die dem Registrierungseintrag zugeordnet sind, eine neue Anforderung senden.](./media/concepts-device-reprovisioning/dps-reprovisioning-migrate.png)

* **Erneut zuweisen und auf anfängliche Konfiguration zurücksetzen**: Diese Richtlinie wird angewendet, wenn Geräte, die dem Registrierungseintrag zugeordnet sind, eine neue Bereitstellungsanforderung senden (1). Je nach Konfiguration des Registrierungseintrags wird das Gerät möglicherweise einem anderen IoT-Hub zugewiesen. Wenn das Gerät den IoT-Hub wechselt, wird die Geräteregistrierung für den ursprünglichen IoT-Hub entfernt. Die anfänglichen Konfigurationsdaten, die von der Provisioning Service-Instanz bei der Bereitstellung des Geräts empfangen wurden, werden an den neuen IoT-Hub gesendet (2). Während der Migration wird der Gerätestatus als **Wird zugewiesen** gemeldet.

    Diese Richtlinie wird häufig beim Zurücksetzen einer Factory verwendet, ohne die IoT-Hubs zu ändern.

    ![Diese Abbildung zeigt, wie eine Richtlinie angewendet wird, wenn Geräte, die dem Registrierungseintrag zugeordnet sind, eine neue Bereitstellungsanforderung senden.](./media/concepts-device-reprovisioning/dps-reprovisioning-reset.png)

* **Niemals erneut bereitstellen**: Das Gerät wird niemals einem anderen Hub zugewiesen. Diese Richtlinie dient der Verwaltung der Abwärtskompatibilität.

> [!NOTE]
> DPS ruft unabhängig von der Richtlinie für die erneute Bereitstellung immer den benutzerdefinierten Zuteilungswebhook auf, falls für das Gerät neue [ReturnData](how-to-send-additional-data.md) vorhanden sind. Wenn die Richtlinie für die erneute Bereitstellung auf **Nie erneut bereitstellen** festgelegt ist, wird der Webhook zwar aufgerufen, das Gerät ändert seinen zugewiesenen Hub jedoch nicht.

### <a name="managing-backwards-compatibility"></a>Verwalten der Abwärtskompatibilität

Vor September 2018 legten Gerätezuweisungen in IoT-Hubs ein bestimmtes Verhalten an den Tag. Wenn ein Gerät den Bereitstellungsprozess erneut durchlief, wurde es immer nur dem gleichen IoT-Hub zugewiesen.

Für Lösungen, die von diesem Verhalten abhängig sind, bietet der Provisioning Service Abwärtskompatibilität. Dieses Verhalten wird derzeit gemäß den folgenden Kriterien für Geräte beibehalten:

1. Die Geräte stellen eine Verbindung mit einer API-Version her, bevor die native Unterstützung der erneuten Bereitstellung im Device Provisioning Service verfügbar ist. Informationen dazu finden Sie in der unten stehenden API-Tabelle.

2. Für den Registrierungseintrag für die Geräte ist keine Richtlinie für die erneute Bereitstellung festgelegt.

Diese Kompatibilität stellt sicher, dass für bereits bereitgestellte Geräte das gleiche Verhalten gilt wie in der anfänglichen Testphase. Um das vorherige Verhalten beizubehalten, speichern Sie für diese Registrierungen keine Richtlinie für die erneute Bereitstellung. Wenn eine Richtlinie für die erneute Bereitstellung festgelegt wurde, hat diese Richtlinie Vorrang vor dem Verhalten. Da die Richtlinie für die erneute Bereitstellung Vorrang hat, können Kunden das Geräteverhalten aktualisieren, ohne ein Reimaging des Geräts durchführen zu müssen.

Das folgende Flussdiagramm zeigt, wann das Verhalten vorhanden ist:

![Flussdiagramm zur Abwärtskompatibilität](./media/concepts-device-reprovisioning/reprovisioning-compatibility-flow.png)

Die folgende Tabelle zeigt die API-Versionen, für welche die native Unterstützung der erneuten Bereitstellung im Device Provisioning Service noch nicht verfügbar ist:

| REST-API | C-SDK | Python SDK |  Node SDK | Java-SDK | .NET SDK |
| -------- | ----- | ---------- | --------- | -------- | -------- |
| [2018-04-01 und früher](/rest/api/iot-dps/service/individual-enrollment/create-or-update#uri-parameters) | [1.2.8 und früher](https://github.com/Azure/azure-iot-sdk-c/blob/master/version.txt) | [1.4.2 und früher](https://github.com/Azure/azure-iot-sdk-python/blob/0a549f21f7f4fc24bc036c1d2d5614e9544a9667/device/iothub_client_python/src/iothub_client_python.cpp#L53) | [1.7.3 und früher](https://github.com/Azure/azure-iot-sdk-node/blob/074c1ac135aebb520d401b942acfad2d58fdc07f/common/core/package.json#L3) | [1.13.0 und früher](https://github.com/Azure/azure-iot-sdk-java/blob/794c128000358b8ed1c4cecfbf21734dd6824de9/device/iot-device-client/pom.xml#L7) | [1.1.0 und früher](https://github.com/Azure/azure-iot-sdk-csharp/blob/9f7269f4f61cff3536708cf3dc412a7316ed6236/provisioning/device/src/Microsoft.Azure.Devices.Provisioning.Client.csproj#L20)

> [!NOTE]
> Diese Werte und Links werden sich voraussichtlich ändern. Dies ist nur ein Platzhalter zur Angabe, wo die Versionen vom Kunden bestimmt werden können und wie die erwarteten Versionen lauten.

## <a name="next-steps"></a>Nächste Schritte

* [Erneutes Bereitstellen von Geräten](how-to-reprovision.md)
