---
title: 'Azure Virtual Desktop-Diagnose (klassisch): Protokollanalyse – Azure'
description: Hier erfahren Sie, wie Sie die Protokollanalyse mit der Azure Virtual Desktop-Diagnosefunktion (klassisch) verwenden.
author: Heidilohr
ms.topic: how-to
ms.date: 03/30/2020
ms.author: helohr
manager: femila
ms.openlocfilehash: 5c1bdadf92d72ebec7ffa122b3c49c5878e2d2b8
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111745167"
---
# <a name="use-log-analytics-for-the-diagnostics-feature-in-azure-virtual-desktop-classic"></a>Verwenden von Log Analytics für das Diagnosefeature in Azure Virtual Desktop (klassisch)

>[!IMPORTANT]
>Dieser Inhalt gilt für Azure Virtual Desktop (klassisch). Der Dienst unterstützt keine Azure Virtual Desktop-Objekte in Azure Resource Manager. Wenn Sie Azure Virtual Desktop-Objekte in Azure Resource Manager verwalten möchten, helfen Ihnen die Informationen in [diesem Artikel](../diagnostics-log-analytics.md) weiter.

Azure Virtual Desktop verfügt über eine Diagnosefunktion, mit der der Administrator Probleme über eine zentrale Benutzeroberfläche identifizieren kann. Diese Funktion protokolliert Diagnoseinformationen, wenn jemand mit Azure Virtual Desktop-Rolle den Dienst verwendet. Neben Mandanten- und Benutzerinformationen enthält jedes Protokoll Informationen darüber, welche Azure Virtual Desktop-Rolle an der Aktivität beteiligt war und welche Fehlermeldungen ggf. während der Sitzung angezeigt wurden. Die Diagnosefunktion erstellt Aktivitätsprotokolle für Benutzer- und Administratoraktionen. Jedes Aktivitätsprotokoll lässt sich einer von drei Hauptkategorien zuordnen:

- Feedabonnementaktivitäten: Ein Benutzer versucht, über Microsoft-Remotedesktopanwendungen eine Verbindung mit seinem Feed herzustellen.
- Verbindungsaktivitäten: Ein Benutzer versucht, über Microsoft-Remotedesktopanwendungen eine Verbindung mit einem Desktop oder einer RemoteApp-Instanz herzustellen.
- Verwaltungsaktivitäten: Ein Administrator führt Verwaltungsvorgänge für das System durch, indem er beispielsweise Hostpools erstellt, App-Gruppen Benutzer zuweist oder Rollenzuweisungen erstellt.

Verbindungen, die Azure Virtual Desktop nicht erreichen, werden nicht in den Diagnoseergebnissen angezeigt, da der Diagnoserollendienst selbst Teil von Azure Virtual Desktop ist. Azure Virtual Desktop-Verbindungsprobleme können auftreten, wenn beim Benutzer Probleme mit der Netzwerkkonnektivität auftreten.

## <a name="why-you-should-use-log-analytics"></a>Gründe für die Verwendung von Log Analytics

Es empfiehlt sich, Log Analytics zum Analysieren von Diagnosedaten im Azure-Client zu verwenden, die über die Problembehandlung für einzelne Benutzer hinausgehen. Da Sie VM-Leistungsindikatoren in Log Analytics pullen können, steht Ihnen ein zentrales Tool zum Sammeln von Informationen für Ihre Bereitstellung zur Verfügung.

## <a name="before-you-get-started"></a>Bevor Sie beginnen

Um Log Analytics mit der Diagnosefunktion verwenden zu können, müssen Sie zunächst [einen Arbeitsbereich erstellen](../../azure-monitor/vm/quick-collect-windows-computer.md#create-a-workspace).

Gehen Sie anschließend wie unter [Verbinden von Windows-Computern mit Azure Monitor](../../azure-monitor/agents/log-analytics-agent.md#workspace-id-and-key) beschrieben vor, um die folgenden Informationen zu erhalten:

- Arbeitsbereich-ID
- Primärschlüssel Ihres Arbeitsbereichs

Diese Informationen werden später bei der Einrichtung benötigt.

## <a name="push-diagnostics-data-to-your-workspace"></a>Pushen von Diagnosedaten an Ihren Arbeitsbereich

Diagnosedaten können von Ihrem Azure Virtual Desktop-Mandanten an die Log Analytics-Instanz für Ihren Arbeitsbereich gepusht werden. Diese Funktion kann direkt bei der Erstellung Ihres Mandanten eingerichtet werden, indem Sie Ihren Arbeitsbereich mit Ihrem Mandanten verknüpfen. Sie kann aber auch später mit einem bereits vorhandenen Mandanten eingerichtet werden.

Wenn Sie Ihren Mandanten im Zuge der Einrichtung Ihres neuen Mandanten mit Ihrem Log Analytics-Arbeitsbereich verknüpfen möchten, führen Sie das folgende Cmdlet aus, um sich mit Ihrem TenantCreator-Benutzerkonto bei Azure Virtual Desktop anzumelden:

```powershell
Add-RdsAccount -DeploymentUrl https://rdbroker.wvd.microsoft.com
```

Wenn Sie anstelle eines neuen Mandanten einen vorhandenen Mandanten verknüpfen möchten, führen Sie stattdessen das folgende Cmdlet aus:

```powershell
Set-RdsTenant -Name <TenantName> -AzureSubscriptionId <SubscriptionID> -LogAnalyticsWorkspaceId <String> -LogAnalyticsPrimaryKey <String>
```

Diese Cmdlets müssen für jeden Mandanten ausgeführt werden, den Sie mit Log Analytics verknüpfen möchten.

>[!NOTE]
>Falls Sie den Log Analytics-Arbeitsbereich nicht bei der Mandantenerstellung verknüpfen möchten, führen Sie stattdessen das Cmdlet `New-RdsTenant` aus.

## <a name="cadence-for-sending-diagnostic-events"></a>Intervall für das Senden von Diagnoseereignissen

Diagnoseereignisse werden nach Abschluss an Log Analytics gesendet.

## <a name="example-queries"></a>Beispielabfragen

Die folgenden Beispielabfragen veranschaulichen, wie die Diagnosefunktion einen Bericht für die häufigsten Aktivitäten in Ihrem System generiert:

Das erste Beispiel zeigt Verbindungsaktivitäten, die von Benutzern mit unterstützten Remotedesktopclients initiiert wurden:

```powershell
WVDActivityV1_CL

| where Type_s == "Connection"

| join kind=leftouter (

    WVDErrorV1_CL

    | summarize Errors = makelist(pack('Time', Time_t, 'Code', ErrorCode_s , 'CodeSymbolic', ErrorCodeSymbolic_s, 'Message', ErrorMessage_s, 'ReportedBy', ReportedBy_s , 'Internal', ErrorInternal_s )) by ActivityId_g

    ) on $left.Id_g  == $right.ActivityId_g 

| join  kind=leftouter (

    WVDCheckpointV1_CL

    | summarize Checkpoints = makelist(pack('Time', Time_t, 'ReportedBy', ReportedBy_s, 'Name', Name_s, 'Parameters', Parameters_s) ) by ActivityId_g

    ) on $left.Id_g  == $right.ActivityId_g

|project-away ActivityId_g, ActivityId_g1
```

Die nächste Beispielabfrage zeigt mandantenbezogene Verwaltungsaktivitäten von Administratoren:

```powershell
WVDActivityV1_CL

| where Type_s == "Management"

| join kind=leftouter (

    WVDErrorV1_CL

    | summarize Errors = makelist(pack('Time', Time_t, 'Code', ErrorCode_s , 'CodeSymbolic', ErrorCodeSymbolic_s, 'Message', ErrorMessage_s, 'ReportedBy', ReportedBy_s , 'Internal', ErrorInternal_s )) by ActivityId_g

    ) on $left.Id_g  == $right.ActivityId_g 

| join  kind=leftouter (

    WVDCheckpointV1_CL

    | summarize Checkpoints = makelist(pack('Time', Time_t, 'ReportedBy', ReportedBy_s, 'Name', Name_s, 'Parameters', Parameters_s) ) by ActivityId_g

    ) on $left.Id_g  == $right.ActivityId_g

|project-away ActivityId_g, ActivityId_g1
```

## <a name="stop-sending-data-to-log-analytics"></a>Beenden des Sendens von Daten an Log Analytics

Wenn von einem vorhandenen Mandanten keine Daten mehr an Log Analytics gesendet werden sollen, führen Sie das folgende Cmdlet aus, und legen Sie leere Zeichenfolgen fest:

```powershell
Set-RdsTenant -Name <TenantName> -AzureSubscriptionId <SubscriptionID> -LogAnalyticsWorkspaceId <String> -LogAnalyticsPrimaryKey <String>
```

Dieses Cmdlet muss für jeden Mandanten ausgeführt werden, von dem keine Daten mehr gesendet werden sollen.

## <a name="next-steps"></a>Nächste Schritte

Informationen zu allgemeinen Fehlerszenarien, die von der Diagnosefunktion identifiziert werden können, finden Sie unter [Identifizieren und Diagnostizieren von Problemen](diagnostics-role-service-2019.md#common-error-scenarios).
