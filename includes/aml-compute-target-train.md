---
title: Datei einfügen
description: include file
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 09/17/2020
ms.openlocfilehash: 729939f9dfe4c995f92513117d9a2631333bc19d
ms.sourcegitcommit: e1d5abd7b8ded7ff649a7e9a2c1a7b70fdc72440
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2021
ms.locfileid: "110630445"
---
**Computeziele können für mehrere Trainingsaufträge wiederverwendet werden.** Beispielsweise können Sie eine Remote-VM, die Sie an Ihren Arbeitsbereich angefügt haben, für mehrere Aufträge wiederverwenden. Verwenden Sie für Machine Learning-Pipelines den entsprechenden [Pipelineschritt](/python/api/azureml-pipeline-steps/azureml.pipeline.steps) für jedes Computeziel.

Sie können für die meisten Aufträge eine beliebige der folgenden Ressourcen als Trainingscomputeziel verwenden. Allerdings können nicht alle Ressourcen für automatisiertes maschinelles Lernen, Machine Learning-Pipelines oder den Machine Learning-Designer verwendet werden.

|Trainings&nbsp;ziele|[Automatisiertes maschinelles Lernen](../articles/machine-learning/concept-automated-ml.md) | [Machine Learning-Pipelines](../articles/machine-learning/concept-ml-pipelines.md) | [Azure Machine Learning-Designer](../articles/machine-learning/concept-designer.md)
|----|:----:|:----:|:----:|
|[Lokaler Computer](../articles/machine-learning/how-to-attach-compute-targets.md#local)| Ja | &nbsp; | &nbsp; |
|[Azure Machine Learning Compute-Cluster](../articles/machine-learning/how-to-create-attach-compute-cluster.md)| Ja | Ja | Ja |
|[Azure Machine Learning-Computeinstanz](../articles/machine-learning/how-to-create-manage-compute-instance.md) | Ja (über SDK)  | Ja |  |
|[Remote-VM](../articles/machine-learning/how-to-attach-compute-targets.md#vm) | Ja  | Ja | &nbsp; |
|[Apache Spark-Pools (Vorschauversion)](../articles/machine-learning/how-to-attach-compute-targets.md#synapse)| Ja (nur lokaler SDK-Modus) | Ja | &nbsp; |
|[Azure&nbsp;Databricks](../articles/machine-learning/how-to-attach-compute-targets.md#databricks)| Ja (nur lokaler SDK-Modus) | Ja | &nbsp; |
|[Azure Data Lake Analytics](../articles/machine-learning/how-to-attach-compute-targets.md#adla) | &nbsp; | Ja | &nbsp; |
|[Azure HDInsight](../articles/machine-learning/how-to-attach-compute-targets.md#hdinsight) | &nbsp; | Ja | &nbsp; |
|[Azure Batch](../articles/machine-learning/how-to-attach-compute-targets.md#azbatch) | &nbsp; | Ja | &nbsp; |

> [!TIP]
> Die Compute-Instanz verfügt über einen 120 GB Betriebssystemdatenträger. Wenn Ihnen der Speicherplatz ausgeht, [verwenden Sie das Terminal](../articles/machine-learning/how-to-access-terminal.md), um mindestens 1–2 GB zu löschen, bevor Sie die Compute-Instanz [beenden oder neu starten](../articles/machine-learning/how-to-create-manage-compute-instance.md#manage).
