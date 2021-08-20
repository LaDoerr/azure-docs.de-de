---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 08/13/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 523883c9ec7ba9ab071784210b5ef9a7aab57815
ms.sourcegitcommit: 2d412ea97cad0a2f66c434794429ea80da9d65aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2021
ms.locfileid: "122356361"
---
|Name<br /><sub>(Azure-Portal)</sub> |BESCHREIBUNG |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[\[Vorschau:\] Azure Spring Cloud-Instanzen überwachen, bei denen die verteilte Ablaufverfolgung nicht aktiviert ist](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0f2d8593-4667-4932-acca-6a9f187af109) |Die Tools für die verteilte Ablaufverfolgung in Azure Spring Cloud ermöglichen das Debuggen und Überwachen der komplexen Verbindungen zwischen Microservices in einer Anwendung. Die Tools für die verteilte Ablaufverfolgung sollten aktiviert und in einem fehlerfreien Zustand sein. |Audit, Disabled |[1.0.0-preview](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Platform/Spring_DistributedTracing_Audit.json) |
|[Azure Spring Cloud muss Netzwerkinjektion verwenden](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Faf35e2a4-ef96-44e7-a9ae-853dd97032c4) |Azure Spring Cloud-Instanzen sollten die VNET-Injektion für folgende Zwecke verwenden: 1. Azure Spring Cloud vom Internet isolieren. 2. Die Interaktion von Azure Spring Cloud mit Systemen in lokalen Rechenzentren oder im Azure-Dienst in anderen VNETs ermöglichen. 3. Kunden die Steuerung der eingehenden und ausgehenden Netzwerkkommunikation für Azure Spring Cloud erlauben |Audit, Disabled, Deny |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Platform/Spring_VNETEnabled_Audit.json) |
