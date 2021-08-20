---
title: Sichern virtueller Azure-Computer mithilfe der REST-API
description: In diesem Artikel erfahren Sie, wie Sie Sicherungsvorgänge von Azure Backup mithilfe der REST-API konfigurieren, initiieren und verwalten.
ms.topic: conceptual
ms.date: 08/03/2018
ms.assetid: b80b3a41-87bf-49ca-8ef2-68e43c04c1a3
ms.openlocfilehash: 57187a9f7ecf3e1d00fa395d25d98fd03f5d2698
ms.sourcegitcommit: 7d63ce88bfe8188b1ae70c3d006a29068d066287
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2021
ms.locfileid: "114461021"
---
# <a name="back-up-an-azure-vm-using-azure-backup-via-rest-api"></a>Sichern eines virtuellen Azure-Computers mithilfe von Azure Backup über die REST-API

In diesem Artikel wird beschrieben, wie Sicherungen für einen virtuellen Azure-Computer mithilfe von Azure Backup über die REST-API verwaltet werden. Konfigurieren Sie entsprechend den Beschreibungen in diesem Artikel den erstmaligen Schutz für einen zuvor nicht geschützten virtuellen Azure-Computer, lösen Sie eine bedarfsgesteuerte Sicherung für einen geschützten virtuellen Azure-Computer aus, und ändern Sie die Sicherungseigenschaften eines gesicherten virtuellen Computers über die REST-API.

Informationen zum Erstellen neuer Tresore und Richtlinien finden Sie in den REST-API-Tutorials zum [Erstellen eines Tresors](backup-azure-arm-userestapi-createorupdatevault.md) und zum [Erstellen einer Richtlinie](backup-azure-arm-userestapi-createorupdatepolicy.md).

Angenommen, Sie möchten den virtuellen Computer „testVM“ unter der Ressourcengruppe „testRG“ im Recovery Services-Tresor „testVault“ schützen, der in der Ressourcengruppe „testVaultRG“ mit der Standardrichtlinie („DefaultPolicy“) vorhanden ist.

## <a name="configure-backup-for-an-unprotected-azure-vm-using-rest-api"></a>Konfigurieren der Sicherung für einen nicht geschützten virtuellen Azure-Computer über die REST-API

### <a name="discover-unprotected-azure-vms"></a>Ermitteln nicht geschützter virtueller Azure-Computer

Zunächst muss der Tresor den virtuellen Azure-Computer ermitteln können. Dies wird über den [„refresh“-Vorgang](/rest/api/backup/protection-containers/refresh) ausgelöst. Es handelt sich um einen asynchronen *POST*-Vorgang, mit dem sichergestellt wird, dass der Tresor die neueste Liste aller nicht geschützten virtuellen Computer im aktuellen Abonnement abruft und „zwischenspeichert“. Wenn der virtuelle Computer „zwischengespeichert“ wurde, kann in Recovery Services auf ihn zugegriffen werden, um ihn zu schützen.

```http
POST https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{vaultresourceGroupname}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/refreshContainers?api-version=2016-12-01
```

Der POST-URI enthält die Parameter `{subscriptionId}`, `{vaultName}`, `{vaultresourceGroupName}` und `{fabricName}`. Für `{fabricName}` ist „Azure“ festgelegt. In unserem Beispiel ist „testVault“ für `{vaultName}` und „testVaultRG“ für `{vaultresourceGroupName}` festgelegt. Da alle erforderlichen Parameter im URI angegeben sind, besteht keine Notwendigkeit eines separaten Anforderungstexts.

```http
POST https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/Microsoft.RecoveryServices/vaults/testVault/backupFabrics/Azure/refreshContainers?api-version=2016-12-01
```

#### <a name="responses-to-refresh-operation"></a>Antworten auf den Aktualisierungsvorgang

Der „refresh“-Vorgang ist ein [asynchroner Vorgang](../azure-resource-manager/management/async-operations.md). Das bedeutet, dass in diesem Vorgang ein anderer Vorgang erstellt wird, der separat nachverfolgt werden muss.

Er gibt zwei Antworten zurück: „202 (Akzeptiert)“, wenn ein anderer Vorgang erstellt wird, und dann „200 (OK)“, wenn dieser Vorgang abgeschlossen ist.

|Name  |type  |BESCHREIBUNG  |
|---------|---------|---------|
|204 Kein Inhalt     |         |  OK, wird ohne Inhalt zurückgegeben      |
|202 – Akzeptiert     |         |     Zulässig    |

##### <a name="example-responses-to-refresh-operation"></a>Beispielantworten auf den Aktualisierungsvorgang

Nachdem die *POST*-Anforderung gesendet wurde, wird die Antwort „202 (Akzeptiert)“ zurückgegeben.

```http
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 60
X-Content-Type-Options: nosniff
x-ms-request-id: 43cf550d-e463-421c-8922-37e4766db27d
x-ms-client-request-id: 4910609f-bb9b-4c23-8527-eb6fa2d3253f; 4910609f-bb9b-4c23-8527-eb6fa2d3253f
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: 43cf550d-e463-421c-8922-37e4766db27d
x-ms-routing-request-id: SOUTHINDIA:20180521T105701Z:43cf550d-e463-421c-8922-37e4766db27d
Cache-Control: no-cache
Date: Mon, 21 May 2018 10:57:00 GMT
Location: https://management.azure.com/subscriptions//00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/operationResults/aad204aa-a5cf-4be2-a7db-a224819e5890?api-version=2019-05-13
X-Powered-By: ASP.NET
```

Verfolgen Sie den resultierenden Vorgang mithilfe des „Location“-Headers mit einem einfachen *GET*-Befehl nach.

```http
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/operationResults/aad204aa-a5cf-4be2-a7db-a224819e5890?api-version=2019-05-13
```

Nachdem alle virtuellen Azure-Computer ermittelt wurden, gibt der GET-Befehl die Antwort „204 (Kein Inhalt)“ zurück. Der Tresor kann nun alle virtuellen Computer im Abonnement ermitteln.

```http
HTTP/1.1 204 NoContent
Pragma: no-cache
X-Content-Type-Options: nosniff
x-ms-request-id: cf6cd73b-9189-4942-a61d-878fcf76b1c1
x-ms-client-request-id: 25bb6345-f9fc-4406-be1a-dc6db0eefafe; 25bb6345-f9fc-4406-be1a-dc6db0eefafe
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-reads: 14997
x-ms-correlation-request-id: cf6cd73b-9189-4942-a61d-878fcf76b1c1
x-ms-routing-request-id: SOUTHINDIA:20180521T105825Z:cf6cd73b-9189-4942-a61d-878fcf76b1c1
Cache-Control: no-cache
Date: Mon, 21 May 2018 10:58:25 GMT
X-Powered-By: ASP.NET
```

### <a name="selecting-the-relevant-azure-vm"></a>Auswählen des entsprechenden virtuellen Azure-Computers

 Sie können überprüfen, ob die „Zwischenspeicherung“ erfolgt ist, indem Sie [alle schützbaren Elemente](/rest/api/backup/backup-protectable-items/list) unter dem Abonnement auflisten und den gewünschten virtuellen Computer in der Antwort suchen. [Die Antwort dieses Vorgangs](#example-responses-to-get-operation) enthält auch Informationen dazu, wie Recovery Services einen virtuellen Computer identifiziert.  Wenn Sie mit dem Muster vertraut sind, können Sie diesen Schritt überspringen und direkt mit dem [Aktivieren des Schutzes](#enabling-protection-for-the-azure-vm) fortfahren.

Dieser Vorgang ist ein *GET*-Vorgang.

```http
GET https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{vaultresourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupProtectableItems?api-version=2016-12-01&$filter=backupManagementType eq 'AzureIaasVM'
```

Der *GET*-URI enthält alle erforderlichen Parameter. Es ist kein zusätzlicher Anforderungstext erforderlich.

#### <a name="responses-to-get-operation"></a>Antworten auf den GET-Vorgang

|Name  |type  |BESCHREIBUNG  |
|---------|---------|---------|
|200 – OK     | [WorkloadProtectableItemResourceList](/rest/api/backup/backup-protectable-items/list#workloadprotectableitemresourcelist)        |       OK |

#### <a name="example-responses-to-get-operation"></a>Beispielantworten auf den GET-Vorgang

Nachdem die *GET*-Anforderung gesendet wurde, wird die Antwort „200 (OK)“ zurückgegeben.

```http
HTTP/1.1 200 OK
Pragma: no-cache
X-Content-Type-Options: nosniff
x-ms-request-id: 7c2cf56a-e6be-4345-96df-c27ed849fe36
x-ms-client-request-id: 40c8601a-c217-4c68-87da-01db8dac93dd; 40c8601a-c217-4c68-87da-01db8dac93dd
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-reads: 14979
x-ms-correlation-request-id: 7c2cf56a-e6be-4345-96df-c27ed849fe36
x-ms-routing-request-id: SOUTHINDIA:20180521T071408Z:7c2cf56a-e6be-4345-96df-c27ed849fe36
Cache-Control: no-cache
Date: Mon, 21 May 2018 07:14:08 GMT
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET

{
  "value": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/IaasVMContainer;iaasvmcontainerv2;testRG;testVM/protectableItems/vm;iaasvmcontainerv2;testRG;testVM",
      "name": "iaasvmcontainerv2;testRG;testVM",
      "type": "Microsoft.RecoveryServices/vaults/backupFabrics/protectionContainers/protectableItems",
      "properties": {
        "virtualMachineId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
        "virtualMachineVersion": "Compute",
        "resourceGroup": "testRG",
        "backupManagementType": "AzureIaasVM",
        "protectableItemType": "Microsoft.Compute/virtualMachines",
        "friendlyName": "testVM",
        "protectionState": "NotProtected"
      }
    },……………..

```

> [!TIP]
> Die Anzahl der Werte in einer *GET*-Antwort ist für eine „Seite“ auf 200 beschränkt. Verwenden Sie das Feld „nextLink“, um die URL für die nächste Gruppe von Antworten abzurufen.

Die Antwort enthält die Liste aller nicht geschützten virtuellen Azure-Computer, und jedes `{value}`-Feld enthält alle Informationen, die in Azure Recovery Services zum Konfigurieren der Sicherung erforderlich sind. Notieren Sie zum Konfigurieren der Sicherung das Feld `{name}` und das Feld `{virtualMachineId}` im Abschnitt `{properties}`. Erstellen Sie wie im Folgenden aufgeführt zwei Variablen aus diesen Feldwerten.

- containerName = "iaasvmcontainer;"+`{name}`
- protectedItemName = "vm;"+ `{name}`
- `{virtualMachineId}` wird später im [Anforderungstext](#example-request-body) verwendet.

Im Beispiel ergeben die Werte oben Folgendes:

- containerName = "iaasvmcontainer;iaasvmcontainerv2;testRG;testVM"
- protectedItemName = "vm;iaasvmcontainerv2;testRG;testVM"

### <a name="enabling-protection-for-the-azure-vm"></a>Aktivieren des Schutzes für den virtuellen Azure-Computer

Nachdem der entsprechende virtuelle Computer zwischengespeichert und ermittelt wurde, wählen Sie die Richtlinie zum Schutz aus. Weitere Informationen zu vorhandenen Richtlinien im Tresor finden Sie in der [Liste der Sicherungsrichtlinien](/rest/api/backup/backup-policies/list). Wählen Sie dann die [entsprechende Richtlinie](/rest/api/backup/protection-policies/get) aus, indem Sie auf den Richtliniennamen verweisen. Informationen zum Erstellen von Richtlinien finden Sie im [Tutorial zum Erstellen von Richtlinien](backup-azure-arm-userestapi-createorupdatepolicy.md). Im Beispiel unten ist „DefaultPolicy“ ausgewählt.

Beim Aktivieren des Schutzes handelt es sich um einen asynchronen *PUT*-Vorgang, mit dem ein „geschütztes Element“ erstellt wird.

```http
https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{vaultresourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}?api-version=2019-05-13
```

`{containerName}` und `{protectedItemName}` entsprechen der Erstellung oben. Für `{fabricName}` ist „Azure“ festgelegt. In unserem Beispiel ergibt dies Folgendes:

```http
PUT https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/Microsoft.RecoveryServices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;iaasvmcontainerv2;testRG;testVM?api-version=2019-05-13
```

#### <a name="create-the-request-body"></a>Erstellen des Anforderungstexts

Zum Erstellen eines geschützten Elements werden die folgenden Komponenten des Anforderungstexts verwendet.

|Name  |type  |BESCHREIBUNG  |
|---------|---------|---------|
|properties     | AzureIaaSVMProtectedItem        |ProtectedItem-Ressourceneigenschaften         |

Die vollständige Liste mit Definitionen des Anforderungstexts und weitere Einzelheiten finden Sie im [Dokument zur REST-API zum Erstellen eines geschützten Elements](/rest/api/backup/protected-items/create-or-update#request-body).

##### <a name="example-request-body"></a>Beispiel für Anforderungstext

Der folgende Anforderungstext definiert zum Erstellen eines geschützten Elements erforderliche Eigenschaften.

```json
{
  "properties": {
    "protectedItemType": "Microsoft.Compute/virtualMachines",
    "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
    "policyId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupPolicies/DefaultPolicy"
  }
}
```

Bei `{sourceResourceId}` handelt es sich um die oben genannte `{virtualMachineId}` aus der [Antwort der Liste der schützbaren Elemente](#example-responses-to-get-operation).

#### <a name="responses-to-create-protected-item-operation"></a>Antworten auf den Vorgang zum Erstellen eines geschützten Elements

Die Erstellung eines geschützten Elements ist ein [asynchroner Vorgang](../azure-resource-manager/management/async-operations.md). Das bedeutet, dass in diesem Vorgang ein anderer Vorgang erstellt wird, der separat nachverfolgt werden muss.

Er gibt zwei Antworten zurück: „202 (Akzeptiert)“, wenn ein anderer Vorgang erstellt wird, und dann „200 (OK)“, wenn dieser Vorgang abgeschlossen ist.

|Name  |type  |BESCHREIBUNG  |
|---------|---------|---------|
|200 – OK     |    [ProtectedItemResource](/rest/api/backup/protected-item-operation-results/get#protecteditemresource)     |  OK       |
|202 – Akzeptiert     |         |     Zulässig    |

##### <a name="example-responses-to-create-protected-item-operation"></a>Beispielantworten auf den Vorgang zum Erstellen eines geschützten Elements

Nachdem Sie die *PUT*-Anforderung für die Erstellung oder Aktualisierung eines geschützten Elements gesendet haben, wird als erste Antwort „202 (Akzeptiert)“ mit einem location- oder Azure-async-Header zurückgegeben.

```http
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 60
Azure-AsyncOperation: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM/operationsStatus/a0866047-6fc7-4ac3-ba38-fb0ae8aa550f?api-version=2019-05-13
X-Content-Type-Options: nosniff
x-ms-request-id: db785be0-bb20-4598-bc9f-70c9428b170b
x-ms-client-request-id: e1f94eef-9b2d-45c4-85b8-151e12b07d03; e1f94eef-9b2d-45c4-85b8-151e12b07d03
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: db785be0-bb20-4598-bc9f-70c9428b170b
x-ms-routing-request-id: SOUTHINDIA:20180521T073907Z:db785be0-bb20-4598-bc9f-70c9428b170b
Cache-Control: no-cache
Date: Mon, 21 May 2018 07:39:06 GMT
Location: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM/operationResults/a0866047-6fc7-4ac3-ba38-fb0ae8aa550f?api-version=2019-05-13
X-Powered-By: ASP.NET
```

Verfolgen Sie anschließend den resultierenden Vorgang mithilfe des location- oder Azure-AsyncOperation-Headers mit einem einfachen *GET*-Befehl nach.

```http
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM/operationsStatus/a0866047-6fc7-4ac3-ba38-fb0ae8aa550f?api-version=2019-05-13
```

Wenn der Vorgang abgeschlossen ist, wird „200 (OK)“ mit dem Inhalt des geschützten Elements im Antworttext zurückgegeben.

```json
{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM",
  "name": "VM;testRG;testVM",
  "type": "Microsoft.RecoveryServices/vaults/backupFabrics/protectionContainers/protectedItems",
  "properties": {
    "friendlyName": "testVM",
    "virtualMachineId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
    "protectionStatus": "Healthy",
    "protectionState": "IRPending",
    "healthStatus": "Passed",
    "lastBackupStatus": "",
    "lastBackupTime": "2001-01-01T00:00:00Z",
    "protectedItemDataId": "17592691116891",
    "extendedInfo": {
      "recoveryPointCount": 0,
      "policyInconsistent": false
    },
    "protectedItemType": "Microsoft.Compute/virtualMachines",
    "backupManagementType": "AzureIaasVM",
    "workloadType": "VM",
    "containerName": "iaasvmcontainerv2;testRG;testVM",
    "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
    "policyId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupPolicies/DefaultPolicy",
    "policyName": "DefaultPolicy"
  }
}
```

Dies bestätigt, dass der Schutz für die VM aktiviert ist und die erste Sicherung gemäß dem Richtlinienzeitplan ausgelöst wird.

### <a name="excluding-disks-in-azure-vm-backup"></a>Ausschließen von Datenträgern von der Sicherung eines virtuellen Azure-Computers

Azure Backup bietet auch eine Möglichkeit der selektiven Sicherung einer Teilmenge von Datenträgern auf dem virtuellen Azure-Computer. [Hier](selective-disk-backup-restore.md) finden Sie weitere Einzelheiten. Wenn Sie während des Aktivierens des Schutzes einzelne Datenträger selektiv sichern wollen, sollte der folgende Codeausschnitt [während des Aktivierens des Schutzes als Anforderungstext](#example-request-body) angegeben werden.

```json
{
"properties": {
    "protectedItemType": "Microsoft.Compute/virtualMachines",
    "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
    "policyId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupPolicies/DefaultPolicy",
    "extendedProperties":  {
      "diskExclusionProperties":{
          "diskLunList":[0,1],
          "isInclusionList":true
        }
    }
}
}
```

Im obigen Anforderungstext werden die zu sichernden Datenträger im Abschnitt „Erweiterte Eigenschaften“ angegeben.

|Eigenschaft  |Wert  |
|---------|---------|
|diskLunList     | Die Datenträger-LUN-Liste ist eine Liste der *LUNs von Datenträgern*. **Der Betriebssystemdatenträger wird immer gesichert und braucht nicht angegeben zu werden**.        |
|IsInclusionList     | Sollte auf **true** festgelegt sein, damit die LUNs während der Sicherung einbezogen werden. Ist diese Option auf **false** festgelegt, werden die oben erwähnten LUNs ausgeschlossen.         |

Soll also nur der Betriebssystemdatenträger gesichert werden, sollten _alle_ Datenträger mit Daten ausgeschlossen werden. In einfacheren Worten könnte man auch sagen, dass keine Datenträger mit Daten einbezogen werden sollten. Die Datenträger-LUN-Liste ist somit leer, und **IsInclusionList** ist **true**. Überlegen Sie sich auch, welche Möglichkeit, eine Teilmenge auszuwählen, einfacher ist: Einige Datenträger sollten immer ausgeschlossen werden, oder einige Datenträger sollten immer eingeschlossen werden. Wählen Sie die LUN-Liste und die boolesche Variable entsprechend aus.

## <a name="trigger-an-on-demand-backup-for-a-protected-azure-vm"></a>Auslösen einer bedarfsgesteuerten Sicherung für einen geschützten virtuellen Azure-Computer

Nachdem eine Azure-VM für die Sicherung konfiguriert wurde, werden Sicherungen gemäß dem Richtlinienzeitplan durchgeführt. Sie können warten, bis die erste geplante Sicherung durchgeführt wird, oder jederzeit eine bedarfsgesteuerte Sicherung auslösen. Die Aufbewahrung für bedarfsgesteuerte Sicherungen unterscheidet sich von der Aufbewahrung bei der Sicherungsrichtlinie und kann für ein bestimmtes Datum und eine bestimmte Uhrzeit angegeben werden. Wenn sie nicht angegeben ist, wird von 30 Tagen ab dem Tag, an dem die bedarfsgesteuerte Sicherung ausgelöst wurde, ausgegangen.

Das Auslösen einer bedarfsgesteuerten Sicherung ist ein *POST*-Vorgang.

```http
POST https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}/backup?api-version=2016-12-01
```

`{containerName}` und `{protectedItemName}` entsprechen der Erstellung [oben](#responses-to-get-operation). Für `{fabricName}` ist „Azure“ festgelegt. In unserem Beispiel ergibt dies Folgendes:

```http
POST https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/Microsoft.RecoveryServices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;iaasvmcontainerv2;testRG;testVM/backup?api-version=2016-12-01
```

### <a name="create-the-request-body-for-on-demand-backup"></a>Erstellen des Anforderungstexts für eine bedarfsgesteuerte Sicherung

Zum Auslösen einer bedarfsgesteuerten Sicherung werden die folgenden Komponenten des Anforderungstexts verwendet.

|Name  |type  |BESCHREIBUNG  |
|---------|---------|---------|
|properties     | [IaaSVMBackupRequest](/rest/api/backup/backups/trigger#iaasvmbackuprequest)        |BackupRequestResource-Eigenschaften         |

Die vollständige Liste mit Definitionen des Anforderungstexts und weitere Einzelheiten finden Sie im [Dokument zur REST-API zum Auslösen von Sicherungen für geschützte Elemente](/rest/api/backup/backups/trigger#request-body).

#### <a name="example-request-body-for-on-demand-backup"></a>Beispielanforderungstext für eine bedarfsgesteuerte Sicherung

Der folgende Anforderungstext definiert Eigenschaften, die zum Auslösen einer Sicherung für ein geschütztes Element erforderlich sind. Wenn keine Aufbewahrungsdauer angegeben ist, wird die Sicherung ab dem Zeitpunkt, zu dem der Sicherungsauftrag ausgelöst wurde, 30 Tage lang aufbewahrt.

```json
{
   "properties": {
    "objectType": "IaasVMBackupRequest",
    "recoveryPointExpiryTimeInUTC": "2018-12-01T02:16:20.3156909Z"
  }
}
```

### <a name="responses-for-on-demand-backup"></a>Antworten auf eine bedarfsgesteuerte Sicherung

Das Auslösen einer bedarfsgesteuerten Sicherung ist ein [asynchroner Vorgang](../azure-resource-manager/management/async-operations.md). Das bedeutet, dass in diesem Vorgang ein anderer Vorgang erstellt wird, der separat nachverfolgt werden muss.

Er gibt zwei Antworten zurück: „202 (Akzeptiert)“, wenn ein anderer Vorgang erstellt wird, und dann „200 (OK)“, wenn dieser Vorgang abgeschlossen ist.

|Name  |type  |BESCHREIBUNG  |
|---------|---------|---------|
|202 – Akzeptiert     |         |     Zulässig    |

#### <a name="example-responses-for-on-demand-backup"></a>Beispielantworten auf eine bedarfsgesteuerte Sicherung

Nachdem Sie die *POST*-Anforderung für eine bedarfsgesteuerte Sicherung gesendet haben, wird als erste Antwort „202 (Akzeptiert)“ mit einem location- oder Azure-async-Header zurückgegeben.

```http
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 60
Azure-AsyncOperation: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testVaultRG;testVM/protectedItems/vm;testRG;testVM/operationsStatus/b8daecaa-f8f5-44ed-9f18-491a9e9ba01f?api-version=2019-05-13
X-Content-Type-Options: nosniff
x-ms-request-id: 7885ca75-c7c6-43fb-a38c-c0cc437d8810
x-ms-client-request-id: 7df8e874-1d66-4f81-8e91-da2fe054811d; 7df8e874-1d66-4f81-8e91-da2fe054811d
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: 7885ca75-c7c6-43fb-a38c-c0cc437d8810
x-ms-routing-request-id: SOUTHINDIA:20180521T083541Z:7885ca75-c7c6-43fb-a38c-c0cc437d8810
Cache-Control: no-cache
Date: Mon, 21 May 2018 08:35:41 GMT
Location: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testVaultRG;testVM/protectedItems/vm;testRG;testVM/operationResults/b8daecaa-f8f5-44ed-9f18-491a9e9ba01f?api-version=2019-05-13
X-Powered-By: ASP.NET
```

Verfolgen Sie anschließend den resultierenden Vorgang mithilfe des location- oder Azure-AsyncOperation-Headers mit einem einfachen *GET*-Befehl nach.

```http
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM/operationsStatus/a0866047-6fc7-4ac3-ba38-fb0ae8aa550f?api-version=2019-05-13
```

Wenn der Vorgang abgeschlossen ist, wird „200 (OK)“ mit der ID des resultierenden Sicherungsauftrags im Antworttext zurückgegeben.

```json
HTTP/1.1 200 OK
Pragma: no-cache
X-Content-Type-Options: nosniff
x-ms-request-id: a8b13524-2c95-445f-b107-920806f385c1
x-ms-client-request-id: 5a63209d-3708-4e69-a75f-9499f4c8977c; 5a63209d-3708-4e69-a75f-9499f4c8977c
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-reads: 14995
x-ms-correlation-request-id: a8b13524-2c95-445f-b107-920806f385c1
x-ms-routing-request-id: SOUTHINDIA:20180521T083723Z:a8b13524-2c95-445f-b107-920806f385c1
Cache-Control: no-cache
Date: Mon, 21 May 2018 08:37:22 GMT
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET

{
  "id": "00000000-0000-0000-0000-000000000000",
  "name": "00000000-0000-0000-0000-000000000000",
  "status": "Succeeded",
  "startTime": "2018-05-21T08:35:40.9488967Z",
  "endTime": "2018-05-21T08:35:40.9488967Z",
  "properties": {
    "objectType": "OperationStatusJobExtendedInfo",
    "jobId": "7ddead57-bcb9-4269-ac31-6a1b57588700"
  }
}
```

Da es sich bei dem Sicherungsauftrag um einen Vorgang mit langer Ausführungsdauer handelt, muss er wie im [Dokument zum Überwachen von Aufträgen mit der REST-API](backup-azure-arm-userestapi-managejobs.md#tracking-the-job) erläutert nachverfolgt werden.

## <a name="modify-the-backup-configuration-for-a-protected-azure-vm"></a>Ändern der Sicherungskonfiguration für einen geschützten virtuellen Azure-Computer

### <a name="changing-the-policy-of-protection"></a>Ändern der Richtlinie für den Schutz

Zum Ändern der Richtlinie, mit der der virtuelle Computer geschützt wird, können Sie das gleiche Format wie beim [Aktivieren des Schutzes](#enabling-protection-for-the-azure-vm) verwenden. Geben Sie einfach die neue Richtlinien-ID im [Anforderungstext](#example-request-body) an, und senden Sie die Anforderung. Beispiel: Geben Sie zum Ändern der Richtlinie für „testVM“ von „DefaultPolicy“ in „ProdPolicy“ die ID von „ProdPolicy“ im Anforderungstext an.

```json
{
  "properties": {
    "protectedItemType": "Microsoft.Compute/virtualMachines",
    "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
    "policyId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupPolicies/ProdPolicy"
  }
}
```

Die Antwort erfolgt im gleichen Format wie beim [Aktivieren des Schutzes](#responses-to-create-protected-item-operation).

#### <a name="excluding-disks-during-azure-vm-protection"></a>Ausschließen von Datenträgern während des Schutzes der virtuellen Azure-Computer

Wenn der virtuelle Azure-Computer bereits gesichert wurde, können Sie die Liste der zu sichernden oder auszuschließenden Datenträger angeben, indem Sie die Schutzrichtlinie ändern. Bereiten Sie die Anforderung einfach im gleichen Format vor, wie beim [Ausschluss von Datenträgern während der Aktivierung des Schutzes](#excluding-disks-in-azure-vm-backup).

> [!IMPORTANT]
> Der obige Anforderungstext ist immer die endgültige Kopie der aus- oder einzuschließenden Datenträger für Daten. Dadurch wird die vorherige Konfiguration nicht *erweitert*. Beispiel: Wenn Sie den Schutz zunächst als „Datenträger 1 ausschließen“ aktualisieren und dann mit „Datenträger 2 ausschließen“ wiederholen, wird in den nachfolgenden Sicherungen *nur Datenträger 2 ausgeschlossen*, und Datenträger 1 wird eingeschlossen. Es ist immer die letzte Liste, die in den nachfolgenden Sicherungen ein-/ausgeschlossen wird.

Wenn Sie die aktuelle Liste der aus- oder eingeschlossenen Datenträger abrufen möchten, erhalten Sie die Informationen zu den geschützten Objekten wie [hier](/rest/api/backup/protected-items/get) erwähnt. Die Antwort liefert die Liste der Datenträger-LUNs und gibt an, ob sie ein- oder ausgeschlossen werden.

### <a name="stop-protection-but-retain-existing-data"></a>Beenden des Schutzes, jedoch Beibehalten vorhandener Daten

Um den Schutz für einen geschützten virtuellen Computer zu entfernen, die bereits gesicherten Daten jedoch beizubehalten, entfernen Sie die Richtlinie im Anforderungstext und senden die Anforderung. Nachdem die Zuordnung zur Richtlinie entfernt wurde, werden keine Sicherungen mehr ausgelöst und keine neuen Wiederherstellungspunkte erstellt.

```http
{
  "properties": {
    "protectedItemType": "Microsoft.Compute/virtualMachines",
    "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
    "policyId": ""
  }
}
```

Die Antwort erfolgt im gleichen Format wie beim [Auslösen einer bedarfsgesteuerten Sicherung](#example-responses-for-on-demand-backup). Der resultierende Auftrag muss wie im [Dokument zum Überwachen von Aufträgen mit der REST-API](backup-azure-arm-userestapi-managejobs.md#tracking-the-job) erläutert nachverfolgt werden.

### <a name="stop-protection-and-delete-data"></a>Beenden des Schutzes und Löschen der Daten

Um den Schutz für einen geschützten virtuellen Computer zu entfernen und auch die Sicherungsdaten zu löschen, führen Sie wie [hier](/rest/api/backup/protected-items/delete) beschrieben einen Löschvorgang durch.

Das Beenden des Schutzes und das Löschen der Daten ist ein *DELETE*-Vorgang.

```http
DELETE https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}?api-version=2019-05-13
```

`{containerName}` und `{protectedItemName}` entsprechen der Erstellung [oben](#responses-to-get-operation). `{fabricName}` ist „Azure“. In unserem Beispiel ergibt dies Folgendes:

```http
DELETE https://management.azure.com//Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/Microsoft.RecoveryServices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;iaasvmcontainerv2;testRG;testVM?api-version=2019-05-13
```

#### <a name="responses-for-delete-protection"></a>Antworten auf einen Vorgang zum Beenden des Schutzes und Löschen der Daten

Der *DELETE*-Vorgang für den Schutz ist ein [asynchroner Vorgang](../azure-resource-manager/management/async-operations.md). Das bedeutet, dass in diesem Vorgang ein anderer Vorgang erstellt wird, der separat nachverfolgt werden muss.

Er gibt zwei Antworten zurück: „202 (Akzeptiert)“, wenn ein anderer Vorgang erstellt wird, und dann „204 (NoContent)“, wenn dieser Vorgang abgeschlossen ist.

|Name  |type  |BESCHREIBUNG  |
|---------|---------|---------|
|204 Kein Inhalt     |         |  Kein Inhalt       |
|202 – Akzeptiert     |         |     Zulässig    |

> [!IMPORTANT]
> Zum Schutz vor versehentlichen Löschungen steht für den Recovery Services-Tresor ein [Feature für vorläufiges Löschen](use-restapi-update-vault-properties.md#soft-delete-state) zur Verfügung. Wenn der Status „Vorläufiges Löschen“ für den Tresor auf „Aktiviert“ festgelegt ist, werden die Daten durch den Löschvorgang nicht sofort gelöscht. Sie werden 14 Tage lang aufbewahrt und dann dauerhaft gelöscht. Für diesen 14-tägigen Zeitraum wird Ihnen der Speicher nicht in Rechnung gestellt. Informationen dazu, wie Sie den Löschvorgang rückgängig machen, finden Sie im Abschnitt zum [Rückgängigmachen des Löschens](#undo-the-deletion).

### <a name="undo-the-deletion"></a>Rückgängigmachen des Löschvorgangs

Das Rückgängigmachen eines versehentlichen Löschens ähnelt dem Erstellen des Sicherungselements. Nachdem der Löschvorgang rückgängig gemacht wurde, wird das Element aufbewahrt, aber es werden keine zukünftigen Sicherungen ausgelöst.

Das Rückgängigmachen einer Löschung ist ein *PUT*-Vorgang, der dem [Ändern der Richtlinie](#changing-the-policy-of-protection) und/oder dem [Aktivieren des Schutzes](#enabling-protection-for-the-azure-vm) sehr ähnlich ist. Geben Sie einfach die Absicht, den Löschvorgang rückgängig zu machen, mit der Variablen *isRehydrate* im [Anforderungstext](#example-request-body) an, und übermitteln Sie die Anforderung. Beispiel: Zum Rückgängigmachen der Löschung von „testVM“ verwenden Sie den folgenden Anforderungstext.

```http
{
  "properties": {
    "protectedItemType": "Microsoft.Compute/virtualMachines",
    "protectionState": "ProtectionStopped",
    "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
    "isRehydrate": true
  }
}
```

Die Antwort erfolgt im gleichen Format wie beim [Auslösen einer bedarfsgesteuerten Sicherung](#example-responses-for-on-demand-backup). Der resultierende Auftrag muss wie im [Dokument zum Überwachen von Aufträgen mit der REST-API](backup-azure-arm-userestapi-managejobs.md#tracking-the-job) erläutert nachverfolgt werden.

## <a name="next-steps"></a>Nächste Schritte

[Wiederherstellen von Daten aus der Sicherung eines virtuellen Azure-Computers](backup-azure-arm-userestapi-restoreazurevms.md).

Weitere Informationen zu den Azure Backup-REST-APIs finden Sie in den folgenden Dokumenten:

- [Azure Recovery Services-Anbieter – REST-API](/rest/api/recoveryservices/)
- [Erste Schritte mit der Azure-REST-API](/rest/api/azure/)
