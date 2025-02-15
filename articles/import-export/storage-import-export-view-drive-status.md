---
title: Überprüfen des Status von Azure Import/Export-Aufträgen | Microsoft-Dokumentation
description: Informationen zum Anzeigen des Status der Azure Import/Export-Aufträge und verwendeten Laufwerke. Machen Sie sich mit den Faktoren vertraut, die sich auf die Verarbeitungsdauer eines Auftrags auswirken.
author: alkohli
services: storage
ms.service: storage
ms.topic: how-to
ms.date: 09/28/2021
ms.author: alkohli
ms.subservice: common
ms.custom: contperf-fy21q3
ms.openlocfilehash: bfa9e8a767167d7ae31dae3269be6cad2a1c1d07
ms.sourcegitcommit: 860f6821bff59caefc71b50810949ceed1431510
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2021
ms.locfileid: "129709381"
---
# <a name="view-the-status-of-azure-importexport-jobs"></a>Anzeigen des Status von Azure Import/Export-Aufträgen

Dieser Artikel enthält Informationen zum Anzeigen des Laufwerks-und Auftragsstatus für Azure Import/Export-Aufträge. Mit dem Azure Import/Export-Dienst werden große Datenmengen sicher in Azure-Blobs und Azure Files übertragen. Der Dienst wird auch verwendet, um Daten aus dem Azure-Blobspeicher zu exportieren.  

## <a name="view-job-and-drive-status"></a>Anzeigen von Auftrags- und Laufwerksstatus
Sie können den Status Ihrer Import- und Exportaufträge im Azure-Portal auf der Registerkarte **Import/Export** nachverfolgen.
1. Melden Sie sich bei https://portal.azure.com/ an.
2. Suchen Sie nach **Aufträge importieren/exportieren**.

    ![Suchen nach „Aufträge importieren/exportieren“](./media/storage-import-export-view-drive-status/open-import-export-tab.png)

 3. Eine Liste Ihrer Import-/Exportaufträge wird auf der Seite angezeigt.

    ![Anzeigen des Auftragsstatus](./media/storage-import-export-view-drive-status/job-state.png)

4. Klicken Sie auf einen Auftrag, um die Auftragsdetails anzuzeigen.

   ![Anzeigen von Auftragsstatusdetails](./media/storage-import-export-view-drive-status/job-detail.png)
  
## <a name="view-job-status"></a>Anzeigen des Auftragsstatus

Je nachdem, an welchem Punkt des Prozesses sich Ihre Festplatte befindet, wird einer der folgenden Auftragsstatus angezeigt.

| Auftragsstatus | BESCHREIBUNG |
|:--- |:--- |
| Wird erstellt | Nachdem ein Auftrag erstellt wurde, ist sein Status auf **Wird erstellt** festgelegt. Während sich der Auftrag im Status **Wird erstellt** befindet, geht der Import/Export-Dienst davon aus, dass die Laufwerke noch nicht an das Rechenzentrum versendet wurden. Ein Auftrag verbleibt bis zu zwei Wochen in diesem Status. Anschließend wird er automatisch vom Dienst gelöscht. |
| Versand | Nachdem Sie Ihr Paket gesendet haben, sollten Sie die Nachverfolgungsinformationen im Azure-Portal aktualisieren. Hierdurch wird der Auftrag in den Zustand **Wird versendet** versetzt. Der Auftrag verbleibt bis zu zwei Wochen im Status **Wird versendet**.<br>Wenn ein Auftrag den Status „Wird versendet“ erreicht, kann er nicht mehr abgebrochen werden.
| Empfangen | Nachdem das Rechenzentrum alle Laufwerke empfangen hat, wird der Status des Auftrags auf **Empfangen** gesetzt.</br>Der Auftragsstatus kann sich innerhalb von einem bis drei Tagen ändern, nachdem der Betreiber das Gerät bereitgestellt hat und die Auftragsverarbeitung im Rechenzentrum abgeschlossen wurde. |
| Übertragung wird ausgeführt | Nachdem die Verarbeitung mindestens eines Laufwerks begonnen hat, wird der Status des Auftrags auf **Übertragung wird ausgeführt** festgelegt. Weitere Informationen finden Sie unter [Laufwerkstatus](#view-drive-status). |
| Verpackung | Nachdem die Verarbeitung aller Laufwerke abgeschlossen ist, wird der Auftrag in den Status **Verpackung** gesetzt, bis die Laufwerke an den Kunden zurückgesendet werden. |
| Abgeschlossen | Nachdem alle Laufwerke an Sie zurückgesendet worden sind, und der Auftrag ohne Fehler abgeschlossen wurde, wird der Auftrag in den **Abgeschlossen**-Status gesetzt. Nach 90 Tagen im Status **Abgeschlossen** wird der Auftrag automatisch gelöscht. |
| Geschlossen | Wenn alle Laufwerke an Sie zurückgesendet wurden und Fehler während der Auftragsverarbeitung aufgetreten sind, wird der Status des Auftrags auf **Geschlossen** festgelegt. Nach 90 Tagen im Status **Geschlossen** wird der Auftrag automatisch gelöscht. |

## <a name="view-drive-status"></a>Anzeigen des Laufwerkstatus

In der folgenden Tabelle wird der Lebenszyklus eines einzelnen Laufwerks mit seinen Übergängen in einen Import- oder Exportauftrag beschrieben. Der aktuelle Status der einzelnen Laufwerke in einem Auftrag wird im Azure-Portal angezeigt.

Die folgende Tabelle beschreibt die einzelnen Status, die jedes Laufwerk in einem Auftrag durchlaufen kann.

| Laufwerkstatus | BESCHREIBUNG |
|:--- |:--- |
| Angegeben | Für einen Importauftrag, der im Azure-Portal erstellt wurde, befindet sich ein Laufwerk anfänglich im Status **Angegeben**. Für einen Exportauftrag befindet sich ein Laufwerk anfänglich im **Empfangen**-Status, da beim Erstellen des Auftrags kein Laufwerk angegeben wird. |
| Empfangen | Das Laufwerk wechselt in den Status **Empfangen**, wenn der Import/Export-Dienstanbieter die Laufwerke verarbeitet hat, die vom Transportunternehmen für einen Importauftrag empfangen wurden. Für einen Exportauftrag ist der anfängliche Laufwerkstatus der Status **Empfangen**. |
| NeverReceived | Das Laufwerk wechselt in den Status **NeverReceived**, wenn das Paket für einen Auftrag eintrifft, aber kein Laufwerk enthält. Ein Laufwerk wird auch in diesen Status versetzt, wenn das Paket noch nicht im Rechenzentrum eingetroffen ist und der Dienst die Versandinformationen vor mindestens zwei Wochen erhalten hat. |
| Übertragung wird ausgeführt | Ein Laufwerk wechselt in den Status **Übertragung wird ausgeführt**, wenn der Dienst beginnt, Daten aus dem Laufwerk in Azure Storage zu übertragen. |
| Abgeschlossen | Ein Laufwerk wechselt in den Status **Abgeschlossen**, wenn der Dienst alle Daten ohne Fehler erfolgreich übertragen hat.
| CompletedMoreInfo | Ein Laufwerk wechselt in den Status **CompletedMoreInfo**, wenn für den Dienst beim Kopieren von Daten vom Laufwerk oder auf das Laufwerk Probleme aufgetreten sind. Die Informationen können Fehler, Warnungen oder Informationsmeldungen zum Überschreiben von Blobs umfassen.
| ShippedBack | Das Laufwerk wechselt in den Status **ShippedBack**, wenn es vom Rechenzentrum an die Rücksendeadresse zurückgesendet wurde. |

Dieser Screenshot des Azure-Portals zeigt den Laufwerkstatus in einem Beispielauftrag:

![Anzeigen des Laufwerkstatus](./media/storage-import-export-view-drive-status/drive-state.png)

Die folgende Tabelle beschreibt die Laufwerkfehlerstatus und die Aktionen, die für jeden Status durchgeführt werden.

| Laufwerkstatus | Ereignis | Lösung / Nächster Schritt |
|:--- |:--- |:--- |
| Nie empfangen | Ein Laufwerk mit der Kennzeichnung **NeverReceived** (da es im Rahmen des Versands des Auftrags nicht eingegangen ist) geht in einer anderen Sendung ein. | Das Betriebsteam verschiebt das Laufwerk nach **Empfangen**. |
| – | Ein Laufwerk, das nicht Teil eines Auftrags ist, trifft als Teil eines anderen Auftrags im Rechenzentrum ein. | Das Laufwerk wird als zusätzliches Laufwerk gekennzeichnet. Es wird an Sie zurückgesendet, nachdem der Auftrag, der dem ursprünglichen Paket zugeordnet ist, abgeschlossen wurde. |

## <a name="time-to-process-job"></a>Verarbeitungsdauer des Auftrags
Der Zeitaufwand zum Verarbeiten eines Import/Export-Auftrags variiert basierend auf einer Reihe von Faktoren wie z.B.:

-  Versandzeit
-  Auftragsaufkommen im Rechenzentrum
-  Auftragstyp und Volumen der zu kopierenden Daten
-  Anzahl der Datenträger in einem Auftrag. 

Der Import/Export-Dienst verfügt nicht über eine SLA. Nach Erhalt der Datenträger versucht der Dienst aber, die Kopie in sieben bis zehn Tagen fertigzustellen. Zusätzlich zum Status, der im Azure-Portal angegeben wird, können Sie die REST-APIs zum Nachverfolgen des Auftragsstatus verwenden. Verwenden Sie den Parameter für die prozentuale Fertigstellung im API-Aufruf des Vorgangs zum [Auflisten von Aufträgen](/previous-versions/azure/dn529083(v=azure.100)), um den Status des Kopiervorgangs in Prozent anzuzeigen.


## <a name="next-steps"></a>Nächste Schritte

* [Übertragen von Daten mit dem Befehlszeilenprogramm AzCopy](../storage/common/storage-use-azcopy-v10.md)
* [Azure Import/Export-REST-API-Beispiel](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/)