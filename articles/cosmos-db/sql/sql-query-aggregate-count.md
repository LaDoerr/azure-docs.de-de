---
title: „COUNT“ in der Abfragesprache für Azure Cosmos DB
description: Hier finden Sie Informationen zur SQL-Systemfunktion „Anzahl“ (COUNT) in Azure Cosmos DB.
author: timsander1
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 12/02/2020
ms.author: tisande
ms.custom: query-reference
ms.openlocfilehash: edc3c824bc9ac08e325259c0b5f736867f877062
ms.sourcegitcommit: 2d412ea97cad0a2f66c434794429ea80da9d65aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2021
ms.locfileid: "122342779"
---
# <a name="count-azure-cosmos-db"></a>COUNT (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](../includes/appliesto-sql-api.md)]

Diese Systemfunktion gibt die Anzahl der Werte im Ausdruck zurück.
  
## <a name="syntax"></a>Syntax
  
```sql
COUNT(<scalar_expr>)  
```  
  
## <a name="arguments"></a>Argumente
  
*scalar_expr*  
   Ist ein beliebiger skalarer Ausdruck.
  
## <a name="return-types"></a>Rückgabetypen
  
Gibt einen numerischen Ausdruck zurück.  
  
## <a name="examples"></a>Beispiele
  
Im folgenden Beispiel wird die Gesamtanzahl von Elementen in einem Container zurückgegeben:
  
```sql
SELECT COUNT(1)
FROM c
``` 
Von „COUNT“ wird ein beliebiger skalarer Ausdruck als Eingabe akzeptiert. Die folgende Abfrage liefert ein entsprechendes Ergebnis:

```sql
SELECT COUNT(2)
FROM c
```

## <a name="remarks"></a>Bemerkungen

Diese Systemfunktion profitiert von einem [Bereichsindex](../index-policy.md#includeexclude-strategy) für jegliche Eigenschaften im Filter der Abfrage.

## <a name="next-steps"></a>Nächste Schritte

- [Mathematische Funktionen in Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Aggregatfunktionen in Azure Cosmos DB](sql-query-aggregate-functions.md)