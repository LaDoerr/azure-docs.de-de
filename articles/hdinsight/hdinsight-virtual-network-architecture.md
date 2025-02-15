---
title: Virtuelle Netzwerkarchitektur mit Azure HDInsight
description: Lernen Sie die Ressourcen kennen, die beim Erstellen eines HDInsight-Clusters in einem virtuellen Azure-Netzwerk verfügbar sind.
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/14/2020
ms.openlocfilehash: 511f1b1bba227e58303a47994d7a6f7bd8b14604
ms.sourcegitcommit: 91fdedcb190c0753180be8dc7db4b1d6da9854a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2021
ms.locfileid: "112290987"
---
# <a name="azure-hdinsight-virtual-network-architecture"></a>Virtuelle Netzwerkarchitektur mit Azure HDInsight

In diesem Artikel werden die Ressourcen erläutert, die beim Bereitstellen eines HDInsight-Clusters in einem benutzerdefinierten virtuellen Azure-Netzwerk vorhanden sind. Diese Informationen sollen Ihnen dabei helfen, Ihre lokalen Ressourcen mit Ihrem HDInsight-Cluster in Azure zu verbinden. Weitere Informationen zu virtuellen Azure-Netzwerken finden Sie unter [Was ist Azure Virtual Network?](../virtual-network/virtual-networks-overview.md).

## <a name="resource-types-in-azure-hdinsight-clusters"></a>Ressourcentypen in Azure HDInsight-Clustern

In Azure HDInsight-Clustern gibt es unterschiedliche Typen virtueller Computer bzw. Knoten. Jeder Knotentyp spielt eine Rolle beim Betrieb des Systems. In der folgenden Tabelle sind diese Knotentypen und ihre Rollen im Cluster zusammengefasst.

| type | BESCHREIBUNG |
| --- | --- |
| Hauptknoten |  Bei allen Clustertypen, mit Ausnahme von Apache Storm, hosten die Hauptknoten die Prozesse, welche die Ausführung der verteilten Anwendung verwalten. Der Hauptknoten ist auch der Knoten, zu dem Sie eine SSH-Verbindung herstellen und mit dem Sie Anwendungen ausführen können, die dann für die Ausführung mit den Clusterressourcen koordiniert werden. Die Anzahl der Hauptknoten ist bei allen Clustertypen auf zwei (2) festgelegt. |
| ZooKeeper-Knoten | Zookeeper koordiniert Aufgaben zwischen den Knoten, welche die Datenverarbeitung ausführen. Er nimmt auch die Auswahl des führenden Hauptknotens vor, und verfolgt, welcher Hauptknoten einen bestimmten Master-Dienst ausführt. Die Anzahl der ZooKeeper-Knoten ist auf drei festgelegt. |
| Workerknoten | Dies sind die Knoten, welche die Datenverarbeitungsfunktionen unterstützen. Workerknoten können dem Cluster hinzugefügt oder aus dem Cluster entfernt werden, um die Computingleistung zu skalieren und die Kosten zu verwalten. |
| Regionsknoten | Beim HBase-Clustertyp führt der Regionsknoten (auch als Datenknoten bezeichnet) den Regionsserver aus. Regionsserver stellen einen Teil der von HBase verwalteten Daten bereit und verwalten diese. Regionsknoten können dem Cluster hinzugefügt oder aus dem Cluster entfernt werden, um die Computingleistung zu skalieren und die Kosten zu verwalten.|
| Nimbusknoten | Beim Storm-Clustertyp stellt der Nimbusknoten eine dem Hauptknoten vergleichbare Funktionalität bereit. Der Nimbusknoten weist anderen Knoten im Cluster Aufgaben über den Zookeeper-Knoten zu, der die Ausführung der Storm-Topologien koordiniert. |
| Supervisorknoten | Beim Storm-Clustertyp führt der Supervisorknoten die vom Nimbus-Knoten bereitgestellten Anweisungen aus, um die Verarbeitung vorzunehmen. |

## <a name="resource-naming-conventions"></a>Konventionen für Ressourcennamen

Verwenden Sie bei der Adressierung von Knoten in Ihrem Cluster vollqualifizierte Domänennamen (FQDNs). Sie können die vollqualifizierten Namen für verschiedene Knotentypen in Ihrem Cluster mithilfe der [Ambari-API](hdinsight-hadoop-manage-ambari-rest-api.md) abrufen.

Diese FQDNs weisen das Format `<node-type-prefix><instance-number>-<abbreviated-clustername>.<unique-identifier>.cx.internal.cloudapp.net` auf.

Das `<node-type-prefix>` ist *hn* für Hauptknoten, *wn* für Workerknoten und *zn* für Zookeeperknoten.

Wenn Sie nur den Hostnamen benötigen, verwenden Sie nur den ersten Teil des FQDN: `<node-type-prefix><instance-number>-<abbreviated-clustername>`

## <a name="basic-virtual-network-resources"></a>Allgemeine virtuelle Netzwerkressourcen

Das folgende Diagramm zeigt die Platzierung von HDInsight-Knoten und Netzwerkressourcen in Azure.

:::image type="content" source="./media/hdinsight-virtual-network-architecture/hdinsight-vnet-diagram.png" alt-text="Diagramm der HDInsight-Entitäten, erstellt in einem benutzerdefinierten virtuellen Azure-Netzwerk" border="false":::

Die Standardressourcen in einem virtuellen Azure-Netzwerk umfassen die in der vorherigen Tabelle genannten Clusterknotentypen. Außerdem Netzwerkgeräte, die die Kommunikation zwischen dem virtuellen Netzwerk und externen Netzwerken unterstützen.

Die folgende Tabelle enthält die neun Clusterknoten, die erstellt werden, wenn HDInsight in einem benutzerdefinierten virtuellen Azure-Netzwerk bereitgestellt wird.

| Ressourcentyp | Vorhandene Anzahl | Details |
| --- | --- | --- |
|Hauptknoten | two |    |
|ZooKeeper-Knoten | three | |
|Workerknoten | two | Diese Anzahl kann sich je nach Clusterkonfiguration und Skalierung ändern. Für Apache Kafka sind mindestens drei Workerknoten erforderlich.  |
|Gatewayknoten | two | Gatewayknoten sind virtuelle Azure-Computer, die zwar in Azure erstellt, in Ihrem Abonnement jedoch nicht angezeigt werden. Wenden Sie sich an den Support, wenn Sie diese Knoten neu starten müssen. |

Die folgenden Netzwerkressourcen werden innerhalb des virtuellen Netzwerks, das mit HDInsight verwendet wird, automatisch erstellt:

| Netzwerkressource | Vorhandene Anzahl | Details |
| --- | --- | --- |
|Load Balancer | three | |
|Netzwerkschnittstellen | neun | Dieser Wert basiert auf einem normalen Cluster, in dem jeder Knoten eine eigene Netzwerkschnittstelle hat. Die neun Schnittstellen sind für die in der vorherigen Tabelle beschriebenen Knoten bestimmt: zwei Hauptknoten, drei Zookeeper-Knoten, zwei Workerknoten und zwei Gatewayknoten. |
|Öffentliche IP-Adressen | two |    |

## <a name="endpoints-for-connecting-to-hdinsight"></a>Endpunkte zum Herstellen einer Verbindung mit HDInsight

Sie können auf drei verschiedene Arten auf Ihren HDInsight-Cluster zugreifen:

- HTTPS-Endpunkt außerhalb des virtuellen Netzwerks in `CLUSTERNAME.azurehdinsight.net`.
- SSH-Endpunkt für die direkte Verbindung mit dem Hauptknoten in `CLUSTERNAME-ssh.azurehdinsight.net`.
- HTTPS-Endpunkt innerhalb des virtuellen Netzwerks `CLUSTERNAME-int.azurehdinsight.net`. Achten Sie auf `-int` in dieser URL. Dieser Endpunkt wird in eine private IP-Adresse im virtuellen Netzwerk aufgelöst und kann nicht über das öffentliche Internet aufgerufen werden.

Diesen drei Endpunkten wird jeweils ein Lastenausgleich zugewiesen.

Öffentliche IP-Adressen werden auch für die beiden Endpunkte bereitgestellt, die eine externe Verbindung (von außerhalb des virtuellen Netzwerks) zulassen.

1. Eine öffentliche IP-Adresse wird dem Lastenausgleich als vollständig qualifizierter Domänenname (FQDN) zugewiesen, der beim Herstellen einer Verbindung mit dem Cluster über das Internet verwendet wird `CLUSTERNAME.azurehdinsight.net`.
1. Die zweite öffentliche IP-Adresse wird für den Domänennamen verwendet, der ausschließlich für die SSH-Verbindung bestimmt ist `CLUSTERNAME-ssh.azurehdinsight.net`.

## <a name="next-steps"></a>Nächste Schritte

- [Sichern des eingehenden Datenverkehrs für HDInsight-Cluster in einem virtuellen Netzwerk mit privatem Endpunkt](https://azure.microsoft.com/blog/secure-incoming-traffic-to-hdinsight-clusters-in-a-vnet-with-private-endpoint/)
