---
title: Benutzerdefinierte Geomatch-Regeln für Azure Web Application Firewall (WAF)
description: Dieser Artikel enthält eine Übersicht über benutzerdefinierte Geomatch-Regeln für WAF (Web Application Firewall) in Azure Application Gateway.
services: web-application-firewall
ms.topic: article
author: vhorne
ms.service: web-application-firewall
ms.date: 07/30/2021
ms.author: victorh
ms.openlocfilehash: 42fcc0daf7fd494918d04dacf4fb65661c837acf
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122355026"
---
# <a name="geomatch-custom-rules"></a>Benutzerdefinierte Geomatch-Regeln

Benutzerdefinierte Regeln ermöglichen Ihnen, maßgeschneiderte Regeln zu erstellen, die genau die Anforderungen Ihrer Anwendungen und Sicherheitsrichtlinien erfüllen. Manchmal ist eine länder-/regionsbasierte Einschränkung des Zugriffs auf Ihre Webanwendungen wünschenswert. Wie bei allen benutzerdefinierten Regeln kann diese Logik auch mit anderen Regeln kombiniert werden, um den Anforderungen Ihrer Anwendung zu entsprechen.

Wählen Sie zum Erstellen einer benutzerdefinierten Regel für die Geofilterung im Azure-Portal einfach *Geografischer Standort* als Übereinstimmungstyp und dann die Länder bzw. Regionen aus, die Sie in Ihrer Anwendung zulassen bzw. blockieren möchten. Verwenden Sie beim Erstellen von Regeln für den geografischen Abgleich mit Azure PowerShell oder dem Azure Resource Manager die Übereinstimmungsvariable `RemoteAddr` und den Operator `Geomatch`. Weitere Informationen finden Sie unter [Konfigurieren von Web Application Firewall v2 für Application Gateway mit einer benutzerdefinierten Regel mithilfe von Azure PowerShell](configure-waf-custom-rules.md) und [Erstellen und Verwenden von benutzerdefinierten Regeln für Web Application Firewall v2 auf Application Gateway](create-custom-waf-rules.md).

## <a name="countryregion-codes"></a>Länder-/Regionscodes

Wenn Sie den Geomatch-Operator verwenden, kann es sich bei den Selektoren um einen der folgenden zweistelligen Länder-/Regionscodes handeln: 

|Länder-/Regionscode | Name des Landes / der Region |
| ----- | ----- |
| AD | Andorra |
| AE | Vereinigte Arabische Emirate|
| AF | Afghanistan|
| Verfügbarkeitsgruppe | Antigua und Barbuda|
| AL | Albanien|
| AM | Armenien|
| AO | Angola|
| AR | Argentinien|
| AS | Amerikanisch-Samoa|
| AT | Österreich|
| AU | Australien|
| RP | Aserbaidschan|
| BA | Bosnien und Herzegowina|
| BB | Barbados|
| BD | Bangladesch|
| BE | Belgien|
| BF | Burkina Faso|
| BG | Bulgarien|
| BH | Bahrain|
| BI | Burundi|
| BJ | Benin|
| BL | St. Barthélemy|
| BN | Brunei Darussalam|
| BO | Bolivien|
| BR | Brasilien|
| BS | Bahamas|
| BT | Bhutan|
| BW | Botsuana|
| BY | Belarus|
| BZ | Belize|
| CA | Canada|
| CD | Demokratische Republik Kongo|
| CF | Zentralafrikanische Republik|
| CH | Schweiz|
| CI | Côte d'Ivoire|
| CL | Chile|
| CM | Kamerun|
| CN | China|
| CO | Kolumbien|
| CR | Costa Rica|
| CU | Kuba|
| CV | Cabo Verde|
| CY | Zypern|
| CZ | Tschechische Republik|
| DE | Deutschland|
| DK | Dänemark|
| DO | Dominikanische Republik|
| DZ | Algerien|
| EC | Ecuador|
| EE | Estland|
| EG | Ägypten|
| ES | Spanien|
| ET | Äthiopien|
| FI | Finnland|
| FJ | Fidschi|
| FM | Föderierte Staaten von Mikronesien|
| BV | Frankreich|
| GB | United Kingdom|
| GE | Georgien|
| GF | Französisch-Guayana|
| GH | Ghana|
| GN | Guinea|
| GP | Guadeloupe|
| GR | Griechenland|
| GT | Guatemala|
| GY | Guayana|
| HK | Hongkong (SAR)|
| HN | Honduras|
| HR | Kroatien|
| HT | Haiti|
| HU | Ungarn|
| id | Indonesien|
| IE | Irland|
| BY | Israel|
| IN | Indien|
| IQ | Irak|
| IR | Islamische Republik Iran|
| IS | Island|
| IT | Italien|
| JM | Jamaika|
| JO | Jordan|
| JP | Japan|
| KE | Kenia|
| KG | Kirgisistan|
| KH | Kambodscha|
| KI | Kiribati|
| KN | St. Kitts und Nevis|
| KP | Demokratische Volksrepublik Korea|
| KR | Republik Korea|
| KW | Kuwait|
| HE | Kaimaninseln|
| KZ | Kasachstan|
| LA | Demokratische Volksrepublik Laos|
| LB | Libanon|
| LI | Liechtenstein|
| LK | Sri Lanka|
| LR | Liberia|
| LS | Lesotho|
| LT | Litauen|
| LU | Luxemburg|
| LV | Lettland|
| LY | Libyen |
| NI | Marokko|
| MD | Republik Moldau|
| MG | Madagaskar|
| MK | Nordmazedonien|
| ML | Mali|
| MM | Myanmar|
| BB | Mongolei|
| MO | Macau (SAR)|
| MQ | Martinique|
| MR | Mauretanien|
| MT | Malta|
| MV | Malediven|
| MW | Malawi|
| MX | Mexiko|
| MY | Malaysia|
| MZ | Mosambik|
| Nicht verfügbar | Namibia|
| NE | Niger|
| NG | Nigeria|
| NI | Nicaragua|
| NL | Niederlande|
| Nein | Norwegen|
| NP | Nepal|
| NR | Nauru|
| NZ | Neuseeland|
| OM | Oman|
| PA | Panama|
| PE | Peru|
| PH | Philippinen|
| PK | Pakistan|
| PL | Polen|
| PR | Puerto Rico|
| PT | Portugal|
| PW | Palau|
| PY | Paraguay|
| QA | Katar|
| RE | Réunion|
| RO | Rumänien|
| RS | Serbien|
| RU | Russische Föderation|
| RW | Ruanda|
| SA | Saudi-Arabien|
| SD | Sudan|
| SE | Schweden|
| SG | Singapur|
| SI | Slowenien|
| SK | Slowakei|
| SN | Senegal|
| SO | Somalia|
| SR | Surinam|
| SS | Südsudan|
| SV | El Salvador|
| SY | Arabische Republik Syrien|
| SZ | Swasiland|
| TC | Turks- und Caicosinseln|
| TG | Togo|
| TH | Thailand|
| TN | Tunesien|
| TR | Türkei|
| TT | Trinidad und Tobago|
| TW | Taiwan|
| TZ | Vereinigte Republik Tansania|
| UA | Ukraine|
| UG | Uganda|
| US | USA|
| UY | Uruguay|
| UZ | Usbekistan|
| VC | St. Vincent und die Grenadinen|
| VE | Venezuela|
| VG | Britische Jungferninseln|
| VI | Amerikanische Jungferninseln|
| VN | Vietnam|
| ZA | Südafrika|
| ZM | Sambia|
| ZW | Simbabwe|

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich mit benutzerdefinierten Regeln vertraut gemacht haben, können Sie [Ihre eigenen benutzerdefinierten Regeln erstellen](create-custom-waf-rules.md).
