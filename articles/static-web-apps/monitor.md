---
title: Überwachen von Azure Static Web Apps
description: Überwachen von Anforderungen, Fehlern und Ablaufverfolgungsinformationen in Azure Static Web Apps
services: static-web-apps
author: craigshoemaker
ms.service: static-web-apps
ms.topic: conceptual
ms.date: 4/23/2021
ms.author: cshoe
ms.openlocfilehash: 8c97c3c008dda4269b282e89af7badda889588fe
ms.sourcegitcommit: bb9a6c6e9e07e6011bb6c386003573db5c1a4810
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110497571"
---
# <a name="monitor-azure-static-web-apps"></a>Überwachen von Azure Static Web Apps

Aktivieren Sie [Application Insights](../azure-monitor/app/app-insights-overview.md) zum Überwachen von API-Anforderungen, Fehlern und Ablaufverfolgungsinformationen.

> [!IMPORTANT]
> Application Insights verfügt über ein von Azure Static Web Apps [unabhängiges Preismodell](https://azure.microsoft.com/pricing/details/monitor).

> [!NOTE]
> Zur Verwendung von Application Insights mit Azure Static Web Apps ist eine Anwendung mit einer [API](./add-api.md) erforderlich.

## <a name="add-monitoring"></a>Hinzufügen von Code zum Überwachen der App

Führen Sie die folgenden Schritte aus, um die Application Insights-Überwachung in einer statischen Web-App hinzuzufügen.

1. Öffnen Sie die Static Web Apps-Instanz im Azure-Portal.

1. Wählen Sie im Menü die Option **Application Insights** aus.

1. Wählen Sie **Ja** neben _Application Insights aktivieren_ aus.

1. Wählen Sie **Speichern**.

:::image type="content" source="media/monitoring/azure-static-web-apps-application-insights-add.png" alt-text="Hinzufügen von Application Insights in Azure Static Web Apps":::

> [!NOTE]
> Nach dem Erstellen der Application Insights-Instanz wird in der Azure Static Web Apps-Instanz eine zugeordnete Anwendungseinstellung erstellt, mit der die Dienste miteinander verknüpft werden.

## <a name="access-data"></a>Zugreifen auf Daten

1. Wählen Sie im Fenster _Übersicht_ in der statischen Web-App den Link neben der _Ressourcengruppe_ aus.

1. Wählen Sie in der Liste die Application Insights-Instanz aus, deren Präfix dem Namen der statischen Web-App entspricht.

Im Folgenden sind einige Menüpositionen im Portal zum Überprüfen von Aspekten der API-Endpunkte der Anwendung aufgeführt.

> [!NOTE]
> Ausführlichere Informationen zur Verwendung von Application Insights finden Sie unter [Wo finde ich meine Telemetriedaten?](../azure-monitor/app/app-insights-overview.md#where-do-i-see-my-telemetry).

| type | Menü | BESCHREIBUNG |
|--- | --- | --- |
| [Fehler](../azure-monitor/app/asp-net-exceptions.md) | _Untersuchen > Fehler_ | Überprüfen fehlerhafter Anforderungen |
| [Serveranforderungen](../azure-monitor/app/tutorial-performance.md) | _Untersuchen > Leistung_ | Überprüfen einzelner API-Anforderungen  |
| [Protokolle](../azure-monitor/app/diagnostic-search.md) | _Überwachung > Protokolle_ | Interagieren in einem Editor zum Abfragen von Transaktionsprotokollen |
| [Metriken](../azure-monitor/essentials/app-insights-metrics.md) | _Überwachung > Metriken_ | Interagieren in einem Designer zum Erstellen von benutzerdefinierten Diagrammen mithilfe verschiedener Metriken |

### <a name="traces"></a>Traces

Mit den folgenden Schritten können Sie Ablaufverfolgungen in Ihrer Anwendung anzeigen.

1. Wählen Sie unter **Überwachung** die Option _Protokolle_ aus.

1. Zeigen Sie mit der Maus auf eine Karte im Fenster _Abfragen_.

1. Wählen Sie die Schaltfläche **In Editor laden** aus.

1. Ersetzen Sie die generierte Abfrage durch das Wort `traces`.

1. Wählen Sie die Schaltfläche **Ausführen**.

:::image type="content" source="media/monitoring/azure-static-web-apps-application-insights-traces.png" alt-text="Anzeigen von Application Insights-Ablaufverfolgungen":::

## <a name="limit-logging"></a>Begrenzen der Protokollierung

In einigen Fällen können Sie die Protokollierung begrenzen und gleichzeitig weiterhin Details zu Fehlern und Warnungen erfassen, indem Sie die folgenden Änderungen an Ihrer Datei _host.json_ der Azure Functions-App vornehmen.

```json
{
    "version": "2.0",
    "logging": {
        "applicationInsights": {
            "samplingSettings": {
              "isEnabled": true
            },
            "enableDependencyTracking": false
        },
        "logLevels": {
            "default": "Warning"
        }
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Einrichten der Authentifizierung und Autorisierung](authentication-authorization.md)
