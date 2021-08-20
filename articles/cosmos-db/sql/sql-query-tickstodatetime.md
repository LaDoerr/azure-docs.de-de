---
title: TicksToDateTime in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion TicksToDateTime in Azure Cosmos DB.
author: timsander1
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 08/18/2020
ms.author: tisande
ms.custom: query-reference
ms.openlocfilehash: 8fe361fd43c9f6f4ae26c1aaa9149324e4384df4
ms.sourcegitcommit: 2d412ea97cad0a2f66c434794429ea80da9d65aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2021
ms.locfileid: "122350480"
---
# <a name="tickstodatetime-azure-cosmos-db"></a>TicksToDateTime (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](../includes/appliesto-sql-api.md)]

Konvertiert den angegebenen Taktwert in einen DateTime-Wert.
  
## <a name="syntax"></a>Syntax
  
```sql
TicksToDateTime (<Ticks>)
```

## <a name="arguments"></a>Argumente

*Ticks*  

Ein numerischer Wert mit Vorzeichen: die aktuelle Anzahl der 100-Nanosekunden-Takte, die seit der Unix-Epoche verstrichen sind. Anders gesagt: Es handelt sich um die Anzahl der 100-Nanosekunden-Takte, die seit Donnerstag, 1. Januar 1970, 00:00:00 verstrichen sind.

## <a name="return-types"></a>Rückgabetypen

Gibt einen Zeichenfolgenwert für das UTC-Datum und die UTC-Uhrzeit gemäß ISO 8601 im Format `YYYY-MM-DDThh:mm:ss.fffffffZ` zurück. Hierbei gilt:
  
|Format|BESCHREIBUNG|
|-|-|
|YYYY|vierstellige Jahreszahl|
|MM|zweistellige Monatszahl (01 = Januar usw.)|
|DD|zweistellige Zahl für den Tag des Monats (01 bis 31)|
|T|Trennzeichen, das den Anfang der Uhrzeitelemente markiert|
|hh|zweistellige Stundenzahl (00 bis 23)|
|MM|zweistellige Minutenzahl (00 bis 59)|
|ss|zweistellige Sekundenzahl (00 bis 59)|
|.fffffff|siebenstellige Sekundenbruchteile|
|Z|UTC-Kennzeichner|
  
  Weitere Informationen zum ISO 8601-Format finden Sie unter [ISO_8601](https://en.wikipedia.org/wiki/ISO_8601).

## <a name="remarks"></a>Bemerkungen

TicksToDateTime gibt `undefined` zurück, wenn der angegebene Taktwert ungültig ist.

## <a name="examples"></a>Beispiele
  
Das folgende Beispiel konvertiert die Takte in einen DateTime-Wert:

```sql
SELECT TicksToDateTime(15943368134575530) AS DateTime
```

```json
[
    {
        "DateTime": "2020-07-09T23:20:13.4575530Z"
    }
]
```  

## <a name="next-steps"></a>Nächste Schritte

- [Datums- und Uhrzeitfunktionen in Azure Cosmos DB](sql-query-date-time-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](../introduction.md)
