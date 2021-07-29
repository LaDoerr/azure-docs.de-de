---
title: Integrieren von Azure Virtual Desktop mit Azure Advisor – Azure
description: So verwenden Sie Azure Advisor mit Ihrer Azure Virtual Desktop-Bereitstellung.
author: Heidilohr
ms.topic: how-to
ms.date: 03/31/2021
ms.author: helohr
manager: femila
ms.openlocfilehash: 25bdeb3db8872550a4f1f03e7165cd24653272c1
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111745347"
---
# <a name="use-azure-advisor-with-azure-virtual-desktop"></a>Verwenden von Azure Advisor mit Azure Virtual Desktop

Azure Advisor unterstützt Benutzer dabei, häufige Probleme selbst zu beheben, ohne dass Supportfälle eröffnet werden müssen. Die Empfehlungen verringern die Anzahl eingereichter Hilfeanfragen, sodass Sie Zeit und Kosten sparen.

In diesem Artikel erfahren Sie, wie Sie Azure Advisor in Ihrer Azure Virtual Desktop-Bereitstellung einrichten, um die Benutzer zu unterstützen.

## <a name="what-is-azure-advisor"></a>Was ist Azure Advisor?

Azure Advisor analysiert Ihre Konfigurationen und Telemetriedaten, um personalisierte Empfehlungen bereitzustellen, mithilfe derer Sie häufige Probleme lösen können. Mit diesen Empfehlungen können Sie Ihre Azure-Ressourcen im Hinblick auf Zuverlässigkeit, Sicherheit, optimalen Betrieb, Leistung und Kosten optimieren. Weitere Informationen finden Sie auf der [Azure Advisor-Website](https://azure.microsoft.com/services/advisor/).

## <a name="how-to-start-using-azure-advisor"></a>Einstieg in die Verwendung von Azure Advisor

Für den Einstieg benötigen Sie lediglich ein Azure-Konto im Azure-Portal. Öffnen Sie zunächst das Azure-Portal unter <https://portal.azure.com/#home>, und wählen Sie dann unter **Azure-Dienste** die Option **Advisor** aus, wie in der folgenden Abbildung dargestellt. Sie können auch „Azure Advisor“ in die Suchleiste im Azure-Portal eingeben.

> [!div class="mx-imgBorder"]
> ![Screenshot des Azure-Portals. Der Benutzer zeigt mit dem Mauszeiger auf den Azure Advisor-Link, was dazu führt, dass ein Dropdownmenü angezeigt wird.](media/azure-advisor.png)

Wenn Sie Azure Advisor öffnen, werden fünf Kategorien angezeigt:

- Kosten
- Sicherheit
- Zuverlässigkeit
- Optimaler Betrieb
- Leistung

> [!div class="mx-imgBorder"]
> ![Screenshot von Azure Advisor, der die fünf Menüs der einzelnen Kategorien anzeigt. Die fünf Elemente, die in separaten Feldern angezeigt werden, sind Kosten, Sicherheit, Zuverlässigkeit, Optimaler Betrieb und Leistung.](media/advisor-categories.png)

Wenn Sie eine Kategorie auswählen, gelangen Sie zur Seite mit aktiven Empfehlungen. Auf dieser Seite sehen Sie, welche Empfehlungen Azure Advisor für Sie hat, wie in der folgenden Abbildung dargestellt.

> [!div class="mx-imgBorder"]
> ![Screenshot der Liste aktiver Empfehlungen für den optimalen Betrieb. Die Liste zeigt sieben Empfehlungen mit unterschiedlichen Prioritätsstufen.](media/active-suggestions.png)

## <a name="additional-tips-for-azure-advisor"></a>Zusätzliche Tipps für Azure Advisor

- Stellen Sie sicher, dass Sie Ihre Empfehlungen häufig (mehr als einmal pro Woche) überprüfen. Azure Advisor aktualisiert die aktiven Empfehlungen mehrmals täglich. Wenn Sie regelmäßig nach neuen Empfehlungen suchen, können Sie größere Probleme dadurch vermeiden, dass Sie kleinere Probleme erkennen und beheben.

- Versuchen Sie immer, die Probleme mit der höchsten Prioritätsstufe in Azure Advisor zu lösen. Probleme mit hoher Priorität sind rot markiert. Wenn Sie Empfehlungen mit hoher Priorität nicht beheben, kann dies später zu Problemen führen.

- Wenn eine Empfehlung weniger wichtig erscheint, können Sie sie verwerfen oder verschieben. Um eine Empfehlung zu verwerfen oder zu verschieben, wechseln Sie zur Spalte **Aktion**, und ändern Sie den Zustand des Elements.

- Verwerfen Sie Empfehlungen nicht, wenn Sie nicht wissen, weshalb diese angezeigt werden, und stellen Sie sicher, dass sie keine negativen Auswirkungen auf Sie oder Ihre Benutzer haben. Wählen Sie immer **Weitere Informationen** aus, um zu erfahren, was das Problem ist. Wenn Sie ein Problem beheben, indem Sie die Anweisungen in Azure Advisor befolgen, wird es automatisch aus der Liste entfernt. Es empfiehlt sich, dass Sie Probleme beheben und sie nicht wiederholt verschieben.

- Wenn Sie ein Problem in Azure Virtual Desktop haben, überprüfen Sie immer zuerst Azure Advisor. In Azure Advisor erfahren Sie, wie Sie das Problem beheben können, oder Sie werden zumindest auf eine Ressource verwiesen, die Ihnen helfen kann.

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Auflösen von Empfehlungen finden Sie unter [Auflösen von Azure Advisor-Empfehlungen](azure-advisor-recommendations.md).
