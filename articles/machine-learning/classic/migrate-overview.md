---
title: 'Azure Machine Learning Studio (klassisch): Migrieren zu Azure Machine Learning'
description: Hier erfahren Sie mehr über das Migrieren von Studio (klassisch) zu Microsoft Azure Machine Learning für eine modernisierte Data-Science-Plattform.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio-classic
ms.topic: how-to
author: xiaoharper
ms.author: zhanxia
ms.date: 03/08/2021
ms.openlocfilehash: 7bb7c3a2d19b4dbb84d09829e2ed181a62ef7fbe
ms.sourcegitcommit: f9e368733d7fca2877d9013ae73a8a63911cb88f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111901293"
---
# <a name="migrate-to-azure-machine-learning"></a>Migrieren zu Azure Machine Learning

In diesem Artikel erfahren Sie, wie Sie Ressourcen aus Studio (klassisch) zu Microsoft Azure Machine Learning migrieren. Zu diesem Zeitpunkt müssen Sie Ihre Experimente zum Migrieren von Ressourcen manuell neu erstellen.

Machine Learning bietet eine modernisierte Data-Science-Plattform, die Ansätze ohne Code und Code-First-Ansätze kombiniert. Weitere Informationen zu den Unterschieden zwischen Studio (klassisch) und Machine Learning finden Sie im Abschnitt [Bewerten von Machine Learning](#step-1-assess-azure-machine-learning).


## <a name="recommended-approach"></a>Empfohlene Vorgehensweise

Für das Migrieren zu Machine Learning wird der folgende Ansatz empfohlen:

> [!div class="checklist"]
> * Schritt 1: Bewerten von Machine Learning
> * Schritt 2: Erstellen eines Migrationsplans
> * Schritt 3: Erneutes Erstellen von Experimenten und Webdiensten
> * Schritt 4: Integrieren von Client-Apps
> * Schritt 5: Bereinigen von Ressourcen in Studio (klassisch)


## <a name="step-1-assess-azure-machine-learning"></a>Schritt 1: Bewerten von Machine Learning
1. Informieren Sie sich über die Vorteile, Kosten und die Architektur von [Machine Learning](https://azure.microsoft.com/services/machine-learning/).

1. Vergleichen Sie die Funktionen von Machine Learning und Studio (klassisch).

    >[!NOTE]
    > Das Feature **Designer** in Machine Learning bietet ähnliche Drag & Drop-Funktionen wie Studio (klassisch). Machine Learning bietet als Alternative jedoch auch robuste [Code-First-Workflows](../concept-model-management-and-deployment.md). Bei dieser Migrationsreihe liegt der Fokus auf dem Designer, da dieser der Funktion in Studio (klassisch) am ähnlichsten ist.

    [!INCLUDE [aml-compare-classic](../../../includes/machine-learning-compare-classic-aml.md)]

3. Vergewissern Sie sich, dass die wichtigen Module aus Studio (klassisch) im Machine Learning-Designer unterstützt werden. Weitere Informationen finden Sie unten in der Tabelle unter [Modulzuordnung zu Studio (klassisch) und dem Designer](#studio-classic-and-designer-module-mapping).

4. [Erstellen Sie einen Azure Machine Learning-Arbeitsbereich](../how-to-manage-workspace.md?tabs=azure-portal).

## <a name="step-2-create-a-migration-plan"></a>Schritt 2: Erstellen eines Migrationsplans

1. Ermitteln Sie die **Datasets**, **Modelle** und **Webdienste** aus Studio (klassisch), die Sie migrieren möchten.

1. Ermitteln Sie, welche Auswirkungen eine Migration auf Ihr Unternehmen hat.

1. Erstellen Sie einen Migrationsplan.

## <a name="step-3-rebuild-experiments-and-web-services"></a>Schritt 3: Erneutes Erstellen von Experimenten und Webdiensten

1. [Migrieren Sie Datasets zu Machine Learning.](migrate-register-dataset.md)
1. Verwenden Sie den Designer zum [erneuten Erstellen von Experimenten](migrate-rebuild-experiment.md).
1. Verwenden Sie den Designer zum [erneuten Bereitstellen von Webdiensten](migrate-rebuild-web-service.md).

    >[!NOTE]
    > Machine Learning unterstützt auch Code-First-Workflows für [Datasets](../how-to-create-register-datasets.md), [Trainings](../how-to-set-up-training-targets.md) und [Bereitstellungen](../how-to-deploy-and-where.md).

## <a name="step-4-integrate-client-apps"></a>Schritt 4: Integrieren von Client-Apps

1. Ändern Sie Clientanwendungen, die Webdienste aus Studio (klassisch) aufrufen, um Ihre neuen [Machine Learning-Endpunkte](migrate-rebuild-integrate-with-client-app.md) zu verwenden.

## <a name="step-5-cleanup-studio-classic-assets"></a>Schritt 5: Bereinigen von Ressourcen in Studio (klassisch)

1. [Bereinigen Sie Ressourcen aus Studio (klassisch)](export-delete-personal-data-dsr.md), um zusätzliche Gebühren zu vermeiden. Sie sollten Ressourcen so lange für Fallbacks aufbewahren, bis Sie die Machine Learning-Workloads überprüft haben.

## <a name="studio-classic-and-designer-module-mapping"></a>Modulzuordnung zu Studio (klassisch) und dem Designer

In der folgenden Tabelle finden Sie Informationen zu den Modulen, die Sie beim erneuten Erstellen von Experimenten aus Studio (klassisch) im Designer verwenden können.


> [!IMPORTANT]
> Der Designer implementiert Module über Open-Source-Python-Pakete anstelle von C#-Paketen wie Studio (klassisch). Aufgrund dieses Unterschieds kann die Ausgabe von Designer-Modulen leicht von den Entsprechungen in Studio (klassisch) abweichen.


|Category|Modul aus Studio (klassisch)|Designer-Ersatzmodul|
|--------------|----------------|--------------------------------------|
|Dateneingabe und -ausgabe|– Manuelle Eingabe von Daten </br> – Exportieren von Daten </br> – Importieren von Daten </br> – Laden eines trainierten Modells </br> – Entpacken von gezippten Datasets|– Manuelle Eingabe von Daten </br> – Exportieren von Daten </br> – Importieren von Daten|
|Data Format Conversions|– Konvertieren in das CSV-Format </br> – Konvertieren in ein Dataset </br> – Konvertieren in das ARFF-Format </br> – Konvertieren in das SVMLight-Format </br> – Konvertieren in das TSV-Format|– Konvertieren in das CSV-Format </br> – Konvertieren in ein Dataset|
|Datentransformation – Manipulation|– Hinzufügen von Spalten</br> – Hinzufügen von Zeilen </br> – Anwenden der SQL-Transformation </br> – Bereinigen fehlender Daten </br> – Konvertieren in Indikatorwerte </br> – Bearbeiten von Metadaten </br> – Verknüpfen von Daten </br> – Entfernen doppelter Zeilen </br> – Auswählen von Spalten im Dataset </br> – Auswählen der Spaltentransformation </br> – SMOTE </br> – Gruppieren von kategorischen Werten|– Hinzufügen von Spalten</br> – Hinzufügen von Zeilen </br> – Anwenden der SQL-Transformation </br> – Bereinigen fehlender Daten </br> – Konvertieren in Indikatorwerte </br> – Bearbeiten von Metadaten </br> – Verknüpfen von Daten </br> – Entfernen doppelter Zeilen </br> – Auswählen von Spalten im Dataset </br> – Auswählen der Spaltentransformation </br> – SMOTE|
|Datentransformation – Skalieren und Verringern |– Abschneiden von Werten </br> – Gruppieren von Daten in Containern </br> – Normalisieren von Daten </br>– Hauptkomponentenanalyse |– Abschneiden von Werten </br> – Gruppieren von Daten in Containern </br> – Normalisieren von Daten|
|Datentransformation – Beispiel und Teilen|– Partition und Beispiel </br> – Teilen von Daten|– Partition und Beispiel </br> – Teilen von Daten|
|Datentransformation – Filter |– Anwenden von Filtern </br> – FIR-Filter </br> – IIR-Filter </br> – Medianfilter </br> – Filter für den gleitenden Durchschnitt </br> – Schwellenwertfilter </br> – Benutzerdefinierter Filter||
|Datentransformation – Lernen mit Zählungen |– Erstellen von Zähltransformationen </br> – Exportieren einer Anzahltabelle </br> – Importieren einer Anzahltabelle </br> – Zusammenführen einer Zähltransformation</br>  – Ändern von Anzahltabellenparametern||
|Featureauswahl |– Filterbasierte Featureauswahl </br> – Fisher‘sche Diskriminanzfunktion  </br> – Permutation der Featurerelevanz |– Filterbasierte Featureauswahl </br>  – Permutation der Featurerelevanz|
| Modell – Klassifizierung| – Entscheidungswald mit mehreren Klassen </br> – Entscheidungsdschungel mit mehreren Klassen  </br> – Logistische Regression mit mehreren Klassen  </br>– Neuronales Netz mit mehreren Klassen  </br>– One-vs-All-Multiklasse </br>– Gemitteltes Perzeptron mit zwei Klassen </br>– Bayes Point Machine mit zwei Klassen </br>– Verstärkte Entscheidungsstruktur mit zwei Klassen  </br> – Entscheidungswald mit zwei Klassen  </br> – Entscheidungsdschungel mit zwei Klassen  </br> – Lokal tiefe SVM mit zwei Klassen </br> – Logistische Regression mit zwei Klassen  </br> – Neuronales Netz mit zwei Klassen </br> – Support Vector Machine mit zwei Klassen  | – Entscheidungswald mit mehreren Klassen </br>  – Verstärkte Entscheidungsstruktur mit mehreren Klassen  </br> – Logistische Regression mit mehreren Klassen </br> – Neuronales Netz mit mehreren Klassen </br> – One-vs-All-Multiklasse  </br> – Gemitteltes Perzeptron mit zwei Klassen  </br> – Verstärkte Entscheidungsstruktur mit zwei Klassen  </br> – Entscheidungswald mit zwei Klassen </br>– Logistische Regression mit zwei Klassen </br> – Neuronales Netz mit zwei Klassen </br>– Support Vector Machine mit zwei Klassen  |
| Modell – Clustering| – k-Means-Algorithmus| – k-Means-Algorithmus|
| Modell – Regression| – Bayes‘sche lineare Regression  </br> – Regression mit verstärkter Entscheidungsstruktur  </br>– Regression mit Entscheidungswald  </br> – Schnelle gesamtstrukturbasierte Quantilregression  </br> – Lineare Regression </br> – Regression mit neuronalem Netz </br> – Ordinale Regression bzw. Poisson-Regression| – Regression mit verstärkter Entscheidungsstruktur  </br>– Regression mit Entscheidungswald  </br> – Schnelle gesamtstrukturbasierte Quantilregression </br> – Lineare Regression  </br> – Regression mit neuronalem Netz </br> – Poisson-Regression|
| Modell – Anomalieerkennung| – Einklassige SVM  </br> – PCA-basierte Anomalieerkennung | – PCA-basierte Anomalieerkennung|
| Machine Learning – Auswerten  | – Durchführen einer Kreuzvalidierung für ein Modell  </br>– Auswerten eines Modells  </br>– Bewerten des Empfehlungsmoduls | – Durchführen einer Kreuzvalidierung für ein Modell  </br>– Auswerten eines Modells </br> – Bewerten des Empfehlungsmoduls|
| Machine Learning – Trainieren| – Sweepclustering  </br> – Trainieren eines Anomalieerkennungsmodells </br>– Trainieren eines Clusteringmodells  </br> – Trainieren des Matchbox-Empfehlungsmoduls –</br> Trainieren eines Modells  </br>– Optimieren von Modellhyperparametern| – Trainieren eines Anomalieerkennungsmodells  </br> – Trainieren eines Clusteringmodells </br> – Trainieren des Modells</br> – Trainieren des PyTorch-Modells  </br>– Trainieren des SVD-Empfehlungsmoduls  </br>– Trainieren des Wide-Learning- und Deep-Learning-Empfehlungsmoduls </br>– Optimieren von Modellhyperparametern|
| – Machine Learning – Bewerten| – Anwenden von Transformationen  </br>– Zuweisen von Daten zu Clustern  </br>– Bewerten des Matchbox-Empfehlungsmoduls </br> – Bewerten eines Modells|– Anwenden von Transformationen  </br> – Zuweisen von Daten zu Clustern </br> – Bewerten eines Bildmodells  </br> – Bewerten eines Modells </br>– Bewerten des SVD-Empfehlungsmoduls </br> – Bewerten des Wide-Learning- und Deep-Learning-Empfehlungsmoduls|
| OpenCV-Bibliotheksmodule| – Importieren von Bildern </br>– Vortrainierte Kaskadenbildklassifizierung | |
| Python-Sprachenmodule| – Ausführen von Python-Skripts| – Ausführen von Python-Skripts  </br> – Erstellen eines Python-Modells |
| R Language Modules  | – Ausführen von R-Skripts  </br> – Erstellen von R-Modellen| – Ausführen von R-Skripts|
| Statistische Funktionen | – Anwenden einer mathematischen Operation </br>– Berechnen grundlegender Statistiken  </br>– Berechnen der linearen Korrelation  </br>– Auswerten von Wahrscheinlichkeitsfunktionen  </br>– Ersetzen diskreter Werte  </br>– Zusammenfassen von Daten  </br>– Testen von Hypothesen mithilfe von t-Tests| – Anwenden einer mathematischen Operation  </br>– Zusammenfassen von Daten|
| Textanalyse| – Erkennen von Sprachen  </br>– Extrahieren von Schlüsselbegriffen aus Text  </br>– Extrahieren von N-Gramm-Features aus Text  </br>– Featurehashing </br>– Latent Dirichlet Allocation  </br>– Erkennung benannter Entitäten </br>– Vorverarbeiten von Text  </br>– Bewerten von Vowpal Wabbit-Modellen (Versionen 7 bis 10)  </br>– Bewerten von Vowpal Wabbit-Modellen (Version 8) </br>– Trainieren von Vowpal Wabbit-Modellen (Versionen 7 bis 10)  </br>– Trainieren von Vowpal Wabbit-Modellen (Version 8) |– Konvertieren von Wörtern in Vektoren </br> – Extrahieren von N-Gramm-Features aus Text </br>– Featurehashing  </br>– Latent Dirichlet Allocation </br>– Vorverarbeiten von Text  </br>– Bewerten von Vowpal Wabbit-Modellen </br> – Trainieren von Vowpal Wabbit-Modellen|
| Zeitreihe| – Anomalieerkennung in Zeitreihen | |
| Webdienst | – Eingabe </br> – Ausgabe | – Eingabe </br>  - Ausgabe|
| Maschinelles Sehen| | – Anwenden von Bildtransformationen </br> – Konvertieren in Bildverzeichnisse </br> – Initialisieren von Bildtransformationen </br> – Teilen von Bildverzeichnissen  </br> – DenseNet-Bildklassifizierung   </br>– ResNet-Bildklassifizierung |

Weitere Informationen zur Verwendung einzelner Designer-Module finden Sie unter [Algorithmen und Module – Referenz für Azure Machine Learning-Designer](../algorithm-module-reference/module-reference.md).

### <a name="what-if-a-designer-module-is-missing"></a>Was geschieht, wenn ein Designer-Modul fehlt?

Der Machine Learning-Designer umfasst die beliebtesten Module von Studio (klassisch). Außerdem sind neue Module enthalten, die die neuesten Techniken im Bereich des maschinellen Lernens nutzen. 

Wenn Ihre Migration aufgrund fehlender Module im Designer blockiert ist, können Sie uns kontaktieren, indem Sie [ein Supportticket erstellen](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="example-migration"></a>Beispielmigration

Bei der folgenden Experimentmigration werden einige der Unterschiede zwischen Studio (klassisch) und Machine Learning hervorgehoben.

### <a name="datasets"></a>Datasets

In Studio (klassisch) wurden **Datasets** in Ihrem Arbeitsbereich gespeichert und konnten nur von Studio (klassisch) verwendet werden.

![automobile-price-classic-dataset](./media/migrate-overview/studio-classic-dataset.png)

In Machine Learning werden **Datasets** im Arbeitsbereich registriert und können für alle Bereiche von Machine Learning verwendet werden. Weitere Informationen zu den Vorteilen von Machine Learning-Datasets finden Sie unter [Verweisen auf Daten im Speicher mit Datasets](../concept-data.md#reference-data-in-storage-with-datasets).

![automobile-price-aml-dataset](./media/migrate-overview/aml-dataset.png)

### <a name="pipeline"></a>Pipeline

In Studio (klassisch) enthielten **Experimente** die Verarbeitungslogik für Ihre Arbeit. Sie haben Experimente mit Drag & Drop-Modulen erstellt.

![automobile-price-classic-experiment](./media/migrate-overview/studio-classic-experiment.png)

In Machine Learning enthalten **Pipelines** die Verarbeitungslogik für Ihre Arbeit. Sie können Pipelines entweder mit Drag & Drop-Modulen oder durch Schreiben von Code erstellen.

![automobile-price-aml-pipeline](./media/migrate-overview/aml-pipeline.png)

### <a name="web-service-endpoint"></a>Webdienst-Endpunkt

Studio (klassisch) hat die **REQUEST/RESPOND-API** für die Echtzeitvorhersage und die **BATCH EXECUTION-API** für die Batchvorhersage oder das erneute Training verwendet.

![automobile-price-classic-webservice](./media/migrate-overview/studio-classic-web-service.png)

Azure Machine Learning verwendet **Echtzeitendpunkte** für Echtzeitvorhersagen und **Pipelineendpunkte** für Batchvorhersagen oder erneutes Training.

![automobile-price-aml-endpoint](./media/migrate-overview/aml-endpoint.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie mehr über die allgemeinen Anforderungen für das Migrieren zu Machine Learning erfahren. Ausführliche Informationen zu den Schritten finden Sie in den anderen Artikeln der Migrationsreihe für Studio (klassisch):

1. **Übersicht über die Migration**.
1. [Migrieren eines Datasets](migrate-register-dataset.md)
1. [Neuerstellen einer Studio (Classic)-Trainingspipeline](migrate-rebuild-experiment.md)
1. [Neuerstellen eines Studio (Classic)-Webdiensts](migrate-rebuild-web-service.md)
1. [Integrieren eines Azure Machine Learning-Webdiensts in Clientanwendungen](migrate-rebuild-integrate-with-client-app.md)
1. [Migrieren von „R-Skript ausführen“-Modulen](migrate-execute-r-script.md)