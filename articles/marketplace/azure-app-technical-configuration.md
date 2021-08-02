---
title: Hinzufügen von technischen Details für ein Azure-Anwendungsangebot
description: Fügen Sie technische Details für ein Azure-Anwendungsangebot in Partner Center (Azure Marketplace) hinzu.
author: aarathin
ms.author: aarathin
ms.reviewer: dannyevers
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
ms.date: 06/01/2021
ms.openlocfilehash: 373dd2c3f0b107b0a910af138570c9cf1782e68c
ms.sourcegitcommit: bd65925eb409d0c516c48494c5b97960949aee05
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/06/2021
ms.locfileid: "111542470"
---
# <a name="add-technical-details-for-an-azure-application-offer"></a>Hinzufügen von technischen Details für ein Azure-Anwendungsangebot

In diesem Artikel wird beschrieben, wie technische Details eingegeben werden, die es ermöglichen, eine Verbindung zwischen dem kommerziellen Microsoft-Marketplace und Ihrer Lösung herzustellen. Durch diese Verbindung können wir Ihr Angebot für den Kunden bereitstellen, wenn er es kaufen und verwalten möchte.

Führen Sie die Schritte in diesem Abschnitt nur aus, wenn Ihr Angebot eine verwaltete Anwendung enthält, die Messungsereignisse mithilfe der [APIs für getaktete Abrechnung im Marketplace](marketplace-metering-service-apis.md) ausgibt, sowie einen Dienst, der mit einem Azure AD-Sicherheitstoken authentifiziert wird. Weitere Informationen zu den verschiedenen Authentifizierungsoptionen finden Sie unter [Authentifizierungsstrategien für den Marketplace-Messungsdienst](marketplace-metering-service-authentication.md).

## <a name="technical-configuration-offer-level"></a>Technische Konfiguration (Angebotsebene)

Die Registerkarte **Technische Konfiguration** gilt für Sie nur, wenn Sie eine verwaltete Anwendung erstellen, die Messungsereignisse mithilfe der [APIs für getaktete Abrechnung im Marketplace](marketplace-metering-service-apis.md) ausgibt. Führen Sie in diesem Fall die folgenden Schritte aus. Fahren Sie andernfalls mit [Nächste Schritte](#next-steps) fort. 

Weitere Informationen zu diesen Feldern finden Sie unter [Planen eines Azure-Anwendungsangebots für den kommerziellen Marketplace](plan-azure-application-offer.md#technical-configuration).

1. Geben Sie auf der Registerkarte **Technische Konfiguration** die **Azure Active Directory-Mandanten-ID** und die **Azure Active Directory-Anwendungs-ID** an. Damit wird überprüft, ob die Verbindung zwischen den beiden Diensten eine authentifizierte Kommunikation bietet.

1. Wählen Sie **Entwurf speichern** aus, bevor Sie mit der nächsten Registerkarte fortfahren: Planübersicht

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen von Plänen für dieses Angebot](azure-app-plans.md)
