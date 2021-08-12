---
title: Problembehandlung für Datadog – Azure-Partnerlösungen
description: Dieser Artikel bietet Informationen zur Problembehandlung für Datadog in Azure.
ms.service: partner-services
ms.topic: conceptual
ms.date: 05/28/2021
author: tfitzmac
ms.author: tomfitz
ms.openlocfilehash: 1c23851dc76cb0e012ad8796d361e5c293bc8d99
ms.sourcegitcommit: 1b698fb8ceb46e75c2ef9ef8fece697852c0356c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2021
ms.locfileid: "110655151"
---
# <a name="fix-common-errors-for-datadog-on-azure"></a>Beheben allgemeiner Fehler bei Datadog in Azure

Dieses Dokument enthält Informationen zur Problembehandlung für Lösungen, die Datadog verwenden.

## <a name="purchase-errors"></a>Fehler beim Kauf

* Beim Kauf tritt ein Fehler auf, weil keine gültige Kreditkarte mit dem Azure-Abonnement verknüpft ist oder keine Zahlungsmethode für das Abonnement angegeben ist.

  Verwenden Sie ein anderes Azure-Abonnement. Alternativ können Sie die Kreditkarte oder Zahlungsmethode für das Abonnement hinzufügen oder aktualisieren. Weitere Informationen finden Sie unter [Hinzufügen oder Aktualisieren einer Kreditkarte für Azure](../../cost-management-billing/manage/change-credit-card.md).

* Das EA-Abonnement lässt keine Marketplace-Käufe zu.

  Verwenden Sie ein anderes Abonnement, oder überprüfen Sie, ob Marketplace-Käufe für Ihr EA-Abonnement aktiviert sind. Weitere Informationen finden Sie unter [Aktivieren von Azure Marketplace-Einkäufen](../../cost-management-billing/manage/ea-azure-marketplace.md#enabling-azure-marketplace-purchases). Wenn hierdurch das Problem nicht behoben wird, wenden Sie sich an den [Datadog-Support](https://www.datadoghq.com/support).

## <a name="unable-to-create-datadog-resource"></a>Erstellen einer Datadog-Ressource nicht möglich

Zum Einrichten der Azure-Datadog-Integration müssen Sie über die Zugriffsberechtigung **Besitzer** für das Azure-Abonnement verfügen. Stellen Sie sicher, dass Sie über den richtigen Zugriff verfügen, bevor Sie mit der Einrichtung beginnen.

## <a name="single-sign-on-errors"></a>Fehler beim einmaligen Anmelden

**Einstellungen für das einmalige Anmelden können nicht gespeichert werden**: Dieser Fehler tritt auf, wenn eine andere Unternehmens-App vorhanden ist, die den SAML-Bezeichner von Datadog verwendet. Um herauszufinden, um welche App es sich handelt, wählen Sie im Abschnitt „Grundlegende SAML-Konfiguration“ die Option **Bearbeiten** aus.

Um dieses Problem zu beheben, deaktivieren Sie die andere App entweder oder verwenden sie als Unternehmens-App, um SAML-SSO mit Datadog einzurichten. Wenn Sie sich für die Verwendung der anderen App entscheiden, stellen Sie sicher, dass diese über die [erforderlichen Einstellungen](create.md#configure-single-sign-on) verfügt.

**App wird auf Einstellungsseite für einmaliges Anmelden nicht angezeigt**: Suchen Sie zunächst nach der Anwendungs-ID. Wenn kein Ergebnis angezeigt wird, überprüfen Sie die SAML-Einstellungen der App. Das Raster zeigt nur Apps mit den richtigen SAML-Einstellungen. 

Die Bezeichner-URL muss `https://us3.datadoghq.com/account/saml/metadata.xml` lauten.

Die Antwort-URL muss `https://us3.datadoghq.com/account/saml/assertion` lauten.

Die folgende Abbildung zeigt die korrekten Werte.
  
:::image type="content" source="media/troubleshoot/troubleshooting.png" alt-text="Überprüfen Sie die SAML-Einstellungen für die Datadog-Anwendung in AAD." border="true":::

**Zum Mandanten eingeladene Gastbenutzer können nicht auf das einmalige Anmelden zugreifen**: Einige Benutzer verfügen über zwei E-Mail-Adressen im Azure-Portal. In der Regel ist eine E-Mail-Adresse der Benutzerprinzipalname (User Principal Name, UPN), die andere eine alternative E-Mail-Adresse.

Wenn Sie Gastbenutzer einladen, verwenden Sie den Benutzerprinzipalnamen im Mandanten. Dadurch bleibt die E-Mail-Adresse während des Prozesses des einmaligen Anmeldens synchron. Sie finden den Benutzerprinzipalnamen, indem Sie in der oberen rechten Ecke der Azure-Portal-Instanz des Benutzers nach der E-Mail-Adresse suchen.
  
## <a name="logs-not-being-emitted"></a>Keine Protokollausgabe

Nur Ressourcen, die in Azure Monitor-Ressourcenprotokollkategorien aufgelistet sind, geben Protokolle an Datadog aus. Um zu überprüfen, ob eine Ressource Protokolle an Datadog ausgibt, navigieren Sie zu den Azure-Diagnoseeinstellungen für diese Ressource. Überprüfen Sie, ob eine Diagnoseeinstellung für Datadog vorhanden ist.

:::image type="content" source="media/troubleshoot/diagnostic-setting.png" alt-text="Datadog-Diagnoseeinstellung in der Azure-Ressource" border="true":::

## <a name="metrics-not-being-emitted"></a>Keine Metrikausgabe

Der Datadog-Ressource ist im entsprechenden Azure-Abonnement die Rolle **Überwachungsleser** zugewiesen. Mit dieser Rolle kann die Datadog-Ressource Metriken erfassen und an Datadog senden.

Um zu überprüfen, ob die Ressource über die richtige Rollenzuweisung verfügt, öffnen Sie das Azure-Portal und wählen das Abonnement aus. Klicken Sie im linken Bereich auf **Zugriffssteuerung (IAM)** . Suchen Sie nach dem Namen der Datadog-Ressource. Vergewissern Sie sich, dass die Datadog-Ressource über die Rollenzuweisung **Überwachungsleser** verfügt, wie unten gezeigt.

:::image type="content" source="media/troubleshoot/datadog-role-assignment.png" alt-text="Datadog-Rollenzuweisung im Azure-Abonnement" border="true":::

## <a name="datadog-agent-installation-fails"></a>Fehler bei der Installation des Datadog-Agents

Die Azure-Datadog-Integration ermöglicht Ihnen die Installation des Datadog-Agents auf einem virtuellen Computer oder in einem App-Dienst. Zum Konfigurieren des Datadog-Agents wird der auf dem Bildschirm mit den API-Schlüsseln als **Standardschlüssel** ausgewählte API-Schlüssel verwendet. Wenn kein Standardschlüssel ausgewählt ist, kann die Datadog-Agent-Installation nicht ausgeführt werden.

Wenn der Datadog-Agent mit einem falschen Schlüssel konfiguriert wurde, navigieren Sie zum Bildschirm mit den API-Schlüsseln und ändern den **Standardschlüssel**. Sie müssen den Datadog-Agent deinstallieren und erneut installieren, um den virtuellen Computer mit den neuen API-Schlüsseln zu konfigurieren.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die [Verwaltung Ihrer Datadog-Instanz](manage.md).
