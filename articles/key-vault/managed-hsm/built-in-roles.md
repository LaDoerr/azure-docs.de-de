---
title: 'Integrierte Rollen der lokalen RBAC für verwaltete HSMs: Azure Key Vault | Microsoft-Dokumentation'
description: Übersicht über integrierte Rollen für verwaltete HSMs, die Benutzern, Dienstprinzipalen, Gruppen und verwalteten Identitäten zugewiesen werden können
services: key-vault
author: amitbapat
ms.service: key-vault
ms.subservice: managed-hsm
ms.topic: tutorial
ms.date: 06/01/2021
ms.author: ambapat
ms.openlocfilehash: a9594157349754c7178f3c73cfe9529ad478de4a
ms.sourcegitcommit: c385af80989f6555ef3dadc17117a78764f83963
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2021
ms.locfileid: "111411015"
---
# <a name="managed-hsm-local-rbac-built-in-roles"></a>Integrierte Rollen der lokalen RBAC für verwaltete HSMs

Die lokale RBAC für verwaltete HSMs beinhaltet mehrere integrierte Rollen. Diese Rollen können Benutzern, Dienstprinzipalen, Gruppen und verwalteten Identitäten zugewiesen werden. Damit ein Prinzipal einen Vorgang ausführen kann, müssen Sie ihm eine Rolle zuweisen, die ihn zur Ausführung der entsprechenden Vorgänge berechtigt. Mit allen diesen Rollen und Vorgängen können nur Berechtigungen für Vorgänge auf der Datenebene verwaltet werden. Wenn Sie Berechtigungen auf der Steuerungsebene für die verwaltete HSM-Ressource verwalten möchten müssen Sie die [rollenbasierte Zugriffssteuerung von Azure (Azure Role-Based Access Control, Azure RBAC)](../../role-based-access-control/overview.md) verwenden. Beispiele für Vorgänge auf der Steuerungsebene sind etwa das Erstellen eines neuen verwalteten HSM oder das Aktualisieren, Verschieben oder Löschen eines vorhandenen HSM.

## <a name="built-in-roles"></a>Integrierte Rollen

|Rollenname|BESCHREIBUNG|id|
|---|---|---|
|Managed HSM Administrator (Administrator für verwaltete HSMs)| Erteilt Berechtigungen zum Ausführen aller Vorgänge im Zusammenhang mit der Sicherheitsdomäne, der vollständigen Sicherung/Wiederherstellung und der Rollenverwaltung. Darf keine Schlüsselverwaltungsvorgänge ausführen.|a290e904-7015-4bba-90c8-60543313cdb4|
|Managed HSM Crypto Officer (Kryptoverantwortlicher für verwaltete HSMs)|Erteilt Berechtigungen zum Ausführen aller Rollenverwaltungsaufgaben, zum Löschen oder Wiederherstellen gelöschter Schlüssel und zum Exportieren von Schlüsseln. Darf keine anderen Schlüsselverwaltungsvorgänge ausführen.|515eb02d-2335-4d2d-92f2-b1cbdf9c3778|
|Managed HSM Crypto User (Kryptografiebenutzer für verwaltete HSMs)|Erteilt Berechtigungen zum Ausführen aller Schlüsselverwaltungsvorgänge mit Ausnahme von Lösch-, Wiederherstellungs- und Exportvorgängen für Schlüssel.|21dbd100-6940-42c2-9190-5d6cb909625b|
|Managed HSM Policy Administrator (Richtlinienadministrator für verwaltete HSMs)| Erteilt die Berechtigung zum Erstellen und Löschen von Rollenzuweisungen.|4bd23610-cdcf-4971-bdee-bdc562cc28e4|
|Managed HSM Crypto Auditor (Kryptografieprüfer für verwaltete HSMs)|Erteilt Leseberechtigungen zum Lesen (aber nicht Verwenden) von Schlüsselattributen.|2c18b078-7c48-4d3a-af88-5a3a1b3f82b3|
|Managed HSM Crypto Service Encryption User (Benutzer der Kryptografiedienstverschlüsselung für verwaltete HSMs)| Erteilt die Berechtigung zum Verwenden eines Schlüssels für die Dienstverschlüsselung. |33413926-3206-4cdd-b39a-83574fe37a17|
|Managed HSM Backup (Sicherung verwalteter HSMs)| Erteilt die Berechtigung zum Ausführen einer HSM-Sicherung für einen einzelnen Schlüssel oder einer vollständigen HSM-Sicherung.|7b127d3c-77bd-4e3e-bbe0-dbb8971fa7f8|

## <a name="permitted-operations"></a>Erlaubte Vorgänge
> [!NOTE]  
> - Ein „X“ gibt an, dass eine Rolle zum Ausführen der Datenaktion berechtigt ist. Eine leere Zelle bedeutet, dass die Rolle nicht zum Ausführen der Datenaktion berechtigt ist.
> - Alle Datenaktionsnamen haben das Präfix „Microsoft.KeyVault/managedHsm“. Dieses wurde in den folgenden Tabellen zur besseren Übersichtlichkeit weggelassen.
> - Alle Rollennamen haben das Suffix „für verwaltete HSMs“. Dieses wurde in der folgenden Tabelle zur besseren Übersichtlichkeit weggelassen.

|Datenaktion | Administrator | Kryptoverantwortlicher | Kryptografiebenutzer | Richtlinien-Administrator | Kryptografiedienstverschlüsselung | Backup | Kryptografieprüfer|
|---|---|---|---|---|---|---|---|
|**Verwaltung der Sicherheitsdomäne**|
/securitydomain/download/action|<center>X</center>||||||
/securitydomain/upload/action|<center>X</center>||||||
/securitydomain/upload/read|<center>X</center>||||||
/securitydomain/transferkey/read|<center>X</center>||||||
|**Schlüsselverwaltung**|
|/keys/read/action|||<center>X</center>||<center>X</center>||<center>X</center>|
|/keys/write/action|||<center>X</center>||||
|/keys/create|||<center>X</center>||||
|/keys/delete|||<center>X</center>||||
|/keys/deletedKeys/read/action||<center>X</center>|||||
|/keys/deletedKeys/recover/action||<center>X</center>|||||
|/keys/deletedKeys/delete||<center>X</center>|||||<center>X</center>|
|/keys/backup/action|||<center>X</center>|||<center>X</center>|
|/keys/restore/action|||<center>X</center>||||
|/keys/export/action||<center>X</center>|||||
|/keys/release/action|||<center>X</center>||||
|/keys/import/action|||<center>X</center>||||
|**Kryptografische Schlüsselvorgänge**|
|/keys/encrypt/action|||<center>X</center>||||
|/keys/decrypt/action|||<center>X</center>||||
|/keys/wrap/action|||<center>X</center>||<center>X</center>||
|/keys/unwrap/action|||<center>X</center>||<center>X</center>||
|/keys/sign/action|||<center>X</center>||||
|/keys/verify/action|||<center>X</center>||||
|**Rollenverwaltung**|
|/roleAssignments/read/action|<center>X</center>|<center>X</center>|<center>X</center>|<center>X</center>|||<center>X</center>
|/roleAssignments/write/action|<center>X</center>|<center>X</center>||<center>X</center>|||
|/roleAssignments/delete/action|<center>X</center>|<center>X</center>||<center>X</center>|||
|/roleDefinitions/read/action|<center>X</center>|<center>X</center>|<center>X</center>|<center>X</center>|||<center>X</center>
|/roleDefinitions/write/action|<center>X</center>|<center>X</center>||<center>X</center>|||
|/roleDefinitions/delete/action|<center>X</center>|<center>X</center>||<center>X</center>|||
|**Verwaltung der Sicherung/Wiederherstellung**|
|/backup/start/action|<center>X</center>|||||<center>X</center>|
|/backup/status/action|<center>X</center>|||||<center>X</center>|
|/restore/start/action|<center>X</center>||||||
|/restore/status/action|<center>X</center>||||||
||||||||

## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich eine Übersicht über die [rollenbasierte Zugriffssteuerung in Azure (Azure Role-Based Access Control, Azure RBAC)](../../role-based-access-control/overview.md) an.
- Sehen Sie sich ein Tutorial zur [Rollenverwaltung für verwaltetes HSM](role-management.md) an.
