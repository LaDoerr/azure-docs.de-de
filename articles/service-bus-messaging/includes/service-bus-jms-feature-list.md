---
title: Datei einfügen
description: include file
services: service-bus-messaging
author: axisc
ms.service: service-bus-messaging
ms.topic: include
ms.date: 6/9/2020
ms.author: aschhab
ms.custom: include file
ms.openlocfilehash: 574507fcc6a3c05919c441bd6d0ec9c573d4b6ae
ms.sourcegitcommit: 5163ebd8257281e7e724c072f169d4165441c326
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2021
ms.locfileid: "112414688"
---
In der folgenden Tabelle sind die zurzeit von Azure Service Bus unterstützten JMS-Funktionen (Java Message Service) aufgelistet. Außerdem werden Funktionen angezeigt, die nicht unterstützt werden.


| Funktion | API |Status |
|---|---|---|
| Warteschlangen   | <ul> <li> JMSContext.createQueue( String queueName) </li> </ul>| **Unterstützt** |
| Themen   | <ul> <li> JMSContext.createTopic( String topicName) </li> </ul>| **Unterstützt** |
| Temporäre Warteschlangen |<ul> <li> JMSContext.createTemporaryQueue() </li> </ul>| **Unterstützt** |
| Temporäre Themen |<ul> <li> JMSContext.createTemporaryTopic() </li> </ul>| **Unterstützt** |
| Message Producer /<br/> JMSProducer |<ul> <li> JMSContext.createProducer() </li> </ul>| **Unterstützt** |
| Warteschlangenbrowser |<ul> <li> JMSContext.createBrowser(Queue queue) </li> <li> JMSContext.createBrowser(Queue queue, String messageSelector) </li> </ul> | **Unterstützt** |
| Message Consumer/ <br/> JMSConsumer | <ul> <li> JMSContext.createConsumer( Destination destination) </li> <li> JMSContext.createConsumer( Destination destination, String messageSelector) </li> <li> JMSContext.createConsumer( Destination destination, String messageSelector, boolean noLocal)</li> </ul>  <br/> „noLocal“ wird zurzeit nicht unterstützt. | **Unterstützt** |
| Freigegebene dauerhafte Abonnements | <ul> <li> JMSContext.createSharedDurableConsumer(Topic topic, String name) </li> <li> JMSContext.createSharedDurableConsumer(Topic topic, String name, String messageSelector) </li> </ul>| **Unterstützt**|
| Nicht freigegebene dauerhafte Abonnements | <ul> <li> JMSContext.createDurableConsumer(Topic topic, String name) </li> <li> createDurableConsumer(Topic topic, String name, String messageSelector, boolean noLocal) </li> </ul> <br/> „noLocal“ wird zurzeit nicht unterstützt und sollte auf „false“ festgelegt werden. | **Unterstützt** |
| Freigegebene nicht dauerhafte Abonnements |<ul> <li> JMSContext.createSharedConsumer(Topic topic, String sharedSubscriptionName) </li> <li> JMSContext.createSharedConsumer(Topic topic, String sharedSubscriptionName, String messageSelector) </li> </ul> | **Unterstützt** |
| Nicht freigegebene nicht dauerhafte Abonnements |<ul> <li> JMSContext.createConsumer(Destination destination) </li> <li> JMSContext.createConsumer( Destination destination, String messageSelector) </li> <li> JMSContext.createConsumer( Destination destination, String messageSelector, boolean noLocal) </li> </ul> <br/> „noLocal“ wird zurzeit nicht unterstützt und sollte auf „false“ festgelegt werden. | **Unterstützt** |
| Nachrichtenselektoren | Abhängig vom erstellten Consumer. | **Unterstützt** |
| Tägliche Zustellung (geplante Nachrichten) | <ul> <li> JMSProducer.setDeliveryDelay( long deliveryDelay) </li> </ul>|**Unterstützt**|
| Nachricht erstellt |<ul> <li> JMSContext.createMessage() </li> <li> JMSContext.createBytesMessage() </li> <li> JMSContext.createMapMessage() </li> <li> JMSContext.createObjectMessage( Serializable object) </li> <li> JMSContext.createStreamMessage() </li> <li> JMSContext.createTextMessage() </li> <li> JMSContext.createTextMessage( String text) </li> </ul>| **Unterstützt** |
| Entitätsübergreifende Transaktionen |<ul> <li> Connection.createSession(true, Session.SESSION_TRANSACTED) </li> </ul> | **Unterstützt** |
| Verteilte Transaktionen || Nicht unterstützt |
