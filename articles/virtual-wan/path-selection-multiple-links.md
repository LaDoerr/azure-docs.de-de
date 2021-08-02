---
title: Azure-Pfadauswahl über mehrere ISP-Links
titleSuffix: Azure Virtual WAN
description: Hier erfahren Sie, wie Azure Virtual WAN Linkinformationen enthalten kann, um den Datenverkehr mithilfe der Azure-Pfadauswahl über verschiedene Links zu steuern.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 04/27/2021
ms.author: cherylmc
ms.openlocfilehash: 9938f9da91f42c8f22b1abca5f85b332321dc486
ms.sourcegitcommit: e1d5abd7b8ded7ff649a7e9a2c1a7b70fdc72440
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2021
ms.locfileid: "110579085"
---
# <a name="azure-path-selection-across-multiple-isp-links"></a>Azure-Pfadauswahl über mehrere ISP-Links

Azure Virtual WAN bietet einem Benutzer die Möglichkeit, Linkinformationen in einer VPN-Site vorzusehen. Dies ermöglicht Szenarien, in denen die VPN/SD-WAN-Gerätelösung branchspezifische Richtlinien programmieren kann, um den Datenverkehr über verschiedene Links nach Azure zu steuern. Dies wird **Azure-Pfadauswahl** genannt.

## <a name="architecture"></a>Aufbau

Um zu verstehen, wie die Azure-Pfadauswahl funktioniert, verwenden wir eine Virtual WAN-VPN-Site und eine Site-to-Site-Verbindung als Beispiel.

Eine VPN-Site stellt das lokale SD-WAN/VPN-Gerät mit Informationen wie öffentliche IP-Adresse, Gerätemodell und-Name usw. dar. Die tatsächliche lokale VPN-Site kann über mehrere ISP-Links verfügen, die ebenfalls in den Informationen für die Virtual WAN-Site enthalten sein können. Dies ermöglicht es Ihnen, die Linkinformationen in Azure anzuzeigen.

Eine Site-to-Site-IPSec-Verbindung, die in das VPN eines Virtual WAN eingeht, endet an den VPN-Gatewayinstanzen in einem virtuellen Hub. Eine Site-to-Site-Verbindung stellt die Konnektivität zwischen der VPN-Site und dem Azure-VPN-Gateway dar. Sie besteht aus mindestens einer Linkverbindung. Jede Linkverbindung besteht aus zwei Tunneln, wobei jeder Tunnel an einer eindeutigen Instanz des VPN-Gateways von Azure Virtual WAN endet. In der Site-to-Site-Verbindung können bis zu vier Linkverbindungen eingerichtet werden. Dadurch ist werden bis zu acht Tunnel in einer Site-to-Site-Verbindung ermöglicht. Azure unterstützt bis zu 2000 Tunnel, die in einem einzelnen Virtual WAN-VPN-Gateway enden.

:::image type="content" source="./media/path-selection-multiple-links/multi-link-site.png" alt-text="Diagramm mit mehreren Links":::

Diese Abbildung veranschaulicht mehrere Links an einer Site für die Verbindung zu Azure Virtual WAN. In diesem Diagramm:

* Der lokale Branch (VPN/SD-WAN-Gerät) umfasst zwei ISP-Links. Jeder ISP-Link entspricht einer Linkverbindung.

* Es wird davon ausgegangen, dass das lokale vom Kunden verwaltete VPN/SD-WAN-Gerät IKEv1 oder IKEv2 IPSec unterstützt.

* Jede Azure-Site-to-Site-Virtual WAN-Verbindung besteht aus internen Linkverbindungen. Eine Verbindung unterstützt bis zu vier Linkverbindungen. Azure berechnet für die Virtual WAN-Verbindung eine Gebühr für angefallene Verbindungseinheiten. Es fallen keine Gebühren für die Linkverbindungen an.

* Jede Linkverbindung besteht wiederum aus zwei IPSec-Tunneln, die an zwei verschiedenen Instanzen des Virtual WAN-VPN-Gateways enden können. Die Gateways werden aus Gründen der Resilienz als Aktiv/Aktiv-Gateways eingerichtet. Jede Linkverbindung muss über eine eindeutige IP-Adresse und eine BGP-Peering-IP verfügen. Im Diagramm kann Tunnel 0 an Instanz 0 enden, und Tunnel 1 kann an Instanz 1 enden.

* Branchgeräte, die eine Pfadauswahl ermöglichen, können eine geeignete Richtlinie in der Branchverwaltungslösung aktivieren, um den Datenverkehr über mehrere Links nach Azure zu steuern. Beispielsweise kann der ISP 1-Link für Datenverkehr mit höherer Priorität verwendet werden, während der ISP 2-Link als Backup verwendet werden kann.

* Beachten Sie, dass das Virtual Hub-VPN ECMP (Equal Cost Multi-Path Routing) für alle dort endenden Tunnel verwendet.

## <a name="next-steps"></a>Nächste Schritte

Siehe [Azure-FAQ](virtual-wan-faq.md).