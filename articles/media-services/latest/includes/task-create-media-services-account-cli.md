---
author: IngridAtMicrosoft
ms.service: media-services
ms.topic: include
ms.date: 08/17/2020
ms.author: inhenkel
ms.custom: CLI, devx-track-azurecli
ms.openlocfilehash: c52dfe33de673f6ad529a3065b9567a685acfad5
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110062546"
---
<!--Create a media services account -->

Der folgende Azure CLI-Befehl erstellt ein neues Media Services-Konto. Sie können die folgenden Werte ersetzen: `your-media-services-account-name`, `your-storage-account-name` und `your-resource-group-name`. Der Befehl setzt voraus, dass Sie bereits eine Ressourcengruppe und ein Speicherkonto erstellt haben.

```azurecli-interactive
az ams account create --name <your-media-services-account-name> -g <your-resource-group-name> --storage-account <your-storage-account-name> -l <your-region>
```

JSON-Beispielantwort:

```json
{
  "id": "/subscriptions/00000000-0000-0000-000000000000/resourceGroups/your-resource-group-name/providers/Microsoft.Media/mediaservices/your-media-services-account-name",
  "location": "your-region",
  "mediaServiceId": "the-media-services-account-id",
  "name": "your-media-services-account-name",
  "resourceGroup": "your-resource-group-name",
  "storageAccounts": [
    {
      "id": "/subscriptions/00000000-0000-0000-000000000000/resourceGroups/your-resource-group-name/providers/Microsoft.Storage/storageAccounts/your-storage-account-name",
      "resourceGroup": "your-resource-group-name",
      "type": "Primary"
    }
  ],
  "tags": null,
  "type": "Microsoft.Media/mediaservices"
}
```
