---
title: Verwenden von Cloudgruppen zum Verwalten von Rollenzuweisungen in Azure Active Directory | Microsoft-Dokumentation
description: 'Vorschau: Benutzerdefinierte Azure AD-Rollen zur Delegierung der Identitätsverwaltung. Verwalten Sie Azure-Rollenzuweisungen im Azure-Portal, mit PowerShell oder über die Graph-API.'
services: active-directory
author: rolyon
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: roles
ms.topic: article
ms.date: 05/05/2021
ms.author: rolyon
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: d037f93b1381fcd7f1816104f6fd7b2e1200c852
ms.sourcegitcommit: 070122ad3aba7c602bf004fbcf1c70419b48f29e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2021
ms.locfileid: "111438934"
---
# <a name="use-cloud-groups-to-manage-role-assignments-in-azure-active-directory-preview"></a>Verwenden von Cloudgruppen zum Verwalten von Rollenzuweisungen in Azure Active Directory (Vorschau)

In Azure Active Directory (Azure AD) wird eine öffentliche Vorschau eingeführt, in der Sie den in Azure AD integrierten Rollen eine Cloudgruppe zuweisen können. Mithilfe dieses Features können Sie Gruppen verwenden, um Administratorzugriff in Azure AD mit minimalem Aufwand von globalen Administratoren und Administratoren für privilegierte Rollen zu gewähren.

Betrachten Sie dieses Beispiel: Contoso hat Personal in verschiedenen geografischen Regionen eingestellt, um Kennwörter für Mitarbeiter in seiner Azure AD-Organisation zu verwalten und zurückzusetzen. Anstatt einen Administrator für privilegierte Rollen oder einen globalen Administrator zu bitten, jeder Person einzeln die Rolle „Helpdeskadministrator“ zuzuweisen, kann eine Gruppe „Contoso_Helpdesk_Administrators“ erstellt und der Rolle zugewiesen werden. Wenn Benutzer der Gruppe beitreten, wird ihnen die Rolle indirekt zugewiesen. Der vorhandene Governanceworkflow kann dann den Genehmigungsprozess und die Überwachung der Gruppenmitgliedschaft übernehmen, um sicherzustellen, dass nur berechtigte Benutzer Mitglieder der Gruppe sind und somit der Rolle „Helpdeskadministrator“ zugewiesen werden.

## <a name="how-this-feature-works"></a>Funktionsweise dieses Features

Erstellen Sie eine neue Microsoft 365- oder Sicherheitsgruppe, deren Eigenschaft „isAssignableToRole“ auf „true“ festgelegt ist. Sie können diese Eigenschaft auch aktivieren, wenn Sie eine Gruppe im Azure-Portal erstellen. Dazu aktivieren Sie die Option **Azure AD-Rollen können der Gruppe zugewiesen werden**. In beiden Fällen können Sie die Gruppe anschließend einer oder mehreren Azure AD-Rollen auf gleiche Weise zuweisen, wie Sie auch Benutzern Rollen zuweisen. In einer einzigen Azure AD-Organisation (Mandant) können maximal 300 Gruppen erstellt werden, denen Rollen zugewiesen werden können.

Wenn Sie nicht möchten, dass Mitglieder der Gruppe dauerhaften Zugriff auf die Rolle haben, können Sie Azure AD Privileged Identity Management verwenden. Weisen Sie eine Gruppe als berechtigtes Mitglied einer Azure AD-Rolle zu. Jedes Mitglied der Gruppe ist dann berechtigt, seine Zuweisung für die Rolle, der die Gruppe zugewiesen ist, aktivieren zu lassen. Anschließend kann das Gruppenmitglied seine Rollenzuweisung für eine festgelegte Zeitspanne aktivieren.

> [!Note]
> Sie müssen über eine aktualisierte Version von Privileged Identity Management verfügen, um einer Azure AD-Rolle über PIM eine Gruppe zuweisen zu können. Möglicherweise verwenden Sie eine ältere PIM-Version, da Ihre Azure AD-Organisation die Privileged Identity Management-API nutzt. Wenden Sie sich an den Alias pim_preview@microsoft.com, um Ihre Organisation umzustellen und die API zu aktualisieren. Weitere Informationen finden Sie unter [Azure AD-Rollen und Features in PIM](../privileged-identity-management/azure-ad-roles-features.md).

## <a name="why-we-enforce-creation-of-a-special-group-for-assigning-it-to-a-role"></a>Gründe für das Erstellen einer speziellen Gruppe für die Zuweisung zu einer Rolle

Wenn eine Gruppe einer Rolle zugewiesen wird, kann jeder IT-Administrator, der die Gruppenmitgliedschaft verwalten kann, indirekt auch die Mitgliedschaft in dieser Rolle verwalten. Angenommen, eine Gruppe namens „Contoso_User_Administrators“ wird der Rolle „Benutzeradministrator“ zugewiesen. Ein Exchange-Administrator, der die Gruppenmitgliedschaft ändern kann, kann sich selbst der Gruppe „Contoso_User_Administrators“ hinzufügen und auf diese Weise zu einem Benutzerkontoadministrator werden. Wie Sie sehen, kann ein Administrator seine Berechtigung auf nicht erwünschte Weise erweitern.

Mit Azure AD können Sie eine Gruppe, die einer Rolle zugewiesen ist, durch eine neue Eigenschaft für Gruppen namens „isAssignableToRole“ schützen. Nur Cloudgruppen, bei denen zum Erstellungszeitpunkt die Eigenschaft „isAssignableToRole“ auf „true“ festgelegt wurde, können einer Rolle zugewiesen werden. Diese Eigenschaft ist unveränderlich. Sobald eine Gruppe mit dieser Eigenschaft als „true“ erstellt wurde, kann dies nicht mehr geändert werden. Sie können die Eigenschaft nicht für eine vorhandene Gruppe festlegen. Die Zuweisung von Gruppen zu Rollen wurde von uns so konzipiert, dass potenzielle Verletzungen verhindert werden können:

- Nur globale Administratoren und Administratoren für privilegierte Rollen können eine Gruppe mit Rollenzuweisung erstellen (mit aktivierter isAssignableToRole-Eigenschaft).
- Es darf sich nicht um eine dynamische Azure AD-Gruppe handeln. Das heißt, der Mitgliedschaftstyp muss „Zugewiesen“ lauten. Das automatisierte Auffüllen dynamischer Gruppen kann dazu führen, dass der Gruppe ein unerwünschtes Konto hinzugefügt und sie somit der Rolle zugewiesen wird.
- Standardmäßig können nur globale Administratoren und Administratoren für privilegierte Rollen die Mitgliedschaft in einer Gruppe mit Rollenzuweisung verwalten, Sie können jedoch die Verwaltung von Gruppen mit Rollenzuweisung delegieren, indem Sie Gruppenbesitzer hinzufügen.
- Um eine Rechteerweiterung zu verhindern, können die Anmeldeinformationen oder die mehrstufige Authentifizierung von Mitgliedern und Besitzern einer Gruppe mit Rollenzuweisung nur von einem privilegierten Authentifizierungsadministrator oder einem globalen Administrator geändert werden.
- Keine Schachtelung. Eine Gruppe kann nicht als Mitglied einer Gruppe mit Rollenzuweisung hinzugefügt werden.

## <a name="limitations"></a>Einschränkungen

Folgende Szenarios werden derzeit nicht unterstützt:  

- Zuweisen lokaler Gruppen zu Azure AD-Rollen (integriert oder benutzerdefiniert)

## <a name="known-issues"></a>Bekannte Probleme

- *Nur Kunden mit Azure AD P2-Lizenz*: Weisen Sie einer Rolle eine Gruppe nicht über Azure AD und Privileged Identity Management (PIM) als „Aktiv“ zu. Vermeiden Sie es, einer Gruppe, der eine Rolle zugewiesen werden kann, eine Rolle bei ihrer Erstellung *und* später noch eine Rolle über PIM zuzuweisen. Dies führt zu Problemen, bei denen Benutzer ihre aktiven Rollenzuweisungen in PIM nicht sehen können und es nicht möglich ist, diese PIM-Zuweisung zu entfernen. Berechtigte Zuweisungen sind in diesem Szenario nicht beeinträchtigt. Wenn Sie versuchen, diese Zuweisung vorzunehmen, tritt möglicherweise ein unerwartetes Verhalten wie z. B. Folgendes auf:
  - Die Endzeit für die Rollenzuweisung wird möglicherweise falsch angezeigt.
  - Im PIM-Portal kann unter **Meine Rollen** nur eine Rollenzuweisung angezeigt werden, unabhängig davon, durch wie viele Methoden die Zuweisung gewährt wird (über eine oder mehrere Gruppen und direkt).
- *Nur Kunden mit Azure AD P2-Lizenz*: Auch nach dem Löschen der Gruppe wird sie noch immer als ein berechtigtes Mitglied der Rolle auf der PIM-Benutzeroberfläche angezeigt. Funktionell gibt es kein Problem. Es ist lediglich ein Cacheproblem im Azure-Portal.  
- Verwenden Sie das neue [Exchange Admin Center](https://admin.exchange.microsoft.com/) zum Zuweisen von Rollen über Gruppenmitgliedschaften. Das alte Exchange Admin Center unterstützt dieses Feature noch nicht. Exchange PowerShell-Cmdlets funktionieren den Erwartungen entsprechend.
- Das Azure Information Protection-Portal (klassisches Portal) erkennt die Rollenmitgliedschaft über eine Gruppe noch nicht. Sie können [zur Plattform für einheitliche Vertraulichkeitsbezeichnungen migrieren](/azure/information-protection/configure-policy-migrate-labels) und dann das Office 365 Security & Compliance Center verwenden, um Gruppenzuweisungen für das Verwalten von Rollen zu nutzen.
- [Apps Admin Center](https://config.office.com/) unterstützt dieses Feature noch nicht. Weisen Sie Benutzer direkt der Rolle als Office Apps-Administrator zu.
- Dieses Feature wird von [Microsoft 365 Compliance Center](https://compliance.microsoft.com/) noch nicht unterstützt. Weisen Sie Benutzer direkt entsprechenden Azure AD-Rollen zu, um dieses Portal zu verwenden.

Diese Probleme werden derzeit behoben.

## <a name="license-requirements"></a>Lizenzanforderungen

Für die Verwendung dieses Features ist eine Azure AD Premium P1-Lizenz erforderlich. Um auch Privileged Identity Management für die Just-In-Time-Rollenaktivierung zu verwenden, benötigen Sie eine Azure AD Premium P2-Lizenz. Um die richtige Lizenz für Ihre Anforderungen zu ermitteln, lesen Sie [Vergleich der allgemein verfügbaren Features der Editionen Free und Premium](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen einer Gruppe, der Rollen zugeordnet werden können](groups-create-eligible.md)
- [Zuweisen einer Rolle zu einer Gruppe, der Rollen zugeordnet werden können](groups-assign-role.md)
