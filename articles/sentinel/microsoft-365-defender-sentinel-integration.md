---
title: Microsoft 365 Defender-Integration in Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Microsoft 365 Defender zusammen mit Azure Sentinel verwenden können, um Azure Sentinel als universelle Queue für Vorfälle zu nutzen und gleichzeitig die Stärken von Microsoft 365 Defender nahtlos für die Untersuchung von Microsoft 365 Sicherheitsvorfällen einzusetzen. Außerdem erfahren Sie, wie Sie erweiterte Hunting-Daten von Defender-Komponenten in Azure Sentinel erfassen.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/21/2021
ms.author: yelevin
ms.openlocfilehash: 86a998e304755085a9ae1e15e011df3b242c4df8
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122345908"
---
# <a name="microsoft-365-defender-integration-with-azure-sentinel"></a>Microsoft 365 Defender-Integration in Azure Sentinel

> [!IMPORTANT]
> Der Microsoft 365 Defender-Connector ist derzeit als **VORSCHAU** verfügbar. Die [zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) enthalten zusätzliche rechtliche Bedingungen, die für Azure-Features gelten, die sich in der Beta- oder Vorschauversion befinden bzw. anderweitig noch nicht zur allgemeinen Verfügbarkeit freigegeben sind.

> [!IMPORTANT]
>
> **Microsoft 365 Defender** wurde bisher als **Microsoft Threat Protection** oder **MTP** bezeichnet.
>
> **Microsoft Defender for Endpoint** wurde bisher als **Microsoft Defender Advanced Threat Protection** oder **MDATP** bezeichnet.
>
> Die alten Namen werden möglicherweise eine Zeit lang weiterhin verwendet.

## <a name="incident-integration"></a>Vorfallsintegration

Die [Microsoft 365 Defender](/microsoft-365/security/mtp/microsoft-threat-protection)-Vorfallintegration von Azure Sentinel ermöglicht es Ihnen, alle Microsoft 365 Defender-Vorfälle in Azure Sentinel zu streamen und sie zwischen beiden Portalen synchron zu bewahren. Vorfälle aus Microsoft 365 Defender (vormals bekannt als Microsoft Threat Protection oder MTP) enthalten alle zugehörigen Warnungen, Entitäten und relevanten Informationen, die Ihnen genügend Kontext bieten, um Triagen und vorläufige Untersuchungen in Azure Sentinel durchzuführen. Sobald sie in Sentinel enthalten sind, bleiben Vorfälle bidirektional mit Microsoft 365 Defender synchronisiert, sodass Sie die Vorteile beider Portale bei Ihrer Vorfallsuntersuchung nutzen können.

Diese Integration ermöglicht Microsoft 365-Sicherheitsvorfälle die Übersicht um von Azure Sentinel aus verwaltet zu werden, als Teil der primären Queue für Vorfälle über die gesamte Organisation, so dass Sie Microsoft 365-Vorfälle zusammen mit denen von all Ihren anderen Cloud- und On-Premises-Systemen sehen - und korrelieren - können. Gleichzeitig können Sie die einzigartigen Stärken und Fähigkeiten von Microsoft 365 Defender für eingehende Untersuchungen und eine Microsoft 365-spezifische Erfahrung im gesamten Microsoft 365-Ökosystem nutzen. Microsoft 365 Defender bereichert und gruppiert Warnmeldungen aus mehreren Microsoft 365-Produkten, wodurch sowohl die Größe der Queue für Vorfälle des SOC reduziert als auch die Zeit zur Behebung verkürzt wird. Die folgenden Komponentendienste sind Teil des Microsoft 365 Defender-Stapels:

- **Microsoft Defender for Endpoint** (vormals Microsoft Defender ATP)
- **Microsoft Defender for Identity** (vormals Azure ATP)
- **Microsoft Defender for Office 365** (vormals Office 365 ATP)
- **Microsoft Cloud App Security**

Zusätzlich zur Erfassung von Warnungen von diesen Komponenten erstellt Microsoft 365 Defender eigene Warnungen. Die Anwendung erstellt Vorfälle aus allen diesen Warnungen und sendet sie an Azure Sentinel.

### <a name="common-use-cases-and-scenarios"></a>Gängige Anwendungsfälle und -szenarien

- Die Ein-Klick-Verbindung von Microsoft 365 Defender-Vorfällen, einschließlich aller Warnungen und Entitäten von Microsoft 365 Defender-Komponenten, in Azure Sentinel.

- Bidirektionale Synchronisierung zwischen Sentinel- und Microsoft 365 Defender-Vorfällen bezüglich Status, Besitzer und Schließungsgrund.

- Nutzen der Funktionen zur Gruppierung und Anreicherung von Microsoft 365 Defender-Warnungen in Azure Sentinel, wodurch sich die Zeit der Problemlösung verkürzt.

- Kontextbezogener Deep Link zwischen einem Azure Sentinel-Vorfall und seinem parallelen Microsoft 365 Defender-Vorfall, um die Untersuchung über beide Portale hinweg zu erleichtern.

### <a name="connecting-to-microsoft-365-defender"></a>Herstellen einer Verbindung mit Microsoft 365 Defender

Sobald Sie den Microsoft 365 Defender-Datenverbinder zum [Erfassen von Vorfällen und Warnungen](connect-microsoft-365-defender.md) aktiviert haben, werden Microsoft 365 Defender-Vorfälle in der Azure Sentinel-Vorfalls-Queue mit **Microsoft 365 Defender** im Feld **Produktname** angezeigt, kurz nachdem sie in Microsoft 365 Defender erstellt wurden.
- Es kann bis zu 10 Minuten ab dem Zeitpunkt dauern, an dem ein Vorfall, der in Microsoft 365 Defender erstellt wird, in Azure Sentinel angezeigt wird.

- Vorfälle werden ohne zusätzliche Kosten erfasst und synchronisiert.

Sobald die Microsoft 365 Defender-Integration verbunden ist, werden alle Komponenten-Warnungsverbinder (Defender für Endpunkt, Defender für Identität, Defender für Office 365 Cloud App Security) automatisch im Hintergrund verbunden, sofern sie es vorher noch nicht waren. Wenn nach der Verbindung von Microsoft 365 Defender Komponenten-Lizenzen erworben wurden, werden die Warnungen und Vorfälle aus dem neuen Produkt weiterhin ohne zusätzliche Konfiguration oder Gebühren an Azure Sentinel weitergeleitet.

### <a name="microsoft-365-defender-incidents-and-microsoft-incident-creation-rules"></a>Microsoft 365 Defender-Vorfälle und Microsoft-Regeln zur Erstellung von Vorfällen

- Von Microsoft 365 Defender generierte Incidents, die auf Warnungen von Microsoft 365 Sicherheitsprodukten basieren, werden mit benutzerdefinierter Microsoft 365 Defender-Logik erstellt.

- Die Microsoft-Regeln für die Erstellung von Vorfällen in Azure Sentinel erstellen ebenfalls Vorfälle aus denselben Warnungen, wobei eine (andere) benutzerdefinierte Azure Sentinel-Logik verwendet wird.

- Die gemeinsame Verwendung beider Mechanismen wird vollständig unterstützt, und diese Konfiguration kann verwendet werden, um den Übergang zur neuen Microsoft 365 Defender-Logik für die Erstellung von Vorfällen zu erleichtern. Dies führt jedoch zu **duplizierten Vorfällen** für dieselben Warnungen.

- Um die Erstellung von duplizierten Vorfällen für dieselben Alarme zu vermeiden, empfehlen wir Kunden, alle **Regeln zur Erstellung von Microsoft-Vorfällen** für Microsoft 365-Produkte (Defender für Endpoint, Defender für Identity und Defender für Office 365 und Cloud App Security) zu deaktivieren, wenn sie den Microsoft 365 Defender verbinden. Dies kann durch das Deaktivieren der Vorfall-Erstellung auf der Verbinder-Seite erfolgen. Beachten Sie bitte, dass in diesem Fall alle Filter, die durch die Regeln zur Vorfallerstellung angewendet wurden, nicht auf die Microsoft 365 Defender-Vorfallintegration angewendet werden.

    > [!NOTE]
    > Alle Warnmeldungstypen von Microsoft Cloud App Security werden gerade in den Microsoft 365 Defender integriert.

### <a name="working-with-microsoft-365-defender-incidents-in-azure-sentinel-and-bi-directional-sync"></a>Mit Microsoft 365 Defender-Vorfällen in Azure Sentinel und der bidirektionalen Synchronisierung arbeiten

M365 Defender-Vorfälle werden in der Azure Sentinel-Vorfalls-Queue mit dem Produktnamen **Microsoft 365 Defender** und mit ähnlichen Details und Funktionen wie alle anderen Sentinel-Vorfälle angezeigt. Jeder Vorfall enthält einen Link, der zurück zum parallelen Vorfall im Microsoft 365 Defender-Portal führt.

In dem Maß, in dem sich der Vorfall in Microsoft 365 Defender weiterentwickelt und weitere Alarme oder Entitäten hinzugefügt werden, wird der Azure Sentinel-Vorfall entsprechend aktualisiert.

Änderungen am Status, am Schließungsgrund oder an der Zuweisung eines Microsoft 365-Vorfalls, die entweder in Microsoft 365 Defender oder in Azure Sentinel vorgenommen werden, werden auch in der Queue des jeweils anderen Vorfalls entsprechend aktualisiert. Die Synchronisierung erfolgt in beiden Portalen ohne Verzögerung sofort nach der Änderung des Vorfalls. Möglicherweise ist eine Aktualisierung erforderlich, um die neuesten Änderungen anzuzeigen.

Im Microsoft 365 Defender können alle Warnungen aus einem Vorfall in einen anderen übertragen werden, was zu einer Zusammenführung der Vorfälle führt. Wenn diese Zusammenführung erfolgt, spiegeln die Azure Sentinel-Vorfälle die Änderungen wider. Ein Vorfall enthält alle Warnungen der beiden ursprünglichen Vorfälle, und der andere Vorfall wird automatisch geschlossen und mit dem Tag „umgeleitet“ versehen.

> [!NOTE]
> Vorfälle in Azure Sentinel können maximal 150 Warnungen enthalten. Microsoft 365 Defender-Vorfälle können aus mehr als dies bestehen. Wenn ein Microsoft 365 Defender-Vorfall mit mehr als 150 Alarmen mit Azure Sentinel synchronisiert wird, wird der Sentinel-Vorfall als mit "150+" Alarmen angezeigt und bietet einen Link zu dem parallelen Vorfall in Microsoft 365 Defender, wo Sie den vollständigen Alarmsatz sehen können.

## <a name="advanced-hunting-event-collection"></a>Erfassung erweiterter Huntingereignisse

Mit dem Microsoft 365 Defender-Connector können Sie auch **erweiterte Huntingereignisse** (eine Art von Ereignisrohdaten) aus Microsoft 365 Defender und den zugehörigen Komponentendiensten in Azure Sentinel streamen. Sie können derzeit [erweiterte Huntingereignisse](/windows/security/threat-protection/microsoft-defender-atp/advanced-hunting-overview) von [Microsoft Defender für Endpunkt (MDATP)](/windows/security/threat-protection/microsoft-defender-atp/microsoft-defender-advanced-threat-protection) erfassen und diese direkt in eigens erstellte Tabellen in Ihrem Azure Sentinel-Arbeitsbereich streamen. Diese Tabellen bauen auf dem gleichen Schema auf, das im Microsoft 365 Defender-Portal verwendet wird, wodurch Sie vollständigen Zugriff auf die gesamte Reihe erweiterter Huntingereignisse erhalten und Folgendes erreichen können:

- Einfaches Kopieren Ihrer vorhandenen erweiterten Huntingabfragen für Microsoft Defender für Endpunkt in Azure Sentinel.

- Verwenden Sie die rohen Ereignisprotokolle, um weitere Erkenntnisse für Ihre Warnungen, die Suche und die Untersuchung zu gewinnen, und diese Ereignisse mit Ereignissen aus anderen Datenquellen im Azure Sentinel korrelieren zu können.

- Speichern der Protokolle mit verlängerter Aufbewahrungsdauer, jenseits der Standardaufbewahrungsdauer von 30 Tagen in Microsoft Defender for Endpoint oder Microsoft 365 Defender. Dies erreichen Sie durch Konfigurieren der Aufbewahrungsdauer Ihres Arbeitsbereichs oder der tabellenweisen Aufbewahrung in Log Analytics.

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument haben Sie erfahren, wie Sie von der Verwendung von Microsoft 365 Defender zusammen mit Azure Sentinel profitieren können, indem Sie den Microsoft 365 Defender-Connector verwenden.

- Rufen Sie Anweisungen zum [Aktivieren des Microsoft 365 Defender-Connectors](connect-microsoft-365-defender.md) ab.
- Erstellen Sie [benutzerdefinierte Warnungen](detect-threats-custom.md), und [untersuchen Sie Vorfälle](investigate-cases.md).
