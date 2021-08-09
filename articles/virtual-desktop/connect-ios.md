---
title: 'Herstellen einer Verbindung mit Azure Virtual Desktop unter iOS: Azure'
description: Informationen zum Herstellen einer Verbindung mit Azure Virtual Desktop mithilfe des iOS-Clients.
author: Heidilohr
ms.topic: how-to
ms.date: 02/08/2020
ms.author: helohr
manager: femila
ms.openlocfilehash: 6ee7b5e8182cf136d16949a30891325bd3f7fabd
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111756723"
---
# <a name="connect-to-azure-virtual-desktop-with-the-ios-client"></a>Herstellen einer Verbindung mit Azure Virtual Desktop mithilfe des iOS-Clients

> Gilt für: iOS 13.0 oder höher. Kompatibel mit iPhone, iPad und iPod Touch.

>[!IMPORTANT]
>Dieser Inhalt gilt für Azure Virtual Desktop mit Azure Virtual Desktop-Objekten für Azure Resource Manager. Wenn Sie Azure Virtual Desktop (klassisch) ohne Azure Resource Manager-Objekte verwenden, finden Sie weitere Informationen in [diesem Artikel](./virtual-desktop-fall-2019/connect-ios-2019.md).

Mit unserem herunterladbaren Client können Sie über Ihr iOS-Gerät auf Azure Virtual Desktop-Ressourcen zugreifen. In dieser Anleitung erfahren Sie, wie Sie den iOS-Client einrichten.

## <a name="install-the-ios-client"></a>Installieren des iOS-Clients

Beginnen Sie mit dem [Herunterladen](https://aka.ms/rdios) des Clients, und installieren Sie ihn auf Ihrem iOS-Gerät.

## <a name="subscribe-to-a-feed"></a>Abonnieren eines Feeds

Abonnieren Sie den Feed, der vom Administrator bereitgestellt wird, um die Liste der verwalteten Ressourcen zu erhalten, auf die Sie über Ihr iOS-Gerät zugreifen können.

Abonnieren Sie einen Feed wie folgt:

1. Tippen Sie im Verbindungscenter auf **+** , und tippen Sie dann auf **Arbeitsbereich hinzufügen**.
2. Geben Sie die Feed-URL in das Feld **Feed-URL** ein. Die Feed-URL kann eine URL oder eine E-Mail-Adresse sein.
   - Wenn Sie eine URL verwenden, verwenden Sie die URL, die Ihr Administrator Ihnen mitgeteilt hat. Normalerweise lautet die URL <https://rdweb.wvd.microsoft.com/api/arm/feeddiscovery>.
   - Um E-Mail zu verwenden, gib deine E-Mail-Adresse ein. Dies weist den Client an, nach einer URL zu suchen, die Ihrer E-Mail-Adresse zugeordnet ist, wenn der Administrator den Server auf diese Weise konfiguriert hat.
   - Verwenden Sie <https://rdweb.wvd.azure.us/api/arm/feeddiscovery>, um eine Verbindung über das US Gov-Portal herzustellen.
3. Tippen Sie auf **Weiter**.
4. Geben Sie Ihre Anmeldeinformationen ein, wenn Sie dazu aufgefordert werden.
   - Geben Sie als **Benutzername** den Benutzernamen mit der Berechtigung für den Zugriff auf Ressourcen an.
   - Geben Sie als **Kennwort** das diesem Benutzernamen zugeordnete Kennwort an.
   - Sie werden möglicherweise auch aufgefordert, zusätzliche Faktoren anzugeben, wenn Ihr Administrator die Authentifizierung entsprechend konfiguriert hat.
5. Tippen Sie auf **Speichern**.

Danach sollten die Remoteressourcen im Connection Center angezeigt werden.

Nachdem du einen Feed abonniert hast, wird der Inhalt dieses Feeds automatisch regelmäßig aktualisiert. Ressourcen können basierend auf Änderungen durch Ihren Administrator hinzugefügt, geändert oder entfernt werden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Verwenden des iOS-Clients finden Sie in der Dokumentation [Erste Schritte mit dem iOS-Client](/windows-server/remote/remote-desktop-services/clients/remote-desktop-ios/).
