---
title: Bicep-Funktionen - any
description: Beschreibt die any-Funktion, die in Bicep zum Konvertieren von Typen verfügbar ist.
ms.topic: conceptual
ms.date: 06/01/2021
ms.openlocfilehash: c4bf30d88f9cad54e83ad1d813242dc09e6c01a5
ms.sourcegitcommit: 7f59e3b79a12395d37d569c250285a15df7a1077
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/02/2021
ms.locfileid: "111026377"
---
# <a name="any-function-for-bicep"></a>any-Funktion für Bicep

Bicep unterstützt eine Funktion namens `any()` zum Auflösen von Typfehlern im Bicep-Typensystem. Diese Funktion wird verwendet, wenn das Format des von Ihnen bereitgestellten Werts nicht mit dem vom Typensystem erwarteten Wert übereinstimmt. Wenn die-Eigenschaft z. B. eine Zahl erfordert, Sie aber eine Zeichenfolge angeben müssen, wie `'0.5'`. Verwenden Sie dann die `any()`-Funktion, um den vom Typensystem gemeldeten Fehler zu unterdrücken.

Diese Funktion ist in der Azure Resource Manager-Vorlagenlaufzeit nicht vorhanden. Sie wird nur von Bicep verwendet und nicht im JSON-Code für die erstellte Vorlage ausgegeben.

## <a name="any"></a>any

`any(value)`

Gibt einen Wert zurück, der mit einem beliebigen Datentyp kompatibel ist.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | BESCHREIBUNG |
|:--- |:--- |:--- |:--- |
| value | Ja | Alle Typen | Der Wert, der in einen kompatiblen Typ konvertiert werden soll. |

### <a name="return-value"></a>Rückgabewert

Der Wert in einer Form, die mit einem beliebigen Datentyp kompatibel ist.

### <a name="examples"></a>Beispiele

Das folgende Beispiel zeigt, wie die `any()`-Funktion verwendet wird, um numerische Werte als Zeichenfolgen bereitzustellen.

```bicep
resource wpAci 'microsoft.containerInstance/containerGroups@2019-12-01' = {
  name: 'wordpress-containerinstance'
  location: location
  properties: {
    containers: [
      {
        name: 'wordpress'
        properties: {
          ...
          resources: {
            requests: {
              cpu: any('0.5')
              memoryInGB: any('0.7')
            }
          }
        }
      }
    ]
  }
}
```

Die Funktion arbeitet mit jedem zugewiesenen Wert in Bicep. Im folgenden Beispiel wird `any()` mit einem ternären Ausdruck als Argument verwendet.

```bicep
publicIPAddress: any((pipId == '') ? null : {
  id: pipId
})
```

## <a name="next-steps"></a>Nächste Schritte

Komplexere Verwendungsmöglichkeiten der `any()`Funktion finden Sie in den folgenden Beispielen:

* [Untergeordnete Ressourcen, die einen bestimmten Namen erfordern](https://github.com/Azure/bicep/blob/main/docs/examples/201/api-management-create-all-resources/main.bicep#L246)
* [Eine Ressourceneigenschaft, die nicht im Typ der Ressource definiert ist, obgleich sie vorhanden ist](https://github.com/Azure/bicep/blob/main/docs/examples/201/log-analytics-with-solutions-and-diagnostics/main.bicep#L26)

