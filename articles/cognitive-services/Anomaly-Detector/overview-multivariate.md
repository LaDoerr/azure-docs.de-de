---
title: Was ist die multivariate Anomalieerkennungs-API?
titleSuffix: Azure Cognitive Services
description: Übersicht der öffentlichen Vorschau der neuen multivariaten Anomalieerkennungs-APIs.
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: conceptual
ms.date: 04/01/2021
ms.author: mbullwin
keywords: Anomalieerkennung, maschinelles Lernen, Algorithmen
ms.openlocfilehash: e913cc4738b0f58a411e80f3e2b602a072f28665
ms.sourcegitcommit: 190658142b592db528c631a672fdde4692872fd8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112005985"
---
# <a name="multivariate-time-series-anomaly-detection-public-preview"></a>Multivariate Zeitreihen-Anomalieerkennung (öffentliche Vorschau)

Die neuen **APIs für die multivariate Anomalieerkennung** erweitern die Möglichkeiten von Entwicklern durch die einfache Integration fortschrittlicher KI zur Erkennung von Anomalien in Metrikgruppen ganz ohne Machine Learning-Kenntnisse oder gekennzeichnete Daten. Abhängigkeiten und gegenseitige Wechselwirkungen zwischen bis zu 300 verschiedenen Signalen werden jetzt automatisch als Schlüsselfaktoren gewertet. Diese neue Funktion hilft Ihnen, Ihre komplexen Systeme wie Softwareanwendungen, Server, Fertigungssysteme, Raumfahrzeuge oder schlicht Ihr Unternehmen proaktiv vor Ausfällen zu schützen.

![Liniendiagramme mehrerer Zeitreihen für Variablen von: Vibration, Temperatur, Druck, Geschwindigkeit, Rotationsgeschwindigkeit. Die Anomalien sind orange hervorgehoben.](./media/multivariate-graph.png)

Stellen Sie sich 20 Sensoren eines Automotors vor, die 20 verschiedene Signale wie Vibration, Temperatur, Kraftstoffdruck usw. generieren. Die Messwerte dieser Signale geben Ihnen einzeln möglicherweise nicht allzu viel Auskunft über Probleme auf Systemebene, aber zusammen können sie die Integrität des Motors darstellen. Wenn die Interaktion dieser Signale außerhalb des üblichen Bereichs abweicht, kann das Feature zur multivariaten Anomalieerkennung die Anomalie wie ein erfahrener Experte erkennen. Die zugrunde liegenden KI-Modelle werden mit Ihren Daten trainiert und angepasst, sodass sie die besonderen Anforderungen Ihres Unternehmens verstehen. Mit den neuen APIs in der Anomalieerkennung können Entwickler jetzt problemlos die Funktionen zur Anomalieerkennung in multivariaten Zeitreihen in Predictive Maintenance-Lösungen, AIOps-Überwachungslösungen für komplexe Unternehmenssoftware oder Business Intelligence-Tools integrieren.

## <a name="when-to-use-multivariate-versus-univariate"></a>Verwendung von **multivariate** gegenüber **univariate**

Wenn Ihr Ziel die Erkennung von Anomalien gegenüber einem normalen Muster für jede einzelne Zeitreihe ist, ausgehend ausschließlich von deren eigenen Verlaufsdaten, verwenden Sie die APIs für die Erkennung von univariaten Anomalien. Beispiel: Sie möchten Anomalien im täglichen Umsatz im Ausgang von den Umsatzdaten selbst oder eine CPU-Spitze ausschließlich auf der Grundlage von CPU-Daten erkennen.

Wenn es Ihr Ziel ist, Anomalien auf der Systemebene anhand einer Gruppe von Zeitreihendaten zu erkennen, verwenden Sie die APIs für die Erkennung von multivariaten Anomalien. Insbesondere, wenn eine einzelne Zeitreihe nicht viel verrät und Sie alle Signale (eine Gruppe von Zeitreihen) holistisch betrachten müssen, um ein Problem auf Systemebene zu bestimmen. Beispiel: Sie besitzen ein teures physisches Objekt wie ein Flugzeug, Ausrüstung auf einer Ölplattform oder einen Satelliten. Jedes dieser Objekte verfügt über Dutzende oder Hunderte verschiedener Sensortypen. Sie müssten sich die Zeitreihensignale aller dieser Sensoren ansehen, um zu entscheiden, ob es ein Problem auf Systemebene gibt.

## <a name="notebook"></a>Notebook

Machen Sie sich anhand [dieses Notebooks](https://github.com/Azure-Samples/AnomalyDetector/blob/master/ipython-notebook/Multivariate%20API%20Demo%20Notebook.ipynb) mit dem Aufrufen der (multivariaten) Anomalieerkennungs-API vertraut. In dieser Jupyter Notebook-Instanz wird gezeigt, wie Sie eine API-Anforderung senden und das Ergebnis visualisieren.

Um das Notebook ausführen zu können, rufen Sie einen gültigen **Abonnementschlüssel** für die Anomalieerkennungs-API sowie einen **API-Endpunkt** ab. Fügen Sie im Notebook den gültigen Abonnementschlüssel für die Anomalieerkennungs-API zur `subscription_key`-Variablen hinzu, und ändern Sie die `endpoint`-Variable in Ihren Endpunkt.


## <a name="region-support"></a>Unterstützung für Regionen

Die öffentliche Vorschauversion der multivariaten Anomalieerkennung ist derzeit in sechs Regionen verfügbar: „USA, Westen 2“, „Europa, Westen“, „USA, Osten 2“, „USA, Süden-Mitte“, „USA, Osten“ und „Vereinigtes Königreich, Süden“.

## <a name="algorithms"></a>Algorithmen

In den folgenden technischen Dokumenten finden Sie Informationen zu den verwendeten Algorithmen:

* Blog: [Einführung der multivariaten Anomalieerkennung](https://techcommunity.microsoft.com/t5/azure-ai/introducing-multivariate-anomaly-detection/ba-p/2260679)
* Dokument: [Multivariate Zeitreihen-Anomalieerkennung über ein Graph-Aufmerksamkeitsnetzwerk](https://arxiv.org/abs/2009.02040)


> [!VIDEO https://www.youtube.com/embed/FwuI02edclQ]


## <a name="join-the-anomaly-detector-community"></a>Beitreten zur Anomalieerkennungs-Community

- Beitreten zur [Ratgebergruppe für die Anomalieerkennung auf Microsoft Teams](https://aka.ms/AdAdvisorsJoin)

## <a name="next-steps"></a>Nächste Schritte

- [Schnellstarts](./quickstarts/client-libraries-multivariate.md).
- [Bewährte Methoden:](./concepts/best-practices-multivariate.md)In diesem Artikel werden empfohlene Muster für die Verwendung mit den multivariaten APIs beschrieben.
