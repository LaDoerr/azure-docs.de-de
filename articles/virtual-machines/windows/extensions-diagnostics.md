---
title: Azure-Diagnoseerweiterung für Windows
description: Überwachen von virtuellen Azure-Computern unter Windows mithilfe der Azure-Diagnoseerweiterung
author: johnkemnetz
manager: ashwink
ms.service: virtual-machines
ms.subservice: extensions
ms.collection: windows
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 04/06/2018
ms.author: johnkem
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 7afc6c67f52d205cdb7aa524ee01f6e14b1c5de2
ms.sourcegitcommit: 58d82486531472268c5ff70b1e012fc008226753
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/23/2021
ms.locfileid: "122690737"
---
# <a name="azure-diagnostics-extension-for-windows-vms"></a>Azure-Diagnoseerweiterung für virtuelle Windows-Computer

**Gilt für**: :heavy_check_mark: Windows-VMs :heavy_check_mark: Flexible Skalierungsgruppen:heavy_check_mark: Einheitliche Skalierungsgruppen

## <a name="overview"></a>Übersicht

Mit der VM-Erweiterung für die Azure-Diagnose können Sie Überwachungsdaten wie Leistungsindikatoren und Ereignisprotokolle auf Ihrem virtuellen Windows-Computer sammeln. Sie können genau angeben, welche Daten erfasst und wo sie gespeichert werden sollen, etwa in einem Azure Storage-Konto oder einem Azure Event Hub. Sie können diese Daten auch zum Erstellen von Diagrammen im Azure-Portal bzw. zum Erstellen von Metrikwarnungen verwenden.

## <a name="prerequisites"></a>Voraussetzungen

### <a name="operating-system"></a>Betriebssystem

Die Azure-Diagnoseerweiterung ist für Windows 10-Clients und Windows Server 2008 R2, 2012, 2012 R2 und 2016 geeignet.

### <a name="internet-connectivity"></a>Internetkonnektivität

Um die Azure-Diagnoseerweiterung verwenden zu können, muss der virtuelle Zielcomputer mit dem Internet verbunden sein. 

## <a name="extension-schema"></a>Erweiterungsschema

[Die Schema- und Eigenschaftswerte der Azure-Diagnoseerweiterung werden in diesem Dokument beschrieben.](../../azure-monitor/agents/diagnostics-extension-schema-windows.md)

## <a name="template-deployment"></a>Bereitstellung von Vorlagen

Azure-VM-Erweiterungen können mithilfe von Azure Resource Manager-Vorlagen bereitgestellt werden. Das im vorherigen Abschnitt erläuterte JSON-Schema kann in einer Azure Resource Manager-Vorlage zum Ausführen der Azure-Diagnoseerweiterung im Rahmen einer Bereitstellung mit einer Azure Resource Manager-Vorlage verwendet werden. Informationen finden Sie unter [Verwenden von Überwachung und Diagnose bei einer Windows-VM und Azure Resource Manager-Vorlagen](../extensions/diagnostics-template.md).

## <a name="azure-cli-deployment"></a>Bereitstellung mithilfe der Azure-Befehlszeilenschnittstelle

Sie können die Azure-Diagnoseerweiterung mithilfe der Azure-Befehlszeilenschnittstelle auf einem vorhandenen virtuellen Computer bereitstellen. Ersetzen Sie die geschützten Einstellungen und Einstellungseigenschaften mit gültigem JSON-Code aus dem oben aufgeführten Erweiterungsschema. 

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name IaaSDiagnostics \
  --publisher Microsoft.Azure.Diagnostics \
  --version 1.9.0.0 --protected-settings protected-settings.json \
  --settings public-settings.json 
```

## <a name="powershell-deployment"></a>PowerShell-Bereitstellung

Sie können die Azure-Diagnoseerweiterung mithilfe des Befehls `Set-AzVMDiagnosticsExtension` einem vorhandenen virtuellen Computer hinzufügen. Informationen finden Sie außerdem unter [Aktivieren der Azure-Diagnose auf einer virtuellen Azure-Maschine unter Windows mithilfe von PowerShell](../extensions/diagnostics-windows.md).

 


```powershell
$vm_resourcegroup = "myvmresourcegroup"
$vm_name = "myvm"
$diagnosticsconfig_path = "DiagnosticsPubConfig.xml"

Set-AzVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup `
  -VMName $vm_name `
  -DiagnosticsConfigurationPath $diagnosticsconfig_path
```

## <a name="troubleshoot-and-support"></a>Problembehandlung und Support

### <a name="troubleshoot"></a>Problembehandlung

Daten zum Status von Erweiterungsbereitstellungen können über das Azure-Portal und mithilfe der Azure-Befehlszeilenschnittstelle abgerufen werden. Führen Sie über die Azure-Befehlszeilenschnittstelle den folgenden Befehl aus, um den Bereitstellungsstatus von Erweiterungen für einen bestimmten virtuellen Computer anzuzeigen.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

In [diesem Artikel](../../azure-monitor/agents/diagnostics-extension-troubleshooting.md) finden Sie einen umfassenderen Leitfaden zur Problembehandlung für die Azure-Diagnoseerweiterung.

### <a name="support"></a>Support

Sollten Sie beim Lesen dieses Artikels feststellen, dass Sie weitere Hilfe benötigen, können Sie sich über das [MSDN Azure-Forum oder über das Stack Overflow-Forum](https://azure.microsoft.com/support/forums/) mit Azure-Experten in Verbindung setzen. Alternativ dazu haben Sie die Möglichkeit, einen Azure-Supportfall zu erstellen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie „Support erhalten“ aus. Informationen zur Nutzung von Azure-Support finden Sie unter [Microsoft Azure-Support-FAQ](https://azure.microsoft.com/support/faq/).

## <a name="next-steps"></a>Nächste Schritte
* [Weitere Informationen zur Azure-Diagnoseerweiterung](../../azure-monitor/agents/diagnostics-extension-overview.md)
* [Informationen zum Schema und zu den Versionen der Erweiterung](../../azure-monitor/agents/diagnostics-extension-schema-windows.md)
