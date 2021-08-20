---
title: RpcTimeoutException für Apache Spark-Thrift – Azure HDInsight
description: Bei der Verarbeitung großer Datasets mit Apache Spark-Thrift-Server werden 502-Fehler angezeigt.
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 07/29/2019
ms.openlocfilehash: 353301b55031e1dde181cac61f47edd68cc33131
ms.sourcegitcommit: 91fdedcb190c0753180be8dc7db4b1d6da9854a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2021
ms.locfileid: "112282185"
---
# <a name="scenario-rpctimeoutexception-for-apache-spark-thrift-server-in-azure-hdinsight"></a>Szenario: RpcTimeoutException für Apache Spark-Thrift-Server in Azure HDInsight

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Verwendung von Apache Spark-Komponenten in Azure HDInsight-Clustern beschrieben.

## <a name="issue"></a>Problem

Die Spark-Anwendung löst die Ausnahme `org.apache.spark.rpc.RpcTimeoutException` mit der Meldung `Futures timed out` aus, wie im folgenden Beispiel gezeigt:

```
org.apache.spark.rpc.RpcTimeoutException: Futures timed out after [120 seconds]. This timeout is controlled by spark.rpc.askTimeout
 at org.apache.spark.rpc.RpcTimeout.org$apache$spark$rpc$RpcTimeout$$createRpcTimeoutException(RpcTimeout.scala:48)
```

Die Fehler `OutOfMemoryError` und `overhead limit exceeded` können auch in `sparkthriftdriver.log` enthalten sein, wie im folgenden Beispiel gezeigt:

```
WARN  [rpc-server-3-4] server.TransportChannelHandler: Exception in connection from /10.0.0.17:53218
java.lang.OutOfMemoryError: GC overhead limit exceeded
```

## <a name="cause"></a>Ursache

Diese Fehler werden durch fehlende Speicherressourcen während der Datenverarbeitung verursacht. Wenn der Java-Garbage Collection-Prozess startet, kann dies dazu führen, dass die Spark-Anwendung nicht mehr reagiert. Es kommt bei den Abfragen zu Timeouts, und die Verarbeitung wird beendet. Der Fehler `Futures timed out` weist auf einen überlasteten Cluster hin.

## <a name="resolution"></a>Lösung

Erhöhen Sie die Clustergröße, indem Sie zusätzliche Workerknoten hinzufügen oder die Arbeitsspeicherkapazität der vorhandenen Clusterknoten erhöhen. Sie können auch die Datenpipeline anpassen, um die Menge der gleichzeitig verarbeiteten Daten zu verringern.

`spark.network.timeout` steuert das Timeout für alle Netzwerkverbindungen. Wenn Sie das Netzwerktimeout erhöhen, wird mehr Zeit für die Beendigung einiger kritischer Vorgänge bereitgestellt – dadurch wird das Problem allerdings nicht vollständig gelöst.

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [troubleshooting next steps](../includes/hdinsight-troubleshooting-next-steps.md)]