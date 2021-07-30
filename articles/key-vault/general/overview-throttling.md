---
title: Anleitung zur Drosselung von Azure Key Vault
description: Die Key Vault-Drosselung schränkt die Anzahl gleichzeitiger Aufrufe ein, um eine übermäßige Nutzung von Ressourcen zu verhindern.
services: key-vault
author: msmbaldwin
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: mbaldwin
ms.openlocfilehash: f9e3f2095e6fd7a744769c11209ed115767c3aed
ms.sourcegitcommit: bc29cf4472118c8e33e20b420d3adb17226bee3f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2021
ms.locfileid: "113492593"
---
# <a name="azure-key-vault-throttling-guidance"></a>Anleitung zur Drosselung von Azure Key Vault

Die Drosselung ist ein Vorgang, den Sie einleiten, um die Anzahl gleichzeitiger Aufrufe für den Azure-Dienst einzuschränken und so eine übermäßige Nutzung von Ressourcen zu verhindern. Azure Key Vault (AKV) dient der Verarbeitung einer große Anzahl von Anforderungen. Sollten jedoch zu viele Anforderungen vorliegen, wird mittels Drosselung die optimale Leistung und Zuverlässigkeit des AKV-Diensts bewahrt.

Die Grenzwerte für die Drosselung sind je nach Szenario unterschiedlich. So ist eine Drosselung beispielsweise wahrscheinlicher, wenn eine Vielzahl von Schreibvorgängen durchgeführt werden, als wenn nur Lesevorgänge durchgeführt werden.

## <a name="how-does-key-vault-handle-its-limits"></a>Wie geht Key Vault mit den Grenzen um?

In Key Vault wird mithilfe von Diensteinschränkungen der Missbrauch von Ressourcen verhindert und die Qualität des Diensts für alle Clients von Key Vault sichergestellt. Wenn der Schwellenwert für einen Dienst überschritten wird, begrenzt Key Vault weitere Anforderungen von diesem Client für eine bestimmte Zeitdauer und gibt den HTTP-Statuscode 429 (Zu viele Anforderungen) zurück. Die Anforderung wird nicht ausgeführt. Anforderungen mit Fehlern, für die der Fehler 429 zurückgegeben wird, werden für den von Key Vault überwachten Drosselungsgrenzwert nicht mitgezählt. 

Key Vault wurde ursprünglich zum Speichern und Abrufen von Geheimnissen zum Zeitpunkt der Bereitstellung konzipiert.  Heute wird Key Vault aber auch zur Laufzeit zum Speichern und Abrufen von Geheimnissen verwendet. Außerdem soll Key Vault in Apps und Diensten häufig wie eine Datenbank verwendet werden.  Die aktuellen Grenzwerte unterstützen keine hohen Durchsatzraten.

Key Vault wurde ursprünglich mit den unter [Grenzwerte des Azure Key Vault-Diensts](service-limits.md) angegebenen Grenzwerten erstellt.  Zur Maximierung der Key Vault-Durchsatzraten werden hier einige empfohlene Richtlinien und bewährte Methoden genannt:
1. Stellen Sie sicher, dass eine Drosselung eingerichtet ist.  Auf dem Client müssen Richtlinien für exponentielles Backoff für 429-Fehler berücksichtigt werden. Außerdem müssen Wiederholungsversuche entsprechend der folgenden Anleitung durchgeführt werden.
1. Teilen Sie den Key Vault-Datenverkehr auf mehrere Tresore und verschiedene Regionen auf.   Verwenden Sie für jede Sicherheits- und Verfügbarkeitsdomäne einen separaten Tresor.   Bei fünf Apps in jeweils zwei Regionen empfiehlt sich die Verwendung von 10 Tresoren, die jeweils die für die App und die Region spezifischen Geheimnisse enthalten.  Für alle Transaktionstypen gilt als abonnementweiter Grenzwert das Fünffache des Grenzwerts der einzelnen Schlüsseltresore. „HSM – andere Transaktionen“ pro Abonnement ist beispielsweise auf 5.000 Transaktionen in 10 Sekunden pro Abonnent beschränkt. Sie können das Geheimnis in dem Dienst oder der App zwischenspeichern, um auch die RPS direkt im Schlüsseltresor zu reduzieren oder Datenverkehr mit Stoßzeiten zu verarbeiten.  Sie können den Datenverkehr auch zwischen verschiedenen Regionen aufteilen, sodass die Latenz minimiert und ein anderes Abonnement und sowie ein anderer Tresor verwendet werden.  Senden Sie nicht mehr Anforderungen als im Grenzwert des Abonnements angegeben an den Key Vault-Dienst in einer einzelnen Azure-Region.
1. Sie sollten die aus Azure Key Vault abgerufenen Geheimnisse im Speicher zwischenspeichern und nach Möglichkeit aus dem Speicher wiederverwenden.  Erneutes Lesen aus Azure Key Vault sollte nur dann erfolgen, wenn die zwischengespeicherte Kopie nicht mehr funktioniert (z. B. weil sie in der Quelle ausgetauscht wurde). 
1. Key Vault ist für die Geheimnisse Ihrer eigenen Dienste konzipiert.   Wenn Sie die Geheimnisse Ihrer Kunden speichern (insbesondere bei Schlüsselspeicherungen mit hohem Durchsatz), können Sie die Schlüssel in einer Datenbank oder einem Speicherkonto mit Verschlüsselung und nur den Hauptschlüssel in Azure Key Vault speichern.
1. Das Verschlüsseln, Umschließen und Überprüfen von Vorgängen mit öffentlichen Schlüsseln kann ohne Zugriff auf Key Vault erfolgen, sodass sich nicht nur das Risiko einer Drosselung verringert, sondern auch die Zuverlässigkeit verbessert (solange Sie die öffentlichen Schlüssel ordnungsgemäß zwischenspeichern).
1. Wenn Sie Key Vault zum Speichern von Anmeldeinformationen für einen Dienst verwenden, überprüfen Sie, ob der Dienst die Azure AD-Authentifizierung unterstützt, sodass die Authentifizierung direkt erfolgen kann. Dadurch wird die Last in Key Vault reduziert, die Zuverlässigkeit optimiert und der Code vereinfacht, da dann in Key Vault das Azure AD-Token verwendet werden kann.  Viele Dienste sind zur Verwendung der Azure AD-Authentifizierung übergegangen.  Die aktuelle Liste dieser Dienste finden Sie unter [Dienste, die verwaltete Identitäten für Azure-Ressourcen unterstützen](../../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-managed-identities-for-azure-resources).
1. Sie können die Auslastung und Bereitstellung über einen längeren Zeitraum staffeln, um die aktuellen RPS-Grenzwerte einzuhalten.
1. Wenn Ihre App mehrere Knoten umfasst, die die gleichen Geheimnisse lesen müssen, empfiehlt sich eine Auffächerung, bei der eine Entität das Geheimnis aus Key Vault liest und an alle anderen Knoten verteilt.   Die abgerufenen Geheimnisse sollten nur im Speicher zwischengespeichert werden.


## <a name="how-to-throttle-your-app-in-response-to-service-limits"></a>Drosseln einer App als Reaktion auf Diensteinschränkungen

Nachfolgend sind die **bewährten Methoden** aufgeführt, die Sie implementieren sollten, wenn Ihr Dienst gedrosselt ist:
- Reduzieren Sie die Anzahl von Vorgängen pro Anforderung.
- Reduzieren Sie die Häufigkeit der Anforderungen.
- Vermeiden Sie schnelle Wiederholungsversuche. 
    - Alle Anforderungen zählen zu Ihren Nutzungseinschränkungen.

Verwenden Sie zur Implementierung der Fehlerbehandlung bei Ihrer App den HTTP-Fehlercode 429, um zu ermitteln, ob eine clientseitige Drosselung erforderlich ist. Wenn die Anforderung mit dem HTTP-Fehlercode 429 erneut fehlschlägt, liegt weiterhin eine Azure-Diensteinschränkung vor. Verwenden Sie weiterhin die empfohlene clientseitige Drosselungsmethode, und wiederholen Sie die Anforderung, bis sie erfolgreich abgeschlossen wird.

Code zur Implementierung eines exponentiellen Backoffs wird unten dargestellt. 
```
SecretClientOptions options = new SecretClientOptions()
    {
        Retry =
        {
            Delay= TimeSpan.FromSeconds(2),
            MaxDelay = TimeSpan.FromSeconds(16),
            MaxRetries = 5,
            Mode = RetryMode.Exponential
         }
    };
    var client = new SecretClient(new Uri("https://keyVaultName.vault.azure.net"), new DefaultAzureCredential(),options);
                                 
    //Retrieve Secret
    secret = client.GetSecret(secretName);
```


Die Verwendung dieses Codes in einer C#-Clientanwendung ist einfach. 

### <a name="recommended-client-side-throttling-method"></a>Empfohlene Methode für die clientseitige Drosselungsmethode

Wenn der HTTP-Fehlercode 429 vorliegt, beginnen Sie die Drosselung Ihres Clients mithilfe eines exponentiellen Backoff-Konzepts:

1. 1 Sekunde warten, Anforderung wiederholen
2. Liegt weiterhin eine Drosselung vor: 2 Sekunden warten, Anforderung wiederholen
3. Liegt weiterhin eine Drosselung vor: 4 Sekunden warten, Anforderung wiederholen
4. Liegt weiterhin eine Drosselung vor: 8 Sekunden warten, Anforderung wiederholen
5. Liegt weiterhin eine Drosselung vor: 16 Sekunden warten, Anforderung wiederholen

Nun sollte der HTTP-Antwortcode 429 nicht mehr angezeigt werden.

## <a name="see-also"></a>Weitere Informationen

Ausführlichere Informationen zur Drosselung in der Microsoft Cloud finden Sie unter [Throttling Pattern (Drosselungsmuster)](/azure/architecture/patterns/throttling).
