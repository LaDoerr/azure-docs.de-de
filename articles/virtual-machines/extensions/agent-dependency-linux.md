---
title: Azure Monitor Dependency-VM-Erweiterung für Linux
description: Stellen Sie den Azure Monitor Dependency-Agent mithilfe einer VM-Erweiterung auf einem virtuellen Linux-Computer bereit.
ms.topic: article
ms.service: virtual-machines
ms.subservice: extensions
author: mgoedtel
ms.author: magoedte
ms.collection: linux
ms.date: 06/01/2021
ms.openlocfilehash: 97f557ec45530de3f42dd61ee1cded57fd7c33a0
ms.sourcegitcommit: 7f59e3b79a12395d37d569c250285a15df7a1077
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/02/2021
ms.locfileid: "110793744"
---
# <a name="azure-monitor-dependency-virtual-machine-extension-for-linux"></a>Azure Monitor Dependency-VM-Erweiterung für Linux

Das Zuordnungsfeature in Azure Monitor für VMs erhält seine Daten vom Microsoft Dependency-Agent. Die Azure VM Dependency-Agent-VM-Erweiterung für Linux wird von Microsoft veröffentlicht und unterstützt. Die Erweiterung installiert den Dependency-Agent auf virtuellen Azure-Computern. Dieses Dokument enthält ausführliche Informationen zu den unterstützten Plattformen, Konfigurationen und Bereitstellungsoptionen für die Azure Dependency-Agent-VM-Erweiterung für Linux.

## <a name="prerequisites"></a>Voraussetzungen

### <a name="operating-system"></a>Betriebssystem

Die Azure VM Dependency-Agent-Erweiterung für Linux kann für die unterstützten Betriebssysteme ausgeführt werden, die im Abschnitt [Unterstützte Betriebssysteme](../../azure-monitor/vm/vminsights-enable-overview.md#supported-operating-systems) des Bereitstellungsartikels zu Azure Monitor für VMs aufgelistet sind.

## <a name="extension-schema"></a>Erweiterungsschema

Das folgende JSON zeigt das Schema für die Azure VM Dependency-Agent-Erweiterung auf einem virtuellen Azure Linux-Computer. 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
  "parameters": {
    "vmName": {
      "type": "string",
      "metadata": {
        "description": "The name of existing Linux Azure VM."
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
        "type": "DependencyAgentLinux",
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
| Type | DependencyAgentLinux |
| typeHandlerVersion | 9.5 |

## <a name="template-deployment"></a>Bereitstellung von Vorlagen

Sie können Azure-VM-Erweiterungen mithilfe von Azure Resource Manager-Vorlagen bereitstellen. Sie können das im vorherigen Abschnitt erläuterte JSON-Schema in einer Azure Resource Manager-Vorlage verwenden, um die Azure VM Dependency-Agent-Erweiterung im Rahmen einer Azure Resource Manager-Bereitstellung auszuführen.

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
        "type": "DependencyAgentLinux",
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
        "type": "DependencyAgentLinux",
        "typeHandlerVersion": "9.5",
        "autoUpgradeMinorVersion": true
    }
}
```

## <a name="azure-cli-deployment"></a>Bereitstellung mithilfe der Azure-Befehlszeilenschnittstelle

Sie können die Dependency-Agent-VM-Erweiterung mithilfe der Azure CLI auf einem vorhandenen virtuellen Computer bereitstellen.  

```azurecli

az vm extension set \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --name DependencyAgentLinux \
    --publisher Microsoft.Azure.Monitoring.DependencyAgent \
    --version 9.5 
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

Daten zum Status von Erweiterungsbereitstellungen können über das Azure-Portal und mithilfe der Azure-Befehlszeilenschnittstelle abgerufen werden. Führen Sie über die Azure-Befehlszeilenschnittstelle den folgenden Befehl aus, um den Bereitstellungsstatus von Erweiterungen für einen bestimmten virtuellen Computer anzuzeigen:

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Die Ausgabe der Erweiterungsausführung wird in der folgenden Datei protokolliert:

```
/opt/microsoft/dependency-agent/log/install.log 
```

### <a name="support"></a>Support

Wenn Sie beim Lesen dieses Artikels feststellen, dass Sie weitere Hilfe benötigen, stellen Sie Azure-Experten im [MSDN Azure-Forum oder im Stack Overflow-Forum](https://azure.microsoft.com/support/forums/)Fragen. Sie können auch einen Azure-Supportfall erstellen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie **Support erhalten** aus. Informationen darüber, wie Azure-Support genutzt werden kann, finden Sie unter [Häufig gestellte Fragen zum Azure-Support](https://azure.microsoft.com/support/faq/).
