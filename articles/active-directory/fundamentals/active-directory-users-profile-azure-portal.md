---
title: Hinzufügen oder Aktualisieren von Benutzerprofilinformationen – Azure AD
description: Anweisungen zum Hinzufügen von Informationen zu einem Benutzerprofil in Azure Active Directory, z.B. ein Bild und Angaben zur Beschäftigung.
services: active-directory
author: ajburnle
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: how-to
ms.date: 06/10/2021
ms.author: ajburnle
ms.reviewer: jeffsta
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6239b0c7f56db49d54ee95b1d6fb0737d2ea9cb8
ms.sourcegitcommit: 942a1c6df387438acbeb6d8ca50a831847ecc6dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112017977"
---
# <a name="add-or-update-a-users-profile-information-using-azure-active-directory"></a>Hinzufügen oder Aktualisieren von Benutzerprofilinformationen mit Azure Active Directory
Lernen Sie, wie Sie Benutzerprofilinformationen, z.B. ein Profilbild oder Angaben zur Beschäftigung, und einige Einstellungen mit Azure Active Directory (Azure AD) hinzufügen. Weitere Informationen zum Hinzufügen neuer Benutzer finden Sie unter [Hinzufügen oder Löschen von Benutzern in Azure Active Directory](add-users-azure-active-directory.md).

## <a name="add-or-change-profile-information"></a>Hinzufügen oder Ändern von Profilinformationen
Wie Sie sehen werden, lassen sich in Benutzerprofilen mehr Informationen angeben, als Sie beim Erstellen eines Benutzers hinzufügen können. Alle diese zusätzlichen Informationen sind optional und können bei Bedarf von Ihrer Organisation hinzugefügt werden.

## <a name="to-add-or-change-profile-information"></a>Hinzufügen oder Ändern von Profilinformationen

>[!Note]
>Die Eigenschaften für Benutzername und E-Mail-Adresse dürfen keine Akzentzeichen enthalten.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) für die Organisation in der Rolle „Benutzeradministrator“ an.

2. Wählen Sie **Azure Active Directory**, **Benutzer** und dann einen Benutzer aus. Beispielsweise _Alain Charon_.

    Daraufhin wird die Seite **Alain Charon – Profil** angezeigt.

    ![Benutzerprofilseite mit bearbeitbaren Informationen](media/active-directory-users-profile-azure-portal/user-profile-all-blade.png)

3. Wählen Sie **Bearbeiten** aus, falls Sie die Informationen in den bearbeitbaren Bereichen hinzufügen oder aktualisieren möchten.

    - **Profilbild:** Wählen Sie ein Miniaturbild für das Benutzerkonto aus. Dieses Bild wird in Azure Active Directory und auf den persönlichen Seiten des Benutzers angezeigt, z.B. auf „myapps.microsoft.com“.

    - **Identität:** Fügen Sie einen zusätzlichen Identitätswerts für den Benutzer hinzu, oder aktualisieren Sie diesen, z. B. einen Ehenamen. Sie können diesen Namen unabhängig von den Werten für „Vorname“ und „Nachname“ festlegen. Sie könnten diesen beispielsweise zum Einschließen von Initialen oder eines Firmennamens oder zum Ändern der Reihenfolge der angezeigten Namen verwenden. In einem weiteren Beispiel könnten Sie für zwei Benutzer mit dem Namen „Chris Green“ die Identitätszeichenfolge verwenden, um die Namen auf „Chris B. Green“ und „Chris R. Green (Contoso)“ festzulegen.

    - **Angaben zum Beruf:** Fügen Sie alle berufsbezogenen Informationen für einen Benutzer hinzu, z.B. seine Berufsbezeichnung, die Abteilung oder den Geschäftsführer.

    - **Einstellungen:** Legen Sie fest, ob sich der Benutzer bei einem Azure Active Directory-Mandanten anmelden kann. Sie können auch den globalen Standort des Benutzers angeben.

    - **Kontaktinformationen:** Fügen Sie alle relevanten Kontaktinformationen für den Benutzer hinzu, außer der Telefonnummer oder Mobiltelefonnummer eines Benutzers (nur ein globaler Administrator kann die Informationen für Benutzer in Administratorrollen aktualisieren).

    - **Kontaktinformationen zur Authentifizierung:** Überprüfen Sie diese Informationen, um sicherzustellen, dass für den Benutzer eine aktive Telefonnummer und E-Mail-Adresse hinterlegt sind. Azure Active Directory verwendet diese Informationen, um den Benutzer beim Anmelden zu authentifizieren. Die Kontaktinformationen für die Authentifizierung können nur von einem globalen Administrator aktualisiert werden.

4. Wählen Sie **Speichern** aus.

    Alle Ihre Änderungen werden für den Benutzer gespeichert.

    >[!Note]
    >Sie müssen Windows Server Active Directory verwenden, um die Identität, Kontaktinformationen oder Jobinformationen für Benutzer zu aktualisieren, deren Autoritätsquelle Windows Server Active Directory ist. Nachdem Sie das Update abgeschlossen haben, müssen Sie bis zum Abschluss des nächsten Synchronisierungszyklus warten, bevor die Änderungen angezeigt werden.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie Ihre Benutzerprofile aktualisiert haben, können Sie die folgenden grundlegenden Prozesse ausführen:

- [Hinzufügen oder Löschen von Benutzern](add-users-azure-active-directory.md)

- [Zuweisen von Rollen zu Benutzern](active-directory-users-assign-role-azure-portal.md)

- [Erstellen einer einfachen Gruppe und Hinzufügen von Mitgliedern](active-directory-groups-create-azure-portal.md)

Sie können auch andere Aufgaben für die Benutzerverwaltung ausführen, z.B. Delegate zuweisen, Richtlinien verwenden und Benutzerkonten freigeben. Weitere Informationen zu anderen verfügbaren Aktionen finden Sie unter [Dokumentation zur Azure Active Directory-Benutzerverwaltung](../enterprise-users/index.yml).
