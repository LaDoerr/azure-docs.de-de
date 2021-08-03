---
title: Übersicht über die Azure-Überwachungs-Agents | Microsoft-Dokumentation
description: Dieser Artikel bietet eine ausführliche Übersicht über die verfügbaren Azure-Agents, die die Überwachung virtueller Computer unterstützen, die in Azure oder einer Hybridumgebung gehostet werden.
services: azure-monitor
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 01/12/2021
ms.openlocfilehash: a7b2e6ee4b54427f33ba9701ae13f139341a1941
ms.sourcegitcommit: c05e595b9f2dbe78e657fed2eb75c8fe511610e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112030951"
---
# <a name="overview-of-azure-monitor-agents"></a>Übersicht über Azure Monitor-Agents

Virtuelle Computer und andere Computeressourcen benötigen einen Agent, um Überwachungsdaten zu erfassen, die für die Messung der Leistung und Verfügbarkeit ihres Gastbetriebssystems und ihrer Workloads erforderlich sind. In diesem Artikel werden die von Azure Monitor verwendeten Agents beschrieben. Außerdem bietet er Hilfe bei der Entscheidung, welche Agents Sie zum Erfüllen der Anforderungen für Ihre bestimmte Umgebung benötigen.

> [!NOTE]
> Aufgrund der kürzlich vorgenommenen Konsolidierung von Azure Monitor und Log Analytics verfügt Azure Monitor über mehrere Agents. Die Features überschneiden sich zwar unter Umständen, jeder hat jedoch seine eigenen Funktionen. Je nach Anforderungen benötigen Sie auf Ihren Computern möglicherweise einen oder mehrere der Agents. 

Es kann vorkommen, dass spezifische Anforderungen für einen bestimmten Computer nicht vollständig durch einen einzelnen Agent erfüllt werden können. Dies wäre etwa der Fall, wenn Sie Metrikwarnungen verwenden möchten, für die die Azure-Diagnoseerweiterung erforderlich ist, gleichzeitig aber auch die Funktionen von VM Insights nutzen möchten, für die der Log Analytics-Agent und der Dependency-Agent benötigt werden. In solchen Fällen können Sie mehrere Agents verwenden. Dies ist ein gängiges Szenario für Kunden, die Funktionen beider Agents benötigen.

## <a name="summary-of-agents"></a>Übersicht über die Agents

Die folgenden Tabellen enthalten eine kurze Gegenüberstellung der Azure Monitor-Agents für Windows und Linux. Weitere Details zu den jeweiligen Agents finden Sie im Abschnitt weiter unten.

> [!NOTE]
> Der Azure Monitor-Agent ist derzeit als Vorschauversion mit eingeschränkten Funktionen verfügbar. Diese Tabelle wird aktualisiert. 

### <a name="windows-agents"></a>Windows-Agents

| | Azure Monitor-Agent (Vorschau) | Diagnose<br>erweiterung (WAD) | Log Analytics<br>Agent | Abhängigkeit<br>Agent |
|:---|:---|:---|:---|:---|
| **Unterstützte Umgebungen** | Azure<br>Andere Cloud (Azure Arc)<br>Lokal (Azure Arc)  | Azure | Azure<br>Andere Cloud<br>Lokal | Azure<br>Andere Cloud<br>Lokal | 
| **Agent-Anforderungen**  | Keine | Keine | Keine | Erfordert Log Analytics-Agent |
| **Gesammelte Daten** | Ereignisprotokolle<br>Leistung | Ereignisprotokolle<br>ETW-Ereignisse<br>Leistung<br>Dateibasierte Protokolle<br>IIS-Protokolle<br>.NET-App-Protokolle<br>Absturzabbilder<br>Agent-Diagnoseprotokolle | Ereignisprotokolle<br>Leistung<br>Dateibasierte Protokolle<br>IIS-Protokolle<br>Erkenntnisse und Lösungen<br>Sonstige Dienste | Prozessabhängigkeiten<br>Netzwerkverbindungsmetriken |
| **Senden von Daten an** | Azure Monitor-Protokolle<br>Azure Monitor-Metriken | Azure Storage<br>Azure Monitor-Metriken<br>Event Hub | Azure Monitor-Protokolle | Azure Monitor-Protokolle<br>(über den Log Analytics-Agent) |
| **Dienste und**<br>**features**<br>**Unterstützt** | Log Analytics<br>Metrik-Explorer | Metrik-Explorer | VM Insights<br>Log Analytics<br>Azure Automation<br>Azure Security Center<br>Azure Sentinel | VM Insights<br>Dienstzuordnung |

### <a name="linux-agents"></a>Linux-Agents

| | Azure Monitor-Agent (Vorschau) | Diagnose<br>erweiterung (LAD) | Telegraf-<br>Agent | Log Analytics<br>Agent | Abhängigkeit<br>Agent |
|:---|:---|:---|:---|:---|:---|
| **Unterstützte Umgebungen** (unterstützte Betriebssysteme finden Sie in der folgenden Tabelle) | Azure<br>Andere Cloud (Azure Arc)<br>Lokal (Azure Arc) | Azure | Azure<br>Andere Cloud<br>Lokal | Azure<br>Andere Cloud<br>Lokal | Azure<br>Andere Cloud<br>Lokal |
| **Agent-Anforderungen**  | Keine | Keine | Keine | Keine | Erfordert Log Analytics-Agent |
| **Gesammelte Daten** | syslog<br>Leistung | syslog<br>Leistung | Leistung | syslog<br>Leistung| Prozessabhängigkeiten<br>Netzwerkverbindungsmetriken |
| **Senden von Daten an** | Azure Monitor-Protokolle<br>Azure Monitor-Metriken | Azure Storage<br>Event Hub | Azure Monitor-Metriken | Azure Monitor-Protokolle | Azure Monitor-Protokolle<br>(über den Log Analytics-Agent) |
| **Dienste und**<br>**features**<br>**Unterstützt** | Log Analytics<br>Metrik-Explorer | | Metrik-Explorer | VM Insights<br>Log Analytics<br>Azure Automation<br>Azure Security Center<br>Azure Sentinel | VM Insights<br>Dienstzuordnung |


## <a name="azure-monitor-agent-preview"></a>Azure Monitor-Agent (Vorschau)

Der [Azure Monitor-Agent](azure-monitor-agent-overview.md) befindet sich derzeit in der Vorschau und wird den Log Analytics-Agent und den Telegraf-Agent sowohl für Windows- als auch für Linux-Computer ersetzen. Er kann Daten an Azure Monitor-Protokolle und Azure Monitor-Metriken senden und nutzt [Data Collection Rules (DCR)](data-collection-rule-overview.md), die eine skalierbare Methode zum Konfigurieren der Datensammlung und der Ziele für jeden Agent bereitstellen.

Verwenden Sie den Azure Monitor-Agent zu folgenden Zwecken:

- Sammeln von Gastprotokollen und Metriken von einem beliebigen Computer in Azure, in anderen Clouds oder in lokalen Systemen. (Für Computer außerhalb von Azure sind [Azure Arc-fähige Server](../../azure-arc/servers/overview.md) erforderlich.) 
- Senden von Daten an Azure Monitor-Protokolle und Azure Monitor- Metriken zur Analyse mit Azure Monitor. 
- Senden von Daten an Azure Storage zur Archivierung.
- Senden von Daten an Drittanbietertools mithilfe von [Azure Event Hubs](./diagnostics-extension-stream-event-hubs.md)
- Verwalten der Sicherheit Ihrer Computer mithilfe von [Azure Security Center](../../security-center/security-center-introduction.md) oder [Azure Sentinel](../../sentinel/overview.md). (In der Vorschau nicht verfügbar.)

Einschränkungen des Azure Monitor-Agents:

- Zurzeit als Public Preview verfügbar. Eine Liste der Einschränkungen während der öffentlichen Vorschau finden Sie unter [Aktuelle Einschränkungen](azure-monitor-agent-overview.md#current-limitations).

## <a name="log-analytics-agent"></a>Log Analytics-Agent

Der [Log Analytics-Agent](./log-analytics-agent.md) erfasst Überwachungsdaten vom Gastbetriebssystem und Workloads von virtuellen Computern in Azure, anderen Cloudanbietern und lokalen Computern. Er sendet Daten an einen Log Analytics-Arbeitsbereich. Der Log Analytics-Agent ist der gleiche Agent, der auch von System Center Operations Manager verwendet wird, und Sie können Multihoming-Agent-Computer verwenden, um gleichzeitig mit Ihrer Verwaltungsgruppe und Azure Monitor zu kommunizieren. Dieser Agent wird auch für bestimmte Erkenntnisse in Azure Monitor und anderen Diensten benötigt.

> [!NOTE]
> Der Log Analytics-Agent für Windows wird häufig als Microsoft Monitoring Agent (MMA) bezeichnet. Der Log Analytics-Agent für Linux wird häufig als OMS-Agent bezeichnet.

Verwenden Sie den Log Analytics-Agent für Folgendes:

* Erfassen von Protokollen und Leistungsdaten von Azure-VMs oder Hybridcomputern, die außerhalb von Azure gehostet werden.
* Senden von Überwachungsdaten an einen Log Analytics-Arbeitsbereich, um die von [Azure Monitor-Protokollen](../logs/data-platform-logs.md) unterstützten Features wie etwa [Protokollabfragen](../logs/log-query-overview.md) zu nutzen
* Verwenden von [VM Insights](../vm/vminsights-overview.md) zur Überwachung Ihrer Computer im großen Stil sowie zur Überwachung der zugehörigen Prozesse und Abhängigkeiten von anderen Ressourcen und externen Prozessen.  
* Verwalten der Sicherheit Ihrer Computer mithilfe von [Azure Security Center](../../security-center/security-center-introduction.md) oder [Azure Sentinel](../../sentinel/overview.md).
* Verwenden von [Azure Automation-Updateverwaltung](../../automation/update-management/overview.md), [Azure Automation State Configuration](../../automation/automation-dsc-overview.md) oder [Azure Automation für Änderungsnachverfolgung und Bestand](../../automation/change-tracking/overview.md) zur umfassenden Verwaltung Ihrer Azure- und Nicht-Azure-Computer.
* Verwenden verschiedener [Lösungen](../monitor-reference.md#insights-and-core-solutions) zur Überwachung eines bestimmten Diensts oder einer bestimmten Anwendung

Einschränkungen des Log Analytics-Agents:

- Kann keine Daten an Azure Monitor-Metriken, an Azure Storage oder an Azure Event Hubs senden
- Das Konfigurieren von eindeutigen Überwachungsdefinitionen für einzelne Agents ist schwierig.
- Die Skalierung im großen Stil ist schwierig, da jeder virtuelle Computer über eine eindeutige Konfiguration verfügt.

## <a name="azure-diagnostics-extension"></a>Azure-Diagnoseerweiterung

Die [Azure-Diagnoseerweiterung](./diagnostics-extension-overview.md) erfasst Überwachungsdaten vom Gastbetriebssystem sowie von Workloads auf virtuellen Azure-Computern und anderen Computeressourcen. Die Daten werden in erster Linie in Azure Storage erfasst. Es können aber auch Datensenken definiert werden, um Daten an andere Ziele zu senden (etwa an Azure Monitor-Metriken oder an Azure Event Hubs).

Verwenden Sie die Azure-Diagnoseerweiterung für Folgendes:

- Senden von Daten an Azure Storage, um sie zu archivieren oder mit Tools wie dem [Azure Storage-Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md) zu analysieren
- Senden von Daten an [Azure Monitor-Metriken](../essentials/data-platform-metrics.md), um sie mit dem [Metrik-Explorer](../essentials/metrics-getting-started.md) analysieren und Features wie [Metrikwarnungen](../alerts/alerts-metric-overview.md) in Quasi-Echtzeit und [automatische Skalierung](../autoscale/autoscale-overview.md) (nur unter Windows) nutzen zu können
- Senden von Daten an Drittanbietertools mithilfe von [Azure Event Hubs](./diagnostics-extension-stream-event-hubs.md)
- Erfassen von [Startdiagnosedaten](/troubleshoot/azure/virtual-machines/boot-diagnostics) zur Untersuchung von VM-Startfehlern

Einschränkungen der Azure-Diagnoseerweiterung:

- Kann nur mit Azure-Ressourcen verwendet werden
- Senden von Daten an Azure Monitor-Protokolle nur eingeschränkt möglich

## <a name="telegraf-agent"></a>Telegraf-Agent

Der [InfluxData Telegraf-Agent](../essentials/collect-custom-metrics-linux-telegraf.md) wird verwendet, um Leistungsdaten von Linux-Computern in Azure Monitor-Metriken zu erfassen.

Verwenden Sie den Telegraf-Agent für Folgendes:

* Senden von Daten an [Azure Monitor-Metriken](../essentials/data-platform-metrics.md), um sie mit dem [Metrik-Explorer](../essentials/metrics-getting-started.md) analysieren und Features wie [Metrikwarnungen](../alerts/alerts-metric-overview.md) in Quasi-Echtzeit und [automatische Skalierung](../autoscale/autoscale-overview.md) (nur unter Linux) nutzen zu können

## <a name="dependency-agent"></a>Abhängigkeits-Agent

Der Dependency-Agent sammelt ermittelte Daten über Prozesse, die auf dem Computer ausgeführt werden, und über Abhängigkeiten von externen Prozessen. 

Verwenden Sie den Dependency-Agent für Folgendes:

* Nutzung des Zuordnungsfeatures [VM Insights](../vm/vminsights-overview.md) oder der [Dienstzuordnung](../vm/service-map.md)

Beachten Sie bei der Verwendung des Dependency-Agents Folgendes:

- Für den Dependency-Agent muss der Log Analytics-Agent auf demselben Computer installiert sein.
- Auf Linux-Computern muss der Log Analytics-Agent vor der Azure-Diagnoseerweiterung installiert werden.

## <a name="virtual-machine-extensions"></a>VM-Erweiterungen

Durch die Log Analytics-Erweiterung für [Windows](../../virtual-machines/extensions/oms-windows.md) und [Linux](../../virtual-machines/extensions/oms-linux.md) wird der Log Analytics-Agent auf virtuellen Azure-Computern installiert. Durch die Azure Monitor-Dependency-Erweiterung für [Windows](../../virtual-machines/extensions/agent-dependency-windows.md) und [Linux](../../virtual-machines/extensions/agent-dependency-linux.md) wird der Dependency-Agent auf virtuellen Azure-Computern installiert. Hierbei handelt es sich um die oben beschriebenen Agents, allerdings mit der Möglichkeit, sie über [VM-Erweiterungen](../../virtual-machines/extensions/overview.md) zu verwalten. Verwenden Sie zum Installieren und Verwalten der Agents möglichst immer Erweiterungen.

Verwenden Sie für Hybridcomputer [Azure Arc-fähige Server](../../azure-arc/servers/manage-vm-extensions.md), um die Log Analytics- und Azure Monitor-Dependency-Erweiterungen für VMs bereitzustellen.

## <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme

In der folgenden Tabelle sind die Betriebssysteme aufgeführt, die von den Azure Monitor-Agents unterstützt werden. Informationen zu speziellen Aspekten sowie zum Installationsvorgang finden Sie in der Dokumentation für den jeweiligen Agent. Sehen Sie in der Telegraf-Dokumentation nach, um mehr über die unterstützten Betriebssysteme zu erfahren. Bei allen Betriebssystemen wird von einer x64-Architektur ausgegangen. x86 wird für kein Betriebssystem unterstützt.

### <a name="windows"></a>Windows

| Betriebssystem | Azure Monitor-Agent | Log Analytics-Agent | Abhängigkeits-Agent | Diagnoseerweiterung | 
|:---|:---:|:---:|:---:|:---:|
| Windows Server 2019                                      | X | X | X | X |
| Windows Server 2019 Core                                 | X |   |   |   |
| Windows Server 2016                                      | X | X | X | X |
| Windows Server 2016 Core                                 | X |   |   | X |
| Windows Server 2012 R2                                   | X | X | X | X |
| Windows Server 2012                                      | X | X | X | X |
| Windows Server 2008 R2 SP1                               | X | X | X | X |
| Windows Server 2008 R2                                   |   | X | X | X |
| Windows 10 Enterprise<br>(einschließlich Multisession) und Pro<br>(nur Serverszenarien)  | X | X | X | X |
| Windows 8 Enterprise und Pro<br>(nur Serverszenarien)  |   | X | X |   |
| Windows 7 SP1<br>(nur Serverszenarien)                 |   | X | X |   |

### <a name="linux"></a>Linux

| Betriebssystem | Azure Monitor-Agent<sup>1</sup> | Log Analytics-Agent<sup>1</sup> | Abhängigkeits-Agent | Diagnoseerweiterung<sup>2</sup>| 
|:---|:---:|:---:|:---:|:---:
| Amazon Linux 2017.09                                        |   | X |   |   |
| CentOS Linux 8                                              | X<sup>3</sup> | X | X |   |
| CentOS Linux 7                                              | X | X | X | X |
| CentOS Linux 6                                              |   | X |   |   |
| CentOS Linux 6.5+                                           |   | X | X | X |
| Debian 10 <sup>1</sup>                                      | X |   |   |   |
| Debian 9                                                    | X | X | w | X |
| Debian 8                                                    |   | X | X |   |
| Debian 7                                                    |   |   |   | X |
| OpenSUSE 13.1+                                              |   |   |   | X |
| Oracle Linux 8                                              | X<sup>3</sup> | X |   |   |
| Oracle Linux 7                                              | X | X |   | X |
| Oracle Linux 6                                              |   | X |   |   |
| Oracle Linux 6.4+                                           |   | X |   | X |
| Red Hat Enterprise Linux Server 8.3                         | X<sup>3</sup> | X | X |   |
| Red Hat Enterprise Linux Server 8                           | X<sup>3</sup> | X | X |   |
| Red Hat Enterprise Linux Server 7                           | X | X | X | X |
| Red Hat Enterprise Linux Server 6                           |   | X | X |   |
| Red Hat Enterprise Linux Server 6.7+                        |   | X | X | X |
| SUSE Linux Enterprise Server 15.2                           | X<sup>3</sup> |   |   |   |
| SUSE Linux Enterprise Server 15.1                           | X<sup>3</sup> | X |   |   |
| SUSE Linux Enterprise Server 15 SP1                         | X | X | X |   |
| SUSE Linux Enterprise Server 15                             | X | X | X |   |
| SUSE Linux Enterprise Server 12 SP5                         | X | X | X | X |
| SUSE Linux Enterprise Server 12                             | X | X | X | X |
| Ubuntu 20.04 LTS                                            | X | X | X |   |
| Ubuntu 18.04 LTS                                            | X | X | X | X |
| Ubuntu 16.04 LTS                                            | X | X | X | X |
| Ubuntu 14.04 LTS                                            |   | X |   | X |

<sup>1</sup> Erfordert die Installation von Python (2 oder 3) auf dem Computer.

<sup>3</sup> Bekanntes Problem beim Sammeln von Syslog-Ereignissen in Versionen vor 1.9.0
#### <a name="dependency-agent-linux-kernel-support"></a>Unterstützung des Linux-Kernels für den Dependency-Agent

Da der Dependency-Agent auf der Kernelebene arbeitet, ist die Unterstützung auch von der Kernelversion abhängig. In der folgenden Tabelle sind die Haupt- und Nebenversionen des Linux-Betriebssystems sowie die unterstützten Kernelversionen für den Dependency-Agent aufgeführt:

| Distribution | Betriebssystemversion | Kernelversion |
|:---|:---|:---|
|  Red Hat Linux 8   | 8,2     | 4.18.0-193.\*el8_2.x86_64 |
|                    | 8.1     | 4.18.0-147.\*el8_1.x86_64 |
|                    | 8.0     | 4.18.0-80.\*el8.x86_64<br>4.18.0-80.\*el8_0.x86_64 |
|  Red Hat Linux 7   | 7.9     | 3.10.0-1160 |
|                    | 7,8     | 3.10.0-1136 |
|                    | 7,7     | 3.10.0-1062 |
|                    | 7.6     | 3.10.0-957  |
|                    | 7,5     | 3.10.0-862  |
|                    | 7.4     | 3.10.0-693  |
| Red Hat Linux 6    | 6.10    | 2.6.32-754 |
|                    | 6.9     | 2.6.32-696  |
| CentOS Linux 8     | 8,2     | 4.18.0-193.\*el8_2.x86_64 |
|                    | 8.1     | 4.18.0-147.\*el8_1.x86_64 |
|                    | 8.0     | 4.18.0-80.\*el8.x86_64<br>4.18.0-80.\*el8_0.x86_64 |
| CentOS Linux 7     | 7.9     | 3.10.0-1160 |
|                    | 7,8     | 3.10.0-1136 |
|                    | 7,7     | 3.10.0-1062 |
| CentOS Linux 6     | 6.10    | 2.6.32-754.3.5<br>2.6.32-696.30.1 |
|                    | 6.9     | 2.6.32-696.30.1<br>2.6.32-696.18.7 |
| Ubuntu Server      | 20.04   | 5.4\* |
|                    | 18,04   | 5.3.0-1020<br>5.0 (enthält optimierten Azure-Kernel)<br>4.18 *<br>4.15* |
|                    | 16.04.3 | 4.15.\* |
|                    | 16.04   | 4.13.\*<br>4.11.\*<br>4.10.\*<br>4.8.\*<br>4.4.\* |
| SUSE Linux 12 Enterprise Server | 15     | 4.12.14-150\*
|                                 | 12 SP4 | 4.12.* (enthält optimierten Azure-Kernel) |
|                                 | 12 SP3 | 4.4.* |
|                                 | 12 SP2 | 4.4.* |
| Debian                          | 9      | 4,9  | 

## <a name="next-steps"></a>Nächste Schritte

Weitere Details zu den einzelnen Agents finden Sie hier:

- [Übersicht über den Log Analytics-Agent](./log-analytics-agent.md)
- [Übersicht zur Azure-Diagnoseerweiterung](./diagnostics-extension-overview.md)
- [Erfassen von benutzerdefinierten Metriken für einen virtuellen Linux-Computer mit dem InfluxData Telegraf-Agent](../essentials/collect-custom-metrics-linux-telegraf.md)
