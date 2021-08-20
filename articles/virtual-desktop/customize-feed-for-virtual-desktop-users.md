---
title: Anpassen des Feeds für Azure Virtual Desktop-Benutzer – Azure
description: Erfahren Sie, wie Sie den Feed für Azure Virtual Desktop-Benutzer mit PowerShell-Cmdlets anpassen.
author: Heidilohr
ms.topic: how-to
ms.date: 09/14/2019
ms.author: helohr
ms.custom: devx-track-azurepowershell
manager: femila
ms.openlocfilehash: d2a1e8df23bca2e57f2d26bd83fc06ad93847903
ms.sourcegitcommit: b044915306a6275c2211f143aa2daf9299d0c574
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2021
ms.locfileid: "113032937"
---
# <a name="customize-the-feed-for-azure-virtual-desktop-users"></a>Anpassen des Feeds für Azure Virtual Desktop-Benutzer

>[!IMPORTANT]
>Dieser Inhalt gilt für Azure Virtual Desktop mit Azure Virtual Desktop-Objekten für Azure Resource Manager. Wenn Sie Azure Virtual Desktop (klassisch) ohne Azure Resource Manager-Objekte verwenden, finden Sie weitere Informationen in [diesem Artikel](./virtual-desktop-fall-2019/customize-feed-virtual-desktop-users-2019.md).

Sie können den Feed anpassen, damit die RemoteApp- und Remotedesktopressourcen den Benutzern auf erkennbare Weise angezeigt werden.

## <a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass Sie das Azure Virtual Desktop PowerShell-Modul bereits heruntergeladen und installiert haben. Wenn dies nicht der Fall ist, befolgen Sie die Anweisungen unter [Einrichten des PowerShell-Moduls](powershell-module.md).

## <a name="customize-the-display-name-for-a-remoteapp"></a>Anpassen des Anzeigenamens für eine RemoteApp

Sie können den Anzeigenamen für eine veröffentlichte RemoteApp ändern, indem Sie ihn festlegen. Standardmäßig ist der Anzeigename identisch mit dem Namen des RemoteApp-Programms.

Um eine Liste der veröffentlichten RemoteApps für eine App-Gruppe abzurufen, führen Sie das folgende PowerShell-Cmdlet aus:

```powershell
Get-AzWvdApplication -ResourceGroupName <resourcegroupname> -ApplicationGroupName <appgroupname>
```

Um einer RemoteApp einen Anzeigenamen zuzuweisen, führen Sie das folgende Cmdlet mit den erforderlichen Parametern aus:

```powershell
Update-AzWvdApplication -ResourceGroupName <resourcegroupname> -ApplicationGroupName <appgroupname> -Name <applicationname> -FriendlyName <newfriendlyname>
```

Angenommen, Sie haben die aktuellen Anwendungen mit dem folgenden Beispiel-Cmdlet abgerufen:

```powershell
Get-AzWvdApplication -ResourceGroupName 0301RG -ApplicationGroupName 0301RAG | format-list
```

Die Ausgabe würde wie folgt aussehen:

```powershell
CommandLineArgument :
CommandLineSetting  : DoNotAllow
Description         :
FilePath            : C:\Program Files\Windows NT\Accessories\wordpad.exe
FriendlyName        : Microsoft Word
IconContent         : {0, 0, 1, 0…}
IconHash            : --iom0PS6XLu-EMMlHWVW3F7LLsNt63Zz2K10RE0_64
IconIndex           : 0
IconPath            : C:\Program Files\Windows NT\Accessories\wordpad.exe
Id                  : /subscriptions/<subid>/resourcegroups/0301RG/providers/Microsoft.DesktopVirtualization/applicationgroups/0301RAG/applications/Microsoft Word
Name                : 0301RAG/Microsoft Word
ShowInPortal        : False
Type                : Microsoft.DesktopVirtualization/applicationgroups/applications
```
Führen Sie dieses Cmdlet aus, um den Anzeigenamen zu aktualisieren:

```powershell
Update-AzWvdApplication -GroupName 0301RAG -Name "Microsoft Word" -FriendlyName "WordUpdate" -ResourceGroupName 0301RG -IconIndex 0 -IconPath "C:\Program Files\Windows NT\Accessories\wordpad.exe" -ShowInPortal:$true -CommandLineSetting DoNotallow -FilePath "C:\Program Files\Windows NT\Accessories\wordpad.exe"
```

Führen Sie dieses Cmdlet aus, um zu bestätigen, dass Sie den Anzeigenamen erfolgreich aktualisiert haben:

```powershell
Get-AzWvdApplication -ResourceGroupName 0301RG -ApplicationGroupName 0301RAG | format-list FriendlyName
```

Die Ausgabe des Cmdlets sollte wie folgt aussehen:

```powershell
FriendlyName        : WordUpdate
```

## <a name="customize-the-display-name-for-a-remote-desktop"></a>Anpassen des Anzeigenamens für einen Remotedesktop

Sie können den Anzeigenamen für einen veröffentlichten Remotedesktop ändern, indem Sie einen Anzeigenamen festlegen. Wenn Sie manuell mit PowerShell einen Hostpool und eine Desktop-App-Gruppe erstellt haben, lautet der Anzeigename standardmäßig „Sitzungsdesktop“. Wenn Sie den Hostpool und die Desktop-App-Gruppe über die Azure Resource Manager-Vorlage von GitHub oder das Angebot im Azure Marketplace erstellt haben, ist der Standardanzeigename mit dem Namen des Hostpools identisch.

Führen Sie zum Abrufen der Remotedesktopressource das folgende PowerShell-Cmdlet aus:

```powershell
Get-AzWvdDesktop -ResourceGroupName <resourcegroupname> -ApplicationGroupName <appgroupname> -Name <applicationname>
```

Führen Sie zum Zuweisen eines Anzeigenamens zur Remotedesktopressource das folgende PowerShell-Cmdlet aus:

```powershell
Update-AzWvdDesktop -ResourceGroupName <resourcegroupname> -ApplicationGroupName <appgroupname> -Name <applicationname> -FriendlyName <newfriendlyname>
```

## <a name="customize-a-display-name-in-azure-portal"></a>Anpassen eines Anzeigenamens im Azure-Portal

Sie können den Anzeigenamen für einen veröffentlichten Remotedesktop ändern, indem Sie im Azure-Portal einen Anzeigenamen festlegen.

1. Melden Sie sich unter <https://portal.azure.com> beim Azure-Portal an.

2. Suchen Sie nach **Azure Virtual Desktop**.

3. Wählen Sie unter „Dienste“ die Option **Azure Virtual Desktop** aus.

4. Wählen Sie auf der Seite „Azure Virtual Desktop“ auf der linken Bildschirmseite **Anwendungsgruppen** aus, und wählen Sie dann den Namen der App-Gruppe aus, den Sie bearbeiten möchten. (Wenn Sie beispielsweise den Anzeigenamen der Desktop-App-Gruppe bearbeiten möchten, klicken Sie auf die App-Gruppe mit dem Namen **Desktop**.)

5. Wählen Sie im Menü auf der linken Bildschirmseite **Anwendungen** aus.

6. Wählen Sie die Anwendung aus, die Sie aktualisieren möchten, und geben Sie dann einen neuen **Anzeigenamen** ein.

7. Wählen Sie **Speichern** aus. Für die Anwendung, die Sie bearbeitet haben, sollte jetzt der aktualisierte Name angezeigt werden.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie den Feed für Benutzer angepasst haben, können Sie sich bei einem Azure Virtual Desktop-Client anmelden, um ihn zu testen. Wechseln Sie dafür zu den Vorgehensweisen für das Herstellen einer Verbindung mit Azure Virtual Desktop:

 * [Herstellen einer Verbindung mit Windows 10 oder Windows 7](./user-documentation/connect-windows-7-10.md)
 * [Herstellen einer Verbindung mit dem Webclient](./user-documentation/connect-web.md)
 * [Herstellen einer Verbindung mit dem Android-Client](./user-documentation/connect-android.md)
 * [Herstellen einer Verbindung mit dem iOS-Client](./user-documentation/connect-ios.md)
 * [Herstellen einer Verbindung mit dem macOS-Client](./user-documentation/connect-macos.md)
