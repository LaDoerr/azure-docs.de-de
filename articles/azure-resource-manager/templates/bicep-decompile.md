---
title: Konvertieren von Vorlagen zwischen JSON und Bicep
description: Hier sind die Befehle zum Konvertieren von Azure Resource Manager-Vorlagen aus Bicep in JSON und aus JSON in Bicep beschrieben.
ms.topic: conceptual
ms.date: 05/17/2021
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: ed273e9cf9af6f29ecc495536fd5d504d2b168f2
ms.sourcegitcommit: 5c136a01bddfccb2cc9f7e7e7741e2cf2651ddbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/03/2021
ms.locfileid: "111353420"
---
# <a name="converting-arm-templates-between-json-and-bicep"></a>Konvertieren von ARM-Vorlagen zwischen JSON und Bicep

In diesem Artikel wird beschrieben, wie Sie ARM-Vorlagen (Azure Resource Manager) aus JSON in Bicep und aus Bicep in JSON konvertieren.

Zum Ausführen der Konvertierungsbefehle muss die [Bicep-CLI installiert](bicep-install.md) sein.

Mit den Konvertierungsbefehlen werden funktional gleichwertige Vorlagen erstellt. Allerdings ist die Implementierung möglicherweise nicht identisch. Wenn eine Vorlage aus JSON in Bicep und zurück in JSON konvertiert wird, weist die resultierende Vorlage wahrscheinlich eine andere Syntax als die ursprüngliche Vorlage auf. Nach der Bereitstellung führen die konvertierten Vorlagen zu denselben Ergebnissen.

## <a name="convert-from-json-to-bicep"></a>Konvertieren aus JSON in Bicep

Die Bicep-CLI bietet einen Befehl zum Dekompilieren beliebiger vorhandener ARM-Vorlagen in eine Bicep-Datei.

Wenn Sie Version 2.20.0 oder höher der Azure CLI verwenden, wird die Bicep CLI automatisch installiert. Verwenden Sie zum Dekompilieren einer JSON-Datei .

```azurecli
az bicep decompile mainTemplate.json
```

Wenn Sie nicht über eine aktuelle Version der Azure CLI verfügen und stattdessen die Bicep CLI manuell installiert haben, verwenden Sie Folgendes:

```bash
bicep decompile mainTemplate.json
```

Dieser Befehl bietet einen Ausgangspunkt für die Entwicklung mit Bicep. Der Befehl funktioniert nicht mit allen Vorlagen. Geschachtelte Vorlagen können derzeit nur dekompiliert werden, wenn sie den Auswertungsbereich „inner“ für Ausdrücke verwenden. Vorlagen mit Kopierschleifen können nicht dekompiliert werden.

## <a name="convert-from-bicep-to-json"></a>Konvertieren aus Bicep in JSON

Die Bicep CLI bietet auch einen Befehl zum Konvertieren von Bicep in JSON. 

Verwenden Sie für Version 2.20.0 oder höher der Azure CLI den folgenden Befehl, um eine JSON-Datei zu erstellen:

```azurecli
az bicep build mainTemplate.bicep
```

Wenn Sie die Bicep CLI manuell installiert haben, verwenden Sie Folgendes:

```bash
bicep build mainTemplate.bicep
```

## <a name="export-template-and-convert"></a>Exportieren der Vorlage und Konvertierung

Sie können die Vorlage für eine Ressourcengruppe exportieren und dann direkt an den Dekompilierungsbefehl übergeben. Im folgenden Beispiel wird gezeigt, wie Sie eine exportierte Vorlage dekompilieren.

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli
az group export --name "your_resource_group_name" > main.json
az bicep decompile main.json
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
Export-AzResourceGroup -ResourceGroupName "your_resource_group_name" -Path ./main.json
bicep decompile main.json
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

[Exportieren Sie die Vorlage](export-template-portal.md) über das Portal.

Verwenden Sie `bicep decompile <filename>` mit der heruntergeladenen Datei.

---

## <a name="side-by-side-view"></a>Parallele Ansicht

Mit einem [Bicep Playground](https://aka.ms/bicepdemo) können Sie sich entsprechende JSON- und Bicep-Dateien nebeneinander anzeigen. Sie können eine Beispielvorlage auswählen, um beide Versionen anzuzeigen. Oder Sie wählen `Decompile` aus, um Ihre eigene JSON-Vorlage hochzuladen und die entsprechende Bicep-Datei anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Bicep finden Sie im [Tutorial zu Bicep](./bicep-tutorial-create-first-bicep.md).
