---
title: Ändern des Kennworts für das Azure AD-Connector-Konto | Microsoft-Dokumentation
description: In diesem Thema wird beschrieben, wie Sie das Azure AD-Connector-Konto wiederherstellen.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 6077043a-27f1-4304-a44b-81dc46620f24
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 04/25/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.custom: has-adal-ref
ms.openlocfilehash: 5a16e571478428ef1eb068236369b6e9af9f19e1
ms.sourcegitcommit: 67cdbe905eb67e969d7d0e211d87bc174b9b8dc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111853692"
---
# <a name="change-the-azure-ad-connector-account-password"></a>Ändern des Kennworts für das Azure AD-Connector-Konto
Das Azure AD-Connector-Konto sollte keine Verwaltung erfordern. Wenn Sie die Anmeldeinformationen des Kontos zurückzusetzen müssen, finden Sie die notwendigen Informationen in diesem Thema. Dies ist beispielsweise der Fall, wenn ein globaler Administrator versehentlich über PowerShell das Kennwort des Kontos zurückgesetzt hat.

## <a name="reset-the-credentials"></a>Zurücksetzen der Anmeldeinformationen
Kann das Azure AD-Connector-Konto aufgrund von Authentifizierungsproblemen keine Verbindung mit Azure AD herstellen, kann das Kennwort zurückgesetzt werden.

1. Melden Sie sich beim Azure AD Connect-Synchronisierungsserver an, und öffnen Sie PowerShell.
2. Führen Sie `$credential = Get-Credential` aus, um die Anmeldeinformationen des globalen Azure AD-Administrators bereitzustellen.
3. Führen Sie das Cmdlet `Add-ADSyncAADServiceAccount -AADCredential $credential` aus.

   Wenn das Cmdlet erfolgreich ist, wird die PowerShell-Eingabeaufforderung angezeigt. 
   
Das Cmdlet setzt das Kennwort für das Dienstkonto zurück und aktualisiert es sowohl in Azure AD als auch im Synchronisierungsmodul.

## <a name="known-issues-these-steps-can-solve"></a>Bekannte Probleme, die sich mit diesen Schritten beheben lassen
Dieser Abschnitt enthält eine Liste mit Fehlern, die von Kunden gemeldet und durch das Zurücksetzen der Anmeldeinformationen für das Azure AD-Connector-Konto behoben wurden.

---
Ereignis 6900. Der Server hat einen unerwarteten Fehler beim Verarbeiten einer Benachrichtigung über eine Kennwortänderung erkannt: AADSTS70002: Fehler beim Überprüfen der Anmeldeinformationen. AADSTS50054: Für die Authentifizierung wurde ein altes Kennwort verwendet.

---
Ereignis 659. Fehler beim Abrufen der Konfiguration für die Kennwortrichtliniensynchronisierung. Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException: AADSTS70002: Fehler beim Überprüfen der Anmeldeinformationen. AADSTS50054: Für die Authentifizierung wurde ein altes Kennwort verwendet.

## <a name="next-steps"></a>Nächste Schritte
**Übersichtsthemen**

* [Azure AD Connect-Synchronisierung: Grundlagen und Anpassung der Synchronisierung](how-to-connect-sync-whatis.md)
* [Integrieren lokaler Identitäten in Azure Active Directory](whatis-hybrid-identity.md)
