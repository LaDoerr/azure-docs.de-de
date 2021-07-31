---
title: FSLogix-Profilcontainerfreigabe mit Azure Virtual Desktop – Azure
description: Es wird beschrieben, wie Sie einen FSLogix-Profilcontainer für einen Azure Virtual Desktop-Hostpool mit einer Dateifreigabe einrichten, die sich auf einem virtuellen Computer befindet.
author: Heidilohr
ms.topic: how-to
ms.date: 08/20/2019
ms.author: helohr
manager: femila
ms.openlocfilehash: a1398c23adcadaf245bae7271ed91e7d2a6763da
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111756093"
---
# <a name="create-a-profile-container-for-a-host-pool-using-a-file-share"></a>Erstellen eines Profilcontainers für einen Hostpool mit einer Dateifreigabe

Der Azure Virtual Desktop-Dienst stellt FSLogix-Profilcontainer als empfohlene Lösung für Benutzerprofile bereit. Wir raten davon ab, die Lösung „Benutzerprofil-Datenträger“ (User Profile Disk, UPD) zu nutzen. Sie wird in den zukünftigen Versionen von Azure Virtual Desktop als veraltet eingestuft.

In diesem Artikel ist beschrieben, wie Sie eine FSLogix-Profilcontainerfreigabe für einen Hostpool mit einer Dateifreigabe einrichten, die sich auf einem virtuellen Computer befindet. Es wird dringend empfohlen, anstelle von Dateifreigaben Azure Files zu verwenden. Weitere FSLogix-Dokumentation finden Sie auf der [FSLogix-Website](https://docs.fslogix.com/).

>[!NOTE]
>Wenn Sie nach Vergleichsmaterial zu den verschiedenen Speicheroptionen des FSLogix-Profilcontainers in Azure suchen, lesen Sie [Speicheroptionen für FSLogix-Profilcontainer](store-fslogix-profile.md).

## <a name="create-a-new-virtual-machine-that-will-act-as-a-file-share"></a>Erstellen eines neuen virtuellen Computers als Dateifreigabe

Achten Sie beim Erstellen des virtuellen Computers darauf, dass Sie ihn entweder in demselben virtuellen Netzwerk wie die virtuellen Computer des Hostpools oder in einem virtuellen Netzwerk anordnen, das über eine Verbindung mit den virtuellen Computern des Hostpools verfügt. Sie können einen virtuellen Computer auf unterschiedliche Arten erstellen:

- [Erstellen eines virtuellen Computers aus einem Azure-Katalogimage](../virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Erstellen eines virtuellen Computers aus einem verwalteten Image](../virtual-machines/windows/create-vm-generalized-managed.md)
- [Erstellen eines virtuellen Computers aus einem nicht verwalteten Image](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.compute/vm-from-user-image)

Nachdem Sie den virtuellen Computer erstellt haben, können Sie ihn in die Domäne einbinden, indem Sie die folgenden Schritte ausführen:

1. [Stellen Sie eine Verbindung mit dem virtuellen Computer her](../virtual-machines/windows/quick-create-portal.md#connect-to-virtual-machine), indem Sie die Anmeldeinformationen verwenden, die Sie beim Erstellen des virtuellen Computers angegeben haben.
2. Starten Sie auf dem virtuellen Computer die **Systemsteuerung**, und wählen Sie **System**.
3. Wählen Sie **Computername**, **Einstellungen ändern** und dann **Ändern...** .
4. Wählen Sie **Domäne**, und geben Sie dann die Active Directory-Domäne im virtuellen Netzwerk ein.
5. Authentifizieren Sie sich mit einem Domänenkonto, das über Berechtigungen für in die Domäne eingebundene Computer verfügt.

## <a name="prepare-the-virtual-machine-to-act-as-a-file-share-for-user-profiles"></a>Vorbereiten des virtuellen Computers für Verwendung als Dateifreigabe für Benutzerprofile

Hier ist eine allgemeine Anleitung zum Vorbereiten eines virtuellen Computers als Dateifreigabe für Benutzerprofile angegeben:

1. Fügen Sie die Azure Virtual Desktop Active Directory-Benutzer einer [Active Directory-Sicherheitsgruppe](/windows/security/identity-protection/access-control/active-directory-security-groups/) hinzu. Diese Sicherheitsgruppe wird zum Authentifizieren der Azure Virtual Desktop-Benutzer gegenüber dem virtuellen Dateifreigabencomputer verwendet, den Sie gerade erstellt haben.
2. [Stellen Sie die Verbindung mit dem virtuellen Dateifreigabencomputer her](../virtual-machines/windows/quick-create-portal.md#connect-to-virtual-machine).
3. Erstellen Sie auf dem virtuellen Dateifreigabencomputer einen Ordner auf **Laufwerk C**, der als Profilfreigabe verwendet wird.
4. Klicken Sie mit der rechten Maustaste auf den neuen Ordner, und wählen Sie dann nacheinander die Optionen **Eigenschaften**, **Freigabe** und **Erweiterte Freigabe...** .
5. Wählen Sie **Diesen Ordner freigeben**, **Berechtigungen...** und dann **Hinzufügen...** .
6. Suchen Sie nach der Sicherheitsgruppe, der Sie die Azure Virtual Desktop-Benutzer hinzugefügt haben, und stellen Sie dann sicher, dass für die Gruppe **Vollzugriff** festgelegt wurde.
7. Klicken Sie nach dem Hinzufügen der Sicherheitsgruppe mit der rechten Maustaste auf den Ordner, wählen Sie **Eigenschaften** und **Freigabe**, und kopieren Sie anschließend den **Netzwerkpfad** zur späteren Verwendung.

Weitere Informationen über Berechtigungen finden Sie in der [FSLogix-Dokumentation](/fslogix/fslogix-storage-config-ht/).

## <a name="configure-the-fslogix-profile-container"></a>Konfigurieren des FSLogix-Profilcontainers

Führen Sie auf allen Computern, die für den Hostpool registriert sind, Folgendes durch, um die virtuellen Computer mit der FSLogix-Software zu konfigurieren:

1. [Stellen Sie eine Verbindung mit dem virtuellen Computer her](../virtual-machines/windows/quick-create-portal.md#connect-to-virtual-machine), indem Sie die Anmeldeinformationen verwenden, die Sie beim Erstellen des virtuellen Computers angegeben haben.
2. Starten Sie einen Internetbrowser, und verwenden Sie [diesen Link](https://go.microsoft.com/fwlink/?linkid=2084562), um den FSLogix-Agent herunterzuladen.
3. Navigieren Sie in der ZIP-Datei entweder zur \\\\Win32\\-Release oder zur \\\\X64\\-Release, und führen Sie **FSLogixAppsSetup** aus, um den FSLogix-Agent zu installieren.  Weitere Informationen zum Installieren von FSLogix finden Sie unter [Herunterladen und Installieren von FSLogix](/fslogix/install-ht/).
4. Navigieren Sie zu **Programme** > **FSLogix** > **Apps**, um zu überprüfen, ob der Agent installiert wurde.
5. Führen Sie im Startmenü **RegEdit** als Administrator aus. Navigieren Sie zu **Computer\\HKEY_LOCAL_MACHINE\\Software\\FSLogix**.
6. Erstellen Sie einen Schlüssel mit dem Namen **Profile**.
7. Erstellen Sie für den Schlüssel „Profile“ die folgenden Werte:

| Name                | type               | Daten/Wert                        |
|---------------------|--------------------|-----------------------------------|
| Aktiviert             | DWORD              | 1                                 |
| VHDLocations        | Mehrteiliger Zeichenfolgenwert | „Netzwerkpfad für Dateifreigabe“     |

>[!IMPORTANT]
>Zum Schutz Ihrer Azure Virtual Desktop-Umgebung in Azure empfiehlt es sich, den eingehenden Port 3389 auf Ihren virtuellen Computern nicht zu öffnen. Für Azure Virtual Desktop muss der eingehende Port 3389 nicht geöffnet sein, damit Benutzer auf die virtuellen Computer des Hostpools zugreifen können. Wenn Sie den Port 3389 zur Problembehandlung öffnen müssen, verwenden Sie am besten den [Just-In-Time-Zugriff auf virtuelle Computer](../security-center/security-center-just-in-time.md).
