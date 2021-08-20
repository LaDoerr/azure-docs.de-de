---
title: Verschieben von Ressourcen in ein neues Abonnement oder eine neue Ressourcengruppe
description: Verwenden Sie Azure Resource Manager, um Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement verschieben.
ms.topic: conceptual
ms.date: 06/03/2021
ms.custom: devx-track-azurecli, devx-track-azurepowershell
ms.openlocfilehash: ec23b4306f088328bfb72f3cf9071a70f8eb2307
ms.sourcegitcommit: b5508e1b38758472cecdd876a2118aedf8089fec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2021
ms.locfileid: "113586777"
---
# <a name="move-resources-to-a-new-resource-group-or-subscription"></a>Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement

In diesem Artikel wird erklärt, wie Sie Azure-Ressourcen entweder in ein anderes Azure-Abonnement oder in eine andere Ressourcengruppe im gleichen Abonnement verschieben. Sie können das Azure-Portal, Azure PowerShell, die Azure-Befehlszeilenschnittstelle oder die REST-API verwenden, um Ressourcen zu verschieben.

Beim Verschieben wird sowohl die Quell- als auch die Zielgruppe gesperrt. Schreib- und Löschvorgänge in den Ressourcengruppen werden bis zum Abschluss der Verschiebung blockiert. Diese Sperre bedeutet, dass Sie in Ressourcengruppen keinen Ressourcen hinzufügen, aktualisieren oder löschen können. Es heißt aber nicht, dass die Ressourcen eingefroren sind. Wenn Sie beispielsweise eine logische Azure SQL Server-Instanz und ihre Datenbanken in eine neue Ressourcengruppe oder ein neues Abonnement verschieben, treten für die Anwendungen, die diese Datenbanken verwenden, keine Ausfallzeiten auf. Sie besitzen weiterhin Lese- und Schreibzugriff auf diese Datenbanken. Die Sperre kann maximal vier Stunden dauern, aber die meisten Verschiebungen werden in viel kürzerer Zeit beendet.

Wenn Sie ein Ressource verschieben, wird sie nur in eine neue Ressourcengruppe oder ein neues Abonnement verschoben. Der Speicherort der Ressource ändert sich nicht.

## <a name="changed-resource-id"></a>Geänderte Ressourcen-ID

Wenn Sie eine Ressource verschieben, ändern Sie ihre Ressourcen-ID. Das Standardformat für eine Ressourcen-ID ist `/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}`. Wenn Sie eine Ressource in eine neue Ressourcengruppe oder ein Abonnement verschieben, ändern Sie einen oder mehrere Werte in dem Pfad.

Wenn Sie die Ressourcen-ID irgendwo verwenden, müssen Sie diesen Wert ändern. Wenn Sie z. B. ein [benutzerdefiniertes Bedienfeld](../../azure-portal/quickstart-portal-dashboard-azure-cli.md) im Portal haben, das auf eine Ressourcen-ID verweist, müssen Sie diesen Wert aktualisieren. Suchen Sie nach allen Skripten oder Vorlagen, die für die neue Ressourcen-ID aktualisiert werden müssen.

## <a name="checklist-before-moving-resources"></a>Checkliste vor dem Verschieben von Ressourcen

Vor dem Verschieben einer Ressource müssen einige wichtige Schritte ausgeführt werden. Indem Sie diese Bedingungen überprüfen, können Sie Fehler vermeiden.

1. Die zu verschiebenden Ressourcen müssen den Verschiebevorgang unterstützen. Eine Liste der Ressourcen, die das Verschieben unterstützen, finden Sie unter [Unterstützung des Verschiebevorgangs für Ressourcen](move-support-resources.md).

1. Für einige Dienste gelten bestimmte Einschränkungen oder Anforderungen beim Verschieben von Ressourcen. Wenn Sie einen der folgenden Dienste verschieben möchten, überprüfen Sie diese Anleitung vor dem Verschieben.

   * Wenn Sie Azure Stack Hub verwenden, können Sie keine Ressourcen zwischen Gruppen verschieben.
   * [Anleitung zum Verschieben von App Services](./move-limitations/app-service-move-limitations.md)
   * [Anleitung zum Verschieben von Azure DevOps Services](/azure/devops/organizations/billing/change-azure-subscription?toc=/azure/azure-resource-manager/toc.json)
   * [Anleitung zum Verschieben des klassischen Bereitstellungsmodells](./move-limitations/classic-model-move-limitations.md): Compute (klassisch), Storage (klassisch), Virtual Networks (klassisch), Cloud Services (klassisch)
   * [Move guidance for networking resources](./move-limitations/networking-move-limitations.md) (Anleitung zum Verschieben von Netzwerkressourcen)
   * [Anleitung zum Verschieben von Recovery Services](../../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json)
   * [Anleitung zum Verschieben von Virtual Machines](./move-limitations/virtual-machines-move-limitations.md)
   * Informationen zum Verschieben eines Azure-Abonnements in eine neue Verwaltungsgruppe finden Sie unter [Verschieben von Abonnements](../../governance/management-groups/manage.md#move-subscriptions).

1. Wenn Sie eine Ressource verschieben, der eine Azure-Rolle direkt zugewiesen ist (oder eine untergeordnete Ressource), wird die Rollenzuweisung nicht verschoben und verwaist. Nach der Verschiebung müssen Sie die Rollenzuweisung erneut erstellen. Letztendlich wird die verwaiste Rollenzuweisung automatisch entfernt, wir empfehlen jedoch, die Rollenzuweisung vor dem Verschieben zu entfernen.

    Informationen zum Verwalten von Rollenzuweisungen finden Sie unter [Auflisten von Azure-Rollenzuweisungen](../../role-based-access-control/role-assignments-list-portal.md#list-role-assignments-at-a-scope) und [Zuweisen von Azure-Rollen über das Azure-Portal](../../role-based-access-control/role-assignments-portal.md).

1. Quellen- und Zielabonnements müssen aktiv sein. Wenn beim Aktivieren eines Kontos, das deaktiviert wurde, Schwierigkeiten auftreten, [erstellen Sie eine Azure-Supportanfrage](../../azure-portal/supportability/how-to-create-azure-support-request.md). Wählen Sie **Abonnementverwaltung** als Problemtyp aus.

1. Quell- und Zielabonnement müssen im selben [Azure Active Directory-Mandanten](../../active-directory/develop/quickstart-create-new-tenant.md) vorhanden sein. Um zu überprüfen, ob beide Abonnements die gleiche Mandanten-ID aufweisen, verwenden Sie Azure PowerShell oder die Azure-Befehlszeilenschnittstelle.

   Verwenden Sie für Azure PowerShell Folgendes:

   ```azurepowershell-interactive
   (Get-AzSubscription -SubscriptionName <your-source-subscription>).TenantId
   (Get-AzSubscription -SubscriptionName <your-destination-subscription>).TenantId
   ```

   Verwenden Sie für die Azure-Befehlszeilenschnittstelle den folgenden Befehl:

   ```azurecli-interactive
   az account show --subscription <your-source-subscription> --query tenantId
   az account show --subscription <your-destination-subscription> --query tenantId
   ```

   Wenn die Mandanten-IDs für das Quell- und das Zielabonnement nicht gleich sind, verwenden Sie die folgenden Methoden, um die Mandanten-IDs aufeinander abzustimmen:

   * [Übertragen des Besitzes eines Azure-Abonnements auf ein anderes Konto](../../cost-management-billing/manage/billing-subscription-transfer.md)
   * [Zuweisen oder Hinzufügen eines Azure-Abonnements zu Azure Active Directory](../../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md)

1. Das Zielabonnement muss für den Ressourcenanbieter der verschobenen Ressource registriert sein. Andernfalls erhalten Sie eine Fehlermeldung, die besagt, dass das **Abonnement nicht für einen Ressourcentyp registriert ist**. Dieser Fehler kann auftreten, wenn eine Ressource in ein neues Abonnement verschoben wird, dieses aber noch nie mit diesem Ressourcentyp verwendet wurde.

   Verwenden Sie für PowerShell die folgenden Befehle zum Abrufen des Registrierungsstatus:

   ```azurepowershell-interactive
   Set-AzContext -Subscription <destination-subscription-name-or-id>
   Get-AzResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState
   ```

   Verwenden Sie zum Registrieren eines Ressourcenanbieters Folgendes:

   ```azurepowershell-interactive
   Register-AzResourceProvider -ProviderNamespace Microsoft.Batch
   ```

   Verwenden Sie für die Azure CLI die folgenden Befehle zum Abrufen des Registrierungsstatus:

   ```azurecli-interactive
   az account set -s <destination-subscription-name-or-id>
   az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
   ```

   Verwenden Sie zum Registrieren eines Ressourcenanbieters Folgendes:

   ```azurecli-interactive
   az provider register --namespace Microsoft.Batch
   ```

1. Das Konto, das die Ressourcen verschiebt, muss mindestens über folgende Berechtigungen verfügen:

   * **Microsoft.Resources/subscriptions/resourceGroups/moveResources/action** für die Quellressourcengruppe.
   * **Microsoft.Resources/subscriptions/resourceGroups/write** für die Zielressourcengruppe.

1. Überprüfen Sie vor dem Verschieben der Ressource die Abonnementkontingente für das Abonnement, zu dem Sie die Ressourcen verschieben. Wenn das Verschieben der Ressourcen bedeutet, dass das Abonnement seine Einschränkungen überschreitet, müssen Sie prüfen, ob Sie eine Erhöhung des Kontingents anfordern können. Eine vollständige Liste zu diesen Einschränkungen und Informationen zur Anforderung einer Kontingenterhöhung finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../../azure-resource-manager/management/azure-subscription-service-limits.md).

1. **Für eine abonnementübergreifende Verschiebung müssen die Ressource und die davon abhängigen Ressourcen in derselben Ressourcengruppe angeordnet sein und zusammen verschoben werden.** Für eine VM mit verwalteten Datenträgern müssen die VM und die verwalteten Datenträger beispielsweise gemeinsam und auch zusammen mit den anderen abhängigen Ressourcen verschoben werden.

   Wenn Sie eine Ressource in ein neues Abonnement verschieben, sollten Sie überprüfen, ob die Ressource über abhängige Ressourcen verfügt und ob diese sich in derselben Ressourcengruppe befinden. Wenn sich die Ressourcen nicht in derselben Ressourcengruppe befinden, sollten Sie überprüfen, ob die Ressourcen in derselben Ressourcengruppe kombiniert werden können. Wenn ja, müssen Sie alle Ressourcen in derselben Ressourcengruppe vereinen, indem Sie übergreifend für die Ressourcengruppen einen Verschiebungsvorgang durchführen.

   Weitere Informationen finden Sie unter [Szenario für eine abonnementübergreifende Verschiebung](#scenario-for-move-across-subscriptions).

## <a name="scenario-for-move-across-subscriptions"></a>Szenario für eine abonnementübergreifende Verschiebung

Das Verschieben von Ressourcen aus einem Abonnement in ein anderes ist ein dreistufiger Prozess:

![Szenario für abonnementübergreifende Verschiebung](./media/move-resource-group-and-subscription/cross-subscription-move-scenario.png)

Der Einfachheit halber verwenden wir hier nur eine abhängige Ressource.

* Schritt 1: Wenn abhängige Ressourcen auf unterschiedliche Ressourcengruppen verteilt sind, müssen Sie diese zuerst in einer Ressourcengruppe zusammenfassen.
* Schritt 2: Verschieben Sie die Ressource und die abhängigen Ressourcen zusammen aus dem Quellabonnement in das Zielabonnement.
* Schritt 3: Optional können Sie die abhängigen Ressourcen auf unterschiedliche Ressourcengruppen im Zielabonnement verteilen.

## <a name="use-the-portal"></a>Verwenden des Portals

Wählen Sie zum Verschieben von Ressourcen die Ressourcengruppe aus, die diese Ressourcen enthält.

Auswählen der Ressourcen, die Sie verschieben möchten Aktivieren Sie das Kontrollkästchen oben in der Liste, um alle Ressourcen zu verschieben. Oder wählen Sie Ressourcen einzeln aus.

:::image type="content" source="./media/move-resource-group-and-subscription/select-resources-to-move.png" alt-text="Auswählen von Ressourcen":::

Wählen Sie die Schaltfläche **Verschieben** aus.

:::image type="content" source="./media/move-resource-group-and-subscription/select-move.png" alt-text="Optionen für den Verschiebevorgang":::

Diese Schaltfläche bietet Ihnen drei Optionen:

* Verschieben in eine neue Ressourcengruppe.
* Verschieben in ein neues Abonnement.
* Verschieben in eine neue Region. Wenn Sie Regionen ändern möchten, finden Sie weitere Informationen unter [Regionsübergreifendes Verschieben von Ressourcen (aus Ressourcengruppe)](../../resource-mover/move-region-within-resource-group.md?toc=/azure/azure-resource-manager/management/toc.json).

Wählen Sie aus, ob Sie die Ressource in eine neue Ressourcengruppe oder ein neues Abonnement verschieben.

Die Quellressourcengruppe wird automatisch festgelegt. Wählen Sie die Zielressourcengruppe aus. Wenn Sie zu einem neuen Abonnement umstiegen, geben Sie auch das Abonnement an. Wählen Sie **Weiter** aus.

:::image type="content" source="./media/move-resource-group-and-subscription/select-destination-group.png" alt-text="Zielressourcengruppe auswählen":::

Das Portal überprüft, ob die Ressourcen verschoben werden können. Warten Sie, bis die Validierung abgeschlossen ist.

:::image type="content" source="./media/move-resource-group-and-subscription/validation.png" alt-text="Validierung verschobener Ressourcen":::

Wenn die Validierung erfolgreich abgeschlossen wurde, wählen Sie **Weiter** aus.

Bestätigen Sie, dass Sie Tools und Skripts für diese Ressourcen aktualisieren müssen. Um mit dem Verschieben der Ressourcen zu beginnen, wählen Sie **Verschieben** aus.

:::image type="content" source="./media/move-resource-group-and-subscription/acknowledge-change.png" alt-text="Auswählen des Ziels":::

Sobald das Verschieben abgeschlossen ist, werden Sie über das Ergebnis informiert.

:::image type="content" source="./media/move-resource-group-and-subscription/view-notification.png" alt-text="Ergebnisse des Verschiebens anzeigen":::

## <a name="use-azure-powershell"></a>Mithilfe von Azure PowerShell

### <a name="validate"></a>Überprüfen

Verwenden Sie den Befehl [Invoke-AzResourceAction](/powershell/module/az.resources/invoke-azresourceaction), um Ihre Verschiebeaktion zu testen, ohne die Ressourcen tatsächlich zu verschieben. Verwenden Sie diesen Befehl nur, wenn Sie die Ergebnisse vorab bestimmen müssen. Um diesen Vorgang ausführen zu können, benötigen Sie Folgendes:

* Ressourcen-ID der Quellressourcengruppe
* Ressourcen-ID der Zielressourcengruppe
* Ressourcen-ID der einzelnen zu verschiebenden Ressourcen

```azurepowershell
Invoke-AzResourceAction -Action validateMoveResources `
-ResourceId "/subscriptions/{subscription-id}/resourceGroups/{source-rg}" `
-Parameters @{ resources= @("/subscriptions/{subscription-id}/resourceGroups/{source-rg}/providers/{resource-provider}/{resource-type}/{resource-name}", "/subscriptions/{subscription-id}/resourceGroups/{source-rg}/providers/{resource-provider}/{resource-type}/{resource-name}", "/subscriptions/{subscription-id}/resourceGroups/{source-rg}/providers/{resource-provider}/{resource-type}/{resource-name}");targetResourceGroup = '/subscriptions/{subscription-id}/resourceGroups/{destination-rg}' }  
```

Wenn die Validierung erfolgreich ist, erfolgt keine Ausgabe.

Wenn die Validierung fehlschlägt, wird eine Fehlermeldung angezeigt, die beschreibt, warum die Ressourcen nicht verschoben werden können.

### <a name="move"></a>Move

Verwenden Sie zum Verschieben vorhandener Ressourcen in eine andere Ressourcengruppe oder ein anderes Abonnement den Befehl [Move-AzResource](/powershell/module/az.resources/move-azresource). Im folgenden Beispiel wird veranschaulicht, wie mehrere Ressourcen in eine neue Ressourcengruppe verschoben werden.

```azurepowershell-interactive
$webapp = Get-AzResource -ResourceGroupName OldRG -ResourceName ExampleSite
$plan = Get-AzResource -ResourceGroupName OldRG -ResourceName ExamplePlan
Move-AzResource -DestinationResourceGroupName NewRG -ResourceId $webapp.ResourceId, $plan.ResourceId
```

Um Ressourcen in ein neues Abonnement zu verschieben, schließen Sie einen Wert für den Parameter `DestinationSubscriptionId` ein.

## <a name="use-azure-cli"></a>Mithilfe der Azure-Befehlszeilenschnittstelle

### <a name="validate"></a>Überprüfen

Verwenden Sie den Befehl [az resource invoke-action](/cli/azure/resource#az_resource_invoke_action), um Ihre Verschiebeaktion zu testen, ohne die Ressourcen tatsächlich zu verschieben. Verwenden Sie diesen Befehl nur, wenn Sie die Ergebnisse vorab bestimmen müssen. Um diesen Vorgang ausführen zu können, benötigen Sie Folgendes:

* Ressourcen-ID der Quellressourcengruppe
* Ressourcen-ID der Zielressourcengruppe
* Ressourcen-ID der einzelnen zu verschiebenden Ressourcen

Verwenden Sie im Anforderungstext `\"`, um doppelte Anführungszeichen zu maskieren.

```azurecli
az resource invoke-action --action validateMoveResources \
  --ids "/subscriptions/{subscription-id}/resourceGroups/{source-rg}" \
  --request-body "{  \"resources\": [\"/subscriptions/{subscription-id}/resourceGroups/{source-rg}/providers/{resource-provider}/{resource-type}/{resource-name}\", \"/subscriptions/{subscription-id}/resourceGroups/{source-rg}/providers/{resource-provider}/{resource-type}/{resource-name}\", \"/subscriptions/{subscription-id}/resourceGroups/{source-rg}/providers/{resource-provider}/{resource-type}/{resource-name}\"],\"targetResourceGroup\":\"/subscriptions/{subscription-id}/resourceGroups/{destination-rg}\" }" 
```

Ausgabe bei erfolgreicher Validierung:

```azurecli
{} Finished .. 
```

Wenn die Validierung fehlschlägt, wird eine Fehlermeldung angezeigt, die beschreibt, warum die Ressourcen nicht verschoben werden können.

### <a name="move"></a>Move

Verwenden Sie zum Verschieben vorhandener Ressourcen in eine andere Ressourcengruppe oder ein anderes Abonnement den Befehl [az resource move](/cli/azure/resource#az_resource_move). Geben Sie die Ressourcen-IDs der zu verschiebenden Ressourcen an. Im folgenden Beispiel wird veranschaulicht, wie mehrere Ressourcen in eine neue Ressourcengruppe verschoben werden. Geben Sie im `--ids`-Parameter eine durch Leerzeichen getrennte Liste der zu verschiebenden Ressourcen-IDs an.

```azurecli
webapp=$(az resource show -g OldRG -n ExampleSite --resource-type "Microsoft.Web/sites" --query id --output tsv)
plan=$(az resource show -g OldRG -n ExamplePlan --resource-type "Microsoft.Web/serverfarms" --query id --output tsv)
az resource move --destination-group newgroup --ids $webapp $plan
```

Um Ressourcen in ein neues Abonnement zu verschieben, geben Sie den Parameter`--destination-subscription-id` an.

## <a name="use-rest-api"></a>REST-API

### <a name="validate"></a>Überprüfen

Der [Vorgang zum Überprüfen der Verschiebung](/rest/api/resources/resources/validate-move-resources) ermöglicht Ihnen, Ihr Verschiebungsszenario zu testen, ohne tatsächlich Ressourcen zu verschieben. Verwenden Sie diesen Vorgang, um zu überprüfen, ob die Verschiebung erfolgreich sein wird. Die Überprüfung wird automatisch aufgerufen, wenn Sie eine Verschiebeanforderung senden. Verwenden Sie diesen Vorgang nur, wenn Sie die Ergebnisse vorab bestimmen müssen. Um diesen Vorgang ausführen zu können, benötigen Sie Folgendes:

* Name der Quellressourcengruppe
* Ressourcen-ID der Zielressourcengruppe
* Ressourcen-ID der einzelnen zu verschiebenden Ressourcen
* [Zugriffstoken](/rest/api/azure/#acquire-an-access-token) für Ihr Konto

Senden Sie die folgende Anforderung:

```HTTP
POST https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<source-group>/validateMoveResources?api-version=2019-05-10
Authorization: Bearer <access-token>
Content-type: application/json
```

Mit dem Anforderungstext:

```json
{
 "resources": ["<resource-id-1>", "<resource-id-2>"],
 "targetResourceGroup": "/subscriptions/<subscription-id>/resourceGroups/<target-group>"
}
```

Wenn die Anforderung ordnungsgemäß formatiert ist, gibt der Vorgang Folgendes zurück:

```HTTP
Response Code: 202
cache-control: no-cache
pragma: no-cache
expires: -1
location: https://management.azure.com/subscriptions/<subscription-id>/operationresults/<operation-id>?api-version=2018-02-01
retry-after: 15
...
```

Die Statuscode 202 gibt an, dass die Überprüfungsanforderung akzeptiert wurde, jedoch noch nicht ermittelt wurde, ob der Verschiebungsvorgang erfolgreich verläuft. Die Wert `location` enthält eine URL, über die Sie den Status des lang andauernden Vorgangs überprüfen können.

Um den Status zu überprüfen, senden Sie die folgende Anforderung:

```HTTP
GET <location-url>
Authorization: Bearer <access-token>
```

Während der Vorgang ausgeführt wird, wird weiterhin der Statuscode 202 angezeigt. Warten Sie die Anzahl von Sekunden, die im Wert `retry-after` angegeben ist, bevor Sie es erneut versuchen. Wenn der Verschiebungsvorgang erfolgreich überprüft wurde, erhalten Sie den Statuscode 204. Wenn bei der Verschiebungsüberprüfung ein Fehler auftritt, erhalten Sie eine Fehlermeldung, z.B.:

```json
{"error":{"code":"ResourceMoveProviderValidationFailed","message":"<message>"...}}
```

### <a name="move"></a>Move

Verwenden Sie zum Verschieben vorhandener Ressourcen in eine andere Ressourcengruppe oder ein anderes Abonnement den Vorgang zum [Verschieben von Ressourcen](/rest/api/resources/resources/moveresources).

```HTTP
POST https://management.azure.com/subscriptions/{source-subscription-id}/resourcegroups/{source-resource-group-name}/moveResources?api-version={api-version}
```

Geben Sie im Anforderungstext die Zielgruppe und die zu verschiebenden Ressourcen an.

```json
{
 "resources": ["<resource-id-1>", "<resource-id-2>"],
 "targetResourceGroup": "/subscriptions/<subscription-id>/resourceGroups/<target-group>"
}
```

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

**Frage: Mein Ressourcenverschiebevorgang, der normalerweise ein paar Minuten dauert, läuft schon seit fast einer Stunde. Stimmt da etwas nicht?**

Das Verschieben einer Ressource ist ein komplexer Vorgang, der in verschiedenen Phasen erfolgt. Es kann mehr als nur den Ressourcenanbieter der Ressource umfassen, die Sie verschieben möchten. Aufgrund der Abhängigkeiten zwischen Ressourcenanbietern stellt Azure Resource Manager vier Stunden bereit, um den Vorgang abzuschließen. Dieser Zeitraum gibt Ressourcenanbietern die Möglichkeit, sich von vorübergehenden Problemen wiederherzustellen. Wenn Ihre Verschiebeanforderung innerhalb des Zeitraums von vier Stunden liegt, wird weiterhin versucht, den Vorgang abzuschließen. Dies kann auch erfolgreich sein. Die Quell- und Zielressourcengruppen sind während dieses Zeitraums gesperrt, um Konsistenzprobleme zu vermeiden.

**Frage: Warum ist meine Ressourcengruppe während der Ressourcenverschiebung vier Stunden lang gesperrt?**

Eine Verschiebeanforderung darf maximal vier Stunden dauern. Um Änderungen an den zu verschiebenden Ressourcen zu verhindern, werden sowohl die Quell- als auch die Zielressourcengruppe während der Ressourcenverschiebung gesperrt.

Es gibt zwei Phasen bei einer Verschiebeanforderung. In der ersten Phase wird die Ressource verschoben. In der zweiten Phase werden Benachrichtigungen an andere Ressourcenanbieter gesendet, die von der Ressource abhängen, die verschoben wird. Eine Ressourcengruppe kann für das gesamte Zeitfenster von vier Stunden gesperrt werden, wenn ein Ressourcenanbieter in einer der beiden Phasen fehlschlägt. Während des gewährten Zeitraums versucht Resource Manager, den fehlgeschlagenen Schritt zu wiederholen.

Wenn eine Ressource nicht innerhalb des Zeitfensters von vier Stunden verschoben werden kann, entsperrt Resource Manager beide Ressourcengruppen. Ressourcen, die erfolgreich verschoben wurden, befinden sich in der Zielressourcengruppe. Ressourcen, die nicht verschoben werden konnten, verbleiben in der Quellressourcengruppe.

**Frage: Welche Auswirkungen hat es, dass die Quell- und Zielressourcengruppen während der Ressourcenverschiebung gesperrt werden?**

Die Sperrung verhindert, dass Sie eine Ressourcengruppe löschen, eine neue Ressource in einer der Ressourcengruppen erstellen oder eine der an der Verschiebung beteiligten Ressourcen löschen.

In der folgenden Abbildung wird eine Fehlermeldung des Azure-Portals angezeigt, wenn ein Benutzer versucht, eine Ressourcengruppe zu löschen, die Bestandteil einer laufenden Verschiebung ist.

![Verschiebefehlermeldung bei Löschversuch](./media/move-resource-group-and-subscription/move-error-delete.png)

**Frage: Was bedeutet der Fehlercode „MissingMoveDependentResources“?**

Beim Verschieben einer Ressource müssen deren abhängige Ressourcen in der Zielressourcengruppe oder im Zielabonnement vorhanden sein oder in die Verschiebeanforderung eingeschlossen werden. Sie erhalten den Fehlercode „MissingMoveDependentResources“, wenn eine abhängige Ressource diese Anforderung nicht erfüllt. Die Fehlermeldung enthält Details über die abhängige Ressource, die in die Verschiebeanforderung eingeschlossen werden muss.

Beispielsweise könnte das Verschieben eines virtuellen Computers erfordern, dass sieben Ressourcentypen mit drei verschiedenen Ressourcenanbietern verschoben werden. Diese Ressourcenanbieter und -typen sind:

* Microsoft.Compute

  * virtualMachines
  * disks
* Microsoft.Network
  * networkInterfaces
  * publicIPAddresses
  * networkSecurityGroups
  * virtualNetworks
* Microsoft.Storage
  * storageAccounts

Ein weiteres gängiges Beispiel beinhaltet das Verschieben eines virtuellen Netzwerks. Sie müssen möglicherweise mehrere weitere Ressourcen verschieben, die diesem virtuellen Netzwerk zugeordnet sind. Die Verschiebeanforderung kann es erfordern, dass öffentliche IP-Adressen, Routingtabellen, virtuelle Netzwerkgateways, Netzwerksicherheitsgruppen und anderes verschoben werden.

**Frage: Was bedeutet der Fehlercode „RequestDisallowedByPolicy“?**

Resource Manager validiert Ihre Verschiebungsanforderung, bevor versucht wird, sie auszuführen. Diese Validierung umfasst das Überprüfen von Richtlinien, die für die an der verschobenen Ressourcen definiert sind. Wenn Sie z. B. versuchen, einen Schlüsseltresor zu verschieben, Ihre Organisation jedoch über eine Richtlinie verfügt, um die Erstellung eines Schlüsseltresors in der Zielressourcengruppe zu verweigern, schlägt die Validierung fehl, und die Verschiebung wird blockiert. Der zurückgegebene Fehlercode ist **RequestDisallowedByPolicy**. 

Weitere Informationen zu Richtlinien finden Sie unter [Was ist Azure Policy?](../../governance/policy/overview.md).

**Frage: Warum kann ich einige Ressourcen in Azure nicht verschieben?**

Derzeit unterstützen nicht alle Ressourcen in Azure das Verschieben. Eine Liste der Ressourcen, die das Verschieben unterstützen, finden Sie unter [Unterstützung des Verschiebevorgangs für Ressourcen](move-support-resources.md).

**Frage: Wie viele Ressourcen kann ich in einem einzigen Vorgang verschieben?**

Unterteilen Sie große Verschiebevorgänge nach Möglichkeit in separate Verschiebevorgänge. Resource Manager gibt sofort einen Fehler aus, wenn ein einziger Vorgang mehr als 800 Ressourcen umfasst. Beim Verschieben von weniger als 800 Ressourcen kann jedoch ebenfalls ein Fehler durch ein Timeout auftreten.

**Frage: Was bedeutet der Fehler, dass eine Ressource nicht erfolgreich ist?**

Wenn Sie eine Fehlermeldung erhalten, die besagt, dass eine Ressource nicht verschoben werden kann, da sie sich nicht in einem erfolgreichen Zustand befindet, blockiert möglicherweise eine abhängige Ressource die Verschiebung. In der Regel lautet der Fehlercode **MoveCannotProceedWithResourcesNotInSucceededState**.

Wenn die Quell- oder Zielgruppenressource ein virtuelles Netzwerk enthält, werden während des Verschiebens die Zustände aller abhängigen Ressourcen des virtuellen Netzwerks überprüft. Die Überprüfung umfasst direkt und indirekt vom virtuellen Netzwerk abhängige Ressourcen. Wenn eine dieser Ressourcen den Zustand „Ausgefallen“ aufweist, wird die Verschiebung blockiert. Wenn beispielsweise ein virtueller Computer, der das virtuelle Netzwerk nutzt, ausgefallen ist, wird das Verschieben blockiert. Das Verschieben wird selbst dann blockiert, wenn der virtuelle Computer nicht zu den zu verschiebenden Ressourcen gehört und sich nicht in einer der Ressourcengruppen des Verschiebevorgangs befindet.

Wenn Sie diesen Fehler erhalten, haben Sie zwei Möglichkeiten. Verschieben Sie entweder Ihre Ressourcen in eine Ressourcengruppe ohne virtuelles Netzwerk, oder [kontaktieren Sie den Support](../../azure-portal/supportability/how-to-create-azure-support-request.md).

## <a name="next-steps"></a>Nächste Schritte

Eine Liste der Ressourcen, die das Verschieben unterstützen, finden Sie unter [Unterstützung des Verschiebevorgangs für Ressourcen](move-support-resources.md).
