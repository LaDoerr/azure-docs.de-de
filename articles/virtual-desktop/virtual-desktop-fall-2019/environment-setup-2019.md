---
title: 'Azure Virtual Desktop-Umgebung (klassisch): Azure'
description: Enthält eine Beschreibung der grundlegenden Elemente einer Azure Virtual Desktop-Umgebung (klassisch).
author: Heidilohr
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: helohr
manager: femila
ms.openlocfilehash: 9062565f5c13d7b278b57068df79c1bb4de66424
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111745059"
---
# <a name="azure-virtual-desktop-classic-environment"></a>Azure Virtual Desktop-Umgebung (klassisch)

>[!IMPORTANT]
>Dieser Inhalt gilt für Azure Virtual Desktop (klassisch). Der Dienst unterstützt keine Azure Virtual Desktop-Objekte in Azure Resource Manager. Wenn Sie Azure Virtual Desktop-Objekte in Azure Resource Manager verwalten möchten, helfen Ihnen die Informationen in [diesem Artikel](../environment-setup.md) weiter.

Azure Virtual Desktop ist ein Dienst, mit dem Benutzer einfachen und sicheren Zugriff auf ihre virtualisierten Desktops und RemoteApps erhalten. In diesem Thema wird die allgemeine Struktur der Azure Virtual Desktop-Umgebung näher erläutert.

## <a name="tenants"></a>Mandanten

Der Azure Virtual Desktop-Mandant ist die primäre Schnittstelle zur Verwaltung Ihrer Azure Virtual Desktop-Umgebung. Jeder Azure Virtual Desktop-Mandant muss der Azure Active Directory-Instanz zugeordnet sein, in der die Benutzer enthalten sind, die sich an der Umgebung anmelden. Über den Azure Virtual Desktop-Mandanten können Sie mit dem Erstellen von Hostpools zum Ausführen der Workloads Ihrer Benutzer beginnen.

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

- Ein Benutzer kann in einem Hostpool sowohl einer Desktop-App-Gruppe als auch einer RemoteApp-App-Gruppe zugewiesen sein.
- Ein Benutzer kann in einem Hostpool mehreren App-Gruppen zugewiesen sein, und der Feed gilt dann gemeinsam für beide App-Gruppen.

## <a name="tenant-groups"></a>Mandantengruppen

Unter Azure Virtual Desktop werden die meisten Einrichtungs- und Konfigurationsschritte auf dem Azure Virtual Desktop-Mandanten ausgeführt. Der Azure Virtual Desktop-Mandant enthält die Hostpools, App-Gruppen und App-Gruppen-Benutzerzuweisungen. Es kann aber bestimmte Situationen geben, in denen Sie mehrere Azure Virtual Desktop-Mandanten gleichzeitig verwalten müssen. Dies gilt besonders, wenn Sie ein Clouddienstanbieter (Cloud Service Provider, CSP) oder ein Hostingpartner sind. In diesen Situationen können Sie eine benutzerdefinierte Azure Virtual Desktop-Mandantengruppe verwenden, um die einzelnen Azure Virtual Desktop-Mandanten der Kunden zu platzieren und den Zugriff zentral zu verwalten. Falls Sie aber nicht mehr als einen Azure Virtual Desktop-Mandanten verwalten, trifft das Konzept der Mandantengruppe nicht zu. Sie können Ihren Mandanten, der in der Standardmandantengruppe vorhanden ist, dann weiter betreiben und verwalten.

## <a name="end-users"></a>Endbenutzer

Nachdem Sie Benutzer ihren App-Gruppen zugewiesen haben, können diese mit beliebigen Azure Virtual Desktop-Clients eine Verbindung mit einer Azure Virtual Desktop-Bereitstellung herstellen.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich unter [Delegierter Zugriff in Azure Virtual Desktop](delegated-access-virtual-desktop-2019.md) weiter über den delegierten Zugriff und das Zuweisen von Rollen zu Benutzern.

Informationen zur Einrichtung Ihres Azure Virtual Desktop-Mandanten finden Sie unter [Erstellen eines Mandanten in Azure Virtual Desktop](tenant-setup-azure-active-directory.md).

Informationen zum Herstellen einer Verbindung mit Azure Virtual Desktop finden Sie in einem der folgenden Artikel:

- [Herstellen einer Verbindung über Windows 10 oder Windows 7](connect-windows-7-10-2019.md)
- [Herstellen einer Verbindung über einen Webbrowser](connect-web-2019.md)
