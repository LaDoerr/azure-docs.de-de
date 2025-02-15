---
title: 'Azure ExpressRoute: Entwerfen für Hochverfügbarkeit'
description: Diese Seite enthält architektonische Empfehlungen für Hochverfügbarkeit bei Verwendung von Azure ExpressRoute.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: article
ms.date: 06/28/2019
ms.author: duau
ms.openlocfilehash: 5dcf58dce7b87862c2f01ad76db8aff66366ee4b
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122339417"
---
# <a name="designing-for-high-availability-with-expressroute"></a>Entwurf für Hochverfügbarkeit mit ExpressRoute

ExpressRoute wurde für Hochverfügbarkeit entwickelt, um Konnektivität für private Netzwerke mit Microsoft-Ressourcen auf Netzbetreiberniveau bereitzustellen. Das heißt, gibt es keine einzelne Fehlerquelle im ExpressRoute-Pfad im Microsoft-Netzwerk. Um die Verfügbarkeit zu maximieren, sollten auch das Kunden- und das Dienstanbietersegment Ihrer ExpressRoute-Verbindung auf Hochverfügbarkeit ausgelegt sein. In diesem Artikel werden zunächst Überlegungen zur Netzwerkarchitektur für den Aufbau einer robusten Netzwerkverbindung mit einer ExpressRoute-Verbindung angestellt, dann werden die Optimierungsmöglichkeiten untersucht, die Ihnen helfen, die Hochverfügbarkeit Ihrer ExpressRoute-Verbindung zu verbessern.

>[!NOTE]
>Die in diesem Artikel beschriebenen Konzepte gelten gleichermaßen, wenn eine ExpressRoute-Leitung innerhalb oder außerhalb eine Virtual WAN erstellt wird.
>

## <a name="architecture-considerations"></a>Überlegungen zur Architektur

Die folgende Abbildung veranschaulicht die empfohlene Methode zum Herstellen einer Verbindung mit einer ExpressRoute-Verbindung zum Maximieren der Verfügbarkeit einer ExpressRoute-Verbindung.

 [![1]][1]

Für Hochverfügbarkeit ist es wichtig, die Redundanz der ExpressRoute-Verbindung im gesamten End-to-End-Netzwerk beizubehalten. Das heißt, Sie müssen die Redundanz in Ihrem lokalen Netzwerk beibehalten und dürfen die Redundanz in Ihrem Dienstanbieternetzwerk nicht gefährden. Die Aufrechterhaltung der Redundanz bedeutet mindestens die Vermeidung einer einzelnen Fehlerquelle im Netzwerk. Eine redundante Stromversorgung und Kühlung der Netzwerkgeräte wird die Hochverfügbarkeit weiter verbessern.

### <a name="first-mile-physical-layer-design-considerations"></a>Überlegungen zum Entwurf der physikalischen Schicht auf der ersten Meile

 Wenn Sie sowohl die primären als auch die sekundären Verbindungen einer ExpressRoute-Verbindung auf demselben CPE (Customer Premises Equipment) beenden, gefährden Sie die Hochverfügbarkeit in Ihrem lokalen Netzwerk. Wenn Sie sowohl die primäre als auch die sekundäre Verbindung über den gleichen Port eines CPE konfigurieren (entweder durch Beenden der beiden Verbindungen unter verschiedenen Unterschnittstellen oder durch Zusammenführen der beiden Verbindungen innerhalb des Partnernetzwerks), zwingen Sie den Partner außerdem, die Hochverfügbarkeit auch in seinem Netzwerksegment zu beeinträchtigen. Diese Beeinträchtigung wird in der folgenden Abbildung dargestellt.

[![2]][2]

Wenn Sie hingegen die primären und sekundären Verbindungen einer ExpressRoute-Verbindung an verschiedenen geografischen Standorten beenden, könnten Sie die Netzwerkleistung der Konnektivität beeinträchtigen. Wenn für den Datenverkehr ein aktiver Lastausgleich zwischen der primären Verbindung und den sekundären Verbindungen besteht, die an verschiedenen geografischen Standorten beendet werden, würde ein potenzieller wesentlicher Unterschied in der Netzwerklatenz zwischen den beiden Pfaden zu einer suboptimalen Netzwerkleistung führen. 

Weitere Informationen zu georedundanten Entwurfsüberlegungen finden Sie unter [Entwurf für die Notfallwiederherstellung mit ExpressRoute][DR].

### <a name="active-active-connections"></a>Aktiv/Aktiv-Verbindungen

Das Microsoft-Netzwerk ist so konfiguriert, dass es die primären und sekundären Verbindungen der ExpressRoute-Verbindungen im Aktiv/Aktiv-Modus betreibt. Durch Ihre Routenankündigungen können Sie jedoch die redundanten Verbindungen einer ExpressRoute-Verbindung zwingen, im Aktiv-Passiv-Modus zu arbeiten. Die Ankündigung spezifischerer Routen und das Voranstellen von BGP AS-Pfaden sind die gebräuchlichsten Techniken, um einen Pfad dem anderen vorzuziehen.

Um die Hochverfügbarkeit zu verbessern, wird empfohlen, beide Verbindungen einer ExpressRoute-Verbindung im Aktiv-Aktiv-Modus zu betreiben. Wenn Sie die Verbindungen im Aktiv-Aktiv-Modus arbeiten lassen, wird das Microsoft-Netzwerk einen Lastenausgleich des Datenverkehrs über die Verbindungen hinweg auf einer Pro-Datenfluss-Basis durchführen.

Wenn die primären und sekundären Verbindungen einer ExpressRoute-Verbindung im Aktiv-Passiv-Modus ausgeführt werden, besteht die Gefahr, dass beide Verbindungen nach einem Fehler im aktiven Pfad ausfallen. Die häufigsten Ursachen für einen Ausfall beim Umschalten sind mangelnde aktive Verwaltung der passiven Verbindung und passive Verbindungsankündigung veralteter Routen.

Alternativ führt das Betreiben der primären und sekundären Verbindungen einer ExpressRoute-Verbindung im Aktiv-Aktiv-Modus dazu, dass nur etwa die Hälfte der Datenflüsse fehlschlägt und nach einem Ausfall der ExpressRoute-Verbindung umleitet wird. Somit wird der Aktiv-Aktiv-Modus wesentlich dazu beitragen, die mittlere Wiederherstellungszeit (Mean Time To Recover, MTTR) zu verbessern.

> [!NOTE]
> Während einer Wartungsaktivität oder bei ungeplanten Ereignissen, die sich auf eine der Verbindungen auswirken, bevorzugt Microsoft die AS-Pfadpräfigierung, um Datenverkehr zum Ausgleich auf die fehlerfreie Verbindung umzuleiten. Sie müssen sicherstellen, dass der Datenverkehr zum fehlerfreien Pfad umgeleitet werden kann, wenn Microsoft die Pfadpräfigierung konfiguriert hat, und dass erforderliche Umleitungsankündigungen entsprechend konfiguriert sind, um Dienstunterbrechungen zu vermeiden. 
> 

### <a name="nat-for-microsoft-peering"></a>NAT für Microsoft-Peering 

Microsoft-Peering dient der Kommunikation zwischen öffentlichen Endpunkten. So sind lokale private Endpunkte häufig netzwerkadressenübersetzt (Network Address Translated, NATed) mit öffentlicher IP-Adresse für das Kunden- oder Partnernetzwerk, bevor sie über Microsoft-Peering kommunizieren. Angenommen, Sie verwenden sowohl die primäre als auch die sekundäre Verbindung im Aktiv-Aktiv-Modus, in dem Ihre NAT einen Einfluss darauf besitzt, wie schnell die Wiederherstellung nach einem Ausfall in einer der ExpressRoute-Verbindungen erfolgt. Zwei verschiedene NAT-Optionen werden in der folgenden Abbildung gezeigt:

[![3]][3]

#### <a name="option-1"></a>Option 1:

NAT wird angewendet, nachdem der Datenverkehr zwischen der primären Verbindung und den sekundären Verbindungen der ExpressRoute-Leitung aufgeteilt wurde. Um die zustandsbehafteten Anforderungen von NAT zu erfüllen, werden unabhängige NAT-Pools für die primären und sekundären Geräte verwendet. Der zurückgegebene Datenverkehr wird auf demselben Edgegerät eintreffen, über das der Datenfluss ausgegangen ist.

Wenn bei der ExpressRoute-Verbindung ein Fehler auftritt, ist die Erreichbarkeit des entsprechenden NAT-Pools unterbrochen. Deshalb müssen alle unterbrochenen Netzwerkdatenflüsse entweder durch TCP oder über die Anwendungsschicht nach dem entsprechenden Fenstertimeout erneut eingerichtet werden. Während des Fehlers kann Azure die lokalen Server nicht über die entsprechende NAT erreichen, bis die Konnektivität für die primären oder sekundären Verbindungen der ExpressRoute-Leitung wiederhergestellt wurde.

#### <a name="option-2"></a>Option 2:

Es wird ein allgemeiner NAT-Pool verwendet, bevor der Datenverkehr zwischen der primären Verbindung und den sekundären Verbindungen der ExpressRoute-Leitung aufgeteilt wird. Es ist wichtig zu unterscheiden, dass der gemeinsame NAT-Pool vor der Aufteilung des Datenverkehrs nicht die Einführung einer einzelnen Fehlerquelle bedeutet und damit die Hochverfügbarkeit beeinträchtigt.

Der NAT-Pool ist auch nach einem Fehler bei der primären oder sekundären Verbindung erreichbar. Aus diesem Grund kann die Netzwerkebene selbst die Pakete umleiten und bei der schnelleren Wiederherstellung nach einem Fehler helfen. 

> [!NOTE]
> * Wenn Sie NAT-Option 1 (unabhängige NAT-Pools für primäre und sekundäre ExpressRoute-Verbindungen) verwenden und einen Port einer IP-Adresse von einem der NAT-Pools einem lokalen Server zuordnen, ist der Server über die ExpressRoute-Verbindung nicht erreichbar, wenn die entsprechende Verbindung ausfällt.
> * Das Beenden von ExpressRoute-BGP-Verbindungen auf zustandsbehafteten Geräten kann bei geplanten oder ungeplanten Wartungen durch Microsoft oder Ihren ExpressRoute-Anbieter zu Problemen beim Failover führen. Sie sollten Ihre Einrichtung testen, um das ordnungsgemäße Failover Ihres Datenverkehrs sicherzustellen, und nach Möglichkeit BGP-Sitzungen auf zustandslosen Geräten beenden.

## <a name="fine-tuning-features-for-private-peering"></a>Optimieren von Funktionen für privates Peering

In diesem Abschnitt erläutern wir optionale Funktionen (abhängig von Ihrer Azure-Bereitstellung und der Empfindlichkeit hinsichtlich der MTTR), die zur Verbesserung der Hochverfügbarkeit Ihrer ExpressRoute-Verbindung beitragen. Befassen wir uns also insbesondere mit der zonenfähigen Bereitstellung von virtuellen ExpressRoute-Netzwerkgateways und der bidirektionalen Weiterleitungserkennung (Bidirectional Forwarding Detection, BFD).

### <a name="availability-zone-aware-expressroute-virtual-network-gateways"></a>Verfügbarkeitszonenfähige ExpressRoute-Gateways für virtuelle Netzwerke

Eine Verfügbarkeitszone in einer Azure-Region ist eine Kombination aus einer Fehlerdomäne und einer Updatedomäne. Wenn Sie sich für eine zonenredundante Azure IaaS-Bereitstellung entscheiden, können Sie auch zonenredundante virtuelle Netzwerkgateways konfigurieren, die privates ExpressRoute-Peering beenden. Mehr dazu erfahren Sie unter [Informationen zu zonenredundanten Gateways für das virtuelle Netzwerk in Azure-Verfügbarkeitszonen][zone redundant vgw]. Informationen zum Konfigurieren eines zonenredundanten Gateways für das virtuelle Netzwerk finden Sie unter [Erstellen eines zonenredundanten Gateways für das virtuelle Netzwerk in Azure-Verfügbarkeitszonen][conf zone redundant vgw].

### <a name="improving-failure-detection-time"></a>Verbessern des Zeitpunkts der Fehlererkennung

ExpressRoute unterstützt BFD über privates Peering. BFD reduziert die Erkennungszeit von Ausfällen über das Layer-2-Netzwerk zwischen Microsoft Enterprise Edge (MSEEs) und deren BGP-Nachbarn auf der lokalen Seite von etwa 3 Minuten (Standard) auf weniger als eine Sekunde. Eine schnelle Ausfallerkennungszeit hilft dabei, die Wiederherstellung nach einem Fehler zu beschleunigen. Weitere Informationen finden Sie unter [Konfigurieren von BFD über ExpressRoute][BFD].

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben wir erläutert, wie Sie die Hochverfügbarkeit der Konnektivität einer ExpressRoute-Verbindung konzipieren können. Ein Peeringpunkt einer ExpressRoute-Verbindung ist an einen geografischen Standort gebunden und kann daher von einem katastrophenbedingten Ausfall betroffen sein, der sich auf den gesamten Standort auswirkt. 

Überlegungen zum Entwurf einer georedundanten Netzwerkverbindung zum Microsoft-Backbone, die katastrophenbedingten Ausfällen standhalten kann, die eine ganze Region betreffen, finden Sie unter [Entwurf für die Notfallwiederherstellung mit privatem ExpressRoute-Peering][DR].

<!--Image References-->
[1]: ./media/designing-for-high-availability-with-expressroute/exr-reco.png "Empfohlene Methode zum Herstellen einer Verbindung mit ExpressRoute"
[2]: ./media/designing-for-high-availability-with-expressroute/suboptimal-lastmile-connectivity.png "Suboptimale Konnektivität der letzten Meile"
[3]: ./media/designing-for-high-availability-with-expressroute/nat-options.png "NAT-Optionen"


<!--Link References-->
[zone redundant vgw]: ../vpn-gateway/about-zone-redundant-vnet-gateways.md
[conf zone redundant vgw]: ../vpn-gateway/create-zone-redundant-vnet-gateway.md
[Configure Global Reach]: ./expressroute-howto-set-global-reach.md
[BFD]: ./expressroute-bfd.md
[DR]: ./designing-for-disaster-recovery-with-expressroute-privatepeering.md
