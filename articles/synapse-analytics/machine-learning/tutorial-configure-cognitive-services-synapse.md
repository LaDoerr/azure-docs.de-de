---
title: 'Tutorial: Voraussetzungen für Cognitive Services in Azure Synapse Analytics'
description: Hier erfahren Sie, wie Sie die Voraussetzungen für die Verwendung von Cognitive Services in Azure Synapse konfigurieren.
services: synapse-analytics
ms.service: synapse-analytics
ms.subservice: machine-learning
ms.topic: tutorial
ms.reviewer: jrasnick, garye
ms.date: 11/20/2020
author: nelgson
ms.author: negust
ms.openlocfilehash: da1760def8f2c036854839787b1051e8f52358d0
ms.sourcegitcommit: ce9178647b9668bd7e7a6b8d3aeffa827f854151
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2021
ms.locfileid: "109809650"
---
# <a name="tutorial-prerequisites-for-using-cognitive-services-in-azure-synapse-analytics"></a>Tutorial: Voraussetzungen für die Verwendung von Cognitive Services in Azure Synapse Analytics

In diesem Tutorial erfahren Sie, wie Sie die Voraussetzungen für die sichere Nutzung von Azure Cognitive Services in Azure Synapse Analytics einrichten.

In diesem Lernprogramm wird Folgendes behandelt:
> [!div class="checklist"]
> - Erstellen einer Cognitive Services-Ressource wie Textanalyse oder Anomalieerkennung
> - Speichern eines Authentifizierungsschlüssel für Cognitive Services-Ressourcen als Geheimnisse in Azure Key Vault und Konfigurieren des Zugriffs für einen Azure Synapse Analytics-Arbeitsbereich
> - Erstellen eines mit Azure Key Vault verknüpften Diensts in Ihrem Azure Synapse Analytics-Arbeitsbereich

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

- [Azure Synapse Analytics-Arbeitsbereich](../get-started-create-workspace.md) mit einem als Standardspeicher konfigurierten Azure Data Lake Storage Gen2-Speicherkonto. Für das hier verwendete Azure Data Lake Storage Gen2-Dateisystem müssen Sie über die Rolle *Mitwirkender an Storage-Blobdaten* verfügen.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="create-a-cognitive-services-resource"></a>Erstellen einer Cognitive Services-Ressource

[Azure Cognitive Services](../../cognitive-services/index.yml) beinhaltet zahlreiche Typen von Diensten. Textanalyse und Anomalieerkennung sind zwei Beispiele in den Azure Synapse-Tutorials.

Sie können eine [Textanalyseressource](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics) im Azure-Portal erstellen:

![Screenshot: Textanalyse im Portal mit der Schaltfläche „Erstellen“](media/tutorial-configure-cognitive-services/tutorial-configure-cognitive-services-00b.png)

Sie können eine [Anomalieerkennungsressource](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics) im Azure-Portal erstellen:

![Screenshot: Anomalieerkennung im Portal mit der Schaltfläche „Erstellen“](media/tutorial-configure-cognitive-services/tutorial-configure-cognitive-services-00a.png)

## <a name="create-a-key-vault-and-configure-secrets-and-access"></a>Erstellen eines Schlüsseltresors und Konfigurieren von Geheimnissen und Zugriff

1. Erstellen Sie einen [Schlüsseltresor](https://ms.portal.azure.com/#create/Microsoft.KeyVault) im Azure-Portal.
2. Navigieren Sie zu **Key Vault** > **Zugriffsrichtlinien**, und gewähren Sie der [MSI des Azure Synapse-Arbeitsbereichs](../security/synapse-workspace-managed-identity.md) Berechtigungen zum Lesen von Geheimnissen aus Azure Key Vault.

   > [!NOTE]
   > Stellen Sie sicher, dass die Richtlinienänderungen gespeichert werden. Dieser Schritt ist leicht zu übersehen.

   ![Screenshot: Auswahl für das Hinzufügen einer Zugriffsrichtlinie](media/tutorial-configure-cognitive-services/tutorial-configure-cognitive-services-00c.png)

3. Navigieren Sie zu Ihrer Cognitive Services-Ressource. Navigieren Sie beispielsweise zu **Anomalieerkennung** > **Schlüssel und Endpunkt**. Kopieren Sie dann einen der beiden Schlüssel in die Zwischenablage.

4. Wechseln Sie zu **Key Vault** > **Geheimnis**, um ein neues Geheimnis zu erstellen. Geben Sie den Namen des Geheimnisses an, und fügen Sie dann den Schlüssel aus dem vorherigen Schritt in das Feld **Wert** ein. Wählen Sie abschließend **Erstellen**.

   ![Screenshot: Auswahl für die Erstellung eines Geheimnisses](media/tutorial-configure-cognitive-services/tutorial-configure-cognitive-services-00d.png)

   > [!IMPORTANT]
   > Merken oder notieren Sie sich diesen Geheimnisnamen. Sie benötigen ihn später zum Herstellen einer Verbindung mit Cognitive Services in Synapse Studio.

## <a name="create-an-azure-key-vault-linked-service-in-azure-synapse"></a>Erstellen eines mit Azure Key Vault verknüpften Diensts in Azure Synapse

1. Öffnen Sie Ihren Arbeitsbereich in Azure Synapse Studio. 
2. Navigieren Sie zu **Verwalten** > **Verknüpfte Dienste**. Erstellen Sie einen mit **Azure Key Vault** verknüpften Dienst, indem Sie auf den soeben erstellten Schlüsseltresor verweisen. 
3. Überprüfen Sie die Verbindung, indem Sie die Schaltfläche **Verbindung testen** auswählen. Ist die Verbindung grün, wählen Sie **Erstellen** und dann **Alle veröffentlichen** aus, um die Änderung zu speichern.

![Screenshot: Azure Key Vault als neuer verknüpfter Dienst](media/tutorial-configure-cognitive-services/tutorial-configure-cognitive-services-00e.png)

Sie können nun mit einem der Tutorials für die Verwendung der Cognitive Services-Benutzeroberfläche in Azure Synapse Studio fortfahren.

## <a name="next-steps"></a>Nächste Schritte

- [Tutorial: Stimmungsanalyse mit Azure Cognitive Services](tutorial-cognitive-services-sentiment.md)
- [Tutorial: Anomalieerkennung mit Azure Cognitive Services](tutorial-cognitive-services-sentiment.md)
- [Tutorial: Assistent für die Modellbewertung in dedizierten SQL-Pools von Azure Synapse](tutorial-sql-pool-model-scoring-wizard.md)
- [Machine Learning-Funktionen in Azure Synapse Analytics](what-is-machine-learning.md)
