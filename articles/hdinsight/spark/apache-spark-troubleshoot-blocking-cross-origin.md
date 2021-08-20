---
title: 'Jupyter 404-Fehler: „Blockieren der ursprungsübergreifenden API“ – Azure HDInsight'
description: Fehler 404 ("Nicht gefunden") auf Jupyter-Server aufgrund von blockierender ursprungsübergreifender API in Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 07/29/2019
ms.openlocfilehash: dc64a375f9010e48403493c1b13efa1e4f551bce
ms.sourcegitcommit: 91fdedcb190c0753180be8dc7db4b1d6da9854a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2021
ms.locfileid: "112286811"
---
# <a name="scenario-jupyter-server-404-not-found-error-due-to-blocking-cross-origin-api-in-azure-hdinsight"></a>Szenario: Fehler 404 („Nicht gefunden“) auf Jupyter-Server aufgrund von blockierender ursprungsübergreifender API in Azure HDInsight

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Verwendung von Apache Spark-Komponenten in Azure HDInsight-Clustern beschrieben.

## <a name="issue"></a>Problem

Wenn Sie auf den Jupyter-Dienst in HDInsight zugreifen, wird Ihnen die Fehlermeldung „Nicht gefunden“ angezeigt. Wenn Sie die Jupyter-Protokolle überprüfen, sehen Sie dort eine Ausgabe ähnlich der Folgenden:

```log
[W 2018-08-21 17:43:33.352 NotebookApp] 404 PUT /api/contents/PySpark/notebook.ipynb (10.16.0.144) 4504.03ms referer=https://pnhr01hdi-corpdir.msappproxy.net/jupyter/notebooks/PySpark/notebook.ipynb
Blocking Cross Origin API request.  
Origin: https://xxx.xxx.xxx, Host: pnhr01.j101qxjrl4zebmhb0vmhg044xe.ax.internal.cloudapp.net:8001
```

Möglicherweise finden Sie im Jupyter-Protokoll im Feld „Origin“ auch eine IP-Adresse.

## <a name="cause"></a>Ursache

Dieser Fehler kann verschiedene Ursachen haben:

- Sie haben Netzwerksicherheitsgruppen-Regeln (Network Security Group, NSG) konfiguriert, die den Zugriff auf den Cluster einschränken. Auch wenn Sie den Zugriff mithilfe von NSG-Regeln eingeschränkt haben, können Sie weiterhin direkt auf Apache Ambari und andere Dienste zugreifen, indem Sie die IP-Adresse anstelle des Clusternamens verwenden. Beim Zugriff auf Jupyter wird dennoch möglicherweise der Fehler 404 („Nicht gefunden“) angezeigt.

- Sie haben für Ihr HDInsight-Gateway einen benutzerdefinierten DNS-Namen verwendet, der vom Standardformat `xxx.azurehdinsight.net` abweicht.

## <a name="resolution"></a>Lösung

1. Ändern Sie die jupyter.py-Dateien an den folgenden beiden Stellen:

    ```bash
    /var/lib/ambari-server/resources/common-services/JUPYTER/1.0.0/package/scripts/jupyter.py
    /var/lib/ambari-agent/cache/common-services/JUPYTER/1.0.0/package/scripts/jupyter.py
    ```

1. Suchen Sie die Zeile mit `NotebookApp.allow_origin='\"https://{2}.{3}\"'`, und ändern Sie sie in `NotebookApp.allow_origin='\"*\"'`.

1. Starten Sie den Jupyter-Dienst über Ambari neu.

1. Wenn Sie `ps aux | grep jupyter` an der Eingabeaufforderung eingeben, sollte angezeigt werden, dass das Herstellen einer Verbindung über eine beliebige URL zulässig ist.

Dies ist weniger sicher als die vorherige Einstellung. Es wird jedoch davon ausgegangen, dass der Zugriff auf den Cluster eingeschränkt ist und dass das Herstellen einer Verbindung mit dem Cluster von außen zulässig ist, weil eine NSG eingerichtet ist.

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [troubleshooting next steps](../includes/hdinsight-troubleshooting-next-steps.md)]