---
title: Azure Active Directory Identity Protection-Richtlinien
description: Identifizieren der drei Richtlinien, die mit Identity Protection aktiviert sind
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 05/20/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: karenhoran
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8f1b31c8f7698e395c4944d0c6283e6e6257c2d2
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122346034"
---
# <a name="identity-protection-policies"></a>Richtlinien für den Identitätsschutz

Azure Active Directory Identity Protection enthält drei Standardrichtlinien, die Administratoren aktivieren können. Diese Richtlinien bieten eingeschränkte Anpassungen, sind jedoch für die meisten Organisationen anwendbar. Alle Richtlinien ermöglichen das Ausschließen von Benutzern, wie z.B. [Benutzer- oder Administratorkonten für den Notfallzugriff](../roles/security-emergency-access.md).

![Richtlinien für den Identitätsschutz](./media/concept-identity-protection-policies/identity-protection-policies.png)

## <a name="azure-ad-mfa-registration-policy"></a>Azure AD MFA-Registrierungsrichtlinie

Identity Protection kann Organisationen beim Rollout von Azure AD Multi-Factor Authentication (MFA) mithilfe einer Richtlinie für den bedingten Zugriff unterstützen, die die Registrierung bei der Anmeldung erfordert. Wenn Sie diese Richtlinie aktivieren, können Sie sicherstellen, dass neue Benutzer in Ihrer Organisation sich am ersten Tag für MFA registrieren. Die mehrstufige Authentifizierung (MFA) ist eine der Methoden zur Eigenwartung für Risikoereignisse in Identity Protection. Eigenwartung ermöglicht Ihren Benutzern, selbst Maßnahmen zu ergreifen, um so das Helpdesk-Telefonaufkommen zu verringern.

Weitere Informationen zu Azure AD Multi-Factor Authentication finden Sie im Artikel [Funktionsweise: Azure AD Multi-Factor Authentication](../authentication/concept-mfa-howitworks.md).

## <a name="sign-in-risk-policy"></a>Richtlinie zum Anmelderisiko

Identity Protection analysiert Signale von jeder Anmeldung (sowohl in Echtzeit als auch offline) und berechnet eine Risikobewertung basierend auf der Wahrscheinlichkeit, dass die Anmeldung nicht vom Benutzer ausgeführt wurde. Administratoren können basierend auf diesem Risikobewertungssignal Entscheidungen treffen, um organisatorische Anforderungen zu erzwingen. Administratoren können den Zugriff blockieren, den Zugriff erlauben oder mehrstufige Authentifizierung anfordern, um den Zugriff zu erlauben.

Wenn Risiken erkannt werden, können die Benutzer die mehrstufige Authentifizierung zur Eigenwartung durchführen und das riskante Anmeldeereignis schließen, um unnötigen Aufwand für Administratoren zu vermeiden.

> [!NOTE] 
> Benutzer müssen sich vor Auslösen der Anmelderisikorichtlinie bereits für Azure AD Multi-Factor Authentication registriert haben.

### <a name="custom-conditional-access-policy"></a>Benutzerdefinierte Richtlinie für bedingten Zugriff

Administratoren können auch eine benutzerdefinierte Richtlinie für den bedingten Zugriff erstellen und das Anmelderisiko als Zuweisungsbedingung einschließen. Weitere Informationen zu Risiko als Bedingung in einer Zugriffsrichtlinie für bedingten Zugriff finden Sie im Artikel [Bedingter Zugriff: Bedingungen](../conditional-access/concept-conditional-access-conditions.md#sign-in-risk)

![Benutzerdefinierte Anmelderisikorichtlinie für den bedingten Zugriff](./media/concept-identity-protection-policies/identity-protection-custom-sign-in-policy.png)

## <a name="user-risk-policy"></a>Richtlinie zum Benutzerrisiko

Identity Protection kann das erwartete „normale“ Verhalten eines Benutzers berechnen, und Entscheidungen bezüglich des Risikos darauf basieren. Das Benutzerrisiko ist eine Berechnung der Wahrscheinlichkeit, dass eine Identität kompromittiert wurde. Administratoren können basierend auf diesem Risikobewertungssignal Entscheidungen treffen, um organisatorische Anforderungen zu erzwingen. Administratoren können mit [Azure AD Self-Service-Kennwortzurücksetzung](../authentication/howto-sspr-deployment.md) den Zugriff blockieren, den Zugriff erlauben oder den Benutzer auffordern, das Kennwort zu ändern, um Zugriff zu erhalten.

Wenn Risiken erkannt werden, können die Benutzer die Self-Service-Kennwortzurücksetzung zur Eigenwartung durchführen und das Benutzerrisikoereignis schließen, um unnötigen Aufwand für Administratoren zu vermeiden.

> [!NOTE] 
> Benutzer müssen sich vor Auslösen der Anmelderisikorichtlinie bereits für die Self-Service-Kennwortzurücksetzung registriert haben.

## <a name="next-steps"></a>Nächste Schritte

- [Aktivieren der Self-Service-Kennwortzurücksetzung in Azure AD](../authentication/howto-sspr-deployment.md)

- [Aktivieren von Azure AD Multi-Factor Authentication](../authentication/howto-mfa-getstarted.md)

- [Aktivieren der Registrierungsrichtlinie für Azure AD Multi-Factor Authentication](howto-identity-protection-configure-mfa-policy.md)

- [Aktivieren von Anmelde- und Benutzerrisikorichtlinien](howto-identity-protection-configure-risk-policies.md)
