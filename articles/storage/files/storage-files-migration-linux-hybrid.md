---
title: Linux-Migration zur Azure-Dateisynchronisierung
description: Hier erfahren Sie, wie Sie Dateien von einem Linux-Serverstandort mit der Azure-Dateisynchronisierung und Azure-Dateifreigaben zu einer Hybrid-Cloud-Bereitstellung migrieren.
author: fauhse
ms.service: storage
ms.topic: how-to
ms.date: 03/19/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: 18438400054a8f7aa4d718efdff4ef2e116b3bf3
ms.sourcegitcommit: 0af634af87404d6970d82fcf1e75598c8da7a044
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/15/2021
ms.locfileid: "114462422"
---
# <a name="migrate-from-linux-to-a-hybrid-cloud-deployment-with-azure-file-sync"></a>Migration von Linux zu einer Hybrid-Cloud-Bereitstellung mit der Azure-Dateisynchronisierung

Dieser Artikel zur Migration ist einer von mehreren Artikeln im Zusammenhang mit den Schlüsselwörtern „NFS“ und „Azure-Dateisynchronisierung“. Überprüfen Sie, ob dieser Artikel für Ihr Szenario zutrifft:

> [!div class="checklist"]
> * Datenquelle: Network Attached Storage (NAS)
> * Migrationsroute: Linux-Server mit SAMBA &rArr; Windows Server 2012R2 oder höher &rArr;-Synchronisierung mit Azure-Dateifreigabe(n)
> * Lokales Zwischenspeichern von Dateien: Ja, das endgültige Ziel ist eine Bereitstellung einer Azure-Dateisynchronisierung.

Wenn Ihr Szenario anders ist, sehen Sie sich die [Tabelle mit Migrationsleitfäden](storage-files-migration-overview.md#migration-guides) an.

Die Azure-Dateisynchronisierung arbeitet auf Windows Server-Instanzen mit direkt angeschlossenem Speicher. Die Synchronisierung mit und von Linux-Clients, SMB-Remotefreigaben (Server Message Block) oder NFS-Freigaben (Network File System) wird nicht unterstützt.

Wenn Sie Ihre Dateidienste in eine Hybridbereitstellung umwandeln, wird daher eine Migration zu Windows Server erforderlich. Dieser Artikel führt Sie durch die Planung und Durchführung einer solchen Migration.

## <a name="applies-to"></a>Gilt für:
| Dateifreigabetyp | SMB | NFS |
|-|:-:|:-:|
| Standard-Dateifreigaben (GPv2), LRS/ZRS | ![Ja](../media/icons/yes-icon.png) | ![Nein](../media/icons/no-icon.png) |
| Standard-Dateifreigaben (GPv2), GRS/GZRS | ![Ja](../media/icons/yes-icon.png) | ![Nein](../media/icons/no-icon.png) |
| Premium-Dateifreigaben (FileStorage), LRS/ZRS | ![Ja](../media/icons/yes-icon.png) | ![Nein](../media/icons/no-icon.png) |

## <a name="migration-goals"></a>Migrationsziele

Das Ziel besteht darin, Freigaben auf Ihrem Linux-Samba-Server auf eine Windows Server-Instanz zu verschieben. Anschließend soll die Azure-Dateisynchronisierung für eine Hybrid Cloud-Bereitstellung verwendet werden. Diese Migration muss so erfolgen, dass die Integrität der Produktionsdaten und die Verfügbarkeit während der Migration gewährleistet wird. Letzteres erfordert minimale Ausfallzeiten, damit sie in normalen Wartungsfenstern stattfinden kann oder diese nur geringfügig überschreitet.

## <a name="migration-overview"></a>Übersicht zur Migration

Wie im [Artikel mit der Migrationsübersicht](storage-files-migration-overview.md) zu Azure Files beschrieben wird, ist es wichtig, das richtige Kopiertool und den richtigen Ansatz zu verwenden. Ihr Linux-Samba-Server macht SMB-Freigaben direkt in Ihrem lokalen Netzwerk verfügbar. Das in Windows Server integrierte Tool Robocopy bietet die beste Methode, Ihre Dateien in diesem Migrationsszenario zu verschieben.

Wenn Sie Samba nicht auf Ihrem Linux-Server ausführen und stattdessen Ordner zu einer Hybridbereitstellung unter Windows Server migrieren möchten, können Sie anstelle von Robocopy die Linux-Kopiertools verwenden. Beachten Sie die Genauigkeitsfunktionen Ihres Kopiertools. Lesen Sie den Abschnitt über [Grundlagen zur Migration](storage-files-migration-overview.md#migration-basics) im Artikel zur Übersicht über die Migration, um zu erfahren, worauf Sie bei einem Kopiertool achten müssen.

## <a name="phase-1-identify-how-many-azure-file-shares-you-need"></a>Phase 1: Ermitteln der Anzahl der benötigten Azure-Dateifreigaben

[!INCLUDE [storage-files-migration-namespace-mapping](../../../includes/storage-files-migration-namespace-mapping.md)]

## <a name="phase-2-provision-a-suitable-windows-server-instance-on-premises"></a>Phase 2: Lokales Bereitstellen einer geeigneten Windows Server-Instanz

* Erstellen Sie eine Instanz von Windows Server 2019 als virtuellen Computer oder physischen Server. Es ist mindestens Windows Server 2012 R2 erforderlich. Ein Windows Server-Failovercluster wird ebenfalls unterstützt.
* Stellen Sie einen direkt angeschlossenen Speicher bereit, oder fügen Sie ihn hinzu. Network Attached Storage (NAS) wird nicht unterstützt.

  Die bereitgestellte Speichermenge kann kleiner sein als jene, die Sie zurzeit auf Ihrem Linux-Samba-Server verwenden, wenn Sie das [Cloudtiering](../file-sync/file-sync-cloud-tiering-overview.md)-Feature der Azure-Dateisynchronisierung verwenden. 

Die von Ihnen bereitgestellte Speichermenge kann kleiner sein als diejenige, die Sie zurzeit auf Ihrem Linux-Samba-Server verwenden. Diese Konfigurationsoption erfordert, dass Sie auch das Feature [Cloudtiering](../file-sync/file-sync-cloud-tiering-overview.md) der Azure-Dateisynchronisierung nutzen. Wenn Sie jedoch Ihre Dateien in einer späteren Phase aus dem größeren Linux-Samba-Serverbereich in das kleinere Windows Server-Volume kopieren, müssen Sie in Batches arbeiten:

  1. Verschieben Sie eine Dateimenge, die auf den Datenträger passt.
  2. Lassen Sie die Dateisynchronisierung und das Cloudtiering interagieren.
  3. Wenn mehr freier Speicherplatz auf dem Volume geschaffen wurde, fahren Sie mit dem nächsten Dateibatch fort. Überprüfen Sie alternativ den RoboCopy-Befehl im [Abschnitt „RoboCopy“](#phase-7-robocopy) weiter unten zur Verwendung des neuen Switches `/LFSM`. Die Verwendung von `/LFSM` kann Ihre RoboCopy-Aufträge erheblich vereinfachen, ist aber mit einigen anderen RoboCopy-Switches nicht kompatibel, von denen Sie möglicherweise abhängig sind.
    
  Sie können diesen Batchverarbeitungsansatz vermeiden, indem Sie auf der Windows Server-Instanz den entsprechenden Speicherplatz bereitstellen, den Ihre Dateien auf dem Linux-Samba-Server belegen. Aktivieren Sie die Deduplizierung unter Windows. Wenn Sie diese große Menge an Speicher nicht dauerhaft auf Ihre Windows Server-Instanz übertragen möchten, können Sie die Volumegröße nach der Migration und vor der Anpassung der Cloudtieringrichtlinien verringern. Dadurch wird ein kleinerer lokaler Cache Ihrer Azure-Dateifreigaben erstellt.

Die Ressourcenkonfiguration (Compute und RAM) der von Ihnen bereitgestellten Windows Server-Instanz hängt größtenteils von der Anzahl der Elemente (Dateien und Ordner) ab, die synchronisiert werden sollen. Wenn Sie Bedenken haben, empfiehlt es sich, eine leistungsstärkere Konfiguration zu verwenden.

[Erfahren Sie, wie Sie die Größe einer Windows Server-Instanz basierend auf der Anzahl der zu synchronisierenden Elemente (Dateien und Ordner) anpassen.](../file-sync/file-sync-planning.md#recommended-system-resources)

> [!NOTE]
> Der zuvor verknüpfte Artikel enthält eine Tabelle mit einem Bereich für den Serverarbeitsspeicher (RAM). Sie können sich an der geringeren Zahl für Ihren Server orientieren, müssen jedoch davon ausgehen, dass die anfängliche Synchronisierung wesentlich mehr Zeit in Anspruch nehmen kann.

## <a name="phase-3-deploy-the-azure-file-sync-cloud-resource"></a>Phase 3: Bereitstellen der Cloudressource für die Azure-Dateisynchronisierung

[!INCLUDE [storage-files-migration-deploy-afs-sss](../../../includes/storage-files-migration-deploy-azure-file-sync-storage-sync-service.md)]

## <a name="phase-4-deploy-azure-storage-resources"></a>Phase 4: Bereitstellen von Azure Storage-Ressourcen

Sehen Sie sich in dieser Phase die Zuordnungstabelle aus Phase 1 an, und verwenden Sie diese, um die richtige Anzahl von Azure-Speicherkonten und Dateifreigaben darin bereitzustellen.

[!INCLUDE [storage-files-migration-provision-azfs](../../../includes/storage-files-migration-provision-azure-file-share.md)]

## <a name="phase-5-deploy-the-azure-file-sync-agent"></a>Phase 5: Bereitstellen des Azure-Dateisynchronisierungs-Agents

[!INCLUDE [storage-files-migration-deploy-afs-agent](../../../includes/storage-files-migration-deploy-azure-file-sync-agent.md)]

## <a name="phase-6-configure-azure-file-sync-on-the-windows-server-deployment"></a>Phase 6: Konfigurieren der Azure-Dateisynchronisierung in der Windows Server-Bereitstellung

Die registrierte lokale Windows Server-Instanz muss für diesen Prozess vorbereitet und mit dem Internet verbunden sein.

[!INCLUDE [storage-files-migration-configure-sync](../../../includes/storage-files-migration-configure-sync.md)]

> [!IMPORTANT]
> Cloudtiering ist ein Feature der Azure-Dateisynchronisierung, das es dem lokalen Server ermöglicht, weniger Speicherkapazität als in der Cloud zu haben, aber trotzdem über den vollständigen Namespace zu verfügen. Lokal relevante Daten werden zudem für eine schnelle Zugriffsleistung lokal zwischengespeichert. Das Cloudtiering ist ein optionales Feature, das pro Serverendpunkt der Azure-Dateisynchronisierung agiert.

> [!WARNING]
> Wenn Sie auf Ihren Windows Server-Volumes weniger Speicher bereitgestellt haben als für die Daten auf dem Linux-Samba-Server verwendet werden, ist das Cloudtiering obligatorisch. Wenn Sie das Cloudtiering nicht aktivieren, gibt der Server nicht genügend Speicherplatz zum Speichern aller Dateien frei. Legen Sie Ihre Tieringrichtlinie vorübergehend für die Migration auf 99 % freien Speicherplatz für ein Volume fest. Achten Sie darauf, nach Abschluss der Migration zu Ihren Cloudtieringeinstellungen zurückkehren und diese auf eine langfristig sinnvollere Stufe festzulegen.

Wiederholen Sie die Schritte zum Erstellen einer Synchronisierungsgruppe und zum Hinzufügen des jeweiligen Serverordners als Serverendpunkt für alle Azure-Dateifreigaben und Serverspeicherorte, die für die Synchronisierung konfiguriert werden müssen.

Nach der Erstellung aller Serverendpunkte funktioniert die Synchronisierung. Sie können eine Testdatei erstellen und beobachten, dass sie an Ihrem Serverstandort mit der verbundenen Azure-Dateifreigabe synchronisiert wird (wie vom Cloudendpunkt in der Synchronisierungsgruppe beschrieben).

Beide Speicherorte – die Serverordner und die Azure-Dateifreigaben – sind andernfalls leer und erwarten Daten. Im nächsten Schritt beginnen Sie mit dem Kopieren von Dateien auf die Windows Server-Instanz, damit die Azure-Dateisynchronisierung diese in die Cloud verschieben kann. Falls Sie das Cloudtiering aktiviert haben, beginnt der Server dann mit der Aufteilung der Dateien, wenn die Kapazität auf den lokalen Volumes ausgeschöpft ist.

## <a name="phase-7-robocopy"></a>Phase 7: Robocopy

Der grundlegende Migrationsansatz ist die Verwendung von Robocopy zum Kopieren von Dateien sowie der Azure-Dateisynchronisierung zum Synchronisieren.

Erstellen Sie die erste lokale Kopie in Ihrem Windows Server-Zielordner:

1. Ermitteln Sie den ersten Speicherort auf Ihrem Linux-Samba-Server.
1. Identifizieren Sie den entsprechenden Ordner auf der Windows Server-Instanz, auf der die Azure-Dateisynchronisierung bereits konfiguriert wurde.
1. Starten Sie den Kopiervorgang mit Robocopy.

Mit dem folgenden Robocopy-Befehl werden Dateien vom Speicher Ihres Linux-Samba-Servers in den Windows Server-Zielordner kopiert. Windows-Server synchronisiert diesen Ordner mit den Azure-Dateifreigaben. 

Wenn Sie auf Ihrer Windows Server-Instanz weniger Speicher bereitgestellt haben als Ihre Daten auf dem Linux-Samba-Server verwenden, haben Sie Cloudtiering konfiguriert. Wenn das lokale Windows Server-Volume voll ist, beginnt das [Cloudtiering](../file-sync/file-sync-cloud-tiering-overview.md) für die Dateien, die bereits erfolgreich synchronisiert wurden. Durch das Cloudtiering wird ausreichend Speicherplatz generiert, um mit dem Kopiervorgang vom Linux-Samba-Server fortzufahren. Einmal pro Stunde wird überprüft, was bereits im Cloudtiering synchronisiert wurde, und Speicherplatz freigegeben, um auf dem Volume einen freien Speicherplatz von 99 % zu erreichen.

Robocopy verschiebt die Dateien möglicherweise zu schnell für den Synchronisierungsvorgang mit der Cloud und führt dann ein lokales Tiering durch. Dadurch kann der Speicherplatz auf dem lokalen Datenträger knapp werden. Robocopy führt dann zu einem Fehler. Es wird empfohlen, die Freigaben nacheinander abzuarbeiten, um dieses Problem zu verhindern. Beispielsweise sollten Sie nicht für alle Freigaben gleichzeitig Robocopy-Aufträge starten. Sie können auch Freigaben verschieben, für die der aktuell freie Speicherplatz auf der Windows Server-Instanz ausreicht. Falls Ihr Robocopy-Auftrag fehlschlägt, können Sie den Befehl jederzeit erneut ausführen, wenn Sie die folgende Option zum Spiegeln/Bereinigen verwenden:

[!INCLUDE [storage-files-migration-robocopy](../../../includes/storage-files-migration-robocopy.md)]

## <a name="phase-8-user-cut-over"></a>Phase 8: Benutzerübernahme

Wenn Sie den Robocopy-Befehl zum ersten Mal ausführen, greifen Ihre Benutzer und Anwendungen weiterhin auf Dateien auf dem Linux-Samba-Server zu und ändern sie möglicherweise. Es kann vorkommen, dass Robocopy ein Verzeichnis verarbeitet, mit dem nächsten fortfährt und dann ein Benutzer am Quellspeicherort (Linux) eine Datei hinzufügt, ändert oder löscht. Diese wird dann während der aktuellen Robocopy-Ausführung nicht verarbeitet. Dies ist das erwartete Verhalten.

Der erste Schritt besteht darin, den Großteil der Daten auf Ihre Windows Server-Instanz und dann über die Azure-Dateisynchronisierung in die Cloud zu verschieben. Der erste Kopiervorgang kann einige Zeit in Anspruch nehmen, die von folgenden Faktoren abhängig ist:

* Downloadbandbreite
* Uploadbandbreite
* Geschwindigkeit des lokalen Netzwerks und Anzahl der optimalen Übereinstimmungen von Robocopy-Threads
* Anzahl der Elemente (Dateien und Ordner), die von Robocopy und der Azure-Dateisynchronisierung verarbeitet werden müssen

Nachdem die erste Ausführung beendet wurde, führen Sie den Befehl erneut aus.

Beim zweiten Mal wird der Vorgang schneller abgeschlossen, da nur Änderungen, die seit der letzten Ausführung aufgetreten sind, übertragen werden müssen. Während dieser zweiten Ausführung können sich auch neue Änderungen ansammeln.

Wiederholen Sie diesen Vorgang, bis Sie der Auffassung sind, dass die bis zum Abschluss eines Robocopy-Vorgangs für einen bestimmten Speicherort benötigte Zeit ein akzeptables Ausfallzeitfenster darstellt.

Wenn Sie die Ausfallzeit als akzeptabel betrachten und darauf vorbereitet sind, den Linux-Speicherort offline zu schalten, können Sie die ACLs für den Freigabestamm ändern, sodass Benutzer nicht mehr auf den Speicherort zugreifen können. Sie können auch einen anderen geeigneten Schritt ausführen, der das Ändern von Inhalten in diesem Ordner auf Ihrem Linux-Server verhindert.

Führen Sie einen letzten Robocopy-Durchgang aus. Dadurch werden alle Änderungen übernommen, die zuvor möglicherweise ausgelassen wurden. Wie lange dieser letzte Schritt dauert, hängt von der Geschwindigkeit des Robocopy-Scans ab. Sie können die Zeit (gleich der Ausfallzeit) schätzen, indem Sie messen, wie lange die vorherige Ausführung gedauert hat.

Erstellen Sie eine Freigabe für den Windows Server-Ordner, und passen Sie Ihre DFS-N-Bereitstellung ggf. so an, dass sie auf diese zeigt. Stellen Sie sicher, dass Sie die gleichen Berechtigungen auf Freigabeebene wie auf den SMB-Freigaben Ihres Linux-Samba-Servers festlegen. Wenn Sie lokale Benutzer auf Ihrem Linux-Samba-Server verwendet haben, müssen Sie diese Benutzer als lokale Benutzer von Windows Server neu erstellen. Sie müssen auch die vorhandenen SIDs, die Robocopy auf Ihre Windows Server-Instanz verschoben hat, den SIDs Ihrer neuen lokalen Benutzer von Windows Server zuordnen. Wenn Sie Active Directory-Konten und ACLs verwendet haben, werden diese von Robocopy unverändert verschoben, und es ist keine weitere Aktion erforderlich.

Sie haben die Migration einer Freigabe oder Gruppe von Freigaben zu einem gemeinsamen Stamm oder Volume (je nach Zuordnung aus Phase 1) abgeschlossen.

Sie können einige dieser Kopiervorgänge parallel ausführen. Es wird empfohlen, jeweils eine Azure-Dateifreigabe auf einmal zu verarbeiten.

> [!WARNING]
> Nachdem Sie alle Daten von Ihrem Linux-Samba-Server auf die Windows Server-Instanz verschoben haben und die Migration abgeschlossen wurde, wechseln Sie im Azure-Portal zu *allen* Synchronisierungsgruppen zurück. Legen Sie den Prozentsatz des freien Speicherplatzes für das Cloudtiering auf einen Wert fest, der für die Cachenutzung besser geeignet ist, z. B. 20 %. 

Die Richtlinie für den freien Speicherplatz für das Cloudtiering wirkt sich auf eine Volumeebene aus, von der aus potenziell mehrere Serverendpunkte synchronisiert werden. Wenn Sie vergessen, den freien Speicherplatz auf einem Serverendpunkt anzupassen, wird für die Synchronisierung weiterhin die restriktivste Regel angewandt, und es wird versucht, 99 % freien Speicherplatz beizubehalten. Der lokale Cache funktioniert in diesem Fall nicht wie erwartet. Dies kann sinnvoll sein, wenn Sie den Namespace für ein Volume erhalten möchten, das ausschließlich selten genutzte Archivdaten enthält, und Sie den restlichen Speicherplatz für ein anderes Szenario reservieren.

## <a name="troubleshoot"></a>Problembehandlung

Das häufigste Problem besteht darin, dass der Robocopy-Befehl mit dem Fehler **Volume voll** auf Windows Server-Seite beendet wird. Das Cloudtiering erfolgt einmal stündlich, um Inhalte vom lokalen Windows Server-Datenträger abzurufen, die bereits synchronisiert wurden. Das Ziel besteht darin, 99 % freien Speicherplatz auf dem Volume zu erreichen.

Warten Sie, bis durch den Synchronisierungsvorgang und das Cloudtiering Speicherplatz freigegeben wurde. Sie können dies im Datei-Explorer unter Windows Server beobachten.

Wenn auf Ihrer Windows Server-Instanz ausreichende Kapazität verfügbar ist, wird das Problem durch erneutes Ausführen des Befehls behoben. Falls Sie auf diese Situation stoßen, treten keine Schäden auf, und Sie können einfach fortfahren. Der zusätzliche Aufwand durch das erneute Ausführen des Befehls ist die einzige Folge.

Unter dem Link im folgenden Abschnitt finden Sie Informationen zur Behandlung von Problemen mit der Azure-Dateisynchronisierung.

## <a name="next-steps"></a>Nächste Schritte

Machen Sie sich weiter mit Azure-Dateifreigaben und der Azure-Dateisynchronisierung vertraut. In den folgenden Artikeln werden erweiterte Optionen, bewährte Methoden und Ansätze zum Troubleshooting erläutert. Diese Artikel sind mit der entsprechenden [Dokumentation zur Azure-Dateifreigabe](storage-files-introduction.md) verlinkt.

* [Azure-Dateisynchronisierung – Übersicht](../file-sync/file-sync-planning.md)
* [Bereitstellen der Azure-Dateisynchronisierung](../file-sync/file-sync-deployment-guide.md)
* [Azure-Dateisynchronisierung – Troubleshooting](../file-sync/file-sync-troubleshoot.md)