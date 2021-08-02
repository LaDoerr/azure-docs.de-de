---
title: Auflisten von Azure AD-Rollendefinitionen – Azure AD
description: Erfahren Sie, wie Sie in Azure integrierte und benutzerdefinierte Rollen auflisten.
services: active-directory
author: rolyon
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: roles
ms.topic: how-to
ms.date: 03/07/2021
ms.author: rolyon
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 00fc768357d80fa22305b3b85e084dc636bd5a7c
ms.sourcegitcommit: 7f59e3b79a12395d37d569c250285a15df7a1077
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/02/2021
ms.locfileid: "110796408"
---
# <a name="list-azure-ad-role-definitions"></a>Auflisten von Azure AD-Rollendefinitionen

Eine Rollendefinition ist eine Sammlung von ausführbaren Berechtigungen wie etwa Lesen, Schreiben und Löschen. Sie wird in der Regel einfach als „Rolle“ bezeichnet. Azure Active Directory enthält über 60 integrierte Rollen. Sie können aber auch eigene benutzerdefinierte Rollen erstellen. Falls Sie sich jemals gefragt haben, welchen Zweck diese Rollen haben, können Sie für jede der Rollen eine ausführliche Liste mit den Berechtigungen anzeigen.

In diesem Artikel wird beschrieben, wie Sie die in Azure AD integrierten und die benutzerdefinierten Rollen zusammen mit ihren Berechtigungen auflisten.

## <a name="list-all-roles"></a>Auflisten aller Rollen

1. Melden Sie sich beim [Azure AD Admin Center](https://aad.portal.azure.com) an, und wählen Sie **Azure Active Directory** aus.

1. Wählen Sie **Rollen und Administratoren** aus, um die Liste mit allen verfügbaren Rollen anzuzeigen.

    ![Liste der Rollen im Azure-Portal](./media/role-definitions-list/view-roles-in-azure-active-directory.png)

1. Wählen Sie auf der rechten Seite die Auslassungspunkte und dann **Beschreibung** aus, um die vollständige Liste der Berechtigungen für eine Rolle anzuzeigen.

    Die Seite enthält Links zu relevanter Dokumentation, die Ihnen bei der Verwaltung von Rollen als Unterstützung dienen soll.

    ![Screenshot der Seite „Globaler Administrator – Beschreibung“](./media/role-definitions-list/role-description.png)

## <a name="next-steps"></a>Nächste Schritte

* Im [Forum für Azure AD-Administratorrollen](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032) können Sie sich gerne mit uns in Verbindung setzen.
* Weitere Informationen zu Rollenberechtigungen finden Sie unter [Integrierte Rollen in Azure AD](permissions-reference.md).
* Informationen zu Standardbenutzerberechtigungen finden Sie unter [Vergleich von Standardbenutzerberechtigungen für Gäste und Mitglieder](../fundamentals/users-default-permissions.md).
