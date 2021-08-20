---
title: Skalare Ausdrücke in SQL-Abfragen in Azure Cosmos DB
description: Erfahren Sie etwas über die SQL-Syntax bei skalaren Ausdrücken für Azure Cosmos DB. Außerdem wird in diesem Artikel beschrieben, wie skalare Ausdrücke mithilfe von Operatoren zu komplexen Ausdrücken zusammengefasst werden.
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 05/17/2019
ms.author: mjbrown
ms.openlocfilehash: 239cc92edc64092556408b90c666dbe27d76b136
ms.sourcegitcommit: 2d412ea97cad0a2f66c434794429ea80da9d65aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2021
ms.locfileid: "122350667"
---
# <a name="scalar-expressions-in-azure-cosmos-db-sql-queries"></a>Skalare Ausdrücke in SQL-Abfragen in Azure Cosmos DB
[!INCLUDE[appliesto-sql-api](../includes/appliesto-sql-api.md)]

Die [SELECT-Klausel](sql-query-select.md) unterstützt skalare Ausdrücke. Ein skalarer Ausdruck ist eine Kombination aus Symbolen und Operatoren, die ausgewertet werden können, um einen einzelnen Wert zu erhalten. Skalare Ausdrücke können beispielsweise Konstanten, Eigenschaftenverweise, Arrayelementverweise, Aliasverweise oder Funktionsaufrufe sein. Skalare Ausdrücke können mit Operatoren in komplexen Ausdrücken kombiniert werden.

## <a name="syntax"></a>Syntax
  
```sql  
<scalar_expression> ::=  
       <constant>
     | input_alias
     | parameter_name  
     | <scalar_expression>.property_name  
     | <scalar_expression>'['"property_name"|array_index']'  
     | unary_operator <scalar_expression>  
     | <scalar_expression> binary_operator <scalar_expression>    
     | <scalar_expression> ? <scalar_expression> : <scalar_expression>  
     | <scalar_function_expression>  
     | <create_object_expression>
     | <create_array_expression>  
     | (<scalar_expression>)
  
<scalar_function_expression> ::=  
        'udf.' Udf_scalar_function([<scalar_expression>][,…n])  
        | builtin_scalar_function([<scalar_expression>][,…n])  
  
<create_object_expression> ::=  
   '{' [{property_name | "property_name"} : <scalar_expression>][,…n] '}'  
  
<create_array_expression> ::=  
   '[' [<scalar_expression>][,…n] ']'  
  
```

## <a name="arguments"></a>Argumente
  
- `<constant>`  
  
   Stellt einen konstanten Wert dar. Weitere Informationen finden Sie im Abschnitt [Konstanten](sql-query-constants.md).  
  
- `input_alias`  
  
   Stellt einen Wert dar, der von dem `input_alias` definiert wird, der in der `FROM` Klausel eingeführt wird.  
  Dieser Wert ist garantiert nicht **undefiniert** –**undefinierte** Werte in der Eingabe werden übersprungen.  
  
- `<scalar_expression>.property_name`  
  
   Stellt einen Wert der Eigenschaft eines Objekts dar. Wenn die Eigenschaft nicht vorhanden ist, oder über einen Wert, der kein Objekt ist, auf die Eigenschaft verwiesen wird, ergibt der Ausdruck den Wert **undefined** (undefiniert).  
  
- `<scalar_expression>'['"property_name"|array_index']'`  
  
   Stellt einen Wert der Eigenschaft mit dem Namen `property_name` oder ein Arrayelement mit Index `array_index` eines Arrays dar. Wenn die Eigenschaft/der Arrayindex nicht vorhanden ist, oder über einen Wert, der kein Objekt/Array ist, auf die Eigenschaft/den Arrayindex verwiesen wird, wird der Ausdruck als undefinierter Wert ausgewertet.  
  
- `unary_operator <scalar_expression>`  
  
   Stellt einen Operator dar, der auf einen einzelnen Wert angewendet wird. Details finden Sie im Abschnitt [Operatoren](sql-query-operators.md).  
  
- `<scalar_expression> binary_operator <scalar_expression>`  
  
   Stellt einen Operator dar, der auf zwei Werte angewendet wird. Details finden Sie im Abschnitt [Operatoren](sql-query-operators.md).  
  
- `<scalar_function_expression>`  
  
   Stellt einen Wert dar, der durch ein Ergebnis eines Funktionsaufrufs definiert wird.  
  
- `udf_scalar_function`  
  
   Name der benutzerdefinierten Skalarfunktion.  
  
- `builtin_scalar_function`  
  
   Name der integrierten skalaren Funktion.  
  
- `<create_object_expression>`  
  
   Stellt einen Wert dar, der durch Erstellen eines neuen Objekts mit festgelegten Eigenschaften und ihren Werten gewonnen wird.  
  
- `<create_array_expression>`  
  
   Stellt einen Wert dar, der durch Erstellen eines neuen Arrays mit festgelegten Werten als Elemente gewonnen wird.  
  
- `parameter_name`  
  
   Stellt einen Wert des angegebenen Parameternamens dar. Das erste Zeichen eines Parameternamens muss ein einzelnes „\@“ sein.  
  
## <a name="remarks"></a>Hinweise
  
  Beim Aufrufen einer integrierten oder benutzerdefinierten Skalarfunktion müssen alle Argumente definiert werden. Wenn eines der Argumente undefiniert ist, wird die Funktion nicht aufgerufen, und das Ergebnis ist undefiniert.  
  
  Beim Erstellen eines Objekts wird jede Eigenschaft, der ein undefinierter Wert zugewiesen ist, übersprungen und nicht in das erstellte Objekt einbezogen.  
  
  Beim Erstellen eines Arrays wird jeder Elementwert, dem ein **undefinierter** Wert zugewiesen ist, übersprungen und nicht in das erstellte Objekt einbezogen. Daraufhin nimmt das nächste definierte Element seinen Platz ein, sodass das erstellte Array keine übersprungenen Indizes aufweist.  

## <a name="examples"></a>Beispiele

```sql
    SELECT ((2 + 11 % 7)-2)/3
```

Die Ergebnisse lauten wie folgt:

```json
    [{
      "$1": 1.33333
    }]
```

Das Ergebnis des skalaren Ausdrucks in der folgenden Abfrage ist ein boolescher Wert:

```sql
    SELECT f.address.city = f.address.state AS AreFromSameCityState
    FROM Families f
```

Die Ergebnisse sind:

```json
    [
      {
        "AreFromSameCityState": false
      },
      {
        "AreFromSameCityState": true
      }
    ]
```

## <a name="next-steps"></a>Nächste Schritte

- [Einführung in Azure Cosmos DB](../introduction.md)
- [Azure Cosmos DB-.NET-Beispiele](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [Unterabfragen](sql-query-subquery.md)