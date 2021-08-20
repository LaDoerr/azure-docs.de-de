---
title: XML-Nachrichten und Flatfiles
description: Verarbeiten, Überprüfen und Transformieren von XML-Nachrichten in Azure Logic Apps mit Enterprise Integration Pack
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 02/27/2017
ms.openlocfilehash: c4e8f8c080d73ca77a69820ffe6af9d46d6f44af
ms.sourcegitcommit: 91fdedcb190c0753180be8dc7db4b1d6da9854a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2021
ms.locfileid: "112293993"
---
# <a name="xml-messages-and-flat-files-in-azure-logic-apps-with-enterprise-integration-pack"></a>XML-Nachrichten und Flatfiles in Azure Logic Apps mit Enterprise Integration Pack

In [Azure Logic Apps](logic-apps-overview.md) können Sie XML-Nachrichten verarbeiten, die Sie mithilfe des Enterprise Integration Packs senden und empfangen. Wenn Sie BizTalk Server verwendet haben, bietet Ihnen das Enterprise Integration Pack ähnliche Funktionen zum Transformieren und Überprüfen von Nachrichten, Arbeiten mit Flatfiles und sogar Verwenden von XPath, um eine Nachricht mit bestimmten Eigenschaften anzureichern oder bestimmte Eigenschaften aus einer Nachricht zu extrahieren. Wenn dieser Bereich neu für Sie ist, erweitern diese Funktionen die Art und Weise, in der Sie Nachrichten im Workflow Ihrer Logik-App. Wenn Sie beispielsweise in einem Business-to-Business-Szenario (B2B) mit bestimmten XML-Schemas arbeiten, können Sie das Enterprise Integration Pack verwenden, um die Art der Verarbeitung dieser Nachrichten durch Ihr Unternehmen zu verbessern.

Das Enterprise Integration Pack enthält beispielsweise diese Funktionen:

* [XML-Überprüfung](logic-apps-enterprise-integration-xml-validation.md): Überprüfen einer ein- oder ausgehenden XML-Nachricht anhand eines bestimmten Schemas.

* [XML-Transformation:](logic-apps-enterprise-integration-transform.md) Konvertieren oder Anpassen einer XML-Nachricht basierend auf Ihren Anforderungen oder den Anforderungen eines Partners mithilfe von Zuordnungen.

* [Flatfile-Codierung und Flatfile-Decodierung](logic-apps-enterprise-integration-flatfile.md): Codieren oder Decodieren einer Flatfile.

  SAP akzeptiert und sendet z.B. IDOC-Dateien im Flatfileformat. Viele Integrationsplattformen einschließlich Logic Apps erstellen XML-Nachrichten. Daher können Sie eine Logik-App erstellen, die mit dem Flatfileencoder XML-Dateien in Flatfiles „konvertiert“.

* [XPath](workflow-definition-language-functions-reference.md#xpath): Anreichern einer Nachricht und Extrahieren bestimmter Eigenschaften aus der Nachricht. Mithilfe der extrahierten Eigenschaften können Sie die Nachricht an einen Ziel- oder Zwischenendpunkt weiterleiten.

## <a name="sample"></a>Beispiel

[Stellen Sie eine voll funktionsfähige Logik-App](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.logic/logic-app-veter-pipeline) (GitHub-Beispiel) mit den XML-Features von Azure Logic Apps bereit.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum [Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md)
