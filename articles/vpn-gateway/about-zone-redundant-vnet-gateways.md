---
title: Zonenredundante VNET-Gateways in Azure-Verfügbarkeitszonen
description: Hier erfahren Sie, wie Sie VPN- und ExpressRoute-Gateways in Azure-Verfügbarkeitszonen bereitstellen, um Resilienz, Skalierbarkeit und eine höhere Verfügbarkeit für VNET-Gateways zu erzielen.
titleSuffix: Azure VPN Gateway
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 05/18/2021
ms.author: cherylmc
ms.openlocfilehash: 0482146a7070083c795a60a4b01fdede7e1b3bf1
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110067382"
---
# <a name="about-zone-redundant-virtual-network-gateways-in-azure-availability-zones"></a>Informationen zu zonenredundanten Gateways für das virtuelle Netzwerk in Azure-Verfügbarkeitszonen

Sie können VPN- und ExpressRoute-Gateways in [Azure-Verfügbarkeitszonen](../availability-zones/az-overview.md) bereitstellen. So erzielen Sie Stabilität, Skalierbarkeit und eine höhere Verfügbarkeit für die Gateways des virtuellen Netzwerks. Durch die Bereitstellung von Gateways in Azure-Verfügbarkeitszonen werden die Gateways innerhalb einer Region physisch und logisch getrennt. Gleichzeitig wird die Konnektivität Ihres lokalen Netzwerks mit Azure vor Ausfällen auf Zonenebene geschützt.

### <a name="zone-redundant-gateways"></a><a name="zrgw"></a>Zonenredundante Gateways

Um Ihre Gateways für virtuelle Netzwerke automatisch über Verfügbarkeitszonen hinweg bereitzustellen, können Sie zonenredundante Gateways für virtuelle Netzwerke verwenden. Mit zonenredundanten Gateways können Sie von Zonenresilienz profitieren, um auf Ihre unternehmenskritischen, skalierbaren Dienste in Azure zuzugreifen.

<br>
<br>

![Grafik: zonenredundante Gateways](./media/create-zone-redundant-vnet-gateway/zonered.png)

### <a name="zonal-gateways"></a><a name="zgw"></a>Zonenbasierte Gateways

Um Gateways in einer bestimmten Zone bereitzustellen, verwenden Sie zonenbasierte Gateways. Wenn Sie ein zonenbasiertes Gateway bereitstellen, werden alle Instanzen des Gateways in derselben Verfügbarkeitszone bereitgestellt.

<br>
<br>

![Grafik: zonenbasierte Gateways](./media/create-zone-redundant-vnet-gateway/zonal.png)

## <a name="gateway-skus"></a><a name="gwskus"></a>Gateway-SKUs

Zonenredundante und zonenbasierte Gateways sind als Gateway-SKUs verfügbar. In Regionen mit Azure-Verfügbarkeitszonen haben wir neue SKUs für virtuelle Netzwerkgateways hinzugefügt. Diese SKUs ähneln den entsprechenden SKUs für ExpressRoute und VPN Gateway, sind aber speziell auf zonenredundante und zonenbasierte Gateways ausgelegt. Sie können diese SKUs am „AZ“ im SKU-Namen identifizieren.

Weitere Informationen zu Gateway-SKUs finden Sie unter [VPN-Gateway-SKUs](vpn-gateway-about-vpngateways.md#gwsku) und [ExpressRoute-Gateway-SKUs](../expressroute/expressroute-about-virtual-network-gateways.md#gwsku).

## <a name="public-ip-skus"></a><a name="pipskus"></a>SKUs für öffentliche IP-Ressourcen

Zonenredundante und zonenbasierte Gateways verwenden die Azure-SKU für öffentliche IP-Ressourcen vom Typ *Standard*. Die Konfiguration der öffentlichen Azure-IP-Ressource bestimmt, ob Sie ein zonenredundantes oder zonenbasiertes Gateway bereitstellen. Wenn Sie eine öffentliche IP-Ressource mit einer *Basic*-SKU erstellen, verfügt das Gateway nicht über Zonenredundanz, und die Gatewayressourcen sind auf eine Region begrenzt.

### <a name="zone-redundant-gateways"></a><a name="pipzrg"></a>Zonenredundante Gateways

Wenn Sie eine öffentliche IP-Adresse mit der **Standard**-SKU für öffentliche IP-Adressen erstellen, ohne eine Zone anzugeben, ändert sich das Verhalten, je nachdem, ob es sich um ein VPN-Gateway oder ein ExpressRoute-Gateway handelt. 

* Im Fall eines VPN-Gateways werden die beiden Gatewayinstanzen in zwei beliebigen der drei Zonen bereitgestellt, um für Zonenredundanz zu sorgen. 
* Da es bei einem ExpressRoute-Gateway mehr als zwei Instanzen geben kann, kann sich das Gateway über alle drei Zonen erstrecken.

### <a name="zonal-gateways"></a><a name="pipzg"></a>Zonenbasierte Gateways

Wenn Sie eine öffentliche IP-Adresse mit der **Standard**-SKU für öffentliche IP-Adressen erstellen und die Zone (1, 2 oder 3) angeben, werden alle Gatewayinstanzen in derselben Zone bereitgestellt.

### <a name="regional-gateways"></a><a name="piprg"></a>Regionsbezogene Gateways

Wenn Sie eine öffentliche IP-Adresse mit der **Basic**-SKU für öffentliche IP-Adressen erstellen, wird das Gateway als regionsbezogenes Gateway bereitgestellt und bietet keine in das Gateway integrierte Zonenredundanz.

## <a name="faq"></a><a name="faq"></a>Häufig gestellte Fragen

### <a name="what-will-change-when-i-deploy-these-skus"></a>Was ändert sich, wenn ich diese SKUs bereitstelle?

Aus Ihrer Sicht können Sie Ihre Gateways mit Zonenredundanz bereitstellen. Das bedeutet, dass alle Instanzen der Gateways in Azure-Verfügbarkeitszonen bereitgestellt werden und jede Verfügbarkeitszone eine eigene Fehler- und Updatedomäne darstellt. Dadurch werden Ihre Gateways zuverlässiger, verfügbarer und robuster bei Zonenausfällen.

### <a name="can-i-use-the-azure-portal"></a>Kann ich das Azure-Portal verwenden?

Ja, Sie können diese SKUs über das Azure-Portal bereitstellen. Allerdings werden diese SKUs nur in den Azure-Regionen angezeigt, in denen es Azure-Verfügbarkeitszonen gibt.

### <a name="what-regions-are-available-for-me-to-use-these-skus"></a>In welchen Regionen sind diese SKUs verfügbar?

Diese SKUs sind in den Azure-Regionen verfügbar, in denen es Azure-Verfügbarkeitszonen gibt. Weitere Informationen finden Sie unter [Azure-Regionen mit Verfügbarkeitszonen](../availability-zones/az-region.md#azure-regions-with-availability-zones).

### <a name="can-i-changemigrateupgrade-my-existing-virtual-network-gateways-to-zone-redundant-or-zonal-gateways"></a>Kann ich meine vorhandenen Gateways für virtuelle Netzwerke in zonenredundante oder zonenbasierte Gateways ändern/migrieren/aktualisieren?

Die Migration vorhandener Gateways für virtuelle Netzwerke zu zonenredundanten oder zonenbasierten Gateways wird derzeit nicht unterstützt. Sie können jedoch Ihr vorhandenes Gateway löschen und ein neues zonenredundantes oder zonenbasiertes Gateway erstellen.

### <a name="can-i-deploy-both-vpn-and-express-route-gateways-in-same-virtual-network"></a>Kann ich VPN- und ExpressRoute-Gateways im gleichen virtuellen Netzwerk bereitstellen?

Die Bereitstellung von VPN- und ExpressRoute-Gateways im gleichen virtuellen Netzwerk wird unterstützt. Sie sollten jedoch den IP-Adressbereich „/27“ für das Gatewaysubnetz reservieren.

## <a name="next-steps"></a>Nächste Schritte

[Erstellen eines zonenredundanten Gateways für virtuelle Netzwerke](create-zone-redundant-vnet-gateway.md)
