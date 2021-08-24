---
title: Offlinebereitstellung
description: Mit der Offlinebereitstellung können Sie Containerimages aus einer privaten Containerregistrierung pullen, anstatt sie aus der Microsoft Container Registry zu pullen.
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: e81ccc8ef133cdf7c454a7a50610a1bfdf2be442
ms.sourcegitcommit: ee8ce2c752d45968a822acc0866ff8111d0d4c7f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/14/2021
ms.locfileid: "113731268"
---
# <a name="offline-deployment-overview"></a>Offlinebereitstellung: Übersicht

Normalerweise werden die Containerimages, die bei der Erstellung des Azure Arc-Datencontrollers, von verwalteten SQL-Instanzen und PostgreSQL Hyperscale-Servergruppen verwendet werden, direkt aus der Microsoft Container Registry (MCR) gepullt. In einigen Fällen wird die Umgebung, in der Sie die Bereitstellung durchführen, keine Verbindung mit der Microsoft Container Registry besitzen.  In solchen Situationen können Sie die Containerimages von einem Computer aus pullen, der Zugriff auf die Microsoft Container Registry _hat_ und sie dann markieren und in eine private Containerregistrierung pushen, mit der aus der Umgebung, in der Sie Azure Arc-fähige Datendienste bereitstellen möchten, eine Verbindung hergestellt werden _kann_.

Da für Azure Arc-fähige Datendienste monatliche Updates bereitgestellt werden und eine große Anzahl von Containerimages vorhanden ist, empfiehlt es sich, diesen Prozess des Pullens, Markierens und Pushens der Containerimages in eine private Containerregistrierung mithilfe eines Skripts durchzuführen.  Das Skript kann entweder automatisiert oder manuell ausgeführt werden.

Ein [Beispielskript](https://raw.githubusercontent.com/microsoft/azure_arc/main/arc_data_services/deploy/scripts/pull-and-push-arc-data-services-images-to-private-registry.py) finden Sie im GitHub-Repository für Azure Arc.

> [!NOTE]
> Dieses Skript erfordert die Installation von Python und der [Docker CLI](https://docs.docker.com/install/).

Das Skript fordert Sie interaktiv zur Eingabe der folgenden Informationen auf.  Alternativ können Sie, wenn das Skript ohne interaktive Eingabeaufforderungen ausgeführt werden soll, die entsprechenden Umgebungsvariablen festlegen, bevor Sie das Skript ausführen.

|Prompt|Umgebungsvariable|Hinweise|
|---|---|---|
|Quellcontainerregistrierung angeben: Drücken Sie die EINGABETASTE, um `mcr.microsoft.com` zu verwenden.|SOURCE_DOCKER_REGISTRY|Normalerweise würden Sie die Images aus der Microsoft Container Registry pullen, aber wenn Sie an einer privaten Vorschau mit einer anderen Registrierung teilnehmen, können Sie die Informationen verwenden, die Ihnen im Rahmen des Vorschauprogramms zur Verfügung gestellt werden.|
|Quellcontainerregistrierungs-Repository angeben: Drücken Sie die EINGABETASTE, um `arcdata` zu verwenden.|SOURCE_DOCKER_REPOSITORY|Wenn Sie aus der Microsoft Container Registry pullen, ist das Repository `arcdata`.|
|Benutzernamen für die Quellcontainerregistrierung angeben: Drücken Sie die EINGABETASTE, um keinen zu verwenden:|SOURCE_DOCKER_USERNAME|Geben Sie nur dann einen Wert an, wenn Sie Containerimages aus einer Quelle pullen, für die eine Anmeldung erforderlich ist.  Die Microsoft Container Registry erfordert keine Anmeldung.|
|Kennwort für die Quellcontainerregistrierung angeben: Drücken Sie die EINGABETASTE, um keins zu verwenden:|SOURCE_DOCKER_PASSWORD|Geben Sie nur dann einen Wert an, wenn Sie Containerimages aus einer Quelle pullen, für die eine Anmeldung erforderlich ist.  Die Microsoft Container Registry erfordert keine Anmeldung. Dies ist eine maskierte Kennworteingabeaufforderung.  Das Kennwort wird nicht angezeigt, wenn Sie es eingeben oder einfügen.|
|Containerimagetag für die Images in der Quelle angeben: Drücken Sie die EINGABETASTE, um „`<current monthly release tag>`“ zu verwenden:|SOURCE_DOCKER_TAG|Der Standardtagname wird monatlich aktualisiert, um den Monat und das Jahr der aktuellen Version in der Microsoft Container Registry widerzuspiegeln.|
|Geben Sie einen DNS-Namen oder eine IP-Adresse für die Zielcontainerregistrierung an:|TARGET_DOCKER_REGISTRY|Der DNS-Name oder die IP-Adresse der Zielregistrierung.  Dies ist die Registrierung, _in_ die die Images gepusht werden.|
|Zielcontainerregistrierungs-Repository angeben:|TARGET_DOCKER_REPOSITORY|Das Repository in der Zielregistrierung, in die die Images gepusht werden sollen.|
|Benutzernamen für die Zielcontainerregistrierung angeben: Drücken Sie die EINGABETASTE, um keinen zu verwenden:|TARGET_DOCKER_USERNAME|Der Benutzername, sofern vorhanden, der zum Anmelden bei der Zielcontainerregistrierung verwendet wird.|
|Kennwort für die Zielcontainerregistrierung angeben: Drücken Sie die EINGABETASTE, um keins zu verwenden:|TARGET_DOCKER_PASSWORD|Das Kennwort, falls vorhanden, das für die Anmeldung bei der Zielcontainerregistrierung verwendet wird. Dies ist eine maskierte Kennworteingabeaufforderung.  Das Kennwort wird nicht angezeigt, wenn Sie es eingeben oder einfügen.|
|Containerimagetag für die Images am Ziel angeben:|TARGET_DOCKER_TAG|Normalerweise würden Sie dasselbe Tag wie bei der Quelle verwenden, um Verwechslungen zu vermeiden.|