---
title: Betriebsmodell von SAP HANA in Azure (große Instanzen) | Microsoft-Dokumentation
description: Erfahren Sie mehr über die SAP HANA-Funktionsprinzipien (große Instanzen) und Ihre Zuständigkeiten.
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
ms.service: virtual-machines-sap
ms.subservice: baremetal-sap
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/17/2021
ms.author: madhukan
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b54b2b6d7b1ca0381615a0b6683eccd78bb4b70c
ms.sourcegitcommit: e1d5abd7b8ded7ff649a7e9a2c1a7b70fdc72440
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2021
ms.locfileid: "110575320"
---
# <a name="operations-model-and-responsibilities"></a>Funktionsprinzipien und Zuständigkeiten

Der mit SAP HANA in Azure (große Instanzen) bereitgestellte Dienst ist auf Azure-IaaS-Dienste ausgerichtet. Sie erhalten eine Instanz von HANA (große Instanzen) mit einem installiertem Betriebssystem, das für SAP HANA optimiert ist. Wie bei Azure-IaaS-VMs sind Sie zuständig für die meisten Aufgaben im Zusammenhang mit dem Härten des Betriebssystems, Installieren weiterer Software, Installieren von HANA, Ausführen des Betriebssystems und von HANA sowie dem Aktualisieren des Betriebssystems und von HANA. Microsoft erzwingt keine Betriebssystemupdates oder HANA-Updates.

![Zuständigkeiten von SAP HANA in Azure (große Instanzen)](./media/hana-overview-architecture/image2-responsibilities.png)

Wie im vorherigen Diagramm dargestellt, ist SAP HANA in Azure (große Instanzen) ein IaaS-Angebot mit mehreren Mandanten. Die Trennung der Zuständigkeiten erfolgt zum größten Teil an der Infrastrukturgrenze zum Betriebssystem. Microsoft ist für alle Aspekte des Diensts unterhalb der Linie des Betriebssystems verantwortlich. Sie sind für alle Aspekte des Diensts oberhalb der Linie zuständig. Das Betriebssystem liegt in Ihrer Verantwortung. Sie können somit weiterhin die meisten aktuellen lokalen Methoden verwenden, die Sie für Konformität, Sicherheit, Anwendungsverwaltung, Basis und Betriebssystemverwaltung einsetzen. Die Systeme erscheinen so, als ob sie sich in Ihrem Netzwerk befinden würden.

Dieser Dienst ist für SAP HANA optimiert. Daher müssen Sie mit Microsoft zusammenarbeiten, um die Funktionen der zugrunde liegenden Infrastruktur optimal zu nutzen.

## <a name="your-responsibilities"></a>Ihre Zuständigkeiten

Die folgende Liste enthält weitere Details zu den einzelnen Schichten und Ihren Zuständigkeiten:

**Netzwerk**: Alle internen Netzwerke für das Umfeld der großen Instanz unter SAP HANA. Sie sind verantwortlich für den Zugriff auf den Speicher, die Konnektivität zwischen den Instanzen (für horizontale Skalierung und andere Funktionen), die Konnektivität mit der Landschaft und die Konnektivität mit Azure, wenn die SAP-Anwendungsschicht auf VMs gehostet wird. Darüber hinaus sind Sie für die WAN-Konnektivität zwischen Azure-Rechenzentren zur Replikation und Notfallwiederherstellung zuständig. Alle Netzwerke sind nach Mandanten partitioniert und unterliegen der Quality of Service (QoS).

**Speicher**: Der virtualisierte partitionierte Speicher für alle Volumes, die von den SAP HANA-Servern sowie für Momentaufnahmen benötigt werden. 

**Server**: Die dedizierten physischen Server zum Ausführen der SAP HANA-Datenbanken, die den Mandanten zugewiesen sind. Der Server der SKU-Klasse Typ I sind hardwareabstrahiert. Mit diesen Servertypen wird die Serverkonfiguration erfasst und in Profilen verwaltet, die von einer physischen Hardware auf eine andere physische Hardware verschoben werden können. Eine solche (manuelle) Verschiebung eines Profils durch Vorgänge ist mit der Azure-Dienstreparatur vergleichbar. Die Server der Typ-II-Klasse-SKU bieten diese Funktionen nicht.

**SDDC**: Die Verwaltungssoftware, die zum Verwalten von Rechenzentren als softwaredefinierte Entitäten verwendet wird. Sie ermöglicht Microsoft das Zusammenfassen von Ressourcen in Pools aus Skalierungs-, Verfügbarkeits- und Leistungsgründen.

**Betriebssystem**: Das von Ihnen ausgewählte Betriebssystem (SUSE Linux oder Red Hat Linux), das auf den Servern ausgeführt wird. Die Betriebssystemimages, die Ihnen zur Verfügung gestellt werden, wurden Microsoft von den einzelnen Linux-Anbietern zur Ausführung von SAP HANA bereitgestellt. Sie benötigen ein Abonnement des Linux-Anbieters für das jeweilige für SAP HANA optimierte Image. Sie sind für die Registrierung der Images beim Betriebssystemanbieter zuständig. 

Ab dem Zeitpunkt der Übergabe durch Microsoft sind Sie für das weitere Patchen des Linux-Betriebssystems verantwortlich. Dazu zählen auch hinzugefügte Pakete, die ggf. für eine erfolgreiche SAP HANA-Installation erforderlich sind und nicht in den für SAP HANA optimierten Betriebssystemimages des Linux-Anbieters enthalten waren. (Weitere Informationen finden Sie in der Installationsdokumentation für SAP HANA und SAP-Hinweisen.) 

Sie sind verantwortlich für das Patchen des Betriebssystems im Fall von Fehlfunktionen oder zur Optimierung des Betriebssystems und der zugehörigen Treiber im Zusammenhang mit der spezifischen Serverhardware. Sie sind auch für das sicherheits- oder funktionsbezogene Patchen des Betriebssystems zuständig. 

In Ihrer Verantwortung liegt die Überwachung und Kapazitätsplanung für Folgendes:

- CPU-Ressourcennutzung
- Arbeitsspeichernutzung
- Datenträgervolumes im Zusammenhang mit freiem Speicherplatz, IOPS und Wartezeit
- Netzwerkvolume-Datenverkehr zwischen HANA (große Instanz) und der SAP-Anwendungsschicht

Die zugrunde liegende Infrastruktur von HANA (große Instanz) bietet Funktionen zur Sicherung und Wiederherstellung des Betriebssystemvolumes. Die Nutzung dieser Funktionalität liegt in Ihrer Verantwortung.

**Middleware:** Hauptsächlich die SAP HANA-Instanz. Verwaltung, Betrieb und Überwachung liegen in Ihrer Verantwortung. Sie können Speichermomentaufnahmen zur Sicherung und Wiederherstellung sowie zur Notfallwiederherstellung verwenden. Diese Funktionen werden von der Infrastruktur bereitgestellt. Sie sind dafür verantwortlich, Hochverfügbarkeit oder Notfallwiederherstellung mit diesen Funktionen zu entwerfen und zu überwachen, um festzustellen, ob Speichermomentaufnahmen erfolgreich ausgeführt wurden.

**Daten**: Ihre von SAP HANA verwalteten Daten und andere Daten, beispielsweise Sicherungsdateien auf Volumes oder Dateifreigaben. Zu Ihren Zuständigkeiten zählen die Überwachung des freien Speicherplatzes und die Verwaltung des Inhalts auf den Volumes. Sie sind auch für die Überwachung der erfolgreichen Ausführung von Sicherungen von Datenträgervolumes und Speichermomentaufnahmen verantwortlich. Die erfolgreiche Ausführung der Datenreplikation an Notfallwiederherstellungsstandorten liegt in der Verantwortung von Microsoft.

**Anwendungen:** Die SAP-Anwendungsinstanzen oder bei Nicht-SAP-Anwendungen die Anwendungsschicht dieser Anwendungen. Ihre Zuständigkeiten umfassen die Bereitstellung, Verwaltung, Ausführung und Überwachung dieser Anwendungen. Sie sind verantwortlich für die Kapazitätsplanung für den CPU-Ressourcenverbrauch, die Arbeitsspeichernutzung, die Azure-Speichernutzung und die Netzwerk-Bandbreitennutzung innerhalb virtueller Netzwerke. Sie sind auch für die Kapazitätsplanung für den Ressourcenverbrauch zwischen virtuellen Netzwerken und SAP HANA in Azure (große Instanzen) zuständig.

**WANs:** Die Verbindungen, die Sie zwischen lokalen Systemen und Azure-Bereitstellungen für Workloads herstellen. Alle Kunden mit HANA (großen Instanzen) verwenden Azure ExpressRoute für die Konnektivität. Diese Verbindung ist nicht Teil von SAP HANA in Azure (große Instanzen). Sie sind für die Einrichtung dieser Verbindung zuständig.

**Archiv:** Möglicherweise möchten Sie Datenkopien mit Ihren eigenen Methoden in Speicherkonten archivieren. Die Archivierung erfordert Verwaltungsaufwand, Konformität, Kosten und Vorgänge. Sie sind für die Erstellung von Archivkopien und Sicherungen in Azure sowie für die konforme Speicherung verantwortlich.

Informationen hierzu finden Sie in der [SLA für SAP HANA in Azure (große Instanzen)](https://azure.microsoft.com/support/legal/sla/sap-hana-large/).

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über kompatible Betriebssysteme für HANA (große Instanzen).

> [!div class="nextstepaction"]
> [Kompatible Betriebssysteme für HANA (große Instanzen)](os-compatibility-matrix-hana-large-instance.md)
