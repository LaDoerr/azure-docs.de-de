---
title: 'Architektur: Migrieren zu Azure Virtual WAN'
description: Erfahren Sie, wie Sie von einer vorhandenen vom Kunden verwalteten Hub-and-Spoke-Topologie zu einem Entwurf migrieren, der von Microsoft verwaltete Virtual WAN-Hubs nutzt.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 04/27/2021
ms.author: cherylmc
ms.openlocfilehash: 3fa07445b667d59fba81040c70a111c4a0f94670
ms.sourcegitcommit: e1d5abd7b8ded7ff649a7e9a2c1a7b70fdc72440
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2021
ms.locfileid: "110580042"
---
# <a name="migrate-to-azure-virtual-wan"></a>Migrieren zu Azure Virtual WAN

Mit Azure Virtual WAN können Unternehmen ihre globale Verbindungstopologie vereinfachen, um von der Skalierbarkeit des globalen Netzwerks von Microsoft zu profitieren. Dieser Artikel enthält technische Details für Unternehmen, die eine Migration von einer vorhandenen, kundenseitig verwalteten Hub-and-Spoke-Topologie zu einem Entwurf durchführen möchten, bei dem von Microsoft verwaltete Virtual WAN-Hubs zum Einsatz kommen.

Unter [Architektur mit einem globalen Transitnetzwerk und Azure Virtual WAN](virtual-wan-global-transit-network-architecture.md) werden die Vorteile hervorgehoben, die sich durch Azure Virtual WAN für Unternehmen ergeben, die ein auf die Cloud ausgerichtetes, modernes und globales Unternehmensnetzwerk einführen.

:::image type="content" source="./media/migrate-from-hub-spoke-topology/hub-spoke.png" alt-text="Hub-and-Spoke":::
**Abbildung: Azure Virtual WAN**

Das Hub-and-Spoke-Konnektivitätsmodell des virtuellen Azure-Rechenzentrums wurde von Tausenden unserer Kunden eingeführt, um auf der Grundlage des standardmäßigen transitiven Routingverhaltens von Azure-Netzwerken einfache und skalierbare Cloudnetzwerke aufzubauen. Azure Virtual WAN baut auf diesen Konzepten auf und bietet neue Funktionen, die nicht nur globale Konnektivitätstopologien zwischen lokalen Standorten und Azure unterstützen, sondern Kunden zudem die Möglichkeit geben, die Skalierbarkeit des Microsoft-Netzwerks zur Erweiterung ihrer bestehenden globalen Netzwerke zu nutzen.

In diesem Artikel wird beschrieben, wie Sie eine vorhandene, vom Kunden verwaltete Hub-and-Spoke-Umgebung zu einer auf dem virtuellen Azure-WAN basierenden Topologie migrieren.

## <a name="scenario"></a>Szenario

Contoso ist eine globale Finanzorganisation mit Niederlassungen in Europa und Asien. Das Unternehmen plant, seine vorhandenen Anwendungen von einem lokalen Rechenzentrum nach Azure zu verschieben und hat einen auf der kundenseitig verwalteten Hub-and-Spoke-Architektur basierenden Entwurf ausgearbeitet, der regionale, virtuelle Hubnetzwerke für Hybridkonnektivität einschließt. Im Rahmen der Umstellung auf cloudbasierte Technologien wurde das Netzwerkteam mit der Optimierung der Konnektivität für die weitere Entwicklung des Unternehmens beauftragt.

Die folgende Abbildung zeigt eine allgemeine Übersicht über das vorhandene globale Netzwerk, einschließlich der Anbindung an mehrere Azure-Regionen.

:::image type="content" source="./media/migrate-from-hub-spoke-topology/contoso-pre-migration.png" alt-text="Vorhandene Netzwerktopologie von Contoso":::
**Abbildung: Vorhandene Netzwerktopologie von Contoso**

Aus der vorhandenen Netzwerktopologie lassen sich folgende Punkte ableiten:

* Eine Hub-and-Spoke-Topologie wird in mehreren Regionen verwendet, einschließlich ExpressRoute-Verbindungen zur Rückverbindung mit einem allgemeinen privaten WAN.

* Einige dieser Standorte verfügen außerdem über direkte VPN-Tunnel mit Azure, um eine Kommunikation mit Anwendungen zu ermöglichen, die in der Cloud gehostete werden.

## <a name="requirements"></a>Requirements (Anforderungen)

Das Netzwerkteam wurde mit der Bereitstellung eines globalen Netzwerkmodells beauftragt, das die Migration von Contoso in die Cloud unterstützen kann und eine Optimierung in Bezug auf Kosten, Skalierung und Leistung ermöglicht. Zusammengefasst müssen die folgenden Anforderungen erfüllt werden:

* Bereitstellung optimierter Pfade zu in der Cloud gehosteten Anwendungen sowohl für den Hauptstandort als auch für die Zweigstellen.
* Beseitigung der Abhängigkeit von vorhandenen lokalen Rechenzentren zur VPN-Terminierung bei gleichzeitiger Beibehaltung der folgenden Konnektivitätspfade:
  * **Branch-to-VNET (B2V)** : Mit dem VPN verbundene Niederlassungen müssen auf Anwendungen zugreifen können, die in der lokalen Azure-Region zur Cloud migriert wurden.
  * **Branch-to-Hub-to-Hub-to-VNET (B2H2H2V)** : Mit dem VPN verbundene Niederlassungen müssen auf Anwendungen zugreifen können, die in der Azure-Remoteregion zur Cloud migriert wurden.
  * **Branch-to-Branch (B2B)** : Regionale Niederlassungen mit VPN-Verbindung müssen untereinander und mit Haupt-/DC-Standorten mit ExpressRoute-Verbindung kommunizieren können.
  * **Branch-to-Hub-to-Hub-to-Branch (B2H2H2B)** : Global getrennte Niederlassungen mit VPN-Verbindung müssen untereinander und mit beliebigen Haupt-/DC-Standorten mit ExpressRoute-Verbindung kommunizieren können.
  * **Branch-to-Internet (B2I)** : Verbundene Standorte müssen mit dem Internet kommunizieren können. Dieser Datenverkehr muss gefiltert und protokolliert werden.
  * **VNET-to-VNET (V2V)** : Virtuelle Spoke-Netzwerke in derselben Region müssen miteinander kommunizieren können.
  * **VNET-to-Hub-to-Hub-to-VNET (V2H2H2V)** : Virtuelle Spoke-Netzwerke in unterschiedlichen Regionen müssen miteinander kommunizieren können.
* Contoso-Roamingbenutzer (Laptop und Telefon) müssen auf Unternehmensressourcen zugreifen können, wenn sie sich nicht im Unternehmensnetzwerk befinden.

## <a name="azure-virtual-wan-architecture"></a><a name="architecture"></a>Azure Virtual WAN-Architektur

Die folgende Abbildung zeigt eine allgemeine Übersicht über die aktualisierte Zieltopologie, in der die im vorherigen Abschnitt beschriebenen Anforderungen mit Azure Virtual WAN erfüllt wurden.

:::image type="content" source="./media/migrate-from-hub-spoke-topology/vwan-architecture.png" alt-text="Virtuelle WAN-Architektur von Contoso":::
**Abbildung: Azure Virtual WAN-Architektur**

Zusammenfassung:

* Der Hauptstandort in Europa bleibt über ExpressRoute verbunden, der lokale DC wird vollständig zu Azure migriert und nun außer Betrieb genommen.
* DC und Hauptstandort in Asien bleiben mit dem privatem WAN verbunden. Ab sofort wird Azure Virtual WAN zur Erweiterung des lokalen Anbieternetzwerks und zur Bereitstellung globaler Konnektivität verwendet.
* Sowohl in der Azure-Region „Europa, Westen“ als auch in der Region „Asien, Südosten“ werden Azure Virtual WAN-Hubs eingesetzt, um Konnektivitätshubs für Geräte bereitzustellen, die über ExpressRoute und VPN verbunden sind.
* Hubs bieten außerdem VPN-Terminierung für Roamingbenutzer für mehrere Clienttypen. Die dabei verwendete OpenVPN-Konnektivität mit dem globalen Mesh-Netzwerk ermöglicht nicht nur den Zugriff auf Anwendungen, die zu Azure migriert wurden, sondern auch auf Ressourcen, die in der lokalen Umgebung verbleiben.
* Azure Virtual WAN stellt Internetkonnektivität für Ressourcen innerhalb eines virtuellen Netzwerks bereit.

Azure Virtual WAN stellt ebenso Internetkonnektivität für Remotestandorte bereit. Durch einen lokalen Internet-Breakout via Partnerintegration wird der Zugriff auf SaaS-Dienste wie Microsoft 365 optimiert.

## <a name="migrate-to-virtual-wan"></a>Migrieren zu Virtual WAN

In diesem Abschnitt werden die verschiedenen Schritte für die Migration zu Azure Virtual WAN erläutert.

### <a name="step-1-single-region-customer-managed-hub-and-spoke"></a>Schritt 1: Hub-and-Spoke vom Kunden verwaltet in einer einzelnen Region

Die folgende Abbildung zeigt die Topologie einer einzelnen Region für Contoso vor dem Rollout von Azure Virtual WAN:

:::image type="content" source="./media/migrate-from-hub-spoke-topology/figure1.png" alt-text="Topologie einer einzelnen Region":::
**Abbildung 1: Hub-and-Spoke manuell – Einzelregion**

In Anlehnung an den Hub-and-Spoke-Ansatz umfasst das kundenseitig verwaltete virtuelle Hubnetzwerk verschiedene Funktionsblöcke:

* Gemeinsame Dienste (alle gemeinsamen Funktionen, die von mehreren Spokes benötigt werden). Beispiel: Contoso verwendet auf IaaS-VMs (Infrastructure-as-a-Service) Windows Server-Domänencontroller.
* IP-/Routingfirewalldienste werden über eine virtuelle Netzwerkappliance eines Drittanbieters bereitgestellt und ermöglichen ein Spoke-übergreifendes Layer-3-IP-Routing.
* Dienste für eingehenden/ausgehenden Internetdatenverkehr. Hierzu zählen beispielsweise Azure Application Gateway für eingehende HTTP-Anforderungen und Proxydienste von Drittanbietern, die auf VMs ausgeführt werden und den ausgehenden Internetdatenverkehr filtern.
* Das ExpressRoute- und VPN-Gateway für virtuelle Netzwerke sorgt für Konnektivität mit lokalen Netzwerken.

### <a name="step-2-deploy-virtual-wan-hubs"></a>Schritt 2: Bereitstellen von Virtual WAN-Hubs

Stellen Sie in jeder Region ein Virtual WAN-Hub bereit. Richten Sie den Virtual WAN-Hub mit VPN- und ExpressRoute-Funktionen ein, wie in den folgenden Artikeln beschrieben:

* [Tutorial: Erstellen einer Site-to-Site-Verbindung per Azure Virtual WAN](virtual-wan-site-to-site-portal.md)
* [Tutorial: Erstellen einer ExpressRoute-Zuordnung per Azure Virtual WAN](virtual-wan-expressroute-portal.md)

> [!NOTE]
> Azure Virtual WAN muss für einige in diesem Artikel gezeigten Datenverkehrspfade die Standard-SKU verwenden.
>

:::image type="content" source="./media/migrate-from-hub-spoke-topology/figure2.png" alt-text="Bereitstellen von Virtual WAN-Hubs":::
**Abbildung 2: Migration von Hub-and-Spoke unter Kundenverwaltung zu Virtual WAN**

### <a name="step-3-connect-remote-sites-expressroute-and-vpn-to-virtual-wan"></a>Schritt 3: Verbinden von Remotestandorten (ExpressRoute und VPN) mit Virtual WAN

Verbinden Sie den Virtual WAN-Hub mit den vorhandenen ExpressRoute-Leitungen, und richten Sie über das Internet Site-to-Site-VPN-Verbindungen mit beliebigen Remotezweigstellen ein.

:::image type="content" source="./media/migrate-from-hub-spoke-topology/figure3.png" alt-text="Verbinden von Remotestandorten mit Virtual WAN":::
**Abbildung 3: Migration von Hub-and-Spoke unter Kundenverwaltung zu Virtual WAN**

An diesem Punkt empfangen lokale Netzwerkgeräte Routen, die dem IP-Adressraum entsprechen, der dem von Virtual WAN verwalteten Hub-VNet zugewiesen wurde. Per VPN-verbundene Remotezweigstellen sehen in dieser Phase zwei Pfade zu beliebigen vorhandenen Anwendungen in den virtuellen Spoke-Netzwerken. Diese Geräte müssen so konfiguriert werden, dass sie auch weiterhin den Tunnel zum kundenseitig verwalteten Hub nutzen können, um während der Übergangsphase ein symmetrisches Routing zu gewährleisten.

### <a name="step-4-test-hybrid-connectivity-via-virtual-wan"></a>Schritt 4: Testen der Hybridkonnektivität über Virtual WAN

Bevor Sie den verwalteten Virtual WAN-Hub für Produktionskonnektivität nutzen, sollten Sie ein virtuelles Spoke-Netzwerk und eine Virtual WAN-VNet-Verbindung einrichten. Stellen Sie sicher, dass die Verbindungen mit dieser Testumgebung über ExpressRoute und Site-to-Site-VPN funktionieren, bevor Sie mit den nächsten Schritten fortfahren.

:::image type="content" source="./media/migrate-from-hub-spoke-topology/figure4.png" alt-text="Testen der Hybridkonnektivität über Virtual WAN":::
**Abbildung 4: Migration von Hub-and-Spoke unter Kundenverwaltung zu Virtual WAN**

Zu diesem Zeitpunkt ist es wichtig zu erkennen, dass sowohl das ursprüngliche, kundenseitig verwaltete virtuelle Hubnetzwerk als auch der neue Virtual WAN-Hub mit derselben ExpressRoute-Verbindung verbunden sind. Dadurch entsteht ein Datenverkehrspfad, der verwendet werden kann, um Spokes in beiden Umgebungen für die Kommunikation zu aktivieren. Beispielsweise durchläuft der Datenverkehr von einem mit dem kundenseitig verwalteten virtuellen Hubnetzwerk verbundenen Spoke die MSEE-Geräte, die für die ExpressRoute-Verbindung verwendet werden. So werden alle über eine VNET-Verbindung mit dem neuen Virtual WAN-Hub verbundenen Spokes erreicht. Dies ermöglicht eine gestaffelte Migration von Spokes in Schritt 5.

### <a name="step-5-transition-connectivity-to-virtual-wan-hub"></a>Schritt 5: Umstellen der Konnektivität auf Virtual WAN-Hub

:::image type="content" source="./media/migrate-from-hub-spoke-topology/figure5.png" alt-text="Umstellen der Konnektivität auf Virtual WAN-Hub":::
**Abbildung 5: Migration von Hub-and-Spoke unter Kundenverwaltung zu Virtual WAN**

**a**: Löschen Sie die vorhandenen Peeringverbindungen zwischen den virtuellen Spoke-Netzwerken und dem alten kundenseitig verwalteten Hub. Der Zugriff auf Anwendungen in virtuellen Spoke-Netzwerken ist erst nach Abschluss der Schritte a bis c wieder möglich.

**b**. Verbinden Sie die virtuellen Spoke-Netzwerke über VNet-Verbindungen mit dem Virtual WAN-Hub.

**c**. Entfernen Sie alle benutzerdefinierten Routen (User-Defined Routes, UDR), die zuvor in den virtuellen Spoke-Netzwerken für die Spoke-to-Spoke-Kommunikation verwendet wurden. Dieser Pfad wird jetzt durch das dynamische Routing im Virtual WAN-Hub aktiviert.

**d**. Vorhandene ExpressRoute- und VPN-Gateways im kundenseitig verwalteten Hub werden nun außer Betrieb genommen, um die nächsten Schritte zu ermöglichen.

**e**. Verbinden Sie den alten kundenseitig verwalteten Hub (virtuelles Hubnetzwerk) über eine neue VNET-Verbindung mit dem Virtual WAN-Hub.

### <a name="step-6-old-hub-becomes-shared-services-spoke"></a>Schritt 6: Alter Hub wird zum Spoke für gemeinsame Dienste.

Wir haben nun unser Azure-Netzwerk überarbeitet, um den Virtual WAN-Hub zum zentralen Punkt in unserer neuen Topologie zu machen.

:::image type="content" source="./media/migrate-from-hub-spoke-topology/figure6.png" alt-text="Alter Hub wird zum Spoke für gemeinsame Dienste":::
**Abbildung 6: Migration von Hub-and-Spoke unter Kundenverwaltung zu Virtual WAN**

Da der Virtual WAN-Hub eine verwaltete Entität ist und die Bereitstellung benutzerdefinierter Ressourcen wie z.B. VMs nicht zulässig ist, ist der Block für gemeinsame Dienste jetzt ein virtuelles Spoke-Netzwerk. Dieses Netzwerk hostet Funktionen wie den eingehenden Internetdatenverkehr über Azure Application Gateway oder eine virtuelle Netzwerkappliance. Der Datenverkehr zwischen der Umgebung für gemeinsame Dienste und virtuellen Back-End-Computern durchläuft nun den von Virtual WAN verwalteten Hub.

### <a name="step-7-optimize-on-premises-connectivity-to-fully-utilize-virtual-wan"></a>Schritt 7: Optimieren der lokalen Konnektivität zur vollständigen Nutzung von Virtual WAN

In diesem Punkt hat Contoso die Migration von Geschäftsanwendungen in die Microsoft Cloud weitgehend abgeschlossen, wobei nur noch wenige Legacyanwendungen auf dem lokalen DC verbleiben.

:::image type="content" source="./media/migrate-from-hub-spoke-topology/figure7.png" alt-text="Optimieren der lokalen Konnektivität zur vollständigen Nutzung von Virtual WAN":::
**Abbildung 7: Migration von Hub-and-Spoke unter Kundenverwaltung zu Virtual WAN**

Um die vollständige Funktionalität von Azure Virtual WAN nutzen zu können, entscheidet sich Contoso dafür, die zuvor verwendeten lokalen VPN-Verbindungen außer Betrieb zu nehmen. Alle Zweigstellen, die weiterhin auf Hauptstandort- oder DC-Netzwerke zugreifen, können das globale Microsoft-Netzwerk mithilfe des integrierten Transitroutings von Azure Virtual WAN durchqueren.

> [!NOTE]
> ExpressRoute Global Reach ist für Kunden erforderlich, die den Microsoft-Backbone für die Bereitstellung von ExpressRoute für ExpressRoute-Transit (in Abbildung 7 nicht dargestellt) nutzen möchten.
>

## <a name="end-state-architecture-and-traffic-paths"></a>Endzustand von Architektur und Datenverkehrspfaden

:::image type="content" source="./media/migrate-from-hub-spoke-topology/figure8.png" alt-text="Endzustand von Architektur und Datenverkehrspfaden":::
**Abbildung: Virtual WAN mit zwei Regionen**

Dieser Abschnitt gibt anhand einiger Beispiele für Datenverkehrsflüsse einen Überblick darüber, wie diese Topologie den ursprünglichen Anforderungen entspricht.

### <a name="path-1"></a>Pfad 1

Pfad 1 zeigt den Datenverkehrsfluss von einer Zweigstelle mit S2S-VPN-Verbindung in Asien zum Azure-VNet in der Region „Asien, Südosten“.

Der Datenverkehr wird wie folgt weitergeleitet:

* Die Zweigstelle in Asien ist über resiliente S2S-BGP-fähige Tunnel mit dem Virtual WAN-Hub in der Region „Asien, Südosten“ verbunden.

* Der Virtual WAN-Hub in Asien leitet Datenverkehr lokal an das verbundene VNET weiter.

:::image type="content" source="./media/migrate-from-hub-spoke-topology/flow1.png" alt-text="Datenfluss 1":::

### <a name="path-2"></a>Pfad 2

Pfad 2 zeigt den Datenverkehrsfluss vom per ExpressRoute verbundenen Hauptstandort in Europa zum Azure-VNet in der Region „Asien, Südosten“.

Der Datenverkehr wird wie folgt weitergeleitet:

* Der Hauptstandort in Europa wird über eine ExpressRoute-Verbindung mit dem Virtual WAN-Hub in der Region „Europa, Westen“ verbunden.

* Die globale Hub-zu-Hub-Konnektivität von Virtual WAN ermöglicht den Transit des Datenverkehrs an ein in der Remoteregion verbundenes VNET.

:::image type="content" source="./media/migrate-from-hub-spoke-topology/flow2.png" alt-text="Datenfluss 2":::

### <a name="path-3"></a>Pfad 3

Pfad 3 zeigt den Datenverkehrsfluss vom lokalen, mit einem privaten WAN verbundenen Rechenzentrum in Asien zu einer europäischen Zweigstelle mit S2S-Verbindung.

Der Datenverkehr wird wie folgt weitergeleitet:

* Der DC in Asien ist mit einem Netzanbieter für das lokale private WAN verbunden.

* Die ExpressRoute-Verbindung ist lokal im privaten WAN terminiert, das mit dem Virtual WAN-Hub in der Region „Asien, Südosten“ verbunden ist.

* Die globale Hub-zu-Hub-Konnektivität von Virtual WAN ermöglicht den Transit des Datenverkehrs.

:::image type="content" source="./media/migrate-from-hub-spoke-topology/flow3.png" alt-text="Datenfluss 3":::

### <a name="path-4"></a>Pfad 4

Pfad 4 zeigt den Datenverkehrsfluss von einem Azure-VNet in der Region „Asien, Südosten“ zu einem Azure-VNet in der Region „Europa, Westen“.

Der Datenverkehr wird wie folgt weitergeleitet:

* Die globale Hub-to-Hub-Konnektivität von Virtual WAN ermöglicht einen nativen Transit für alle verbundenen Azure-VNETs, ohne dass eine weitere Benutzerkonfiguration erforderlich ist.

:::image type="content" source="./media/migrate-from-hub-spoke-topology/flow4.png" alt-text="Datenfluss 4":::

### <a name="path-5"></a>Pfad 5

Pfad 5 zeigt den Datenverkehrsfluss von Roaming-VPN-Benutzern (P2S) zu einem Azure-VNet in der Region „Europa, Westen“.

Der Datenverkehr wird wie folgt weitergeleitet:

* Laptop- und Mobilgerätebenutzer nutzen den OpenVPN-Client für eine transparente Konnektivität mit dem P2S-VPN-Gateway in der Region „Europa, Westen“.

* Der Virtual WAN-Hub in der Region „Europa, Westen“ leitet Datenverkehr lokal an das verbundene VNET weiter.

:::image type="content" source="./media/migrate-from-hub-spoke-topology/flow5.png" alt-text="Datenfluss 5":::

## <a name="security-and-policy-control-via-azure-firewall"></a>Sicherheits- und Richtliniensteuerung über Azure Firewall

Contoso hat nun die Konnektivität zwischen allen Zweigstellen und VNets gemäß den zuvor in diesem Artikel beschriebenen Anforderungen sichergestellt. Um die geltenden Anforderungen in Bezug auf Sicherheitssteuerung und Netzwerkisolierung zu erfüllen, muss das Unternehmen den Datenverkehr weiterhin über das Hubnetzwerk trennen und protokollieren. Bisher wurde diese Funktion von einer virtuellen Netzwerkappliance übernommen. Contoso möchte zudem die vorhandenen Proxydienste außer Betrieb nehmen und native Azure-Dienste für die Filterung des ausgehenden Internetdatenverkehrs nutzen.

:::image type="content" source="./media/migrate-from-hub-spoke-topology/security-policy.png" alt-text="Sicherheits- und Richtliniensteuerung über Azure Firewall":::
**Abbildung: Azure Firewall in Virtual WAN (geschützter virtueller Hub)**

Die folgenden allgemeinen Schritte sind für die Einführung von Azure Firewall in den Virtual WAN-Hubs erforderlich, um einen einheitlichen Ausgangspunkt für die Richtliniensteuerung zu ermöglichen. Weitere Informationen zu diesem Vorgang und zu geschützten virtuellen Hubs finden Sie unter [Dokumentation für Azure Firewall Manager](../firewall-manager/index.yml).

1. Erstellen Sie eine Azure Firewall-Richtlinie.
2. Verknüpfen Sie die Firewallrichtlinie mit dem Azure Virtual WAN-Hub. Durch diesen Schritt kann der vorhandene Virtual WAN-Hub als geschützter virtueller Hub fungieren und die erforderlichen Azure Firewall-Ressourcen bereitstellen.

> [!NOTE]
> Es gibt Einschränkungen in Bezug auf die Verwendung von gesicherten virtuellen Hubs, einschließlich des regionsübergreifenden Datenverkehrs. Weitere Informationen finden Sie unter [Firewall Manager – Bekannte Probleme](../firewall-manager/overview.md#known-issues).
>

Die folgenden Pfade zeigen die Konnektivitätspfade, die durch die Verwendung von geschützten virtuellen Azure-Hubs aktiviert werden:

### <a name="path-6"></a>Pfad 6

Pfad 6 zeigt den sicheren Datenverkehrsfluss für VNets innerhalb derselben Region.

Der Datenverkehr wird wie folgt weitergeleitet:

* Virtuelle Netzwerke, die mit demselben geschützten virtuellen Hub verbunden sind, leiten nun Datenverkehr über Azure Firewall weiter.

* Azure Firewall kann eine Richtlinie auf diese Datenflüsse anwenden.

:::image type="content" source="./media/migrate-from-hub-spoke-topology/flow6.png" alt-text="Datenfluss 6":::

### <a name="path-7"></a>Pfad 7

Pfad 7 zeigt den Datenverkehrsfluss von einem Azure-VNet zum Internet oder Sicherheitsdienst eines Drittanbieters.

Der Datenverkehr wird wie folgt weitergeleitet:

* Virtuelle Netzwerke, die mit dem geschützten virtuellen Hub verbunden sind, können den Datenverkehr an öffentliche Ziele im Internet senden, indem sie den geschützten Hub als zentralen Punkt für den Internetzugang verwenden.

* Dieser Datenverkehr kann lokal mithilfe von Azure Firewall-FQDN-Regeln gefiltert oder zur Überprüfung an einen Sicherheitsdienst eines Drittanbieters gesendet werden.

:::image type="content" source="./media/migrate-from-hub-spoke-topology/flow7.png" alt-text="Datenfluss 7":::

### <a name="path-8"></a>Pfad 8

Pfad 8 zeigt den Datenfluss von der Zweigstelle zum Internet oder an den Sicherheitsdienst eines Drittanbieters.

Der Datenverkehr wird wie folgt weitergeleitet:

* Zweigstellen, die mit dem geschützten virtuellen Hub verbunden sind, können den Datenverkehr an öffentliche Ziele im Internet senden, indem sie den geschützten Hub als zentralen Punkt für den Internetzugang verwenden.

* Dieser Datenverkehr kann lokal mithilfe von Azure Firewall-FQDN-Regeln gefiltert oder zur Überprüfung an einen Sicherheitsdienst eines Drittanbieters gesendet werden.

:::image type="content" source="./media/migrate-from-hub-spoke-topology/flow8.png" alt-text="Datenfluss 8":::

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über [Azure Virtual WAN](virtual-wan-about.md).
