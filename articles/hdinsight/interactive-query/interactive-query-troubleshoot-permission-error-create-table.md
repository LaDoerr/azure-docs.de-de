---
title: Fehler vom Typ „Berechtigung verweigert“ bei Apache Hive-Tabelle in Azure HDInsight
description: Fehler vom Typ „Berechtigung verweigert“ beim Erstellen einer Apache Hive-Tabelle in Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 08/09/2019
ms.openlocfilehash: bd9f4f6da3ee82947192041cefe40261a182341b
ms.sourcegitcommit: 91fdedcb190c0753180be8dc7db4b1d6da9854a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2021
ms.locfileid: "112290645"
---
# <a name="scenario-permission-denied-error-when-trying-to-create-an-apache-hive-table-in-azure-hdinsight"></a>Szenario: Fehler vom Typ „Berechtigung verweigert“ beim Erstellen einer Apache Hive-Tabelle in Azure HDInsight

Dieser Artikel beschreibt Schritte zur Problembehandlung sowie mögliche Lösungen für Probleme bei der Verwendung von Interactive Query-Komponenten in Azure HDInsight-Clustern.

## <a name="issue"></a>Problem

Beim Versuch, eine Tabelle zu erstellen, wird der folgende Fehler angezeigt:

```
java.sql.SQLException: Error while compiling statement: FAILED: HiveAccessControlException Permission denied: user [hdiuser] does not have [ALL] privilege on [wasbs://data@xxxxx.blob.core.windows.net/path/table]
```

Wenn Sie den folgenden HDFS-Speicherbefehl ausführen, wird eine ähnliche Fehlermeldung angezeigt:

```
hdfs dfs -mkdir wasbs://data@xxxxx.blob.core.windows.net/path/table
```

## <a name="cause"></a>Ursache

Ob eine Tabelle in Apache Hive erstellt werden kann, hängt von den Berechtigungen ab, die auf das Speicherkonto des Clusters angewendet wurden. Sind die Berechtigungen für das Clusterspeicherkonto nicht korrekt, können Sie keine Tabellen erstellen. Das bedeutet, dass möglicherweise trotz korrekter Ranger-Richtlinien für die Tabellenerstellung Fehler vom Typ „Berechtigung verweigert“ angezeigt werden.

## <a name="resolution"></a>Lösung

Dieses Problem ist auf unzureichende Berechtigungen für den verwendeten Speichercontainer zurückzuführen. Der Benutzer, der die Hive-Tabelle erstellt, muss über Lese-, Schreib- und Ausführungsberechtigungen für den Container verfügen. Weitere Informationen finden Sie unter [Best Practices for Hive Authorization Using Apache Ranger in HDP 2.2](https://hortonworks.com/blog/best-practices-for-hive-authorization-using-apache-ranger-in-hdp-2-2/) (Bewährte Methoden für die Hive-Autorisierung mit Apache Ranger in HDP 2.2).

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [troubleshooting next steps](../includes/hdinsight-troubleshooting-next-steps.md)]