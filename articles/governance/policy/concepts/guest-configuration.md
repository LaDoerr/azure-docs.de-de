---
title: Informationen zum Überwachen der Inhalte virtueller Computer
description: Hier erfahren Sie, wie Azure Policy mithilfe des Gastkonfigurationsclients Einstellungen in VMs überwacht.
ms.date: 05/01/2021
ms.topic: conceptual
ms.openlocfilehash: 6ecfd3fd9f426676fe0b5c9a69af26b1245b7824
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122339839"
---
# <a name="understand-azure-policys-guest-configuration"></a>Informationen zu Guest Configuration von Azure Policy

Mit Azure Policy können Einstellungen innerhalb eines Computers überwacht werden. Dies gilt für in Azure ausgeführte Computer sowie für [über Arc verbundene Computer](../../../azure-arc/servers/overview.md). Für die Überprüfung verwenden Sie die Erweiterung und den Client Guest Configuration. Die Erweiterung überprüft über den Client u. a. die folgenden Einstellungen:

- Die Konfiguration des Betriebssystems
- Die Konfiguration oder das Vorhandensein der Anwendung
- Umgebungseinstellungen

Derzeit wird für die meisten Richtliniendefinitionen der Azure Policy-Gastkonfiguration nur eine Überprüfung der Einstellungen auf dem Computer durchgeführt. Es werden keine Konfigurationen angewendet. Eine Ausnahme ist hierbei eine integrierte Richtlinie, die [weiter unten beschrieben ist](#applying-configurations-using-guest-configuration).

[Für dieses Dokument ist ein Video zur exemplarischen Vorgehensweise verfügbar](https://youtu.be/Y6ryD3gTHOs).

## <a name="enable-guest-configuration"></a>Aktivieren der Gastkonfiguration

Überprüfen Sie die folgenden Details, um den Status von Computern in Ihrer Umgebung zu überwachen, einschließlich Computern in Azure und über Arc verbundener Computer.

## <a name="resource-provider"></a>Ressourcenanbieter

Bevor Sie Guest Configuration verwenden können, müssen Sie den Ressourcenanbieter registrieren.
Wenn die Zuweisung einer Gastkonfigurationsrichtlinie über das Portal erfolgt oder das Abonnement in Azure Security Center registriert ist, wird der Ressourcenanbieter automatisch registriert. Hierfür können Sie über das [Portal](../../../azure-resource-manager/management/resource-providers-and-types.md#azure-portal), die [Azure PowerShell](../../../azure-resource-manager/management/resource-providers-and-types.md#azure-powershell) oder [Azure CLI](../../../azure-resource-manager/management/resource-providers-and-types.md#azure-cli) manuell registrieren.

## <a name="deploy-requirements-for-azure-virtual-machines"></a>Bereitstellen von Anforderungen für virtuelle Azure-Computer

Zum Überwachen von Einstellungen innerhalb eines Computers ist eine [VM-Erweiterung](../../../virtual-machines/extensions/overview.md) aktiviert, und der Computer muss über eine systemseitig verwaltete Identität verfügen. Mit der Erweiterung werden anwendbare Richtlinienzuweisungen sowie die entsprechende Konfigurationsdefinition heruntergeladen. Die Identität wird verwendet, um den Computer zu authentifizieren, wenn Lese- und Schreibvorgänge im Gastkonfigurationsdienst durchgeführt werden. Die Erweiterung ist für über Arc verbundene Computer nicht erforderlich, da sie im Agent für über Arc verbundene Computer enthalten ist.

> [!IMPORTANT]
> Die Gastkonfigurationserweiterung und eine verwaltete Identität sind zum Überwachen von virtuellen Azure-Computern erforderlich. Weisen Sie die folgenden Richtlinieninitiative zu, um die Erweiterung im gewünschten Umfang bereitzustellen:
>
> `Deploy prerequisites to enable Guest Configuration policies on virtual machines`

### <a name="limits-set-on-the-extension"></a>Für die Erweiterung festgelegte Grenzwerte

Um die Auswirkungen der Erweiterung auf die auf dem Computer ausgeführten Anwendungen zu beschränken, darf die Gastkonfiguration höchstens 5 % der CPU auslasten. Diese Einschränkung gilt sowohl für integrierte als auch für angepasste Definitionen. Dies gilt auch für den Gastkonfigurationsdienst im Agent für über Arc verbundene Computer.

### <a name="validation-tools"></a>Überprüfungstools

Auf dem Computer verwendet der Gastkonfigurationsclient lokale Tools zum Ausführen der Überprüfung.

In der folgenden Tabelle sind die lokalen Tools aufgeführt, die unter den jeweiligen unterstützten Betriebssystemen verwendet werden. Bei integrierten Inhalten werden diese Tools in der Gastkonfiguration automatisch geladen.

|Betriebssystem|Überprüfungstool|Notizen|
|-|-|-|
|Windows|[PowerShell Desired State Configuration](/powershell/scripting/dsc/overview/overview) v2| Wird mittels Sideloading in einen Ordner quergeladen, der nur von Azure Policy verwendet wird. Verursacht keinen Konflikt mit Windows PowerShell DSC. PowerShell Core wird nicht zum Systempfad hinzugefügt.|
|Linux|[Chef InSpec](https://www.chef.io/inspec/) | Installiert Chef InSpec (Version 2.2.61) am Standardspeicherort und wird dem Systempfad hinzugefügt. Abhängigkeiten für das InSpec-Paket einschließlich Ruby und Python werden ebenfalls installiert. |

### <a name="validation-frequency"></a>Validierungshäufigkeit

Der Guest Configuration-Client überprüft alle 5 Minuten, ob Gastzuweisungen hinzugefügt oder geändert wurden. Nachdem eine Gastzuweisung empfangen wurde, werden die Einstellungen für diese Konfiguration alle 15 Minuten erneut überprüft. Die Ergebnisse werden an den Guest Configuration-Ressourcenanbieter gesendet, wenn die Überwachung abgeschlossen ist.
Wenn eine [Auswertungsauslöser](../how-to/get-compliance-data.md#evaluation-triggers)-Richtlinie auftritt, wird der Zustand des Computers an den Guest Configuration-Ressourcenanbieter geschrieben. Dieses Update bewirkt, dass Azure Policy die Azure Resource Manager-Eigenschaften auswertet. Eine bedarfsgesteuerte Auswertung durch Azure Policy ruft den aktuellen Wert beim Guest Configuration-Ressourcenanbieter ab. Es wird jedoch keine neue Überwachung der Konfiguration auf dem Computer ausgelöst. Der Status wird gleichzeitig in Azure Resource Graph geschrieben.

## <a name="supported-client-types"></a>Unterstützte Clienttypen

Richtliniendefinitionen der Gastkonfiguration enthalten die neuen Versionen. Ältere Versionen von Betriebssystemen, die im Azure Marketplace verfügbar sind, sind ausgeschlossen, wenn der Gastkonfigurationsclient nicht kompatibel ist. In der folgenden Tabelle werden die für Azure-Images unterstützten Betriebssysteme aufgeführt.
Die Angabe „.x“ steht symbolisch für neue Nebenversionen von Linux-Distributionen.

|Herausgeber|Name|Versionen|
|-|-|-|
|Canonical|Ubuntu Server|14.04 - 20.x|
|Credativ|Debian|8 - 10.x|
|Microsoft|Windows Server|2012–2019|
|Microsoft|Windows-Client|Windows 10|
|OpenLogic|CentOS|7.3 -8.x|
|Red Hat|Red Hat Enterprise Linux\*|7.4 - 8.x|
|SUSE|SLES|12 SP3-SP5, 15.x|

\* Red Hat CoreOS wird nicht unterstützt.

Benutzerdefinierte Images von virtuellen Computern werden von Richtliniendefinitionen der Gastkonfiguration unterstützt, sofern es sich um eines der Betriebssysteme in der obigen Tabelle handelt.

## <a name="network-requirements"></a>Netzwerkanforderungen

Virtuelle Computer in Azure können entweder ihren lokalen Netzwerkadapter oder einen privaten Link verwenden, um mit dem Gastkonfigurationsdienst zu kommunizieren.

Azure Arc-Computer stellen mithilfe der lokalen Netzwerkinfrastruktur eine Verbindung her, um Azure-Dienste zu erreichen und den Compliancestatus zu melden.

### <a name="communicate-over-virtual-networks-in-azure"></a>Kommunizieren über virtuelle Netzwerke in Azure

Für die Kommunikation mit dem Gastkonfigurations-Ressourcenanbieter in Azure benötigen Computer ausgehenden Zugriff auf Azure-Rechenzentren über Port **443**. Wenn ein Netzwerk in Azure keinen ausgehenden Datenverkehr zulässt, müssen Ausnahmen über [Netzwerksicherheitsgruppen](../../../virtual-network/manage-network-security-group.md#create-a-security-rule)-Regeln konfiguriert werden. Die [Diensttags](../../../virtual-network/service-tags-overview.md) „AzureArcInfrastructure“ und „Storage“ kann verwendet werden, um auf die Gästekonfigurations- und Speicherdienste zu verweisen, anstatt die [Liste der IP-Bereiche](https://www.microsoft.com/download/details.aspx?id=56519) für Azure-Rechenzentren manuell zu führen. Beide Tags sind erforderlich, da Gastkonfigurations-Inhaltspakete von Azure Storage gehostet werden.

### <a name="communicate-over-private-link-in-azure"></a>Kommunizieren über Private Link in Azure

Für die Kommunikation mit dem Gastkonfigurationsdienst können virtuelle Computer einen [privaten Link](../../../private-link/private-link-overview.md) verwendet. Wenden Sie ein Tag mit dem Namen `EnablePrivateNetworkGC` und dem Wert `TRUE` an, um dieses Feature zu aktivieren. Das Tag kann vor oder nach der Anwendung von Richtliniendefinitionen der Gastkonfiguration auf den Computer angewendet werden.

Der Datenverkehr wird mithilfe der [virtuellen öffentlichen IP-Adresse](../../../virtual-network/what-is-ip-address-168-63-129-16.md) von Azure weitergeleitet, um einen sicheren, authentifizierten Kanal mit Azure-Plattformressourcen einzurichten.

### <a name="azure-arc-connected-machines"></a>Vernetzte Azure Arc-Computer

Knoten, die sich außerhalb von Azure befinden und über Azure Arc verbunden sind, benötigen eine Verbindung mit dem Gastkonfigurationsdienst. Details zu den Netzwerk- und Proxyanforderungen finden Sie in der [Azure Arc-Dokumentation](../../../azure-arc/servers/overview.md).

Lassen Sie für über Arc verbundene Server in privaten Rechenzentren Datenverkehr über die folgenden Muster zu:

- Port: Für ausgehenden Zugriff auf das Internet ist nur TCP 443 erforderlich
- Globale URL: `*.guestconfiguration.azure.com`

## <a name="managed-identity-requirements"></a>Anforderungen für verwaltete Identitäten

Durch Richtliniendefinitionen in der Initiative _Voraussetzungen zum Aktivieren der Gastkonfigurationsrichtlinien auf VMs bereitstellen_ wird eine systemseitig zugewiesene verwaltete Identität aktiviert, falls noch nicht vorhanden. Die Initiative enthält zwei Richtliniendefinitionen, durch die die Identitätserstellung verwaltet wird. Die IF-Bedingungen in den Richtliniendefinitionen gewährleisten das korrekte Verhalten basierend auf dem aktuellen Zustand der Computerressource in Azure.

Wenn der Computer derzeit keine verwalteten Identitäten aufweist, lautet die effektive Richtlinie: [Systemseitig zugewiesene verwaltete Identität hinzufügen, um Gastkonfigurationszuweisungen auf VMs ohne Identität zu aktivieren](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F3cf2ab00-13f1-4d0c-8971-2ac904541a7e)

Wenn der Computer derzeit über eine benutzerseitig zugewiesene Systemidentität verfügt, lautet die effektive Richtlinie: [Systemseitig zugewiesene verwaltete Identität hinzufügen, um Gastkonfigurationszuweisungen auf VMs mit einer benutzerseitig zugewiesenen Identität zu aktivieren](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F497dff13-db2a-4c0f-8603-28fa3b331ab6)

## <a name="guest-configuration-definition-requirements"></a>Anforderungen an die Guest Configuration-Definition

Die Richtliniendefinitionen der Gastkonfiguration verwenden den Effekt **AuditIfNotExists**. Wenn die Definition zugewiesen ist, verarbeitet ein Back-End-Dienst automatisch den Lebenszyklus aller Anforderungen im Azure-Ressourcenanbieter `Microsoft.GuestConfiguration`.

Die **AuditIfNotExists**-Richtliniendefinitionen geben erst dann Konformitätsergebnisse zurück, wenn alle Anforderungen auf dem Computer erfüllt sind. Eine Beschreibung der Anforderungen finden Sie im Abschnitt [Bereitstellen von Anforderungen für virtuelle Azure-Computer](#deploy-requirements-for-azure-virtual-machines).

> [!IMPORTANT]
> In einem früheren Release der Gastkonfiguration war eine Initiative erforderlich, um die Definitionen **DeployIfNotExists** und **AuditIfNotExists** zu kombinieren. **DeployIfNotExists**-Definitionen sind nicht mehr erforderlich. Die Definitionen und Initiativen sind als `[Deprecated]` gekennzeichnet, aber vorhandene Zuweisungen funktionieren weiterhin. Informationen finden Sie in folgendem Blogbeitrag: [Wichtige Änderung für Überwachungsrichtlinien für Gastkonfigurationen](https://techcommunity.microsoft.com/t5/azure-governance-and-management/important-change-released-for-guest-configuration-audit-policies/ba-p/1655316)

### <a name="what-is-a-guest-assignment"></a>Was ist eine Gastzuweisung?

Wenn eine Azure Policy-Richtlinie zugewiesen ist, enthält die Kategorie „Gastkonfiguration“ Metadaten, die eine Gastzuweisung beschreiben. Sie können sich eine Gastzuweisung als Verknüpfung zwischen einem Computer und einem Azure Policy-Szenario vorstellen. Beispielsweise ordnet der folgende Codeausschnitt allen Computern im Geltungsbereich der Richtlinie die Azure Windows-Baselinekonfiguration mit der Mindestversion `1.0.0` zu.
Standardmäßig wird von der Gastzuweisung nur eine Überwachung des Computers durchgeführt.

```json
"metadata": {
    "category": "Guest Configuration",
    "guestConfiguration": {
        "name": "AzureWindowsBaseline",
        "version": "1.*"
    }
//additional metadata properties exist
```

Gastzuweisungen werden vom Gastkonfigurationsdienst automatisch pro Computer erstellt. Der Ressourcentyp lautet `Microsoft.GuestConfiguration/guestConfigurationAssignments`. Azure Policy verwendet die **complianceStatus**-Eigenschaft der Gastzuweisungsressource, um den Konformitätsstatus zu melden. Weitere Informationen finden Sie unter [Abrufen von Konformitätsdaten](../how-to/get-compliance-data.md).

#### <a name="auditing-operating-system-settings-following-industry-baselines"></a>Überwachen von Betriebssystemeinstellungen anhand von Branchenrichtlinien

Eine Initiative in Azure Policy überwacht Betriebssystemeinstellungen anhand einer „Baseline“. Die Definition _\[Vorschau\]: Windows-Computer müssen die Anforderungen für die Azure-Sicherheitsbaseline erfüllen_ enthält einen Satz von Regeln, die auf der Active Directory-Gruppenrichtlinie basieren.

Die meisten Einstellungen sind als Parameter verfügbar. Mithilfe von Parametern können Sie anpassen, was überwacht werden soll.
Passen Sie die Richtlinie an Ihre Anforderungen an, oder ordnen Sie die Richtlinie Drittanbieterinformationen (z. B. branchenspezifischen Standards) zu.

Einige Parameter unterstützen einen ganzzahligen Wertebereich. Beispielsweise könnte mit der Einstellung „Maximales Kennwortalter“ die effektive „Gruppenrichtlinie“-Einstellung überwacht werden. Der Bereich „1,70“ würde sicherstellen, dass Benutzer ihre Kennwörter mindestens alle 70 Tage, aber nicht früher als nach einem Tag ändern müssen.

Wenn Sie die Richtlinie mithilfe einer Azure Resource Manager-Vorlage (ARM-Vorlage) zuweisen, verwenden Sie eine Parameterdatei, um Ausnahmen zu verwalten. Checken Sie die Dateien in ein Versionskontrollsystem ein, z. B. Git. Kommentare zu Dateiänderungen geben Aufschluss darüber, warum eine Zuweisung eine Ausnahme vom erwarteten Wert darstellt.

#### <a name="applying-configurations-using-guest-configuration"></a>Anwenden von Konfigurationen mithilfe der Gastkonfiguration

Nur mit der Definition _Konfigurieren der Zeitzone auf Windows-Computern_ werden durch Konfigurieren der Zeitzone Änderungen am Computer vorgenommen. Benutzerdefinierte Richtliniendefinitionen zum Konfigurieren von Einstellungen auf Computern werden nicht unterstützt.

Beim Zuweisen von Definitionen, die mit _Konfigurieren_ beginnen, müssen Sie auch die Definition _Bereitstellen von Voraussetzungen, um die Gastkonfigurationsrichtlinie auf Windows-VMs zu aktivieren_ zuweisen. Sie können diese Definitionen auf Wunsch in einer Initiative kombinieren.

> [!NOTE]
> Die integrierte Zeitzonenrichtlinie ist die einzige Definition, die das Konfigurieren von Einstellungen auf Computern unterstützt. Benutzerdefinierte Richtliniendefinitionen zum Konfigurieren von Einstellungen auf Computern werden nicht unterstützt.

#### <a name="assigning-policies-to-machines-outside-of-azure"></a>Zuweisen von Richtlinien zu Computern außerhalb von Azure

Die für die Gastkonfiguration verfügbaren Überwachungsrichtliniendefinitionen umfassen den Ressourcentyp **Microsoft.HybridCompute/machines**. Alle Computer, die in [Azure Arc für Server](../../../azure-arc/servers/overview.md) integriert sind und sich im Geltungsbereich der Richtlinienzuweisung befinden, werden automatisch eingeschlossen.

## <a name="availability"></a>Verfügbarkeit

Kunden, die eine hochverfügbare Lösung entwerfen, sollten die Anforderungen an die Redundanzplanung für [virtuelle Computer](../../../virtual-machines/availability.md) berücksichtigen, da Gastzuweisungen Erweiterungen von Computerressourcen in Azure sind. Wenn Gastzuweisungsressourcen in einer Azure-Region bereitgestellt werden, die [gekoppelt](../../../best-practices-availability-paired-regions.md) ist, sind Gastzuweisungsberichte verfügbar, solange mindestens eine Region des Paars verfügbar ist. Wenn die Azure-Region nicht gekoppelt und nicht mehr verfügbar ist, ist es erst nach der Wiederherstellung der Region möglich, auf Berichte für eine Gastzuweisung zuzugreifen.

Wenn Sie eine Architektur für hochverfügbare Anwendungen in Betracht ziehen, insbesondere wenn virtuelle Computer in [Verfügbarkeitsgruppen](../../../virtual-machines/availability.md#availability-sets) hinter einer Lastenausgleichslösung bereitgestellt werden, um Hochverfügbarkeit zu gewährleisten, wird empfohlen, allen Computern in der Lösung dieselben Richtliniendefinitionen mit denselben Parametern zu zuweisen. Wenn möglich, würde eine einzelne Richtlinienzuweisung, die alle Computer umfasst, den geringsten Verwaltungsaufwand bieten.

Stellen Sie für Computer, die von [Azure Site Recovery](../../../site-recovery/site-recovery-overview.md) geschützt werden, sicher, dass die Computer an einem sekundären Standort innerhalb des Bereichs von Azure Policy-Zuweisungen für dieselben Definitionen liegen, die dieselben Parameterwerte wie Computer am primären Standort verwenden.

## <a name="data-residency"></a>Datenresidenz

Die Gastkonfiguration speichert/verarbeitet Kundendaten. Standardmäßig werden Kundendaten in die [gekoppelte Region](../../../best-practices-availability-paired-regions.md) repliziert.
Für eine einzelne gebietsansässige Region werden alle Kundendaten in der Region gespeichert und verarbeitet.

## <a name="troubleshooting-guest-configuration"></a>Problembehandlung bei der Gastkonfiguration

Weitere Informationen zur Problembehandlung bei der Gastkonfiguration finden Sie unter [Problembehandlung mit Azure Policy](../troubleshoot/general.md).

### <a name="multiple-assignments"></a>Mehrere Zuweisungen

Aufgrund der Richtliniendefinitionen für die Gastkonfiguration kann die gleiche Gastzuweisung derzeit lediglich einmal pro Computer zugewiesen werden, auch wenn bei der Richtlinienzuweisung andere Parameter verwendet werden.

### <a name="client-log-files"></a>Protokolldateien des Clients

Die Guest Configuration-Erweiterung schreibt Protokolldateien an die folgenden Speicherorte:

Windows: `C:\ProgramData\GuestConfig\gc_agent_logs\gc_agent.log`

Linux

- Azure-VM: `/var/lib/GuestConfig/gc_agent_logs/gc_agent.log`
- Azure-VM: `/var/lib/GuestConfig/arc_policy_logs/gc_agent.log`

### <a name="collecting-logs-remotely"></a>Remotesammeln von Protokollen

Im ersten Schritt bei der Problembehandlung von Konfigurationen oder Modulen durch die Gastkonfiguration sollte das Cmdlet `Test-GuestConfigurationPackage` anhand der Schritte unter [Erstellen eines benutzerdefinierten Richtlinienpakets für Gastkonfigurationen](../how-to/guest-configuration-create.md#step-by-step-creating-a-custom-guest-configuration-audit-policy-for-windows) ausgeführt werden.
Wenn dies nicht erfolgreich ist, kann das Sammeln von Clientprotokollen helfen, Probleme zu diagnostizieren.

#### <a name="windows"></a>Windows

Erfassen Sie Informationen aus Protokolldateien mithilfe des Azure-VM-Befehls [Ausführen](../../../virtual-machines/windows/run-command.md), wobei das folgende PowerShell-Beispiel hilfreich sein kann.

```powershell
$linesToIncludeBeforeMatch = 0
$linesToIncludeAfterMatch = 10
$logPath = 'C:\ProgramData\GuestConfig\gc_agent_logs\gc_agent.log'
Select-String -Path $logPath -pattern 'DSCEngine','DSCManagedEngine' -CaseSensitive -Context $linesToIncludeBeforeMatch,$linesToIncludeAfterMatch | Select-Object -Last 10
```

#### <a name="linux"></a>Linux

Erfassen Sie Informationen aus Protokolldateien mithilfe des Azure VM-Befehls [Ausführen](../../../virtual-machines/linux/run-command.md), wobei das folgende Bash-Beispiel hilfreich sein kann.

```bash
linesToIncludeBeforeMatch=0
linesToIncludeAfterMatch=10
logPath=/var/lib/GuestConfig/gc_agent_logs/gc_agent.log
egrep -B $linesToIncludeBeforeMatch -A $linesToIncludeAfterMatch 'DSCEngine|DSCManagedEngine' $logPath | tail
```

### <a name="client-files"></a>Clientdateien

Der Gastkonfigurationsclient lädt Inhaltspakete auf einen Computer herunter und extrahiert den Inhalt.
Um zu überprüfen, welche Inhalte heruntergeladen und gespeichert wurden, sehen Sie sich die unten angegebenen Ordnerspeicherorte an.

Windows: `c:\programdata\guestconfig\configuration`

Linux: `/var/lib/GuestConfig/Configuration`

## <a name="guest-configuration-samples"></a>Beispiele für Guest Configuration

Beispiele für integrierte Gastkonfigurationsrichtlinien finden Sie unter:

- [Integrierte Richtliniendefinitionen: Gastkonfiguration](../samples/built-in-policies.md#guest-configuration)
- [Integrierte Initiativen: Gastkonfiguration](../samples/built-in-initiatives.md#guest-configuration)
- [Azure Policy-Beispiele: GitHub-Repository](https://github.com/Azure/azure-policy/tree/master/built-in-policies/policySetDefinitions/Guest%20Configuration)

### <a name="video-overview"></a>Videoübersicht

Die folgende Übersicht über die Azure Policy-Gastkonfiguration stammt aus den ITOps Talks 2021.

[Governing baselines in hybrid server environments using Azure Policy Guest Configuration](https://techcommunity.microsoft.com/t5/itops-talk-blog/ops114-governing-baselines-in-hybrid-server-environments-using/ba-p/2109245)(Steuern von Baselines in Hybridserverumgebungen mit der Azure Policy-Gastkonfiguration, in englischer Sprache)

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie die Details der einzelnen Einstellungen in der [Kompatibilitätsansicht der Gastkonfiguration](../how-to/determine-non-compliance.md#compliance-details-for-guest-configuration) anzeigen.
- Sehen Sie sich die Beispiele unter [Azure Policy-Beispiele](../samples/index.md) an.
- Lesen Sie die Informationen unter [Struktur von Azure Policy-Definitionen](./definition-structure.md).
- Lesen Sie [Grundlegendes zu Richtlinienauswirkungen](./effects.md).
- Informieren Sie sich über das [programmgesteuerte Erstellen von Richtlinien](../how-to/programmatically-create.md).
- Informieren Sie sich über das [Abrufen von Konformitätsdaten](../how-to/get-compliance-data.md).
- Erfahren Sie, wie Sie [nicht konforme Ressourcen korrigieren](../how-to/remediate-resources.md) können.
- Weitere Informationen zu Verwaltungsgruppen finden Sie unter [Organisieren Ihrer Ressourcen mit Azure-Verwaltungsgruppen](../../management-groups/overview.md).
