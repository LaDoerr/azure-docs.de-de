---
title: Zuordnen von Konsistenzebenen für die Azure Cosmos DB-API für MongoDB
description: Hier wird das Zuordnen von Konsistenzebenen für die Azure Cosmos DB-API für MongoDB erläutert.
author: gahl-levy
ms.author: gahllevy
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 10/12/2020
ms.reviewer: sngun
ms.openlocfilehash: fa434fc45b46353d04a22000f9f2f252898cb715
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122345870"
---
# <a name="consistency-levels-for-azure-cosmos-db-and-the-api-for-mongodb"></a>Konsistenzebenen für Azure Cosmos DB und die API für MongoDB
[!INCLUDE[appliesto-mongodb-api](../includes/appliesto-mongodb-api.md)]

Im Gegensatz zu Azure Cosmos DB bietet MongoDB nativ keine genau definierten Konsistenzgarantien. Stattdessen ermöglicht MongoDB Benutzern nativ die Konfiguration folgender Konsistenzgarantien: eine Schreibbestätigung, eine Lesebestätigung und die „isMaster“-Anweisung, um die Lesevorgänge entweder zu primären oder sekundären Replikaten zu leiten, damit die gewünschte Konsistenzebene erreicht wird.

Wenn Sie die Azure Cosmos DB-API für MongoDB verwenden, behandelt der MongoDB-Treiber Ihren Schreibbereich als primäres Replikat und alle anderen Bereiche als Lesereplikat. Sie können wählen, welche mit Ihrem Azure Cosmos-Konto verknüpfte Region das primäre Replikat ist.

> [!NOTE]
> Das Standardkonsistenzmodell für Azure Cosmos DB lautet „Sitzung“. „Sitzung“ ist ein clientorientiertes Konsistenzmodell, das nicht nativ von Cassandra oder MongoDB unterstützt wird. Weitere Informationen dazu, welches Konsistenzmodell gewählt werden sollte, finden Sie unter [Konsistenzebenen in Azure Cosmos DB](../consistency-levels.md).

Bei Verwenden der Azure Cosmos DB-API für MongoDB gilt Folgendes:

* Die Schreibbestätigung wird der Standardkonsistenzebene zugeordnet, die in Ihrem Azure Cosmos-Konto konfiguriert ist.

* Azure Cosmos DB ordnet die vom MongoDB-Clienttreiber angegebene Lesebestätigung dynamisch einer der Azure Cosmos DB-Konsistenzebenen zu, die bei einer Leseanforderung dynamisch konfiguriert wird.  

* Sie können eine bestimmte mit Ihrem Azure Cosmos-Konto verknüpfte Region als „Primär“ kennzeichnen, indem Sie die Region als erste beschreibbare Region festlegen. 

## <a name="mapping-consistency-levels"></a>Zuordnen von Konsistenzebenen

Die folgende Tabelle veranschaulicht, wie die nativen Schreib- und Lesebestätigungen von MongoDB den Konsistenzebenen von Azure Cosmos DB zugeordnet werden, wenn die Azure Cosmos DB-API für MongoDB verwendet wird:

:::image type="content" source="../media/consistency-levels-across-apis/consistency-model-mapping-mongodb.png" alt-text="Zuordnung zum MongoDB-Konsistenzmodell" lightbox= "../media/consistency-levels-across-apis/consistency-model-mapping-mongodb.png":::

Wenn Ihr Azure Cosmos-Konto mit einer anderen Konsistenzebene als der starken Konsistenz konfiguriert ist, können Sie anhand der *PBS*-Metrik (Probabilistically Bounded Staleness) die Wahrscheinlichkeit ermitteln, dass Ihre Clients starke und konsistente Lesevorgänge für Ihre Workloads erzielen. Diese Metrik wird im Azure-Portal verfügbar gemacht. Weitere Informationen finden Sie unter [Überwachen der PBS-Metrik (Probabilistically Bounded Staleness)](../how-to-manage-consistency.md#monitor-probabilistically-bounded-staleness-pbs-metric).

PBS (Probabilistic Bounded Staleness) zeigt, wie letztendlich Ihre letztendliche Konsistenz ist. Diese Metrik gibt Aufschluss darüber, wie oft Sie eine stärkere Konsistenz erreichen können als die Konsistenzebene, die Sie aktuell in Ihrem Azure Cosmos-Konto konfiguriert haben. Das heißt, Sie sehen die Wahrscheinlichkeit (gemessen in Millisekunden), dass stark konsistente Lesevorgänge in Regionen mit Schreib- und Lesevorgängen auftreten.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich weiter über die globale Verteilung und Konsistenzebenen in Azure Cosmos DB:

* [Übersicht über die globale Verteilung](../distribute-data-globally.md)
* [Was sind Konsistenzebenen in Azure Cosmos DB?](../consistency-levels.md)
* [Hochverfügbarkeit](../high-availability.md)
