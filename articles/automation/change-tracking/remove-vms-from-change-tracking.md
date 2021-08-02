---
title: Entfernen von VMs aus Änderungsnachverfolgung und Bestand in Azure Automation
description: In diesem Artikel erfahren Sie, wie Sie VMs aus Änderungsnachverfolgung und Bestand entfernen.
services: automation
ms.subservice: change-inventory-management
ms.topic: conceptual
ms.date: 05/26/2021
ms.openlocfilehash: 3a39294c2ecfe7b26cb3ef3d65c11cbcd665d220
ms.sourcegitcommit: 1b698fb8ceb46e75c2ef9ef8fece697852c0356c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2021
ms.locfileid: "110654123"
---
# <a name="remove-vms-from-change-tracking-and-inventory"></a>Entfernen virtueller Computer aus Änderungsnachverfolgung und Bestand

Wenn Sie mit der Nachverfolgung von Änderungen auf den VMs in Ihrer Umgebung fertig sind, können Sie deren Verwaltung mithilfe des Features [Änderungsnachverfolgung und Bestand](overview.md) beenden. Um die Bearbeitung zu beenden, bearbeiten Sie die gespeicherte Suchabfrage `MicrosoftDefaultComputerGroup` in Ihrem Log Analytics-Arbeitsbereich, der mit Ihrem Automation-Konto verknüpft ist.

## <a name="sign-into-the-azure-portal"></a>Anmelden beim Azure-Portal

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

## <a name="to-remove-your-vms"></a>So entfernen Sie Ihre VMs

1. Starten Sie **Cloud Shell** über den oberen Navigationsbereich im Azure-Portal. Wenn Sie mit Azure Cloud Shell nicht vertraut sind, finden Sie weitere Informationen unter [Übersicht über Azure Cloud Shell](../../cloud-shell/overview.md).

2. Ermitteln Sie mit dem folgenden Befehl die UUID eines Computers, den Sie aus der Verwaltung entfernen möchten.

    ```azurecli
    az vm show -g MyResourceGroup -n MyVm -d
    ```

3. Navigieren Sie im Azure-Portal zu **Log Analytics-Arbeitsbereiche**. Wählen Sie Ihren Arbeitsbereich in der Liste aus.

4. Wählen Sie in Ihrem Log Analytics-Arbeitsbereich im linken Menü **Computergruppen** aus.

5. Im rechten Bereich unter **Computergruppen** wird standardmäßig die Registerkarte **Gespeicherte Gruppen** angezeigt.

6. Klicken Sie in der Tabelle auf das Symbol **Abfrage ausführen** rechts neben dem Element **MicrosoftDefaultComputerGroup** mit dem **Legacykategorie**-Wert **ChangeTracking**.

7. Überprüfen Sie die Abfrage im Abfrage-Editor, und suchen Sie die UUID für den virtuellen Computer. Entfernen Sie die UUID für den virtuellen Computer, und wiederholen Sie die Schritte für alle anderen VMs, die Sie entfernen möchten.

   > [!NOTE]
   > Erstellen Sie als zusätzlichen Schutz unbedingt eine Kopie der Abfrage, bevor Sie Bearbeitungen vornehmen. Auf diese Weise können Sie sie wiederherstellen, wenn ein Problem auftritt.

   Wenn Sie mit der ursprünglichen Abfrage beginnen und VMs zum Zweck einer Bereinigungs- oder Wartungsaktivität erneut hinzufügen möchten, kopieren Sie die folgende Abfrage:

   ```kusto
   Heartbeat
   | where Computer in~ ("") or VMUUID in~ ("")
   | distinct Computer
   ```

8. Speichern Sie die gespeicherte Suche, wenn Sie die Bearbeitung abgeschlossen haben, indem Sie in der oberen Leiste **Speichern > Als Funktion speichern** auswählen. Geben Sie bei Aufforderung Folgendes an:

    * **Name**: ChangeTracking__MicrosoftDefaultComputerGroup
    * **Als Computergruppe speichern** ist ausgewählt
    * **Legacykategorie:** ChangeTracking

>[!NOTE]
>Computer werden weiterhin angezeigt, nachdem Sie die Registrierung aufgehoben haben, da wir alle in den letzten 24 Stunden bewerteten Computer melden. Nachdem Sie den Computer entfernt haben, müssen Sie 24 Stunden warten, bis er nicht mehr aufgelistet wird.

## <a name="next-steps"></a>Nächste Schritte

Informationen zum erneuten Aktivieren dieses Features finden Sie unter [Aktivieren des Features für Änderungsnachverfolgung und Bestand über ein Automation-Konto](enable-from-automation-account.md), [Aktivieren des Features für Änderungsnachverfolgung und Bestand durch das Durchsuchen des Azure-Portals](enable-from-portal.md), [Aktivieren des Features für Änderungsnachverfolgung und Bestand über ein Runbook](enable-from-runbook.md) oder [Aktivieren des Features für Änderungsnachverfolgung und Bestand über eine Azure-VM](enable-from-vm.md).