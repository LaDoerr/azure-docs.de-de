---
title: Grenzwerte für Azure Data Box Heavy | Microsoft-Dokumentation
description: Beschreibt systembedingte Grenzwerte und empfohlene Größen für Microsoft Azure Data Box Heavy-Komponenten und -Verbindungen.
services: databox
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: article
ms.date: 06/28/2021
ms.author: alkohli
ms.openlocfilehash: fedd7417f6f199289bfd5c6e11a1cf1bd8c98a82
ms.sourcegitcommit: 40dfa64d5e220882450d16dcc2ebef186df1699f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2021
ms.locfileid: "113038352"
---
# <a name="azure-data-box-heavy-limits"></a>Grenzwerte für Azure Data Box Heavy

Berücksichtigen Sie beim Bereitstellen und Betreiben Ihres Azure Data Box Heavy-Geräts die folgenden Grenzwerte. In der folgenden Tabelle werden die für die Data Box geltenden Einschränkungen beschrieben.


## <a name="data-box-heavy-service-limits"></a>Grenzwerte für den Data Box Heavy-Dienst

[!INCLUDE [data-box-service-limits](../../includes/data-box-service-limits.md)]

## <a name="data-box-heavy-limits"></a>Grenzwerte für Data Box Heavy

- In Data Box Heavy kann maximal 1 Milliarde Dateien pro Knoten gespeichert werden.
- Data Box Heavy unterstützt maximal 512 Container oder Freigaben pro Knoten in der Cloud. Die Verzeichnisse der obersten Ebene innerhalb der Benutzerfreigabe werden zu Containern oder Azure-Dateifreigaben in der Cloud. 

## <a name="azure-storage-limits"></a>Grenzwerte für Azure Storage

[!INCLUDE [data-box-storage-limits](../../includes/data-box-storage-limits.md)]

## <a name="data-upload-caveats"></a>Einschränkungen beim Hochladen von Daten

[!INCLUDE [data-box-data-upload-caveats](../../includes/data-box-data-upload-caveats.md)]

## <a name="azure-storage-account-size-limits"></a>Größenbeschränkungen für das Azure-Speicherkonto

[!INCLUDE [data-box-storage-account-size-limits](../../includes/data-box-storage-account-size-limits.md)]

## <a name="azure-object-size-limits"></a>Größenbeschränkungen für das Azure-Objekt

[!INCLUDE [data-box-object-size-limits](../../includes/data-box-object-size-limits.md)]

## <a name="azure-block-blob-page-blob-and-file-naming-conventions"></a>Benennungskonventionen für Azure-Blockblobs, -Seitenblobs und -Dateien

[!INCLUDE [data-box-naming-conventions](../../includes/data-box-naming-conventions.md)]
