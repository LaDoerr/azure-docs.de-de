---
title: 'Azure Relay Hybrid Connections: HTTP-Anforderungen in .NET'
description: Schreiben einer C#-Konsolenanwendung für HTTP-Anforderungen von Azure Relay Hybrid Connections in .NET
ms.topic: conceptual
ms.custom: devx-track-csharp
ms.date: 06/23/2021
ms.openlocfilehash: c3b78a3acf2e14a399e3c294dc40748fae5d7ce9
ms.sourcegitcommit: d9a2b122a6fb7c406e19e2af30a47643122c04da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2021
ms.locfileid: "114668269"
---
# <a name="get-started-with-relay-hybrid-connections-http-requests-in-net"></a>Erste Schritte mit HTTP-Anforderungen von Relay Hybrid Connections in .NET
[!INCLUDE [relay-selector-hybrid-connections](./includes/relay-selector-hybrid-connections.md)]

In dieser Schnellstartanleitung erstellen Sie Sender- und Empfängeranwendungen in .NET, die mithilfe von HTTP Nachrichten senden und empfangen. Die Anwendungen verwenden das Hybrid Connections-Feature von Azure Relay. Allgemeine Informationen zu Azure Relay finden Sie unter [Was ist Azure Relay?](relay-what-is-it.md). 

Diese Schnellstartanleitung umfasst folgende Schritte:

1. Erstellen eines Relay-Namespace über das Azure-Portal
2. Erstellen einer Hybridverbindung in diesem Namespace über das Azure-Portal
3. Erstellen einer Serverkonsolenanwendung (Listener) zum Empfangen von Nachrichten
4. Erstellen einer Clientkonsolenanwendung (Absender) zum Senden von Nachrichten
5. Ausführen von Anwendungen 

## <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

* [Visual Studio 2015 oder eine höhere Version.](https://www.visualstudio.com) Für die Beispiele in diesem Tutorial wird Visual Studio 2017 verwendet.
* ein Azure-Abonnement Falls Sie kein Abonnement besitzen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/), bevor Sie beginnen.

## <a name="create-a-namespace"></a>Erstellen eines Namespace
[!INCLUDE [relay-create-namespace-portal](./includes/relay-create-namespace-portal.md)]

## <a name="create-a-hybrid-connection"></a>Hybridverbindung erstellen
[!INCLUDE [relay-create-hybrid-connection-portal](./includes/relay-create-hybrid-connection-portal.md)]

## <a name="create-a-server-application-listener"></a>Erstellen einer Serveranwendung (Listener)
Schreiben Sie in Visual Studio eine C#-Konsolenanwendung, um auf Nachrichten des Relays zu lauschen und sie zu empfangen.

[!INCLUDE [relay-hybrid-connections-http-requests-dotnet-get-started-server](./includes/relay-hybrid-connections-http-requests-dotnet-get-started-server.md)]

## <a name="create-a-client-application-sender"></a>Erstellen einer Clientanwendung (Absender)
Schreiben Sie in Visual Studio eine C#-Konsolenanwendung, um Nachrichten an das Relay zu senden.

[!INCLUDE [relay-hybrid-connections-http-requests-dotnet-get-started-client](./includes/relay-hybrid-connections-http-requests-dotnet-get-started-client.md)]

## <a name="run-the-applications"></a>Ausführen der Anwendungen
1. Führen Sie die Serveranwendung aus. Der folgende Text wird im Konsolenfenster angezeigt:

    ```
    Online
    Server listening
    ```
1. Führen Sie die Clientanwendung aus. Sie sehen `hello!` im Clientfenster. Der Client sendet eine HTTP-Anforderung an den Server, und der Server antwortet mit `hello!`. 
3. Um das Konsolenfenster zu schließen, drücken Sie in beiden Konsolenfenstern die **EINGABETASTE**. 

Glückwunsch! Sie haben eine vollständige Hybrid Connections-Anwendung erstellt!

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie Client- und Serveranwendungen in .NET erstellt, die mithilfe von HTTP Nachrichten senden und empfangen. Das Hybrid Connections-Feature von Azure Relay unterstützt auch die Verwendung von WebSockets zum Senden und Empfangen von Nachrichten. Informationen zur Verwendung von WebSockets mit Hybrid Connections von Azure Relay finden Sie unter [Erste Schritte mit WebSockets von Relay Hybrid Connections in Node](relay-hybrid-connections-dotnet-get-started.md).

In dieser Schnellstartanleitung haben Sie .NET Framework zum Erstellen von Client- und Serveranwendungen verwendet. Informationen zum Schreiben von Client- und Serveranwendungen mithilfe von Node.js finden Sie unter [Erste Schritte mit WebSockets von Relay Hybrid Connections in Node](relay-hybrid-connections-node-get-started.md) oder [Erste Schritte mit HTTP-Anforderungen von Relay Hybrid Connections in .NET](relay-hybrid-connections-http-requests-dotnet-get-started.md).
