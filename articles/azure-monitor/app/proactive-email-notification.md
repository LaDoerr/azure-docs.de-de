---
title: Änderung an Benachrichtigungen der intelligenten Erkennung – Azure Application Insights
description: Änderung an standardmäßigen Benachrichtigungsempfängern der intelligenten Erkennung. Mit der intelligenten Erkennung können Sie Anwendungsablaufverfolgungen mit Azure Application Insights auf ungewöhnliche Muster in der Ablaufverfolgungstelemetrie überwachen.
ms.topic: conceptual
author: harelbr
ms.author: harelbr
ms.date: 02/14/2021
ms.reviewer: mbullwin
ms.openlocfilehash: 0937683eaacefc9fe4bbee21802a0f1657918efd
ms.sourcegitcommit: bd65925eb409d0c516c48494c5b97960949aee05
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/06/2021
ms.locfileid: "111536711"
---
# <a name="smart-detection-e-mail-notification-change"></a>Änderung an E-Mail-Benachrichtigungen der intelligenten Erkennung

>[!NOTE]
>Sie können Ihre Application Insight-Ressourcen zu der auf Warnungen basierenden intelligenten Erkennung (Vorschauversion) migrieren. Bei der Migration werden Warnungsregeln für die verschiedenen Module der intelligenten Erkennung erstellt. Nach der Erstellung können Sie diese Regeln wie alle anderen Azure Monitor-Warnungsregeln verwalten und konfigurieren. Sie können auch Aktionsgruppen für diese Regeln konfigurieren, was mehrere Methoden für das Ausführen von Aktionen oder für das Auslösen von Benachrichtigungen bei neuen Erkennungen ermöglicht.
>
> Weitere Informationen zum Migrationsprozess und zum Verhalten der intelligenten Erkennung nach der Migration finden Sie unter [Warnungen der intelligenten Erkennung – Migration](../alerts/alerts-smart-detections-migration.md).

Basierend auf Kundenfeedback ändern wir am 1. April 2019 die Standardrollen, die E-Mail-Benachrichtigungen von der intelligenten Erkennung erhalten.

## <a name="what-is-changing"></a>Was ändert sich?

Derzeit werden E-Mail-Benachrichtigungen der intelligenten Erkennung standardmäßig an die Rollen _Abonnementbesitzer_, _Abonnementmitwirkender_ und _Abonnementleser_ gesendet. Zu diesen Rollen gehören häufig Benutzer, die nicht aktiv an der Überwachung beteiligt sind, daher erhalten viele dieser Benutzer unnötige Benachrichtigungen. Um diesen Umstand zu verbessern, nehmen wir eine Änderung vor, sodass E-Mail-Benachrichtigungen standardmäßig nur an die Rollen [Benutzer mit Leseberechtigung für Überwachungsdaten](../../role-based-access-control/built-in-roles.md#monitoring-reader) und [Mitwirkender an der Überwachung](../../role-based-access-control/built-in-roles.md#monitoring-contributor) gesendet werden.

## <a name="scope-of-this-change"></a>Geltungsbereich dieser Änderung

Diese Änderung wirkt sich mit folgenden Ausnahmen auf alle Regeln der intelligenten Erkennung aus:

* Regeln der intelligenten Erkennung, die als Vorschauversion gekennzeichnet sind. Diese Regeln der intelligenten Erkennung unterstützen gegenwärtig keine E-Mail-Benachrichtigungen.

* Regel für Fehleranomalien.

## <a name="how-to-prepare-for-this-change"></a>Vorbereitung auf diese Änderung

Um sicherzustellen, dass E-Mail-Benachrichtigungen von der intelligenten Erkennung an die relevanten Benutzer gesendet werden, müssen diese Benutzer den Rollen [Benutzer mit Leseberechtigung für Überwachungsdaten](../../role-based-access-control/built-in-roles.md#monitoring-reader) und [Mitwirkender an der Überwachung](../../role-based-access-control/built-in-roles.md#monitoring-contributor) des Abonnements zugewiesen werden.

Um Benutzer über das Azure-Portal der Rolle „Benutzer mit Leseberechtigung für Überwachungsdaten“ oder „Mitwirkender an der Überwachung“ zuzuweisen, befolgen Sie die im Artikel [Hinzufügen oder Entfernen von Azure-Rollenzuweisungen über das Azure-Portal](../../role-based-access-control/role-assignments-portal.md) beschriebenen Schritte. Achten Sie darauf, _Benutzer mit Leseberechtigung für Überwachungsdaten_ oder _Mitwirkender an der Überwachung_ als die Rolle auszuwählen, der die Benutzer zugewiesen werden.

> [!NOTE]
> Spezifische Empfänger von Benachrichtigungen der intelligenten Erkennung, die in den Regeleinstellungen über die Option _Weitere E-Mail-Empfänger_ konfiguriert wurden, sind von dieser Änderung nicht betroffen. Diese Empfänger erhalten weiterhin die E-Mail-Benachrichtigungen.

Wenn Sie Fragen oder Bedenken zu dieser Änderung haben, treten Sie gerne mit uns in [Kontakt](mailto:smart-alert-feedback@microsoft.com).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur intelligenten Erkennung:

- [Anomalien bei Fehlern](./proactive-failure-diagnostics.md)
- [Arbeitsspeicherverluste](./proactive-potential-memory-leak.md)
- [Leistungsanomalien](./proactive-performance-diagnostics.md)

