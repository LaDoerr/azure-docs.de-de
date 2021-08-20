---
title: 'Schnellstart: Abrufen und Verwalten von Telefonnummern mithilfe von Azure Communication Services'
description: Hier erfahren Sie, wie Sie Telefonnummern mithilfe von Azure Communication Services verwalten.
author: prakulka
manager: nmurav
services: azure-communication-services
ms.author: prakulka
ms.date: 06/30/2021
ms.topic: quickstart
ms.service: azure-communication-services
ms.custom: references_regions
zone_pivot_groups: acs-azp-java-net-python-csharp-js
ms.openlocfilehash: c03633425a962b1d67b61fc32be89d88fe08ef7b
ms.sourcegitcommit: 7d63ce88bfe8188b1ae70c3d006a29068d066287
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2021
ms.locfileid: "114448580"
---
# <a name="quickstart-get-and-manage-phone-numbers"></a>Schnellstart: Abrufen und Verwalten von Telefonnummern

[!INCLUDE [Regional Availability Notice](../../includes/regional-availability-include.md)]

[!INCLUDE [Bulk Acquisition Instructions](../../includes/phone-number-special-order.md)]

::: zone pivot="platform-azp"
[!INCLUDE [Azure portal](./includes/phone-numbers-portal.md)]
::: zone-end

::: zone pivot="programming-language-csharp"
[!INCLUDE [Azure portal](./includes/phone-numbers-net.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [Java](./includes/phone-numbers-java.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [Python](./includes/phone-numbers-python.md)]
::: zone-end

::: zone pivot="programming-language-javascript"
[!INCLUDE [JavaScript](./includes/phone-numbers-js.md)]
::: zone-end

## <a name="troubleshooting"></a>Problembehandlung

Allgemeine Fragen und Probleme:

- Der Erwerb von Telefonnummern wird nur in den USA unterstützt. Stellen Sie zum Erwerben von Telefonnummern Folgendes sicher:
  - Die zugehörige Abrechnungsadresse für das Azure-Abonnement liegt in den USA. Derzeit ist es nicht möglich, eine Ressource in ein anderes Abonnement zu verschieben.
  - Ihre Communication Services-Ressource wird am Datenstandort „USA“ bereitgestellt. Derzeit ist es nicht möglich, eine Ressource an einen anderen Datenstandort zu verschieben.

- Wenn eine Telefonnummer freigegeben wurde, kann sie bis zum Ende des Abrechnungszeitraums nicht freigegeben oder erneut erworben werden.

- Wenn eine Communication Services-Ressource gelöscht wird, werden die Telefonnummern, die dieser Ressource zugeordnet sind, gleichzeitig automatisch freigegeben.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erwerben einer Telefonnummer
> * Verwalten Ihrer Telefonnummer
> * Freigeben einer Telefonnummer

> [!div class="nextstepaction"]
> [Senden einer SMS](../telephony-sms/send.md)
> [Erste Schritte für das Tätigen von Anrufen](../voice-video-calling/getting-started-with-calling.md)
