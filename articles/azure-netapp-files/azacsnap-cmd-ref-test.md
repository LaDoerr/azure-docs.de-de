---
title: Testen des Tools für konsistente Momentaufnahmen in Azure-Anwendungen für Azure NetApp Files | Microsoft-Dokumentation
description: Erfahren Sie, wie der Testbefehl des Tools für konsistente Momentaufnahmen in Azure-Anwendungen, das Sie mit Azure NetApp Files verwenden können, ausgeführt wird.
services: azure-netapp-files
documentationcenter: ''
author: Phil-Jensen
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 08/04/2021
ms.author: phjensen
ms.openlocfilehash: eb41e1ebda2e5a14bc2987dded8948221ee93452
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122339189"
---
# <a name="test-azure-application-consistent-snapshot-tool"></a>Testen des Tools für konsistente Momentaufnahmen in Azure-Anwendungen

In diesem Artikel wird erläutert, wie der Testbefehl des Tools für konsistente Momentaufnahmen in Azure-Anwendungen, das Sie mit Azure NetApp Files verwenden können, ausgeführt wird.

## <a name="introduction"></a>Einführung

Die Konfiguration wird mit dem Befehl `azacsnap -c test` getestet.

## <a name="command-options"></a>Befehlsoptionen

Der Befehl `-c test` hat folgende Optionen:

- `--test hana` testet die Verbindung mit der SAP HANA-Instanz.

- `--test storage` testet die Kommunikation mit der zugrunde liegenden Speicherschnittstelle, indem eine temporäre Speichermomentaufnahme für alle konfigurierten `data`-Volumes erstellt wird und diese dann entfernt werden. 

- `--test all` führt die Tests `hana` und `storage` nacheinander aus.

- `[--configfile <config filename>]` ist ein optionaler Parameter, der benutzerdefinierte Konfigurationsdateinamen zulässt.

## <a name="check-connectivity-with-sap-hana-azacsnap--c-test---test-hana"></a>Testen der Konnektivität mit SAP HANA mit `azacsnap -c test --test hana`

Mit diesem Befehl wird die HANA-Konnektivität für alle HANA-Instanzen in der Konfigurationsdatei überprüft. Er stellt über den HDBuserstore eine Verbindung mit der SYSTEMDB her und ruft die SID-Informationen ab.

Mit SSL bietet dieser Befehl folgende optionale Argumente:

- `--ssl=` erzwingt eine verschlüsselte Verbindung mit der Datenbank und definiert die Verschlüsselungsmethode, die für die Kommunikation mit SAP HANA verwendet wird – `openssl` oder `commoncrypto`. Wenn dieses Argument festgelegt wird, erwartet der Befehl zwei Dateien im gleichen Verzeichnis. Diese Dateien müssen nach der entsprechenden SID benannt sein. Weitere Informationen finden Sie unter [Verwenden von SSL für die Kommunikation mit SAP HANA](azacsnap-installation.md#using-ssl-for-communication-with-sap-hana).

### <a name="output-of-the-azacsnap--c-test---test-hana-command"></a>Ausgabe des Befehls `azacsnap -c test --test hana`

```bash
azacsnap -c test --test hana
```

```output
BEGIN : Test process started for 'hana'
BEGIN : SAP HANA tests
PASSED: Successful connectivity to HANA version 2.00.032.00.1533114046
END   : Test process complete for 'hana'
```

## <a name="check-connectivity-with-storage-azacsnap--c-test---test-storage"></a>Testen der Konnektivität mit dem Speicher mit `azacsnap -c test --test storage`

Der Befehl `azacsnap` erstellt eine Momentaufnahme für alle konfigurierten Datenvolumes. Damit wird überprüft, ob auf die Volumes jeder SAP HANA-Instanz der richtige Zugriff möglich ist. Dabei wird eine temporäre Momentaufnahme für jedes Datenvolume erstellt und anschließend wieder entfernt, um den Momentaufnahmezugriff für jedes Dateisystem zu überprüfen.

### <a name="output-of-the-azacsnap--c-test---test-storage-command"></a>Ausgabe des Befehls `azacsnap -c test --test storage`

```bash
azacsnap -c test --test storage
```

```output
BEGIN : Test process started for 'storage'
BEGIN : Storage test snapshots on 'data' volumes
BEGIN : 2 task(s) to Test Snapshots for Storage Volume Type 'data'
PASSED: Task#2/2 Storage test successful for Volume
PASSED: Task#1/2 Storage test successful for Volume
END   : Storage tests complete
END   : Test process complete for 'storage'
```

## <a name="next-steps"></a>Nächste Schritte

- [Sichern von Momentaufnahmen mit dem Tool für konsistente Momentaufnahmen in Azure-Anwendungen](azacsnap-cmd-ref-backup.md)
