---
title: Automatisierung der Vorfallbehandlung in Azure Sentinel | Microsoft Docs
description: In diesem Artikel erfahren Sie, wie Sie mithilfe von Automatisierungsregeln die Behandlung von Vorfällen automatisieren können, um die Effizienz und Effektivität Ihres SOC bei der Reaktion auf Sicherheitsbedrohungen zu maximieren.
services: sentinel
cloud: na
documentationcenter: na
author: yelevin
manager: rkarlin
ms.assetid: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/14/2021
ms.author: yelevin
ms.openlocfilehash: 869693765463589c3e94aef9a1cee17867117c5d
ms.sourcegitcommit: 8651d19fca8c5f709cbb22bfcbe2fd4a1c8e429f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2021
ms.locfileid: "112072671"
---
# <a name="automate-incident-handling-in-azure-sentinel-with-automation-rules"></a>Automatisierung der Vorfallbehandlung in Azure Sentinel mit Automatisierungsregeln

In diesem Artikel erfahren Sie, was Azure Sentinel Automatisierungsregeln sind und wie Sie sie für die Implementierung Ihrer Security Orchestration, Automation and Response (SOAR) nutzen können, um die Effektivität Ihres SOCs zu erhöhen und Zeit und Ressourcen zu sparen.

> [!IMPORTANT]
>
> - Das Feature **Automatisierungsregeln** befindet sich derzeit in der **VORSCHAUPHASE**. Die [zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) enthalten zusätzliche rechtliche Bedingungen, die für Azure-Features gelten, die sich in der Beta- oder Vorschauversion befinden bzw. anderweitig noch nicht zur allgemeinen Verfügbarkeit freigegeben sind.

## <a name="what-are-automation-rules"></a>Was sind Automatisierungsregeln?

Automatisierungsregeln sind ein neues Konzept in Azure Sentinel. Diese Funktion ermöglicht es Benutzern, die Automatisierung der Behandlung von Vorfällen zentral zu verwalten. Mit Automatisierungsregeln können Sie nicht nur Playbooks Incidents zuweisen (nicht nur Alerts wie bisher), sondern auch Antworten für mehrere Analyseregeln gleichzeitig automatisieren, Incidents automatisch kennzeichnen, zuweisen oder schließen, ohne dass Playbooks erforderlich sind, und die Reihenfolge der ausgeführten Aktionen steuern. Automatisierungsregeln optimieren den Einsatz von Automatisierungen in Azure Sentinel und ermöglichen es Ihnen, komplexe Workflows für Ihre Incident-Orchestrierungsprozesse zu vereinfachen.

## <a name="components"></a>Komponenten

Automatisierungsregeln bestehen aus mehreren Komponenten:

### <a name="trigger"></a>Trigger

Automatisierungsregeln werden durch die Erstellung eines Incidents ausgelöst. 

Zu überprüfen: Vorfälle werden durch Analyseregeln aus Alarmen erstellt, von denen es mehrere Typen gibt, wie im Tutorial [Erkennen von Bedrohungen mit integrierten Analyseregeln in Azure Sentinel](tutorial-detect-threats-built-in.md) erläutert.

### <a name="conditions"></a>Bedingungen

Es können komplexe Bedingungssätze definiert werden, die bestimmen, wann Aktionen (siehe unten) ausgeführt werden sollen. Diese Bedingungen basieren in der Regel auf den Zuständen oder Werten von Attributen von Incidents und deren Entitäten, und Sie können `AND` / `OR` / `NOT` / `CONTAINS` Operatoren einschließen.

### <a name="actions"></a>Aktionen

Es können Aktionen definiert werden, die ausgeführt werden, wenn die Bedingungen (siehe oben) erfüllt sind. Sie können viele Aktionen in einer Regel definieren, und Sie können die Reihenfolge wählen, in der sie ausgeführt werden (siehe unten). Die folgenden Aktionen können mithilfe von Automatisierungsregeln definiert werden, ohne dass die [erweiterte Funktionalität eines Playbooks](automate-responses-with-playbooks.md)erforderlich ist:

- Ändern des Status eines Incidents, sodass der Workflow auf dem neuesten Stand bleibt.

  - Wenn Sie zu „geschlossen“ wechseln, wird der [Schließungsgrund](tutorial-investigate-cases.md#closing-an-incident) angegeben und ein Kommentar hinzugefügt. Dies hilft Ihnen dabei, ihre Leistung und Effektivität zu überwachen und eine Feinabstimmung durchführen, um [False Positives](false-positives.md) zu reduzieren.

- Ändern des Schweregrades eines Incidents – basierend auf dem Vorhandensein, der Abwesenheit, den Werten oder den Attributen der am Incident beteiligten Entitäten können Sie eine Neubewertung und Neupriorisierung vornehmen.

- Zuweisung eines Vorfalls an einen Besitzer – dies hilft Ihnen, Vorfallstypen an die Mitarbeiter weiterzuleiten, die am besten geeignet sind, sie zu bearbeiten, oder aber an das am schnellsten verfügbare Personal.

- Hinzufügen eines Tags zu einem Incident – dies ist hilfreich bei der Klassifizierung von Incidents nach Betreff, Angreifer oder nach einem anderen gemeinsamen Nenner.

Außerdem können Sie eine Aktion zum [**Ausführen eines Playbooks**](tutorial-respond-threats-playbook.md) definieren, um komplexere Antwortaktionen zu erstellen, die auch externe Systeme involvieren können. **Nur** durch den [**Incidenttrigger**](automate-responses-with-playbooks.md#azure-logic-apps-basic-concepts) aktivierte Playbooks sind für Automatisierungsregeln verfügbar. Sie können eine Aktion so definieren, dass sie mehrere Playbooks oder Kombinationen von Playbooks und anderen Aktionen umfasst, sowie die Reihenfolge, in der sie ausgeführt werden sollen.

### <a name="expiration-date"></a>Ablaufdatum

Sie können ein Ablaufdatum für eine Automatisierungsregel definieren. Die Regel wird nach diesem Datum deaktiviert. Dies ist nützlich für die Behandlung (d. h. das Schließen) von "Rausch"-Incidents, die durch geplante, zeitlich begrenzte Aktivitäten wie Penetrationstests verursacht werden.

### <a name="order"></a>Auftrag

Sie können die Reihenfolge definieren, in der die Automatisierungsregeln ausgeführt werden. Spätere Automatisierungsregeln bewerten die Bedingungen des Incidents entsprechend seines Zustands, nachdem sie von vorherigen Automatisierungsregeln beeinflusst wurden.

Wenn z. B. die „Erste Automatisierungsregel“ den Schweregrad eines Vorfalls von „Mittel“ auf „Niedrig“ gesetzt hat und die "Zweite Automatisierungsregel" so definiert ist, dass sie nur bei Vorfällen mit mittlerem oder höherem Schweregrad ausgeführt wird, wird sie bei diesem Vorfall nicht ausgeführt.

## <a name="common-use-cases-and-scenarios"></a>Gängige Anwendungsfälle und -szenarien

### <a name="incident-triggered-automation"></a>Vom Incident ausgelöste Automatisierung

Bis jetzt können nur Warnungen eine automatisierte Antwort durch die Verwendung von Playbooks auslösen. Mit Automatisierungsregeln können Incidents jetzt automatisierte Reaktionsketten auslösen, die neue Incident-ausgelöste Playbooks enthalten können ([spezielle Berechtigungen sind erforderlich](#permissions-for-automation-rules-to-run-playbooks)), wenn ein Incident erstellt wird. 

### <a name="trigger-playbooks-for-microsoft-providers"></a>Playbooks für Microsoft-Anbieter auslösen

Automatisierungsregeln bieten eine Möglichkeit, die Behandlung von Microsoft-Sicherheitswarnungen zu automatisieren, indem diese Regeln auf Incidents angewendet werden, die aus den Warnmeldungen erstellt wurden. Die Automatisierungsregeln können Playbooks aufrufen ([spezielle Berechtigungen sind erforderlich](#permissions-for-automation-rules-to-run-playbooks)) und die Vorfälle mit allen Details, einschließlich Alarmen und Entitäten, an diese übergeben. IIm Allgemeinen empfehlen die Best Practices von Azure Sentinel die Verwendung der Warteschlange für Incidents als zentralen Punkt der Sicherheitsoperationen.

Microsoft-Sicherheitswarnungen umfassen Folgendes:

- Microsoft Cloud App Security (MCAS)
- Azure AD Identity Protection
- Azure Defender (ASC)
- Defender für IoT (vormals ASC für IoT)
- Microsoft Defender für Office 365 (vormals Office 365 ATP)
- Microsoft Defender für den Endpunkt (vormals MDATP)
- Microsoft Defender für die Identität (vormals Azure ATP)

### <a name="multiple-sequenced-playbooksactions-in-a-single-rule"></a>Mehrere sequenzierte Playbooks/Aktionen in einer einzigen Regel

Sie haben jetzt die Möglichkeit, die Ausführungsreihenfolge von Aktionen und Playbooks in einer einzigen Automatisierungsregel nahezu vollständig zu steuern. Außerdem steuern Sie die Reihenfolge der Ausführung der Automatisierungsregeln selbst. So können Sie Ihre Playbooks stark vereinfachen, indem Sie sie auf eine einzelne Aufgabe oder eine kleine, überschaubare Abfolge von Aufgaben reduzieren und diese kleinen Playbooks in verschiedenen Kombinationen in unterschiedlichen Automatisierungsregeln kombinieren.

### <a name="assign-one-playbook-to-multiple-analytics-rules-at-once"></a>Gleichzeitiges Zuweisen eines Playbooks zu mehreren Analyse-Regeln

Wenn Sie eine Aufgabe haben, die Sie für alle Ihre Analyseregeln automatisieren möchten, z. B. die Erstellung eines Support-Tickets in einem externen Ticketing-System, können Sie ein einziges Playbook auf alle Ihre Analyseregeln – einschließlich aller zukünftigen Regeln – in einem Zug anwenden. Das macht einfache, aber immer wiederkehrende Wartungs- und Housekeeping-Aufgaben deutlich weniger mühsam.

### <a name="automatic-assignment-of-incidents"></a>Automatische Zuweisung von Incidents

Sie können Incidents automatisch dem richtigen Besitzer zuweisen. Wenn es in Ihrem SOC einen Analysten gibt, der auf eine bestimmte Plattform spezialisiert ist, können alle Incidents, die sich auf diese Plattform beziehen, automatisch diesem Analysten zugewiesen werden.

### <a name="incident-suppression"></a>Incidentunterdrückung anwenden

Sie können Regeln zur automatischen Behebung von Incidents mit bekannten falsch/gutartigen Positivmeldungen ohne Verwendung von Playbooks anwenden. Bei der Durchführung von Penetrationstests, geplanten Wartungen oder Upgrades oder beim Testen von Automatisierungsverfahren können beispielsweise viele falsch-positive Vorfälle erzeugt werden, die das SOC ignorieren möchte. Eine zeitlich begrenzte Automatisierungsregel kann diese Vorfälle automatisch schließen, wenn sie erstellt werden, und sie gleichzeitig mit einem Deskriptor für die Ursache ihrer Erstellung versehen.

### <a name="time-limited-automation"></a>Zeitlich begrenzte Automatisierung

Sie können Ihren Automatisierungsregeln Ablaufdaten hinzufügen. Es kann auch andere Fälle als die Störungsunterdrückung geben, die eine zeitlich begrenzte Automatisierung erfordern. Vielleicht möchten Sie einen bestimmten Incident-Typ einem bestimmten Benutzer (z. B. einem Praktikanten oder einem Berater) für einen bestimmten Zeitraum zuweisen. Wenn der Zeitrahmen im Voraus bekannt ist, können Sie effektiv bewirken, dass die Regel am Ende ihrer Relevanz deaktiviert wird, ohne dass Sie daran denken müssen.

### <a name="automatically-tag-incidents"></a>Vorfälle automatisch taggen

Sie können automatisch Freitext-Tags zu Ereignissen hinzufügen, um sie nach beliebigen Kriterien zu gruppieren oder zu klassifizieren.

## <a name="automation-rules-execution"></a>Ausführung von Automatisierungsregeln

Automatisierungsregeln werden sequenziell ausgeführt, entsprechend der von Ihnen festgelegten Reihenfolge. Jede Automatisierungsregel wird ausgeführt, nachdem die vorhergehende ihre Ausführung beendet hat. Innerhalb einer Automatisierungsregel werden alle Aktionen nacheinander in der Reihenfolge ausgeführt, in der sie definiert sind.

Bei Playbook-Aktionen gibt es eine Verzögerung von zwei Minuten zwischen dem Anfang der Playbook-Aktion und der nächsten Aktion in der Liste.

### <a name="permissions-for-automation-rules-to-run-playbooks"></a>Berechtigungen für Automatisierungsregeln zum Ausführen von Playbooks

Wenn eine Azure Sentinel Automatisierungsregel ein Playbook ausführt, verwendet sie ein spezielles Azure Sentinel Dienstkonto, das speziell für diese Aktion autorisiert ist. Die Verwendung dieses Kontos (im Gegensatz zu Ihrem Benutzerkonto) erhöht die Sicherheitsstufe des Diensts.

Damit eine Automatisierungsregel ein Playbook ausführen kann, muss diesem Konto explizit die Berechtigung für die Ressourcengruppe erteilt werden, in der sich das Playbook befindet. An diesem Punkt kann jede Automatisierungsregel jedes beliebige Playbook in dieser Ressourcengruppe ausführen.

Wenn Sie eine Automatisierungsregel konfigurieren und eine Aktion **Playbook ausführen** hinzufügen, wird eine Dropdown Liste mit Playbooks angezeigt. Playbooks, in denen Azure Sentinel nicht über Berechtigungen verfügt, werden als nicht verfügbar ("ausgeblendet") angezeigt. Sie können Azure Sentinel Berechtigungen für die Ressourcengruppen der Playbooks direkt erteilen, indem Sie den Link **Playbook Berechtigungen** verwalten auswählen.

#### <a name="permissions-in-a-multi-tenant-architecture"></a>Berechtigungen in einer Multi-Tenant-Architektur

Automatisierungsregeln unterstützen arbeitsplatzübergreifende und [mehrinstanzfähige Bereitstellungen](extend-sentinel-across-workspaces-tenants.md#managing-workspaces-across-tenants-using-azure-lighthouse) vollständig (im Falle von mehrinstanzfähigen-Bereitstellungen unter Verwendung von [Azure Lighthouse](../lighthouse/index.yml)).

Wenn Ihre Azure Sentinel Bereitstellung eine mehrinstanzfähige Architektur aufweist, können Sie daher eine Automatisierungsregel in einem Mandanten ein Playbook ausführen lassen, das sich in einem anderen Mandanten befindet, aber die Berechtigungen für Sentinel zur Ausführung der Playbooks müssen in dem Mandanten definiert werden, in dem sich die Playbooks befinden, nicht in dem Mandanten, in dem die Automatisierungsregeln definiert sind.

Im speziellen Fall eines Dienstanbieters für verwaltete Sicherheit (Managed Security Service Provider, MSSP), bei dem ein Dienstanbieter-Mandant einen Azure Sentinel-Arbeitsbereich in einem Kunden-Mandanten verwaltet, gibt es zwei wichtige Szenarien:

- **Eine Automatisierungsregel, die im Kunden-Mandanten erstellt wurde, ist so konfiguriert, dass ein Playbook ausgeführt wird, das sich im Mandanten des Dienstanbieters befindet.** 

    Dieser Ansatz dient normalerweise zum Schutz geistigen Eigentums im Playbook. Dies ist ohne Weiteres umsetzbar. Wenn Sie eine Playbookaktion in Ihrer Automatisierungsregel definieren und die Phase erreichen, in der Sie Azure Sentinel-Berechtigungen für die Ressourcengruppe erteilen, in der sich das Playbook befindet (über das Panel **Playbookberechtigungen verwalten**), sehen Sie die zum Mandanten des Dienstanbieters gehörenden Ressourcengruppen unter den wählbaren Ressourcengruppen. [Sehen Sie sich hier eine Beschreibung des gesamten Ablaufs an.](tutorial-respond-threats-playbook.md#respond-to-incidents)

- **Eine Automatisierungsregel, die (angemeldet beim Mandanten des Dienstanbieters) im Arbeitsbereich des Kunden erstellt wurde, ist so konfiguriert, dass ein Playbook ausgeführt wird, das sich im Mandanten des Kunden befindet.**

    Diese Konfiguration wird verwendet, wenn keine Notwendigkeit besteht, geistiges Eigentum zu schützen. Damit dieses Szenario funktioniert, benötigt Azure Sentinel Berechtigungen zum Ausführen des Playbooks in ***beiden Mandanten** _. Im Kunden-Mandanten gewähren Sie diese im Panel _ *Playbookberechtigungen verwalten**, genau wie im obigen Szenario. Um die relevanten Berechtigungen im Mandanten des Dienstanbieters zu erteilen, müssen Sie der Ressourcengruppe, in der sich das Playbook befindet, eine zusätzliche Azure Lighthouse-Delegierung hinzufügen, die der **Azure Security Insights**-App mit der Rolle **Mitwirkender für Azure Sentinel-Automatisierung** Zugriffsrechte erteilt.

    Das Szenario sieht folgendermaßen aus:

    :::image type="content" source="./media/automate-incident-handling-with-automation-rules/automation-rule-multi-tenant.png" alt-text="Regelarchitektur der mehrinstanzfähigen Automatisierung":::

    Informationen zum Einrichten finden Sie in [unseren Anweisungen](tutorial-respond-threats-playbook.md#permissions-to-run-playbooks).

## <a name="creating-and-managing-automation-rules"></a>Erstellen und Verwalten von Automatisierungsregeln

Sie können Automatisierungsregeln von verschiedenen Punkten in der Azure Sentinel Umgebung aus erstellen und verwalten, abhängig von Ihrem speziellen Bedarf und Anwendungsfall.

- **Automatisierung**

    Automatisierungsregeln können im neuen Blade **Automatisierung** (das das Blade **Playbooks** ersetzt) auf der Registerkarte **Automatisierungsregeln** zentral verwaltet werden. (Sie können jetzt auch Playbooks in diesem Blade unter der Registerkarte **Playbooks** verwalten.) Von dort aus können Sie neue Automatisierungsregeln erstellen und die vorhandenen Regeln bearbeiten. Sie können auch Automatisierungsregeln verschieben, um die Reihenfolge der Ausführung zu ändern, und sie aktivieren oder deaktivieren.

    Im **Automatisierung-** Blade werden alle Regeln angezeigt, die im Arbeitsbereich definiert sind, zusammen mit Ihrem Status (aktiviert/deaktiviert) und den Analyseregeln, auf die Sie angewendet werden.

    Wenn Sie eine Automatisierungsregel benötigen, die auf viele Analyseregeln angewendet wird, erstellen Sie Sie direkt im **Automatisierungs-** Blade. Klicken Sie im oberen Menü auf **Erstellen** und **neue Regel hinzufügen**. Dadurch wird der Bereich **neue Automatisierungsregel erstellen** geöffnet. Von hier aus haben Sie die Möglichkeit, die Regel flexibel zu konfigurieren: Sie können Sie auf alle Analyse Regeln (einschließlich zukünftiger) anwenden und die größtmögliche Anzahl von Bedingungen und Aktionen definieren.

- **Analyseregel-Assistent**

    In der Registerkarte **automatisierte Antwort** des Analyseregel-Assistenten können Sie Automatisierungsregeln anzeigen, verwalten und erstellen, die für die bestimmte Analyseregel gelten, die im Assistenten erstellt oder bearbeitet wird.

    Wenn Sie auf  **Erstellen** und einen der Regeltypen (**Regel für geplante Abfragen**  oder  **Regel für die Erstellung von Microsoft Incidents**) aus dem oberen Menü im **Analytics** Blade klicken, oder wenn Sie eine vorhandene Analyseregel auswählen und auf **Bearbeiten** klicken, wird der Regelassistent geöffnet. Wenn Sie die Registerkarte **automatisierte Antwort** auswählen, wird ein Bereich mit der Bezeichnung **Incident Automatisierung** angezeigt, in dem die Automatisierungsregeln angezeigt werden, die zurzeit für diese Regel gelten. Sie können eine vorhandene Automatisierungsregel auswählen, um Sie zu bearbeiten, oder klicken Sie auf **Neu hinzufügen**, um ein neue zu erstellen.

    Sie werden feststellen, dass beim Erstellen der Automatisierungsregel von hier aus im Bedienfeld **Neue Automatisierungsregel erstellen** die Bedingung für diese **Analyseregel** als nicht verfügbar angezeigt wird, da diese Regel bereits so eingestellt ist, dass sie nur für die Analyseregel gilt, die Sie im Assistenten bearbeiten. Alle anderen Konfigurationsoptionen sind weiterhin verfügbar.

- **Blatt für Incidents**

    Sie können auch eine Automatisierungsregel über das Blatt **Incidents** erstellen, um auf einen einzelnen, wiederkehrenden Incident zu reagieren. Dies ist hilfreich beim Erstellen einer [Unterdrückungsregel](#incident-suppression) zum automatischen Schließen von „Noisy“-Incidents. Wählen Sie einen Incident aus der Warteschlange aus, und klicken Sie im oberen Menü auf **Automatisierungsregel**.

    Sie werden feststellen, dass der Bereich **neue Automatisierungsregel erstellen** alle Felder mit Werten aus dem Incident aufgefüllt hat. Sie benennt die Regel mit demselben Namen wie den Vorfall, wendet sie auf die Analyseregel an, die den Vorfall erzeugt hat, und verwendet alle verfügbaren Entitäten im Incident als Bedingungen der Regel. Außerdem wird standardmäßig eine Unterdrückungsaktion (Schließen) vorgeschlagen, und es wird ein Ablaufdatum für die Regel vorgeschlagen. Sie können Bedingungen und Aktionen hinzufügen oder entfernen und das Ablaufdatum ändern, wie Sie möchten.

## <a name="auditing-automation-rule-activity"></a>Aktivität Automatisierungsregel überwachen

Sie möchten vielleicht wissen, was mit einem bestimmten Incident passiert ist und was eine bestimmte Automatisierungsregel damit gemacht haben könnte oder auch nicht. Eine vollständige Aufzeichnung der Incident-Chroniken steht Ihnen in der Tabelle *SecurityIncident* im **Logs** Blade zur Verfügung. Verwenden Sie die folgende Abfrage, um alle Aktivitäten Ihrer Automatisierungsregeln zu sehen:

```kusto
SecurityIncident
| where ModifiedBy contains "Automation"
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument haben Sie gelernt, wie Sie mithilfe von Automatisierungsregeln Ihre Azure Sentinel Warteschlange für Incidents verwalten und einige grundlegende Automatisierungen für die Incident-Behandlung implementieren können.

- Weitere Informationen zu den erweiterten Automatisierungsoptionen finden Sie unter [Automatisieren der Bedrohungsabwehr mit Playbooks in Azure Sentinel](automate-responses-with-playbooks.md).
- Hilfe bei der Implementierung von Automatisierungsregeln und Playbooks finden Sie im [Tutorial: Einsatz von Playbooks zur Automatisierung von Bedrohungsreaktionen in Azure Sentinel](tutorial-respond-threats-playbook.md).
