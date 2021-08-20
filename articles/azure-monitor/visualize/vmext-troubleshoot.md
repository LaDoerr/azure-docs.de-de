---
title: Problembehandlung für die VM-Erweiterung von Azure Log Analytics
description: Beschrieben werden die Symptome, Ursachen und Lösungen für die häufigsten Probleme, die bei der Log Analytics-Erweiterung für virtuelle Windows und Linux Azure-Computer auftreten.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 06/06/2019
ms.openlocfilehash: 80cec6ad233a6f6dc4771e82a98601f5db774b0f
ms.sourcegitcommit: 2d412ea97cad0a2f66c434794429ea80da9d65aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2021
ms.locfileid: "122342894"
---
# <a name="troubleshooting-the-log-analytics-vm-extension-in-azure-monitor"></a>Problembehandlung für die Log Analytics-VM-Erweiterung in Azure Monitor
Dieser Artikel enthält Hilfe zur Problembehandlung von Fehlern, die im Zusammenhang mit der Log Analytics-VM-Erweiterung für virtuelle Windows- und Linux-Computer mit Microsoft Azure auftreten können, sowie Vorschläge für mögliche Problemlösungen.

Um den Status der Erweiterung zu überprüfen, führen Sie im Azure-Portal die folgenden Schritte aus.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie im Azure-Portal auf **Alle Dienste**. Geben Sie in der Ressourcenliste **Virtuelle Computer** ein. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Wählen Sie **Virtuelle Computer** aus.
3. Suchen Sie in der Liste der virtuellen Computer nach dem entsprechenden Computer, und wählen Sie ihn aus.
3. Klicken Sie auf dem virtuellen Computer auf **Erweiterungen**.
4. Überprüfen Sie in der Liste, ob die Log Analytics-Erweiterung aktiviert oder deaktiviert ist.  Bei Linux wird der Agent als **OMSAgentforLinux** und bei Windows als **MicrosoftMonitoringAgent** aufgeführt.

   ![Ansicht der VM-Erweiterung](./media/vmext-troubleshoot/log-analytics-vmview-extensions.png)

4. Klicken Sie auf die Erweiterung, um Details anzuzeigen. 

   ![Details der VM-Erweiterung](./media/vmext-troubleshoot/log-analytics-vmview-extensiondetails.png)

## <a name="troubleshooting-azure-windows-vm-extension"></a>Problembehandlung für die Azure Windows-VM-Erweiterung

Wenn die VM-Erweiterung *MicrosoftMonitoringAgent* nicht installiert ist oder keine Berichte erstellt, können Sie die folgenden Schritte ausführen, um das Problem zu beheben.

1. Überprüfen Sie, ob der Azure-VM-Agent installiert ist und ordnungsgemäß funktioniert, indem Sie die Schritte unter [KB 2965986](https://support.microsoft.com/kb/2965986#mt1) ausführen.
   * Sie können auch die Protokolldatei `C:\WindowsAzure\logs\WaAppAgent.log` des VM-Agents überprüfen.
   * Wenn das Protokoll nicht vorhanden ist, wurde der VM-Agent nicht installiert.
   * [Installieren des Azure-VM-Agents](../vm/monitor-virtual-machine.md#agents)
2. Überprüfen Sie die Protokolldateien der Microsoft Monitoring Agent-VM-Erweiterung in `C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent`.
3. Stellen Sie sicher, dass der virtuelle Computer PowerShell-Skripts ausführen kann.
4. Vergewissern Sie sich, dass die Berechtigungen für „C:\Windows\temp“ nicht geändert wurden.
5. Zeigen Sie den Status des Microsoft Monitoring Agents an, indem Sie Folgendes in einem PowerShell-Fenster mit erhöhten Rechten auf dem virtuellen Computer eingeben: `(New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg').GetCloudWorkspaces() | Format-List`.
6. Überprüfen Sie die Setupprotokolldateien des Microsoft Monitoring Agents in `C:\Windows\System32\config\systemprofile\AppData\Local\SCOM\Logs`.

Weitere Informationen finden Sie unter [Behandeln von Problemen bei Windows-Erweiterungen](../../virtual-machines/extensions/oms-windows.md).

## <a name="troubleshooting-linux-vm-extension"></a>Problembehandlung für die Linux-VM-Erweiterung
[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)] 
Wenn die VM-Erweiterung *Log Analytics-Agent für Linux* nicht installiert ist oder keine Berichte erstellt, können Sie die folgenden Schritte ausführen, um das Problem zu beheben.

1. Wenn der Status der Erweiterung *Unbekannt* lautet, überprüfen Sie, ob der Azure-VM-Agent installiert ist und ordnungsgemäß funktioniert. Dazu überprüfen Sie die Protokolldatei des VM-Agents: `/var/log/waagent.log`.
   * Wenn das Protokoll nicht vorhanden ist, wurde der VM-Agent nicht installiert.
   * [Installieren des Azure-VM-Agents auf Linux-VMs](../../virtual-machines/extensions/agent-linux.md#installation)
2. Bei einem anderen fehlerhaften Status sollten Sie die Protokolldateien der VM-Erweiterung Log Analytics-Agent für Linux in `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/extension.log` und `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/CommandExecution.log` überprüfen.
3. Wenn der Status der Erweiterung fehlerfrei ist, aber keine Daten hochgeladen werden, sollten Sie Protokolldateien des Log Analytics-Agents für Linux in `/var/opt/microsoft/omsagent/log/omsagent.log` überprüfen.

Weitere Informationen finden Sie unter [Behandeln von Problemen bei Linux-Erweiterungen](../../virtual-machines/extensions/oms-linux.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Anleitungen zur Fehlerbehebung im Zusammenhang mit dem Log Analytics-Agent für Linux, der auf Computern außerhalb von Azure gehostet wird, finden Sie unter [Problembehandlung für den Linux-Agent bei Azure Log Analytics](../agents/agent-linux-troubleshoot.md).