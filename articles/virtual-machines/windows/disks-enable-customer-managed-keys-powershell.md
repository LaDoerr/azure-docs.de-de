---
title: 'Azure PowerShell: Aktivieren kundenseitig verwalteter Schlüssel mit SSE – verwaltete Datenträger'
description: Aktivieren Sie die serverseitige Verschlüsselung mithilfe kundenseitig verwalteter Schlüssel auf Ihren verwalteten Datenträgern mit Azure PowerShell.
author: roygara
ms.date: 06/29/2021
ms.topic: how-to
ms.author: rogarana
ms.service: storage
ms.subservice: disks
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: c5d0bf7f209683668ddbffff7be11f932f3eeaee
ms.sourcegitcommit: 82d82642daa5c452a39c3b3d57cd849c06df21b0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2021
ms.locfileid: "113359030"
---
# <a name="azure-powershell---enable-customer-managed-keys-with-server-side-encryption---managed-disks"></a>Azure PowerShell: Aktivieren kundenseitig verwalteter Schlüssel mit serverseitiger Verschlüsselung – verwaltete Datenträger

Mit Azure Disk Storage können Sie Ihre eigenen Schlüssel verwalten, wenn Sie die serverseitige Verschlüsselung (Server Side Encryption, SSE) für verwaltete Datenträger verwenden. Informationen zum Konzept der SSE mit kundenseitig verwalteten Schlüsseln sowie weitere Verschlüsselungstypen für verwaltete Datenträger finden Sie im Abschnitt [Kundenseitig verwaltete Schlüssel](../disk-encryption.md#customer-managed-keys) in folgenden Artikeln zur Datenträgerverschlüsselung.

## <a name="restrictions"></a>Beschränkungen

Vorerst gelten für vom Kunden verwaltete Schlüssel die folgenden Einschränkungen:

- Wenn dieses Feature für Ihren Datenträger aktiviert ist, können Sie es nicht deaktivieren.
    Bei Bedarf müssen Sie [alle Daten auf einen anderen verwalteten Datenträger ohne kundenseitig verwaltete Schlüssel kopieren](disks-upload-vhd-to-managed-disk-powershell.md#copy-a-managed-disk).
[!INCLUDE [virtual-machines-managed-disks-customer-managed-keys-restrictions](../../../includes/virtual-machines-managed-disks-customer-managed-keys-restrictions.md)]

## <a name="set-up-an-azure-key-vault-and-diskencryptionset-without-automatic-key-rotation"></a>Einrichten von Azure Key Vault und DiskEncryptionSet ohne automatische Schlüsselrotation

Wenn Sie kundenseitig verwaltete Schlüssel mit SSE verwenden möchten, müssen Sie eine Azure Key Vault- und eine DiskEncryptionSet-Ressource einrichten.

[!INCLUDE [virtual-machines-disks-encryption-create-key-vault-powershell](../../../includes/virtual-machines-disks-encryption-create-key-vault-powershell.md)]

## <a name="set-up-an-azure-key-vault-and-diskencryptionset-with-automatic-key-rotation-preview"></a>Einrichten von Azure Key Vault und DiskEncryptionSet ohne automatische Schlüsselrotation

1. Vergewissern Sie sich, dass die neueste [Azure PowerShell-Version](/powershell/azure/install-az-ps) installiert ist und Sie mit `Connect-AzAccount` bei einem Azure-Konto angemeldet sind.
1. Erstellen Sie eine Azure Key Vault-Instanz und den Verschlüsselungsschlüssel.

    Beim Erstellen der Key Vault-Instanz müssen Sie den Schutz vor endgültigem Löschen aktivieren. Der Schutz vor endgültigem Löschen stellt sicher, dass ein gelöschter Schlüssel erst nach Ablauf der Aufbewahrungsdauer dauerhaft gelöscht werden kann. Diese Einstellung schützt Sie vor Datenverlust durch versehentliches Löschen und ist für die Verschlüsselung von verwalteten Datenträgern obligatorisch.
    
    ```powershell
    $ResourceGroupName="yourResourceGroupName"
    $LocationName="westcentralus"
    $keyVaultName="yourKeyVaultName"
    $keyName="yourKeyName"
    $keyDestination="Software"
    $diskEncryptionSetName="yourDiskEncryptionSetName"

    $keyVault = New-AzKeyVault -Name $keyVaultName -ResourceGroupName $ResourceGroupName -Location $LocationName -EnablePurgeProtection

    $key = Add-AzKeyVaultKey -VaultName $keyVaultName -Name $keyName -Destination $keyDestination  
    ```

1.  Erstellen Sie ein DiskEncryptionSet mithilfe der API-Version `2020-12-01`, und legen Sie die Eigenschaft `rotationToLatestKeyVersionEnabled` über die Azure Resource Manager-Vorlage [CreateDiskEncryptionSetWithAutoKeyRotation.json](https://raw.githubusercontent.com/Azure-Samples/managed-disks-powershell-getting-started/master/AutoKeyRotation/CreateDiskEncryptionSetWithAutoKeyRotation.json) auf „True“ fest.
    
    ```powershell
    New-AzResourceGroupDeployment -ResourceGroupName $ResourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure-Samples/managed-disks-powershell-getting-started/master/AutoKeyRotation/CreateDiskEncryptionSetWithAutoKeyRotation.json" `
    -diskEncryptionSetName $diskEncryptionSetName `
    -keyVaultId $($keyVault.ResourceId) `
    -keyVaultKeyUrl $($key.Key.Kid) `
    -encryptionType "EncryptionAtRestWithCustomerKey" `
    -region $LocationName
    ```

1.  Gewähren Sie der DiskEncryptionSet-Ressource Zugriff auf den Schlüsseltresor.

    > [!NOTE]
    > Es kann einige Minuten dauern, bis Azure die Identität des Datenträgerverschlüsselungssatzes in Azure Active Directory erstellt hat. Wenn Sie bei der Ausführung des folgenden Befehls einen ähnlichen Fehler wie „Active Directory-Objekt kann nicht gefunden werden“ erhalten, warten Sie einige Minuten, und versuchen Sie es erneut.

    ```powershell
    $des=Get-AzDiskEncryptionSet -Name $diskEncryptionSetName -ResourceGroupName $ResourceGroupName
    Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -ObjectId $des.Identity.PrincipalId -PermissionsToKeys wrapkey,unwrapkey,get
    ```

## <a name="examples"></a>Beispiele

Nachdem Sie diese Ressourcen erstellt und konfiguriert haben, können Sie sie zum Schützen Ihrer verwalteten Datenträger verwenden. Nachfolgend sehen Sie Beispielskripts mit jeweils einem entsprechenden Szenario, die Sie zum Schützen Ihrer verwalteten Datenträger verwenden können.

### <a name="create-a-vm-using-a-marketplace-image-encrypting-the-os-and-data-disks-with-customer-managed-keys"></a>Erstellen einer VM mit einem Marketplace-Image, Verschlüsseln der Datenträger für Betriebssystem und Daten mit vom Kunden verwalteten Schlüsseln

Kopieren Sie das Skript, ersetzen Sie alle Beispielwerte durch Ihre eigenen Parameter, und führen Sie es dann aus.

```powershell
$VMLocalAdminUser = "yourVMLocalAdminUserName"
$VMLocalAdminSecurePassword = ConvertTo-SecureString <password> -AsPlainText -Force
$LocationName = "yourRegion"
$ResourceGroupName = "yourResourceGroupName"
$ComputerName = "yourComputerName"
$VMName = "yourVMName"
$VMSize = "yourVMSize"
$diskEncryptionSetName="yourdiskEncryptionSetName"
    
$NetworkName = "yourNetworkName"
$NICName = "yourNICName"
$SubnetName = "yourSubnetName"
$SubnetAddressPrefix = "10.0.0.0/24"
$VnetAddressPrefix = "10.0.0.0/16"
    
$SingleSubnet = New-AzVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $SubnetAddressPrefix
$Vnet = New-AzVirtualNetwork -Name $NetworkName -ResourceGroupName $ResourceGroupName -Location $LocationName -AddressPrefix $VnetAddressPrefix -Subnet $SingleSubnet
$NIC = New-AzNetworkInterface -Name $NICName -ResourceGroupName $ResourceGroupName -Location $LocationName -SubnetId $Vnet.Subnets[0].Id
    
$Credential = New-Object System.Management.Automation.PSCredential ($VMLocalAdminUser, $VMLocalAdminSecurePassword);
    
$VirtualMachine = New-AzVMConfig -VMName $VMName -VMSize $VMSize
$VirtualMachine = Set-AzVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate
$VirtualMachine = Add-AzVMNetworkInterface -VM $VirtualMachine -Id $NIC.Id
$VirtualMachine = Set-AzVMSourceImage -VM $VirtualMachine -PublisherName 'MicrosoftWindowsServer' -Offer 'WindowsServer' -Skus '2012-R2-Datacenter' -Version latest

$diskEncryptionSet=Get-AzDiskEncryptionSet -ResourceGroupName $ResourceGroupName -Name $diskEncryptionSetName

$VirtualMachine = Set-AzVMOSDisk -VM $VirtualMachine -Name $($VMName +"_OSDisk") -DiskEncryptionSetId $diskEncryptionSet.Id -CreateOption FromImage

$VirtualMachine = Add-AzVMDataDisk -VM $VirtualMachine -Name $($VMName +"DataDisk1") -DiskSizeInGB 128 -StorageAccountType Premium_LRS -CreateOption Empty -Lun 0 -DiskEncryptionSetId $diskEncryptionSet.Id 
    
New-AzVM -ResourceGroupName $ResourceGroupName -Location $LocationName -VM $VirtualMachine -Verbose
```

### <a name="create-an-empty-disk-encrypted-using-server-side-encryption-with-customer-managed-keys-and-attach-it-to-a-vm"></a>Erstellen eines leeren Datenträgers, der mit serverseitiger Verschlüsselung und vom Kunden verwalteten Schlüsseln verschlüsselt wird, und Anfügen an eine VM

Kopieren Sie das Skript, ersetzen Sie alle Beispielwerte durch Ihre eigenen Parameter, und führen Sie es dann aus.

```PowerShell
$vmName = "yourVMName"
$LocationName = "westcentralus"
$ResourceGroupName = "yourResourceGroupName"
$diskName = "yourDiskName"
$diskSKU = "Premium_LRS"
$diskSizeinGiB = 30
$diskLUN = 1
$diskEncryptionSetName="yourDiskEncryptionSetName"


$vm = Get-AzVM -Name $vmName -ResourceGroupName $ResourceGroupName 

$diskEncryptionSet=Get-AzDiskEncryptionSet -ResourceGroupName $ResourceGroupName -Name $diskEncryptionSetName

$vm = Add-AzVMDataDisk -VM $vm -Name $diskName -CreateOption Empty -DiskSizeInGB $diskSizeinGiB -StorageAccountType $diskSKU -Lun $diskLUN -DiskEncryptionSetId $diskEncryptionSet.Id 

Update-AzVM -ResourceGroupName $ResourceGroupName -VM $vm

```

### <a name="encrypt-existing-managed-disks"></a>Verschlüsseln vorhandener verwalteter Datenträger 

Die vorhandenen Datenträger dürfen nicht an einen laufenden virtuellen Computer angefügt werden, damit Sie sie mithilfe des folgenden Skripts verschlüsseln können:

```PowerShell
$rgName = "yourResourceGroupName"
$diskName = "yourDiskName"
$diskEncryptionSetName = "yourDiskEncryptionSetName"
 
$diskEncryptionSet = Get-AzDiskEncryptionSet -ResourceGroupName $rgName -Name $diskEncryptionSetName
 
New-AzDiskUpdateConfig -EncryptionType "EncryptionAtRestWithCustomerKey" -DiskEncryptionSetId $diskEncryptionSet.Id | Update-AzDisk -ResourceGroupName $rgName -DiskName $diskName
```

### <a name="encrypt-an-existing-virtual-machine-scale-set-with-sse-and-customer-managed-keys"></a>Verschlüsseln einer vorhandenen VM-Skalierungsgruppe mit SSE und kundenseitig verwalteten Schlüsseln 

Kopieren Sie das Skript, ersetzen Sie alle Beispielwerte durch Ihre eigenen Parameter, und führen Sie es dann aus:

```powershell
#set variables 
$vmssname = "name of the vmss that is already created"
$diskencryptionsetname = "name of the diskencryptionset already created"
$vmssrgname = "vmss resourcegroup name"
$diskencryptionsetrgname = "diskencryptionset resourcegroup name"

#get vmss object and create diskencryptionset object attach to vmss os disk
$ssevmss = get-azvmss -ResourceGroupName $vmssrgname -VMScaleSetName $vmssname
$ssevmss.VirtualMachineProfile.StorageProfile.OsDisk.ManagedDisk.DiskEncryptionSet = New-Object -TypeName Microsoft.Azure.Management.Compute.Models.DiskEncryptionSetParameters

#get diskencryption object and retrieve the resource id
$des = Get-AzDiskEncryptionSet -ResourceGroupName $diskencryptionsetrgname -Name $diskencryptionsetname
write-host "the diskencryptionset resource id is:" $des.Id

#associate DES resource id to os disk and update vmss 
$ssevmss.VirtualMachineProfile.StorageProfile.OsDisk.ManagedDisk.DiskEncryptionSet.id = $des.Id
$ssevmss | update-azvmss
```

### <a name="create-a-virtual-machine-scale-set-using-a-marketplace-image-encrypting-the-os-and-data-disks-with-customer-managed-keys"></a>Erstellen einer VM-Skalierungsgruppe mit einem Marketplace-Image, Verschlüsseln der Datenträger für Betriebssystem und Daten mit vom Kunden verwalteten Schlüsseln

Kopieren Sie das Skript, ersetzen Sie alle Beispielwerte durch Ihre eigenen Parameter, und führen Sie es dann aus.

```PowerShell
$VMLocalAdminUser = "yourLocalAdminUser"
$VMLocalAdminSecurePassword = ConvertTo-SecureString Password@123 -AsPlainText -Force
$LocationName = "westcentralus"
$ResourceGroupName = "yourResourceGroupName"
$ComputerNamePrefix = "yourComputerNamePrefix"
$VMScaleSetName = "yourVMSSName"
$VMSize = "Standard_DS3_v2"
$diskEncryptionSetName="yourDiskEncryptionSetName"
    
$NetworkName = "yourVNETName"
$SubnetName = "yourSubnetName"
$SubnetAddressPrefix = "10.0.0.0/24"
$VnetAddressPrefix = "10.0.0.0/16"
    
$SingleSubnet = New-AzVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $SubnetAddressPrefix

$Vnet = New-AzVirtualNetwork -Name $NetworkName -ResourceGroupName $ResourceGroupName -Location $LocationName -AddressPrefix $VnetAddressPrefix -Subnet $SingleSubnet

$ipConfig = New-AzVmssIpConfig -Name "myIPConfig" -SubnetId $Vnet.Subnets[0].Id 

$VMSS = New-AzVmssConfig -Location $LocationName -SkuCapacity 2 -SkuName $VMSize -UpgradePolicyMode 'Automatic'

$VMSS = Add-AzVmssNetworkInterfaceConfiguration -Name "myVMSSNetworkConfig" -VirtualMachineScaleSet $VMSS -Primary $true -IpConfiguration $ipConfig

$diskEncryptionSet=Get-AzDiskEncryptionSet -ResourceGroupName $ResourceGroupName -Name $diskEncryptionSetName

# Enable encryption at rest with customer managed keys for OS disk by setting DiskEncryptionSetId property 

$VMSS = Set-AzVmssStorageProfile $VMSS -OsDiskCreateOption "FromImage" -DiskEncryptionSetId $diskEncryptionSet.Id -ImageReferenceOffer 'WindowsServer' -ImageReferenceSku '2012-R2-Datacenter' -ImageReferenceVersion latest -ImageReferencePublisher 'MicrosoftWindowsServer'

$VMSS = Set-AzVmssOsProfile $VMSS -ComputerNamePrefix $ComputerNamePrefix -AdminUsername $VMLocalAdminUser -AdminPassword $VMLocalAdminSecurePassword

# Add a data disk encrypted at rest with customer managed keys by setting DiskEncryptionSetId property 

$VMSS = Add-AzVmssDataDisk -VirtualMachineScaleSet $VMSS -CreateOption Empty -Lun 1 -DiskSizeGB 128 -StorageAccountType Premium_LRS -DiskEncryptionSetId $diskEncryptionSet.Id

$Credential = New-Object System.Management.Automation.PSCredential ($VMLocalAdminUser, $VMLocalAdminSecurePassword);

New-AzVmss -VirtualMachineScaleSet $VMSS -ResourceGroupName $ResourceGroupName -VMScaleSetName $VMScaleSetName
```

### <a name="change-the-key-of-a-diskencryptionset-to-rotate-the-key-for-all-the-resources-referencing-the-diskencryptionset"></a>Ändern des Schlüssels eines DiskEncryptionSet, um den Schlüssel für alle Ressourcen, die auf das DiskEncryptionSet verweisen, zu rotieren

Kopieren Sie das Skript, ersetzen Sie alle Beispielwerte durch Ihre eigenen Parameter, und führen Sie es dann aus.

```PowerShell
$ResourceGroupName="yourResourceGroupName"
$keyVaultName="yourKeyVaultName"
$keyName="yourKeyName"
$diskEncryptionSetName="yourDiskEncryptionSetName"

$keyVault = Get-AzKeyVault -VaultName $keyVaultName -ResourceGroupName $ResourceGroupName

$keyVaultKey = Get-AzKeyVaultKey -VaultName $keyVaultName -Name $keyName

Update-AzDiskEncryptionSet -Name $diskEncryptionSetName -ResourceGroupName $ResourceGroupName -SourceVaultId $keyVault.ResourceId -KeyUrl $keyVaultKey.Id
```

### <a name="find-the-status-of-server-side-encryption-of-a-disk"></a>Ermitteln des Status der serverseitigen Verschlüsselung eines Datenträgers

[!INCLUDE [virtual-machines-disks-encryption-status-powershell](../../../includes/virtual-machines-disks-encryption-status-powershell.md)]

> [!IMPORTANT]
> Von Kunden verwaltete Schlüssel basieren auf verwalteten Identitäten für Azure-Ressourcen, einem Feature von Azure Active Directory (Azure AD). Wenn Sie vom Kunden verwaltete Schlüssel konfigurieren, wird Ihren Ressourcen im Hintergrund automatisch eine verwaltete Identität zugewiesen. Wenn Sie anschließend das Abonnement, die Ressourcengruppe oder den verwalteten Datenträger von einem Azure AD-Verzeichnis in ein anderes Verzeichnis verschieben, wird die den verwalteten Datenträgern zugeordnete verwaltete Identität nicht an den neuen Mandanten übertragen, sodass kundenseitig verwaltete Schlüssel möglicherweise nicht mehr funktionieren. Weitere Informationen finden Sie unter [Übertragen eines Abonnements zwischen Azure AD-Verzeichnissen](../../active-directory/managed-identities-azure-resources/known-issues.md#transferring-a-subscription-between-azure-ad-directories).

## <a name="next-steps"></a>Nächste Schritte

- [Untersuchen der Azure Resource Manager-Vorlagen zum Erstellen verschlüsselter Datenträger mit vom Kunden verwalteten Schlüsseln](https://github.com/ramankumarlive/manageddiskscmkpreview)
- [Replizieren von Computern mit Datenträgern, die für kundenseitig verwaltete Schlüssel aktiviert sind](../../site-recovery/azure-to-azure-how-to-enable-replication-cmk-disks.md)
- [Einrichten der Notfallwiederherstellung von virtuellen VMware-Computern in Azure mithilfe von PowerShell](../../site-recovery/vmware-azure-disaster-recovery-powershell.md#replicate-vmware-vms)
- [Einrichten der Notfallwiederherstellung in Azure für Hyper-V-VMs mithilfe von PowerShell und Azure Resource Manager](../../site-recovery/hyper-v-azure-powershell-resource-manager.md#step-7-enable-vm-protection)
