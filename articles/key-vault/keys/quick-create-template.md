---
title: 'Azure-Schnellstart: Erstellen eines Azure-Schlüsseltresors und eines Schlüssels per Azure Resource Manager-Vorlage | Microsoft-Dokumentation'
description: In dieser Schnellstartanleitung wird veranschaulicht, wie Sie Azure-Schlüsseltresore erstellen und den Tresoren Schlüssel hinzufügen, indem Sie die Azure Resource Manager-Vorlage (ARM-Vorlage) verwenden.
services: key-vault
author: sebansal
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: keys
ms.topic: quickstart
ms.custom: mvc,subject-armqs, devx-track-azurepowershell
ms.date: 10/14/2020
ms.author: sebansal
ms.openlocfilehash: 8e8a0e931a2c33883bf18ad92a367110d9be093e
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2021
ms.locfileid: "128572961"
---
# <a name="quickstart-create-an-azure-key-vault-and-a-key-by-using-arm-template"></a>Schnellstart: Erstellen eines Azure-Schlüsseltresors und eines Schlüssels mithilfe einer ARM-Vorlage 

[Azure Key Vault](../general/overview.md) ist ein Clouddienst, der einen sicheren Speicher für Geheimnisse bereitstellt, z. B. für Schlüssel, Kennwörter, Zertifikate usw. In dieser Schnellstartanleitung geht es um die Bereitstellung einer Azure Resource Manager-Vorlage (ARM-Vorlage) zum Erstellen eines Schlüsseltresors und eines Schlüssels.


## <a name="prerequisites"></a>Voraussetzungen

Führen Sie für diesen Artikel die folgenden Schritte aus:

- Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.
- Dem Benutzer muss eine integrierte Azure-Rolle zugewiesen sein. Empfohlen wird die Rolle **Mitwirkender**. [Weitere Informationen finden Sie hier](../../role-based-access-control/role-assignments-portal.md)
- Ihre Azure AD-Benutzerobjekt-ID wird von der Vorlage zum Konfigurieren von Berechtigungen benötigt. Die folgende Prozedur ruft die Objekt-ID (GUID) ab.

    1. Führen Sie den folgenden Azure PowerShell- oder Azure CLI-Befehl aus, indem Sie die Option **Ausprobieren** wählen und das Skript anschließend in den Shell-Bereich einfügen. Klicken Sie zum Einfügen des Skripts mit der rechten Maustaste auf die Shell, und wählen Sie **Einfügen** aus.

        # <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/CLI)
        ```azurecli-interactive
        echo "Enter your email address that is used to sign in to Azure:" &&
        read upn &&
        az ad user show --id $upn --query "objectId" &&
        echo "Press [ENTER] to continue ..."
        ```

        # <a name="powershell"></a>[PowerShell](#tab/PowerShell)
        ```azurepowershell-interactive
        $upn = Read-Host -Prompt "Enter your email address used to sign in to Azure"
        (Get-AzADUser -UserPrincipalName $upn).Id
        Write-Host "Press [ENTER] to continue..."
        ```

        ---

    1. Notieren Sie sich die Objekt-ID. Sie benötigen sie im nächsten Abschnitt dieser Schnellstartanleitung.

## <a name="review-the-template"></a>Überprüfen der Vorlage

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "vaultName": {
      "type": "string",
      "metadata": {
        "description": "The name of the key vault to be created."
      }
    },
    "skuName": {
      "type": "string",
      "defaultValue": "Standard",
      "allowedValues": [
        "Standard",
        "Premium"
      ],
      "metadata": {
        "description": "The SKU of the vault to be created."
      }
    },
    "keyName": {
      "type": "string",
      "metadata": {
        "description": "The name of the key to be created."
      }
    },
    "keyType": {
      "type": "string",
      "metadata": {
        "description": "The JsonWebKeyType of the key to be created."
      }
    },
    "keyOps": {
      "type": "array",
      "defaultValue": [],
      "metadata": {
        "description": "The permitted JSON web key operations of the key to be created."
      }
    },
    "keySize": {
      "type": "int",
      "defaultValue": 2048,
      "metadata": {
        "description": "The size in bits of the key to be created."
      }
    },
    "curveName": {
      "type": "string",
      "defaultValue": "",
      "metadata": {
        "description": "The JsonWebKeyCurveName of the key to be created."
      }
    }
  },
  "resources": [
    {
      "type": "Microsoft.KeyVault/vaults",
      "apiVersion": "2019-09-01",
      "name": "[parameters('vaultName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "enableRbacAuthorization": false,
        "enableSoftDelete": false,
        "enabledForDeployment": false,
        "enabledForDiskEncryption": false,
        "enabledForTemplateDeployment": false,
        "tenantId": "[subscription().tenantId]",
        "accessPolicies": [],
        "sku": {
          "name": "[parameters('skuName')]",
          "family": "A"
        },
        "networkAcls": {
          "defaultAction": "Allow",
          "bypass": "AzureServices"
        }
      }
    },
    {
      "type": "Microsoft.KeyVault/vaults/keys",
      "apiVersion": "2019-09-01",
      "name": "[concat(parameters('vaultName'), '/', parameters('keyName'))]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[resourceId('Microsoft.KeyVault/vaults', parameters('vaultName'))]"
      ],
      "properties": {
        "kty": "[parameters('keyType')]",
        "keyOps": "[parameters('keyOps')]",
        "keySize": "[parameters('keySize')]",
        "curveName": "[parameters('curveName')]"
      }
    }
  ],
  "outputs": {
    "proxyKey": {
      "type": "object",
      "value": "[reference(resourceId('Microsoft.KeyVault/vaults/keys', parameters('vaultName'), parameters('keyName')))]"
    }
  }
}
```

In der Vorlage sind zwei Ressourcen definiert:

- [Microsoft.KeyVault/vaults](/azure/templates/microsoft.keyvault/vaults)
- Microsoft.KeyVault/vaults/keys

Weitere Vorlagenbeispiele für Azure Key Vault finden Sie in [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Keyvault&pageNumber=1&sort=Popular).

## <a name="parameters-and-definitions"></a>Parameter und Definitionen

|Parameter  |Definition  |
|---------|---------|
|**keyOps**  | Gibt Vorgänge an, die unter Verwendung des Schlüssels ausgeführt werden können. Ohne Angabe dieses Parameters können alle Vorgänge ausgeführt werden. Zulässige Werte für diesen Parameter sind eine durch Kommas getrennte Liste mit Schlüsselvorgängen gemäß [JWK-Spezifikation (JSON Web Key)](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-41): <br> `["sign", "verify", "encrypt", "decrypt", " wrapKey", "unwrapKey"]` |
|**CurveName**  |  Name der elliptischen Kurve für den EC-Schlüsseltyp. Weitere Informationen finden Sie unter [JsonWebKeyCurveName](/rest/api/keyvault/createkey/createkey#jsonwebkeycurvename). |
|**Kty**  |  Die Art des zu erstellenden Schlüssels. Gültige Werte finden Sie unter [JsonWebKeyType](/rest/api/keyvault/createkey/createkey#jsonwebkeytype). |
|**Tags** | Anwendungsspezifische Metadaten in Form von Schlüssel-Wert-Paaren.  |
|**nbf**  |  Gibt die Zeit (als DateTime-Objekt) an, vor der der Schlüssel nicht verwendet werden kann. Das Format ist ein Unix-Zeitstempel (die Anzahl von Sekunden nach der Unix-Epoche am 1. Januar 1970 in UTC).  |
|**exp**  |  Gibt die Ablaufzeit als DateTime-Objekt an. Das Format ist ein Unix-Zeitstempel (die Anzahl von Sekunden nach der Unix-Epoche am 1. Januar 1970 in UTC). |

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage
Sie können auch das [Azure-Portal](../../azure-resource-manager/templates/deploy-portal.md), Azure PowerShell, die Azure CLI oder die REST-API verwenden. Informationen zu Bereitstellungsmethoden finden Sie unter [Bereitstellen von Vorlagen](../../azure-resource-manager/templates/deploy-powershell.md).

## <a name="review-deployed-resources"></a>Überprüfen der bereitgestellten Ressourcen

Sie können entweder das Azure-Portal nutzen, um den Schlüsseltresor und den Schlüssel zu überprüfen, oder das unten angegebene Azure CLI- oder Azure PowerShell-Skript, um den erstellten Schlüssel aufzulisten.

# <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/CLI)

```azurecli-interactive
echo "Enter your key vault name:" &&
read keyVaultName &&
az keyvault key list --vault-name $keyVaultName &&
echo "Press [ENTER] to continue ..."
```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell-interactive
$keyVaultName = Read-Host -Prompt "Enter your key vault name"
Get-AzKeyVaultKey -vaultName $keyVaultName
Write-Host "Press [ENTER] to continue..."
```

---

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Andere Schnellstartanleitungen und Tutorials für Key Vault bauen auf dieser Schnellstartanleitung auf. Falls Sie mit weiteren Schnellstartanleitungen und Tutorials fortfahren möchten, sollten Sie die Ressourcen nicht bereinigen.
Wenn Sie die Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe. Dadurch werden die Key Vault-Instanz und die dazugehörigen Ressourcen gelöscht. Die Ressourcengruppe kann über die Azure-Befehlszeilenschnittstelle oder mithilfe von Azure PowerShell gelöscht werden:

# <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/CLI)

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName &&
echo "Press [ENTER] to continue ..."
```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName
Write-Host "Press [ENTER] to continue..."
```

---

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie mithilfe einer ARM-Vorlage einen Schlüsseltresor und einen Schlüssel erstellt und die Bereitstellung überprüft. Weitere Informationen zu Key Vault und Azure Resource Manager finden Sie in den folgenden Artikeln:

- [Was ist der Azure-Schlüsseltresor?](../general/overview.md)
- Lesen Sie weitere Informationen zu [Azure Resource Manager](../../azure-resource-manager/management/overview.md).
- [Azure Key Vault-Sicherheitsübersicht](../general/security-features.md)
