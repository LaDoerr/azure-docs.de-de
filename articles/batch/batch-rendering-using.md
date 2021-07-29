---
title: Verwenden von Renderingfunktionen
description: Hier erfahren Sie mehr über die Verwendung von Azure Batch-Renderingfunktionen Testen Sie die Batch Explorer-Anwendung entweder direkt oder über ein Plug-In einer Clientanwendung.
author: mscurrell
ms.author: markscu
ms.date: 03/12/2020
ms.topic: how-to
ms.openlocfilehash: d164eb0250c98573e781b87be339748900c4920b
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110452064"
---
# <a name="using-azure-batch-rendering"></a>Verwenden des Renderings mit Azure Batch

> [!IMPORTANT]
> Das Rendern von VM-Images und die nutzungsbasierte Lizenzierung wurden [als veraltet markiert und werden am 29. Februar 2024 eingestellt](https://azure.microsoft.com/updates/azure-batch-rendering-vm-images-licensing-will-be-retired-on-29-february-2024/). Um mit Batch zu rendern, [sollten ein benutzerdefiniertes VM-Image und eine Standardanwendungslizenz verwendet werden](batch-rendering-functionality.md#batch-pools-using-custom-vm-images-and-standard-application-licensing).

Es gibt mehrere Möglichkeiten, das Rendering mit Azure Batch zu verwenden:

* APIs:
  * Schreiben Sie Code, indem Sie eine der Batch-APIs verwenden.  Entwickler können Azure Batch-Funktionen in ihre vorhandenen Anwendungen oder den Workflow integrieren (in der Cloud oder lokal).
* Befehlszeilentools:
  * Die [Azure-Befehlszeile](/cli/azure/) oder [PowerShell](/powershell/azure/) können verwendet werden, um Skripts für die Batch-Nutzung zu erstellen.
  * Besonders die [Unterstützung für die Batch-CLI-Vorlage](./batch-cli-templates.md) vereinfacht die Erstellung von Pools und die Übermittlung von Aufträgen.
* Batch Explorer-Benutzeroberfläche:
  * [Batch Explorer](https://github.com/Azure/BatchLabs) ist ein plattformübergreifendes Clienttool, mit dem Batch-Konten verwaltet und überwacht werden können.
  * Für die einzelnen Renderinganwendungen werden verschiedene Pool- und Auftragsvorlagen bereitgestellt, die genutzt werden können, um auf einfache Weise Pools zu erstellen und Aufträge zu übermitteln.  Auf der Benutzeroberfläche der Anwendung sind einige Vorlagen aufgeführt, und auf die Vorlagendateien wird auf GitHub zugegriffen.
  * Benutzerdefinierte Vorlagen können ganz neu erstellt werden, oder die bereitgestellten Vorlagen auf GitHub können kopiert und geändert werden.
* Plug-Ins für Clientanwendung:
  * Es sind Plug-Ins verfügbar, mit denen das Batch-Rendering direkt in den Anwendungen für den Cliententwurf und die Modellierung verwendet werden kann.  Die Plug-Ins rufen hauptsächlich die Batch Explorer-Anwendung mit Kontextinformationen zum aktuellen 3D-Modell auf und enthalten Features für die Verwaltung von Medienobjekten.

Die beste Möglichkeit zum Ausprobieren des Renderings mit Azure Batch und die einfachste Vorgehensweise für Endbenutzer, bei denen es sich nicht um Entwickler oder Azure-Experten handelt, ist die Verwendung der Batch Explorer-Anwendung – entweder direkt oder per Aufruf über ein Plug-In der Clientanwendung.

## <a name="using-batch-explorer"></a>Verwenden von Batch Explorer

Batch Explorer-[Downloads sind für Windows, OSX und Linux verfügbar](https://azure.github.io/BatchExplorer/).

### <a name="using-templates-to-create-pools-and-run-jobs"></a>Verwenden von Vorlagen zum Erstellen von Pools und Ausführen von Aufträgen

Für die Nutzung mit Batch Explorer ist ein umfassender Satz mit Vorlagen verfügbar, mit denen das Erstellen von Pools und Übermitteln von Aufträgen für die verschiedenen Renderinganwendungen vereinfacht wird. Es müssen nicht alle Eigenschaften angegeben werden, die zum Erstellen von Pools, Aufträgen und Aufgaben direkt mit Batch erforderlich sind.  Die in Batch Explorer verfügbaren Vorlagen werden in einem [GitHub-Repository](https://github.com/Azure/BatchExplorer-data/tree/master/ncj) gespeichert und können dort angezeigt werden.

![Batch Explorer-Katalog](./media/batch-rendering-using/batch-explorer-gallery.png)

Es werden Vorlagen bereitgestellt, mit denen alle Anwendungen abgedeckt sind, die in den Rendering-VM-Images auf dem Marketplace vorhanden sind.  Für jede Anwendung sind mehrere Vorlagen vorhanden, z.B. Poolvorlagen für CPU- und GPU-Pools, Windows- und Linux-Pools. Zu den Auftragsvorlagen gehören Vorlagen für das Blender-Rendering (vollständiger Frame oder Kacheln) und das verteilte V-Ray-Rendering. Die bereitgestellten Vorlagen werden im Laufe der Zeit erweitert, um andere Batch-Funktionen abzudecken, z.B. die automatische Skalierung von Pools.

Es ist auch möglich, benutzerdefinierte Vorlagen zu erstellen – entweder ganz neu oder durch das Ändern der bereitgestellten Vorlagen. Benutzerdefinierte Vorlagen können verwendet werden, indem im Abschnitt „Katalog“ von Batch Explorer die Option „Lokale Vorlagen“ gewählt wird.

### <a name="file-system-and-data-movement"></a>Dateisystem und Datenverschiebung

Im Abschnitt „Daten“ in Batch Explorer können Dateien zwischen einem lokalen Dateisystem und Azure Storage-Konten kopiert werden.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über die [Verwendung von Renderinganwendungen mit Batch](batch-rendering-applications.md).
* Erfahren Sie mehr über [Optionen für die Speicherung und Datenverschiebung zum Rendern von Medienobjekt- und Ausgabedateien](batch-rendering-storage-data-movement.md).
