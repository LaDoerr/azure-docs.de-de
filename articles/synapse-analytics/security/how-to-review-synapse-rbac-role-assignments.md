---
title: Überprüfen von Synapse RBAC-Rollenzuweisungen in Synapse Studio
description: In diesem Artikel erfahren Sie, wie Sie Synapse RBAC-Rollenzuweisungen in Synapse Studio überprüfen.
author: RonyMSFT
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: security
ms.date: 12/1/2020
ms.author: ronytho
ms.reviewer: jrasnick
ms.openlocfilehash: acc1ed8cece087628f6f8deb41696acefe124874
ms.sourcegitcommit: 7f59e3b79a12395d37d569c250285a15df7a1077
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/02/2021
ms.locfileid: "110784028"
---
# <a name="how-to-review-synapse-rbac-role-assignments"></a>Überprüfen von Synapse RBAC-Rollenzuweisungen

Mithilfe von Synapse RBAC-Rollen werden Benutzern, Gruppen und anderen Sicherheitsprinzipalen Berechtigungen zugewiesen, mit denen sie auf Synapse-Ressourcen zugreifen und diese verwenden können.  [Weitere Informationen](./synapse-workspace-synapse-rbac.md)

In diesem Artikel wird erläutert, wie Sie die aktuellen Rollenzuweisungen für einen Arbeitsbereich überprüfen.

Sie können für jede Synapse RBAC-Rolle die Synapse RBAC-Rollenzuweisungen für alle Bereiche auflisten, einschließlich Zuweisungen für Objekte, auf die Sie keinen Zugriff haben. Nur ein Synapse-Administrator kann Synapse RBAC-Zugriff gewähren.  

>[!Note]
> Gastbenutzer (aus einem anderen AD-Mandanten) können Rollenzuweisungen auch anzeigen und verwalten, nachdem ihnen die Rolle „Synapse-Administrator“ zugewiesen wurde.    

## <a name="open-synapse-studio"></a>Öffnen von Synapse Studio  

Öffnen Sie zum Überprüfen der Rollenzuweisungen zunächst [Synapse Studio](https://web.azuresynapse.net/), und wählen Sie Ihren Arbeitsbereich aus. 

![Anmelden beim Arbeitsbereich](./media/common/login-workspace.png) 
 
 Nachdem Sie den Arbeitsbereich geöffnet haben, wählen Sie auf der linken Seite den Hub **Verwalten** aus. Erweitern Sie dann den Abschnitt **Sicherheit**, und wählen Sie **Zugriffssteuerung** aus. 

 ![Auswählen von „Zugriffssteuerung“ auf der linken Seite im Abschnitt „Sicherheit“](./media/how-to-manage-synapse-rbac-role-assignments/left-nav-security-access-control.png)

## <a name="review-workspace-role-assignments"></a>Überprüfen der Rollenzuweisungen im Arbeitsbereich

Auf dem Bildschirm „Zugriffssteuerung“ werden alle aktuellen Rollenzuweisungen für den Arbeitsbereich gruppiert nach der Rolle aufgelistet. Jede Zuweisung umfasst den Prinzipalnamen, den Prinzipaltyp, die Rolle und ihren Bereich.

![Bildschirm „Zugriffssteuerung“](./media/how-to-review-synapse-rbac-role-assignments/access-control-assignments.png)

Wenn einem Prinzipal dieselbe Rolle in mehreren Bereichen zugewiesen wurde, werden für diesen Prinzipal mehrere Zuweisungen jeweils für die einzelnen Bereiche angezeigt.  

Wenn eine Rolle einer Sicherheitsgruppe zugewiesen wurde, werden die Rollen angezeigt, die der Gruppe explizit zugewiesen sind, jedoch keine von übergeordneten Gruppen geerbten Rollen.  

Sie können die Liste nach dem Prinzipalnamen oder der E-Mail-Adresse sowie die Objekttypen, Rollen und Bereiche einzeln filtern. Geben Sie Ihren Namen oder E-Mail-Alias im Namensfilter ein, um die Ihnen zugewiesenen Rollen anzuzeigen. Nur ein Synapse-Administrator kann Ihre Rollen ändern.

>[!Important] 
>Wenn Sie direkt oder indirekt Mitglied einer Gruppe sind, der Rollen zugewiesen sind, verfügen Sie möglicherweise über Berechtigungen, die hier nicht angezeigt werden.

>[!tip]
>Sie finden Ihre Gruppenmitgliedschaften mithilfe von Azure Active Directory im Azure-Portal.  

Wenn Sie einen neuen Arbeitsbereich erstellen, werden Ihnen und dem Dienstprinzipal der MSI für den Arbeitsbereich automatisch die Rolle „Synapse-Administrator“ auf Arbeitsbereichsebene zugewiesen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Verwalten von Synapse RBAC-Rollenzuweisungen](./how-to-manage-synapse-rbac-role-assignments.md).

Erfahren Sie, [welche Rolle Sie für bestimmte Aufgaben benötigen](./synapse-workspace-understand-what-role-you-need.md).