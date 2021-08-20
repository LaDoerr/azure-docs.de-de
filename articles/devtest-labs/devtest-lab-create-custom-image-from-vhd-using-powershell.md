---
title: Erstellen eines benutzerdefinierten Images aus einer VHD-Datei mit Azure PowerShell
description: Automatisieren Sie das Erstellen von benutzerdefinierten Azure DevTest Labs-Images aus VHD-Dateien mithilfe von PowerShell.
ms.topic: article
ms.date: 06/26/2020
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 8994f51311ec2b6942e807e5e501a25b45130f60
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122340253"
---
# <a name="create-a-custom-image-from-a-vhd-file-using-powershell"></a>Erstellen eines benutzerdefinierten Images aus einer VHD-Datei mithilfe von PowerShell

[!INCLUDE [devtest-lab-create-custom-image-from-vhd-selector](../../includes/devtest-lab-create-custom-image-from-vhd-selector.md)]

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

[!INCLUDE [devtest-lab-upload-vhd-options](../../includes/devtest-lab-upload-vhd-options.md)]

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="step-by-step-instructions"></a>Schrittweise Anweisungen

Die folgenden Schritte führen Sie durch die Erstellung eines benutzerdefinierten Images aus einer VHD-Datei mithilfe von PowerShell:

1. Melden Sie sich an einer PowerShell-Eingabeaufforderung mit dem folgenden Aufruf des Cmdlets **Connect-AzAccount** bei Ihrem Azure-Konto an.

    ```powershell
    Connect-AzAccount
    ```

1.  Wählen Sie das gewünschte Azure-Abonnement aus, indem Sie das Cmdlet **Select-AzSubscription** aufrufen. Ersetzen Sie den folgenden Platzhalter für die Variable **$subscriptionId** durch eine gültige Azure-Abonnement-ID.

    ```powershell
    $subscriptionId = '<Specify your subscription ID here>'
    Select-AzSubscription -SubscriptionId $subscriptionId
    ```

1.  Rufen Sie das Lab-Objekt durch Aufrufen des Cmdlets **Get-AzResource** ab. Ersetzen Sie die folgenden Platzhalter für die Variablen **$labRg** und **$labName** durch die entsprechenden Werte für Ihre Umgebung.

    ```powershell
    $labRg = '<Specify your lab resource group name here>'
    $labName = '<Specify your lab name here>'
    $lab = Get-AzResource -ResourceId ('/subscriptions/' + $subscriptionId + '/resourceGroups/' + $labRg + '/providers/Microsoft.DevTestLab/labs/' + $labName)
    ```

1.  Ersetzen Sie den folgenden Platzhalter für die Variable **$vhdUri** durch den URI zu der hochgeladenen VHD-Datei. Sie finden den URI der VHD-Datei im Azure-Portal auf dem Blatt „Blob“ des Speicherkontos.

    ```powershell
    $vhdUri = '<Specify the VHD URI here>'
    ```

1.  Erstellen Sie das benutzerdefinierte Image mit dem Cmdlet **New-AzResourceGroupDeployment**. Ersetzen Sie die folgenden Platzhalter für die Variablen **$customImageName** und **$customImageDescription** durch aussagekräftige Namen für Ihre Umgebung.

    ```powershell
    $customImageName = '<Specify the custom image name>'
    $customImageDescription = '<Specify the custom image description>'

    $parameters = @{existingLabName="$($lab.Name)"; existingVhdUri=$vhdUri; imageOsType='windows'; isVhdSysPrepped=$false; imageName=$customImageName; imageDescription=$customImageDescription}

    New-AzResourceGroupDeployment -ResourceGroupName $lab.ResourceGroupName -Name CreateCustomImage -TemplateUri 'https://raw.githubusercontent.com/Azure/azure-devtestlab/master/samples/DevTestLabs/QuickStartTemplates/201-dtl-create-customimage-from-vhd/azuredeploy.json' -TemplateParameterObject $parameters
    ```

## <a name="powershell-script-to-create-a-custom-image-from-a-vhd-file"></a>PowerShell-Skript zum Erstellen eines benutzerdefinierten Images aus einer VHD-Datei

Das folgende PowerShell-Skript kann zum Erstellen eines benutzerdefinierten Images aus einer VHD-Datei verwendet werden. Ersetzen Sie die Platzhalter (beginnend und endend mit spitzen Klammern) durch die entsprechenden Werte für Ihre Anforderungen.

```powershell
# Log in to your Azure account.
Connect-AzAccount

# Select the desired Azure subscription.
$subscriptionId = '<Specify your subscription ID here>'
Select-AzSubscription -SubscriptionId $subscriptionId

# Get the lab object.
$labRg = '<Specify your lab resource group name here>'
$labName = '<Specify your lab name here>'
$lab = Get-AzResource -ResourceId ('/subscriptions/' + $subscriptionId + '/resourceGroups/' + $labRg + '/providers/Microsoft.DevTestLab/labs/' + $labName)

# Set the URI of the VHD file.
$vhdUri = '<Specify the VHD URI here>'

# Set the custom image name and description values.
$customImageName = '<Specify the custom image name>'
$customImageDescription = '<Specify the custom image description>'

# Set up the parameters object.
$parameters = @{existingLabName="$($lab.Name)"; existingVhdUri=$vhdUri; imageOsType='windows'; isVhdSysPrepped=$false; imageName=$customImageName; imageDescription=$customImageDescription}

# Create the custom image.
New-AzResourceGroupDeployment -ResourceGroupName $lab.ResourceGroupName -Name CreateCustomImage -TemplateUri 'https://raw.githubusercontent.com/Azure/azure-devtestlab/master/samples/DevTestLabs/QuickStartTemplates/201-dtl-create-customimage-from-vhd/azuredeploy.json' -TemplateParameterObject $parameters
```

## <a name="related-blog-posts"></a>Verwandte Blogbeiträge

- [Custom images or formulas? (Benutzerdefinierte Images oder Formeln?)](/azure/devtest-labs/devtest-lab-faq#blog-post)
- [Copying Custom Images between Azure DevTest Labs (Kopieren benutzerdefinierter Images zwischen Azure DevTest Labs)](https://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

## <a name="next-steps"></a>Nächste Schritte

- [Hinzufügen eines virtuellen Computers zum Lab](devtest-lab-add-vm.md)