---
title: Deaktivieren von Geosicherungen
description: Schrittanleitung zum Deaktivieren von Geosicherungen für einen dedizierten SQL-Pool (früher SQL Data Warehouse) in Azure Synapse Analytics
services: synapse-analytics
author: joannapea
manager: igorstan
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 01/06/2021
ms.author: joanpo
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 2cebee3ad9b515c6f40529fe5d25da687fd53687
ms.sourcegitcommit: 32ee8da1440a2d81c49ff25c5922f786e85109b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2021
ms.locfileid: "109786283"
---
# <a name="disable-geo-backups-for-a-dedicated-sql-pool-formerly-sql-dw-in-azure-synapse-analytics"></a>Deaktivieren von Geosicherungen für einen [dedizierten SQL-Pool (früher SQL Data Warehouse)](sql-data-warehouse-overview-what-is.md) in Azure Synapse Analytics

In diesem Artikel erfahren Sie, wie Sie Geosicherungen für Ihren dedizierten SQL-Pool (früher SQL Data Warehouse) über das Azure-Portal deaktivieren.

## <a name="disable-geo-backups-through-azure-portal"></a>Deaktivieren von Geosicherungen über das Azure-Portal

Befolgen Sie diese Schritte, um Geosicherungen für Ihren dedizierten SQL-Pool (früher SQL Data Warehouse) zu deaktivieren:

> [!NOTE]
> Wenn Sie Geosicherungen deaktivieren, können Sie Ihren dedizierten SQL-Pool (früher SQL DW) nicht mehr in einer anderen Azure-Region wiederherstellen. 
> 

1. Melden Sie sich bei Ihrem [Azure-Portal](https://portal.azure.com/)-Konto an.
1. Wählen Sie die dedizierte SQL-Pool-Ressource (früher SQL DW) aus, für die Sie Geosicherungen deaktivieren möchten. 
1. Wählen Sie im linken Navigationsbereich unter **Einstellungen** die Option **Richtlinie für die Geosicherung** aus.

   ![Deaktivieren von Geosicherungen](./media/sql-data-warehouse-restore-from-geo-backup/disable-geo-backup-1.png)

1. Wählen Sie **Deaktiviert** aus, um Geosicherungen zu deaktivieren. 

   ![Deaktivierte Geosicherung](./media/sql-data-warehouse-restore-from-geo-backup/disable-geo-backup-2.png)

1. Wählen Sie *Speichern* aus, um sicherzustellen, dass die Einstellungen gespeichert werden. 

   ![Speichern von Geosicherungseinstellungen](./media/sql-data-warehouse-restore-from-geo-backup/disable-geo-backup-3.png)

## <a name="next-steps"></a>Nächste Schritte

- [Wiederherstellen eines vorhandenen dedizierten SQL-Pools (früher SQL DW)](sql-data-warehouse-restore-active-paused-dw.md)
- [Wiederherstellen eines gelöschten dedizierten SQL-Pools (früher SQL Data Warehouse)](sql-data-warehouse-restore-deleted-dw.md)
