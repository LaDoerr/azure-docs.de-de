---
title: 'Herstellen einer Verbindung mit Azure Virtual Desktop (klassisch) unter Windows 10 oder 7: Azure'
description: Informationen zum Herstellen einer Verbindung mit Azure Virtual Desktop (klassisch) mithilfe des Windows Desktop-Clients.
author: Heidilohr
ms.topic: how-to
ms.date: 07/16/2020
ms.author: helohr
manager: femila
ms.openlocfilehash: 004fff41e30ca9c60d51847035e30262e359270e
ms.sourcegitcommit: b044915306a6275c2211f143aa2daf9299d0c574
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2021
ms.locfileid: "113031119"
---
# <a name="connect-with-the-windows-desktop-classic-client"></a>Herstellen einer Verbindung mit dem Windows-Desktopclient (klassisch)

> Gilt für: Windows 7, Windows 10 und Windows 10 IoT Enterprise

>[!IMPORTANT]
>Dieser Inhalt gilt für Azure Virtual Desktop (klassisch). Der Dienst unterstützt keine Azure Virtual Desktop-Objekte in Azure Resource Manager. Wenn Sie Azure Virtual Desktop-Objekte in Azure Resource Manager verwalten möchten, helfen Ihnen die Informationen in [diesem Artikel](../user-documentation/connect-windows-7-10.md) weiter.

Mit dem Windows Desktop-Client können Sie auf Geräten mit Windows 7, Windows 10 und Windows 10 IoT Enterprise auf Azure Virtual Desktop-Ressourcen zugreifen. Der Client unterstützt weder Windows 8 noch Windows 8.1.

>[!NOTE]
>Der Windows-Client verwendet standardmäßig automatisch Azure Virtual Desktop (klassisch). Wenn der Client jedoch erkennt, dass der Benutzer auch über Azure Resource Manager-Ressourcen verfügt, werden die Ressourcen automatisch hinzugefügt, oder der Benutzer wird benachrichtigt, dass sie verfügbar sind.

> [!IMPORTANT]
> Azure Virtual Desktop unterstützt nicht den RemoteApp-Client und den Client für Desktopverbindungen (RADC). Der Client für die Remotedesktopverbindung (MSTSC) wird ebenfalls nicht unterstützt.

> [!IMPORTANT]
> Azure Virtual Desktop verfügt derzeit nicht über Unterstützung für den Remotedesktopclient aus dem Windows Store.

## <a name="install-the-windows-desktop-client"></a>Installieren des Windows Desktop-Clients

Wählen Sie den Client aus, der Ihrer Windows-Version entspricht:

- [Windows (64-Bit)](https://go.microsoft.com/fwlink/?linkid=2068602)
- [Windows 32-Bit](https://go.microsoft.com/fwlink/?linkid=2098960)
- [Windows ARM64](https://go.microsoft.com/fwlink/?linkid=2098961)

Sie können den Client für den aktuellen Benutzer installieren. Dafür sind keine Administratorrechte erforderlich. Der Administrator kann den Client aber auch so installieren und konfigurieren, dass alle Benutzer auf dem Gerät darauf zugreifen können.

Nach der Installation kann der Client über das Startmenü gestartet werden, indem Sie nach **Remote Desktop** suchen.

## <a name="subscribe-to-a-workspace"></a>Abonnieren eines Arbeitsbereichs

Es gibt zwei Möglichkeiten zum Abonnieren eines Arbeitsbereichs. Der Client kann versuchen, die Ressourcen, die dir über dein Geschäfts- oder Schul-/Unikonto zur Verfügung stehen, zu ermitteln, oder du kannst die URL, unter der deine Ressourcen zu finden sind, direkt angeben, falls der Client sie nicht finden kann. Sobald du einen Arbeitsbereich abonniert hast, kannst du Ressourcen mit einer der folgenden Methoden starten:

- Wechsle zum Connection Center, und doppelklicke auf eine Ressource, um sie zu starten.
- Alternativ kannst du zum Startmenü navigieren und nach einem Ordner mit dem Namen des Arbeitsbereichs suchen oder den Namen der Ressource in die Suchleiste eingeben.

### <a name="subscribe-with-a-user-account"></a>Abonnieren mit einem Benutzerkonto

1. Klicken Sie auf der Hauptseite des Clients auf **Abonnieren**.
2. Melden Sie sich mit Ihrem Benutzerkonto an, wenn Sie dazu aufgefordert werden.
3. Die Ressourcen werden nach Arbeitsbereichen gruppiert im Connection Center angezeigt.

### <a name="subscribe-with-a-url"></a>Abonnieren mit einer URL

1. Klicken Sie auf der Hauptseite des Clients auf **Subscribe with URL** (Mit URL abonnieren).
2. Geben Sie die Arbeitsbereichs-URL oder Ihre E-Mail-Adresse ein:
   - Verwende im Fall einer **Arbeitsbereichs-URL** diejenige, die du vom Administrator erhalten hast. Wenn Sie über Azure Virtual Desktop auf Ressourcen zugreifen, können Sie eine der folgenden URLs verwenden:
     - Azure Virtual Desktop (klassisch): `https://rdweb.wvd.microsoft.com/api/feeddiscovery/webfeeddiscovery.aspx`
     - Azure Virtual Desktop: `https://rdweb.wvd.microsoft.com/api/arm/feeddiscovery`
   - Wenn Sie stattdessen das Feld **E-Mail** verwenden, geben Sie Ihre E-Mail-Adresse ein. Dies weist den Client an, nach einer URL zu suchen, die Ihrer E-Mail-Adresse zugeordnet ist, wenn Ihr Administrator [E-Mail-Ermittlung](/windows-server/remote/remote-desktop-services/rds-email-discovery) eingerichtet hat.
3. Wählen Sie **Weiter** aus.
4. Melden Sie sich mit Ihrem Benutzerkonto an, wenn Sie dazu aufgefordert werden.
5. Die Ressourcen sollten im Connection Center nach Arbeitsbereich gruppiert angezeigt werden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung des Windows Desktop-Clients finden Sie unter [Erste Schritte mit dem Windows Desktop-Client](/windows-server/remote/remote-desktop-services/clients/windowsdesktop/).
