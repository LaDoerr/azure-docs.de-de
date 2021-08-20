---
title: Hinzufügen einer VM-Skalierungsgruppenerweiterung zu einem verwalteten Service Fabric-Clusterknotentyp
description: Im Folgenden wird beschrieben, wie Sie eine VM-Skalierungsgruppenerweiterung einem verwalteten Service Fabric-Clusterknotentyp hinzufügen.
ms.topic: article
ms.date: 5/10/2021
ms.openlocfilehash: 0c0aac8d7804caeb6c08657b1ed36c45958a0ea5
ms.sourcegitcommit: b044915306a6275c2211f143aa2daf9299d0c574
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2021
ms.locfileid: "113033693"
---
# <a name="add-a-virtual-machine-scale-set-extension-to-a-service-fabric-managed-cluster-node-type"></a>Hinzufügen einer VM-Skalierungsgruppenerweiterung zu einem verwalteten Service Fabric-Clusterknotentyp

Jeder Knotentyp in einem verwalteten Service Fabric-Cluster wird von einer VM-Skalierungsgruppe unterstützt. Im Folgenden wird beschrieben, wie Sie [VM-Skalierungsgruppenerweiterungen](../virtual-machines/extensions/overview.md) einem verwalteten Service Fabric-Clusterknotentyp hinzufügen.

Sie können eine VM-Skalierungsgruppenerweiterung einem Knotentyp mithilfe des PowerShell-Befehls [Add-AzServiceFabricManagedNodeTypeVMExtension](/powershell/module/az.servicefabric/add-azservicefabricmanagednodetypevmextension) hinzufügen.

Alternativ können Sie eine VM-Skalierungsgruppenerweiterung für einen verwalteten Service Fabric-Clusterknotentyp in Ihrer Azure Resource Manager-Vorlage hinzufügen. Beispiel:

```json
{
    "type": "Microsoft.ServiceFabric/managedclusters/nodetypes",
    "apiVersion": "[variables('sfApiVersion')]",
    "name": "[concat(parameters('clusterName'), '/', parameters('nodeTypeName'))]",
    "dependsOn": [
        "[concat('Microsoft.ServiceFabric/managedclusters/', parameters('clusterName'))]"
    ],
    "location": "[resourceGroup().location]",
    "properties": {
        "isPrimary": true,
        "vmInstanceCount": 3,
        "dataDiskSizeGB": 100,
        "vmSize": "Standard_D2",
        "vmImagePublisher": "MicrosoftWindowsServer",
        "vmImageOffer": "WindowsServer",
        "vmImageSku": "2019-Datacenter",
        "vmImageVersion": "latest",
        "vmExtensions": [{
            "name": "ExtensionA",
            "properties": {
                "publisher": "ExtensionA.Publisher",
                "type": "KeyVaultForWindows",
                "typeHandlerVersion": "1.0",
                "autoUpgradeMinorVersion": true,
                "settings": {
                }
            }
        }]
    }
}
```

Weitere Informationen zum Konfigurieren von verwalteten Service Fabric-Clusterknotentypen finden Sie unter [Verwalteter Clusterknotentyp](/azure/templates/microsoft.servicefabric/2020-01-01-preview/managedclusters/nodetypes).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu verwalteten Service Fabric-Clustern finden Sie in den folgenden Artikeln:

> [!div class="nextstepaction"]
> [Häufig gestellte Fragen zu verwalteten Service Fabric-Clustern](./faq-managed-cluster.yml)
