---
title: Konfigurieren der GPU-Überwachung mit Container Insights | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie die Überwachung von Kubernetes-Clustern mit GPU-fähigen Knoten von NVIDIA und AMD mit Container Insights konfigurieren können.
ms.topic: conceptual
ms.date: 03/27/2020
ms.openlocfilehash: 9f18628b2814305d8629d3216bb0a0d1bab092fb
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122339428"
---
# <a name="configure-gpu-monitoring-with-container-insights"></a>Konfigurieren der GPU-Überwachung mit Container Insights

Ab der Agent-Version *ciprod03022019* unterstützt der integrierte Agent von Container Insights das Überwachen der GPU-Nutzung (Graphical Processing Unit) auf GPU-fähigen Kubernetes-Clusterknoten sowie das Überwachen von Pods/Containern, die GPU-Ressourcen anfordern und verwenden.

## <a name="supported-gpu-vendors"></a>Unterstützte GPU-Anbieter

Container Insights unterstützt die Überwachung von GPU-Clustern von folgenden GPU-Anbietern:

- [NVIDIA](https://developer.nvidia.com/kubernetes-gpu)

- [AMD](https://github.com/RadeonOpenCompute/k8s-device-plugin)

Container Insights beginnt automatisch mit der Überwachung der GPU-Nutzung auf Knoten sowie GPUs anfordernden Pods und Workloads, indem die folgenden Metriken in 60-Sekunden-Intervallen gesammelt und in der **InsightMetrics**-Tabelle gespeichert werden.

>[!NOTE]
>Stellen Sie nach dem Bereitstellen eines Clusters mit GPU-Knoten sicher, dass der [GPU-Treiber](../../aks/gpu-cluster.md) installiert ist, wie von AKS für die Ausführung von GPU-Workloads erforderlich. Container Insights sammelt GPU-Metriken über GPU-Treiberpods, die auf dem Knoten ausgeführt werden. 

|Metrikname |Metrikdimension (Tags) |Beschreibung |
|------------|------------------------|------------|
|containerGpuDutyCycle |container.azm.ms/clusterId, container.azm.ms/clusterName, containerName, gpuId, gpuModel, gpuVendor|Der Prozentsatz der Zeit im Verlauf des letzten Beispielzeitraums (60 Sekunden), während dessen die GPU ausgelastet war/aktiv die Verarbeitung für einen Container ausgeführt hat. Der Arbeitszyklus ist eine Zahl zwischen 1 und 100. |
|containerGpuLimits |container.azm.ms/clusterId, container.azm.ms/clusterName, containerName |In jedem Container können Grenzwerte als eine oder mehrere GPUs angegeben werden. Es ist nicht möglich, einen Bruchteil einer GPU anzufordern oder einzuschränken. |
|containerGpuRequests |container.azm.ms/clusterId, container.azm.ms/clusterName, containerName |Jeder Container kann einen oder mehrere GPUs anfordern. Es ist nicht möglich, einen Bruchteil einer GPU anzufordern oder einzuschränken.|
|containerGpumemoryTotalBytes |container.azm.ms/clusterId, container.azm.ms/clusterName, containerName, gpuId, gpuModel, gpuVendor |Menge an GPU-Arbeitsspeicher in Bytes, die für einen bestimmten Container verwendet werden kann. |
|containerGpumemoryUsedBytes |container.azm.ms/clusterId, container.azm.ms/clusterName, containerName, gpuId, gpuModel, gpuVendor |Menge an GPU-Arbeitsspeicher in Bytes, die für einen bestimmten Container verwendet wird. |
|nodeGpuAllocatable |container.azm.ms/clusterId, container.azm.ms/clusterName, gpuVendor |Anzahl von GPUs in einem Knoten, die von Kubernetes verwendet werden können. |
|nodeGpuCapacity |container.azm.ms/clusterId, container.azm.ms/clusterName, gpuVendor |Gesamtanzahl der GPUs in einem Knoten. |

## <a name="gpu-performance-charts"></a>GPU-Leistungsdiagramme 

Container Insights enthält vorkonfigurierte Diagramme für die Metriken, die weiter oben in der Tabelle als GPU-Arbeitsmappe für jeden Cluster aufgeführt sind. Unter [Berichte in Azure Monitor für Container](container-insights-reports.md) finden Sie eine Beschreibung der für Container Insights verfügbaren Arbeitsmappen.

## <a name="next-steps"></a>Nächste Schritte

- In [Verwenden von GPUs für computeintensive Workloads in Azure Kubernetes Service (AKS)](../../aks/gpu-cluster.md) erfahren Sie, wie Sie einen AKS-Cluster bereitstellen, der GPU-fähige Knoten enthält.

- Erfahren Sie mehr über [Für GPU optimierte VM-Größen](../../virtual-machines/sizes-gpu.md).

- In [Schedule GPUs](https://kubernetes.io/docs/tasks/manage-gpus/scheduling-gpus/) (Planen von GPUs)erfahren Sie mehr darüber, wie Kubernetes Experimente zur Verwaltung von GPUs auf einem oder mehreren Knoten in einem Cluster unterstützt.
