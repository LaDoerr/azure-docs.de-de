---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 07/16/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 06f337cb524b1ca158baf0627ccdfa88c4ae4e40
ms.sourcegitcommit: e2fa73b682a30048907e2acb5c890495ad397bd3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2021
ms.locfileid: "114387862"
---
|Name |BESCHREIBUNG |Richtlinien |Version |
|---|---|---|---|
|[Computer mit unsicheren Einstellungen zur Kennwortsicherheit überwachen](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Guest%20Configuration/GuestConfiguration_WindowsPasswordSettingsAINE.json) |Diese Initiative sorgt für die Bereitstellung der erforderlichen Richtlinienkomponenten und überwacht Computer mit unsicheren Einstellungen für die Kennwortsicherheit. Weitere Informationen zu Richtlinien für die Gastkonfiguration finden Sie unter [https://aka.ms/gcpol](../../../../articles/governance/policy/concepts/guest-configuration.md). |9 |1.0.0 |
|[Voraussetzungen zum Aktivieren der Gastkonfigurationsrichtlinien auf VMs bereitstellen](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Guest%20Configuration/GuestConfiguration_Prerequisites.json) |Mit dieser Initiative wird eine systemseitig zugewiesene verwaltete Identität hinzugefügt und die plattformgerechte Erweiterung für die Gastkonfiguration auf VMs bereitgestellt, die zur Überwachung durch Gastkonfigurationsrichtlinien berechtigt sind. Dies ist eine Voraussetzung für alle Gastkonfigurationsrichtlinien und erfordert eine Zuweisung zum Richtlinienzuweisungsbereich, bevor eine Gastkonfigurationsrichtlinie verwendet werden kann. Weitere Informationen zur Gastkonfiguration finden Sie unter [https://aka.ms/gcpol](../../../../articles/governance/policy/concepts/guest-configuration.md). |4 |1.0.0 |
|[\[Vorschau\]: Windows-Computer müssen die Anforderungen für die Azure Compute-Sicherheitsbaseline erfüllen](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Guest%20Configuration/GuestConfiguration_AzureBaseline.json) |Diese Initiative überwacht Windows-Computer mit Einstellungen, die nicht die Azure Compute-Sicherheitsbaseline erfüllen. Ausführliche Informationen finden Sie unter [https://aka.ms/gcpol](../../../../articles/governance/policy/concepts/guest-configuration.md). |29 |2.0.1-preview |