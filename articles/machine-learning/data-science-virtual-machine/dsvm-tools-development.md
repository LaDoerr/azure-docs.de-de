---
title: Entwicklungstools
titleSuffix: Azure Data Science Virtual Machine
description: Hier erfahren Sie mehr über die in Data Science Virtual Machine verfügbaren Tools und integrierten Entwicklungsumgebungen.
keywords: Data Science-Tools, virtuelle Computer für Data Science, Tools für Data Science, Linux Data Science
services: machine-learning
ms.service: data-science-vm
author: timoklimmer
ms.author: tklimmer
ms.topic: conceptual
ms.date: 05/12/2021
ms.openlocfilehash: efe57637347b5886827f5da443acd9240974d687
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110070964"
---
# <a name="development-tools-on-the-azure-data-science-virtual-machine"></a>Entwicklungstools in Azure Data Science Virtual Machine

Auf der Data Science Virtual Machine (DVSM) sind mehrere beliebte Tools in einer hochgradig produktiven integrierten Entwicklungsumgebung (Integrated Development Environment, IDE) gebündelt. Im Anschluss sind einige der Tools aufgeführt, die auf der DSVM zur Verfügung stehen:

## <a name="visual-studio-community-edition"></a>Visual Studio Community Edition

| Category | Wert |
|--|--|
| Was ist das? | Universelle IDE |
| Unterstützte DSVM-Versionen | Windows Server 2019: Visual Studio 2019 |
| Typische Verwendung | Softwareentwicklung |
| Konfiguration und Installation auf der DSVM | Data Science-Workload (Python- und R-Tools), Azure-Workload (Hadoop, Data Lake), Node.js, SQL Server-Tools, [Azure Machine Learning für Visual Studio Code](https://github.com/Microsoft/vs-tools-for-ai) |
| Verwenden und Ausführen | Desktopverknüpfung (`C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\Common7\IDE\devenv.exe`). Öffnen Sie Visual Studio auf der Benutzeroberfläche über das Desktopsymbol oder das **Startmenü**. Suchen Sie nach Programmen (Windows-Logo-Taste+S) und anschließend nach **Visual Studio**. Von dort aus können Sie Projekte in Sprachen wie C#, Python, R und Node.js erstellen. |

> [!NOTE]
> Es kann eine Meldung angezeigt werden, die besagt, dass der Evaluierungszeitraum abgelaufen ist. Geben Sie die Anmeldeinformationen Ihres Microsoft-Kontos ein. Oder erstellen Sie ein neues kostenloses Konto für den Zugriff auf Visual Studio Community.

## <a name="visual-studio-code"></a>Visual Studio Code 

| Category | Wert |
|--|--|
| Was ist das? | Universelle IDE |
| Unterstützte DSVM-Versionen | Windows, Linux |
| Typische Verwendung | Code-Editor und Git-Integration |
| Verwenden und Ausführen | Desktopverknüpfung (`C:\Program Files (x86)\Microsoft VS Code\Code.exe`) unter Windows, Desktopverknüpfung oder Terminal (`code`) unter Linux |

## <a name="rstudio-desktop"></a>RStudio Desktop

| Category | Wert |
|--|--|
| Was ist das? | Client-IDE für R |
| Unterstützte DSVM-Versionen | Windows, Linux |
| Typische Verwendung | R-Entwicklung |
| Verwenden und Ausführen | Desktopverknüpfung (`C:\Program Files\RStudio\bin\rstudio.exe`) unter Windows, Desktopverknüpfung (`/usr/bin/rstudio`) unter Linux |

## <a name="pycharm"></a>PyCharm

| Category | Wert |
|--|--|
| Was ist das? | Client-IDE für Python |
| Unterstützte DSVM-Versionen | Windows 2019, Ubuntu 18.04 |
| Typische Verwendung | Python-Entwicklung |
| Verwenden und Ausführen | Desktopverknüpfung (`C:\Program Files\tk`) unter Windows Desktopverknüpfung (`/usr/bin/pycharm`) unter Linux |
