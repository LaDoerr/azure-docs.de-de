---
title: Azure Monitor Dependency-VM-Erweiterung für Windows
description: Stellen Sie den Azure Monitor Dependency-Agent auf einem virtuellen Windows-Computer bereit, indem Sie eine VM-Erweiterung verwenden.
ms.topic: article
ms.service: virtual-machines
ms.subservice: extensions
author: mgoedtel
ms.author: magoedte
ms.collection: windows
ms.date: 06/01/2021
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 1de4facc6cc945b5cada2201d3da667efae793aa
ms.sourcegitcommit: 7f59e3b79a12395d37d569c250285a15df7a1077
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/02/2021
ms.locfileid: "110797355"
---
# <a name="azure-monitor-dependency-virtual-machine-extension-for-windows"></a>Azure Monitor Dependency-VM-Erweiterung für Windows

Das Zuordnungsfeature in Azure Monitor für VMs erhält seine Daten vom Microsoft Dependency-Agent. Die Azure VM Dependency-Agent-VM-Erweiterung für Windows wird von Microsoft veröffentlicht und unterstützt. Die Erweiterung installiert den Dependency-Agent auf virtuellen Azure-Computern. Dieses Dokument enthält ausführliche Informationen zu den unterstützten Plattformen, Konfigurationen und Bereitstellungsoptionen für die Azure Dependency-Agent-VM-Erweiterung für Windows.

## <a name="operating-system"></a>Betriebssystem

Die Azure VM Dependency-Agent-Erweiterung für Windows kann für die unterstützten Betriebssysteme ausgeführt werden, die im Abschnitt [Unterstützte Betriebssysteme](../../azure-monitor/vm/vminsights-enable-overview.md#supported-operating-systems) des Bereitstellungsartikels zu Azure Monitor für VMs aufgelistet sind.

## <a name="extension-schema"></a>Erweiterungsschema

Das folgende JSON zeigt das Schema für die Azure VM Dependency-Agent-Erweiterung auf einem virtuellen Azure Windows-Computer.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
  "parameters": {
    "vmName": {
      "type": "string",
      "metadata": {
        "description": "The name of existing Azure VM. Supported Windows Server versions:  2008 R2 and above (x64)."
      }
    }
  },
  "variables": {
    "vmExtensionsApiVersion": "2017-03-30"
  },
  "resources": [
    {
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "name": "[concat(parameters('vmName'),'/DAExtension')]",
      "apiVersion": "[variables('vmExtensionsApiVersion')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
      ],
      "properties": {
        "publisher": "Microsoft.Azure.Monitoring.DependencyAgent",
        "type": "DependencyAgentWindows",
        "typeHandlerVersion": "9.5",
        "autoUpgradeMinorVersion": true
      }
    }
  ],
    "outputs": {
    }
}
```

### <a name="property-values"></a>Eigenschaftswerte

| Name | Beispiel/Wert |
| ---- | ---- |
| apiVersion | 2015-01-01 |
| publisher | Microsoft.Azure.Monitoring.DependencyAgent |
| Type | DependencyAgentWindows |
| typeHandlerVersion | 9.5 |

## <a name="template-deployment"></a>Bereitstellung von Vorlagen

Sie können die Azure-VM-Erweiterungen mithilfe von Azure Resource Manager-Vorlagen bereitstellen. Sie können das im vorherigen Abschnitt erläuterte JSON-Schema in einer Azure Resource Manager-Vorlage verwenden, um die Azure VM Dependency-Agent-Erweiterung im Rahmen einer Azure Resource Manager-Bereitstellung auszuführen.

Der JSON-Code für eine VM-Erweiterung kann innerhalb der VM-Ressource geschachtelt werden. Alternativ können Sie den Code auf der Stamm- bzw. obersten Ebene einer Resource Manager-JSON-Vorlage platzieren. Die Platzierung des JSON-Codes wirkt sich auf den Wert von Name und Typ der Ressource aus. Weitere Informationen finden Sie unter [Set name and type for child resources](../../azure-resource-manager/templates/child-resource-name-type.md) (Festlegen von Name und Typ für untergeordnete Ressourcen).

Im folgenden Beispiel wird davon ausgegangen, dass die Dependency-Agent-Erweiterung in der VM-Ressource geschachtelt ist. Wenn Sie die Erweiterungsressource schachteln, wird der JSON-Code im `"resources": []`-Objekt des virtuellen Computers platziert.


```json
{
    "type": "extensions",
    "name": "DAExtension",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Azure.Monitoring.DependencyAgent",
        "type": "DependencyAgentWindows",
        "typeHandlerVersion": "9.5",
        "autoUpgradeMinorVersion": true
    }
}
```

Wenn Sie den JSON-Code für die Erweiterung auf der Stammebene der Vorlage platzieren, enthält der Ressourcenname einen Verweis auf den übergeordneten virtuellen Computer. Der Typ spiegelt die geschachtelte Konfiguration wider.

```json
{
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "name": "<parentVmResource>/DAExtension",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Azure.Monitoring.DependencyAgent",
        "type": "DependencyAgentWindows",
        "typeHandlerVersion": "9.5",
        "autoUpgradeMinorVersion": true
    }
}
```

## <a name="powershell-deployment"></a>PowerShell-Bereitstellung

Sie können den Befehl `Set-AzVMExtension` verwenden, um die Dependency-Agent-VM-Erweiterung auf einem vorhandenen virtuellen Computer bereitzustellen. Bevor Sie den Befehl ausführen, müssen die öffentliche und die private Konfiguration in einer PowerShell-Hashtabelle gespeichert werden.

```powershell

Set-AzVMExtension -ExtensionName "Microsoft.Azure.Monitoring.DependencyAgent" `
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Publisher "Microsoft.Azure.Monitoring.DependencyAgent" `
    -ExtensionType "DependencyAgentWindows" `
    -TypeHandlerVersion 9.5 `
    -Location WestUS 
```

## <a name="automatic-upgrade-preview"></a>Automatisches Upgrade (Vorschau)
Ein neues Feature zum automatischen Aktualisieren von Nebenversionen der Dependency-Erweiterung ist jetzt als öffentliche Vorschau verfügbar. Zum Aktivieren dieses Features müssen Sie die folgenden Konfigurationsänderungen vornehmen.

-   Verwenden Sie eine der Methoden unter [Aktivieren des Vorschauzugriffs](../automatic-extension-upgrade.md#enabling-preview-access), um das Feature für Ihr Abonnement zu aktivieren.
- Fügen Sie der Vorlage das Attribut `enableAutomaticUpgrade` hinzu.

Für das Schema der Versionsverwaltung der Dependency-Agent-Erweiterung gilt das folgende Format:

```
<MM.mm.bb.rr> where M = Major version number, m = minor version number, b = bug number, r = revision number.
```

Die Attribute `enableAutomaticUpgrade` und `autoUpgradeMinorVersion` bestimmen gemeinsam, wie Upgrades für virtuelle Computer im Abonnement behandelt werden.

| enableAutomaticUpgrade | autoUpgradeMinorVersion | Wirkung |
|:---|:---|:---|
| true | false | Aktualisierung des Dependency-Agents, falls eine neuere Version von „bb.rr“ vorhanden ist. Wenn Sie beispielsweise Version 9.6.0.1355 ausführen und die neuere Version 9.6.2.1366 lautet, werden Ihre virtuellen Computer in aktivierten Abonnements auf 9.6.2.1366 aktualisiert. |
| true | true |  Dadurch wird der Dependency-Agent aktualisiert, wenn eine neuere Version von „mm.bb.rr“ oder „bb.rr“ vorhanden ist. Wenn Sie beispielsweise Version 9.6.0.1355 ausführen und die neuere Version 9.7.1.1416 lautet, werden Ihre virtuellen Computer in aktivierten Abonnements auf 9.7.1.1416 aktualisiert. Wenn Sie beispielsweise Version 9.6.0.1355 ausführen und die neuere Version 9.6.2.1366 lautet, werden Ihre virtuellen Computer in aktivierten Abonnements ebenfalls auf 9.6.2.1366 aktualisiert. |
| false | true oder false | Das automatische Upgrade ist deaktiviert.

> [!IMPORTANT]
> Wenn Sie Ihrer Vorlage `enableAutomaticUpgrade` hinzufügen, stellen Sie sicher, dass Sie mindestens die API-Version 2019-12-01 verwenden.

## <a name="troubleshoot-and-support"></a>Problembehandlung und Support

### <a name="troubleshoot"></a>Problembehandlung

Daten zum Status von Erweiterungsbereitstellungen können aus dem Azure-Portal und über das Azure PowerShell-Modul abgerufen werden. Führen Sie im Azure PowerShell-Modul den folgenden Befehl aus, um den Bereitstellungsstatus von Erweiterungen für einen bestimmten virtuellen Computer anzuzeigen:

```powershell
Get-AzVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

Die Ausgabe der Erweiterungsausführung wird in Dateien im folgenden Verzeichnis protokolliert:

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Monitoring.DependencyAgent\
```

### <a name="support"></a>Support

Sollten Sie beim Lesen dieses Artikels feststellen, dass Sie weitere Hilfe benötigen, können Sie sich über das [MSDN Azure-Forum oder über das Stack Overflow-Forum](https://azure.microsoft.com/support/forums/) mit Azure-Experten in Verbindung setzen. Sie können auch einen Azure-Supportfall erstellen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie **Support erhalten** aus. Informationen darüber, wie Azure-Support genutzt werden kann, finden Sie unter [Häufig gestellte Fragen zum Azure-Support](https://azure.microsoft.com/support/faq/).
