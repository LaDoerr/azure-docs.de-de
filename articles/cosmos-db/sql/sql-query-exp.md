---
title: EXP in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion Exponent (EXP) in Azure Cosmos DB, die den exponentiellen Wert des angegebenen numerischen Ausdrucks zurückgibt.
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 18be2065ef2442f8370b780b81c769b2765e5ddb
ms.sourcegitcommit: 2d412ea97cad0a2f66c434794429ea80da9d65aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2021
ms.locfileid: "122350756"
---
# <a name="exp-azure-cosmos-db"></a>EXP (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](../includes/appliesto-sql-api.md)]

 Gibt den Exponentialwert des angegebenen numerischen Ausdrucks zurück.  
  
## <a name="syntax"></a>Syntax
  
```sql
EXP (<numeric_expr>)  
```  
  
## <a name="arguments"></a>Argumente
  
*numeric_expr*  
   Ist ein numerischer Ausdruck.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen numerischen Ausdruck zurück.  
  
## <a name="remarks"></a>Bemerkungen
  
  Die Konstante **e** (2,718281...) ist die Basis des natürlichen Logarithmus.  
  
  Der Exponent einer Zahl ist die Konstante **e** potenziert mit der Zahl. Beispiel: EXP(1,0) = e^1,0 = 2,71828182845905 und EXP(10) = e^10 = 22026,4657948067.  
  
  Der Exponentialwert des natürlichen Logarithmus einer Zahl ist die Zahl selbst: EXP (LOG (n)) = n. Und der natürliche Logarithmus des Exponentialwerts einer Zahl ist die Zahl selbst: LOG (EXP (n)) = n.  
  
## <a name="examples"></a>Beispiele
  
  Im folgenden Beispiel wird eine Variable deklariert und der Exponentialwert der angegebenen Variablen zurückgegeben (10).  
  
```sql
SELECT EXP(10) AS exp  
```  
  
 Hier ist das Resultset.  
  
```json
[{exp: 22026.465794806718}]  
```  
  
 Das folgende Beispiel gibt den Exponentialwert des natürlichen Logarithmus von 20 und den natürlichen Logarithmus des Exponentialwerts von 20 zurück. Da diese Funktionen Umkehrfunktionen der jeweils anderen sind, beträgt der Rückgabewert mit Rundung für die Gleitkommaberechnung in beiden Fällen 20.  
  
```sql
SELECT EXP(LOG(20)) AS exp1, LOG(EXP(20)) AS exp2  
```  
  
 Hier ist das Resultset.  
  
```json
[{exp1: 19.999999999999996, exp2: 20}]  
```  

## <a name="next-steps"></a>Nächste Schritte

- [Mathematische Funktionen in Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](../introduction.md)
