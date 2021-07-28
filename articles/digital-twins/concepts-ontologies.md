---
title: Was ist eine Ontologie?
titleSuffix: Azure Digital Twins
description: Hier erfahren Sie mehr über branchenspezifische DTDL-Ontologien für die Modellierung in einer bestimmten Domäne.
author: baanders
ms.author: baanders
ms.date: 2/12/2021
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: 17411f3c0a12e544d3204b2194e53d9565b164a9
ms.sourcegitcommit: 23040f695dd0785409ab964613fabca1645cef90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2021
ms.locfileid: "112061900"
---
# <a name="what-is-an-ontology"></a>Was ist eine Ontologie? 

Das Vokabular einer Azure Digital Twins-Lösung wird mit [Modellen](concepts-models.md)definiert, die die Arten von Entitäten in Ihrer Umgebung beschreiben.

Wenn es sich bei Ihrer Lösung um eine branchenspezifische Lösung handelt, kann es manchmal einfacher und effektiver sein, mit einer Gruppe bereits vorhandener Modelle für diese Branche zu arbeiten statt ein eigenes Modell von Grund auf neu zu erstellen. Diese Gruppen bereits vorhandener Modelle werden **Ontologien** genannt. 

Allgemein ist eine Ontologie eine Gruppe von Modellen für eine bestimmte Domäne (z. B. eine Gebäudestruktur, ein IoT-System, eine Smart City, ein Energienetz oder Webinhalte). Ontologien werden häufig als Schemas für Zwillingsgraphen verwendet, da sie Folgendes ermöglichen können:
* Harmonisierung von Softwarekomponenten, Dokumentation, Abfragebibliotheken usw.
* Geringerer Investitionsaufwand für konzeptionelle Modellierung und Systementwicklung
* Einfachere Interoperabilität von Daten auf Semantikebene
* Wiederverwendung unter Berücksichtigung bewährter Methoden statt von Grund auf neu zu beginnen oder das Rad neu zu erfinden

In diesem Artikel werden die Verwendung von Ontologien für Ihre Azure Digital Twins-Modelle und Gründe dafür erläutert. Außerdem erfahren Sie, welche Ontologien und Tools Ihnen für diese Modelle aktuell zur Verfügung stehen.

## <a name="using-ontologies-for-azure-digital-twins"></a>Verwenden von Ontologien für Azure Digital Twins

Ontologien sind ein hervorragender Ausgangspunkt für Lösungen mit digitalen Zwillingen. Sie umfassen eine Reihe domänenspezifischer Modelle und Beziehungen zwischen Entitäten zum Entwerfen, Erstellen und Analysieren von Graphen für digitale Zwillinge. Mit ihrer Hilfe können Lösungsentwickler bei Lösungen mit digitalen Zwillingen an einem bewährten Ausgangspunkt beginnen und sich auf das Lösen von Geschäftsproblemen konzentrieren. Die von Microsoft bereitgestellten Ontologien sind so gestaltet, dass sie leicht erweitert werden können, sodass Sie sie für Ihre Lösung anpassen können. 

Außerdem können diese Ontologien durch die Verwendung in Ihren Lösungen eine nahtlosere Integration zwischen verschiedenen Partnern und Herstellern ermöglichen, da sie ein gemeinsames Vokabular für verschiedene Lösungen bieten können.

Da Modelle in Azure Digital Twins in [DTDL (Digital Twins Definition Language)](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md) dargestellt werden, sind die Ontologien für Azure Digital Twins ebenfalls in DTDL geschrieben. 

## <a name="strategies-for-integrating-ontologies"></a>Strategien für die Integration von Ontologien

Es gibt drei mögliche Strategien für das Integrieren von Branchenstandardontologien mit DTDL. Sie können die Strategie verwenden, die basierend auf Ihren Anforderungen am besten für Sie geeignet ist:

| Strategie | BESCHREIBUNG | Ressourcen |
| --- | --- | --- |
| **Übernehmen** | Sie können Ihre Lösung mit einer Open-Source-DTDL-Ontologie beginnen, die auf weit verbreiteten Branchenstandards basiert. Sie können diese Gruppen von Modellen entweder so verwenden, wie sie bereitgestellt werden, oder um eigene Ergänzungen für eine benutzerdefinierte Lösung erweitern. | [Konzepte:&nbsp;Übernehmen&nbsp;von&nbsp;Branchenstandardontologien](concepts-ontologies-adopt.md)<br><br>[Konzepte:&nbsp;Erweitern&nbsp;von Ontologien](concepts-ontologies-extend.md) |
| **Konvertieren** | Wenn Sie bereits über vorhandene Modelle in einem anderen Standardformat verfügen, können Sie diese für die Verwendung in Azure Digital Twins in DTDL konvertieren. | [Konzepte:&nbsp;Konvertieren&nbsp; von Ontologien](concepts-ontologies-convert.md)<br><br>[Konzepte:&nbsp;Erweitern&nbsp;von Ontologien](concepts-ontologies-extend.md) |
| **Autor** | Sie können sich immer von anwendbaren Branchenstandards inspirieren lassen und eigene benutzerdefinierte DTDL-Modelle von Grund auf neu entwickeln. | [Konzepte: DTDL-Modelle](concepts-models.md) |

### <a name="using-ontology-strategies-in-a-model-development-path"></a>Verwenden von Ontologiestrategien in einem Modellentwicklungspfad

Unabhängig davon, für welche Strategie zur Integration einer Ontologie in Azure Digital Twins Sie sich entscheiden, kann der gesamte folgende Pfad Sie durch das Erstellen und Hochladen Ihrer Ontologie als DTDL-Modelle führen.

1. Beginnen Sie damit, sich über [DTDL-Modellierung in Azure Digital Twins](concepts-models.md) zu informieren und ein Verständnis dafür zu entwickeln.
1. Fahren Sie dann mit der gewählten Strategie für die Ontologieintegration von oben fort: [Übernehmen](concepts-ontologies-adopt.md), [konvertieren](concepts-ontologies-convert.md) oder [erstellen](concepts-models.md) Sie Ihre Modelle basierend auf Ihrer Ontologie.
    1. [Erweitern](concepts-ontologies-extend.md) Sie ggf. Ihre Ontologie, um sie an Ihre Anforderungen anzupassen.
1. [Überprüfen](how-to-parse-models.md) Sie Ihre Modelle, um sicherzustellen, dass es sich um funktionierende DTDL-Dokumente handelt.
1. Laden Sie Ihre fertigen Modelle über die [APIs](how-to-manage-model.md#upload-models) oder eine Beispielanwendung wie [Azure Digital Twins Model Uploader](https://github.com/Azure/opendigitaltwins-tools/tree/master/ADTTools#uploadmodels) in Azure Digital Twins hoch.

Danach sollten Sie Ihre Modelle in Ihrer Azure Digital Twins-Instanz verwenden können. 

>[!TIP]
> Sie können die Modelle in Ihrer Ontologie mit dem [Azure Digital Twins-Explorer](concepts-azure-digital-twins-explorer.md) oder dem [Azure Digital Twins-Modellvisualisierer](https://github.com/Azure/opendigitaltwins-building-tools/tree/master/AdtModelVisualizer) visualisieren.

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie weitere Artikel über die Strategien des Übernehmens, Konvertierens und Erstellens von Ontologien:
* [Konzepte: Übernehmen von Branchenstandardontologien](concepts-ontologies-adopt.md)
* [Konzepte: Konvertieren von Ontologien](concepts-ontologies-convert.md)
* [Vorgehensweise: Verwalten von DTDL-Modellen](how-to-manage-model.md)

Oder informieren Sie sich darüber, wie mit Modellen digitale Zwillinge erstellt werden: [Konzepte: Grundlegendes zu digitalen Zwillingen und zum zugehörigen Zwillingsgraphen](concepts-twins-graph.md).