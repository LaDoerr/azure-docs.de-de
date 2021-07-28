---
title: 'Schnellstart: Sichern eines virtuellen Computers mit PowerShell'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie Ihre virtuellen Azure-Computer mit dem Azure PowerShell-Modul sichern.
ms.devlang: azurecli
ms.topic: quickstart
ms.date: 04/16/2019
ms.custom: mvc, devx-track-azurepowershell
ms.openlocfilehash: c9ff33fa1600043aaae8b8ffc6c1f9dc5252bd65
ms.sourcegitcommit: ef950cf37f65ea7a0f583e246cfbf13f1913eb12
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2021
ms.locfileid: "111420804"
---
# <a name="back-up-a-virtual-machine-in-azure-with-powershell"></a>Sichern eines virtuellen Computers in Azure mit PowerShell

Mit dem [Azure PowerShell Az](/powershell/azure/new-azureps-module-az)-Modul können Sie Azure-Ressourcen über die Befehlszeile oder in Skripts erstellen und verwalten.

Mit [Azure Backup](backup-overview.md) können Sie lokale Computer und Apps sowie virtuelle Azure-Computer sichern. In diesem Artikel wird beschrieben, wie Sie mit dem Az-Modul einen virtuellen Azure-Computer sichern. Alternativ können Sie einen virtuellen Computer mithilfe der [Azure-Befehlszeilenschnittstelle](quick-backup-vm-cli.md) oder im [Azure-Portal](quick-backup-vm-portal.md) sichern.

In dieser Schnellstartanleitung wird die Sicherung für einen vorhandenen virtuellen Azure-Computer aktiviert. Wenn Sie eine VM erstellen müssen, können Sie die [Erstellung mit Azure PowerShell durchführen](/previous-versions/azure/virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm?toc=%2fpowershell%2fmodule%2ftoc.json).

Für diese Schnellstartanleitung ist Version 1.0.0 oder höher des Azure PowerShell Az-Moduls erforderlich. Führen Sie `Get-Module -ListAvailable Az` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Install and configure Azure PowerShell](/powershell/azure/install-az-ps) (Installieren des Azure PowerShell-Moduls) Informationen dazu.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="sign-in-and-register"></a>Anmeldung und Registrierung

1. Melden Sie sich mit dem Befehl `Connect-AzAccount` bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm.

    ```powershell
    Connect-AzAccount
    ```

2. Bei der ersten Verwendung von Azure Backup müssen Sie den Azure Recovery Service-Anbieter in Ihrem Abonnement mit [Register-AzResourceProvider](/powershell/module/az.Resources/Register-azResourceProvider) wie folgt registrieren:

    ```powershell
    Register-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

## <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors

Ein [Recovery Services-Tresor](backup-azure-recovery-services-vault-overview.md) ist ein logischer Container, in dem Sicherungsdaten für geschützte Ressourcen wie Azure-VMs gespeichert werden. Beim Ausführen eines Sicherungsauftrags wird im Recovery Services-Tresor ein Wiederherstellungspunkt erstellt. Sie können einen dieser Wiederherstellungspunkte dann verwenden, um Daten für einen bestimmten Zeitpunkt wiederherzustellen.

Beachten Sie beim Erstellen des Tresors die folgenden Punkte:

- Geben Sie für die Ressourcengruppe und den Speicherort die Ressourcengruppe und den Speicherort des zu sichernden virtuellen Computers an.
- Wenn Sie dieses [Beispielskript](/previous-versions/azure/virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm?toc=%2fpowershell%2fmodule%2ftoc.json) zum Erstellen des virtuellen Computers verwendet haben, heißt die Ressourcengruppe **myResourceGroup**, der virtuelle Computer **_myVM_*, und die Ressourcen befinden sich in der Region **WestEurope**.
- Azure Backup übernimmt automatisch die Speicherung der gesicherten Daten. Der Tresor verwendet standardmäßig den [georedundanten Speicher (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage). Durch Georedundanz wird sichergestellt, dass die gesicherten Daten in einer sekundären Azure-Region repliziert werden, die Hunderte von Kilometern von der primären Region entfernt ist.

Erstellen Sie jetzt einen Tresor:

1. Verwenden Sie das Cmdlet [New-AzRecoveryServicesVault](/powershell/module/az.recoveryservices/new-azrecoveryservicesvault), um den Tresor zu erstellen:

    ```powershell
    New-AzRecoveryServicesVault `
        -ResourceGroupName "myResourceGroup" `
        -Name "myRecoveryServicesVault" `
    -Location "WestEurope"
    ```

2. Legen Sie den Tresorkontext mit [Set-AzRecoveryServicesVaultContext](/powershell/module/az.RecoveryServices/Set-azRecoveryServicesVaultContext) wie folgt fest:

    ```powershell
    Get-AzRecoveryServicesVault `
        -Name "myRecoveryServicesVault" | Set-AzRecoveryServicesVaultContext
    ```

3. Ändern Sie die Speicherredundanzkonfiguration (LRS/GRS) des Tresors wie folgt mit [Set-AzRecoveryServicesBackupProperty](/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupproperty):

    ```powershell
    Get-AzRecoveryServicesVault `
        -Name "myRecoveryServicesVault" | Set-AzRecoveryServicesBackupProperty -BackupStorageRedundancy LocallyRedundant/GeoRedundant
    ```

    > [!NOTE]
    > Die Speicherredundanz kann nur geändert werden, wenn im Tresor keine Sicherungselemente vorhanden sind.

## <a name="enable-backup-for-an-azure-vm"></a>Aktivieren der Sicherung für eine Azure-VM

Sie aktivieren die Sicherung für einen virtuellen Azure-Computer und geben eine Sicherungsrichtlinie an.

- Die Richtlinie definiert, wann Sicherungen ausgeführt werden und wie lange die von den Sicherungen erstellten Wiederherstellungspunkte beibehalten werden sollen.
- Bei der Standardschutzrichtlinie wird einmal täglich eine Sicherung für den virtuellen Computer ausgeführt, und die erstellten Wiederherstellungspunkte werden 30 Tage lang beibehalten. Mit dieser Standardrichtlinie können Sie Ihren virtuellen Computer schnell schützen.

Aktivieren Sie die Sicherung wie folgt:

1. Legen Sie zuerst die Standardrichtlinie mit [Get-AzRecoveryServicesBackupProtectionPolicy](/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupprotectionpolicy) fest:

    ```powershell
    $policy = Get-AzRecoveryServicesBackupProtectionPolicy     -Name "DefaultPolicy"
    ```

2. Aktivieren Sie die VM-Sicherung mit [Enable-AzRecoveryServicesBackupProtection](/powershell/module/az.recoveryservices/enable-azrecoveryservicesbackupprotection). Geben Sie die Richtlinie, die Ressourcengruppe und den VM-Namen an.

    ```powershell
    Enable-AzRecoveryServicesBackupProtection `
        -ResourceGroupName "myResourceGroup" `
        -Name "myVM" `
        -Policy $policy
    ```

## <a name="start-a-backup-job"></a>Starten eines Sicherungsauftrags

Sicherungen werden gemäß dem in der Sicherungsrichtlinie angegebenen Zeitplan ausgeführt. Sie können auch eine bedarfsgesteuerte Sicherung durchführen:

- Beim ersten Sicherungsauftrag wird ein vollständiger Wiederherstellungspunkt erstellt.
- Bei jedem Sicherungsauftrag nach der ersten Sicherung werden inkrementelle Wiederherstellungspunkte erstellt.
- Inkrementelle Wiederherstellungspunkte sind in Bezug auf die Speicherung und die Dauer effizient, da nur Änderungen übertragen werden, die seit der letzten Sicherung vorgenommen wurden.

Zum Ausführen einer bedarfsgesteuerten Sicherung verwenden Sie das Cmdlet [Backup-AzRecoveryServicesBackupItem](/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupitem).

- Mit [Get-AzRecoveryServicesBackupContainer](/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupcontainer) geben Sie einen Container im Tresor an, der Ihre Sicherungsdaten enthält.
- Jede zu sichernde VM wird als gesondertes Element behandelt. Zum Starten eines Sicherungsauftrags rufen Sie mit [Get-AzRecoveryServicesBackupItem](/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupitem) Informationen zum virtuellen Computer ab.

Führen Sie eine bedarfsgesteuerte Sicherung wie folgt aus:

1. Geben Sie den Container an, rufen Sie die VM-Informationen ab, und führen Sie die Sicherung aus.

    ```powershell
    $backupcontainer = Get-AzRecoveryServicesBackupContainer `
        -ContainerType "AzureVM" `
        -FriendlyName "myVM"

    $item = Get-AzRecoveryServicesBackupItem `
        -Container $backupcontainer `
        -WorkloadType "AzureVM"

    Backup-AzRecoveryServicesBackupItem -Item $item
    ```

2. Da beim ersten Sicherungsauftrag ein vollständiger Wiederherstellungspunkt erstellt wird, müssen Sie möglicherweise bis zu 20 Minuten warten. Überwachen Sie den Auftrag wie im nächsten Verfahren beschrieben.

## <a name="monitor-the-backup-job"></a>Überwachen des Sicherungsauftrags

1. Führen Sie [Get-AzRecoveryservicesBackupJob](/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupjob) aus, um den Auftragsstatus zu überwachen.

    ```powershell
    Get-AzRecoveryservicesBackupJob
    ```

    Die Ausgabe ähnelt dem folgenden Beispiel, in dem für den Auftrag der Status **InProgress** angezeigt wird:

    ```output
    WorkloadName   Operation         Status       StartTime              EndTime                JobID
    ------------   ---------         ------       ---------              -------                -----
    myvm           Backup            InProgress   9/18/2017 9:38:02 PM                          9f9e8f14
    myvm           ConfigureBackup   Completed    9/18/2017 9:33:18 PM   9/18/2017 9:33:51 PM   fe79c739
    ```

2. Wenn der Auftragsstatus **Completed** lautet, ist der virtuelle Computer geschützt, und es wurde ein vollständiger Wiederherstellungspunkt gespeichert.

## <a name="manage-vm-backups"></a>Verwalten von VM-Sicherungen

Informationen zu weiteren Aktionen, z. B. Ändern oder Bearbeiten der Richtlinie usw., finden Sie im Abschnitt [Verwalten von VM-Sicherungen](backup-azure-vms-automation.md#manage-azure-vm-backups).

## <a name="clean-up-the-deployment"></a>Bereinigen der Bereitstellung

Wenn der virtuelle Computer nicht mehr gesichert werden muss, können Sie ihn bereinigen.

- Wenn Sie das Wiederherstellen des virtuellen Computers ausprobieren möchten, überspringen Sie die Bereinigung.
- Wenn Sie einen vorhandenen virtuellen Computer verwendet haben, können Sie das letzte Cmdlet [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) überspringen, um die Ressourcengruppe und den VM beizubehalten.

Deaktivieren Sie den Schutz, und entfernen Sie die Wiederherstellungspunkte und den Tresor. Löschen Sie anschließend die Ressourcengruppe und die zugehörigen VM-Ressourcen wie folgt:

```powershell
Disable-AzRecoveryServicesBackupProtection -Item $item -RemoveRecoveryPoints
$vault = Get-AzRecoveryServicesVault -Name "myRecoveryServicesVault"
Remove-AzRecoveryServicesVault -Vault $vault
Remove-AzResourceGroup -Name "myResourceGroup"
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einen Recovery Services-Tresor erstellt, den Schutz für einen virtuellen Computer aktiviert und den ersten Wiederherstellungspunkt erstellt.

- [Erfahren Sie, wie](tutorial-backup-vm-at-scale.md) Sie im Azure-Portal VMs sichern.
- [Erfahren Sie, wie](tutorial-restore-disk.md) Sie einen virtuellen Computer schnell wiederherstellen.