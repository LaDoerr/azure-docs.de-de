---
title: Zuweisen einer Azure Key Vault-Zugriffsrichtlinie (Portal)
description: Erfahren Sie, wie Sie einem Sicherheitsprinzipal oder einer Anwendungsidentität mithilfe des Azure-Portals eine Key Vault-Zugriffsrichtlinie zuweisen.
services: key-vault
author: msmbaldwin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: how-to
ms.date: 08/27/2020
ms.author: mbaldwin
ms.openlocfilehash: 6258de3e6f81ef25c4f515c956662be13eb5f1e2
ms.sourcegitcommit: 8942cdce0108372d6fc5819c71f7f3cf2f02dc60
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/01/2021
ms.locfileid: "113136488"
---
# <a name="assign-a-key-vault-access-policy-using-the-azure-portal"></a>Zuweisen einer Key Vault-Zugriffsrichtlinie über das Azure-Portal

Eine Key Vault-Zugriffsrichtlinie legt fest, ob ein bestimmter Sicherheitsprinzipal (eine Anwendung oder eine Benutzergruppe) verschiedene Vorgänge für [Geheimnisse](../secrets/index.yml), [Schlüssel](../keys/index.yml) und [Zertifikate](../certificates/index.yml) in Key Vault ausführen kann. Sie können mithilfe des Azure-Portals (dieser Artikel), der [Azure-Befehlszeilenschnittstelle](assign-access-policy-cli.md) oder von [Azure PowerShell](assign-access-policy-powershell.md) Zugriffsrichtlinien zuweisen.

[!INCLUDE [key-vault-access-policy-limits.md](../../../includes/key-vault-access-policy-limits.md)]

Weitere Informationen zum Erstellen von Gruppen in Azure Active Directory über das Azure-Portal finden Sie unter [Erstellen einer einfachen Gruppe und Hinzufügen von Mitgliedern](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

## <a name="assign-an-access-policy"></a>Zuweisen einer Zugriffsrichtlinie

1.  Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu der Key Vault-Ressource. 

1.  Wählen Sie unter **Einstellungen** die Option **Zugriffsrichtlinien** und dann **Zugriffsrichtlinie hinzufügen** aus:

    ![Auswählen von „Zugriffsrichtlinien“, Auswählen von „Rollenzuweisung hinzufügen“](../media/authentication/assign-policy-portal-01.png)

1.  Wählen Sie die gewünschten Berechtigungen unter **Zertifikatberechtigungen**, **Schlüsselberechtigungen** und **Geheimnisberechtigungen** aus. Sie können auch eine Vorlage auswählen, die gebräuchliche Kombinationen aus Berechtigungen enthält:

    ![Angeben von Zugriffsrichtlinienberechtigungen](../media/authentication/assign-policy-portal-02.png)

1. Wählen Sie unter **Prinzipal auswählen** den Link **Keine ausgewählt** aus, um den Auswahlbereich **Prinzipal** zu öffnen. Geben Sie den Namen des Benutzers, der App oder des Dienstprinzipals in das Suchfeld ein, wählen Sie das entsprechende Ergebnis und wählen Sie dann **Auswählen**.

    ![Auswählen des Sicherheitsprinzipals für die Zugriffsrichtlinie](../media/authentication/assign-policy-portal-03.png)

    Wenn Sie für die App eine verwaltete Identität verwenden, suchen Sie nach dem Namen der App, und wählen Sie ihn aus. (Weitere Informationen zu Sicherheitsprinzipalen finden Sie unter [Key Vault-Authentifizierung](authentication.md).
 
1.  Wählen Sie im Bereich **Zugriffsrichtlinie hinzufügen** die Option **Hinzufügen** aus, um die Zugriffsrichtlinie zu speichern.

    ![Hinzufügen der Zugriffsrichtlinie mit dem zugewiesenen Sicherheitsprinzipal](../media/authentication/assign-policy-portal-04.png)

1. Vergewissern Sie sich auf der Seite **Zugriffsrichtlinien**, dass Ihre Zugriffsrichtlinie unter **Aktuelle Zugriffsrichtlinien** aufgeführt ist, und wählen Sie dann **Speichern** aus. Zugriffsrichtlinien werden erst angewandt, wenn Sie sie speichern.

    ![Speichern der Änderungen an den Zugriffsrichtlinien](../media/authentication/assign-policy-portal-05.png)


## <a name="next-steps"></a>Nächste Schritte

- [Azure Key Vault – Sicherheit](security-features.md)
- [Entwicklerhandbuch zu Azure Key Vault](developers-guide.md)
