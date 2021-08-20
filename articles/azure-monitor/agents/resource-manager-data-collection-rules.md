---
title: Resource Manager-Beispielvorlagen für Datensammlungsregeln
description: Azure Resource Manager-Beispielvorlagen zum Erstellen von Zuordnungen zwischen Datensammlungsregeln und virtuellen Computern in Azure Monitor.
ms.topic: sample
author: bwren
ms.author: bwren
ms.date: 11/17/2020
ms.openlocfilehash: 24bc9b0f92a82354f511f534eb9b0e86d59d336b
ms.sourcegitcommit: e1874bb73cb669ce1e5203ec0a3777024c23a486
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/16/2021
ms.locfileid: "112201264"
---
# <a name="resource-manager-template-samples-for-data-collection-rules-in-azure-monitor"></a>Resource Manager-Beispielvorlagen für Datensammlungsregeln in Azure Monitor
Dieser Artikel enthält Beispiele für [Azure Resource Manager-Vorlagen](../../azure-resource-manager/templates/syntax.md) zum Erstellen einer Zuordnung zwischen einer [Datensammlungsregel](data-collection-rule-overview.md) und dem [Azure Monitor-Agent](./azure-monitor-agent-overview.md). Jedes Beispiel umfasst eine Vorlagendatei und eine Parameterdatei mit Beispielwerten für die Vorlage.

[!INCLUDE [azure-monitor-samples](../../../includes/azure-monitor-resource-manager-samples.md)]

## <a name="create-rule-sample"></a>Erstellen einer Regel (Beispiel)

Sehen Sie sich das [Vorlagenformat](/azure/templates/microsoft.insights/datacollectionrules) an.

## <a name="create-association-with-azure-vm"></a>Erstellen einer Zuordnung mit einem virtuellen Azure-Computer

Mit dem folgenden Beispiel wird eine Zuordnung zwischen einem virtuellen Azure-Computer und einer Datensammlungsregel erstellt.

### <a name="template-file"></a>Vorlagendatei

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "vmName": {
            "type": "string",
            "metadata": {
                "description": "Name of the virtual machine."
            }
        },
        "associationName": {
            "type": "string",
            "metadata": {
                "description": "Name of the association."
            }
        },
        "dataCollectionRuleId": {
            "type": "string",
            "metadata": {
                "description": "Resource ID of the data collection rule."
            }
        }
    },
    "resources": [
        {
            "type": "Microsoft.Compute/virtualMachines/providers/dataCollectionRuleAssociations",
            "name": "[concat(parameters('vmName'),'/microsoft.insights/', parameters('associationName'))]",
            "apiVersion": "2019-11-01-preview",
            "properties": {
                "description": "Association of data collection rule. Deleting this association will break the data collection for this virtual machine.",
                "dataCollectionRuleId": "[parameters('dataCollectionRuleId')]"
            }
        }
    ]
}
```

### <a name="parameter-file"></a>Parameterdatei

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "vmName": {
        "value": "my-windows-vm"
      },
      "location": {
        "value": "eastus"
      }
  }
}
```

## <a name="create-association-with-azure-arc"></a>Erstellen einer Zuordnung mit Azure Arc

Im folgenden Beispiel wird eine Zuordnung zwischen einem Server mit Azure Arc-Unterstützung und einer Datensammlungsregel erstellt.

### <a name="template-file"></a>Vorlagendatei

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "vmName": {
            "type": "string",
            "metadata": {
                "description": "Name of the virtual machine."
            }
        },
        "associationName": {
            "type": "string",
            "metadata": {
                "description": "Name of the association."
            }
        },
        "dataCollectionRuleId": {
            "type": "string",
            "metadata": {
                "description": "Resource ID of the data collection rule."
            }
        }
    },
    "resources": [
        {
            "type": "Microsoft.HybridCompute/machines/providers/dataCollectionRuleAssociations",
            "name": "[concat(parameters('vmName'),'/microsoft.insights/', parameters('associationName'))]",
            "apiVersion": "2019-11-01-preview",
            "properties": {
                "description": "Association of data collection rule. Deleting this association will break the data collection for this Arc server.",
                "dataCollectionRuleId": "[parameters('dataCollectionRuleId')]"
            }
        }
    ]
}
```

### <a name="parameter-file"></a>Parameterdatei

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "vmName": {
        "value": "my-windows-vm"
      },
      "location": {
        "value": "eastus"
      }
  }
}
```


## <a name="next-steps"></a>Nächste Schritte

* [Rufen Sie weitere Beispielvorlagen für Azure Monitor ab](../resource-manager-samples.md).
* [Erfahren Sie mehr über den Log Analytics-Agent](./log-analytics-agent.md).
* [Erfahren Sie mehr über die Diagnoseerweiterung](./diagnostics-extension-overview.md).
