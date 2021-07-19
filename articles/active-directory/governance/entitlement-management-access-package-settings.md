---
title: Freigeben des Links zum Anfordern eines Zugriffspakets in der Azure AD-Berechtigungsverwaltung – Azure Active Directory
description: Erfahren Sie, wie Sie den Link zum Anfordern eines Zugriffspakets in der Azure Active Directory-Berechtigungsverwaltung freigeben.
services: active-directory
documentationCenter: ''
author: ajburnle
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.subservice: compliance
ms.date: 06/18/2020
ms.author: ajburnle
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 25cbef009f73ac788da36d60f2313dcfe9b49ebe
ms.sourcegitcommit: 5da0bf89a039290326033f2aff26249bcac1fe17
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2021
ms.locfileid: "109713556"
---
# <a name="share-link-to-request-an-access-package-in-azure-ad-entitlement-management"></a>Freigeben des Links zum Anfordern eines Zugriffspakets in der Azure AD-Berechtigungsverwaltung

Die meisten Benutzer in Ihrem Verzeichnis können sich beim Portal „Mein Zugriff“ anmelden, wo automatisch eine Liste mit den Zugriffspaketen, die sie anfordern können, angezeigt wird. Handelt es sich bei den Benutzern jedoch um externe Geschäftspartner, die sich noch nicht in Ihrem Verzeichnis befinden, müssen Sie diesen einen Link zur Anforderung eines Zugriffspakets senden. 

Wenn der Katalog für das Zugriffspaket [für externe Benutzer aktiviert ist](entitlement-management-catalog-create.md) und Sie über eine [Richtlinie für das Verzeichnis des externen Benutzers](entitlement-management-access-package-request-policy.md) verfügen, kann der externe Benutzer über den Link zum Portal „Mein Zugriff“ das Zugriffspaket anfordern.

## <a name="share-link-to-request-an-access-package"></a>Teilen des Links zum Anfordern eines Zugriffspakets

**Erforderliche Rolle:** Globaler Administrator, Identity Governance-Administrator, Benutzeradministrator, Katalogbesitzer oder Zugriffspaket-Manager

1. Klicken Sie im Azure-Portal auf **Azure Active Directory** und dann auf **Identity Governance**.

1. Klicken Sie im Menü auf der linken Seite auf **Zugriffspakete**, und öffnen Sie das Zugriffspaket.

1. Kopieren Sie auf der Seite „Übersicht“ den **Link zum Portal „Mein Zugriff“**.

    ![Zugriffspaket (Übersicht): Link zum Portal „Mein Zugriff“](./media/entitlement-management-shared/my-access-portal-link.png)

    Es ist wichtig, dass Sie den gesamten Link zum Portal „Mein Zugriff“ kopieren, wenn Sie ihn an einen internen Geschäftspartner senden. Dies stellt sicher, dass der Partner Zugriff auf das Portal Ihres Verzeichnisses erhält, um seine Anfrage zu stellen. Der Link beginnt mit `myaccess`, enthält einen Verzeichnishinweis und endet mit einer Zugriffspaket-ID.  (Für die US-Regierung lautet die Domäne im Link für das Portal „Mein Zugriff“ `myaccess.microsoft.us`.)

    `https://myaccess.microsoft.com/@<directory_hint>#/access-packages/<access_package_id>`

1. Senden Sie den Link per E-Mail an Ihren externen Geschäftspartner. Dieser kann den Link für seine Benutzer zur Anforderung des Zugriffspakets freigeben.

## <a name="next-steps"></a>Nächste Schritte

- [Anfordern des Zugriffs auf ein Zugriffspaket](entitlement-management-request-access.md)
- [Genehmigen oder Ablehnen von Zugriffsanforderungen](entitlement-management-request-approve.md)