---
title: Grenzwerte für Azure Stack Edge Pro FPGA | Microsoft-Dokumentation
description: Hier erfahren Sie mehr über Grenzwerte und empfohlene Größen beim Bereitstellen und Betreiben von Azure Stack Edge Pro FPGA, einschließlich Grenzwerte für Dienste, Geräte und Speicher.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 10/12/2020
ms.author: alkohli
ms.openlocfilehash: 8f10b6e1a621f9851ba5654f01a621551cf25f6e
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110080954"
---
# <a name="azure-stack-edge-pro-fpga-limits"></a>Grenzwerte für Azure Stack Edge Pro FPGA

Berücksichtigen Sie beim Bereitstellen und Betreiben Ihrer Microsoft Azure Stack Edge Pro FPGA-Lösung die folgenden Grenzwerte. 

## <a name="azure-stack-edge-service-limits"></a>Grenzwerte für den Azure Stack Edge-Dienst

[!INCLUDE [data-box-edge-gateway-service-limits](../../includes/data-box-edge-gateway-service-limits.md)]

## <a name="azure-stack-edge-device-limits"></a>Grenzwerte für Azure Stack Edge-Geräte

In der folgenden Tabelle sind die für Azure Stack Edge Pro FPGA-Geräte geltenden Grenzwerte aufgeführt. 

In der folgenden Tabelle werden die für ein Azure Stack Edge-Gerät geltenden Grenzwerte beschrieben.

| BESCHREIBUNG | Wert |
|---|---|
|Anzahl der Dateien pro Gerät |100 Mio. |
|Anzahl der Freigaben pro Container |1 |
|Maximale Anzahl von Freigabeendpunkten und REST-Endpunkten pro Gerät| 24 |
|Maximale Anzahl von mehrstufigen Speicherkonten pro Gerät| 24|
|Maximale in eine Freigabe geschriebene Dateigröße| 5 TB |
|Maximale Anzahl von Ressourcengruppen pro Gerät| 800 |

## <a name="azure-storage-limits"></a>Speichergrenzwerte für Azure

[!INCLUDE [data-box-edge-gateway-storage-limits](../../includes/data-box-edge-gateway-storage-limits.md)]

## <a name="data-upload-caveats"></a>Einschränkungen beim Hochladen von Daten

[!INCLUDE [data-box-edge-gateway-storage-data-upload-caveats](../../includes/data-box-edge-gateway-storage-data-upload-caveats.md)]

## <a name="azure-storage-account-size-and-object-size-limits"></a>Größenbeschränkungen für das Azure-Speicherkonto und Objekte

[!INCLUDE [data-box-edge-gateway-storage-acct-limits](../../includes/data-box-edge-gateway-storage-acct-limits.md)]


## <a name="azure-object-size-limits"></a>Größenbeschränkungen für das Azure-Objekt

[!INCLUDE [data-box-edge-gateway-storage-object-limits](../../includes/data-box-edge-gateway-storage-object-limits.md)]

## <a name="next-steps"></a>Nächste Schritte

- [Vorbereiten der Bereitstellung von Azure Stack Edge Pro FPGA](azure-stack-edge-deploy-prep.md)
