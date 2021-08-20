---
title: Das Azure Defender-Dashboard und seine Features
description: Erfahren Sie mehr über die Features des Azure Defender-Dashboards.
author: memildin
ms.author: memildin
ms.date: 9/22/2020
ms.topic: how-to
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: df7af81b51428f53beb7722c7f224dc7afdd28cb
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122354996"
---
# <a name="the-azure-defender-dashboard"></a>Das Azure Defender-Dashboard

Das Azure Defender-Dashboards bietet:

- Einblicke in die Abdeckung von Azure Defender für Ihre verschiedenen Ressourcentypen
- Links zum Konfigurieren der Advanced Threat Protection-Funktionen
- Onboardingstatus und Agent-Installation
- Warnungen zur netzwerkbasierten Bedrohung von Azure Defender 

Um auf das Azure Defender-Dashboard zuzugreifen, wählen Sie im Menü von Security Center im Abschnitt zur Cloudsicherheit **Azure Defender** aus.

## <a name="whats-shown-on-the-dashboard"></a>Was wird auf dem Dashboard angezeigt?

:::image type="content" source="./media/azure-defender-dashboard/sample-defender-dashboard-numbered.png" alt-text="Beispiel für das Azure Defender-Dashboard" lightbox="./media/azure-defender-dashboard/sample-defender-dashboard-numbered.png":::

Das Dashboard enthält die folgenden Abschnitte:

1. **Azure Defender-Abdeckung:** Hier sehen Sie die Ressourcentypen, die in Ihrem Abonnement verfügbar und für den Schutz durch Azure Defender geeignet sind. Sofern dies relevant ist, können Sie hier auch ein Upgrade durchführen. Wenn Sie alle möglichen berechtigten Ressourcen aktualisieren möchten, wählen Sie **Alle aktualisieren** aus.

2. **Sicherheitswarnungen:** Wenn Azure Defender in einem der Bereiche Ihrer Umgebung eine Bedrohung erkennt, wird eine Warnung generiert. Diese Warnungen beschreiben Details zu den betroffenen Ressourcen, empfohlene Problembehandlungsschritte sowie in einigen Fällen eine Option, mit der eine Logik-App als Reaktion ausgelöst werden kann. Durch Auswählen einer beliebigen Stelle in diesem Diagramm wird die Seite **Sicherheitswarnungen** geöffnet.

3. **Erweiterter Schutz:** Azure Defender bietet viele Advanced Threat Protection-Funktionen für virtuelle Computer, SQL-Datenbanken, Container, Webanwendungen, Ihr Netzwerk u. v. m. In diesem Abschnitt zum erweiterten Schutz können Sie den Status der Ressourcen in den ausgewählten Abonnements für jede dieser Schutzmaßnahmen einsehen. Wählen Sie eine beliebige Option aus, um direkt zum Konfigurationsbereich für diesen Schutztyp zu wechseln.

4. **Erkenntnisse:** Dieser rollierende Bereich von Nachrichten, empfohlener Lektüre und Warnungen mit hoher Priorität stellt die Erkenntnisse von Security Center zu dringenden Sicherheitsangelegenheiten bereit, die für Sie und Ihr Abonnement relevant sind. Unabhängig davon, ob es sich um eine Liste von CVEs mit hohem Schweregrad handelt, die von einem Tool zur Sicherheitsrisikoanalyse auf Ihren VMs erkannt wurden, oder um einen neuen Blogbeitrag von einem Mitglied des Security Center-Teams, es wird hier im Bereich „Erkenntnisse“ des **Azure Defender-Dashboards** angezeigt.




## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie etwas über das Azure Defender-Dashboard erfahren. 

> [!div class="nextstepaction"]
> [Aktivieren von Azure Defender](enable-azure-defender.md)

Weitere Informationen zu Azure Defender finden Sie unter [Einführung in Azure Defender](azure-defender.md).