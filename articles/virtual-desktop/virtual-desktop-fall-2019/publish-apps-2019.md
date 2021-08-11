---
title: Veröffentlichen integrierter Anwendungen in Azure Virtual Desktop (klassisch) – Azure
description: So veröffentlichen Sie integrierte Anwendungen in Azure Virtual Desktop (klassisch).
author: Heidilohr
ms.topic: how-to
ms.date: 03/30/2020
ms.author: helohr
manager: femila
ms.openlocfilehash: 0a375f61098a1000b101be74d137fabd023cf4d5
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111751773"
---
# <a name="publish-built-in-apps-in-azure-virtual-desktop-classic"></a>Veröffentlichen integrierter Anwendungen in Azure Virtual Desktop (klassisch)

>[!IMPORTANT]
>Dieser Inhalt gilt für Azure Virtual Desktop (klassisch). Der Dienst unterstützt keine Azure Virtual Desktop-Objekte in Azure Resource Manager. Wenn Sie Azure Virtual Desktop-Objekte in Azure Resource Manager verwalten möchten, helfen Ihnen die Informationen in [diesem Artikel](../publish-apps.md) weiter.

In diesem Artikel erfahren Sie, wie Sie Apps in Ihrer Azure Virtual Desktop-Umgebung veröffentlichen.

## <a name="publish-built-in-apps"></a>Veröffentlichen von integrierten Apps

So veröffentlichen Sie eine integrierte App:

1. Stellen Sie eine Verbindung mit einem der virtuellen Computer in Ihrem Hostpool her.
2. Rufen Sie den **PackageFamilyName** der zu veröffentlichenden App ab. Befolgen Sie dazu die Anweisungen in [diesem Artikel](/powershell/module/appx/get-appxpackage).
3. Führen Sie schließlich das folgende Cmdlet aus; ersetzen Sie dabei `<PackageFamilyName>` durch den **PackageFamilyName** aus dem vorherigen Schritt:

   ```powershell
   New-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname> -Name <remoteappname> -FriendlyName <remoteappname> -FilePath "shell:appsFolder\<PackageFamilyName>!App"
   ```

>[!NOTE]
> Azure Virtual Desktop unterstützt nur das Veröffentlichen von Apps mit Installationsspeicherorten, die mit `C:\Program Files\Windows Apps`beginnen.

## <a name="update-app-icons"></a>Aktualisieren von App-Symbolen

Nach dem Veröffentlichen einer App weist diese das Windows-Standardsymbol für Apps und nicht ihr normales App-Symbolbild auf. Wenn Sie das Symbol in das normale Symbol ändern möchten, legen Sie das Bild für das gewünschte Symbol auf einer Netzwerkfreigabe ab. Unterstützte Bildformate sind PNG, BMP, GIF, JPG, JPEG und ICO.

## <a name="publish-microsoft-edge"></a>Veröffentlichen von Microsoft Edge

Der Vorgang zum Veröffentlichen von Microsoft Edge unterscheidet sich etwas von dem Vorgang beim Veröffentlichen anderer Apps. Führen Sie dieses Cmdlet aus, um Microsoft Edge mit der standardmäßigen Startseite zu veröffentlichen:

```powershell
New-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname> -Name <remoteappname> -FriendlyName <remoteappname> -FilePath "shell:Appsfolder\Microsoft.MicrosoftEdge_8wekyb3d8bbwe!MicrosoftEdge"
```

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich unter [Anpassen von Feeds für Azure Virtual Desktop-Benutzer](customize-feed-virtual-desktop-users-2019.md), wie Sie Feeds konfigurieren, um festzulegen, wie Apps für Benutzer angezeigt werden.
- Machen Sie sich unter [Einrichten des MSIX-Features zum Anfügen von Apps](../app-attach.md) mit dem MSIX-Feature zum Anfügen von Apps vertraut.

