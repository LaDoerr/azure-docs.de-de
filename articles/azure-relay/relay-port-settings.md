---
title: Azure Relay-Porteinstellungen | Microsoft-Dokumentation
description: Dieser Artikel enthält eine Tabelle, die die erforderliche Konfiguration für Portwerte für Azure Relay beschreibt.
ms.topic: article
ms.date: 06/23/2021
ms.openlocfilehash: b293d58d2e555b7e0d01ea1a9e421f78cf23105c
ms.sourcegitcommit: d9a2b122a6fb7c406e19e2af30a47643122c04da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2021
ms.locfileid: "114668680"
---
# <a name="azure-relay-port-settings"></a>Azure Relay-Porteinstellungen

Die folgende Tabelle beschreibt die erforderliche Konfiguration für Portwerte für Azure Relay.

## <a name="hybrid-connections"></a>Hybridverbindungen

Hybrid Connections verwendet WebSockets an Port 443 mit TLS als zugrunde liegenden Transportmechanismus, der ausschließlich **HTTPS** verwendet. 

## <a name="wcf-relays"></a>WCF-Relays
  
|Bindung|Transportsicherheit|Port|  
|-------------|------------------------|----------|  
|[BasicHttpRelayBinding-Klasse](/dotnet/api/microsoft.servicebus.basichttprelaybinding) (Client)|Ja|HTTPS| 
|" |Nein|HTTP|  
|[BasicHttpRelayBinding-Klasse](/dotnet/api/microsoft.servicebus.basichttprelaybinding) (Dienste)|Sowohl als auch|9351/HTTP|  
|[NetEventRelayBinding-Klasse](/dotnet/api/microsoft.servicebus.neteventrelaybinding) (Client)|Ja|9351/HTTPS|  
|" |Nein|9350/HTTP|  
|[NetEventRelayBinding-Klasse](/dotnet/api/microsoft.servicebus.neteventrelaybinding) (Dienst)|Sowohl als auch|9351/HTTP|  
|[NetTcpRelayBinding-Klasse](/dotnet/api/microsoft.servicebus.nettcprelaybinding) (Client/Dienst)|Sowohl als auch|5671/9352/HTTP (9352/9353, wenn eine Hybridverbindung verwendet)|  
|[NetOnewayRelayBinding Class](/dotnet/api/microsoft.servicebus.netonewayrelaybinding) (Client)|Ja|9351/HTTPS|  
|" |Nein|9350/HTTP|  
|[NetOnewayRelayBinding-Klasse](/dotnet/api/microsoft.servicebus.netonewayrelaybinding) (Dienst)|Sowohl als auch|9351/HTTP|  
|[WebHttpRelayBinding-Klasse](/dotnet/api/microsoft.servicebus.webhttprelaybinding) (Client)|Ja|HTTPS|  
|" |Nein|HTTP|  
|[WebHttpRelayBinding-Klasse](/dotnet/api/microsoft.servicebus.webhttprelaybinding) (Client)|Sowohl als auch|9351/HTTP|  
|[WS2007HttpRelayBinding-Klasse](/dotnet/api/microsoft.servicebus.ws2007httprelaybinding) (Client)|Ja|HTTPS|  
|" |Nein|HTTP|  
|[WS2007HttpRelayBinding-Klasse](/dotnet/api/microsoft.servicebus.ws2007httprelaybinding) (Dienst)|Sowohl als auch|9351/HTTP|

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Azure Relay finden Sie unter diesen Links:
* [Was ist Azure Relay?](relay-what-is-it.md)
* [Relay – häufig gestellte Fragen](relay-faq.yml)