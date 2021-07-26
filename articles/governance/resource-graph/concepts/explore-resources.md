---
title: Untersuchen von Azure-Ressourcen
description: Hier erfahren Sie, wie Sie mit der Abfragesprache Resource Graph Ihre Ressourcen untersuchen und herausfinden, wie diese miteinander vernetzt sind.
ms.date: 05/11/2021
ms.topic: conceptual
ms.openlocfilehash: f76d163ca113617aa25917feb24908eb72d4f515
ms.sourcegitcommit: 1b19b8d303b3abe4d4d08bfde0fee441159771e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/11/2021
ms.locfileid: "109750879"
---
# <a name="explore-your-azure-resources-with-resource-graph"></a>Untersuchen Ihrer Azure-Ressourcen mit Resource Graph

Mit Azure Resource Graph können Sie Ihre Azure-Ressourcen schnell und bedarfsgerecht erkunden und ermitteln. Resource Graph wurde speziell für schnelle Reaktionen entwickelt und bietet eine gute Möglichkeit, eine Umgebung sowie die Eigenschaften kennenzulernen, die in Azure-Ressourcen enthalten sind.

## <a name="explore-virtual-machines"></a>Erkunden von virtuellen Computern

Ein virtueller Computer ist eine typische Ressource in Azure. Als Ressourcentyp verfügen virtuelle Computer über viele Eigenschaften, die abgefragt werden können. Anhand der einzelnen Eigenschaften kann nach der jeweils gewünschten Ressource gefiltert oder gesucht werden.

### <a name="virtual-machine-discovery"></a>Ermittlung von virtuellen Computern

Wir beginnen zunächst mit einer einfachen Abfrage zum Abrufen eines einzelnen virtuellen Computers in unserer Umgebung und untersuchen anschließend die zurückgegebenen Eigenschaften.

```kusto
Resources
| where type =~ 'Microsoft.Compute/virtualMachines'
| limit 1
```

```azurecli-interactive
az graph query -q "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | limit 1"
```

```azurepowershell-interactive
(Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | limit 1").Data | ConvertTo-Json -Depth 100
```

> [!NOTE]
> Das Azure PowerShell-Cmdlet `Search-AzGraph` gibt standardmäßig **PSResourceGraphResponse** zurück. Damit die Ausgabe dem von der Azure-Befehlszeilenschnittstelle zurückgegebenen Ergebnis entspricht, wird das Cmdlet `ConvertTo-Json` für die Eigenschaft **Data** verwendet. Der Standardwert für **Depth** ist _2_. Bei einem Wert von _100_ sollten alle zurückgegebenen Ebenen konvertiert werden.

Die JSON-Ergebnisse sind etwa wie im folgenden Beispiel strukturiert:

```json
[
  {
    "id": "/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/virtualMachines/ContosoVM1",
    "kind": "",
    "location": "westus2",
    "managedBy": "",
    "name": "ContosoVM1",
    "plan": {},
    "properties": {
      "hardwareProfile": {
        "vmSize": "Standard_B2s"
      },
      "networkProfile": {
        "networkInterfaces": [
          {
            "id": "/subscriptions/<subscriptionId>/MyResourceGroup/providers/Microsoft.Network/networkInterfaces/contosovm1535",
            "resourceGroup": "MyResourceGroup"
          }
        ]
      },
      "osProfile": {
        "adminUsername": "localAdmin",
        "computerName": "ContosoVM1",
        "secrets": [],
        "windowsConfiguration": {
          "enableAutomaticUpdates": true,
          "provisionVMAgent": true
        }
      },
      "provisioningState": "Succeeded",
      "storageProfile": {
        "dataDisks": [],
        "imageReference": {
          "offer": "WindowsServer",
          "publisher": "MicrosoftWindowsServer",
          "sku": "2016-Datacenter",
          "version": "latest"
        },
        "osDisk": {
          "caching": "ReadWrite",
          "createOption": "FromImage",
          "diskSizeGB": 127,
          "managedDisk": {
            "id": "/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/disks/ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166",
            "resourceGroup": "MyResourceGroup",
            "storageAccountType": "Premium_LRS"
          },
          "name": "ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166",
          "osType": "Windows"
        }
      },
      "vmId": "bbb9b451-6dc7-4117-bec5-c971eb1118c6"
    },
    "resourceGroup": "MyResourceGroup",
    "sku": {},
    "subscriptionId": "<subscriptionId>",
    "tags": {},
    "type": "microsoft.compute/virtualmachines"
  }
]
```

Die Eigenschaften vermitteln uns zusätzliche Informationen über den virtuellen Computer selbst. Die Eigenschaften umfassen: Betriebssystem, Datenträger, Tags sowie die Ressourcengruppe und das Abonnement, denen der virtuelle Computer angehört.

### <a name="virtual-machines-by-location"></a>Virtuelle Computer nach Standort

Ausgehend von dem, was wir über die VM-Ressource erfahren haben, zählen wir anhand der Eigenschaft **location** alle virtuellen Computer nach Standort. Wir aktualisieren die Abfrage, indem wir den Grenzwert entfernen und die Anzahl der Speicherortwerte summieren.

```kusto
Resources
| where type =~ 'Microsoft.Compute/virtualMachines'
| summarize count() by location
```

```azurecli-interactive
az graph query -q "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | summarize count() by location"
```

```azurepowershell-interactive
(Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | summarize count() by location").Data | ConvertTo-Json
```

Die JSON-Ergebnisse sind etwa wie im folgenden Beispiel strukturiert:

```json
[
  {
    "count_": 386,
    "location": "eastus"
  },
  {
    "count_": 215,
    "location": "southcentralus"
  },
  {
    "count_": 59,
    "location": "westus"
  }
]
```

Wir sehen nun, wie viele virtuelle Computer sich in den einzelnen Azure-Regionen befinden.

### <a name="virtual-machines-by-sku"></a>Virtuelle Computer nach SKU

Kehren wir zu den ursprünglichen Eigenschaften des virtuellen Computers zurück und suchen nun alle virtuellen Computer mit der SKU-Größe **Standard_B2s**. Anhand des JSON-Ergebnisse können wir erkennen, dass dieser Wert in **properties.hardwareprofile.vmsize** gespeichert ist. Also aktualisieren wir die Abfrage so, dass alle virtuellen Computer mit dieser Größe gesucht und nur die Namen der virtuellen Computer und die jeweiligen Regionen zurückgegeben werden.

```kusto
Resources
| where type =~ 'Microsoft.Compute/virtualMachines' and properties.hardwareProfile.vmSize == 'Standard_B2s'
| project name, resourceGroup
```

```azurecli-interactive
az graph query -q "Resources | where type =~ 'Microsoft.Compute/virtualMachines' and properties.hardwareProfile.vmSize == 'Standard_B2s' | project name, resourceGroup"
```

```azurepowershell-interactive
(Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Compute/virtualMachines' and properties.hardwareProfile.vmSize == 'Standard_B2s' | project name, resourceGroup").Data | ConvertTo-Json
```

### <a name="virtual-machines-connected-to-premium-managed-disks"></a>Mit Managed Disks Premium vernetzte virtuelle Computer

Um die Details von Managed Disks Premium abzurufen, die diesen virtuellen **Standard_B2s**-Computern zugeordnet sind, erweitern wir die Abfrage, sodass die Ressourcen-IDs dieser verwalteten Datenträger zurückgegeben werden.

```kusto
Resources
| where type =~ 'Microsoft.Compute/virtualmachines' and properties.hardwareProfile.vmSize == 'Standard_B2s'
| extend disk = properties.storageProfile.osDisk.managedDisk
| where disk.storageAccountType == 'Premium_LRS'
| project disk.id
```

```azurecli-interactive
az graph query -q "Resources | where type =~ 'Microsoft.Compute/virtualmachines' and properties.hardwareProfile.vmSize == 'Standard_B2s' | extend disk = properties.storageProfile.osDisk.managedDisk | where disk.storageAccountType == 'Premium_LRS' | project disk.id"
```

```azurepowershell-interactive
(Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Compute/virtualmachines' and properties.hardwareProfile.vmSize == 'Standard_B2s' | extend disk = properties.storageProfile.osDisk.managedDisk | where disk.storageAccountType == 'Premium_LRS' | project disk.id").Data | ConvertTo-Json
```

Das Ergebnis ist eine Liste mit Datenträger-IDs.

### <a name="managed-disk-discovery"></a>Ermittlung von verwalteten Datenträgern

Mit dem ersten Datensatz aus der vorherigen Abfrage untersuchen wir die Eigenschaften für den verwalteten Datenträger, der dem ersten virtuellen Computer zugeordnet wurde. Für die aktualisierte Abfrage wird die Datenträger-ID verwendet und der Typ geändert.

Beispielausgabe aus der vorherigen Abfrage:

```json
[
  {
    "disk_id": "/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/disks/ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166"
  }
]
```

```kusto
Resources
| where type =~ 'Microsoft.Compute/disks' and id == '/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/disks/ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166'
```

Woher wussten wir vor dem Ausführen der Abfrage, dass für **type** jetzt **Microsoft.Compute/disks** festgelegt werden muss? Wenn Sie sich die vollständige ID ansehen, erkennen Sie, dass **/providers/Microsoft.Compute/disks/** Teil der Zeichenfolge ist.
Dieses Zeichenfolgenfragment ist ein Hinweis darauf, nach welchem Typ gesucht werden muss. Alternativ kann auch der Grenzwert nach Typ entfernt und stattdessen nur nach dem ID-Feld gesucht werden. Da die ID eindeutig ist, wird nur ein Datensatz zurückgegeben, und dessen **type**-Eigenschaft enthält diese Information.

> [!NOTE]
> Damit dieses Beispiel funktioniert, müssen Sie das ID-Feld durch ein Ergebnis aus Ihrer Umgebung ersetzen.

```azurecli-interactive
az graph query -q "Resources | where type =~ 'Microsoft.Compute/disks' and id == '/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/disks/ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166'"
```

```azurepowershell-interactive
(Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Compute/disks' and id == '/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/disks/ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166'").Data | ConvertTo-Json
```

Die JSON-Ergebnisse sind etwa wie im folgenden Beispiel strukturiert:

```json
[
  {
    "id": "/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/disks/ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166",
    "kind": "",
    "location": "westus2",
    "managedBy": "",
    "name": "ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166",
    "plan": {},
    "properties": {
      "creationData": {
        "createOption": "Empty"
      },
      "diskSizeGB": 127,
      "diskState": "ActiveSAS",
      "provisioningState": "Succeeded",
      "timeCreated": "2018-09-14T12:17:32.2570000Z"
    },
    "resourceGroup": "MyResourceGroup",
    "sku": {
      "name": "Premium_LRS",
      "tier": "Premium"
    },
    "subscriptionId": "<subscriptionId>",
    "tags": {
      "environment": "prod"
    },
    "type": "microsoft.compute/disks"
  }
]
```

## <a name="explore-virtual-machines-to-find-public-ip-addresses"></a>Erkunden von virtuellen Computern zum Suchen nach öffentlichen IP-Adressen

Mit dieser Gruppe von Abfragen werden zuerst alle Netzwerkschnittstellenressourcen gefunden und gespeichert, die mit virtuellen Computern verbunden sind. Anschließend wird die Liste mit den Netzwerkschnittstellen von den Abfragen genutzt, um nach den einzelnen IP-Adressressourcen zu suchen, bei denen es sich um eine öffentliche IP-Adresse handelt. Diese Werte werden dann gespeichert. Abschließend wird von den Abfragen eine Liste mit den öffentlichen IP-Adressen bereitgestellt.

```azurecli-interactive
# Use Resource Graph to get all NICs and store in the 'nics.txt' file
az graph query -q "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | project nic = tostring(properties['networkProfile']['networkInterfaces'][0]['id']) | where isnotempty(nic) | distinct nic | limit 20" --output table | tail -n +3 > nics.txt

# Review the output of the query stored in 'nics.txt'
cat nics.txt
```

```azurepowershell-interactive
# Use Resource Graph to get all NICs and store in the $nics variable
$nics = (Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | project nic = tostring(properties['networkProfile']['networkInterfaces'][0]['id']) | where isnotempty(nic) | distinct nic | limit 20").Data

# Review the output of the query stored in the variable
$nics.nic
```

Verwenden Sie die Datei (Azure CLI) oder die Variable (Azure PowerShell) in der nächsten Abfrage zum Abrufen der Details der entsprechenden Netzwerkschnittstellenressourcen, denen eine öffentliche IP-Adresse zugeordnet ist.

```azurecli-interactive
# Use Resource Graph with the 'nics.txt' file to get all related public IP addresses and store in 'publicIp.txt' file
az graph query -q="Resources | where type =~ 'Microsoft.Network/networkInterfaces' | where id in ('$(awk -vORS="','" '{print $0}' nics.txt | sed 's/,$//')') | project publicIp = tostring(properties['ipConfigurations'][0]['properties']['publicIPAddress']['id']) | where isnotempty(publicIp) | distinct publicIp" --output table | tail -n +3 > ips.txt

# Review the output of the query stored in 'ips.txt'
cat ips.txt
```

```azurepowershell-interactive
# Use Resource Graph  with the $nics variable to get all related public IP addresses and store in $ips variable
$ips = (Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Network/networkInterfaces' | where id in ('$($nics.nic -join "','")') | project publicIp = tostring(properties['ipConfigurations'][0]['properties']['publicIPAddress']['id']) | where isnotempty(publicIp) | distinct publicIp").Data

# Review the output of the query stored in the variable
$ips.publicIp
```

Zum Schluss verwenden Sie die in der Datei (Azure CLI) oder der Variablen (Azure PowerShell) gespeicherte Liste der öffentlichen IP-Adressressourcen zum Abrufen der tatsächlichen öffentlichen IP-Adresse aus dem zugehörigen Objekt und zeigen diese an.

```azurecli-interactive
# Use Resource Graph with the 'ips.txt' file to get the IP address of the public IP address resources
az graph query -q="Resources | where type =~ 'Microsoft.Network/publicIPAddresses' | where id in ('$(awk -vORS="','" '{print $0}' ips.txt | sed 's/,$//')') | project ip = tostring(properties['ipAddress']) | where isnotempty(ip) | distinct ip" --output table
```

```azurepowershell-interactive
# Use Resource Graph with the $ips variable to get the IP address of the public IP address resources
(Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Network/publicIPAddresses' | where id in ('$($ips.publicIp -join "','")') | project ip = tostring(properties['ipAddress']) | where isnotempty(ip) | distinct ip").Data | ConvertTo-Json
```

Im Beispiel [Auflisten virtueller Computer mit deren Netzwerkschnittstelle und der öffentlichen IP-Adresse](../samples/advanced.md#join-vmpip) wird gezeigt, wie Sie diese Schritte in einer einzelnen Abfrage mit dem Operator `join` ausführen.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [Abfragesprache](query-language.md).
- Informationen zur verwendeten Sprache finden Sie unter [Einfache Abfragen](../samples/starter.md).
- Informationen zur anspruchsvolleren Nutzung finden Sie unter [Erweiterte Abfragen](../samples/advanced.md).
