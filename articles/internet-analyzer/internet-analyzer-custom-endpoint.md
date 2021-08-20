---
title: Erstellen eines benutzerdefinierten Endpunkts | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie einen benutzerdefinierten Endpunkt für die Messung mit Ihrer Internet Analyzer-Ressource konfigurieren.
services: internet-analyzer
author: KumudD
ms.service: internet-analyzer
ms.topic: how-to
ms.date: 10/16/2019
ms.author: kumud
ms.openlocfilehash: caf4f93dc1a9341a4dca3b7df110f263eb47fd16
ms.sourcegitcommit: 7d63ce88bfe8188b1ae70c3d006a29068d066287
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2021
ms.locfileid: "114455104"
---
# <a name="measure-custom-endpoints-to-evaluate-in-your-internet-analyzer-tests"></a>Messen von benutzerdefinierten Endpunkten für die Evaluierung in Internet Analyzer-Tests 

In diesem Artikel wird veranschaulicht, wie Sie einen benutzerdefinierten Endpunkt einrichten, der im Rahmen Ihrer Internet Analyzer-Tests gemessen werden soll. Benutzerdefinierte Endpunkte helfen bei der Evaluierung von lokalen Workloads, Workloads, die von anderen Cloudanbietern ausgeführt werden, und benutzerdefinierten Azure-Konfigurationen.  Der Vergleich von zwei benutzerdefinierten Endpunkten in einem Test ist möglich, wenn es sich bei einem Endpunkt um eine Azure-Ressource handelt. Weitere Informationen zu Internet Analyzer finden Sie in der [Übersicht](internet-analyzer-overview.md). 

> [!IMPORTANT]
> Diese öffentliche Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und sollte nicht für Produktionsworkloads verwendet werden. Unter Umständen werden bestimmte Features nicht unterstützt, verfügen über eingeschränkte Funktionen und sind nicht an allen Azure-Standorten verfügbar. Weitere Informationen finden Sie unter [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="before-you-begin"></a>Voraussetzungen

Stellen Sie sicher, dass eine Internet Analyzer-Ressource eingerichtet wurde, und wählen Sie die Option „Benutzerdefinierter Endpunkt“ aus. Internet Analyzer geht davon aus, dass über das Internet auf den benutzerdefinierten Endpunkt zugegriffen werden kann. Weitere Informationen finden Sie unter [Erstellen einer Internet Analyzer-Ressource](internet-analyzer-create-test-portal.md).


## <a name="create-custom-endpoint"></a>Erstellen eines benutzerdefinierten Endpunkts

1. Laden Sie [hier](https://fpc.msedge.net/apc/trans.gif) ein transparentes 1 Pixel großes Testbild herunter. Dieses 1 Pixel große Bild ist die Ressource, die der JavaScript-Client zum Messen der Leistung abruft.
2. Stellen Sie das Testbild in Ihrer benutzerdefinierten Webanwendung an einem öffentlich zugänglichen Pfad bereit. Der Pfad muss über HTTPS erreichbar sein. 
3. Kopieren Sie während der Testerstellung die vollständige URL des benutzerdefinierten Endpunkts (z. B. `https://contoso.com/test/trans.gif`) in das Feld des benutzerdefinierten Endpunkts.

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die [häufig gestellten Fragen zu Internet Analyzer](internet-analyzer-faq.md).

