---
author: DCtheGeek
ms.service: resource-graph
ms.topic: include
ms.date: 07/21/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 1f6e286a6acf8183eb12354ea7930a9aedcbd748
ms.sourcegitcommit: 7d63ce88bfe8188b1ae70c3d006a29068d066287
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2021
ms.locfileid: "114456857"
---
### <a name="combine-results-from-two-queries-into-a-single-result"></a>Vereinen von Ergebnissen aus zwei Abfragen in einem einzigen Ergebnis

Die folgende Abfrage verwendet `union`, um Ergebnisse aus der Tabelle _ResourceContainers_  abzurufen und sie den Ergebnissen aus der Tabelle _Resources_  hinzuzufügen.

```kusto
ResourceContainers
| where type=='microsoft.resources/subscriptions/resourcegroups' | project name, type  | limit 5
| union  (Resources | project name, type | limit 5)
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az graph query -q "ResourceContainers | where type=='microsoft.resources/subscriptions/resourcegroups' | project name, type | limit 5 | union (Resources | project name, type | limit 5)"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "ResourceContainers | where type=='microsoft.resources/subscriptions/resourcegroups' | project name, type | limit 5 | union (Resources | project name, type | limit 5)"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

:::image type="icon" source="../../../../articles/governance/resource-graph/media/resource-graph-small.png":::Probieren Sie im Azure Resource Graph-Explorer die folgende Abfrage aus:

- Azure-Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/ResourceContainers%0a%7c%20where%20type%3d%3d%27microsoft.resources%2fsubscriptions%2fresourcegroups%27%20%7c%20project%20name%2c%20type%20%20%7c%20limit%205%0a%7c%20union%20%20(Resources%20%7c%20project%20name%2c%20type%20%7c%20limit%205)" target="_blank">portal.azure.com</a>
- Azure Government-Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/ResourceContainers%0a%7c%20where%20type%3d%3d%27microsoft.resources%2fsubscriptions%2fresourcegroups%27%20%7c%20project%20name%2c%20type%20%20%7c%20limit%205%0a%7c%20union%20%20(Resources%20%7c%20project%20name%2c%20type%20%7c%20limit%205)" target="_blank">portal.azure.us</a>
- Azure China 21Vianet-Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/ResourceContainers%0a%7c%20where%20type%3d%3d%27microsoft.resources%2fsubscriptions%2fresourcegroups%27%20%7c%20project%20name%2c%20type%20%20%7c%20limit%205%0a%7c%20union%20%20(Resources%20%7c%20project%20name%2c%20type%20%7c%20limit%205)" target="_blank">portal.azure.cn</a>

---

### <a name="count-azure-resources"></a>Zählen der Azure-Ressourcen

Diese Abfrage gibt die Anzahl der Azure-Ressourcen zurück, die in den Abonnements vorhanden sind, auf die Sie zugreifen können. Sie ist auch eine gute Abfrage, um sicherzustellen, dass für die Shell Ihrer Wahl die entsprechenden Azure Resource Graph-Komponenten installiert und in funktionsfähigem Zustand sind.

```kusto
Resources
| summarize count()
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | summarize count()"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | summarize count()"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

:::image type="icon" source="../../../../articles/governance/resource-graph/media/resource-graph-small.png":::Probieren Sie im Azure Resource Graph-Explorer die folgende Abfrage aus:

- Azure-Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20summarize%20count()" target="_blank">portal.azure.com</a>
- Azure Government-Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20summarize%20count()" target="_blank">portal.azure.us</a>
- Azure China 21Vianet-Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20summarize%20count()" target="_blank">portal.azure.cn</a>

---

### <a name="count-key-vault-resources"></a>Anzahl von Key Vault-Ressourcen

Diese Abfrage verwendet `count` anstelle von `summarize`, um die Anzahl der zurückgegebenen Datensätze zu zählen. In der Anzahl sind nur Schlüsseltresore enthalten.

```kusto
Resources
| where type =~ 'microsoft.keyvault/vaults'
| count
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type =~ 'microsoft.keyvault/vaults' | count"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'microsoft.keyvault/vaults' | count"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

:::image type="icon" source="../../../../articles/governance/resource-graph/media/resource-graph-small.png":::Probieren Sie im Azure Resource Graph-Explorer die folgende Abfrage aus:

- Azure-Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27microsoft.keyvault%2fvaults%27%0a%7c%20count" target="_blank">portal.azure.com</a>
- Azure Government-Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27microsoft.keyvault%2fvaults%27%0a%7c%20count" target="_blank">portal.azure.us</a>
- Azure China 21Vianet-Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27microsoft.keyvault%2fvaults%27%0a%7c%20count" target="_blank">portal.azure.cn</a>

---

### <a name="count-resources-that-have-ip-addresses-configured-by-subscription"></a>Zählen von Ressourcen, für die IP-Adressen konfiguriert sind (nach Abonnement)

Mithilfe der Beispielabfrage „Alle öffentlichen IP-Adressen auflisten“ und Hinzufügen von `summarize` und `count()` können wir eine Liste nach Abonnement von Ressourcen mit konfigurierten IP-Adressen abrufen.

```kusto
Resources
| where type contains 'publicIPAddresses' and isnotempty(properties.ipAddress)
| summarize count () by subscriptionId
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type contains 'publicIPAddresses' and isnotempty(properties.ipAddress) | summarize count () by subscriptionId"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type contains 'publicIPAddresses' and isnotempty(properties.ipAddress) | summarize count () by subscriptionId"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

:::image type="icon" source="../../../../articles/governance/resource-graph/media/resource-graph-small.png":::Probieren Sie im Azure Resource Graph-Explorer die folgende Abfrage aus:

- Azure-Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20contains%20%27publicIPAddresses%27%20and%20isnotempty(properties.ipAddress)%0a%7c%20summarize%20count%20()%20by%20subscriptionId" target="_blank">portal.azure.com</a>
- Azure Government-Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20contains%20%27publicIPAddresses%27%20and%20isnotempty(properties.ipAddress)%0a%7c%20summarize%20count%20()%20by%20subscriptionId" target="_blank">portal.azure.us</a>
- Azure China 21Vianet-Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20contains%20%27publicIPAddresses%27%20and%20isnotempty(properties.ipAddress)%0a%7c%20summarize%20count%20()%20by%20subscriptionId" target="_blank">portal.azure.cn</a>

---

### <a name="count-virtual-machines-by-os-type"></a>Zählen von virtuellen Computern nach Betriebssystemtyp

Auf der vorherigen Abfrage aufbauend schränken wir immer noch nach Azure-Ressourcen vom Typ `Microsoft.Compute/virtualMachines` ein, jedoch nicht mehr die Anzahl der zurückgegebenen Datensätze. Stattdessen haben wir `summarize` und `count()` verwendet, um festzulegen, wie Werte nach Eigenschaft gruppiert und aggregiert werden sollen, in diesem Beispiel `properties.storageProfile.osDisk.osType`. Ein Beispiel, wie diese Zeichenfolge im vollständigen Objekt aussieht, finden Sie unter [Untersuchen virtueller Computer – VM-Ermittlung](../../../../articles/governance/resource-graph/concepts/explore-resources.md#virtual-machine-discovery).

```kusto
Resources
| where type =~ 'Microsoft.Compute/virtualMachines'
| summarize count() by tostring(properties.storageProfile.osDisk.osType)
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | summarize count() by tostring(properties.storageProfile.osDisk.osType)"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | summarize count() by tostring(properties.storageProfile.osDisk.osType)"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

:::image type="icon" source="../../../../articles/governance/resource-graph/media/resource-graph-small.png":::Probieren Sie im Azure Resource Graph-Explorer die folgende Abfrage aus:

- Azure-Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27Microsoft.Compute%2fvirtualMachines%27%0a%7c%20summarize%20count()%20by%20tostring(properties.storageProfile.osDisk.osType)" target="_blank">portal.azure.com</a>
- Azure Government-Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27Microsoft.Compute%2fvirtualMachines%27%0a%7c%20summarize%20count()%20by%20tostring(properties.storageProfile.osDisk.osType)" target="_blank">portal.azure.us</a>
- Azure China 21Vianet-Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27Microsoft.Compute%2fvirtualMachines%27%0a%7c%20summarize%20count()%20by%20tostring(properties.storageProfile.osDisk.osType)" target="_blank">portal.azure.cn</a>

---

### <a name="count-virtual-machines-by-os-type-with-extend"></a>Anzahl von virtuellen Computern sortiert nach dem Betriebssystemtyp mit Erweiterungen

Eine andere Möglichkeit zum Schreiben der „Anzahl von virtuellen Computern sortiert nach dem Betriebssystemtyp“-Abfrage ist, eine Eigenschaft zu `extend` und ihr einen temporären Namen für die Verwendung in der Abfrage zu geben, in diesem Fall **OS**. **OS** wird dann wie im vorherigen Beispiel von `summarize` und `count()` verwendet.

```kusto
Resources
| where type =~ 'Microsoft.Compute/virtualMachines'
| extend os = properties.storageProfile.osDisk.osType
| summarize count() by tostring(os)
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | extend os = properties.storageProfile.osDisk.osType | summarize count() by tostring(os)"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | extend os = properties.storageProfile.osDisk.osType | summarize count() by tostring(os)"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

:::image type="icon" source="../../../../articles/governance/resource-graph/media/resource-graph-small.png":::Probieren Sie im Azure Resource Graph-Explorer die folgende Abfrage aus:

- Azure-Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27Microsoft.Compute%2fvirtualMachines%27%0a%7c%20extend%20os%20%3d%20properties.storageProfile.osDisk.osType%0a%7c%20summarize%20count()%20by%20tostring(os)" target="_blank">portal.azure.com</a>
- Azure Government-Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27Microsoft.Compute%2fvirtualMachines%27%0a%7c%20extend%20os%20%3d%20properties.storageProfile.osDisk.osType%0a%7c%20summarize%20count()%20by%20tostring(os)" target="_blank">portal.azure.us</a>
- Azure China 21Vianet-Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27Microsoft.Compute%2fvirtualMachines%27%0a%7c%20extend%20os%20%3d%20properties.storageProfile.osDisk.osType%0a%7c%20summarize%20count()%20by%20tostring(os)" target="_blank">portal.azure.cn</a>

---

### <a name="find-storage-accounts-with-a-specific-case-insensitive-tag-on-the-resource-group"></a>Suchen von Speicherkonten mit einem bestimmten Tag (unter Vernachlässigung der Groß-/Kleinschreibung) in der Ressourcengruppe

Ähnlich wie bei der Abfrage "Suchen von Speicherkonten mit einem bestimmten Tag mit Beachtung der Groß-/Kleinschreibung in der Ressourcengruppe", aber wenn nach einem Tagnamen und tag-Wert gesucht werden muss, bei dem die Groß-/Kleinschreibung nicht beachtet wird, verwenden Sie `mv-expand` mit dem Parameter **bagexpansion**. Bei dieser Abfrage wird ein höheres Kontingent als in der ursprünglichen Abfrage verwendet. Verwenden Sie `mv-expand` also nur, wenn dies unbedingt erforderlich ist.

```kusto
Resources
| where type =~ 'microsoft.storage/storageaccounts'
| join kind=inner (
    ResourceContainers
    | where type =~ 'microsoft.resources/subscriptions/resourcegroups'
    | mv-expand bagexpansion=array tags
    | where isnotempty(tags)
    | where tags[0] =~ 'key1' and tags[1] =~ 'value1'
    | project subscriptionId, resourceGroup)
on subscriptionId, resourceGroup
| project-away subscriptionId1, resourceGroup1
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type =~ 'microsoft.storage/storageaccounts' | join kind=inner ( ResourceContainers | where type =~ 'microsoft.resources/subscriptions/resourcegroups' | mv-expand bagexpansion=array tags | where isnotempty(tags) | where tags[0] =~ 'key1' and tags[1] =~ 'value1' | project subscriptionId, resourceGroup) on subscriptionId, resourceGroup | project-away subscriptionId1, resourceGroup1"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'microsoft.storage/storageaccounts' | join kind=inner ( ResourceContainers | where type =~ 'microsoft.resources/subscriptions/resourcegroups' | mv-expand bagexpansion=array tags | where isnotempty(tags) | where tags[0] =~ 'key1' and tags[1] =~ 'value1' | project subscriptionId, resourceGroup) on subscriptionId, resourceGroup | project-away subscriptionId1, resourceGroup1"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

:::image type="icon" source="../../../../articles/governance/resource-graph/media/resource-graph-small.png":::Probieren Sie im Azure Resource Graph-Explorer die folgende Abfrage aus:

- Azure-Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27microsoft.storage%2fstorageaccounts%27%0a%7c%20join%20kind%3dinner%20(%0a%09ResourceContainers%0a%09%7c%20where%20type%20%3d%7e%20%27microsoft.resources%2fsubscriptions%2fresourcegroups%27%0a%09%7c%20mv-expand%20bagexpansion%3darray%20tags%0a%09%7c%20where%20isnotempty(tags)%0a%09%7c%20where%20tags%5b0%5d%20%3d%7e%20%27key1%27%20and%20tags%5b1%5d%20%3d%7e%20%27value1%27%0a%09%7c%20project%20subscriptionId%2c%20resourceGroup)%0aon%20subscriptionId%2c%20resourceGroup%0a%7c%20project-away%20subscriptionId1%2c%20resourceGroup1" target="_blank">portal.azure.com</a>
- Azure Government-Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27microsoft.storage%2fstorageaccounts%27%0a%7c%20join%20kind%3dinner%20(%0a%09ResourceContainers%0a%09%7c%20where%20type%20%3d%7e%20%27microsoft.resources%2fsubscriptions%2fresourcegroups%27%0a%09%7c%20mv-expand%20bagexpansion%3darray%20tags%0a%09%7c%20where%20isnotempty(tags)%0a%09%7c%20where%20tags%5b0%5d%20%3d%7e%20%27key1%27%20and%20tags%5b1%5d%20%3d%7e%20%27value1%27%0a%09%7c%20project%20subscriptionId%2c%20resourceGroup)%0aon%20subscriptionId%2c%20resourceGroup%0a%7c%20project-away%20subscriptionId1%2c%20resourceGroup1" target="_blank">portal.azure.us</a>
- Azure China 21Vianet-Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27microsoft.storage%2fstorageaccounts%27%0a%7c%20join%20kind%3dinner%20(%0a%09ResourceContainers%0a%09%7c%20where%20type%20%3d%7e%20%27microsoft.resources%2fsubscriptions%2fresourcegroups%27%0a%09%7c%20mv-expand%20bagexpansion%3darray%20tags%0a%09%7c%20where%20isnotempty(tags)%0a%09%7c%20where%20tags%5b0%5d%20%3d%7e%20%27key1%27%20and%20tags%5b1%5d%20%3d%7e%20%27value1%27%0a%09%7c%20project%20subscriptionId%2c%20resourceGroup)%0aon%20subscriptionId%2c%20resourceGroup%0a%7c%20project-away%20subscriptionId1%2c%20resourceGroup1" target="_blank">portal.azure.cn</a>

---

### <a name="find-storage-accounts-with-a-specific-case-sensitive-tag-on-the-resource-group"></a>Suchen von Speicherkonten mit einem bestimmten Tag (unter Berücksichtigung der Groß-/Kleinschreibung) in der Ressourcengruppe

In der folgenden Abfrage wird **inner** `join` zum Verbinden von Speicherkonten mit Ressourcengruppen verwendet, die über einen Tagnamen und einen Tagwert verfügen, bei denen die Groß-/Kleinschreibung berücksichtigt wird.

```kusto
Resources
| where type =~ 'microsoft.storage/storageaccounts'
| join kind=inner (
    ResourceContainers
    | where type =~ 'microsoft.resources/subscriptions/resourcegroups'
    | where tags['Key1'] =~ 'Value1'
    | project subscriptionId, resourceGroup)
on subscriptionId, resourceGroup
| project-away subscriptionId1, resourceGroup1
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type =~ 'microsoft.storage/storageaccounts' | join kind=inner ( ResourceContainers | where type =~ 'microsoft.resources/subscriptions/resourcegroups' | where tags['Key1'] =~ 'Value1' | project subscriptionId, resourceGroup) on subscriptionId, resourceGroup | project-away subscriptionId1, resourceGroup1"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'microsoft.storage/storageaccounts' | join kind=inner ( ResourceContainers | where type =~ 'microsoft.resources/subscriptions/resourcegroups' | where tags['Key1'] =~ 'Value1' | project subscriptionId, resourceGroup) on subscriptionId, resourceGroup | project-away subscriptionId1, resourceGroup1"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

:::image type="icon" source="../../../../articles/governance/resource-graph/media/resource-graph-small.png":::Probieren Sie im Azure Resource Graph-Explorer die folgende Abfrage aus:

- Azure-Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27microsoft.storage%2fstorageaccounts%27%0a%7c%20join%20kind%3dinner%20(%0a%09ResourceContainers%0a%09%7c%20where%20type%20%3d%7e%20%27microsoft.resources%2fsubscriptions%2fresourcegroups%27%0a%09%7c%20where%20tags%5b%27Key1%27%5d%20%3d%7e%20%27Value1%27%0a%09%7c%20project%20subscriptionId%2c%20resourceGroup)%0aon%20subscriptionId%2c%20resourceGroup%0a%7c%20project-away%20subscriptionId1%2c%20resourceGroup1" target="_blank">portal.azure.com</a>
- Azure Government-Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27microsoft.storage%2fstorageaccounts%27%0a%7c%20join%20kind%3dinner%20(%0a%09ResourceContainers%0a%09%7c%20where%20type%20%3d%7e%20%27microsoft.resources%2fsubscriptions%2fresourcegroups%27%0a%09%7c%20where%20tags%5b%27Key1%27%5d%20%3d%7e%20%27Value1%27%0a%09%7c%20project%20subscriptionId%2c%20resourceGroup)%0aon%20subscriptionId%2c%20resourceGroup%0a%7c%20project-away%20subscriptionId1%2c%20resourceGroup1" target="_blank">portal.azure.us</a>
- Azure China 21Vianet-Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27microsoft.storage%2fstorageaccounts%27%0a%7c%20join%20kind%3dinner%20(%0a%09ResourceContainers%0a%09%7c%20where%20type%20%3d%7e%20%27microsoft.resources%2fsubscriptions%2fresourcegroups%27%0a%09%7c%20where%20tags%5b%27Key1%27%5d%20%3d%7e%20%27Value1%27%0a%09%7c%20project%20subscriptionId%2c%20resourceGroup)%0aon%20subscriptionId%2c%20resourceGroup%0a%7c%20project-away%20subscriptionId1%2c%20resourceGroup1" target="_blank">portal.azure.cn</a>

---

### <a name="get-virtual-machine-scale-set-capacity-and-size"></a>Abrufen der Kapazität und Größe von VM-Skalierungsgruppen

Diese Abfrage sucht nach Ressourcen für VM-Skalierungsgruppen und ruft verschiedene Details (einschließlich der Größe des virtuellen Computers und der Kapazität der Skalierungsgruppe) ab. Mit dieser Abfrage wandelt die `toint()`-Funktion die Kapazität in eine Zahl um, damit sie sortiert werden kann. Schließlich werden die Spalten in benutzerdefinierte benannte Eigenschaften umbenannt.

```kusto
Resources
| where type=~ 'microsoft.compute/virtualmachinescalesets'
| where name contains 'contoso'
| project subscriptionId, name, location, resourceGroup, Capacity = toint(sku.capacity), Tier = sku.name
| order by Capacity desc
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type=~ 'microsoft.compute/virtualmachinescalesets' | where name contains 'contoso' | project subscriptionId, name, location, resourceGroup, Capacity = toint(sku.capacity), Tier = sku.name | order by Capacity desc"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type=~ 'microsoft.compute/virtualmachinescalesets' | where name contains 'contoso' | project subscriptionId, name, location, resourceGroup, Capacity = toint(sku.capacity), Tier = sku.name | order by Capacity desc"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

:::image type="icon" source="../../../../articles/governance/resource-graph/media/resource-graph-small.png":::Probieren Sie im Azure Resource Graph-Explorer die folgende Abfrage aus:

- Azure-Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%3d%7e%20%27microsoft.compute%2fvirtualmachinescalesets%27%0a%7c%20where%20name%20contains%20%27contoso%27%0a%7c%20project%20subscriptionId%2c%20name%2c%20location%2c%20resourceGroup%2c%20Capacity%20%3d%20toint(sku.capacity)%2c%20Tier%20%3d%20sku.name%0a%7c%20order%20by%20Capacity%20desc" target="_blank">portal.azure.com</a>
- Azure Government-Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%3d%7e%20%27microsoft.compute%2fvirtualmachinescalesets%27%0a%7c%20where%20name%20contains%20%27contoso%27%0a%7c%20project%20subscriptionId%2c%20name%2c%20location%2c%20resourceGroup%2c%20Capacity%20%3d%20toint(sku.capacity)%2c%20Tier%20%3d%20sku.name%0a%7c%20order%20by%20Capacity%20desc" target="_blank">portal.azure.us</a>
- Azure China 21Vianet-Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%3d%7e%20%27microsoft.compute%2fvirtualmachinescalesets%27%0a%7c%20where%20name%20contains%20%27contoso%27%0a%7c%20project%20subscriptionId%2c%20name%2c%20location%2c%20resourceGroup%2c%20Capacity%20%3d%20toint(sku.capacity)%2c%20Tier%20%3d%20sku.name%0a%7c%20order%20by%20Capacity%20desc" target="_blank">portal.azure.cn</a>

---

### <a name="get-virtual-networks-and-subnets-of-network-interfaces"></a>Abrufen von virtuellen Netzwerken und Subnetzen von Netzwerkschnittstellen

Verwenden Sie einen regulären `parse`-Ausdruck, um die Namen des virtuellen Netzwerks und des Subnetzes aus der Ressourcen-ID-Eigenschaft zu erhalten. Während `parse` das Abrufen von Daten aus einem komplexen Feld ermöglicht, ist es besser, direkt auf die vorhandenen Eigenschaften zuzugreifen, anstatt `parse` zu verwenden.

```kusto
Resources
| where type =~ 'microsoft.network/networkinterfaces'
| project id, ipConfigurations = properties.ipConfigurations
| mvexpand ipConfigurations
| project id, subnetId = tostring(ipConfigurations.properties.subnet.id)
| parse kind=regex subnetId with '/virtualNetworks/' virtualNetwork '/subnets/' subnet
| project id, virtualNetwork, subnet
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type =~ 'microsoft.network/networkinterfaces' | project id, ipConfigurations = properties.ipConfigurations | mvexpand ipConfigurations | project id, subnetId = tostring(ipConfigurations.properties.subnet.id) | parse kind=regex subnetId with '/virtualNetworks/' virtualNetwork '/subnets/' subnet | project id, virtualNetwork, subnet"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'microsoft.network/networkinterfaces' | project id, ipConfigurations = properties.ipConfigurations | mvexpand ipConfigurations | project id, subnetId = tostring(ipConfigurations.properties.subnet.id) | parse kind=regex subnetId with '/virtualNetworks/' virtualNetwork '/subnets/' subnet | project id, virtualNetwork, subnet"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

:::image type="icon" source="../../../../articles/governance/resource-graph/media/resource-graph-small.png":::Probieren Sie im Azure Resource Graph-Explorer die folgende Abfrage aus:

- Azure-Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27microsoft.network%2fnetworkinterfaces%27%0a%7c%20project%20id%2c%20ipConfigurations%20%3d%20properties.ipConfigurations%0a%7c%20mvexpand%20ipConfigurations%0a%7c%20project%20id%2c%20subnetId%20%3d%20tostring(ipConfigurations.properties.subnet.id)%0a%7c%20parse%20kind%3dregex%20subnetId%20with%20%27%2fvirtualNetworks%2f%27%20virtualNetwork%20%27%2fsubnets%2f%27%20subnet%0a%7c%20project%20id%2c%20virtualNetwork%2c%20subnet" target="_blank">portal.azure.com</a>
- Azure Government-Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27microsoft.network%2fnetworkinterfaces%27%0a%7c%20project%20id%2c%20ipConfigurations%20%3d%20properties.ipConfigurations%0a%7c%20mvexpand%20ipConfigurations%0a%7c%20project%20id%2c%20subnetId%20%3d%20tostring(ipConfigurations.properties.subnet.id)%0a%7c%20parse%20kind%3dregex%20subnetId%20with%20%27%2fvirtualNetworks%2f%27%20virtualNetwork%20%27%2fsubnets%2f%27%20subnet%0a%7c%20project%20id%2c%20virtualNetwork%2c%20subnet" target="_blank">portal.azure.us</a>
- Azure China 21Vianet-Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27microsoft.network%2fnetworkinterfaces%27%0a%7c%20project%20id%2c%20ipConfigurations%20%3d%20properties.ipConfigurations%0a%7c%20mvexpand%20ipConfigurations%0a%7c%20project%20id%2c%20subnetId%20%3d%20tostring(ipConfigurations.properties.subnet.id)%0a%7c%20parse%20kind%3dregex%20subnetId%20with%20%27%2fvirtualNetworks%2f%27%20virtualNetwork%20%27%2fsubnets%2f%27%20subnet%0a%7c%20project%20id%2c%20virtualNetwork%2c%20subnet" target="_blank">portal.azure.cn</a>

---

### <a name="key-vaults-with-subscription-name"></a>Schlüsseltresore mit Abonnementname

Die folgende Abfrage zeigt eine komplexe Verwendung von `join` mit **kind** als _leftouter_. Die Abfrage schränkt die verknüpfte Tabelle auf Abonnementressourcen ein, und mit `project` wird nur das ursprüngliche Feld _subscriptionId_ einbezogen, und das Feld _name_ wird in _SubName_ umbenannt. Beim Umbenennen des Felds wird das Hinzufügen von `join` als _name1_ vermieden, da das Feld bereits unter _resources_ vorhanden ist. Die ursprüngliche Tabelle wird mit `where` gefiltert, und das folgende `project`-Element enthält Spalten aus beiden Tabellen. Das Abfrageergebnis enthält alle Schlüsseltresore mit Typ, Name des Schlüsseltresors und Name des Abonnements, in dem er sich befindet.

```kusto
Resources
| join kind=leftouter (ResourceContainers | where type=='microsoft.resources/subscriptions' | project SubName=name, subscriptionId) on subscriptionId
| where type == 'microsoft.keyvault/vaults'
| project type, name, SubName
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | join kind=leftouter (ResourceContainers | where type=='microsoft.resources/subscriptions' | project SubName=name, subscriptionId) on subscriptionId | where type == 'microsoft.keyvault/vaults' | project type, name, SubName"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | join kind=leftouter (ResourceContainers | where type=='microsoft.resources/subscriptions' | project SubName=name, subscriptionId) on subscriptionId | where type == 'microsoft.keyvault/vaults' | project type, name, SubName"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

:::image type="icon" source="../../../../articles/governance/resource-graph/media/resource-graph-small.png":::Probieren Sie im Azure Resource Graph-Explorer die folgende Abfrage aus:

- Azure-Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20join%20kind%3dleftouter%20(ResourceContainers%20%7c%20where%20type%3d%3d%27microsoft.resources%2fsubscriptions%27%20%7c%20project%20SubName%3dname%2c%20subscriptionId)%20on%20subscriptionId%0a%7c%20where%20type%20%3d%3d%20%27microsoft.keyvault%2fvaults%27%0a%7c%20project%20type%2c%20name%2c%20SubName" target="_blank">portal.azure.com</a>
- Azure Government-Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20join%20kind%3dleftouter%20(ResourceContainers%20%7c%20where%20type%3d%3d%27microsoft.resources%2fsubscriptions%27%20%7c%20project%20SubName%3dname%2c%20subscriptionId)%20on%20subscriptionId%0a%7c%20where%20type%20%3d%3d%20%27microsoft.keyvault%2fvaults%27%0a%7c%20project%20type%2c%20name%2c%20SubName" target="_blank">portal.azure.us</a>
- Azure China 21Vianet-Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20join%20kind%3dleftouter%20(ResourceContainers%20%7c%20where%20type%3d%3d%27microsoft.resources%2fsubscriptions%27%20%7c%20project%20SubName%3dname%2c%20subscriptionId)%20on%20subscriptionId%0a%7c%20where%20type%20%3d%3d%20%27microsoft.keyvault%2fvaults%27%0a%7c%20project%20type%2c%20name%2c%20SubName" target="_blank">portal.azure.cn</a>

---

### <a name="list-all-azure-arc-enabled-kubernetes-clusters-without-azure-monitor-extension"></a>Auflisten aller Azure Arc-fähigen Kubernetes-Cluster ohne Azure Monitor-Erweiterung

Gibt die verbundene Cluster-ID jedes Azure Arc-fähigen Kubernetes-Clusters zurück, in dem die Azure Monitor-Erweiterung nicht vorhanden ist.

```kusto
Resources
| where type =~ 'Microsoft.Kubernetes/connectedClusters' | extend connectedClusterId = tolower(id) | project connectedClusterId 
| join kind = leftouter
    (KubernetesConfigurationResources
    | where type == 'microsoft.kubernetesconfiguration/extensions'
    | where properties.ExtensionType  == 'microsoft.azuremonitor.containers'
    | parse tolower(id) with connectedClusterId '/providers/microsoft.kubernetesconfiguration/extensions' *
    | project connectedClusterId
)  on connectedClusterId
| where connectedClusterId1 == ''
| project connectedClusterId
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type =~ 'Microsoft.Kubernetes/connectedClusters' | extend connectedClusterId = tolower(id) | project connectedClusterId | join kind = leftouter (KubernetesConfigurationResources | where type == 'microsoft.kubernetesconfiguration/extensions' | where properties.ExtensionType == 'microsoft.azuremonitor.containers' | parse tolower(id) with connectedClusterId '/providers/microsoft.kubernetesconfiguration/extensions' * | project connectedClusterId ) on connectedClusterId | where connectedClusterId1 == '' | project connectedClusterId"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Kubernetes/connectedClusters' | extend connectedClusterId = tolower(id) | project connectedClusterId | join kind = leftouter (KubernetesConfigurationResources | where type == 'microsoft.kubernetesconfiguration/extensions' | where properties.ExtensionType == 'microsoft.azuremonitor.containers' | parse tolower(id) with connectedClusterId '/providers/microsoft.kubernetesconfiguration/extensions' * | project connectedClusterId ) on connectedClusterId | where connectedClusterId1 == '' | project connectedClusterId"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

:::image type="icon" source="../../../../articles/governance/resource-graph/media/resource-graph-small.png":::Probieren Sie im Azure Resource Graph-Explorer die folgende Abfrage aus:

- Azure-Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27Microsoft.Kubernetes%2fconnectedClusters%27%20%7c%20extend%20connectedClusterId%20%3d%20tolower(id)%20%7c%20project%20connectedClusterId%20%0a%7c%20join%20kind%20%3d%20leftouter%0a%09(KubernetesConfigurationResources%0a%09%7c%20where%20type%20%3d%3d%20%27microsoft.kubernetesconfiguration%2fextensions%27%0a%09%7c%20where%20properties.ExtensionType%20%20%3d%3d%20%27microsoft.azuremonitor.containers%27%0a%09%7c%20parse%20tolower(id)%20with%20connectedClusterId%20%27%2fproviders%2fmicrosoft.kubernetesconfiguration%2fextensions%27%20*%0a%09%7c%20project%20connectedClusterId%0a)%20%20on%20connectedClusterId%0a%7c%20where%20connectedClusterId1%20%3d%3d%20%27%27%0a%7c%20project%20connectedClusterId" target="_blank">portal.azure.com</a>
- Azure Government-Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27Microsoft.Kubernetes%2fconnectedClusters%27%20%7c%20extend%20connectedClusterId%20%3d%20tolower(id)%20%7c%20project%20connectedClusterId%20%0a%7c%20join%20kind%20%3d%20leftouter%0a%09(KubernetesConfigurationResources%0a%09%7c%20where%20type%20%3d%3d%20%27microsoft.kubernetesconfiguration%2fextensions%27%0a%09%7c%20where%20properties.ExtensionType%20%20%3d%3d%20%27microsoft.azuremonitor.containers%27%0a%09%7c%20parse%20tolower(id)%20with%20connectedClusterId%20%27%2fproviders%2fmicrosoft.kubernetesconfiguration%2fextensions%27%20*%0a%09%7c%20project%20connectedClusterId%0a)%20%20on%20connectedClusterId%0a%7c%20where%20connectedClusterId1%20%3d%3d%20%27%27%0a%7c%20project%20connectedClusterId" target="_blank">portal.azure.us</a>
- Azure China 21Vianet-Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27Microsoft.Kubernetes%2fconnectedClusters%27%20%7c%20extend%20connectedClusterId%20%3d%20tolower(id)%20%7c%20project%20connectedClusterId%20%0a%7c%20join%20kind%20%3d%20leftouter%0a%09(KubernetesConfigurationResources%0a%09%7c%20where%20type%20%3d%3d%20%27microsoft.kubernetesconfiguration%2fextensions%27%0a%09%7c%20where%20properties.ExtensionType%20%20%3d%3d%20%27microsoft.azuremonitor.containers%27%0a%09%7c%20parse%20tolower(id)%20with%20connectedClusterId%20%27%2fproviders%2fmicrosoft.kubernetesconfiguration%2fextensions%27%20*%0a%09%7c%20project%20connectedClusterId%0a)%20%20on%20connectedClusterId%0a%7c%20where%20connectedClusterId1%20%3d%3d%20%27%27%0a%7c%20project%20connectedClusterId" target="_blank">portal.azure.cn</a>

---

### <a name="list-all-azure-arc-enabled-kubernetes-resources"></a>Auflisten aller Azure Arc-fähigen Kubernetes-Ressourcen

Gibt eine Liste der einzelnen Azure Arc-fähigen Kubernetes-Cluster sowie relevante Metadaten zu den einzelnen Clustern zurück.

```kusto
Resources
| project id, subscriptionId, location, type, properties.agentVersion, properties.kubernetesVersion, properties.distribution, properties.infrastructure, properties.totalNodeCount, properties.totalCoreCount
| where type =~ 'Microsoft.Kubernetes/connectedClusters'
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | project id, subscriptionId, location, type, properties.agentVersion, properties.kubernetesVersion, properties.distribution, properties.infrastructure, properties.totalNodeCount, properties.totalCoreCount | where type =~ 'Microsoft.Kubernetes/connectedClusters'"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | project id, subscriptionId, location, type, properties.agentVersion, properties.kubernetesVersion, properties.distribution, properties.infrastructure, properties.totalNodeCount, properties.totalCoreCount | where type =~ 'Microsoft.Kubernetes/connectedClusters'"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

:::image type="icon" source="../../../../articles/governance/resource-graph/media/resource-graph-small.png":::Probieren Sie im Azure Resource Graph-Explorer die folgende Abfrage aus:

- Azure-Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20project%20id%2c%20subscriptionId%2c%20location%2c%20type%2c%20properties.agentVersion%2c%20properties.kubernetesVersion%2c%20properties.distribution%2c%20properties.infrastructure%2c%20properties.totalNodeCount%2c%20properties.totalCoreCount%0a%7c%20where%20type%20%3d%7e%20%27Microsoft.Kubernetes%2fconnectedClusters%27" target="_blank">portal.azure.com</a>
- Azure Government-Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20project%20id%2c%20subscriptionId%2c%20location%2c%20type%2c%20properties.agentVersion%2c%20properties.kubernetesVersion%2c%20properties.distribution%2c%20properties.infrastructure%2c%20properties.totalNodeCount%2c%20properties.totalCoreCount%0a%7c%20where%20type%20%3d%7e%20%27Microsoft.Kubernetes%2fconnectedClusters%27" target="_blank">portal.azure.us</a>
- Azure China 21Vianet-Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20project%20id%2c%20subscriptionId%2c%20location%2c%20type%2c%20properties.agentVersion%2c%20properties.kubernetesVersion%2c%20properties.distribution%2c%20properties.infrastructure%2c%20properties.totalNodeCount%2c%20properties.totalCoreCount%0a%7c%20where%20type%20%3d%7e%20%27Microsoft.Kubernetes%2fconnectedClusters%27" target="_blank">portal.azure.cn</a>

---

### <a name="list-all-extensions-installed-on-a-virtual-machine"></a>Auflisten aller Erweiterungen, die auf einem virtuellen Computer installiert sind

Zuerst wird bei dieser Abfrage `extend` für den VM-Ressourcentyp verwendet, um die ID in Großbuchstaben (`toupper()`) und dann den Namen und Typ des Betriebssystems sowie die VM-Größe abzurufen. Das Abrufen der Ressourcen-ID in Großbuchstaben ist eine gute Möglichkeit, um das Einbinden in eine andere Eigenschaft vorzubereiten. Anschließend wird für die Abfrage `join` mit **kind** als _leftouter_-Element verwendet, um VM-Erweiterungen abzurufen. Hierfür wird ein Abgleich mit dem `substring`-Element der Erweiterungs-ID in Großbuchstaben durchgeführt. Da der Teil der ID vor „/extensions/\<ExtensionName\>“ das gleiche Format wie die VM-ID hat, verwenden wie diese Eigenschaft für den `join`-Vorgang. `summarize` wird dann mit `make_list` im Namen der VM-Erweiterung verwendet, um die Namen der einzelnen Erweiterungen zu kombinieren. Hierbei sind _id_, _OSName_, _OSType_ und _VMSize_ für jede Arrayeigenschaft jeweils identisch. Abschließend führen wir `order by` für das _OSName_-Element in Kleinbuchstaben mit **asc** durch. Standardmäßig wird für `order by` „descending“ (absteigend) und nicht „ascending“ (aufsteigend) verwendet.

```kusto
Resources
| where type == 'microsoft.compute/virtualmachines'
| extend
    JoinID = toupper(id),
    OSName = tostring(properties.osProfile.computerName),
    OSType = tostring(properties.storageProfile.osDisk.osType),
    VMSize = tostring(properties.hardwareProfile.vmSize)
| join kind=leftouter(
    Resources
    | where type == 'microsoft.compute/virtualmachines/extensions'
    | extend 
        VMId = toupper(substring(id, 0, indexof(id, '/extensions'))),
        ExtensionName = name
) on $left.JoinID == $right.VMId
| summarize Extensions = make_list(ExtensionName) by id, OSName, OSType, VMSize
| order by tolower(OSName) asc
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type == 'microsoft.compute/virtualmachines' | extend JoinID = toupper(id), OSName = tostring(properties.osProfile.computerName), OSType = tostring(properties.storageProfile.osDisk.osType), VMSize = tostring(properties.hardwareProfile.vmSize) | join kind=leftouter( Resources | where type == 'microsoft.compute/virtualmachines/extensions' | extend  VMId = toupper(substring(id, 0, indexof(id, '/extensions'))),  ExtensionName = name ) on $left.JoinID == $right.VMId | summarize Extensions = make_list(ExtensionName) by id, OSName, OSType, VMSize | order by tolower(OSName) asc"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type == 'microsoft.compute/virtualmachines' | extend JoinID = toupper(id), OSName = tostring(properties.osProfile.computerName), OSType = tostring(properties.storageProfile.osDisk.osType), VMSize = tostring(properties.hardwareProfile.vmSize) | join kind=leftouter( Resources | where type == 'microsoft.compute/virtualmachines/extensions' | extend  VMId = toupper(substring(id, 0, indexof(id, '/extensions'))),  ExtensionName = name ) on $left.JoinID == $right.VMId | summarize Extensions = make_list(ExtensionName) by id, OSName, OSType, VMSize | order by tolower(OSName) asc"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

:::image type="icon" source="../../../../articles/governance/resource-graph/media/resource-graph-small.png":::Probieren Sie im Azure Resource Graph-Explorer die folgende Abfrage aus:

- Azure-Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%3d%20%27microsoft.compute%2fvirtualmachines%27%0a%7c%20extend%0a%09JoinID%20%3d%20toupper(id)%2c%0a%09OSName%20%3d%20tostring(properties.osProfile.computerName)%2c%0a%09OSType%20%3d%20tostring(properties.storageProfile.osDisk.osType)%2c%0a%09VMSize%20%3d%20tostring(properties.hardwareProfile.vmSize)%0a%7c%20join%20kind%3dleftouter(%0a%09Resources%0a%09%7c%20where%20type%20%3d%3d%20%27microsoft.compute%2fvirtualmachines%2fextensions%27%0a%09%7c%20extend%20%0a%09%09VMId%20%3d%20toupper(substring(id%2c%200%2c%20indexof(id%2c%20%27%2fextensions%27)))%2c%0a%09%09ExtensionName%20%3d%20name%0a)%20on%20%24left.JoinID%20%3d%3d%20%24right.VMId%0a%7c%20summarize%20Extensions%20%3d%20make_list(ExtensionName)%20by%20id%2c%20OSName%2c%20OSType%2c%20VMSize%0a%7c%20order%20by%20tolower(OSName)%20asc" target="_blank">portal.azure.com</a>
- Azure Government-Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%3d%20%27microsoft.compute%2fvirtualmachines%27%0a%7c%20extend%0a%09JoinID%20%3d%20toupper(id)%2c%0a%09OSName%20%3d%20tostring(properties.osProfile.computerName)%2c%0a%09OSType%20%3d%20tostring(properties.storageProfile.osDisk.osType)%2c%0a%09VMSize%20%3d%20tostring(properties.hardwareProfile.vmSize)%0a%7c%20join%20kind%3dleftouter(%0a%09Resources%0a%09%7c%20where%20type%20%3d%3d%20%27microsoft.compute%2fvirtualmachines%2fextensions%27%0a%09%7c%20extend%20%0a%09%09VMId%20%3d%20toupper(substring(id%2c%200%2c%20indexof(id%2c%20%27%2fextensions%27)))%2c%0a%09%09ExtensionName%20%3d%20name%0a)%20on%20%24left.JoinID%20%3d%3d%20%24right.VMId%0a%7c%20summarize%20Extensions%20%3d%20make_list(ExtensionName)%20by%20id%2c%20OSName%2c%20OSType%2c%20VMSize%0a%7c%20order%20by%20tolower(OSName)%20asc" target="_blank">portal.azure.us</a>
- Azure China 21Vianet-Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%3d%20%27microsoft.compute%2fvirtualmachines%27%0a%7c%20extend%0a%09JoinID%20%3d%20toupper(id)%2c%0a%09OSName%20%3d%20tostring(properties.osProfile.computerName)%2c%0a%09OSType%20%3d%20tostring(properties.storageProfile.osDisk.osType)%2c%0a%09VMSize%20%3d%20tostring(properties.hardwareProfile.vmSize)%0a%7c%20join%20kind%3dleftouter(%0a%09Resources%0a%09%7c%20where%20type%20%3d%3d%20%27microsoft.compute%2fvirtualmachines%2fextensions%27%0a%09%7c%20extend%20%0a%09%09VMId%20%3d%20toupper(substring(id%2c%200%2c%20indexof(id%2c%20%27%2fextensions%27)))%2c%0a%09%09ExtensionName%20%3d%20name%0a)%20on%20%24left.JoinID%20%3d%3d%20%24right.VMId%0a%7c%20summarize%20Extensions%20%3d%20make_list(ExtensionName)%20by%20id%2c%20OSName%2c%20OSType%2c%20VMSize%0a%7c%20order%20by%20tolower(OSName)%20asc" target="_blank">portal.azure.cn</a>

---

### <a name="list-all-public-ip-addresses"></a>Auflisten aller öffentlichen IP-Adressen

Ähnlich wie bei der Abfrage „Anzeigen von Ressourcen mit Speicher“ wird jeder Typ mit dem Wort **publicIPAddresses** gefunden. Diese Abfrage baut auf diesem Muster auf, um nur Ergebnisse mit **properties.ipAddress** `isnotempty` einzuschließen, um nur die **properties.ipAddress** zurückzugeben und die Ergebnisse auf die obersten 100 zu begrenzen (`limit`). Je nach Ihrer ausgewählten Shell müssen Sie Anführungszeichen möglicherweise mit Escapezeichen versehen.

```kusto
Resources
| where type contains 'publicIPAddresses' and isnotempty(properties.ipAddress)
| project properties.ipAddress
| limit 100
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type contains 'publicIPAddresses' and isnotempty(properties.ipAddress) | project properties.ipAddress | limit 100"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type contains 'publicIPAddresses' and isnotempty(properties.ipAddress) | project properties.ipAddress | limit 100"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

:::image type="icon" source="../../../../articles/governance/resource-graph/media/resource-graph-small.png":::Probieren Sie im Azure Resource Graph-Explorer die folgende Abfrage aus:

- Azure-Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20contains%20%27publicIPAddresses%27%20and%20isnotempty(properties.ipAddress)%0a%7c%20project%20properties.ipAddress%0a%7c%20limit%20100" target="_blank">portal.azure.com</a>
- Azure Government-Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20contains%20%27publicIPAddresses%27%20and%20isnotempty(properties.ipAddress)%0a%7c%20project%20properties.ipAddress%0a%7c%20limit%20100" target="_blank">portal.azure.us</a>
- Azure China 21Vianet-Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20contains%20%27publicIPAddresses%27%20and%20isnotempty(properties.ipAddress)%0a%7c%20project%20properties.ipAddress%0a%7c%20limit%20100" target="_blank">portal.azure.cn</a>

---

### <a name="list-all-storage-accounts-with-specific-tag-value"></a>Auflisten aller Speicherkonten mit einem bestimmten Tagwert

Kombinieren Sie die Filterfunktionen des vorherigen Beispiels, und filtern Sie den Azure-Ressourcentyp nach der Eigenschaft **type**. Diese Abfrage schränkt auch unsere Suche nach spezifischen Azure-Ressourcentypen mit einem bestimmten Tagnamen und -wert ein.

```kusto
Resources
| where type =~ 'Microsoft.Storage/storageAccounts'
| where tags['tag with a space']=='Custom value'
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type =~ 'Microsoft.Storage/storageAccounts' | where tags['tag with a space']=='Custom value'"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Storage/storageAccounts' | where tags['tag with a space']=='Custom value'"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

:::image type="icon" source="../../../../articles/governance/resource-graph/media/resource-graph-small.png":::Probieren Sie im Azure Resource Graph-Explorer die folgende Abfrage aus:

- Azure-Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27Microsoft.Storage%2fstorageAccounts%27%0a%7c%20where%20tags%5b%27tag%20with%20a%20space%27%5d%3d%3d%27Custom%20value%27" target="_blank">portal.azure.com</a>
- Azure Government-Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27Microsoft.Storage%2fstorageAccounts%27%0a%7c%20where%20tags%5b%27tag%20with%20a%20space%27%5d%3d%3d%27Custom%20value%27" target="_blank">portal.azure.us</a>
- Azure China 21Vianet-Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27Microsoft.Storage%2fstorageAccounts%27%0a%7c%20where%20tags%5b%27tag%20with%20a%20space%27%5d%3d%3d%27Custom%20value%27" target="_blank">portal.azure.cn</a>

---

### <a name="list-all-tag-names"></a>Auflisten aller Tagnamen

Diese Abfrage beginnt mit dem Tag und erstellt ein JSON-Objekt, das alle eindeutigen Tagnamen und ihre entsprechenden Typen auflistet.

```kusto
Resources
| project tags
| summarize buildschema(tags)
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | project tags | summarize buildschema(tags)"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | project tags | summarize buildschema(tags)"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

:::image type="icon" source="../../../../articles/governance/resource-graph/media/resource-graph-small.png":::Probieren Sie im Azure Resource Graph-Explorer die folgende Abfrage aus:

- Azure-Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20project%20tags%0a%7c%20summarize%20buildschema(tags)" target="_blank">portal.azure.com</a>
- Azure Government-Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20project%20tags%0a%7c%20summarize%20buildschema(tags)" target="_blank">portal.azure.us</a>
- Azure China 21Vianet-Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20project%20tags%0a%7c%20summarize%20buildschema(tags)" target="_blank">portal.azure.cn</a>

---

### <a name="list-all-tags-and-their-values"></a>Auflisten aller Tags und der zugehörigen Werte

Diese Abfrage listet Tags in Verwaltungsgruppen, Abonnements und Ressourcen sowie die zugehörigen Werte auf. Die Abfrage wird zunächst auf Ressourcen mit nicht leeren Tags (`isnotempty()`) beschränkt. Außerdem werden die einbezogenen Felder beschränkt, indem nur _tags_ in `project` und `mvexpand` eingeschlossen wird, und es wird `extend` verwendet, um die Datenpaare aus dem Eigenschaftenbehälter abzurufen. Anschließend wird `union` verwendet, um die Ergebnisse aus _ResourceContainers_ mit den gleichen Ergebnissen aus _Resources_ zu kombinieren, wodurch eine umfassende Abdeckung der abgerufenen Tags erzielt wird. Abschließend werden die Ergebnisse auf individuelle (`distinct`) Datenpaare beschränkt, und vom System ausgeblendete Tags werden ausgeschlossen.

```kusto
ResourceContainers
| where isnotempty(tags)
| project tags
| mvexpand tags
| extend tagKey = tostring(bag_keys(tags)[0])
| extend tagValue = tostring(tags[tagKey])
| union (
    resources
    | where isnotempty(tags)
    | project tags
    | mvexpand tags
    | extend tagKey = tostring(bag_keys(tags)[0])
    | extend tagValue = tostring(tags[tagKey])
)
| distinct tagKey, tagValue
| where tagKey !startswith "hidden-"
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az graph query -q "ResourceContainers | where isnotempty(tags) | project tags | mvexpand tags | extend tagKey = tostring(bag_keys(tags)[0]) | extend tagValue = tostring(tags[tagKey]) | union ( resources | where isnotempty(tags) | project tags | mvexpand tags | extend tagKey = tostring(bag_keys(tags)[0]) | extend tagValue = tostring(tags[tagKey]) ) | distinct tagKey, tagValue | where tagKey !startswith "hidden-""
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "ResourceContainers | where isnotempty(tags) | project tags | mvexpand tags | extend tagKey = tostring(bag_keys(tags)[0]) | extend tagValue = tostring(tags[tagKey]) | union ( resources | where isnotempty(tags) | project tags | mvexpand tags | extend tagKey = tostring(bag_keys(tags)[0]) | extend tagValue = tostring(tags[tagKey]) ) | distinct tagKey, tagValue | where tagKey !startswith "hidden-""
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

:::image type="icon" source="../../../../articles/governance/resource-graph/media/resource-graph-small.png":::Probieren Sie im Azure Resource Graph-Explorer die folgende Abfrage aus:

- Azure-Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/ResourceContainers%0a%7c%20where%20isnotempty(tags)%0a%7c%20project%20tags%0a%7c%20mvexpand%20tags%0a%7c%20extend%20tagKey%20%3d%20tostring(bag_keys(tags)%5b0%5d)%0a%7c%20extend%20tagValue%20%3d%20tostring(tags%5btagKey%5d)%0a%7c%20union%20(%0a%09resources%0a%09%7c%20where%20isnotempty(tags)%0a%09%7c%20project%20tags%0a%09%7c%20mvexpand%20tags%0a%09%7c%20extend%20tagKey%20%3d%20tostring(bag_keys(tags)%5b0%5d)%0a%09%7c%20extend%20tagValue%20%3d%20tostring(tags%5btagKey%5d)%0a)%0a%7c%20distinct%20tagKey%2c%20tagValue%0a%7c%20where%20tagKey%20!startswith%20%22hidden-%22" target="_blank">portal.azure.com</a>
- Azure Government-Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/ResourceContainers%0a%7c%20where%20isnotempty(tags)%0a%7c%20project%20tags%0a%7c%20mvexpand%20tags%0a%7c%20extend%20tagKey%20%3d%20tostring(bag_keys(tags)%5b0%5d)%0a%7c%20extend%20tagValue%20%3d%20tostring(tags%5btagKey%5d)%0a%7c%20union%20(%0a%09resources%0a%09%7c%20where%20isnotempty(tags)%0a%09%7c%20project%20tags%0a%09%7c%20mvexpand%20tags%0a%09%7c%20extend%20tagKey%20%3d%20tostring(bag_keys(tags)%5b0%5d)%0a%09%7c%20extend%20tagValue%20%3d%20tostring(tags%5btagKey%5d)%0a)%0a%7c%20distinct%20tagKey%2c%20tagValue%0a%7c%20where%20tagKey%20!startswith%20%22hidden-%22" target="_blank">portal.azure.us</a>
- Azure China 21Vianet-Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/ResourceContainers%0a%7c%20where%20isnotempty(tags)%0a%7c%20project%20tags%0a%7c%20mvexpand%20tags%0a%7c%20extend%20tagKey%20%3d%20tostring(bag_keys(tags)%5b0%5d)%0a%7c%20extend%20tagValue%20%3d%20tostring(tags%5btagKey%5d)%0a%7c%20union%20(%0a%09resources%0a%09%7c%20where%20isnotempty(tags)%0a%09%7c%20project%20tags%0a%09%7c%20mvexpand%20tags%0a%09%7c%20extend%20tagKey%20%3d%20tostring(bag_keys(tags)%5b0%5d)%0a%09%7c%20extend%20tagValue%20%3d%20tostring(tags%5btagKey%5d)%0a)%0a%7c%20distinct%20tagKey%2c%20tagValue%0a%7c%20where%20tagKey%20!startswith%20%22hidden-%22" target="_blank">portal.azure.cn</a>

---

### <a name="list-azure-arc-enabled-custom-locations-with-vmware-or-scvmm-enabled"></a>Auflisten benutzerdefinierter Standorte mit Azure Arc-Unterstützung mit VMware- oder SCVMM-Unterstützung

Stellt eine Liste aller benutzerdefinierter Standorte mit Azure Arc-Unterstützung bereit, für die VMware- oder SCVMM-Ressourcentypen aktiviert sind.

```kusto
Resources
| where type =~ 'microsoft.extendedlocation/customlocations' and properties.provisioningState =~ 'succeeded'
| extend clusterExtensionIds=properties.clusterExtensionIds
| mvexpand clusterExtensionIds
| extend clusterExtensionId = tolower(clusterExtensionIds)
| join kind=leftouter(
    ExtendedLocationResources
    | where type =~ 'microsoft.extendedlocation/customLocations/enabledResourcetypes'
    | project clusterExtensionId = tolower(properties.clusterExtensionId), extensionType = tolower(properties.extensionType)
    | where extensionType in~ ('microsoft.scvmm','microsoft.vmware')
) on clusterExtensionId
| where extensionType in~ ('microsoft.scvmm','microsoft.vmware')
| summarize virtualMachineKindsEnabled=make_set(extensionType) by id,name,location
| sort by name asc
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type =~ 'microsoft.extendedlocation/customlocations' and properties.provisioningState =~ 'succeeded' | extend clusterExtensionIds=properties.clusterExtensionIds | mvexpand clusterExtensionIds | extend clusterExtensionId = tolower(clusterExtensionIds) | join kind=leftouter( ExtendedLocationResources | where type =~ 'microsoft.extendedlocation/customLocations/enabledResourcetypes' | project clusterExtensionId = tolower(properties.clusterExtensionId), extensionType = tolower(properties.extensionType) | where extensionType in~ ('microsoft.scvmm','microsoft.vmware') ) on clusterExtensionId | where extensionType in~ ('microsoft.scvmm','microsoft.vmware') | summarize virtualMachineKindsEnabled=make_set(extensionType) by id,name,location | sort by name asc"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'microsoft.extendedlocation/customlocations' and properties.provisioningState =~ 'succeeded' | extend clusterExtensionIds=properties.clusterExtensionIds | mvexpand clusterExtensionIds | extend clusterExtensionId = tolower(clusterExtensionIds) | join kind=leftouter( ExtendedLocationResources | where type =~ 'microsoft.extendedlocation/customLocations/enabledResourcetypes' | project clusterExtensionId = tolower(properties.clusterExtensionId), extensionType = tolower(properties.extensionType) | where extensionType in~ ('microsoft.scvmm','microsoft.vmware') ) on clusterExtensionId | where extensionType in~ ('microsoft.scvmm','microsoft.vmware') | summarize virtualMachineKindsEnabled=make_set(extensionType) by id,name,location | sort by name asc"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

:::image type="icon" source="../../../../articles/governance/resource-graph/media/resource-graph-small.png":::Probieren Sie im Azure Resource Graph-Explorer die folgende Abfrage aus:

- Azure-Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27microsoft.extendedlocation%2fcustomlocations%27%20and%20properties.provisioningState%20%3d%7e%20%27succeeded%27%0a%7c%20extend%20clusterExtensionIds%3dproperties.clusterExtensionIds%0a%7c%20mvexpand%20clusterExtensionIds%0a%7c%20extend%20clusterExtensionId%20%3d%20tolower(clusterExtensionIds)%0a%7c%20join%20kind%3dleftouter(%0a%09ExtendedLocationResources%0a%09%7c%20where%20type%20%3d%7e%20%27microsoft.extendedlocation%2fcustomLocations%2fenabledResourcetypes%27%0a%09%7c%20project%20clusterExtensionId%20%3d%20tolower(properties.clusterExtensionId)%2c%20extensionType%20%3d%20tolower(properties.extensionType)%0a%09%7c%20where%20extensionType%20in%7e%20(%27microsoft.scvmm%27%2c%27microsoft.vmware%27)%0a)%20on%20clusterExtensionId%0a%7c%20where%20extensionType%20in%7e%20(%27microsoft.scvmm%27%2c%27microsoft.vmware%27)%0a%7c%20summarize%20virtualMachineKindsEnabled%3dmake_set(extensionType)%20by%20id%2cname%2clocation%0a%7c%20sort%20by%20name%20asc" target="_blank">portal.azure.com</a>
- Azure Government-Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27microsoft.extendedlocation%2fcustomlocations%27%20and%20properties.provisioningState%20%3d%7e%20%27succeeded%27%0a%7c%20extend%20clusterExtensionIds%3dproperties.clusterExtensionIds%0a%7c%20mvexpand%20clusterExtensionIds%0a%7c%20extend%20clusterExtensionId%20%3d%20tolower(clusterExtensionIds)%0a%7c%20join%20kind%3dleftouter(%0a%09ExtendedLocationResources%0a%09%7c%20where%20type%20%3d%7e%20%27microsoft.extendedlocation%2fcustomLocations%2fenabledResourcetypes%27%0a%09%7c%20project%20clusterExtensionId%20%3d%20tolower(properties.clusterExtensionId)%2c%20extensionType%20%3d%20tolower(properties.extensionType)%0a%09%7c%20where%20extensionType%20in%7e%20(%27microsoft.scvmm%27%2c%27microsoft.vmware%27)%0a)%20on%20clusterExtensionId%0a%7c%20where%20extensionType%20in%7e%20(%27microsoft.scvmm%27%2c%27microsoft.vmware%27)%0a%7c%20summarize%20virtualMachineKindsEnabled%3dmake_set(extensionType)%20by%20id%2cname%2clocation%0a%7c%20sort%20by%20name%20asc" target="_blank">portal.azure.us</a>
- Azure China 21Vianet-Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27microsoft.extendedlocation%2fcustomlocations%27%20and%20properties.provisioningState%20%3d%7e%20%27succeeded%27%0a%7c%20extend%20clusterExtensionIds%3dproperties.clusterExtensionIds%0a%7c%20mvexpand%20clusterExtensionIds%0a%7c%20extend%20clusterExtensionId%20%3d%20tolower(clusterExtensionIds)%0a%7c%20join%20kind%3dleftouter(%0a%09ExtendedLocationResources%0a%09%7c%20where%20type%20%3d%7e%20%27microsoft.extendedlocation%2fcustomLocations%2fenabledResourcetypes%27%0a%09%7c%20project%20clusterExtensionId%20%3d%20tolower(properties.clusterExtensionId)%2c%20extensionType%20%3d%20tolower(properties.extensionType)%0a%09%7c%20where%20extensionType%20in%7e%20(%27microsoft.scvmm%27%2c%27microsoft.vmware%27)%0a)%20on%20clusterExtensionId%0a%7c%20where%20extensionType%20in%7e%20(%27microsoft.scvmm%27%2c%27microsoft.vmware%27)%0a%7c%20summarize%20virtualMachineKindsEnabled%3dmake_set(extensionType)%20by%20id%2cname%2clocation%0a%7c%20sort%20by%20name%20asc" target="_blank">portal.azure.cn</a>

---

### <a name="list-cosmos-db-with-specific-write-locations"></a>Auflisten von Cosmos DB mit bestimmten Schreibstandorten

Die folgende Abfrage schränkt die Cosmos DB-Ressourcen ein, verwendet `mv-expand`, um den Eigenschaftenbehälter für **properties.writeLocations** zu erweitern, anschließend bestimmte Felder zu projizieren und die Ergebnisse weiter auf **properties.writeLocations.locationName**-Werte einzuschränken, die „USA, Osten“ oder „USA, Westen“ entsprechen.

```kusto
Resources
| where type =~ 'microsoft.documentdb/databaseaccounts'
| project id, name, writeLocations = (properties.writeLocations)
| mv-expand writeLocations
| project id, name, writeLocation = tostring(writeLocations.locationName)
| where writeLocation in ('East US', 'West US')
| summarize by id, name
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type =~ 'microsoft.documentdb/databaseaccounts' | project id, name, writeLocations = (properties.writeLocations) | mv-expand writeLocations | project id, name, writeLocation = tostring(writeLocations.locationName) | where writeLocation in ('East US', 'West US') | summarize by id, name"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'microsoft.documentdb/databaseaccounts' | project id, name, writeLocations = (properties.writeLocations) | mv-expand writeLocations | project id, name, writeLocation = tostring(writeLocations.locationName) | where writeLocation in ('East US', 'West US') | summarize by id, name"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

:::image type="icon" source="../../../../articles/governance/resource-graph/media/resource-graph-small.png":::Probieren Sie im Azure Resource Graph-Explorer die folgende Abfrage aus:

- Azure-Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27microsoft.documentdb%2fdatabaseaccounts%27%0a%7c%20project%20id%2c%20name%2c%20writeLocations%20%3d%20(properties.writeLocations)%0a%7c%20mv-expand%20writeLocations%0a%7c%20project%20id%2c%20name%2c%20writeLocation%20%3d%20tostring(writeLocations.locationName)%0a%7c%20where%20writeLocation%20in%20(%27East%20US%27%2c%20%27West%20US%27)%0a%7c%20summarize%20by%20id%2c%20name" target="_blank">portal.azure.com</a>
- Azure Government-Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27microsoft.documentdb%2fdatabaseaccounts%27%0a%7c%20project%20id%2c%20name%2c%20writeLocations%20%3d%20(properties.writeLocations)%0a%7c%20mv-expand%20writeLocations%0a%7c%20project%20id%2c%20name%2c%20writeLocation%20%3d%20tostring(writeLocations.locationName)%0a%7c%20where%20writeLocation%20in%20(%27East%20US%27%2c%20%27West%20US%27)%0a%7c%20summarize%20by%20id%2c%20name" target="_blank">portal.azure.us</a>
- Azure China 21Vianet-Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27microsoft.documentdb%2fdatabaseaccounts%27%0a%7c%20project%20id%2c%20name%2c%20writeLocations%20%3d%20(properties.writeLocations)%0a%7c%20mv-expand%20writeLocations%0a%7c%20project%20id%2c%20name%2c%20writeLocation%20%3d%20tostring(writeLocations.locationName)%0a%7c%20where%20writeLocation%20in%20(%27East%20US%27%2c%20%27West%20US%27)%0a%7c%20summarize%20by%20id%2c%20name" target="_blank">portal.azure.cn</a>

---

### <a name="list-resources-sorted-by-name"></a>Auflisten von Ressourcen, sortiert nach Name

Mit dieser Abfrage werden alle Ressourcentypen, aber nur die Eigenschaften **name**, **type** und **location** zurückgegeben. Sie nutzt `order by`, um die Eigenschaften in aufsteigender Reihenfolge (`asc`) nach der Eigenschaft **name** zu sortieren.

```kusto
Resources
| project name, type, location
| order by name asc
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | project name, type, location | order by name asc"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | project name, type, location | order by name asc"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

:::image type="icon" source="../../../../articles/governance/resource-graph/media/resource-graph-small.png":::Probieren Sie im Azure Resource Graph-Explorer die folgende Abfrage aus:

- Azure-Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20project%20name%2c%20type%2c%20location%0a%7c%20order%20by%20name%20asc" target="_blank">portal.azure.com</a>
- Azure Government-Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20project%20name%2c%20type%2c%20location%0a%7c%20order%20by%20name%20asc" target="_blank">portal.azure.us</a>
- Azure China 21Vianet-Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20project%20name%2c%20type%2c%20location%0a%7c%20order%20by%20name%20asc" target="_blank">portal.azure.cn</a>

---

### <a name="list-resources-with-a-specific-tag-value"></a>Auflisten von Ressourcen mit einem bestimmten Tagwert

Wir können die Ergebnisse nach anderen Eigenschaften als dem Azure-Ressourcentyp einschränken, z.B. einem Tag. In diesem Beispiel filtern wir mit dem Tagnamen **Environment** mit dem Wert **Internal** nach Azure-Ressourcen. Wenn Sie darüber hinaus die Tags der Ressource und ihre Werte zurückgeben möchten, fügen Sie dem Schlüsselwort `project` die Eigenschaft **tags** hinzu.

```kusto
Resources
| where tags.environment=~'internal'
| project name, tags
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where tags.environment=~'internal' | project name, tags"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where tags.environment=~'internal' | project name, tags"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

:::image type="icon" source="../../../../articles/governance/resource-graph/media/resource-graph-small.png":::Probieren Sie im Azure Resource Graph-Explorer die folgende Abfrage aus:

- Azure-Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20tags.environment%3d%7e%27internal%27%0a%7c%20project%20name%2c%20tags" target="_blank">portal.azure.com</a>
- Azure Government-Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20tags.environment%3d%7e%27internal%27%0a%7c%20project%20name%2c%20tags" target="_blank">portal.azure.us</a>
- Azure China 21Vianet-Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20tags.environment%3d%7e%27internal%27%0a%7c%20project%20name%2c%20tags" target="_blank">portal.azure.cn</a>

---

### <a name="list-sql-databases-and-their-elastic-pools"></a>Auflisten von SQL-Datenbanken und deren Pools für elastische Datenbanken

Die folgende Abfrage verwendet **leftouter** `join`, um SQL-Datenbank-Ressourcen und ggf. die zugehörigen Pools für elastische Datenbanken zusammenzuführen.

```kusto
Resources
| where type =~ 'microsoft.sql/servers/databases'
| project databaseId = id, databaseName = name, elasticPoolId = tolower(tostring(properties.elasticPoolId))
| join kind=leftouter (
    Resources
    | where type =~ 'microsoft.sql/servers/elasticpools'
    | project elasticPoolId = tolower(id), elasticPoolName = name, elasticPoolState = properties.state)
    on elasticPoolId
| project-away elasticPoolId1
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type =~ 'microsoft.sql/servers/databases' | project databaseId = id, databaseName = name, elasticPoolId = tolower(tostring(properties.elasticPoolId)) | join kind=leftouter ( Resources | where type =~ 'microsoft.sql/servers/elasticpools' | project elasticPoolId = tolower(id), elasticPoolName = name, elasticPoolState = properties.state) on elasticPoolId | project-away elasticPoolId1"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'microsoft.sql/servers/databases' | project databaseId = id, databaseName = name, elasticPoolId = tolower(tostring(properties.elasticPoolId)) | join kind=leftouter ( Resources | where type =~ 'microsoft.sql/servers/elasticpools' | project elasticPoolId = tolower(id), elasticPoolName = name, elasticPoolState = properties.state) on elasticPoolId | project-away elasticPoolId1"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

:::image type="icon" source="../../../../articles/governance/resource-graph/media/resource-graph-small.png":::Probieren Sie im Azure Resource Graph-Explorer die folgende Abfrage aus:

- Azure-Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27microsoft.sql%2fservers%2fdatabases%27%0a%7c%20project%20databaseId%20%3d%20id%2c%20databaseName%20%3d%20name%2c%20elasticPoolId%20%3d%20tolower(tostring(properties.elasticPoolId))%0a%7c%20join%20kind%3dleftouter%20(%0a%09Resources%0a%09%7c%20where%20type%20%3d%7e%20%27microsoft.sql%2fservers%2felasticpools%27%0a%09%7c%20project%20elasticPoolId%20%3d%20tolower(id)%2c%20elasticPoolName%20%3d%20name%2c%20elasticPoolState%20%3d%20properties.state)%0a%09on%20elasticPoolId%0a%7c%20project-away%20elasticPoolId1" target="_blank">portal.azure.com</a>
- Azure Government-Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27microsoft.sql%2fservers%2fdatabases%27%0a%7c%20project%20databaseId%20%3d%20id%2c%20databaseName%20%3d%20name%2c%20elasticPoolId%20%3d%20tolower(tostring(properties.elasticPoolId))%0a%7c%20join%20kind%3dleftouter%20(%0a%09Resources%0a%09%7c%20where%20type%20%3d%7e%20%27microsoft.sql%2fservers%2felasticpools%27%0a%09%7c%20project%20elasticPoolId%20%3d%20tolower(id)%2c%20elasticPoolName%20%3d%20name%2c%20elasticPoolState%20%3d%20properties.state)%0a%09on%20elasticPoolId%0a%7c%20project-away%20elasticPoolId1" target="_blank">portal.azure.us</a>
- Azure China 21Vianet-Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27microsoft.sql%2fservers%2fdatabases%27%0a%7c%20project%20databaseId%20%3d%20id%2c%20databaseName%20%3d%20name%2c%20elasticPoolId%20%3d%20tolower(tostring(properties.elasticPoolId))%0a%7c%20join%20kind%3dleftouter%20(%0a%09Resources%0a%09%7c%20where%20type%20%3d%7e%20%27microsoft.sql%2fservers%2felasticpools%27%0a%09%7c%20project%20elasticPoolId%20%3d%20tolower(id)%2c%20elasticPoolName%20%3d%20name%2c%20elasticPoolState%20%3d%20properties.state)%0a%09on%20elasticPoolId%0a%7c%20project-away%20elasticPoolId1" target="_blank">portal.azure.cn</a>

---

### <a name="list-virtual-machines-with-their-network-interface-and-public-ip"></a>Auflisten virtueller Computer mit deren Netzwerkschnittstelle und der öffentlichen IP-Adresse

Diese Abfrage verwendet zwei **leftouter** `join`-Befehle, um mit dem Resource Manager-Bereitstellungsmodell erstellte virtuelle Maschinen, ihre zugehörigen Netzwerkschnittstellen und alle öffentlichen IP-Adressen zusammenzuführen, die sich auf diese Netzwerkschnittstellen beziehen.

```kusto
Resources
| where type =~ 'microsoft.compute/virtualmachines'
| extend nics=array_length(properties.networkProfile.networkInterfaces)
| mv-expand nic=properties.networkProfile.networkInterfaces
| where nics == 1 or nic.properties.primary =~ 'true' or isempty(nic)
| project vmId = id, vmName = name, vmSize=tostring(properties.hardwareProfile.vmSize), nicId = tostring(nic.id)
| join kind=leftouter (
    Resources
    | where type =~ 'microsoft.network/networkinterfaces'
    | extend ipConfigsCount=array_length(properties.ipConfigurations)
    | mv-expand ipconfig=properties.ipConfigurations
    | where ipConfigsCount == 1 or ipconfig.properties.primary =~ 'true'
    | project nicId = id, publicIpId = tostring(ipconfig.properties.publicIPAddress.id))
    on nicId
| project-away nicId1
| summarize by vmId, vmName, vmSize, nicId, publicIpId
| join kind=leftouter (
    Resources
    | where type =~ 'microsoft.network/publicipaddresses'
    | project publicIpId = id, publicIpAddress = properties.ipAddress)
on publicIpId
| project-away publicIpId1
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type =~ 'microsoft.compute/virtualmachines' | extend nics=array_length(properties.networkProfile.networkInterfaces) | mv-expand nic=properties.networkProfile.networkInterfaces | where nics == 1 or nic.properties.primary =~ 'true' or isempty(nic) | project vmId = id, vmName = name, vmSize=tostring(properties.hardwareProfile.vmSize), nicId = tostring(nic.id) | join kind=leftouter ( Resources | where type =~ 'microsoft.network/networkinterfaces' | extend ipConfigsCount=array_length(properties.ipConfigurations) | mv-expand ipconfig=properties.ipConfigurations | where ipConfigsCount == 1 or ipconfig.properties.primary =~ 'true' | project nicId = id, publicIpId = tostring(ipconfig.properties.publicIPAddress.id)) on nicId | project-away nicId1 | summarize by vmId, vmName, vmSize, nicId, publicIpId | join kind=leftouter ( Resources | where type =~ 'microsoft.network/publicipaddresses' | project publicIpId = id, publicIpAddress = properties.ipAddress) on publicIpId | project-away publicIpId1"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'microsoft.compute/virtualmachines' | extend nics=array_length(properties.networkProfile.networkInterfaces) | mv-expand nic=properties.networkProfile.networkInterfaces | where nics == 1 or nic.properties.primary =~ 'true' or isempty(nic) | project vmId = id, vmName = name, vmSize=tostring(properties.hardwareProfile.vmSize), nicId = tostring(nic.id) | join kind=leftouter ( Resources | where type =~ 'microsoft.network/networkinterfaces' | extend ipConfigsCount=array_length(properties.ipConfigurations) | mv-expand ipconfig=properties.ipConfigurations | where ipConfigsCount == 1 or ipconfig.properties.primary =~ 'true' | project nicId = id, publicIpId = tostring(ipconfig.properties.publicIPAddress.id)) on nicId | project-away nicId1 | summarize by vmId, vmName, vmSize, nicId, publicIpId | join kind=leftouter ( Resources | where type =~ 'microsoft.network/publicipaddresses' | project publicIpId = id, publicIpAddress = properties.ipAddress) on publicIpId | project-away publicIpId1"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

:::image type="icon" source="../../../../articles/governance/resource-graph/media/resource-graph-small.png":::Probieren Sie im Azure Resource Graph-Explorer die folgende Abfrage aus:

- Azure-Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27microsoft.compute%2fvirtualmachines%27%0a%7c%20extend%20nics%3darray_length(properties.networkProfile.networkInterfaces)%0a%7c%20mv-expand%20nic%3dproperties.networkProfile.networkInterfaces%0a%7c%20where%20nics%20%3d%3d%201%20or%20nic.properties.primary%20%3d%7e%20%27true%27%20or%20isempty(nic)%0a%7c%20project%20vmId%20%3d%20id%2c%20vmName%20%3d%20name%2c%20vmSize%3dtostring(properties.hardwareProfile.vmSize)%2c%20nicId%20%3d%20tostring(nic.id)%0a%7c%20join%20kind%3dleftouter%20(%0a%09Resources%0a%09%7c%20where%20type%20%3d%7e%20%27microsoft.network%2fnetworkinterfaces%27%0a%09%7c%20extend%20ipConfigsCount%3darray_length(properties.ipConfigurations)%0a%09%7c%20mv-expand%20ipconfig%3dproperties.ipConfigurations%0a%09%7c%20where%20ipConfigsCount%20%3d%3d%201%20or%20ipconfig.properties.primary%20%3d%7e%20%27true%27%0a%09%7c%20project%20nicId%20%3d%20id%2c%20publicIpId%20%3d%20tostring(ipconfig.properties.publicIPAddress.id))%0a%09on%20nicId%0a%7c%20project-away%20nicId1%0a%7c%20summarize%20by%20vmId%2c%20vmName%2c%20vmSize%2c%20nicId%2c%20publicIpId%0a%7c%20join%20kind%3dleftouter%20(%0a%09Resources%0a%09%7c%20where%20type%20%3d%7e%20%27microsoft.network%2fpublicipaddresses%27%0a%09%7c%20project%20publicIpId%20%3d%20id%2c%20publicIpAddress%20%3d%20properties.ipAddress)%0aon%20publicIpId%0a%7c%20project-away%20publicIpId1" target="_blank">portal.azure.com</a>
- Azure Government-Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27microsoft.compute%2fvirtualmachines%27%0a%7c%20extend%20nics%3darray_length(properties.networkProfile.networkInterfaces)%0a%7c%20mv-expand%20nic%3dproperties.networkProfile.networkInterfaces%0a%7c%20where%20nics%20%3d%3d%201%20or%20nic.properties.primary%20%3d%7e%20%27true%27%20or%20isempty(nic)%0a%7c%20project%20vmId%20%3d%20id%2c%20vmName%20%3d%20name%2c%20vmSize%3dtostring(properties.hardwareProfile.vmSize)%2c%20nicId%20%3d%20tostring(nic.id)%0a%7c%20join%20kind%3dleftouter%20(%0a%09Resources%0a%09%7c%20where%20type%20%3d%7e%20%27microsoft.network%2fnetworkinterfaces%27%0a%09%7c%20extend%20ipConfigsCount%3darray_length(properties.ipConfigurations)%0a%09%7c%20mv-expand%20ipconfig%3dproperties.ipConfigurations%0a%09%7c%20where%20ipConfigsCount%20%3d%3d%201%20or%20ipconfig.properties.primary%20%3d%7e%20%27true%27%0a%09%7c%20project%20nicId%20%3d%20id%2c%20publicIpId%20%3d%20tostring(ipconfig.properties.publicIPAddress.id))%0a%09on%20nicId%0a%7c%20project-away%20nicId1%0a%7c%20summarize%20by%20vmId%2c%20vmName%2c%20vmSize%2c%20nicId%2c%20publicIpId%0a%7c%20join%20kind%3dleftouter%20(%0a%09Resources%0a%09%7c%20where%20type%20%3d%7e%20%27microsoft.network%2fpublicipaddresses%27%0a%09%7c%20project%20publicIpId%20%3d%20id%2c%20publicIpAddress%20%3d%20properties.ipAddress)%0aon%20publicIpId%0a%7c%20project-away%20publicIpId1" target="_blank">portal.azure.us</a>
- Azure China 21Vianet-Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27microsoft.compute%2fvirtualmachines%27%0a%7c%20extend%20nics%3darray_length(properties.networkProfile.networkInterfaces)%0a%7c%20mv-expand%20nic%3dproperties.networkProfile.networkInterfaces%0a%7c%20where%20nics%20%3d%3d%201%20or%20nic.properties.primary%20%3d%7e%20%27true%27%20or%20isempty(nic)%0a%7c%20project%20vmId%20%3d%20id%2c%20vmName%20%3d%20name%2c%20vmSize%3dtostring(properties.hardwareProfile.vmSize)%2c%20nicId%20%3d%20tostring(nic.id)%0a%7c%20join%20kind%3dleftouter%20(%0a%09Resources%0a%09%7c%20where%20type%20%3d%7e%20%27microsoft.network%2fnetworkinterfaces%27%0a%09%7c%20extend%20ipConfigsCount%3darray_length(properties.ipConfigurations)%0a%09%7c%20mv-expand%20ipconfig%3dproperties.ipConfigurations%0a%09%7c%20where%20ipConfigsCount%20%3d%3d%201%20or%20ipconfig.properties.primary%20%3d%7e%20%27true%27%0a%09%7c%20project%20nicId%20%3d%20id%2c%20publicIpId%20%3d%20tostring(ipconfig.properties.publicIPAddress.id))%0a%09on%20nicId%0a%7c%20project-away%20nicId1%0a%7c%20summarize%20by%20vmId%2c%20vmName%2c%20vmSize%2c%20nicId%2c%20publicIpId%0a%7c%20join%20kind%3dleftouter%20(%0a%09Resources%0a%09%7c%20where%20type%20%3d%7e%20%27microsoft.network%2fpublicipaddresses%27%0a%09%7c%20project%20publicIpId%20%3d%20id%2c%20publicIpAddress%20%3d%20properties.ipAddress)%0aon%20publicIpId%0a%7c%20project-away%20publicIpId1" target="_blank">portal.azure.cn</a>

---

### <a name="remove-columns-from-results"></a>Entfernen von Spalten aus den Ergebnissen

Die folgende Abfrage verwendet `summarize`, um Ressourcen nach Abonnement zu zählen, `join`, um sie mit Abonnementdetails aus der Tabelle _ResourceContainers_ zu vereinen, und anschließend `project-away`, um einige der Spalten zu entfernen.

```kusto
Resources
| summarize resourceCount=count() by subscriptionId
| join (ResourceContainers | where type=='microsoft.resources/subscriptions' | project SubName=name, subscriptionId) on subscriptionId
| project-away subscriptionId, subscriptionId1
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | summarize resourceCount=count() by subscriptionId | join (ResourceContainers | where type=='microsoft.resources/subscriptions' | project SubName=name, subscriptionId) on subscriptionId | project-away subscriptionId, subscriptionId1"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | summarize resourceCount=count() by subscriptionId | join (ResourceContainers | where type=='microsoft.resources/subscriptions' | project SubName=name, subscriptionId) on subscriptionId | project-away subscriptionId, subscriptionId1"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

:::image type="icon" source="../../../../articles/governance/resource-graph/media/resource-graph-small.png":::Probieren Sie im Azure Resource Graph-Explorer die folgende Abfrage aus:

- Azure-Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20summarize%20resourceCount%3dcount()%20by%20subscriptionId%0a%7c%20join%20(ResourceContainers%20%7c%20where%20type%3d%3d%27microsoft.resources%2fsubscriptions%27%20%7c%20project%20SubName%3dname%2c%20subscriptionId)%20on%20subscriptionId%0a%7c%20project-away%20subscriptionId%2c%20subscriptionId1" target="_blank">portal.azure.com</a>
- Azure Government-Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20summarize%20resourceCount%3dcount()%20by%20subscriptionId%0a%7c%20join%20(ResourceContainers%20%7c%20where%20type%3d%3d%27microsoft.resources%2fsubscriptions%27%20%7c%20project%20SubName%3dname%2c%20subscriptionId)%20on%20subscriptionId%0a%7c%20project-away%20subscriptionId%2c%20subscriptionId1" target="_blank">portal.azure.us</a>
- Azure China 21Vianet-Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20summarize%20resourceCount%3dcount()%20by%20subscriptionId%0a%7c%20join%20(ResourceContainers%20%7c%20where%20type%3d%3d%27microsoft.resources%2fsubscriptions%27%20%7c%20project%20SubName%3dname%2c%20subscriptionId)%20on%20subscriptionId%0a%7c%20project-away%20subscriptionId%2c%20subscriptionId1" target="_blank">portal.azure.cn</a>

---

### <a name="show-all-virtual-machines-ordered-by-name-in-descending-order"></a>Anzeigen aller virtuellen Computer nach Name in absteigender Reihenfolge

Wenn Sie nur virtuelle Computer (Typ: `Microsoft.Compute/virtualMachines`) auflisten möchten, können Sie die Eigenschaft **type** in den Ergebnissen zum Abgleich heranziehen. Ähnelt der vorherigen Abfrage, `desc` ändert `order by` in absteigend. `=~` in der Typübereinstimmung weist Resource Graph an, die Groß-/Kleinschreibung nicht zu berücksichtigen.

```kusto
Resources
| project name, location, type
| where type =~ 'Microsoft.Compute/virtualMachines'
| order by name desc
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | project name, location, type | where type =~ 'Microsoft.Compute/virtualMachines' | order by name desc"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | project name, location, type | where type =~ 'Microsoft.Compute/virtualMachines' | order by name desc"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

:::image type="icon" source="../../../../articles/governance/resource-graph/media/resource-graph-small.png":::Probieren Sie im Azure Resource Graph-Explorer die folgende Abfrage aus:

- Azure-Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20project%20name%2c%20location%2c%20type%0a%7c%20where%20type%20%3d%7e%20%27Microsoft.Compute%2fvirtualMachines%27%0a%7c%20order%20by%20name%20desc" target="_blank">portal.azure.com</a>
- Azure Government-Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20project%20name%2c%20location%2c%20type%0a%7c%20where%20type%20%3d%7e%20%27Microsoft.Compute%2fvirtualMachines%27%0a%7c%20order%20by%20name%20desc" target="_blank">portal.azure.us</a>
- Azure China 21Vianet-Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20project%20name%2c%20location%2c%20type%0a%7c%20where%20type%20%3d%7e%20%27Microsoft.Compute%2fvirtualMachines%27%0a%7c%20order%20by%20name%20desc" target="_blank">portal.azure.cn</a>

---

### <a name="show-first-five-virtual-machines-by-name-and-their-os-type"></a>Anzeigen der ersten fünf virtuellen Computer nach Name und Betriebssystemtyp

Diese Abfrage verwendet `top`, um nur fünf übereinstimmende Datensätze abzurufen, die nach Namen sortiert werden. Der Typ der Azure-Ressource ist `Microsoft.Compute/virtualMachines`. `project` teilt Azure Ressource Graph mit, welche Eigenschaften einbezogen werden sollen.

```kusto
Resources
| where type =~ 'Microsoft.Compute/virtualMachines'
| project name, properties.storageProfile.osDisk.osType
| top 5 by name desc
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | project name, properties.storageProfile.osDisk.osType | top 5 by name desc"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | project name, properties.storageProfile.osDisk.osType | top 5 by name desc"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

:::image type="icon" source="../../../../articles/governance/resource-graph/media/resource-graph-small.png":::Probieren Sie im Azure Resource Graph-Explorer die folgende Abfrage aus:

- Azure-Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27Microsoft.Compute%2fvirtualMachines%27%0a%7c%20project%20name%2c%20properties.storageProfile.osDisk.osType%0a%7c%20top%205%20by%20name%20desc" target="_blank">portal.azure.com</a>
- Azure Government-Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27Microsoft.Compute%2fvirtualMachines%27%0a%7c%20project%20name%2c%20properties.storageProfile.osDisk.osType%0a%7c%20top%205%20by%20name%20desc" target="_blank">portal.azure.us</a>
- Azure China 21Vianet-Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27Microsoft.Compute%2fvirtualMachines%27%0a%7c%20project%20name%2c%20properties.storageProfile.osDisk.osType%0a%7c%20top%205%20by%20name%20desc" target="_blank">portal.azure.cn</a>

---

### <a name="show-resource-types-and-api-versions"></a>Anzeigen von Ressourcentypen und API-Versionen

Resource Graph verwendet hauptsächlich die neueste Version (keine Vorschauversion) der API eines Ressourcenanbieters, um mittels `GET` Ressourceneigenschaften während einer Aktualisierung abzurufen. In einigen Fällen wurde die verwendete API-Version überschrieben, um in den Ergebnissen neuere oder gängigere Eigenschaften bereitzustellen. In der folgenden Abfrage wird die API-Version veranschaulicht, die zum Sammeln von Eigenschaften für die einzelnen Ressourcentypen verwendet wird:

```kusto
Resources
| distinct type, apiVersion
| where isnotnull(apiVersion)
| order by type asc
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | distinct type, apiVersion | where isnotnull(apiVersion) | order by type asc"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | distinct type, apiVersion | where isnotnull(apiVersion) | order by type asc"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

:::image type="icon" source="../../../../articles/governance/resource-graph/media/resource-graph-small.png":::Probieren Sie im Azure Resource Graph-Explorer die folgende Abfrage aus:

- Azure-Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20distinct%20type%2c%20apiVersion%0a%7c%20where%20isnotnull(apiVersion)%0a%7c%20order%20by%20type%20asc" target="_blank">portal.azure.com</a>
- Azure Government-Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20distinct%20type%2c%20apiVersion%0a%7c%20where%20isnotnull(apiVersion)%0a%7c%20order%20by%20type%20asc" target="_blank">portal.azure.us</a>
- Azure China 21Vianet-Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20distinct%20type%2c%20apiVersion%0a%7c%20where%20isnotnull(apiVersion)%0a%7c%20order%20by%20type%20asc" target="_blank">portal.azure.cn</a>

---

### <a name="show-resources-that-contain-storage"></a>Anzeigen von Ressourcen mit Speicher

Anstatt explizit den Typ zu definieren, mit dem Übereinstimmung vorliegen muss, findet diese Beispielabfrage jede Azure-Ressource, die das Wort **storage** enthält (`contains`).

```kusto
Resources
| where type contains 'storage' | distinct type
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type contains 'storage' | distinct type"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type contains 'storage' | distinct type"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

:::image type="icon" source="../../../../articles/governance/resource-graph/media/resource-graph-small.png":::Probieren Sie im Azure Resource Graph-Explorer die folgende Abfrage aus:

- Azure-Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20contains%20%27storage%27%20%7c%20distinct%20type" target="_blank">portal.azure.com</a>
- Azure Government-Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20contains%20%27storage%27%20%7c%20distinct%20type" target="_blank">portal.azure.us</a>
- Azure China 21Vianet-Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20contains%20%27storage%27%20%7c%20distinct%20type" target="_blank">portal.azure.cn</a>

---

### <a name="show-unassociated-network-security-groups"></a>Anzeigen nicht zugeordneter Netzwerksicherheitsgruppen

Diese Abfrage gibt Netzwerksicherheitsgruppen (Network Security Groups, NSGs) zurück, die keiner Netzwerkschnittstelle und keinem Subnetz zugeordnet sind.

```kusto
Resources
| where type =~ 'microsoft.network/networksecuritygroups' and isnull(properties.networkInterfaces) and isnull(properties.subnets)
| project name, resourceGroup
| sort by name asc
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type =~ 'microsoft.network/networksecuritygroups' and isnull(properties.networkInterfaces) and isnull(properties.subnets) | project name, resourceGroup | sort by name asc"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'microsoft.network/networksecuritygroups' and isnull(properties.networkInterfaces) and isnull(properties.subnets) | project name, resourceGroup | sort by name asc"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

:::image type="icon" source="../../../../articles/governance/resource-graph/media/resource-graph-small.png":::Probieren Sie im Azure Resource Graph-Explorer die folgende Abfrage aus:

- Azure-Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27microsoft.network%2fnetworksecuritygroups%27%20and%20isnull(properties.networkInterfaces)%20and%20isnull(properties.subnets)%0a%7c%20project%20name%2c%20resourceGroup%0a%7c%20sort%20by%20name%20asc" target="_blank">portal.azure.com</a>
- Azure Government-Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27microsoft.network%2fnetworksecuritygroups%27%20and%20isnull(properties.networkInterfaces)%20and%20isnull(properties.subnets)%0a%7c%20project%20name%2c%20resourceGroup%0a%7c%20sort%20by%20name%20asc" target="_blank">portal.azure.us</a>
- Azure China 21Vianet-Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27microsoft.network%2fnetworksecuritygroups%27%20and%20isnull(properties.networkInterfaces)%20and%20isnull(properties.subnets)%0a%7c%20project%20name%2c%20resourceGroup%0a%7c%20sort%20by%20name%20asc" target="_blank">portal.azure.cn</a>

---

### <a name="summarize-virtual-machine-by-the-power-states-extended-property"></a>Zusammenfassen virtueller Computer anhand der erweiterten Eigenschaft für Energiezustände

Diese Abfrage verwendet die [erweiterten Eigenschaften](../../../../articles/governance/resource-graph/concepts/query-language.md#extended-properties) auf virtuellen Computern, um sie nach Energiezuständen zusammenzufassen.

```kusto
Resources
| where type == 'microsoft.compute/virtualmachines'
| summarize count() by tostring(properties.extended.instanceView.powerState.code)
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type == 'microsoft.compute/virtualmachines' | summarize count() by tostring(properties.extended.instanceView.powerState.code)"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type == 'microsoft.compute/virtualmachines' | summarize count() by tostring(properties.extended.instanceView.powerState.code)"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

:::image type="icon" source="../../../../articles/governance/resource-graph/media/resource-graph-small.png":::Probieren Sie im Azure Resource Graph-Explorer die folgende Abfrage aus:

- Azure-Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%3d%20%27microsoft.compute%2fvirtualmachines%27%0a%7c%20summarize%20count()%20by%20tostring(properties.extended.instanceView.powerState.code)" target="_blank">portal.azure.com</a>
- Azure Government-Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%3d%20%27microsoft.compute%2fvirtualmachines%27%0a%7c%20summarize%20count()%20by%20tostring(properties.extended.instanceView.powerState.code)" target="_blank">portal.azure.us</a>
- Azure China 21Vianet-Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%3d%20%27microsoft.compute%2fvirtualmachines%27%0a%7c%20summarize%20count()%20by%20tostring(properties.extended.instanceView.powerState.code)" target="_blank">portal.azure.cn</a>

---

### <a name="virtual-machines-matched-by-regex"></a>Einem regulären Ausdruck entsprechende virtuelle Computer

Diese Abfrage sucht nach virtuellen Computern, die einem [regulären Ausdruck](/dotnet/standard/base-types/regular-expression-language-quick-reference) (als _RegEx_ bekannt) entsprechen. Mit **matches regex \@** wird der reguläre Ausdruck für den Abgleich definiert: `^Contoso(.*)[0-9]+$`. Diese RegEx-Definition wird wie folgt erläutert:

- `^` – Die Übereinstimmung muss am Anfang der Zeichenfolge beginnen.
- `Contoso` – Zeichenfolge mit Beachtung von Groß-/Kleinschreibung
- `(.*)` – Eine Übereinstimmung mit einem Teilausdruck:
  - `.` – stimmt mit einem beliebigen einzelnen Zeichen (außer eines Zeilenumbruchs) überein.
  - `*` – stimmt mit dem vorhergehenden Element null Mal oder mehrfach überein.
- `[0-9]` – Übereinstimmung von Zeichengruppe für die Ziffern 0 bis 9.
- `+` – stimmt mit dem vorhergehenden Element ein Mal oder mehrfach überein.
- `$` – Übereinstimmung des vorhergehenden Elements muss am Ende der Zeichenfolge auftreten.

Nach dem Namensabgleich stellt die Abfrage den Namen dar und sortiert aufsteigend nach Name.

```kusto
Resources
| where type =~ 'microsoft.compute/virtualmachines' and name matches regex @'^Contoso(.*)[0-9]+$'
| project name
| order by name asc
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type =~ 'microsoft.compute/virtualmachines' and name matches regex @'^Contoso(.*)[0-9]+$' | project name | order by name asc"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'microsoft.compute/virtualmachines' and name matches regex @'^Contoso(.*)[0-9]+$' | project name | order by name asc"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

:::image type="icon" source="../../../../articles/governance/resource-graph/media/resource-graph-small.png":::Probieren Sie im Azure Resource Graph-Explorer die folgende Abfrage aus:

- Azure-Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27microsoft.compute%2fvirtualmachines%27%20and%20name%20matches%20regex%20%40%27%5eContoso(.*)%5b0-9%5d%2b%24%27%0a%7c%20project%20name%0a%7c%20order%20by%20name%20asc" target="_blank">portal.azure.com</a>
- Azure Government-Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27microsoft.compute%2fvirtualmachines%27%20and%20name%20matches%20regex%20%40%27%5eContoso(.*)%5b0-9%5d%2b%24%27%0a%7c%20project%20name%0a%7c%20order%20by%20name%20asc" target="_blank">portal.azure.us</a>
- Azure China 21Vianet-Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0a%7c%20where%20type%20%3d%7e%20%27microsoft.compute%2fvirtualmachines%27%20and%20name%20matches%20regex%20%40%27%5eContoso(.*)%5b0-9%5d%2b%24%27%0a%7c%20project%20name%0a%7c%20order%20by%20name%20asc" target="_blank">portal.azure.cn</a>

---

