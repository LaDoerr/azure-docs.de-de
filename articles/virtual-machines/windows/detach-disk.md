---
title: Trennen eines Datenträgers mit Daten von einem virtuellen Windows-Computer – Azure
description: Trennen eines Datenträgers unter Verwendung des Resource Manager-Bereitstellungsmodells von einem virtuellen Computer in Azure
author: cynthn
ms.service: virtual-machines
ms.subservice: disks
ms.collection: windows
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 03/03/2021
ms.author: cynthn
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 49b58a46b4abc65089970982c26424a2f615bad4
ms.sourcegitcommit: 58d82486531472268c5ff70b1e012fc008226753
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/23/2021
ms.locfileid: "122693416"
---
# <a name="how-to-detach-a-data-disk-from-a-windows-virtual-machine"></a>Trennen eines Datenträgers von einem virtuellen Windows-Computer

**Gilt für**: :heavy_check_mark: Windows-VMs :heavy_check_mark: Flexible Skalierungsgruppen 

Wenn Sie einen Datenträger, der an einen virtuellen Computer angefügt ist, nicht mehr benötigen, können Sie ihn leicht trennen. Dadurch wird der Datenträger von dem virtuellen Computer entfernt, aber nicht aus dem Speicher.

> [!WARNING]
> Beim Trennen eines Datenträgers wird dieser nicht automatisch gelöscht. Wenn Sie Storage Premium abonniert haben, fallen auch weiterhin Speichergebühren für den Datenträger an. Weitere Informationen finden Sie unter [Preisgestaltung und Abrechnung bei der Verwendung von Storage Premium](../disks-types.md#billing).

Wenn Sie die vorhandenen Daten erneut auf dem Datenträger verwenden möchten, können Sie ihn erneut an denselben virtuellen Computer oder an einen anderen anfügen.

 

## <a name="detach-a-data-disk-using-powershell"></a>Trennen eines Datenträgers mit PowerShell

Sie können mit PowerShell einen Datenträger *im laufenden Betrieb* entfernen. Stellen Sie jedoch sicher, dass der Datenträger nicht aktiv verwendet wird, bevor Sie ihn vom virtuellen Computer trennen.

In diesem Beispiel wird der Datenträger namens **myDisk** vom virtuellen Computer **myVM** in der Ressourcengruppe **myResourceGroup** entfernt. Sie entfernen zunächst den Datenträger mit dem Cmdlet [Remove-AzVMDataDisk](/powershell/module/az.compute/remove-azvmdatadisk). Anschließend wird der Status des virtuellen Computers über das Cmdlet [Update-AzVM](/powershell/module/az.compute/update-azvm) aktualisiert, um den Vorgang zum Entfernen des Datenträgers abzuschließen.

```azurepowershell-interactive
$VirtualMachine = Get-AzVM `
   -ResourceGroupName "myResourceGroup" `
   -Name "myVM"
Remove-AzVMDataDisk `
   -VM $VirtualMachine `
   -Name "myDisk"
Update-AzVM `
   -ResourceGroupName "myResourceGroup" `
   -VM $VirtualMachine
```

Der Datenträger verbleibt im Speicher, ist jedoch nicht mehr an einen virtuellen Computer angefügt.

## <a name="detach-a-data-disk-using-the-portal"></a>Trennen eines Datenträgers im Portal

Sie können einen Datenträger für Daten *im laufenden Betrieb* entfernen. Stellen Sie jedoch sicher, dass der Datenträger nicht aktiv verwendet wird, bevor Sie ihn vom virtuellen Computer trennen.

1. Wählen Sie im Menü auf der linken Seite die Option **Virtuelle Computer** aus.
1. Wählen Sie den virtuellen Computer mit dem zu trennenden Datenträger aus.
1. Wählen Sie unter **Einstellungen** die Option **Datenträger** aus.
1. Wählen Sie im Bereich **Datenträger** ganz rechts neben dem Datenträger, den Sie trennen möchten, die Schaltfläche **X** zum Trennen aus.
1. Wählen Sie oben auf der Seite **Speichern** aus, um Ihre Änderungen zu speichern.

Der Datenträger verbleibt im Speicher, ist jedoch nicht mehr an einen virtuellen Computer angefügt. Der Datenträger wird nicht gelöscht.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie den Datenträger wiederverwenden möchten, können Sie ihn einfach [an einen anderen virtuellen Computer anfügen](attach-managed-disk-portal.md).

Wenn Sie den Datenträger löschen möchten, sodass keine Speicherkosten mehr anfallen, finden Sie entsprechende Informationen unter [Suchen und Löschen von nicht angefügten verwalteten und nicht verwalteten Azure-Datenträgern – Azure-Portal](../disks-find-unattached-portal.md).
