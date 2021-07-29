---
title: Azure Arc-fähigen Server zu Azure migrieren
description: Erfahren Sie, wie Sie Ihre Azure Arc-fähigen Server, die lokal oder in einer anderen Cloudumgebung ausgeführt werden, zu Azure migrieren können.
ms.date: 05/06/2021
ms.topic: conceptual
ms.openlocfilehash: 4c8a5ea1974e251ba147fb7ee09b9ac49c364876
ms.sourcegitcommit: c1b0d0b61ef7635d008954a0d247a2c94c1a876f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2021
ms.locfileid: "109629492"
---
# <a name="migrate-your-on-premises-or-other-cloud-arc-enabled-server-to-azure"></a>Migrieren Ihres lokalen oder anderen Arc-fähigen Cloudservers zu Azure

Dieser Artikel soll Sie bei der Planung und erfolgreichen Migration Ihres lokalen Servers oder virtuellen Computers, der von Azure Arc-fähigen Servern verwaltet wird, zu Azure unterstützen. Durch Ausführen dieser Schritte können Sie die Verwaltung von Arc-fähigen Servern basierend auf den installierten unterstützten VM-Erweiterungen und Azure-Diensten basierend auf der Arc-Serverressourcenidentität übertragen.

Bevor Sie diese Schritte ausführen, lesen Sie den Artikel [Vorbereiten von lokalen Computern für die Migration zu Azure](../../migrate/prepare-for-migration.md). Hier erfahren Sie, was Sie tun müssen, um Azure Migrate verwenden zu können.

In diesem Artikel führen Sie folgende Schritte aus:

* Inventarisieren unterstützter Azure Arc-fähiger Server mit installierten VM-Erweiterungen.
* Deinstallieren Sie alle VM-Erweiterungen vom Arc-fähigen Server.
* Identifizieren Sie die Azure-Dienste, die für die Authentifizierung bei Ihrer Arc-Server-verwalteten Identität konfiguriert sind, und bereiten Sie die Aktualisierung dieser Dienste für die Verwendung der Azure-VM-Identität nach der Migration vor.
* Überprüfen Sie die Azure RBAC-Zugriffsrechte (rollenbasierte Zugriffssteuerung in Azure), die der Arc-fähigen Serverressource gewährt wurden, um zu verwalten, wer Zugriff auf die Ressource hat, nachdem sie auf eine Azure-VM migriert wurde. 
* Löschen Sie die Arc-fähige Serverressourcenidentität aus Azure und entfernen Sie den Arc-fähigen Server-Agent.
* Installieren Sie den Azure-Gast-Agent.
* Migrieren Sie den Server oder die VM zu Azure.

## <a name="step-1-inventory-and-remove-vm-extensions"></a>Schritt 1: Inventarisieren und Entfernen von VM-Erweiterungen

Um die VM-Erweiterungen, die auf Ihrem Arc-fähigen Server installiert sind, zu inventarisieren, können Sie diese mithilfe von Azure CLI oder Azure PowerShell in einer Liste darstellen.

Verwenden Sie mit Azure PowerShell den Befehl [Get-AzConnectedMachineExtension](/powershell/module/az.connectedmachine/get-azconnectedmachineextension) mit den Parametern `-MachineName` und `-ResourceGroupName`.

Verwenden Sie mit Azure CLI den Befehl [az connectedmachine extension list](/cli/azure/ext/connectedmachine/connectedmachine/extension#ext_connectedmachine_az_connectedmachine_extension_list) mit den Parametern `--machine-name` und `--resource-group`. Standardmäßig erfolgt die Ausgabe von Azure CLI-Befehlen in JSON (JavaScript Object Notation). Um beispielsweise die Standardausgabe in eine Liste oder Tabelle zu ändern, verwenden Sie [az configure --output](/cli/azure/reference-index). Ferner können Sie `--output` an beliebige Befehle anfügen, um eine einmalige Änderung des Ausgabeformats zu erreichen.

Nachdem Sie identifiziert haben, welche VM-Erweiterungen bereitgestellt werden, können Sie sie mithilfe von [Azure-Portal](manage-vm-extensions-portal.md) über [Azure PowerShell ](manage-vm-extensions-powershell.md) oder [Azure CLI](manage-vm-extensions-cli.md) entfernen. Wenn die VM-Erweiterung der Protokollanalyse oder die VM-Erweiterung des Abhängigkeits-Agents mithilfe von Azure Policy und der [VM Erkenntnis-Initiative](../../azure-monitor/vm/vminsights-enable-policy.md) bereitgestellt wurde, muss [eine Ausnahme erstellt](../../governance/policy/tutorials/create-and-manage.md#remove-a-non-compliant-or-denied-resource-from-the-scope-with-an-exclusion) werden, um eine erneute Auswertung und Bereitstellung der Erweiterungen auf dem Arc-fähigen Server zu verhindern, bevor die Migration abgeschlossen ist.

## <a name="step-2-review-access-rights"></a>Schritt 2: Überprüfen der Zugriffsrechte 

Erstellen Sie eine Liste der Rollenzuweisungen für die Arc-fähige Serverressource, indem Sie die Ergebnisse mit [Azure PowerShell](../../role-based-access-control/role-assignments-list-powershell.md#list-role-assignments-for-a-resource) und mit anderem PowerShell-Code in CSV oder ein anderes Format exportieren. 

Wenn Sie eine verwaltete Identität für eine Anwendung oder einen Prozess verwenden, die/der auf einem Arc-fähigen Server ausgeführt wird, müssen Sie sicherstellen, dass dem virtuellen Azure-Computer eine verwaltete Identität zugewiesen ist. Um die Rollenzuweisung für eine verwaltete Identität anzuzeigen, können Sie das cmdlet Azure PowerShell `Get-AzADServicePrincipal` verwenden. Weitere Informationen finden Sie unter [Auflisten von Rollenzuweisungen für eine verwaltete Identität](../../role-based-access-control/role-assignments-list-powershell.md#list-role-assignments-for-a-managed-identity). 

Eine vom System verwaltete Identität wird auch verwendet, wenn Azure Policy genutzt wird, um Einstellungen auf einem Computer oder Server zu überwachen. Bei Arc-fähigen Servern ist der Gastkonfigurations-Agent enthalten und führt die Überprüfung der Überwachungseinstellungen durch. Nach der Migration finden Sie unter [Anforderungen zur Bereitstellung virtueller Azure-Computer](../../governance/policy/concepts/guest-configuration.md#deploy-requirements-for-azure-virtual-machines) Informationen zum Konfigurieren Ihrer Azure-VM manuell oder mit der Richtlinie mit der Gastkonfigurationserweiterung.

Aktualisieren Sie die Rollenzuweisung mit allen Ressourcen, auf die von der verwalteten Identität zugegriffen wird, damit sich die neue Azure-VM-Identität bei diesen Diensten authentifizieren kann. Im Folgenden erfahren Sie, [wie verwaltete Identitäten für Azure-Ressourcen für einen virtuellen Azure-Computer (VM) funktionieren](../../active-directory/managed-identities-azure-resources/how-managed-identities-work-vm.md).

## <a name="step-3-disconnect-from-azure-arc-and-uninstall-agent"></a>Schritt 3: Trennen der Verbindung mit Azure Arc und Deinstallieren des Agents

Löschen Sie die Ressourcen-ID des Arc-fähigen Servers in Azure mit einer der folgenden Methoden:

   * Ausführen des Befehls `azcmagent disconnect` auf dem Computer oder dem Server

   * Durch Auswählen des registrierten Servers im Azure-Portal und Klicken auf **Löschen** in der oberen Leiste

   * Mithilfe der [Azure CLI](../../azure-resource-manager/management/delete-resource-group.md?tabs=azure-cli#delete-resource) oder mithilfe von [Azure PowerShell](../../azure-resource-manager/management/delete-resource-group.md?tabs=azure-powershell#delete-resource) Verwenden Sie `Microsoft.HybridCompute/machines` für den Parameter `ResourceType`.

Entfernen Sie dann den Windows- oder Linux-Agent der Azure Arc-fähigen Server und befolgen Sie dazu die Schritte zum [Entfernen von Agents](manage-agent.md#remove-the-agent).

## <a name="step-4-install-the-azure-guest-agent"></a>Schritt 4: Installieren des Azure-Gast-Agents

Auf VMs, die aus der lokalen Umgebung zu Azure migriert werden, ist der Linux oder Windows Azure-Gast-Agent nicht installiert. In diesen Szenarien müssen Sie den VM-Agent manuell installieren. Weitere Informationen zum Installieren des VM-Agents finden Sie unter [Übersicht über den Windows-Agent für virtuelle Azure-Computer](../../virtual-machines/extensions/agent-windows.md) oder [Übersicht über den Linux-Agent für virtuelle Azure Computer](../../virtual-machines/extensions/agent-linux.md).

## <a name="step-5-migrate-server-or-machine-to-azure"></a>Schritt 5: Migrieren eines Servers oder Computers zu Azure

Bevor Sie mit der Migration mit Azure-Migration fortfahren, lesen Sie den Artikel [Vorbereiten von lokalen Computern für die Migration zu Azure](../../migrate/prepare-for-migration.md), um mehr über die Anforderungen zu erfahren, die für die Verwendung von Azure Migrate erforderlich sind. Informationen zum Abschließen der Migration zu Azure finden Sie basierend auf Ihrer Umgebung in den [Migrationsoptionen](../../migrate/prepare-for-migration.md#next-steps) für Azure Migrate.

## <a name="step-6-deploy-azure-vm-extensions"></a>Schritt 6: Bereitstellen von Azure-VM-Erweiterungen

Nach der Migration und dem Abschluss aller Konfigurationsschritte nach der Migration können Sie nun die Azure-VM-Erweiterungen basierend auf den VM-Erweiterungen bereitstellen, die ursprünglich auf Ihrem Arc-fähigen Server installiert waren. Lesen Sie das Kapitel [Erweiterungen und Features für virtuelle Azure-Computer](../../virtual-machines/extensions/overview.md), um die Bereitstellung Ihrer Erweiterung zu planen. 

Informationen zum Fortsetzen der Verwendung von Überwachungseinstellungen auf einem Computer mit Azure Policy Gastkonfigurationsrichtliniendefinitionen finden Sie unter [Aktivieren der Gastkonfiguration](../../governance/policy/concepts/guest-configuration.md#enable-guest-configuration).

Wenn die VM-Erweiterung der Protokollanalyse oder die VM-Erweiterung des Abhängigkeits-Agent mithilfe von Azure Policy der [VM-Erkenntnis-Initiative](../../azure-monitor/vm/vminsights-enable-policy.md) bereitgestellt wurde, entfernen Sie die [Ausnahme](../../governance/policy/tutorials/create-and-manage.md#remove-a-non-compliant-or-denied-resource-from-the-scope-with-an-exclusion), den Sie zuvor erstellt haben. Informationen, wie Sie Azure Policy zum Aktivieren virtueller Azure-Computer verwenden können, finden Sie in [Bereitstellen von Azure Monitor im großen Stil mithilfe von Azure Policy](../../azure-monitor/deploy-scale.md#vm-insights). 

## <a name="next-steps"></a>Nächste Schritte

Informationen zur Problembehandlung finden Sie im [Leitfaden zur Problembehandlung des Connected Machine-Agents](troubleshoot-agent-onboard.md).