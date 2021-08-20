---
title: Häufig gestellte Fragen
description: Häufig gestellte Fragen zum Azure Spatial Anchors-Dienst
author: msftradford
manager: MehranAzimi-msft
services: azure-spatial-anchors
ms.author: parkerra
ms.date: 11/20/2020
ms.topic: overview
ms.service: azure-spatial-anchors
ms.openlocfilehash: 2ddbf0c33cfe5377e8070438facf170b48312d8c
ms.sourcegitcommit: 91fdedcb190c0753180be8dc7db4b1d6da9854a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2021
ms.locfileid: "112289457"
---
# <a name="frequently-asked-questions-about-azure-spatial-anchors"></a>Häufig gestellte Fragen zu Azure Spatial Anchors

Azure Spatial Anchors ist ein verwalteter Clouddienst und eine Entwicklerplattform für Mixed Reality-Umgebungen mit mehreren Benutzern und räumlichem Bezug für HoloLens-, iOS- und Android-Geräte.

Weitere Informationen finden Sie unter [Azure Spatial Anchors-Übersicht](overview.md).

## <a name="azure-spatial-anchors-product-faqs"></a>Azure Spatial Anchors: Häufig gestellte Fragen zum Produkt

**F: Welche Geräte unterstützt Azure Spatial Anchors?**

**A:** Mit Azure Spatial Anchors können Entwickler Apps für HoloLens, iOS-Geräte mit ARKit-Unterstützung und Android-Geräte mit ARCore-Unterstützung erstellen. Für iOS und Android gilt dies sowohl für Smartphones als auch für Tablets.

**F: Muss eine Verbindung mit der Cloud bestehen, um Azure Spatial Anchors verwenden zu können?**

**A:** Für Azure Spatial Anchors ist derzeit eine Netzwerkverbindung mit dem Internet erforderlich. Wir freuen uns über Ihre Kommentare auf unserer [Website für Feedback](https://feedback.azure.com/forums/919252-azure-spatial-anchors).

**F: Welche Konnektivitätsanforderungen gelten für Azure Spatial Anchors?**

**A:** Azure Spatial Anchors funktioniert mit WLAN- und mobile Breitbandverbindungen.

**F: Wie präzise können mit Azure Spatial Anchors Anker gefunden werden?**

**A:** Es gibt viele Faktoren, die sich auf die Präzision beim Auffinden von Ankern auswirken: Lichtverhältnisse, Objekte in der Umgebung und sogar die Oberfläche, auf der der Anker angeordnet ist. Gehen Sie wie folgt vor, um zu ermitteln, ob die Präzision für Ihre Zwecke ausreicht: Probieren Sie die Anker in Umgebungen aus, die für die später verwendeten Umgebungen repräsentativ sind. Falls Umgebungen vorhanden sind, in denen die Präzision Ihre Anforderungen nicht erfüllt, helfen Ihnen die Informationen unter [Logging and diagnostics in Azure Spatial Anchors](./concepts/logging-diagnostics.md) (Protokollierung und Diagnose in Azure Spatial Anchors) weiter.

**F: Wie lange dauert es, Anker zu erstellen und zu finden?**

**A:** Die Dauer für das Erstellen und Finden von Ankern hängt von vielen Faktoren ab: der Netzwerkverbindung, den Verarbeitungseigenschaften und der Last des Geräts sowie der konkreten Umgebung. Unsere Kunden entwickeln Anwendungen in vielen Branchen, z. B. Fertigung, Einzelhandel und Gaming. Dies verdeutlicht, dass der Dienst in all diesen Fällen die Schaffung einer hervorragenden Benutzerumgebung für das jeweilige Szenario ermöglicht.

## <a name="privacy-faq"></a>Datenschutz: Häufig gestellte Fragen

**F: Wie speichert Azure Spatial Anchors Daten?**

**A:** Alle Daten werden verschlüsselt mit einem von Microsoft verwalteten Datenverschlüsselungsschlüssel gespeichert, und alle Daten werden für jede der Ressourcen regional gespeichert.

**F: Wo speichert Azure Spatial Anchors Daten?**

**A:** Mit Azure Spatial Anchors-Konten können Sie die Region angeben, in der Ihre Daten gespeichert werden. Microsoft kann Daten aus Resilienzgründen in anderen Regionen replizieren, die Daten verlassen aber niemals die geografische Region. Diese Daten werden in der Region gespeichert, in der das Azure Spatial Anchors-Konto konfiguriert wird. Wenn das Konto beispielsweise in der Region „USA, Osten“ registriert ist, werden diese Daten in der Region „USA, Osten“ gespeichert, können jedoch in einer anderen Region in der Geografie „Nordamerika“ repliziert werden, um Resilienz zu gewährleisten.

**F: Welche Informationen zu einer Umgebung werden bei Verwendung von Azure Spatial Anchors für den Dienst übertragen und gespeichert? Werden Bilder der Umgebung übertragen und gespeichert?**

**A:** Beim Erstellen oder Suchen nach Ankern werden Bilder der Umgebung auf dem Gerät in einem abgeleiteten Format verarbeitet. Dieses abgeleitete Format wird an den Dienst übertragen und darunter gespeichert.

Um dies transparenter darzustellen, ist unten eine Abbildung einer Umgebung und der abgeleiteten Punktwolke mit den Verknüpfungspunkten angegeben. Die Punktwolke veranschaulicht die geometrische Darstellung der Umgebung, die übertragen und unter dem Dienst gespeichert wird. Für jeden Punkt der Punktwolke mit den Verknüpfungspunkten übertragen und speichern wir jeweils einen Hash mit seinen visuellen Merkmalen. Der Hash wird von den Pixeldaten abgeleitet, aber diese sind nicht im Hash enthalten.

Für Azure Spatial Anchors werden [Servicevertrag & Bestimmungen](https://go.microsoft.com/fwLink/?LinkID=522330&amp;amp;clcid=0x9) und die [Datenschutzerklärung von Microsoft](https://go.microsoft.com/fwlink/?LinkId=521839&amp;clcid=0x409) eingehalten.

![Umgebung und abgeleitete Punktwolke mit Verknüpfungspunkten](./media/sparse-point-cloud.png)
*Abbildung 1: Umgebung und abgeleitete Punktwolke mit Verknüpfungspunkten*

**F: Gibt es eine Möglichkeit zum Senden von Diagnoseinformationen an Microsoft?**

**A:** Ja. Azure Spatial Anchors verfügt über einen Diagnosemodus, den Entwickler über die Azure Spatial Anchors-API nutzen können. Dies ist beispielsweise hilfreich, wenn Sie in einer Umgebung Anker nicht auf vorhersagbare Weise erstellen und finden können. Unter Umständen werden Sie von uns gefragt, ob Sie einen Diagnosebericht mit Informationen übermitteln können, die für den Debugvorgang nützlich sind. Weitere Informationen finden Sie unter [Logging and diagnostics in Azure Spatial Anchors](./concepts/logging-diagnostics.md) (Protokollierung und Diagnose in Azure Spatial Anchors).

## <a name="privacy-faq-for-developers"></a>Häufig gestellte Fragen zum Datenschutz (für Entwickler)

**F: Wenn meine Anwendung einen räumlichen Anker an einem bestimmten Ort platziert, haben dann alle Apps Zugriff darauf?**

**A:** Anker liegen jeweils nach Azure-Konto isoliert vor. Nur Apps, denen Sie Zugriff auf Ihr Konto gewähren, können im Konto auf Anker zugreifen.

**F: Welche Geschäftsbedingungen gelten, wenn ASA in meiner App oder meinem SDK verwendet wird?**

**A:** Es gelten diese [Bedingungen](https://www.nuget.org/packages/Microsoft.Azure.SpatialAnchors.WinRT/2.9.0/License) sowie alle in diese Lizenz eingebetteten Bedingungen.

### <a name="i-want-to-release-an-app-or-sdk-that-includes-asa"></a>Ich möchte eine App oder ein SDK mit ASA veröffentlichen.
**F: Muss ich hierfür weiteren Bedingungen zustimmen?**

**A:** Nein. Durch die Nutzung von ASA stimmen Sie den Nutzungsbedingungen, die unter dem obigen Link angegeben sind, und den eingebetteten Bedingungen zu. Es sind keine weiteren Bedingungen erforderlich.

**F: Besteht von Microsoft aus die Anforderung, dass die Nutzung von ASA in meiner Anwendung bzw. meinem SDK für meine Benutzer offengelegt werden muss?**

**A:** Microsoft verlangt keine Offenlegung, sofern dies nicht im Rahmen Ihrer vor Ort geltenden Gesetze erforderlich ist, um Datenschutzbestimmungen oder andere Anforderungen zu erfüllen.  Ermitteln Sie zusammen mit Ihren rechtlichen Beratern, ob die Offenlegung der ASA-Nutzung oder der Datenschutzregelungen von Microsoft aufgrund der für Sie geltenden Gesetze vor Ort erforderlich ist.

**F: Müssen meine Benutzer bestimmten Bedingungen zustimmen?**

**A:** Nein. Die Vertragsbeziehung besteht zwischen Microsoft und dem Entwickler der App bzw. des SDK.  Ermitteln Sie zusammen mit Ihren rechtlichen Beratern, ob die Zustimmung zur Nutzung von ASA oder der Datenschutzregelungen von Microsoft aufgrund der für Sie vor Ort geltenden Gesetze erforderlich ist.

**F: Ich möchte meine Benutzer darüber informieren, dass ASA für meine App bzw. mein SDK genutzt wird. Wie lautet hierfür die empfohlene Vorgehensweise?**

**A:** „Für diese App wird Azure Spatial Anchors von Microsoft genutzt.  Weitere Informationen zu Azure Spatial Anchors finden Sie unter [Azure Spatial Anchors | Microsoft Azure](https://azure.microsoft.com/en-us/services/spatial-anchors/).“

## <a name="availability-and-pricing-faqs"></a>Verfügbarkeit und Preise: Häufig gestellte Fragen

**F: Wird eine Vereinbarung zum Servicelevel (SLA) bereitgestellt?**

**A:** Wie bei Azure-Diensten üblich, streben wir eine Verfügbarkeit von über 99,9 % an. 

**F: Kann ich meine Apps mit Azure Spatial Anchors in App Stores veröffentlichen? Kann ich Azure Spatial Anchors für unternehmenskritische Produktionsszenarien nutzen?**

**A:** Ja, Azure Spatial Anchors ist allgemein verfügbar und verfügt über eine Standard-SLA für Azure-Dienste. Gerne können Sie Apps für Ihre Produktionsbereitstellungen entwickeln und uns Ihr [Feedback](https://feedback.azure.com/forums/919252-azure-spatial-anchors) zum Produkt mitteilen.

**F: Gelten bestimmte Drosselungslimits?**

**A:** Ja, es gelten Drosselungslimits.  Es ist nicht zu erwarten, dass Sie diese Limits bei typischen Vorgängen der Anwendungsentwicklung und beim Testen überschreiten. Für Produktionsbereitstellungen haben wir das Ziel, die hohen Skalierungsanforderungen unserer Kunden zu unterstützen. [Nehmen Sie Kontakt mit uns auf](mailto:azuremrscontact@microsoft.com), um dies mit uns zu besprechen.

**F: In welchen Regionen ist Azure Spatial Anchors verfügbar?**

**A:** Azure Spatial Anchors ist derzeit in den folgenden Regionen verfügbar: „USA, Westen 2“, „USA, Osten“, „USA, Osten 2“, „USA, Süden-Mitte“, „Europa, Westen“, „Europa, Norden“, „Vereinigtes Königreich, Süden“, „Australien, Osten“, „Asien, Südosten“ und „Südkorea, Mitte“. Weitere Regionen werden im Laufe der Zeit verfügbar.

Dies bedeutet, dass sich sowohl die Compute- als auch die Speicherressourcen dieses Diensts in diesen Regionen befinden. Es gelten aber keine Einschränkungen in Bezug darauf, wo sich Ihre Clients befinden. 

**F: Fallen für Azure Spatial Anchors Gebühren an?**

**A:** Ausführliche Informationen zu den geltenden Preisen finden Sie auf unserer [Seite mit der Preisübersicht](https://azure.microsoft.com/pricing/details/spatial-anchors/).

## <a name="technical-faqs"></a>Häufig gestellte Fragen: Technik

**F: Wie funktioniert Azure Spatial Anchors?**

**A:** Azure Spatial Anchors basiert auf der Mixed Reality/Augmented Reality-Nachverfolgung. Diese Nachverfolgungsmodule (Tracker) überwachen die Umgebung mit Kameras und verfolgen das Gerät mit „6DoF“ (6-degrees-of-freedom) nach, während es sich durch den Raum bewegt.

Bei Verwendung eines 6DoF-Trackers als Baustein können Sie mit Azure Spatial Anchors in Ihren realen Umgebungen bestimmte wichtige Punkte als „Ankerpunkte“ festlegen. Beispielsweise können Sie einen Anker verwenden, um Inhalt an einem bestimmten Ort in der realen Welt zu rendern.

Wenn Sie einen Anker erstellen, erfasst das Client-SDK die Umgebungsinformationen in der Nähe dieses Punkts und überträgt sie an den Dienst. Wenn dann ein anderes Gerät an demselben Ort nach dem Anker sucht, werden ähnliche Daten an den Dienst übertragen. Diese Daten werden mit den zuvor gespeicherten Umgebungsdaten abgeglichen. Die Position des Ankers relativ zum Gerät wird dann für die Nutzung in der Anwendung zurückgesendet.

**F: Wie kann Azure Spatial Anchors mit ARKit und ARCore unter iOS und Android integriert werden?**

**A:** Azure Spatial Anchors nutzt die nativen Nachverfolgungsfunktionen von ARKit und ARCore. Darüber hinaus verfügen unsere SDKs für iOS und Android über Funktionen, z. B. die Speicherung von Ankern in einem verwalteten Clouddienst. Ihre Apps können diese Anker dann leicht wiederfinden, indem sie eine Verbindung mit dem Dienst herstellen.

**F: Wie kann Azure Spatial Anchors mit HoloLens integriert werden?**

**A:** Azure Spatial Anchors nutzt die nativen Nachverfolgungsfunktionen von HoloLens. Wir stellen ein Azure Spatial Anchors SDK zum Erstellen von Apps für HoloLens bereit. Das SDK wird mit den nativen HoloLens-Funktionen integriert und stellt zusätzliche Funktionen bereit. Mit diesen Funktionen wird es beispielsweise ermöglicht, dass App-Entwickler Anker in einem verwalteten Clouddienst speichern und Ihre Apps diese Anker wiederfinden können, indem eine Verbindung mit dem Dienst hergestellt wird.

**F: Welche Plattformen und Sprachen werden von Azure Spatial Anchors unterstützt?**

**A:** Entwickler können Apps mit Azure Spatial Anchors erstellen, indem sie vertraute Tools und Frameworks für ihr Gerät verwenden:

- Unity für HoloLens, iOS und Android
- Xamarin unter iOS und Android
- Swift oder Objective-C unter iOS
- Java oder das Android NDK unter Android
- C++/WinRT für HoloLens

[Hier finden Sie Informationen zu den ersten Entwicklungsschritten](index.yml).

**F: Ist die Nutzung mit Unreal möglich?**

**A:** Unterstützung für Unreal wird für die Zukunft in Betracht gezogen.

**F: Welche Ports und Protokolle verwendet Azure Spatial Anchors?**

**A:** Azure Spatial Anchors kommuniziert über den TCP-Port 443 mithilfe eines verschlüsselten Protokolls. Für die Authentifizierung wird [Azure Active Directory](../active-directory/index.yml) verwendet, das über HTTPS und Port 443 kommuniziert.