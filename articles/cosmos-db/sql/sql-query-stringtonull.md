---
title: StringToNull in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion StringToNull in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 754921e570326a329aea60110a11ae8998b55772
ms.sourcegitcommit: 2d412ea97cad0a2f66c434794429ea80da9d65aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2021
ms.locfileid: "122350688"
---
# <a name="stringtonull-azure-cosmos-db"></a>StringToNull (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](../includes/appliesto-sql-api.md)]

 Gibt den Ausdruck übersetzt in NULL zurück. Wenn der Ausdruck nicht übersetzt werden kann, wird „undefined“ zurückgegeben.  
  
## <a name="syntax"></a>Syntax
  
```sql
StringToNull(<str_expr>)  
```  
  
## <a name="arguments"></a>Argumente
  
*str_expr*  
   Ist ein Zeichenfolgenausdruck, der als NULL-Ausdruck analysiert werden soll.
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen NULL-Ausdruck oder „undefined“ zurück.  
  
## <a name="examples"></a>Beispiele
  
  Im folgenden Beispiel wird das typübergreifende Verhalten von `StringToNull` veranschaulicht. 

Die folgenden Beispiele zeigen eine gültige Eingabe.

 Leerzeichen sind ausschließlich vor oder nach „null“ zulässig.

```sql
SELECT 
    StringToNull("null") AS n1, 
    StringToNull("  null ") AS n2,
    IS_NULL(StringToNull("null   ")) AS n3
```  
  
 Hier ist das Resultset.  
  
```json
[{"n1": null, "n2": null, "n3": true}]
```  

Die folgenden Beispiele zeigen eine ungültige Eingabe.

„null“ darf nur Kleinbuchstaben enthalten.

```sql
SELECT    
    StringToNull("NULL"),
    StringToNull("Null")
```  
  
 Hier ist das Resultset.  
  
```json
[{}]
```  

Der übergebene Ausdruck wird als NULL-Ausdruck analysiert. Die folgenden Eingaben werden nicht als NULL-Typ ausgewertet und geben daher „undefined“ zurück:

```sql
SELECT    
    StringToNull("true"), 
    StringToNull(false), 
    StringToNull(undefined),
    StringToNull(NaN) 
```  
  
 Hier ist das Resultset.  
  
```json
[{}]
```  

## <a name="remarks"></a>Bemerkungen

Der Index wird von dieser Systemfunktion nicht verwendet.

## <a name="next-steps"></a>Nächste Schritte

- [Zeichenfolgenfunktionen in Azure Cosmos DB](sql-query-string-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](../introduction.md)
