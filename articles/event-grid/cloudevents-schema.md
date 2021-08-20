---
title: Verwenden von Azure Event Grid mit Ereignissen im CloudEvents-Schema
description: Beschreibt die Verwendung des CloudEvents-Schemas für Ereignisse in Azure Event Grid. Der Dienst unterstützt Ereignisse in der JSON-Implementierung von CloudEvents.
ms.topic: conceptual
ms.date: 07/22/2021
ms.custom: devx-track-js, devx-track-csharp, devx-track-azurecli, devx-track-azurepowershell
ms.openlocfilehash: 0fcf4a5cd629401c44a208d6d697a04855b4a902
ms.sourcegitcommit: 7d63ce88bfe8188b1ae70c3d006a29068d066287
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2021
ms.locfileid: "114469483"
---
# <a name="use-cloudevents-v10-schema-with-event-grid"></a>Verwenden des CloudEvents 1.0-Schemas mit Event Grid
Zusätzlich zu seinem [Standardereignisschema](event-schema.md) unterstützt Azure Event Grid nativ Ereignisse in der [JSON-Implementierung von CloudEvents 1.0](https://github.com/cloudevents/spec/blob/v1.0/json-format.md) und in der [HTTP-Bindung](https://github.com/cloudevents/spec/blob/v1.0/http-protocol-binding.md). [CloudEvents](https://cloudevents.io/) ist eine [offene Spezifikation](https://github.com/cloudevents/spec/blob/v1.0/spec.md) zur Beschreibung von Ereignisdaten.

CloudEvents vereinfacht die Interoperabilität mithilfe eines gemeinsamen Ereignisschemas für die Veröffentlichung und Nutzung von cloudbasierten Ereignissen. Dieses Schema ermöglicht einheitliche Tools, Standardverfahren für Routing und Behandlung von Ereignissen sowie universelle Verfahren zum Deserialisieren des äußeren Ereignisschemas. Mit einem allgemeinen Schema können Sie Arbeit leichter plattformübergreifend integrieren.

CloudEvents wird von mehreren [Partnern](https://github.com/cloudevents/spec/blob/master/community/contributors.md), einschließlich Microsoft, über die [Cloud Native Computing Foundation](https://www.cncf.io/) entwickelt. Es ist derzeit in Version 1.0 verfügbar.

Dieser Artikel beschreibt den Einsatz des CloudEvents-Schemas mit Event Grid.

## <a name="cloudevent-schema"></a>CloudEvents-Schema

Hier ist ein Beispiel für ein Azure Blob Storage-Ereignis im CloudEvents-Format:

``` JSON
{
    "specversion": "1.0",
    "type": "Microsoft.Storage.BlobCreated",  
    "source": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-account}",
    "id": "9aeb0fdf-c01e-0131-0922-9eb54906e209",
    "time": "2019-11-18T15:13:39.4589254Z",
    "subject": "blobServices/default/containers/{storage-container}/blobs/{new-file}",
    "dataschema": "#",
    "data": {
        "api": "PutBlockList",
        "clientRequestId": "4c5dd7fb-2c48-4a27-bb30-5361b5de920a",
        "requestId": "9aeb0fdf-c01e-0131-0922-9eb549000000",
        "eTag": "0x8D76C39E4407333",
        "contentType": "image/png",
        "contentLength": 30699,
        "blobType": "BlockBlob",
        "url": "https://gridtesting.blob.core.windows.net/testcontainer/{new-file}",
        "sequencer": "000000000000000000000000000099240000000000c41c18",
        "storageDiagnostics": {
            "batchId": "681fe319-3006-00a8-0022-9e7cde000000"
        }
    }
}
```

Eine ausführliche Beschreibung der verfügbaren Felder sowie ihrer Typen und Definitionen finden Sie unter [CloudEvents v1.0](https://github.com/cloudevents/spec/blob/v1.0/spec.md#required-attributes).

Die im CloudEvents- und Event Grid-Schema übermittelten Headerwerte für Ereignisse sind mit Ausnahme von `content-type` identisch. Für das CloudEvents-Schema ist dieser Headerwert `"content-type":"application/cloudevents+json; charset=utf-8"`. Für das Event Grid-Schema ist dieser Headerwert `"content-type":"application/json; charset=utf-8"`.

## <a name="configure-event-grid-for-cloudevents"></a>Konfigurieren von Event Grid für CloudEvents

Sie können Event Grid für die Eingabe und Ausgabe von Ereignissen im CloudEvents-Schema verwenden. In der folgenden Tabelle sind die möglichen Transformationen beschrieben:

 Event Grid-Ressource | Eingabeschema       | Schema für Bereitstellung
|---------------------|-------------------|---------------------
| Systemthemen       | Event Grid-Schema | Event Grid-Schema oder CloudEvents-Schema
| Benutzerdefinierte Themen/Domänen | Event Grid-Schema | Event Grid-Schema oder CloudEvents-Schema
| Benutzerdefinierte Themen/Domänen | CloudEvents-Schema | CloudEvents-Schema
| Benutzerdefinierte Themen/Domänen | Benutzerdefiniertes Schema     | Benutzerdefiniertes Schema, Event Grid-Schema oder CloudEvents-Schema
| Partnerthemen       | CloudEvents-Schema | CloudEvents-Schema

In allen Ereignisschemas erfordert Event Grid vor dem Veröffentlichen in einem Event Grid-Thema und vor dem Erstellen eines Ereignisabonnements eine Überprüfung.

Weitere Informationen finden Sie unter [Event Grid – Sicherheit und Authentifizierung](security-authentication.md).

### <a name="input-schema"></a>Eingabeschema

Das Eingabeschema für ein benutzerdefiniertes Thema wird beim Erstellen des benutzerdefinierten Themas festgelegt.

Verwenden Sie für die Azure CLI Folgendes:

```azurecli-interactive
az eventgrid topic create \
  --name <topic_name> \
  -l westcentralus \
  -g gridResourceGroup \
  --input-schema cloudeventschemav1_0
```

Verwenden Sie für PowerShell Folgendes:

```azurepowershell-interactive
New-AzEventGridTopic `
  -ResourceGroupName gridResourceGroup `
  -Location westcentralus `
  -Name <topic_name> `
  -InputSchema CloudEventSchemaV1_0
```

### <a name="output-schema"></a>Ausgabeschema

Das Ausgabeschema wird beim Erstellen des Ereignisabonnements festgelegt.

Verwenden Sie für die Azure CLI Folgendes:

```azurecli-interactive
topicID=$(az eventgrid topic show --name <topic-name> -g gridResourceGroup --query id --output tsv)

az eventgrid event-subscription create \
  --name <event_subscription_name> \
  --source-resource-id $topicID \
  --endpoint <endpoint_URL> \
  --event-delivery-schema cloudeventschemav1_0
```

Verwenden Sie für PowerShell Folgendes:
```azurepowershell-interactive
$topicid = (Get-AzEventGridTopic -ResourceGroupName gridResourceGroup -Name <topic-name>).Id

New-AzEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName <event_subscription_name> `
  -Endpoint <endpoint_URL> `
  -DeliverySchema CloudEventSchemaV1_0
```

 Momentan kann für eine Azure Functions-App kein Event Grid-Trigger verwendet werden, wenn das Ereignis im CloudEvents-Schema übermittelt wird. Verwenden Sie einen HTTP-Trigger. Beispiele für die Implementierung eines HTTP-Triggers, der Ereignisse im CloudEvents-Schema empfängt, finden Sie unter [Verwenden von CloudEvents mit Azure Functions](#azure-functions).

## <a name="endpoint-validation-with-cloudevents-v10"></a>Endpunktüberprüfung mit CloudEvents 1.0

Wenn Sie mit Event Grid bereits vertraut sind, kennen Sie wahrscheinlich auch den Handshake zur Überprüfung von Endpunkten, mit dem Missbrauch verhindert werden soll. CloudEvents v1.0 implementiert eine eigene [Semantik für den Schutz vor Missbrauch](webhook-event-delivery.md) über die HTTP OPTIONS-Methode. Weitere Informationen dazu finden Sie unter [HTTP 1.1-Webhooks für die Ereignisbereitstellung – Version 1.0](https://github.com/cloudevents/spec/blob/v1.0/http-webhook.md#4-abuse-protection). Wenn Sie das CloudEvents-Schema für die Ausgabe nutzen, verwendet Event Grid anstelle des Event Grid-Mechanismus für Überprüfungsereignisse den Missbrauchschutz von CloudEvents v1.0.

<a name="azure-functions"></a>

## <a name="use-with-azure-functions"></a>Verwendung mit Azure Functions

Die [Azure Functions Event Grid-Bindung](../azure-functions/functions-bindings-event-grid.md) bietet keine native Unterstützung für CloudEvents, sodass zum Lesen von CloudEvents-Nachrichten über HTTP ausgelöste Funktionen verwendet werden. Bei Verwendung eines HTTP-Triggers zum Lesen von CloudEvents müssen Sie Code für folgende Vorgänge schreiben, die über den Event Grid-Trigger automatisch ausgeführt werden:

* Senden einer Überprüfungsantwort an eine [Anforderung zur Abonnementüberprüfung](../event-grid/webhook-event-delivery.md)
* Aufrufen der Funktion einmal pro Element des im Anforderungstext enthaltenen Ereignisarrays

Informationen zu der URL, mit der die Funktion aufgerufen werden soll, wenn diese lokal oder in Azure ausgeführt wird, finden Sie in der [Referenzdokumentation zu HTTP-Triggerbindungen](../azure-functions/functions-bindings-http-webhook.md).

Mit dem folgenden C#-Beispielcode für einen HTTP-Trigger wird das Verhalten eines Event Grid-Triggers simuliert. Verwenden Sie dieses Beispiel für Ereignisse, die im CloudEvents-Schema bereitgestellt werden.

```csharp
[FunctionName("HttpTrigger")]
public static async Task<HttpResponseMessage> Run([HttpTrigger(AuthorizationLevel.Anonymous, "post", "options", Route = null)]HttpRequestMessage req, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");
    if (req.Method == HttpMethod.Options)
    {
        // If the request is for subscription validation, send back the validation code
        
        var response = req.CreateResponse(HttpStatusCode.OK);
        response.Headers.Add("Webhook-Allowed-Origin", "eventgrid.azure.net");

        return response;
    }

    var requestmessage = await req.Content.ReadAsStringAsync();
    var message = JToken.Parse(requestmessage);

    // The request isn't for subscription validation, so it's for an event.
    // CloudEvents schema delivers one event at a time.
    log.LogInformation($"Source: {message["source"]}");
    log.LogInformation($"Time: {message["eventTime"]}");
    log.LogInformation($"Event data: {message["data"].ToString()}");

    return req.CreateResponse(HttpStatusCode.OK);
}
```

Mit dem folgenden JavaScript-Beispielcode für einen HTTP-Trigger wird das Verhalten eines Event Grid-Triggers simuliert. Verwenden Sie dieses Beispiel für Ereignisse, die im CloudEvents-Schema bereitgestellt werden.

```javascript
module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');
    
    if (req.method == "OPTIONS") {
        // If the request is for subscription validation, send back the validation code
        
        context.log('Validate request received');
        context.res = {
            status: 200,
            headers: {
                'Webhook-Allowed-Origin': 'eventgrid.azure.net',
            },
         };
    }
    else
    {
        var message = req.body;
        
        // The request isn't for subscription validation, so it's for an event.
        // CloudEvents schema delivers one event at a time.
        var event = JSON.parse(message);
        context.log('Source: ' + event.source);
        context.log('Time: ' + event.eventTime);
        context.log('Data: ' + JSON.stringify(event.data));
    }
 
    context.done();
};
```

## <a name="next-steps"></a>Nächste Schritte

* Informationen zur Überwachung von Ereignisübermittlungen finden Sie unter [Überwachen der Event Grid-Nachrichtenübermittlung](monitor-event-delivery.md).
* Wir empfehlen, CloudEvents zu testen, zu kommentieren und dazu [beizutragen](https://github.com/cloudevents/spec/blob/master/community/CONTRIBUTING.md).
* Weitere Informationen zum Erstellen eines Azure Event Grid-Abonnements finden Sie unter [Event Grid-Abonnementschema](subscription-creation-schema.md).
