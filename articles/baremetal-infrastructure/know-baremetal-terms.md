---
title: Wichtige Begriffe in der Azure BareMetal-Infrastruktur
description: Machen Sie sich mit wichtigen Begriffen in der Azure BareMetal-Infrastruktur vertraut.
ms.topic: conceptual
ms.date: 07/13/2021
ms.openlocfilehash: a4bf8b24bc412215f595e12128c0a63f40e8fbfd
ms.sourcegitcommit: 9339c4d47a4c7eb3621b5a31384bb0f504951712
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/14/2021
ms.locfileid: "113767115"
---
# <a name="know-the-terms-for-baremetal-infrastructure"></a>Wichtige Begriffe in der BareMetal-Infrastruktur

In diesem Artikel werden einige wichtige Begriffe im Zusammenhang mit der BareMetal-Infrastruktur behandelt.

- **Revision**: Es gibt zwei unterschiedliche Stamprevisionen für Stamps der Bare-Metal-Infrastruktur (große HANA-Instanz). Diese Revisionen unterscheiden sich in der Architektur und in der Nähe zu Hosts für virtuelle Computer in Azure:
    - „Revision 3“ (Rev 3): Der ursprüngliche Entwurf, der Mitte 2016 bereitgestellt wurde.
    - „Revision 4.2“ (Rev 4.2): Ein neuer Entwurf, der eine größere Nähe zu Hosts für virtuelle Computer in Azure mit äußerst niedriger Netzwerklatenz zwischen virtuellen Azure-Computern und großen HANA-Instanzen bietet. Ressourcen im Azure-Portal werden als „Bare-Metal-Infrastruktur“ bezeichnet, und Kunden können über das Azure-Portal auf ihre Ressourcen als Bare-Metal-Instanzen zugreifen.

- **Stamp**: Definiert die interne Microsoft-Bereitstellungsgröße von BareMetal-Instanzen. Bevor Instanzen bereitgestellt werden können, muss ein Stamp von BareMetal-Instanzen, der aus Compute-, Netzwerk- und Speicherracks besteht, an einem Rechenzentrumsstandort bereitgestellt werden. Eine solche Bereitstellung wird als Stamp einer BareMetal-Instanz bezeichnet.

- **Mandant**: Ein Kunde, der einen BareMetal-Instanzstamp bereitstellt, wird als *Mandant* isoliert. Ein Mandant wird in der Schicht der Netzwerke, Speicher und von Compute von anderen Mandanten isoliert. Speicher- und Compute-Einheiten, die unterschiedlichen Mandanten zugewiesen sind, können einander auf der Ebene des Stamps der BareMetal-Instanz nicht sehen und nicht miteinander kommunizieren. Ein Kunde kann Bereitstellungen in unterschiedlichen Mandanten durchführen. Auch dann ist die Kommunikation auf der Ebene des Stamps der BareMetal-Instanz nicht möglich.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Bare-Metal-Infrastruktur.

> [!div class="nextstepaction"]
> [BareMetal-Infrastruktur](concepts-baremetal-infrastructure-overview.md)
