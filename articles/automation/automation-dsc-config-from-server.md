---
title: Erstellen von Konfigurationen von vorhandenen Servern für Azure Automation State Configuration
description: In diesem Artikel erfahren Sie, wie Sie Konfigurationen aus vorhandenen Servern für Azure Automation State Configuration erstellen.
keywords: DSC,PowerShell,Konfiguration,Setup,Einrichtung
services: automation
ms.service: automation
ms.subservice: dsc
author: mgreenegit
ms.author: migreene
ms.date: 08/08/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 8fc5210ad64d534f8be015d324a454b513b7fccc
ms.sourcegitcommit: 02d443532c4d2e9e449025908a05fb9c84eba039
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108758337"
---
# <a name="create-configurations-from-existing-servers"></a>Erstellen von Konfigurationen aus vorhandenen Servern

> Gilt für: Windows PowerShell 5.1

Erstellen von Konfigurationen aus vorhandenen Servern kann eine Herausforderung darstellen.
Sie möchten nicht unbedingt *alle* Einstellungen, sondern nur die Einstellungen, die Sie interessieren.
Auch dann müssen Sie wissen, in welcher Reihenfolge die Einstellungen angewendet werden müssen, damit die Konfiguration erfolgreich angewendet werden kann.

> [!NOTE]
> Dieser Artikel bezieht sich auf eine Lösung, die von der Open Source-Community verwaltet wird.
> Support ist nur in Form der GitHub-Zusammenarbeit erhältlich, nicht von Microsoft.

## <a name="community-project-reversedsc"></a>Communityprojekt: ReverseDSC

Eine von der Community verwaltete Lösung namens [ReverseDSC](https://github.com/microsoft/reversedsc) wurde erstellt, um in diesem Bereich zu arbeiten, beginnend mit SharePoint.

Die Lösung baut auf der [SharePointDSC-Ressource](https://github.com/powershell/sharepointdsc) auf und erweitert sie so, dass das [Sammeln von Informationen](https://github.com/Microsoft/sharepointDSC.reverse#how-to-use) von vorhandenen Servern orchestriert wird, auf denen SharePoint ausgeführt wird.
Die neueste Version verfügt über mehrere [Extraktionsmodi](https://github.com/Microsoft/SharePointDSC.Reverse/wiki/Extraction-Modes), um die Stufe der einzuschließenden Informationen zu bestimmen.

Das Ergebnis der Verwendung der Lösung ist das Generieren von [Konfigurationsdaten](https://github.com/Microsoft/sharepointDSC.reverse#configuration-data) zur Verwendung mit SharePointDSC-Konfigurationsskripts.

Sobald die Datendateien einmal generiert sind, können Sie sie mit [DSC-Konfigurationsskripts](/powershell/scripting/dsc/overview/overview) verwenden, um MOF-Dateien zu generieren und die [MOF-Dateien in Azure Automation hochzuladen](./tutorial-configure-servers-desired-state.md#create-and-upload-a-configuration-to-azure-automation).
Danach registrieren Sie Ihre Server entweder von [lokal](./automation-dsc-onboarding.md#enable-physicalvirtual-linux-machines) oder [in Azure](./automation-dsc-onboarding.md#enable-azure-vms) um Konfigurationen abzurufen.

Um ReverseDSC zu testen, besuchen Sie den [PowerShell-Katalog](https://www.powershellgallery.com/packages/ReverseDSC/), und laden Sie die Lösung herunter, oder klicken Sie auf „Projektwebsite“, um die [Dokumentation](https://github.com/Microsoft/sharepointDSC.reverse) anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

- Eine Einführung in PowerShell DSC finden Sie unter [Windows PowerShell DSC – Übersicht](/powershell/scripting/dsc/overview/overview).
- Erfahren Sie mehr über PowerShell DSC-Ressourcen in [DSC-Ressourcen](/powershell/scripting/dsc/resources/resources).
- Weitere Informationen zur Konfiguration des lokalen Konfigurations-Managers finden Sie unter [Konfigurieren des lokalen Konfigurations-Managers](/powershell/scripting/dsc/managing-nodes/metaconfig).
