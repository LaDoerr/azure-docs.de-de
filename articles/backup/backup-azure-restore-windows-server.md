---
title: Wiederherstellen von Dateien auf Windows Server mit dem MARS-Agent
description: In diesem Artikel erfahren Sie, wie Sie in Azure gespeicherte Daten mit dem Microsoft Azure Recovery Services-Agent (MARS) auf einem Windows-Server oder Windows-Computer wiederherstellen.
ms.topic: conceptual
ms.date: 09/07/2018
ms.openlocfilehash: fd17d4f2199440c72e5b0ceb4ec7fda7ab40d837
ms.sourcegitcommit: 832e92d3b81435c0aeb3d4edbe8f2c1f0aa8a46d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2021
ms.locfileid: "111555108"
---
# <a name="restore-files-to-windows-server-using-the-mars-agent"></a>Wiederherstellen von Dateien auf Windows Server mit dem MARS-Agent

In diesem Artikel wird das Wiederherstellen von Daten aus einem Sicherungstresor erläutert. Zum Wiederherstellen von Daten verwenden Sie den Assistenten zum Wiederherstellen von Daten im Microsoft Azure Recovery Services-Agent (MARS). Ihre Möglichkeiten:

* Wiederherstellen von Daten auf dem gleichen Computer, auf dem die Sicherungen erstellt wurden
* Wiederherstellen auf einem anderen Computer.

Verwenden Sie das Feature für sofortige Wiederherstellung, um eine beschreibbare Wiederherstellungspunkt-Momentaufnahme als Wiederherstellungsvolume einzubinden. Sie können dann das Wiederherstellungsvolume durchsuchen und Dateien auf einen lokalen Computer kopieren und so Dateien selektiv wiederherstellen.

> [!NOTE]
> Das [Azure Backup-Update vom Januar 2017](https://support.microsoft.com/help/3216528/azure-backup-update-for-microsoft-azure-recovery-services-agent-januar) ist erforderlich, wenn Sie die sofortige Wiederherstellung verwenden möchten, um Daten wiederherzustellen. Außerdem müssen die Sicherungsdaten in Tresoren an einem der Orte, die im Supportartikel aufgeführt sind, geschützt sein. Unter [Azure Backup-Update vom Januar 2017](https://support.microsoft.com/help/3216528/azure-backup-update-for-microsoft-azure-recovery-services-agent-januar) finden Sie die neueste Liste der Regionen, in denen die sofortige Wiederherstellung unterstützt wird.
>

Verwenden Sie im Azure-Portal die sofortige Wiederherstellung mit Recovery Services-Tresoren. Wenn Sie Daten in Sicherungstresoren gespeichert haben, wurden sie automatisch in Recovery Services-Tresore umgewandelt. Wenn Sie die sofortige Wiederherstellung einsetzen möchten, laden Sie das MARS-Update herunter und befolgen Sie die Verfahren, in denen die sofortige Wiederherstellung erwähnt wird.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="use-instant-restore-to-recover-data-to-the-same-machine"></a>Verwenden der sofortigen Wiederherstellung zum Wiederherstellen von Daten auf demselben Computer

Wenn Sie versehentlich eine Datei gelöscht haben und sie auf demselben Computer (auf dem die Sicherung erstellt wurde) wiederherstellen möchten, können Sie die folgenden Schritte zum Wiederherstellen der Daten ausführen.

1. Öffnen Sie das Snap-In **Microsoft Azure Backup**. Falls Sie nicht wissen, wo das Snap-In installiert wurde, können Sie den für **Microsoft Azure Backup** verwendeten Computer oder Server durchsuchen.

    Die Desktop-App sollten in den Suchergebnissen angezeigt werden.

2. Wählen Sie **Daten wiederherstellen** aus, um den Assistenten zu starten.

    ![Screenshot von Azure Backup mit hervorgehobener Option „Daten wiederherstellen“ (Wiederherstellen auf demselben Computer)](./media/backup-azure-restore-windows-server/recover.png)

3. Wählen Sie im Bereich **Erste Schritte** die Option **Dieser Server (`<server name>`)**  > **Weiter** aus, um die Daten auf demselben Server oder Computer wiederherzustellen.

    ![Screenshot der Seite „Erste Schritte“ des Assistenten zum Wiederherstellen von Daten (Wiederherstellen auf demselben Computer)](./media/backup-azure-restore-windows-server/samemachine_gettingstarted_instantrestore.png)

4. Wählen Sie auf der Seite **Wiederherstellungsmodus auswählen** die Option **Einzelne Dateien und Ordner** > **Weiter** aus.

    ![Screenshot der Seite „Wiederherstellungsmodus auswählen“ des Assistenten zum Wiederherstellen von Daten (Wiederherstellen auf demselben Computer)](./media/backup-azure-restore-windows-server/samemachine_selectrecoverymode_instantrestore.png)
   > [!IMPORTANT]
   > Für die Option zum Wiederherstellen von einzelnen Dateien und Ordnern ist .NET Framework 4.5.2 oder höher erforderlich. Falls die Option **Einzelne Dateien und Ordner** nicht angezeigt wird, müssen Sie .NET Framework auf Version 4.5.2 oder höher aktualisieren und es erneut versuchen.

   > [!TIP]
   > Die Option **Individual Files and Folders** (Einzelne Dateien und Ordner) ermöglicht den schnellen Zugriff auf Daten des Wiederherstellungspunkts. Sie eignet sich für die Wiederherstellung einzelner Dateien und wird für eine Gesamtgröße von unter 80 GB empfohlen. Sie bietet Übertragungs- oder Kopiergeschwindigkeiten von bis zu 6 MBit/s während der Wiederherstellung. Die Option **Volume** stellt alle gesicherten Daten auf einem angegebenen Volume wieder her. Mit dieser Option erreichen Sie höhere Übertragungsgeschwindigkeiten (bis zu 40 MBit/s), daher wird sie für das Wiederherstellen großer Daten oder ganzer Volumes empfohlen.

5. Wählen Sie auf der Seite **Volume und Datum auswählen** das Volume aus, das die wiederherzustellenden Dateien und Ordner enthält.

    Wählen Sie im Kalender einen Wiederherstellungspunkt aus. Wenn Datumsangaben in **Fettdruck** angezeigt werden, ist dies ein Hinweis darauf, dass mindestens ein Wiederherstellungszeitpunkt verfügbar ist. Falls mehrere Wiederherstellungspunkte für ein einzelnes Datum verfügbar sind, wählen Sie im Dropdownmenü **Uhrzeit** den jeweiligen Wiederherstellungspunkt aus.

    ![Screenshot der Seite „Volume und Datum auswählen“ des Assistenten zum Wiederherstellen von Daten (Wiederherstellen auf demselben Computer)](./media/backup-azure-restore-windows-server/samemachine_selectvolumedate_instantrestore.png)

6. Wählen Sie nach dem Auswählen des wiederherzustellenden Wiederherstellungspunkts **Einbinden** aus.

    Azure Backup stellt den lokalen Wiederherstellungspunkt bereit und verwendet ihn als Wiederherstellungsvolume.

7. Wählen Sie auf der Seite **Browse and Recover Files** (Dateien durchsuchen und wiederherstellen) die Option **Durchsuchen** aus, um Windows-Explorer zu öffnen und nach den gewünschten Dateien und Ordnern zu suchen.

    ![Screenshot der Seite „Dateien suchen und wiederherstellen“ des Assistenten zum Wiederherstellen von Daten (Wiederherstellen auf demselben Computer)](./media/backup-azure-restore-windows-server/samemachine_browserecover_instantrestore.png)

8. Kopieren Sie in Windows-Explorer die wiederherzustellenden Dateien und Ordner, und fügen Sie sie an einem lokalen Speicherort auf dem Server oder Computer ein. Sie können die Dateien direkt vom Wiederherstellungsvolume öffnen oder streamen und überprüfen, ob Sie die richtigen Versionen wiederherstellen.

    ![Screenshot von Windows-Explorer mit hervorgehobener Option „Kopieren“ (Wiederherstellen auf demselben Computer)](./media/backup-azure-restore-windows-server/samemachine_copy_instantrestore.png)

9. Zum Schluss wählen Sie auf der Seite **Dateien durchsuchen und wiederherstellen** die Option **Bereitstellung aufheben** aus. Wählen Sie anschließend **Ja** aus, um zu bestätigen, dass Sie die Einbindung des Volumes aufheben möchten.

    ![Screenshot der Seite „Dateien suchen und wiederherstellen“ des Assistenten zum Wiederherstellen von Daten (Wiederherstellen auf demselben Computer) – Bereitstellungsaufhebung für Wiederherstellungsvolume bestätigen](./media/backup-azure-restore-windows-server/samemachine_unmount_instantrestore.png)

    > [!Important]
    > Wenn Sie nicht **Bereitstellung aufheben** auswählen, bleibt die Einbindung des Wiederherstellungsvolumes ab dem Einbindungszeitpunkt sechs Stunden lang bestehen. Im Fall eines laufenden Dateikopiervorgangs wird die Einbindungszeit jedoch auf bis zu 7 Tage verlängert. Während der Bereitstellung des Volumes werden keine Sicherungsvorgänge durchgeführt. Alle Sicherungsvorgänge, deren Ausführung für den Zeitraum der Volumeeinbindung geplant ist, werden ausgeführt, nachdem die Einbindung des Wiederherstellungsvolumes aufgehoben wurde.
    >

## <a name="use-instant-restore-to-restore-data-to-an-alternate-machine"></a>Verwenden der sofortigen Wiederherstellung zum Wiederherstellen von Daten auf einem anderen Computer

Wenn der gesamte Server verloren geht, können Sie Daten aus Azure Backup dennoch auf einem anderen Computer wiederherstellen. Der Workflow wird in den folgenden Schritten beschrieben.

Bei diesen Schritte wird die folgende Terminologie verwendet:

* *Quellcomputer:* der ursprüngliche Computer, von dem die Sicherung erstellt wurde und der zurzeit nicht verfügbar ist.
* *Zielcomputer* – Der Computer, auf dem die Daten wiederhergestellt werden.
* *Beispieltresor:* der Recovery Services-Tresor, bei dem der Quellcomputer und der Zielcomputer registriert sind.

> [!NOTE]
> Sicherungen können nicht auf einem Zielcomputer wiederhergestellt werden, auf dem eine ältere Version des Betriebssystems ausgeführt wird. Beispielsweise kann eine Sicherung, die von einem Windows 7-Computer erstellt wurde, auf einem Windows 7-Computer (oder höher) wiederhergestellt werden. Es ist aber nicht möglich, eine Sicherung, die von einem Windows 10-Computer erstellt wurde, auf einem Windows 7-Computer wiederherzustellen.
>
>

1. Öffnen Sie das Snap-In **Microsoft Azure Backup** auf dem Zielcomputer.

2. Stellen Sie sicher, dass der Zielcomputer und der Quellcomputer beim selben Recovery Services-Tresor registriert sind.

3. Wählen Sie **Daten wiederherstellen** aus, um den **Assistenten zum Wiederherstellen von Daten** zu öffnen.

    ![Screenshot von Azure Backup mit hervorgehobener Option „Daten wiederherstellen“ (Wiederherstellen auf einem anderen Computer)](./media/backup-azure-restore-windows-server/recover.png)

4. Wählen Sie auf der Seite **Erste Schritte** die Option **Anderer Server** aus.

    ![Screenshot der Seite „Erste Schritte“ des Assistenten zum Wiederherstellen von Daten (Wiederherstellen auf einem anderen Computer)](./media/backup-azure-restore-windows-server/alternatemachine_gettingstarted_instantrestore.png)

5. Geben Sie die Datei mit den Tresoranmeldeinformationen an, die dem Beispieltresor entspricht, und wählen Sie **Weiter** aus.

    Wenn die Datei mit den Tresoranmeldeinformationen ungültig (oder abgelaufen) ist, [laden Sie eine neue Anmeldeinformationsdatei für den Tresor aus dem Beispieltresor im Azure-Portal herunter](backup-azure-file-folder-backup-faq.yml#where-can-i-download-the-vault-credentials-file-). Nachdem Sie gültige Tresoranmeldeinformationen angegeben haben, wird der Name des entsprechenden Sicherungstresors angezeigt.

6. Wählen Sie auf der Seite **Sicherungsserver auswählen** in der Liste mit den angezeigten Computern den Quellcomputer aus, und geben Sie die Passphrase an. Wählen Sie **Weiter** aus.

    ![Screenshot der Seite „Sicherungsserver auswählen“ des Assistenten zum Wiederherstellen von Daten (Wiederherstellen auf einem anderen Computer)](./media/backup-azure-restore-windows-server/alternatemachine_selectmachine_instantrestore.png)

7. Wählen Sie auf der Seite **Wiederherstellungsmodus auswählen** die Option **Individual Files and Folders (Einzelne Dateien und Ordner)**  > **Weiter** aus.

    ![Screenshot der Seite „Wiederherstellungsmodus auswählen“ des Assistenten zum Wiederherstellen von Daten (Wiederherstellen auf einem anderen Computer)](./media/backup-azure-restore-windows-server/alternatemachine_selectrecoverymode_instantrestore.png)

8. Wählen Sie auf der Seite **Volume und Datum auswählen** das Volume aus, das die wiederherzustellenden Dateien und Ordner enthält.

    Wählen Sie im Kalender einen Wiederherstellungspunkt aus. Wenn Datumsangaben in **Fettdruck** angezeigt werden, ist dies ein Hinweis darauf, dass mindestens ein Wiederherstellungszeitpunkt verfügbar ist. Falls mehrere Wiederherstellungspunkte für ein einzelnes Datum verfügbar sind, wählen Sie im Dropdownmenü **Uhrzeit** den jeweiligen Wiederherstellungspunkt aus.

    ![Screenshot der Seite „Volume und Datum auswählen“ des Assistenten zum Wiederherstellen von Daten (Wiederherstellen auf einem anderen Computer)](./media/backup-azure-restore-windows-server/alternatemachine_selectvolumedate_instantrestore.png)

9. Wählen Sie **Einbinden** aus, um den Wiederherstellungspunkt lokal als Wiederherstellungsvolume auf Ihrem Zielcomputer einzubinden.

10. Wählen Sie auf der Seite **Browse and Recover Files** (Dateien durchsuchen und wiederherstellen) die Option **Durchsuchen** aus, um Windows-Explorer zu öffnen und nach den gewünschten Dateien und Ordnern zu suchen.

    ![Screenshot der Seite „Dateien suchen und wiederherstellen“ des Assistenten zum Wiederherstellen von Daten (Wiederherstellen auf einem anderen Computer)](./media/backup-azure-restore-windows-server/alternatemachine_browserecover_instantrestore.png)

11. Kopieren Sie in Windows-Explorer die Dateien und Ordner vom Wiederherstellungsvolume, und fügen Sie sie am Speicherort auf dem Zielcomputer ein. Sie können die Dateien direkt vom Wiederherstellungsvolume öffnen oder streamen und überprüfen, ob die richtigen Versionen wiederhergestellt werden.

    ![Screenshot von Windows-Explorer mit hervorgehobener Option „Kopieren“ (Wiederherstellen auf einem anderen Computer)](./media/backup-azure-restore-windows-server/alternatemachine_copy_instantrestore.png)

12. Zum Schluss wählen Sie auf der Seite **Dateien durchsuchen und wiederherstellen** die Option **Bereitstellung aufheben** aus. Wählen Sie anschließend **Ja** aus, um zu bestätigen, dass Sie die Einbindung des Volumes aufheben möchten.

    ![Bereitstellung des Volumes aufheben (Wiederherstellen auf einem anderen Computer)](./media/backup-azure-restore-windows-server/alternatemachine_unmount_instantrestore.png)

    > [!Important]
    > Wenn Sie nicht **Bereitstellung aufheben** auswählen, bleibt die Einbindung des Wiederherstellungsvolumes ab dem Einbindungszeitpunkt sechs Stunden lang bestehen. Im Fall eines laufenden Dateikopiervorgangs wird die Einbindungszeit jedoch auf bis zu 24 Stunden verlängert. Während der Bereitstellung des Volumes werden keine Sicherungsvorgänge durchgeführt. Alle Sicherungsvorgänge, deren Ausführung für den Zeitraum der Volumeeinbindung geplant ist, werden ausgeführt, nachdem die Einbindung des Wiederherstellungsvolumes aufgehoben wurde.
    >

## <a name="next-steps"></a>Nächste Schritte

* Nachdem Sie nun Ihre Dateien und Ordner wiederhergestellt haben, können Sie [Ihre Sicherungen verwalten](backup-azure-manage-windows-server.md).

* Lesen Sie die Informationen unter [Häufig gestellte Fragen zum Sichern von Dateien und Ordnern](backup-azure-file-folder-backup-faq.yml).
