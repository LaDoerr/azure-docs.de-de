---
title: Neuerungen in Azure Cache for Redis
description: Neueste Updates für Azure Cache for Redis
author: yegu-ms
ms.service: cache
ms.topic: reference
ms.date: 09/28/2020
ms.author: yegu
ms.openlocfilehash: 21ee51ac53147482210aa0bda410e11d12e5f479
ms.sourcegitcommit: c385af80989f6555ef3dadc17117a78764f83963
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2021
ms.locfileid: "111413787"
---
# <a name="whats-new-in-azure-cache-for-redis"></a>Neuerungen in Azure Cache for Redis

## <a name="azure-tls-certificate-change"></a>TLS-Zertifikatänderungen für Azure

Microsoft aktualisiert Azure-Dienste für die Verwendung von TLS-Serverzertifikaten aus einer anderen Gruppe von Zertifizierungsstellen (Certificate Authorities, CAs). Diese Änderung wird in Phasen zwischen 13. August 2020 und 26. Oktober 2020 (geschätzt) eingeführt. Diese Änderung wird in Azure vorgenommen, da [die aktuellen Zertifikate der Zertifizierungsstellen eine der grundlegenden Anforderungen des Zertifizierungsstellen-/Browserforums nicht erfüllen](https://bugzilla.mozilla.org/show_bug.cgi?id=1649951). Das Problem wurde am 1. Juli 2020 gemeldet und gilt für mehrere beliebte PKI-Anbieter (Public Key-Infrastruktur) weltweit. Die meisten TLS-Zertifikate, die heute von Azure-Diensten verwendet werden, stammen aus der *Baltimore CyberTrust Root*-PKI. Der Azure Cache for Redis-Dienst wird weiterhin mit Baltimore CyberTrust Root verkettet. Seine TLS-Serverzertifikate werden jedoch ab 12. Oktober 2020 von neuen Zwischenzertifizierungsstellen (Intermediate Certificate Authorities, ICAs) ausgestellt.

> [!NOTE]
> Diese Änderung ist auf Dienste in öffentlichen [Azure-Regionen](https://azure.microsoft.com/global-infrastructure/geographies/) beschränkt. Dies schließt unabhängige (z. B. China) oder Government Clouds aus.
>
>

### <a name="does-this-change-affect-me"></a>Betrifft mich diese Änderung?

Wir gehen davon aus, dass die meisten Kunden von Azure Cache for Redis von der Änderung nicht betroffen sind. Ihre Anwendung kann betroffen sein, wenn darin explizit eine Liste zulässiger Zertifizierungsstellen angegeben wird, ein Verfahren, das als „Certificate Pinning“ bezeichnet wird. Wenn das Zertifikat anstatt Baltimore CyberTrust Root ein Zwischen- oder Blattzertifikat als Grundlage verwendet, sollten Sie **sofort Maßnahmen ergreifen**, um die Zertifikatkonfiguration zu ändern.

Die folgende Tabelle enthält Informationen zu den Zertifikaten, die umgestellt werden. Je nachdem, welches Zertifikat Ihre Anwendung verwendet, müssen Sie es möglicherweise aktualisieren, um den Verlust der Konnektivität mit Ihrer Azure Cache for Redis-Instanz zu verhindern.

| Zertifizierungsstellentyp | Aktuell | Nach der Umstellung (12. Oktober 2020) | Aktion |
| ----- | ----- | ----- | ----- |
| Root | Fingerabdruck: d4de20d05e66fc53fe1a50882c78db2852cae474<br><br> Ablauf: Montag, 12. Mai 2025, 17:59:00<br><br> Antragstellername:<br> CN = Baltimore CyberTrust Root<br> OU = CyberTrust<br> O = Baltimore<br> C = IE | Keine Änderung | Keine |
| Zwischenzertifizierungsstellen | Fingerabdrücke:<br> CN = Microsoft IT TLS CA 1<br> Fingerabdruck: 417e225037fbfaa4f95761d5ae729e1aea7e3a42<br><br> CN = Microsoft IT TLS CA 2<br> Fingerabdruck: 54d9d20239080c32316ed9ff980a48988f4adf2d<br><br> CN = Microsoft IT TLS CA 4<br> Fingerabdruck: 8a38755d0996823fe8fa3116a277ce446eac4e99<br><br> CN = Microsoft IT TLS CA 5<br> Fingerabdruck: Ad898ac73df333eb60ac1f5fc6c4b2219ddb79b7<br><br> Ablauf: Freitag, 20. Mai 2024 17:52:38<br><br> Antragstellername:<br> OU = Microsoft IT<br> O = Microsoft Corporation<br> L = Redmond<br> S = Washington<br> C = US<br> | Fingerabdrücke:<br> CN = Microsoft RSA TLS CA 01<br> Fingerabdruck: 703d7a8f0ebf55aaa59f98eaf4a206004eb2516a<br><br> CN = Microsoft RSA TLS CA 02<br> Fingerabdruck: b0c2d2d13cdd56cdaa6ab6e2c04440be4a429c75<br><br> Ablauf: Dienstag, 8. Oktober 2024 12:00:00;<br><br> Antragstellername:<br> O = Microsoft Corporation<br> C = US<br> | Erforderlich |

### <a name="what-actions-should-i-take"></a>Welche Aktionen zieht das für mich nach sich?

Wenn Ihre Anwendung den Zertifikatspeicher des Betriebssystems verwendet oder Baltimore Root angibt, ist keine Aktion erforderlich. 

Wenn Ihre Anwendung ein Zwischen- oder Blatt-TLS-Zertifikat angibt, empfiehlt es sich, die folgenden Stämme anzuheften:

| Zertifikat | Fingerabdruck |
| ----- | ----- |
| [Baltimore Root CA](https://cacerts.digicert.com/BaltimoreCyberTrustRoot.crt) | d4de20d05e66fc53fe1a50882c78db2852cae474 |
| [Microsoft RSA Root Certificate Authority 2017](https://www.microsoft.com/pkiops/certs/Microsoft%20RSA%20Root%20Certificate%20Authority%202017.crt) | 73a5e64a3bff8316ff0edccc618a906e4eae4d74 |
| [Digicert Global Root G2](https://cacerts.digicert.com/DigiCertGlobalRootG2.crt) | df3c24f9bfd666761b268073fe06d1cc8d4f82a4 |

> [!TIP]
> Sowohl das Zwischen- als auch das Blattzertifikat wird wahrscheinlich häufig geändert. Es wird empfohlen, keine Abhängigkeit von diesen Zertifikaten zu verwenden. Heften Sie die Anwendung stattdessen an ein Stammzertifikat an, da dieses weniger häufig umgestellt wird.
>
>

Um weiterhin Zwischenzertifikate anzuheften, fügen Sie Folgendes der Liste der angehefteten Zwischenzertifikate hinzu, die nur wenige weitere Zertifikate enthält, um zukünftige Änderungen zu minimieren:

| Allgemeiner Name der Zertifizierungsstelle | Fingerabdruck |
| ----- | ----- |
| [Microsoft RSA TLS CA 01](https://www.microsoft.com/pki/mscorp/Microsoft%20RSA%20TLS%20CA%2001.crt) | 703d7a8f0ebf55aaa59f98eaf4a206004eb2516a |
| [Microsoft RSA TLS CA 02](https://www.microsoft.com/pki/mscorp/Microsoft%20RSA%20TLS%20CA%2002.crt) | b0c2d2d13cdd56cdaa6ab6e2c04440be4a429c75 |
| [Microsoft Azure TLS Issuing CA 01](https://www.microsoft.com/pkiops/certs/Microsoft%20Azure%20TLS%20Issuing%20CA%2001.cer) | 2f2877c5d778c31e0f29c7e371df5471bd673173 |
| [Microsoft Azure TLS Issuing CA 02](https://www.microsoft.com/pkiops/certs/Microsoft%20Azure%20TLS%20Issuing%20CA%2002.cer) | e7eea674ca718e3befd90858e09f8372ad0ae2aa |
| [Microsoft Azure TLS Issuing CA 05](https://www.microsoft.com/pkiops/certs/Microsoft%20Azure%20TLS%20Issuing%20CA%2005.cer) | 6c3af02e7f269aa73afd0eff2a88a4a1f04ed1e5 |
| [Microsoft Azure TLS Issuing CA 06](https://www.microsoft.com/pkiops/certs/Microsoft%20Azure%20TLS%20Issuing%20CA%2006.cer) | 30e01761ab97e59a06b41ef20af6f2de7ef4f7b0 |

Wenn Ihre Anwendung das Zertifikat im Code überprüft, müssen Sie ihn ändern, um die Eigenschaften --- beispielsweise Aussteller, Fingerabdruck --- der neu angehefteten Zertifikate zu erkennen. Diese zusätzliche Überprüfung sollte alle angehefteten Zertifikate abdecken, um für die Zukunft gerüstet zu sein.

## <a name="next-steps"></a>Nächste Schritte

Sollten Sie weitere Fragen haben, wenden Sie sich an den [Support](https://azure.microsoft.com/support/options/).  
