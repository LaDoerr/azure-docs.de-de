---
title: Features für Kundendatenanforderungen für Azure DPS-Geräte
description: Dieser Artikel erläutert für Administratoren das Exportieren oder Löschen personenbezogener Daten bei persönlichen Geräten, die im Azure Device Provisioning Service (DPS) verwaltet werden.
author: dominicbetts
ms.author: dobett
ms.date: 05/16/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
ms.openlocfilehash: aed9a608dd8a958a298b3a7f502dac6d587b2a2e
ms.sourcegitcommit: 7d63ce88bfe8188b1ae70c3d006a29068d066287
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2021
ms.locfileid: "114466746"
---
# <a name="summary-of-customer-data-request-features"></a>Zusammenfassung der Features für Kundendatenanforderungen

Azure IoT Hub Device Provisioning Service ist ein REST-API-basierter Clouddienst für Unternehmenskunden, der ohne Benutzereingriff eine nahtlose, automatisierte Bereitstellung von Geräten in Azure IoT Hub ermöglicht – mit einer Sicherheit, die auf dem Gerät beginnt und erst in der Cloud endet.

[!INCLUDE [gdpr-intro-sentence](../../includes/gdpr-intro-sentence.md)]

Ein Mandantenadministrator weist einzelnen Geräten eine Registrierungs-ID und eine Geräte-ID zu. Daten von und zu diesen Geräten basieren auf diesen IDs. Microsoft behält keine Informationen und hat keinen Zugriff auf Daten, die eine Korrelation zwischen diesen Geräten und dem Benutzer zulassen würden.

Viele der in Device Provisioning Service verwalteten Geräte sind keine persönlichen Geräte, z. B. ein Bürothermostat oder ein Werkroboter. Kunden können jedoch einige Geräte als persönlich identifizierbar betrachten und nach eigenem Ermessen ihre eigenen Asset- oder Bestandsnachverfolgungsmethoden anwenden, die Geräte an Personen binden. Device Provisioning Service verwaltet und speichert alle Daten, die mit Geräten verknüpft sind, genau wie persönliche Daten.

Mandantenadministratoren können das Azure-Portal oder die REST-APIs des Diensts verwenden, um Informationsanforderungen durch das Exportieren oder Löschen der einer Geräte-ID oder Registrierungs-ID zugeordneten Daten zu erfüllen.

> [!NOTE]
> Für Geräte, die in Azure IoT Hub über Device Provisioning Service bereitgestellt wurden, werden zusätzliche Daten im Azure IoT Hub-Dienst gespeichert. Informationen zum Ausfüllen einer vollständigen Anfrage nach einem bestimmten Gerät finden Sie in der [Azure IoT Hub-Referenzdokumentation](../iot-hub/iot-hub-customer-data-requests.md).

## <a name="deleting-customer-data"></a>Löschen von Kundendaten

In Device Provisioning Service werden Registrierungen und Registrierungsdatensätze gespeichert. Registrierungen enthalten Informationen zu Geräten, die bereitgestellt werden dürfen, und Registrierungsdatensätze zeigen, welche Geräte den Bereitstellungsprozess bereits durchlaufen haben.

Mandantenadministratoren können Registrierungen aus dem Azure-Portal entfernen. Dadurch werden auch alle zugeordneten Registrierungsdatensätze entfernt.

Weitere Informationen finden Sie unter [Verwalten von Geräteregistrierungen](how-to-manage-enrollments.md).

Löschvorgänge für Registrierungen und Registrierungsdatensätze können auch mithilfe von REST-APIs ausgeführt werden:

* Um Registrierungsinformationen für ein einzelnes Gerät zu löschen, können Sie [Geräteregistrierung > Löschen](/rest/api/iot-dps/service/individual-enrollment/delete) verwenden.
* Um Registrierungsinformationen für eine Gruppe von Geräten zu löschen, können Sie [Geräteregistrierungsgruppe > Löschen](/rest/api/iot-dps/service/enrollment-group/delete) verwenden.
* Um Informationen zu bereitgestellten Geräten zu löschen, können Sie [Registrierungsstatus > Registrierungsstatus löschen](/rest/api/iot-dps/service/device-registration-state/delete) verwenden.

## <a name="exporting-customer-data"></a>Exportieren von Kundendaten

In Device Provisioning Service werden Registrierungen und Registrierungsdatensätze gespeichert. Registrierungen enthalten Informationen zu Geräten, die bereitgestellt werden dürfen, und Registrierungsdatensätze zeigen, welche Geräte den Bereitstellungsprozess bereits durchlaufen haben.

Mandantenadministratoren können Registrierungen und Registrierungsdatensätze über das Azure-Portal anzeigen und über Kopieren und Einfügen exportieren.

Weitere Informationen zur Verwaltung von Registrierungen finden Sie unter [Verwalten von Geräteregistrierungen](how-to-manage-enrollments.md).

Exportvorgänge für Registrierungen und Registrierungsdatensätze können auch mithilfe von REST-APIs ausgeführt werden:

* Um Registrierungsinformationen für ein einzelnes Gerät zu exportieren, können Sie [Geräteregistrierung > Abrufen](/rest/api/iot-dps/service/individual-enrollment/get) verwenden.
* Um Registrierungsinformationen für eine Gruppe von Geräten zu exportieren, können Sie [Geräteregistrierungsgruppe > Abrufen](/rest/api/iot-dps/service/enrollment-group/get) verwenden.
* Um Informationen zu bereits bereitgestellten Geräten zu exportieren, können Sie [Registrierungsstatus > Registrierungsstatus abrufen](/rest/api/iot-dps/service/device-registration-state/get) verwenden.

> [!NOTE]
> Wenn Sie Microsoft Enterprise Services verwenden, generiert Microsoft einige Informationen, die als vom System generierte Protokolle bezeichnet werden. Auf einige vom System generierte Device Provisioning Service-Protokolle können Mandantenadministratoren nicht zugreifen bzw. können einige dieser Protokolle nicht von Mandantenadministratoren exportiert werden. Diese Protokolle stellen faktische Aktionen dar, die innerhalb der Dienst- und Diagnosedaten einzelner Geräte ausgeführt werden.

## <a name="links-to-additional-documentation"></a>Links zu zusätzlicher Dokumentation

Die vollständige Dokumentation zu Device Provisioning Service-APIs finden Sie unter [https://docs.microsoft.com/rest/api/iot-dps](/rest/api/iot-dps).

[Features für Kundendatenanforderungen](../iot-hub/iot-hub-customer-data-requests.md) in Azure IoT Hub.