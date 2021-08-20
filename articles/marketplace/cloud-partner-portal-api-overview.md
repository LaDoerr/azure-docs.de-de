---
title: Cloud-Partnerportal-API-Referenz – kommerzieller Microsoft-Marketplace
description: Beschreibung der, Voraussetzungen zum Verwenden der und die Liste der Marketplace-API-Vorgänge.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
author: mingshen-ms
ms.author: mingshen
ms.date: 07/14/2020
ms.openlocfilehash: 5813b08a14a95a8b7bbb51b3d6593fe374a83ba6
ms.sourcegitcommit: 91fdedcb190c0753180be8dc7db4b1d6da9854a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2021
ms.locfileid: "112282041"
---
# <a name="cloud-partner-portal-api-reference"></a>Cloud-Partnerportal-API-Referenz

> [!NOTE]
> Die Cloud-Partnerportal-APIs sind in Partner Center integriert und funktionieren auch weiterhin. Der Übergang führt zu kleineren Änderungen. Beachten Sie die in diesem Dokument aufgeführten [Änderungen an CCP-APIs](#changes-to-cpp-apis-after-the-migration-to-partner-center), um sicherzustellen, dass Ihr Code nach dem Produktübergang zu Partner Center weiterhin funktioniert. CPP-APIs sollten nur für vorhandene Produkte verwendet werden, die bereits vor dem Übergang zu Partner Center integriert wurden. Neue Produkte sollten die Partner Center-APIs für die Übermittlung verwenden.

Die Cloud-Partnerportal-REST-APIs ermöglichen programmgesteuertes Abrufen und Bearbeiten von Workloads, Angeboten und Herausgeberprofilen. In den APIs wird rollenbasierte Zugriffssteuerung von Azure (Azure Role-Based Access Control, Azure RBAC) verwendet, um die richtigen Berechtigungen während der Verarbeitungszeit zu erzwingen.

Diese Referenz enthält die technischen Details für die Cloud-Partnerportal-REST-APIs. Die Nutzlastbeispiele in diesem Dokument dienen nur zu Referenzzwecken und können geändert werden, wenn neue Funktionalität hinzugefügt wurde.

## <a name="prerequisites-and-considerations"></a>Voraussetzungen und Überlegungen

Bevor Sie die APIs verwenden, sollten Sie sich Folgendes ansehen:

- Den Artikel [Voraussetzungen](./cloud-partner-portal-api-prerequisites.md), um zu erfahren, wie Sie einen Dienstprinzipal zu Ihrem Konto hinzufügen und ein Azure Active Directory-Zugriffstoken (Azure AD-Zugriffstokens) für die Authentifizierung abrufen.
- Die beiden Strategien zur [Steuerung der Parallelität](./cloud-partner-portal-api-concurrency-control.md), die zum Aufrufen dieser APIs verfügbar sind.
- Weitere API-[Überlegungen](./cloud-partner-portal-api-considerations.md), z. B. Versionsverwaltung und Fehlerbehandlung.

## <a name="changes-to-cpp-apis-after-the-migration-to-partner-center"></a>Änderungen an CPP-APIs nach der Migration zu Partner Center

| **API** | **Beschreibung der Änderung** | **Auswirkung** |
| ------- | ---------------------- | ---------- |
| POST Publish, GoLive, Cancel | Bei migrierten Angeboten weist der Antwortheader ein anderes Format auf. Er funktioniert jedoch weiterhin auf dieselbe Weise und gibt einen relativen Pfad zum Abrufen des Vorgangsstatus an. | Beim Senden einer der entsprechenden POST-Anforderungen für ein Angebot weist der Adressheader je nach Migrationsstatus des Angebots eines der beiden folgenden Formate auf:<ul><li>Nicht migrierte Angebote<br>`/api/operations/{PublisherId}${offerId}$2$preview?api-version=2017-10-31`</li><li>Migrierte Angebote<br>`/api/publishers/{PublisherId}/offers/{offereId}/operations/408a4835-0000-1000-0000-000000000000?api-version=2017-10-31`</li> |
| GET-Vorgang | Bei Angebotstypen, die zuvor das Feld „Benachrichtigungs-E-Mail“ in der Antwort unterstützten, wird dieses Feld entfernt und für migrierte Angebote nicht mehr zurückgegeben. | Bei migrierten Angeboten werden keine Benachrichtigungen mehr an die Liste der in den Anforderungen angegebenen E-Mail-Adressen gesendet. Stattdessen wird der API-Dienst an den Benachrichtigungs-E-Mail-Prozess in Partner Center angepasst, um E-Mails zu senden. Insbesondere werden Benachrichtigungen an die E-Mail-Adresse gesendet, die in Partner Center in Ihren Kontoeinstellungen im Abschnitt der Kontaktinformationen des Verkäufers festgelegt wurde, um Sie über den Fortschritt des Vorgangs zu benachrichtigen.<br><br>Überprüfen Sie die in Partner Center in den [Kontoeinstellungen](https://go.microsoft.com/fwlink/?linkid=2165291) im Abschnitt der Kontaktinformationen des Verkäufers festgelegte E-Mail-Adresse, um sicherzustellen, dass die richtige E-Mail-Adresse für Benachrichtigungen angegeben ist. |

## <a name="common-tasks"></a>Häufige Aufgaben

In dieser Referenz werden APIs ausführlich erläutert, mit denen die folgenden üblichen Aufgaben ausgeführt werden können.

### <a name="offers"></a>Angebote

- [Abrufen aller Angebote](./cloud-partner-portal-api-retrieve-offers.md)
- [Abrufen eines bestimmten Angebots](./cloud-partner-portal-api-retrieve-specific-offer.md)
- [Abrufen des Angebotsstatus](./cloud-partner-portal-api-retrieve-offer-status.md)
- [Erstellen von Angeboten](./cloud-partner-portal-api-creating-offer.md)
- [Veröffentlichen eines Angebots](./cloud-partner-portal-api-publish-offer.md)

### <a name="operations"></a>Operationen (Operations)

- [Abrufen von Vorgängen](./cloud-partner-portal-api-retrieve-operations.md)
- [Abbrechen von Vorgängen](./cloud-partner-portal-api-cancel-operations.md)

### <a name="publish-an-app"></a>Veröffentlichen einer App

- [Veröffentlichung](./cloud-partner-portal-api-go-live.md)

### <a name="other-tasks"></a>Weitere Aufgaben

- [Festlegen der Preise für VM-Angebote](./cloud-partner-portal-api-setting-price.md)

### <a name="troubleshooting"></a>Problembehandlung

- [Beheben von Authentifizierungsfehlern](./cloud-partner-portal-api-troubleshooting-authentication-errors.md)
