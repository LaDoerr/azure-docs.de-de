---
title: Identifizieren von ausgehenden öffentlichen IP-Adressen in Azure Spring Cloud
description: Anzeigen der statischen ausgehenden öffentlichen IP-Adressen für die Kommunikation mit externen Ressourcen, z. B. mit Datenbanken, Speicher, Key Vault usw.
author: karlerickson
ms.author: karler
ms.service: spring-cloud
ms.topic: how-to
ms.date: 09/17/2020
ms.custom: devx-track-java
ms.openlocfilehash: cc3050cb7c92e0e46c02a4a12f218364b94b2d1f
ms.sourcegitcommit: 7f3ed8b29e63dbe7065afa8597347887a3b866b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122340468"
---
# <a name="how-to-identify-outbound-public-ip-addresses-in-azure-spring-cloud"></a>Identifizieren von ausgehenden öffentlichen IP-Adressen in Azure Spring Cloud

Auf dieser Seite wird erläutert, wie Sie statische ausgehende öffentliche IP-Adressen von Azure Spring Cloud-Anwendungen anzeigen.  Öffentliche IP-Adressen werden für die Kommunikation mit externen Ressourcen verwendet, z. B. mit Datenbanken, Speicher und Schlüsseltresoren.

## <a name="how-ip-addresses-work-in-azure-spring-cloud"></a>Funktionsweise von IP-Adressen in Azure Spring Cloud

Ein Azure Spring Cloud-Dienst verfügt über mindestens eine ausgehende öffentliche IP-Adresse. Die Anzahl der ausgehenden öffentlichen IP-Adressen kann je nach Ebene und anderen Faktoren variieren.

Die ausgehenden öffentlichen IP-Adressen sind in der Regel konstant und bleiben unverändert, es gibt jedoch auch Ausnahmen.

## <a name="when-outbound-ips-change"></a>Situationen, in denen sich ausgehende IP-Adressen ändern

Jede Azure Spring Cloud-Instanz verfügt jederzeit über eine festgelegte Anzahl von ausgehenden öffentlichen IP-Adressen. Jede ausgehende Verbindung von Anwendungen (beispielsweise mit einer Back-End-Datenbank) verwendet eine der ausgehenden öffentlichen IP-Adressen als IP-Ursprungsadresse. Die IP-Adresse wird zur Laufzeit zufällig ausgewählt, sodass Ihr Back-End-Dienst seine Firewall für alle ausgehenden IP-Adressen öffnen muss.

Die Anzahl der ausgehenden öffentlichen IP-Adressen ändert sich, wenn Sie eine der folgenden Aktionen durchführen:

- Aktualisieren Ihrer Azure Spring Cloud-Instanz zwischen Ebenen.
- Einreichen eines Supporttickets für eine größere Anzahl ausgehender öffentlicher IP-Adressen aufgrund von Geschäftsanforderungen.

## <a name="find-outbound-ips"></a>Ermitteln der ausgehenden IP-Adressen

Klicken Sie im linken Navigationsbereich Ihrer Instanz auf **Netzwerk**, um die ausgehenden öffentlichen IP-Adressen zu ermitteln, die derzeit von der Dienstinstanz im Azure-Portal verwendet werden. Sie werden im Feld **Ausgehende IP-Adressen** aufgelistet.

Die gleichen Informationen erhalten Sie, indem Sie den folgenden Befehl in Cloud Shell ausführen.

```azurecli
az spring-cloud show --resource-group <group_name> --name <service_name> --query properties.networkProfile.outboundIps.publicIps --output tsv
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
* [Weitere Informationen zu verwalteten Identitäten für Azure-Ressourcen](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/active-directory/managed-identities-azure-resources/overview.md)
* [Weitere Informationen zu Key Vault in Azure Spring Cloud](./tutorial-managed-identities-key-vault.md)
