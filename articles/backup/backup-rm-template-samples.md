---
title: Azure-Ressourcen-Manager-Vorlagen
description: Azure Resource Manager-Vorlagen für die Verwendung mit Recovery Services-Tresoren und Azure Backup-Features
ms.topic: sample
ms.date: 01/31/2019
ms.custom: mvc
ms.openlocfilehash: 4e3ef31252956532be38d987ffa40d1581377b1a
ms.sourcegitcommit: 91fdedcb190c0753180be8dc7db4b1d6da9854a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2021
ms.locfileid: "112293642"
---
# <a name="azure-resource-manager-templates-for-azure-backup"></a>Azure Resource Manager-Vorlagen für Azure Backup

Die folgende Tabelle enthält Links zu Azure Resource Manager-Vorlagen für die Verwendung mit Recovery Services-Tresoren und Azure Backup-Features. Weitere Informationen zur JSON-Syntax und zu Eigenschaften finden Sie unter [Microsoft.RecoveryServices resource types](/azure/templates/microsoft.recoveryservices/allversions) (Microsoft.RecoveryServices-Ressourcentypen).

| Vorlage | BESCHREIBUNG |
|---|---|
|**Recovery Services-Tresor** | |
| [Erstellen eines Recovery Services-Tresors](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.recoveryservices/recovery-services-vault-create)| Erstellen Sie einen Recovery Services-Tresor. Der Tresor kann für Azure Backup und für Azure Site Recovery verwendet werden. |
|**Sichern virtueller Computer**| |
| [Sichern von Resource Manager-VMs](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.recoveryservices/recovery-services-backup-vms) | Verwenden Sie den vorhandenen Recovery Services-Tresor und die vorhandene Sicherungsrichtlinie, um Resource Manager-VMs in der gleichen Ressourcengruppe zu sichern.|
| [Sichern von IaaS-VMs im Recovery Services-Tresor](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.recoveryservices/recovery-services-backup-classic-resource-manager-vms) | Vorlage zum Sichern klassischer und Resource Manager-basierter virtueller Computer. |
| [Erstellen einer wöchentlichen Sicherungsrichtlinie für IaaS-VMs](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.recoveryservices/recovery-services-weekly-backup-policy-create) | Vorlage zum Erstellen eines Recovery Services-Tresors und einer wöchentlichen Sicherungsrichtlinie zum Sichern klassischer und Resource Manager-basierter virtueller Computer|
| [Erstellen einer täglichen Sicherungsrichtlinie für IaaS-VMs](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.recoveryservices/recovery-services-daily-backup-policy-create) | Vorlage zum Erstellen eines Recovery Services-Tresors und einer täglichen Sicherungsrichtlinie zum Sichern klassischer und Resource Manager-basierter virtueller Computer|
| [Bereitstellen eines virtuellen Windows Server-Computers mit aktivierter Sicherung](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.recoveryservices/recovery-services-create-vm-and-configure-backup) | Vorlage zum Erstellen eines virtuellen Windows Server-Computers und eines Recovery Services-Tresors mit aktivierter Standardsicherungsrichtlinie.|
|**Überwachen von Sicherungsaufträgen** |  |
| [Verwenden von Azure Monitor-Protokollen mit Azure Backup](https://github.com/Azure/azure-quickstart-templates/tree/master/demos/backup-oms-monitoring) | Vorlage zum Bereitstellen von Azure Monitor-Protokollen für Azure Backup zur Überwachung von Sicherungs- und Wiederherstellungsaufträgen, Sicherungswarnungen sowie zur Überwachung des Cloudspeichers, der in Ihren Recovery Services-Tresoren verwendet wird|
|**Sichern von SQL Server auf einem virtuellen Azure-Computer** |  |
| [Sichern von SQL Server auf einem virtuellen Azure-Computer](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.recoveryservices/recovery-services-vm-workload-backup) | Vorlage zum Erstellen eines Recovery Services-Tresors und einer workloadspezifischen Sicherungsrichtlinie. Der virtuelle Computer wird beim Azure Backup-Dienst registriert, und der Schutz wird auf diesem virtuellen Computer konfiguriert. Gegenwärtig können nur SQL-Katalogimages verwendet werden. |
|**Sichern von Azure-Dateifreigaben** |  |
| [Sichern von Azure-Dateifreigaben](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.recoveryservices/recovery-services-backup-file-share) | Mit dieser Vorlage wird der Schutz für eine vorhandene Azure-Dateifreigabe konfiguriert, indem entsprechende Details für den Recovery Services-Tresor und die Sicherungsrichtlinie angegeben werden. Optional erstellt sie einen neuen Recovery Services-Tresor und eine neue Sicherungsrichtlinie und registriert das Speicherkonto, das die Dateifreigabe enthält, im Recovery Services-Tresor. |
|   |   |
