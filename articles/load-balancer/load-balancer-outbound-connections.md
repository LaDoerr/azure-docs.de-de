---
title: SNAT (Source Network Address Translation, Quellnetzwerkadress-Übersetzung) für ausgehende Verbindungen
titleSuffix: Azure Load Balancer
description: Erfahren Sie, wie Azure Load Balancer für ausgehende Internetverbindungen (SNAT) verwendet wird.
services: load-balancer
author: asudbring
ms.service: load-balancer
ms.topic: conceptual
ms.custom: contperf-fy21q1
ms.date: 05/05/2021
ms.author: allensu
ms.openlocfilehash: f0c8d42538cd437d4817f75552133efca96b8d6e
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110451820"
---
# <a name="using-source-network-address-translation-snat-for-outbound-connections"></a>Verwendung von SNAT (Source Network Address Translation) für ausgehende Verbindungen

Bestimmte Szenarien erfordern, dass virtuelle Computer oder Compute-Instanzen über ausgehende Verbindungen mit dem Internet verfügen. Mithilfe der Front-End-IP-Adressen einer öffentlichen Azure Load Balancer-Instanz können ausgehende Verbindungen mit dem Internet für Back-End-Instanzen bereitgestellt werden. Bei dieser Konfiguration wird die **Quell-Netzwerkadressenübersetzung (Source Network Address Translation, SNAT)** verwendet, da die IP-Adresse der **Quelle** oder des virtuellen Computers in eine öffentliche IP-Adresse übersetzt wird. Die IP-Adresse des Back-Ends wird von SNAT auf die öffentliche IP-Adresse Ihrer Load Balancer-Instanz abgebildet. Dies verhindert, dass externe Quellen eine direkte Adresse für die Back-End-Instanzen abrufen können.

## <a name="azures-outbound-connectivity-methods"></a><a name="scenarios"></a>Methoden für ausgehende Verbindungen in Azure

Ausgehende Verbindungen mit dem Internet können auf folgende Weise aktiviert werden:

| # | Methode | Typ der Portzuordnung | Produktionsqualität? | Rating |
| ------------ | ------------ | ------ | ------------ | ------------ |
| 1 | Nutzen der Front-End-IP-Adressen eines Load Balancers für ausgehende Daten über Ausgangsregeln | Statisch, explizit | Ja, aber nicht im großen Stil | OK | 
| 2 | Zuordnen eines NAT-Gateways zum Subnetz | Statisch, explizit | Yes | Sehr hoch | 
| 3 | Zuweisen einer öffentlichen IP-Adresse zum virtuellen Computer | Statisch, explizit | Yes | OK | 
| 4 | Nutzen der Front-End-IP-Adressen eines Load Balancers für ausgehende (und eingehende) Daten | Standard, implizit | No | Minimum


## <a name="leveraging-the-frontend-ip-address-of-a-load-balancer-for-outbound-via-outbound-rules"></a><a name="outboundrules"></a>Nutzen der Front-End-IP-Adresse eines Load Balancers für ausgehende Daten über Ausgangsregeln

Mit Ausgangsregeln können Sie die SNAT (Quell-Netzwerkadressenübersetzung) für eine öffentliche Load Balancer Standard-Instanz explizit definieren. In dieser Konfiguration können Sie die öffentliche IP-Adresse Ihrer Load Balancers-Instanz verwenden, um ausgehende Internetkonnektivität für Ihre Back-End-Instanzen bereitzustellen.

Diese Konfiguration ermöglicht Folgendes:

- IP-Maskierung
- Vereinfachen ihrer Positivlisten.
- Verringert die Anzahl öffentlicher IP-Ressourcen für die Bereitstellung.

Mit Ausgangsregeln besitzen Sie vollständige deklarative Kontrolle über ausgehende Internetkonnektivität. Ausgangsregeln ermöglichen Ihnen das Skalieren und Optimieren dieser Fähigkeit gemäß Ihren speziellen Anforderungen.

Weitere Informationen zu Ausgangsregeln finden Sie unter [Ausgangsregeln](outbound-rules.md).

>[!Important]
> Wenn ein Back-End-Pool über eine IP-Adresse konfiguriert wird, verhält er sich wie ein Load Balancer im Tarif „Basic“ mit aktivierter Standardeinstellung für ausgehenden Datenverkehr. Konfigurieren Sie den Back-End-Pool nach NIC, um standardmäßig eine sichere Konfiguration zu gewährleisten und Anwendungen mit hohen Anforderungen für ausgehenden Datenverkehr nutzen zu können.

## <a name="associating-a-vnet-nat-to-the-subnet"></a>Zuordnen einer VNet NAT zum Subnetz

Virtual Network NAT (Network Address Translation, Netzwerkadressenübersetzung) vereinfacht die Einrichtung von ausschließlich ausgehender Internetkonnektivität für virtuelle Netzwerke. Bei der Konfiguration in einem Subnetz werden für die gesamte Konnektivität in ausgehender Richtung die von Ihnen angegebenen statischen öffentlichen IP-Adressen verwendet. Die ausgehende Konnektivität ist möglich, ohne dass ein Lastenausgleich oder öffentliche IP-Adressen direkt virtuellen Computern zugeordnet werden. NAT ist vollständig verwaltet und äußerst resilient.

Die Verwendung einer VNet NAT ist die beste Methode für ausgehende Verbindungen, da sie hochgradig skalierbar und zuverlässig ist und in Bezug auf die SNAT-Portauslastung nicht denselben Bedenken unterliegt.

Weitere Informationen zu Azure Virtual Network NAT finden Sie unter [Was ist Azure Virtual Network NAT?](../virtual-network/nat-overview.md)

##  <a name="assigning-a-public-ip-to-the-virtual-machine"></a>Zuweisen einer öffentlichen IP-Adresse zum virtuellen Computer

 | Associations | Methode | IP-Protokolle |
 | ---------- | ------ | ------------ |
 | Öffentliche IP-Adresse auf der NIC des virtuellen Computers | [SNAT (Source Network Address Translation)](#snat) </br> wird nicht verwendet. | TCP (Transmission Control Protocol) </br> UDP (User Datagram Protocol) </br> ICMP (Internet Control Message Protocol) </br> ESP (Encapsulating Security Payload) |

 Der gesamte Datenverkehr wird von der öffentlichen IP-Adresse des virtuellen Computers (IP-Adresse auf Instanzebene) wieder an den anfordernden Client geleitet.
 
 In Azure wird die öffentliche IP-Adresse verwendet, die der IP-Konfiguration der NIC einer Instanz für alle ausgehenden Datenflüsse zugewiesen ist. Für die Instanz sind alle kurzlebigen Ports verfügbar. Es ist unerheblich, ob der virtuelle Computer einen Lastenausgleich aufweist oder nicht. Dieses Szenario hat Vorrang vor den anderen Szenarien. 

 Eine öffentliche IP-Adresse, die einem virtuellen Computer zugewiesen ist, ist eine 1:1-Beziehung (keine 1:n-Beziehung) und wird als zustandslose 1:1-NAT implementiert.



## <a name="leveraging-the-frontend-ip-address-of-a-load-balancer-for-outbound-and-inbound"></a><a name="snat"></a> Nutzen der Front-End-IP-Adresse eines Load Balancers für ausgehende (und eingehende) Daten
>[!NOTE]
> Diese Methode wird für Produktionsworkloads **NICHT empfohlen**, da sie das Risiko einer Portüberlastung erhöht. Verwenden Sie diese Methode nicht für Produktionsworkloads, um potenzielle Verbindungsfehler zu vermeiden. 


Wenn für die Back-End-Ressourcen eines Load Balancers keine Ausgangsregeln, öffentliche IP-Adressen auf Instanzebene oder eine VNet NAT konfiguriert sind, richten sie ausgehende Verbindungen über die Front-End-IP des Load Balancers ein. Dies wird als Standard-SNAT bezeichnet.


### <a name="what-are-snat-ports"></a>Was sind SNAT-Ports?
Ports werden verwendet, um eindeutige Bezeichner zu generieren, mit denen unterschiedliche Datenflüsse verwaltet werden. Das Internet verwendet ein 5-Tupel, um diesen Unterschied zu gewährleisten.

Wenn ein Port für eingehende Verbindungen verwendet wird, verfügt er über einen **Listener** für eingehende Verbindungsanforderungen an diesem Port. Dieser Port kann nicht für ausgehende Verbindungen verwendet werden. Um eine ausgehende Verbindung herzustellen, wird ein **kurzlebiger Port** verwendet, um dem Ziel einen Port bereitzustellen, über den ein eindeutiger Datenverkehrsfluss kommuniziert und verwaltet werden kann. Wenn diese kurzlebigen Ports für SNAT verwendet werden, bezeichnet man sie als **SNAT-Ports**. 

Per Definition umfasst jede IP-Adresse 65.535 Ports. Jeder Port kann entweder für eingehende oder ausgehende Verbindungen für TCP (Transmission Control Protocol) und UDP (User Datagram Protocol) verwendet werden. Wenn einem Lastenausgleich eine öffentliche IP-Adresse als Front-End-IP-Adresse hinzugefügt wird, werden in Azure 64.000 Ports für SNAT freigegeben.

Jeder Port, der für eine Lastenausgleichsregel oder NAT-Regel für eingehenden Datenverkehr verwendet wird, nutzt von diesen 64.000 Ports einen Bereich von acht Ports, wodurch die Anzahl der für SNAT verfügbaren Ports verringert wird. Wenn eine Lastenausgleichsregel oder NAT-Regel für eingehenden Datenverkehr denselben Bereich von acht Ports einschließt, werden keine zusätzlichen Ports genutzt. 

### <a name="how-does-default-snat-work"></a>Wie funktioniert Standard-SNAT?
Wenn eine VM einen ausgehenden Datenfluss erstellt, übersetzt Azure die Quell-IP-Adresse in die öffentliche IP-Adresse des öffentlichen Load Balancer-Front-Ends. Diese Übersetzung erfolgt über [SNAT](#snat). 

Bei Verwendung von Standard-SNAT über eine Lastenausgleichsregel werden SNAT-Ports vorab zugeordnet, wie in der [Tabelle zur Zuordnung von Standard-SNAT-Ports](#snatporttable) angegeben.
 
Bei Verwendung einer internen Instanz von Load Balancer Standard werden keine kurzlebigen IP-Adressen für SNAT verwendet. Diese Funktion unterstützt standardmäßig die Sicherheit. Mit dieser Funktion wird sichergestellt, dass alle von Ressourcen verwendeten IP-Adressen konfigurierbar sind und reserviert werden können. Um die ausgehende Konnektivität mit dem Internet zu erreichen, wenn Sie eine interne Instanz von Load Balancer Standard verwenden, konfigurieren Sie:
- eine öffentliche IP-Adresse auf Instanzebene 
- VNet NAT
-  Back-End-Instanzen einer öffentlichen Load Balancer Standard-Instanz mit einer konfigurierten Ausgangsregel.  

### <a name="what-is-the-ip-for-default-snat"></a>Wie sieht die IP-Adresse für Standard-SNAT aus?
Wenn die VM einen ausgehenden Datenfluss erstellt, übersetzt Azure die Quell-IP-Adresse in eine dynamisch zugeordnete öffentliche Quell-IP-Adresse. Diese öffentliche IP-Adresse **ist nicht konfigurierbar** und kann nicht reserviert werden. Diese Adresse wird nicht auf den Grenzwert der öffentlichen IP-Ressourcen des Abonnements angerechnet. 

Die öffentliche IP-Adresse wird freigegeben und eine neue öffentliche IP-Adresse wird angefordert, wenn Sie Folgendes erneut bereitstellen: 
 * Virtual Machine
 * Verfügbarkeitsgruppe
 * VM-Skalierungsgruppe 

>[!NOTE]
> Diese Methode wird für Produktionsworkloads **NICHT empfohlen**, da sie das Risiko einer Portüberlastung erhöht. Verwenden Sie diese Methode nicht für Produktionsworkloads, um potenzielle Verbindungsfehler zu vermeiden. 

| Typ | Ausgehendes Verhalten | 
 | ------------ | ------ | 
 | Öffentlicher Load Balancer Standard | Verwendung von Load Balancer-Front-End-IP-Adressen für SNAT 
 | Interner Load Balancer Standard | Keine Internetverbindung, standardmäßig sicher | 
Öffentlicher Load Balancer Basic | Verwendung von Load Balancer-Front-End-IP-Adressen für SNAT |
Interner Load Balancer Basic | SNAT mit unbekannter dynamischer IP-Adresse| 

## <a name="exhausting-ports"></a> Ausschöpfung von Ports

Für jede Verbindung mit derselben IP-Adresse und demselben Zielport wird ein SNAT-Port verwendet. Diese Verbindung verwaltet einen eindeutigen **Datenverkehrsfluss** von der Back-End-Instanz oder dem **Client** zu einem **Server**. Durch diesen Prozess erhält der Server einen eindeutigen Port, an den Datenverkehr adressiert werden soll. Ohne diesen Prozess weiß der Clientcomputer nicht, zu welchem Datenfluss ein Paket gehört.

Stellen Sie sich vor, dass mehrere Browser https://www.microsoft.com verwenden:

* IP-Zieladresse = 23.53.254.142
* Zielport = 443
* Protokoll = TCP

Ohne unterschiedliche Zielports für den Rückgabedatenverkehr (der SNAT-Port, der zum Herstellen der Verbindung verwendet wird) hat der Client keine Möglichkeit, ein Abfrageergebnis von einem anderen zu trennen.

Für ausgehende Verbindungen kann ein Burst auftreten. Einer Back-End-Instanz können nicht genügend Ports zugeordnet werden. Wenn **Wiederverwendung von Verbindungen** nicht aktiviert ist, wird das Risiko von SNAT-**Porterschöpfung** gesteigert.

Neue ausgehende Verbindungen mit einer IP-Zieladresse schlagen fehl, wenn Porterschöpfung auftritt. Verbindungen werden erfolgreich hergestellt, wenn ein Port verfügbar wird. Diese Erschöpfung tritt auf, wenn die 64.000 Ports von einer IP-Adresse dünn über viele Back-End-Instanzen verteilt sind. Anleitungen zur Entschärfung von SNAT-Porterschöpfung finden Sie im [Leitfaden zur Problembehandlung](./troubleshoot-outbound-connection.md).  

Bei TCP-Verbindungen verwendet der Load Balancer einen einzelnen SNAT-Port für jede Ziel-IP und jeden -Port. Diese Mehrfachnutzung ermöglicht mehrere Verbindungen mit derselben IP-Zieladresse und demselben SNAT-Port. Diese Mehrfachnutzung ist eingeschränkt, wenn die Verbindung nicht mit verschiedenen Zielports erfolgt.

Für UDP-Verbindungen verwendet der Load Balancer einen **porteingeschränkten Cone NAT**-Algorithmus, der einen SNAT-Port pro IP-Zieladresse für den Zielport verwendet. 

Ein Port wird für eine unbegrenzte Anzahl von Verbindungen wiederverwendet. Der Port wird nur wiederverwendet, wenn die IP-Zieladresse oder der -port unterschiedlich ist.

## <a name="default-port-allocation"></a><a name="preallocatedports"></a> Standardportzuordnung

Jede öffentliche IP-Adresse, die als Front-End-IP-Adresse Ihres Load Balancers zugewiesen ist, erhält 64.000 SNAT-Ports für die zugehörigen Back-End-Elemente. Ports können nicht für Back-End-Poolelemente freigegeben werden. Ein Bereich von SNAT-Ports kann nur von einer einzelnen Back-End-Instanz verwendet werden, um sicherzustellen, dass Rückgabepakete ordnungsgemäß weitergeleitet werden. 

Sollten Sie automatische Zuordnung von ausgehendem SNAT durch eine Lastausgleichsregel verwenden, definiert die Zuordnungstabelle Ihre Portzuordnung.

In der folgenden <a name="snatporttable"></a>-Tabelle werden die SNAT-Port-Vorabzuordnungen für die Ebenen der Back-End-Poolgrößen angegeben:

| Poolgröße (VM-Instanzen) | Standard-SNAT-Ports pro IP-Konfiguration |
| --- | --- |
| 1-50 | 1\.024 |
| 51-100 | 512 |
| 101-200 | 256 |
| 201-400 | 128 |
| 401-800 | 64 |
| 801-1.000 | 32 | 

## <a name="constraints"></a>Einschränkungen

*   Wenn sich eine Verbindung im Leerlauf befindet und keine neuen Pakete gesendet werden, werden die Ports nach 4 bis 120 Minuten freigegeben.
  * Dieser Schwellenwert kann über Ausgangsregeln konfiguriert werden.
*   Jede IP-Adresse stellt 64.000 Ports bereit, die für SNAT verwendet werden können.
*   Jeder Port kann sowohl für TCP- als auch für UDP-Verbindungen mit einer IP-Zieladresse verwendet werden.
  * Ein UDP-SNAT-Port ist unabhängig davon erforderlich, ob der Zielport eindeutig ist. Für jede UDP-Verbindung mit einer IP-Zieladresse wird ein UDP-SNAT-Port verwendet.
  * Ein TCP-SNAT-Port kann für mehrere Verbindungen mit derselben IP-Zieladresse verwendet werden, vorausgesetzt, die Zielports unterscheiden sich.
*   SNAT-Erschöpfung tritt auf, wenn einer Back-End-Instanz die angegebenen SNAT-Ports ausgehen. Ein Load Balancer kann weiterhin nicht verwendete SNAT-Ports aufweisen. Wenn die von einer Back-End-Instanz verwendeten SNAT-Ports die angegebenen SNAT-Ports überschreiten, können keine neuen ausgehenden Verbindungen hergestellt werden.

## <a name="next-steps"></a>Nächste Schritte

*   [Problembehandlung von Fehlern bei ausgehenden Verbindungen aufgrund von SNAT-Erschöpfung](./troubleshoot-outbound-connection.md)
*   [Überprüfen Sie die SNAT-Metriken](./load-balancer-standard-diagnostics.md#how-do-i-check-my-snat-port-usage-and-allocation), und machen Sie sich mit den richtigen Verfahren zum Filtern, Aufteilen und Anzeigen dieser Metriken vertraut.
