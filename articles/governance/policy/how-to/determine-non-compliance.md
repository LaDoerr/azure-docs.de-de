---
title: Ermitteln der Ursachen für Nichtkonformität
description: Wenn eine Ressource nicht konform ist, kann das viele mögliche Ursachen haben. Erfahren Sie, wie Sie die Ursache für die Nichtkonformität ermitteln können.
ms.date: 08/17/2021
ms.topic: how-to
ms.openlocfilehash: e09bdaee974e77a3afecaaa35a37c56ed3cd56ec
ms.sourcegitcommit: 5f659d2a9abb92f178103146b38257c864bc8c31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2021
ms.locfileid: "122343663"
---
# <a name="determine-causes-of-non-compliance"></a>Ermitteln der Ursachen für Nichtkonformität

Wenn festgestellt wird, dass eine Azure-Ressource nicht konform mit einer Richtlinienregel ist, ist es wichtig zu bestimmen, gegen welchen Teil der Regel die Ressource verstößt. Außerdem ist es ebenso nützlich, ermitteln zu können, welche Änderung dazu geführt hat, dass eine ehemals konforme Ressource nun nicht mehr konform ist. Es gibt zwei Möglichkeiten, diese Informationen zu bestimmen:

- [Konformitätsdetails](#compliance-details)
- [Änderungsverlauf (Vorschau)](#change-history)

## <a name="compliance-details"></a>Konformitätsdetails

Wenn eine Ressource nicht konform ist, können Sie über die Seite **Richtlinienkonformität** auf die Konformitätsdetails dieser Ressource zugreifen. Der Bereich mit den Kompatibilitätsdetails enthält die folgenden Informationen:

- Angaben zur Ressource wie der Name, der Typ, der Speicherort und die Ressourcen-ID
- Konformitätsstatus und Zeitstempel der letzten Evaluierung für die aktuelle Richtlinienzuweisung
- Eine Liste mit _Gründen_ für die Nichtkonformität der Ressource

> [!IMPORTANT]
> Die Konformitätsdetails einer _nicht konformen_ Ressource enthalten die aktuellen Werte der Eigenschaften der Ressource. Deshalb muss der **Benutzer über Leseberechtigungen** für den **Ressourcentyp** verfügen. Wenn die _nicht konforme_ Ressource z.B. vom Typ **Microsoft.Compute/virtualMachines** ist, muss der Leser über die Berechtigung **Microsoft.Compute/virtualMachines/read** verfügen. Wenn der Benutzer nicht über die erforderliche Berechtigung verfügt, wird ein Zugriffsfehler angezeigt.

Führen Sie die folgenden Schritte durch, um die Konformitätsdetails anzuzeigen:

1. Starten Sie den Azure Policy-Dienst über das Azure-Portal, indem Sie die Option **Alle Dienste** auswählen und dann nach **Policy** suchen und die entsprechende Option auswählen.

1. Wählen Sie auf der Seite **Übersicht** oder **Konformität** eine Richtlinie mit dem **Konformitätsstatus**_Nicht konform_ aus.

1. Öffnen Sie auf der Seite **Richtlinienkompatibilität** auf der Registerkarte **Ressourcenkonformität** das Kontextmenü, oder wählen Sie die Auslassungspunkte neben einer Ressource mit dem **Konformitätszustand** _Nicht konform_ aus. Wählen Sie dann **Konformitätsdetails anzeigen** aus.

   :::image type="content" source="../media/determine-non-compliance/view-compliance-details.png" alt-text="Screenshot: Link „Konformitätsdetails anzeigen“ auf der Registerkarte „Ressourcenkonformität“" border="false":::

1. Im Bereich **Konformitätsdetails** werden Informationen der letzten Auswertung der Ressource mit der aktuellen Richtlinienzuweisung angezeigt. In diesem Beispiel enthält das Feld **Microsoft.Sql/servers/version**_12.0_, und die Richtliniendefinition hat _14.0_ erwartet. Wenn es mehrere Gründen für die Nichtkonformität der Ressource gibt, werde diese alle in diesem Bereich aufgeführt.

   :::image type="content" source="../media/determine-non-compliance/compliance-details-pane.png" alt-text="Screenshot: Bereich „Konformitätsdetails“ und Gründe für Nichtkonformität (aktueller Wert ist „12“ und Zielwert ist „14“)" border="false":::

   Für **auditIfNotExists**- oder **deployIfNotExists**-Richtliniendefinitionen enthalten die Details die Eigenschaft **details.type** und alle optionalen Eigenschaften. Eine Liste finden Sie unter [„Grundlegendes zu Azure Policy-Auswirkungen“ im Abschnitt „Eigenschaften von „auditIfNotExists“](../concepts/effects.md#auditifnotexists-properties) und unter [„Grundlegendes zu Azure Policy-Auswirkungen“ im Abschnitt „Eigenschaften von „DeployIfNotExists“](../concepts/effects.md#deployifnotexists-properties). **Zuletzt ausgewertete Ressource** ist eine verwandte Ressource aus dem **Detailabschnitt** der Definition.

   Hier sehen Sie einen Beispielausschnitt aus der Definition von **deployIfNotExists**:

   ```json
   {
       "if": {
           "field": "type",
           "equals": "[parameters('resourceType')]"
       },
       "then": {
           "effect": "DeployIfNotExists",
           "details": {
               "type": "Microsoft.Insights/metricAlerts",
               "existenceCondition": {
                   "field": "name",
                   "equals": "[concat(parameters('alertNamePrefix'), '-', resourcegroup().name, '-', field('name'))]"
               },
               "existenceScope": "subscription",
               "deployment": {
                   ...
               }
           }
       }
   }
   ```

   :::image type="content" source="../media/determine-non-compliance/compliance-details-pane-existence.png" alt-text="Screenshot: Bereich „Konformitätsdetails“ für „ifNotExists“, einschließlich Anzahl ausgewerteter Ressourcen" border="false":::

> [!NOTE]
> Der aktuelle Wert zeigt Asteriske an, wenn ein Eigenschaftswert ein _Geheimnis_ ist, um Daten zu schützen.

In den Details wird angegeben, warum eine Ressource nicht konform ist. Allerdings wird nicht angegeben, wann eine Änderung an der Ressource vorgenommen wurde, die dazu geführt hat, dass sie nicht mehr konform ist. Zu diesen Informationen erfahren Sie im Abschnitt [Änderungsverlauf (Vorschau)](#change-history) mehr.

### <a name="compliance-reasons"></a>Konformitätsgründe

In der folgenden Tabelle wird jeder mögliche _Grund_ der entsprechenden [Bedingung](../concepts/definition-structure.md#conditions) in der Richtliniendefinition zugeordnet:

|`Reason` | Bedingung |
|-|-|
|Der aktuelle Wert muss den Zielwert als Schlüssel enthalten. |containsKey oder **nicht** notContainsKey |
|Der aktuelle Wert muss den Zielwert enthalten. |contains oder **nicht** notContains |
|Der aktuelle Wert muss gleich dem Zielwert sein. |equals der **nicht** notEquals |
|Der aktuelle Wert muss kleiner als der Zielwert sein. |less oder **nicht** greaterOrEquals |
|Der aktuelle Wert muss größer oder gleich dem Zielwert sein. |greaterOrEquals oder **nicht** less |
|Der aktuelle Wert muss größer als der Zielwert sein. |greater oder **nicht** lessOrEquals |
|Der aktuelle Wert muss kleiner oder gleich dem Zielwert sein. |lessOrEquals oder **nicht** greater |
|Der aktuelle Wert muss vorhanden sein. |exists |
|Der aktuelle Wert muss im Zielwert enthalten sein. |in der **nicht** notIn |
|Der aktuelle Wert muss dem Zielwert entsprechen. |like oder **nicht** notLike |
|Der aktuelle Wert muss mit dem Zielwert übereinstimmen (Berücksichtigung der Groß-/Kleinschreibung). |match oder **nicht** notMatch |
|Der aktuelle Wert muss mit dem Zielwert übereinstimmen (keine Berücksichtigung der Groß-/Kleinschreibung). |matchInsensitively oder **nicht** notMatchInsensitively |
|Der aktuelle Wert darf den Zielwert nicht als Schlüssel enthalten. |notContainsKeyontainsKey oder **nicht** ContainsKey|
|Der aktuelle Wert darf nicht den Zielwert enthalten. |notContains oder **nicht** contains |
|Der aktuelle Wert darf nicht gleich dem Zielwert sein. |notEquals der **nicht** equals |
|Der aktuelle Wert darf nicht vorhanden sein. |**nicht** exists  |
|Der aktuelle Wert darf nicht im Zielwert enthalten sein. |notIn oder **nicht** in |
|Der aktuelle Wert darf nicht dem Zielwert entsprechen. |notLike oder **nicht** like |
|Der aktuelle Wert darf nicht mit dem Zielwert übereinstimmen (Berücksichtigung der Groß-/Kleinschreibung). |notMatch oder **nicht** match |
|Der aktuelle Wert darf nicht mit dem Zielwert übereinstimmen (keine Berücksichtigung der Groß-/Kleinschreibung). |notMatchInsensitively oder **nicht** matchInsensitively |
|Keine der zugehörigen Ressourcen entspricht den Effektdetails in der Richtliniendefinition. |Eine Ressource des in **then.details.type** definierten Typs, die mit der im **if**-Abschnitt der Richtlinienregel definierten Ressource verwandt ist, ist nicht vorhanden. |

## <a name="component-details-for-resource-provider-modes"></a>Komponentendetails für Ressourcenanbietermodi

Bei Zuweisungen mit einem [Ressourcenanbietermodus](../concepts/definition-structure.md#resource-manager-modes) wählen Sie die _nicht konforme_ Ressource aus, um eine detailliertere Ansicht zu öffnen. Unter der Registerkarte **Komponentenkonformität** befinden sich zusätzliche Informationen, die spezifisch für den Modus „Ressourcenanbieter“ der zugewiesenen Richtlinie sind und die _nicht konforme_ **Komponente** und die **Komponenten-ID** anzeigen.

:::image type="content" source="../media/getting-compliance-data/compliance-components.png" alt-text="Screenshot der Registerkarte „Komponentenkonformität“ und Konformitätsdetails für eine Zuweisung im Ressourcenanbietermodus" border="false":::

## <a name="compliance-details-for-guest-configuration"></a>Details zur Konformität für die Gastkonfiguration

Für Überwachungsrichtlinien (_auditIfNotExists_) in der Kategorie _Gastkonfiguration_ können auf dem virtuellen Computer mehrere Einstellungen ausgewertet werden, und Sie müssen die Details pro Einstellung anzeigen. Wenn Sie beispielsweise eine Überprüfung der Liste mit Kennwortrichtlinien durchführen und nur ein Eintrag den Status _Nicht konform_ aufweist, müssen Sie ermitteln, welche spezifischen Kennwortrichtlinien nicht konform sind und was der Grund dafür ist.

Unter Umständen haben Sie auch keinen Zugriff für die direkte Anmeldung auf dem virtuellen Computer, müssen aber melden, warum der virtuelle Computer _nicht konform_ ist.

### <a name="azure-portal"></a>Azure-Portal

Beginnen Sie, indem Sie die Schritte zum Anzeigen der Konformitätsdetails der Richtlinien im obigen Abschnitt ausführen.

Wählen Sie im Bereich „Konformitätsdetails“ den Link **Zuletzt ausgewertete Ressource** aus.

:::image type="content" source="../media/determine-non-compliance/guestconfig-auditifnotexists-compliance.png" alt-text="Screenshot: Anzeigen der Konformitätsdetails zur Definition von „auditIfNotExists“" border="false":::

Auf der Seite **Gastzuweisung** werden alle verfügbaren Konformitätsdetails angezeigt. Jede Zeile der Ansicht steht für eine Auswertung, die auf dem Computer durchgeführt wurde. In der Spalte **Grund** wird eine Beschreibung angezeigt, mit der der Grund für den Status _Nicht konform_ für die Gastzuweisung angegeben wird. Wenn Sie beispielsweise Kennwortrichtlinien überprüfen, wird in der Spalte **Grund** Text mit dem aktuellen Wert für jede Einstellung angezeigt.

:::image type="content" source="../media/determine-non-compliance/guestconfig-compliance-details.png" alt-text="Screenshot: Konformitätsdetails zur Gastzuweisung" border="false":::

## <a name="change-history-preview"></a><a name="change-history"></a>Änderungsverlauf (Vorschau)

Im Rahmen einer neuen **öffentlichen Vorschau** sind die letzten 14 Tage des Änderungsverlaufs für alle Azure-Ressourcen verfügbar, die die [Löschung im vollständigen Modus](../../../azure-resource-manager/templates/deployment-complete-mode-deletion.md) unterstützen. Der Änderungsverlauf enthält Details dazu, wann eine Änderung erkannt wurde, und eine _Visual Diff_ jeder Änderung. Eine Änderungserkennung wird ausgelöst, wenn Azure Resource Manager-Eigenschaften hinzugefügt, entfernt oder geändert werden.

1. Starten Sie den Azure Policy-Dienst über das Azure-Portal, indem Sie die Option **Alle Dienste** auswählen und dann nach **Policy** suchen und die entsprechende Option auswählen.

1. Wählen Sie auf der Seite **Übersicht** oder **Konformität** eine Richtlinie mit einem beliebigen **Konformitätsstatus** aus.

1. Wählen Sie auf der Seite **Richtlinienkonformität** auf der Registerkarte **Ressourcenkonformität** eine Ressource aus.

1. Wählen Sie auf der Seite **Ressourcenkonformität** die Registerkarte **Änderungsverlauf (Vorschau)** aus. Eine Liste der erkannten Änderungen, falls vorhanden, wird angezeigt.

   :::image type="content" source="../media/determine-non-compliance/change-history-tab.png" alt-text="Screenshot: Registerkarte „Änderungsverlauf“ und Zeiten der erkannten Änderungen auf der Seite „Ressourcenkonformität“" border="false":::

1. Wählen Sie eine der erkannten Änderungen aus. Die _Visual Diff_ für die Ressource wird auf der Seite **Änderungsverlauf** angezeigt.

   :::image type="content" source="../media/determine-non-compliance/change-history-visual-diff.png" alt-text="Screenshot: Visual Diff des Änderungsverlaufs für den Vorher/Nachher-Status von Eigenschaften auf der Seite „Änderungsverlauf“" border="false":::

Die _Visual Diff_ hilft, Änderungen an einer Ressource zu identifizieren. Die erkannten Änderungen müssen nicht zwangsläufig mit dem geänderten Konformitätsstatus der Ressource zusammenhängen.

Die Verlaufsdaten zu Änderungen werden von [Azure Resource Graph](../../resource-graph/overview.md) bereitgestellt. Wie Sie diese Informationen außerhalb des Azure-Portals abfragen, erfahren Sie unter [Get resource changes (Abrufen von Ressourcenänderungen)](../../resource-graph/how-to/get-resource-changes.md).

## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich die Beispiele unter [Azure Policy-Beispiele](../samples/index.md) an.
- Lesen Sie die Informationen unter [Struktur von Azure Policy-Definitionen](../concepts/definition-structure.md).
- Lesen Sie [Grundlegendes zu Richtlinienauswirkungen](../concepts/effects.md).
- Informieren Sie sich über das [programmgesteuerte Erstellen von Richtlinien](programmatically-create.md).
- Informieren Sie sich über das [Abrufen von Konformitätsdaten](get-compliance-data.md).
- Erfahren Sie, wie Sie [nicht konforme Ressourcen korrigieren](remediate-resources.md) können.
- Weitere Informationen zu Verwaltungsgruppen finden Sie unter [Organisieren Ihrer Ressourcen mit Azure-Verwaltungsgruppen](../../management-groups/overview.md).