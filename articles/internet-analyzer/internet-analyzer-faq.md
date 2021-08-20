---
title: Häufig gestellte Fragen zu Azure Internet Analyzer | Microsoft-Dokumentation
description: Häufig gestellte Fragen zu Azure Internet Analyzer.
services: internet-analyzer
author: KumudD
ms.service: internet-analyzer
ms.topic: guide
ms.date: 10/16/2019
ms.author: kumud
ms.openlocfilehash: 9a9bedc1dcc1d217a3f7dac270245afa923482d8
ms.sourcegitcommit: 7d63ce88bfe8188b1ae70c3d006a29068d066287
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2021
ms.locfileid: "114454831"
---
# <a name="azure-internet-analyzer-faq-preview"></a>Häufig gestellte Fragen zu Azure Internet Analyzer (Vorschauversion)

Dies sind die häufig gestellten Fragen zu Azure Internet Analyzer. Wenn Sie weitere Fragen haben, besuchen Sie das [Feedbackforum](https://aka.ms/internetAnalyzerFeedbackForum), und stellen Sie Ihre Frage. Wenn eine Frage häufiger gestellt wird, fügen wir sie diesem Artikel hinzu, damit sie schnell und einfach gefunden werden kann.

## <a name="how-do-i-participate-in-the-preview"></a>Wie nehme ich an der Vorschauversion teil?

Die Vorschauversion ist für ausgewählte Kunden verfügbar. Wenn Sie an der Vorschauversion interessiert sind, gehen Sie so vor:

1. Melden Sie sich beim [Azure-Portal](https://ms.portal.azure.com) an.
2. Navigieren Sie zur Seite **Abonnements**.
3. Klicken Sie auf das Azure-Abonnement, für das Sie Internet Analyzer verwenden möchten.
4. Wechseln Sie zu den Einstellungen für **Ressourcenanbieter** für das Abonnement.
5. Suchen Sie nach **Microsoft.Network**, und klicken Sie auf die Schaltfläche **Registrieren** (bzw. **Erneut registrieren**).
![Zugriff beantragen](./media/ia-faq/request-preview-access.png)

6. [Beantragen Sie die Genehmigung](https://aka.ms/internetAnalyzerContact), indem Sie uns Ihre E-Mail-Adresse und Azure-Abonnement-ID mitteilen, mit der der Zugriff beantragt wurde.
7. Nach der Genehmigung Ihres Antrags erhalten Sie eine E-Mail-Bestätigung und können Internet Analyzer-Ressourcen im Rahmen des neu zugelassenen Azure-Abonnements erstellen, aktualisieren und ändern.

## <a name="do-i-need-to-embed-the-client-to-run-a-test"></a>Muss ich den Client installieren, um einen Test ausführen zu können?

Ja, der Internet Analyzer-Client muss in Ihrer Anwendung installiert sein, um für Ihre Benutzer spezifische Metriken zu sammeln. [Erfahren Sie, wie Sie den Client installieren.](internet-analyzer-embed-client.md) 

## <a name="do-i-get-billed-for-participating-in-the-preview"></a>Fallen für die Nutzung der Vorschauversion Gebühren an?
Nein, Azure Internet Analyzer ist in der Vorschauphase kostenlos. In Vorschauphase gibt es keine Vereinbarung zum Servicelevel.

## <a name="what-scenarios-is-internet-analyzer-designed-to-address"></a>Für welche Szenarien ist Internet Analyzer konzipiert?

Internet Analyzer ist so konzipiert, dass Sie Einblicke in die Netzwerkleistung erhalten, die auf Ihrer Benutzerpopulation basieren. Um für Ihre Benutzer die besten Entscheidungen hinsichtlich Leistung zu treffen, vergleicht Internet Analyzer die Leistung von zwei Endpunkten im Internet anhand Ihrer besonderen Benutzerpopulation. Internet Analyzer kann eine Vielzahl von Fragen beantworten, darunter diese sehr häufig gestellten:

* Wie wirkt sich die Migration zur Cloud auf die Leistung aus? 
    * *Vorgeschlagener Test: Benutzerdefiniert (Ihre aktuelle lokale Infrastruktur) im Vergleich zu Azure (beliebiger vorkonfigurierter Endpunkt)*
* Welche Vorteile ergeben sich, wenn ich meine Daten statt in einem Rechenzentrum am Edge platziere? 
    *  *Vorgeschlagener Test: Azure im Vergleich zu Azure Front Door, Azure im Vergleich zu Azure CDN von Microsoft*
* Welche Leistungsvorteile bietet Azure Front Door?
    *  *Vorgeschlagener Test: Benutzerdefiniert/Azure/CDN im Vergleich zu Azure Front Door*
* Welche Leistungsvorteile bietet Azure CDN von Microsoft? 
    *  *Vorgeschlagener Test: Benutzerdefiniert/Azure/Azure Front Door im Vergleich zu Azure CDN von Microsoft*
* Wie schneidet Azure CDN von Microsoft ab? 
    *  *Vorgeschlagener Test: Benutzerdefiniert (anderer CDN-Endpunkt) im Vergleich zu Azure CDN von Microsoft*
* Welches ist die beste Cloud für Ihre Endbenutzerpopulation in jeder Region? 
    *  *Vorgeschlagener Test: Benutzerdefiniert (anderer Clouddienst) im Vergleich zu Azure (beliebiger vorkonfigurierter Endpunkt)*

## <a name="which-tests-can-i-run-in-preview"></a>Welche Tests können in der Vorschauphase ausgeführt werden?

Jeder Test, den Sie in Internet Analyzer erstellen, besteht aus zwei Endpunkten: Endpunkt A und Endpunkt B. Alle folgenden Kombinationen können als Test ausgeführt werden:  
* Zwei vorkonfigurierte Endpunkte,
* Ein benutzerdefinierter und ein vorkonfigurierter Endpunkt oder
* Zwei [benutzerdefinierte Endpunkte](internet-analyzer-custom-endpoint.md) (ein benutzerdefinierter Endpunkt muss sich in Azure befinden).

In der Vorschauphase sind die folgenden vorkonfigurierten Endpunkte verfügbar:
* **Azure-Regionen**
    * Brasilien Süd
    * Indien, Mitte
    * USA (Mitte)
    * Asien, Osten
    * East US
    * Japan, Westen
    * Nordeuropa
    * Südafrika, Norden
    * Asien, Südosten
    * Vereinigte Arabische Emirate, Norden
    * UK, Westen  
    * Europa, Westen
    * USA (Westen)
    * USA, Westen 2
* **Kombinationen mehrerer Azure-Regionen**
    * USA, Osten; Brasilien, Süden
    * USA, Osten; Asien, Osten
    * Europa, Westen; Brasilien, Süden
    * Europa, Westen; Asien, Südosten
    * Europa, Westen; VAE, Norden
    * USA, Westen; USA, Osten
    * USA, Westen; Europa, Westen
    * USA, Westen; VAE, Norden
    * Europa, Westen; VAE, Norden; Asien, Südosten
    * USA, Westen; Europa, Westen; Asien, Osten
    * USA, Westen; Europa, Norden; Asien, Südosten; VAE, Norden; Südafrika, Norden 
* **Azure & Azure Front Door**: in jeder beliebigen oben aufgeführten Azure-Einzelregion oder Kombination aus mehreren Azure-Regionen bereitgestellt
* **Azure & Azure CDN von Microsoft**: in jeder beliebigen oben aufgeführten Azure-Einzelregion bereitgestellt
* **Azure & Azure Traffic Manager**: in jeder beliebigen oben aufgeführten Kombination aus mehreren Azure-Regionen bereitgestellt

## <a name="how-is-internet-analyzer-different-from-other-monitoring-services-provided-by-azure"></a>Wie unterscheidet sich Internet Analyzer von anderen von Azure bereitgestellten Überwachungsdiensten?

Internet Analyzer unterstützt Sie dabei, die Leistung Ihrer Endbenutzer zu verstehen und Entscheidungen zum Verbessern der Leistung zu treffen. Während andere Azure-Überwachungstools Einblicke in Ihre Azure-Dienste geben, konzentriert sich Internet Analyzer auf die Messung der Internetleistung für Ihre Benutzer.

## <a name="how-is-measurement-data-handled-by-internet-analyzer"></a>Wie werden Messungsdaten von Internet Analyzer behandelt?

Azure verfügt über [ausgefeilte Sicherheitsprozesse und erfüllt eine breite Palette von Kompatibilitätsstandards](https://azure.microsoft.com/support/trust-center/). Auf die Daten haben nur Sie selbst und Ihr Team Zugriff. Microsoft-Mitarbeiter können nur unter ganz bestimmten Bedingungen und mit Ihrem Wissen eingeschränkten Zugriff auf die Daten erlangen. Die Daten sind während der Übertragung und im Ruhezustand verschlüsselt.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in unserer [Übersicht über Internet Analyzer](internet-analyzer-overview.md).
