---
title: Azure Virtual Desktop-Umgebung – Azure
description: Lernen Sie die grundlegenden Elemente einer Azure Virtual Desktop-Umgebung kennen, etwa Hostpools und App-Gruppen.
author: Heidilohr
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: helohr
manager: femila
ms.openlocfilehash: b6162657520f91168f46c43c1d6d7f5cbfbe6d38
ms.sourcegitcommit: b044915306a6275c2211f143aa2daf9299d0c574
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2021
ms.locfileid: "113031733"
---
# <a name="azure-virtual-desktop-environment"></a>Azure Virtual Desktop-Umgebung

>[!IMPORTANT]
>Dieser Inhalt gilt für Azure Virtual Desktop mit Azure Virtual Desktop-Objekten für Azure Resource Manager. Wenn Sie Azure Virtual Desktop (klassisch) ohne Azure Resource Manager-Objekte verwenden, finden Sie weitere Informationen in [diesem Artikel](./virtual-desktop-fall-2019/environment-setup-2019.md).

Azure Virtual Desktop ist ein Dienst, mit dem Benutzer einfachen und sicheren Zugriff auf ihre virtualisierten Desktops und RemoteApps erhalten. In diesem Thema wird die allgemeine Struktur der Azure Virtual Desktop-Umgebung näher erläutert.

## <a name="host-pools"></a>Hostpools

Ein Hostpool ist eine Sammlung mit virtuellen Azure-Computern, die unter Azure Virtual Desktop als Sitzungshosts registriert werden, wenn Sie den Azure Virtual Desktop-Agent ausführen. Alle virtuellen Computer von Sitzungshosts in einem Hostpool sollten über dasselbe Image erstellt werden, um eine einheitliche Benutzeroberfläche zu erhalten.

Bei einem Hostpool kann es sich um einen von zwei Typen handeln:

- Persönlich, wobei jeder Sitzungshost einzelnen Benutzern zugewiesen wird.
- In einem Pool, wobei Sitzungshosts Verbindungen von jedem Benutzer akzeptieren können, der im Hostpool für eine App-Gruppe autorisiert ist.

Sie können für den Hostpool zusätzliche Eigenschaften festlegen, um Folgendes zu ändern: Verhalten beim Lastenausgleich, Anzahl von Sitzungen, die für einen Sitzungshost jeweils verwendet werden können, und welche Schritte Benutzer für Sitzungshosts im Hostpool ausführen können, während sie an ihren Azure Virtual Desktop-Sitzungen angemeldet sind. Sie steuern die Ressourcen, die für Benutzer veröffentlicht werden, über App-Gruppen.

## <a name="app-groups"></a>App-Gruppen

Eine App-Gruppe ist eine logische Gruppierung von Anwendungen, die auf Sitzungshosts im Hostpool installiert sind. Eine App-Gruppe kann einen von zwei Typen aufweisen:

- RemoteApp, über die Benutzer auf die RemoteApps zugreifen, die Sie einzeln auswählen und für die App-Gruppe veröffentlichen
- Desktop, über den Benutzer auf den vollständigen Desktop zugreifen

Standardmäßig wird jeweils automatisch eine Desktop-App-Gruppe (mit dem Namen „Desktopanwendungsgruppe“) erstellt, wenn Sie einen Hostpool erstellen. Sie können diese App-Gruppe jederzeit entfernen. Es ist aber nicht möglich, im Hostpool eine weitere Desktop-App-Gruppe zu erstellen, während eine Desktop-App-Gruppe vorhanden ist. Sie müssen eine RemoteApp-App-Gruppe erstellen, um RemoteApps veröffentlichen zu können. Sie können mehrere RemoteApp-App-Gruppen erstellen, um unterschiedliche Workerszenarien abzudecken. Unterschiedliche RemoteApp-App-Gruppen können auch sich überlappende RemoteApps enthalten.

Zum Veröffentlichen von Ressourcen für Benutzer müssen Sie sie App-Gruppen zuweisen. Beachten Sie Folgendes, wenn Sie Benutzer App-Gruppen zuweisen:

- Ein Benutzer kann im gleichen Hostpool sowohl einer Desktop-App-Gruppe als auch einer RemoteApp-App-Gruppe zugewiesen sein. Benutzer können jedoch nur einen Typ von App-Gruppe pro Sitzung starten. Benutzer können nicht beide Typen von App-Gruppen gleichzeitig in einer einzigen Sitzung starten.
- Ein Benutzer kann in einem Hostpool mehreren App-Gruppen zugewiesen sein, und der Feed gilt dann gemeinsam für beide App-Gruppen.

## <a name="workspaces"></a>Arbeitsbereiche

Ein Arbeitsbereich ist eine logische Gruppierung von Anwendungsgruppen in Azure Virtual Desktop. Jede Azure Virtual Desktop-Anwendungsgruppe muss einem Arbeitsbereich zugeordnet sein, damit Benutzer die Remote-Apps und Desktops anzeigen können, die für sie veröffentlicht wurden.

## <a name="end-users"></a>Endbenutzer

Nachdem Sie Benutzer ihren App-Gruppen zugewiesen haben, können diese mit beliebigen Azure Virtual Desktop-Clients eine Verbindung mit einer Azure Virtual Desktop-Bereitstellung herstellen.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich unter [Delegierter Zugriff in Azure Virtual Desktop](delegated-access-virtual-desktop.md) weiter über den delegierten Zugriff und das Zuweisen von Rollen zu Benutzern.

Informationen zum Einrichten des Azure Virtual Desktop-Hostpools finden Sie unter [Erstellen eines Hostpools mit dem Azure-Portal](create-host-pools-azure-marketplace.md).

Informationen zum Herstellen einer Verbindung mit Azure Virtual Desktop finden Sie in einem der folgenden Artikel:

- [Herstellen einer Verbindung mit Windows 10 oder Windows 7](./user-documentation/connect-windows-7-10.md)
- [Herstellen einer Verbindung mit einem Webbrowser](./user-documentation/connect-web.md)
- [Herstellen einer Verbindung mit dem Android-Client](./user-documentation/connect-android.md)
- [Herstellen einer Verbindung mit dem macOS-Client](./user-documentation/connect-macos.md)
- [Herstellen einer Verbindung mit dem iOS-Client](./user-documentation/connect-ios.md)