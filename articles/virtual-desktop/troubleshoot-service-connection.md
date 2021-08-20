---
title: Problembehandlung bei Azure Virtual Desktop-Dienstverbindungen – Azure
description: Informationen zum Beheben von Problemen beim Einrichten von Dienstverbindungen in einer Azure Virtual Desktop-Mandantenumgebung.
author: Heidilohr
ms.topic: troubleshooting
ms.date: 10/15/2020
ms.author: helohr
manager: femila
ms.openlocfilehash: 8f87adc97039eda25b29116108069d685fd0a8bd
ms.sourcegitcommit: 9339c4d47a4c7eb3621b5a31384bb0f504951712
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/14/2021
ms.locfileid: "113767130"
---
# <a name="azure-virtual-desktop-service-connections"></a>Azure Virtual Desktop-Dienstverbindungen

>[!IMPORTANT]
>Dieser Inhalt gilt für Azure Virtual Desktop mit Azure Virtual Desktop-Objekten für Azure Resource Manager. Wenn Sie Azure Virtual Desktop (klassisch) ohne Azure Resource Manager-Objekte verwenden, finden Sie weitere Informationen in [diesem Artikel](./virtual-desktop-fall-2019/troubleshoot-service-connection-2019.md).

Verwenden Sie diesen Artikel zum Beheben von Problemen mit Azure Virtual Desktop-Clientverbindungen.

## <a name="provide-feedback"></a>Feedback geben

In der [Azure Virtual Desktop Tech Community](https://techcommunity.microsoft.com/t5/azure-virtual-desktop/bd-p/AzureVirtualDesktopForum) können Sie uns Feedback geben und sich mit dem Produktteam und anderen aktiven Communitymitgliedern über den Azure Virtual Desktop-Dienst austauschen.

## <a name="user-connects-but-nothing-is-displayed-no-feed"></a>Der Benutzer stellt eine Verbindung her, aber es wird nichts angezeigt (kein Feed).

Ein Benutzer kann Remotedesktopclients starten und sich authentifizieren, ihm werden jedoch im Websuchfeed keine Symbole angezeigt.

1. Vergewissern Sie sich, dass der Benutzer, der die Probleme meldet, mithilfe dieser Befehlszeile Anwendungsgruppen zugewiesen wurde:

     ```powershell
     Get-AzRoleAssignment -SignInName <userupn>
     ```

2. Vergewissern Sie sich, dass sich der Benutzer mit den richtigen Anmeldeinformationen anmeldet.

3. Wenn der Webclient verwendet wird, vergewissern Sie sich, dass keine Probleme mit zwischengespeicherten Anmeldeinformationen vorliegen.

4. Wenn der Benutzer einer Azure AD-Benutzergruppe (Active Directory) angehört, stellen Sie sicher, dass die Benutzergruppe eine Sicherheitsgruppe und keine Verteilergruppe ist. Azure AD-Verteilergruppen werden von Azure Virtual Desktop nicht unterstützt.

## <a name="user-loses-existing-feed-and-no-remote-resource-is-displayed-no-feed"></a>Der Benutzer verliert den vorhandenen Feed, und es wird keine Remoteressource angezeigt (kein Feed)

Dieser Fehler tritt normalerweise auf, nachdem ein Benutzer sein Abonnement von einem Azure AD-Mandanten zu einem anderen verschoben hat. Aufgrund dieses Fehlers kann der Dienst die Benutzerzuweisungen nicht mehr verfolgen, da diese weiterhin an den alten Azure AD-Mandanten gebunden sind.

Um dieses Problem zu beheben, müssen Sie lediglich die Benutzer ihren jeweiligen Anwendungsgruppen erneut zuweisen.

Dies kann auch der Fall sein, wenn ein CSP-Anbieter das Abonnement erstellt und dann an den Kunden übertragen hat. Um dieses Problem zu beheben, registrieren Sie den Ressourcenanbieter erneut.

1. Melden Sie sich beim Azure-Portal an.
2. Wechseln Sie zu **Abonnements**, und wählen Sie Ihr Abonnement aus.
3. Wählen Sie im Menü links auf der Seite die Option **Ressourcenanbieter** aus.
4. Suchen Sie nach **Microsoft.DesktopVirtualization**, wählen Sie die Option aus, und wählen Sie dann **Erneut registrieren** aus.

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über die Problembehandlung von Azure Virtual Desktop und die Eskalationspfade finden Sie unter [Problembehandlung: Übersicht, Feedback und Support](troubleshoot-set-up-overview.md).
- Informationen zur Problembehandlung beim Erstellen einer Azure Virtual Desktop-Umgebung und eines Hostpools in einer Azure Virtual Desktop-Umgebung finden Sie unter [Umgebungs- und Hostpoolerstellung](troubleshoot-set-up-issues.md).
- Informationen zur Problembehandlung bei der Konfiguration eines virtuellen Computers (VM) in Azure Virtual Desktop finden Sie unter [Konfiguration des virtuellen Sitzungshostcomputers](troubleshoot-vm-configuration.md).
- Informationen zur Problembehandlung beim Azure Virtual Desktop-Agent oder der Sitzungskonnektivität finden Sie unter [Beheben häufiger Probleme mit dem Azure Virtual Desktop-Agent](troubleshoot-agent.md).
- Informationen zur Problembehandlung bei der Verwendung von PowerShell mit Azure Virtual Desktop finden Sie unter [Azure Virtual Desktop – PowerShell](troubleshoot-powershell.md).
- Ein Tutorial zur Problembehandlung finden Sie unter [Tutorial: Problembehandlung von Bereitstellungen der Resource Manager-Vorlage](../azure-resource-manager/templates/template-tutorial-troubleshoot.md).
