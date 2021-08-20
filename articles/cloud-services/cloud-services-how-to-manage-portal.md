---
title: Allgemeine Verwaltungsaufgaben für Clouddienste | Microsoft-Dokumentation
description: Hier erfahren Sie mehr über die Verwaltung von Clouddiensten im Azure-Portal. In diesen Beispielen wird das Azure-Portal verwendet.
ms.topic: article
ms.service: cloud-services
ms.date: 10/14/2020
author: hirenshah1
ms.author: hirshah
ms.reviewer: mimckitt
ms.custom: ''
ms.openlocfilehash: 5ccdd2fb8e4f3f644ed94214d80ac115e5b5321d
ms.sourcegitcommit: 8b38eff08c8743a095635a1765c9c44358340aa8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/30/2021
ms.locfileid: "113093803"
---
# <a name="manage-cloud-services-classic-in-the-azure-portal"></a>Verwalten von Cloud Services (klassisch) im Azure-Portal

> [!IMPORTANT]
> [Azure Cloud Services (erweiterter Support)](../cloud-services-extended-support/overview.md) ist ein neues auf Azure Resource Manager basierendes Bereitstellungsmodell für Azure Cloud Services. Im Zuge dieser Änderung wurden Azure Cloud Services-Instanzen, die unter dem auf Azure Service Manager basierenden Bereitstellungsmodell ausgeführt werden, in „Cloud Services (klassisch)“ umbenannt. Für alle neuen Bereitstellungen wird [Azure Cloud Services (erweiterter Support)](../cloud-services-extended-support/overview.md) verwendet.

Im Bereich **Clouddienste** des Azure-Portals haben Sie folgende Möglichkeiten:

* Aktualisieren einer Dienstrolle oder -bereitstellung
* Heraufstufen einer gestaffelten Bereitstellung in die Produktion
* Verknüpfen von Ressourcen mit Ihrem Clouddienst, um die Ressourcenabhängigkeiten anzuzeigen und die Ressourcen gemeinsam zu skalieren
* Löschen eines Clouddiensts oder einer Clouddienstbereitstellung

Weitere Informationen zum Skalieren Ihres Clouddiensts finden Sie unter [Konfigurieren der automatischen Skalierung für einen Clouddienst im Portal](cloud-services-how-to-scale-portal.md).

## <a name="update-a-cloud-service-role-or-deployment"></a>Aktualisieren einer Clouddienstrolle oder -bereitstellung
Wenn Sie den Anwendungscode für den Clouddienst aktualisieren müssen, verwenden Sie die Option **Aktualisieren** auf dem Blatt für Clouddienste. Sie können eine oder alle Rollen aktualisieren. Zum Aktualisieren können Sie ein neues Dienstpaket oder eine Dienstkonfigurationsdatei hochladen.

1. Wählen Sie im [Azure-Portal][Azure portal]den Clouddienst aus, den Sie aktualisieren möchten. Dieser Schritt öffnet das Blatt für die Clouddienstinstanz.

2. Wählen Sie auf dem Blatt **Aktualisieren** aus.

    ![Schaltfläche „Aktualisieren“](./media/cloud-services-how-to-manage-portal/update-button.png)

3. Aktualisieren Sie die Bereitstellung mit einer neuen Dienstpaketdatei (.cspkg) und Dienstkonfigurationsdatei (.cscfg).

    ![Bereitstellung aktualisieren](./media/cloud-services-how-to-manage-portal/update-blade.png)

4. Optional können Sie die Bezeichnung für das Speicherkonto und die Bereitstellung aktualisieren.

5. Wenn Rollen nur eine Rolleninstanz umfassen, aktivieren Sie das Kontrollkästchen **Auch dann bereitstellen, wenn für mindestens eine Rolle nur eine Instanz vorhanden ist**, damit das Upgrade fortgesetzt werden kann.

    Azure kann während des Updates eines Clouddiensts nur dann eine Dienstverfügbarkeit von 99,95 Prozent garantieren, wenn jede Rolle über mindestens zwei Rolleninstanzen (virtuelle Computer) verfügt. Mit zwei Rolleninstanzen kann ein virtueller Computer Clientanforderungen verarbeiten, während der andere aktualisiert wird.

6. Aktivieren Sie das Kontrollkästchen **Bereitstellung starten**, damit das Update angewendet wird, nachdem der Upload des Pakets abgeschlossen wurde.

7. Klicken Sie auf **OK**, um mit der Aktualisierung des Diensts zu beginnen.

## <a name="swap-deployments-to-promote-a-staged-deployment-to-production"></a>Austauschen von Bereitstellungen zum Heraufstufen einer Bereitstellung in die Produktion
Wenn Sie beschließen, eine neue Version eines Clouddiensts bereitzustellen, stellen Sie die neue Version in der Clouddienst-Stagingumgebung bereit, und testen Sie sie dort. Tauschen Sie mit **Austauschen** die URLs aus, mit denen die beiden Bereitstellungen adressiert werden, und stufen Sie eine neue Version zur Produktion herauf.

Sie können Bereitstellungen über die Seite **Cloud-Dienste** oder über das Dashboard austauschen.

1. Wählen Sie im [Azure-Portal][Azure portal]den Clouddienst aus, den Sie aktualisieren möchten. Dieser Schritt öffnet das Blatt für die Clouddienstinstanz.

2. Wählen Sie auf dem Blatt **Tausch** aus.

    ![Schaltfläche zum Austauschen von Clouddiensten](./media/cloud-services-how-to-manage-portal/swap-button.png)

3. Die folgende Bestätigungsaufforderung wird geöffnet:

    ![Austauschen von Cloud-Diensten](./media/cloud-services-how-to-manage-portal/swap-prompt.png)

4. Klicken Sie nach dem Überprüfen der Bereitstellungsinformationen auf **OK**, um die Bereitstellungen auszutauschen.

    Der Austausch der Bereitstellungen erfolgt schnell, da sich nur die virtuellen IP-Adressen für die Bereitstellungen ändern.

    Um Rechenkosten zu sparen, können Sie Ihre Stagingbereitstellung löschen, nachdem Sie verifiziert haben, dass die Produktionsbereitstellung wie erwartet funktioniert.

### <a name="common-questions-about-swapping-deployments"></a>Häufig gestellte Fragen zum Austauschen der Bereitstellungen

**Was sind die Voraussetzungen zum Austauschen von Bereitstellungen?**

Es gibt zwei wichtige Voraussetzungen für einen erfolgreichen Austausch von Bereitstellungen:

- Wenn Sie eine statische IP-Adresse für Ihren Produktionsslot verwenden möchten, müssen Sie auch eine IP für den Stagingslot reservieren. Andernfalls tritt bei dem Austausch ein Fehler auf.

- Alle Instanzen Ihrer Rollen müssen ausgeführt werden, bevor Sie den Austausch durchführen können. Sie können den Status Ihrer Instanzen im Azure-Portal auf dem Blatt **Übersicht** überprüfen. Alternativ können Sie den Befehl [Get-AzureRole](/powershell/module/servicemanagement/azure.service/get-azurerole) in Windows PowerShell verwenden.

Beachten Sie, dass Updates von Gastbetriebssystemen und Dienstreparaturvorgänge auch dazu führen können, dass beim Austausch von Bereitstellungen ein Fehler auftritt. Weitere Informationen finden Sie unter [Behandeln von Problemen mit der Clouddienstbereitstellung](cloud-services-troubleshoot-deployment-problems.md).

**Führt ein Austausch zu einer Ausfallzeit für die Anwendung? Wie sollte ich dabei vorgehen?**

Wie im vorherigen Abschnitt beschrieben, erfolgt der Austausch von Bereitstellungen in der Regel schnell, da es sich nur um eine Konfigurationsänderung im Azure Load Balancer handelt. In einigen Fällen kann der Austausch jedoch zehn oder mehr Sekunden dauern und zu vorübergehenden Verbindungsausfällen führen. Um die Auswirkungen auf Ihre Kunden zu minimieren, ist es empfehlenswert, [Clientwiederholungslogik](/azure/architecture/best-practices/transient-faults) zu implementieren.

## <a name="delete-deployments-and-a-cloud-service"></a>Löschen von Bereitstellungen und eines Clouddiensts
Bevor Sie einen Cloud-Dienst löschen können, müssen Sie die einzelnen bestehenden Bereitstellungen löschen.

Um Rechenkosten zu sparen, können Sie Ihre Stagingbereitstellung löschen, nachdem Sie verifiziert haben, dass die Produktionsbereitstellung wie erwartet funktioniert. Für angehaltene bereitgestellte Rolleninstanzen werden Ihnen Rechenkosten in Rechnung gestellt.

Gehen Sie folgendermaßen vor, um eine Bereitstellung oder Ihren Cloud-Dienst zu löschen.

1. Wählen Sie im [Azure-Portal][Azure portal]den Clouddienst aus, den Sie löschen möchten. Dieser Schritt öffnet das Blatt für die Clouddienstinstanz.

2. Wählen Sie auf dem Blatt **Löschen** aus.

    ![Schaltfläche zum Löschen von Clouddiensten](./media/cloud-services-how-to-manage-portal/delete-button.png)

3. Um den gesamten Clouddienst zu löschen, aktivieren Sie das Kontrollkästchen **Clouddienst und zugehörige Bereitstellungen**. Alternativ können Sie entweder das Kontrollkästchen **Produktionsbereitstellung** oder das Kontrollkästchen **Stagingbereitstellung** aktivieren.

    ![Löschen von Clouddiensten](./media/cloud-services-how-to-manage-portal/delete-blade.png)

4. Wählen Sie unten die Option **Löschen** aus.

5. Um den Clouddienst zu löschen, klicken Sie auf **Clouddienst löschen**. Klicken Sie dann an der Bestätigungsaufforderung auf **Ja**.

> [!NOTE]
> Wenn ein Clouddienst gelöscht wird und die ausführliche Überwachung konfiguriert ist, müssen Sie die Daten manuell aus Ihrem Speicherkonto löschen. Informationen zum Standort der Metriktabellen finden Sie unter [Einführung in die Überwachung von Clouddiensten](cloud-services-how-to-monitor.md).


## <a name="find-more-information-about-failed-deployments"></a>Suche nach Informationen zu fehlerhaften Bereitstellungen
Oben auf dem Blatt **Übersicht** befindet sich eine Statusleiste. Wenn Sie auf diese Leiste klicken, wird ein neues Blatt geöffnet, auf dem eventuelle Fehlerinformationen angezeigt werden. Wenn die Bereitstellung keine Fehler enthält, ist dieses Blatt leer.

![Clouddienstübersicht](./media/cloud-services-how-to-manage-portal/status-info.png)



[Azure portal]: https://portal.azure.com

## <a name="next-steps"></a>Nächste Schritte
* [Allgemeine Konfiguration Ihres Clouddiensts](cloud-services-how-to-configure-portal.md)
* Weitere Informationen zum [Bereitstellen eines Clouddiensts](cloud-services-how-to-create-deploy-portal.md)
* [Konfigurieren eines benutzerdefinierten Domänennamens](cloud-services-custom-domain-name-portal.md)
* Konfigurieren von [TLS/SSL-Zertifikaten](cloud-services-configure-ssl-certificate-portal.md)