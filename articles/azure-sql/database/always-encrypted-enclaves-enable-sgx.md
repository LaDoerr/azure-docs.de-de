---
title: Aktivieren von Intel SGX für Always Encrypted
description: Erfahren Sie, wie Sie Intel SGX für Always Encrypted mit Secure Enclaves in Azure SQL-Datenbank aktivieren, indem Sie eine SGX-fähige Hardwaregeneration auswählen.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.devlang: ''
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
ms.reviwer: vanto
ms.date: 01/15/2021
ms.openlocfilehash: 1bfab8547f3c6cc010036170c082eecdb4e335ae
ms.sourcegitcommit: c385af80989f6555ef3dadc17117a78764f83963
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2021
ms.locfileid: "111411561"
---
# <a name="enable-intel-sgx-for-always-encrypted-for-your-azure-sql-database"></a>Aktivieren von Intel SGX für Always Encrypted für Ihre Azure SQL-Datenbank-Instanz 

[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

> [!NOTE]
> Always Encrypted mit Secure Enclaves für Azure SQL-Datenbank befindet sich derzeit in der **öffentlichen Vorschau**.

[Always Encrypted mit Secure Enclaves](/sql/relational-databases/security/encryption/always-encrypted-enclaves) in Azure SQL-Datenbank nutzt [Intel Software Guard Extensions-Enclaves (Intel SGX)](https://itpeernetwork.intel.com/microsoft-azure-confidential-computing/). Damit Intel SGX verfügbar ist, müssen für die Datenbank das [V-Kern-Modell](service-tiers-vcore.md) und die Hardwaregeneration der [DC-Serie](service-tiers-sql-database-vcore.md#dc-series) verwendet werden.

Für die Konfiguration der Hardwaregeneration der DC-Serie zur Unterstützung von Intel SGX-Enclaves ist der Azure SQL-Datenbankadministrator zuständig. Weitere Informationen finden Sie unter [Rollen und Verantwortlichkeiten beim Konfigurieren von SGX-Enclaves und von Attestation](always-encrypted-enclaves-plan.md#roles-and-responsibilities-when-configuring-sgx-enclaves-and-attestation).

> [!NOTE]
> Intel SGX ist für andere Hardwaregenerationen als die DC-Serie nicht verfügbar. So ist Intel SGX beispielsweise nicht für Gen5-Hardware oder für Datenbanken verfügbar, die das [DTU-Modell](service-tiers-dtu.md) verwenden.

> [!IMPORTANT]
> Bevor Sie die Hardwaregeneration der DC-Serie für Ihre Datenbank konfigurieren, überprüfen Sie die regionale Verfügbarkeit der DC-Serie, und stellen Sie sicher, dass die Leistungseinschränkungen kein Problem darstellen. Weitere Informationen finden Sie unter [DC-Serie](service-tiers-sql-database-vcore.md#dc-series).

Ausführliche Anweisungen zum Konfigurieren einer neuen oder vorhandenen Datenbank für die Verwendung einer bestimmten Hardwaregeneration finden Sie unter [Auswählen einer Hardwaregeneration](service-tiers-sql-database-vcore.md#selecting-a-hardware-generation).
   
## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren von Azure Attestation für Ihren Azure SQL-Datenbankserver](always-encrypted-enclaves-configure-attestation.md)

## <a name="see-also"></a>Weitere Informationen

- [Tutorial: Erste Schritte mit Always Encrypted mit Secure Enclaves in Azure SQL-Datenbank](always-encrypted-enclaves-getting-started.md)