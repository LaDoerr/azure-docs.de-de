---
title: API zum Anhalten von Berichtsausführungen
description: Verwenden Sie diese API, um die geplante Ausführung eines Analyseberichts im kommerziellen Marketplace anzuhalten.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
author: smannepalle
ms.author: smannepalle
ms.reviewer: sroy
ms.date: 3/08/2021
ms.openlocfilehash: 80a7238e76b7152f13dd157aa0a1b7fc9937867a
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122355712"
---
# <a name="pause-report-executions-api"></a>API zum Anhalten von Berichtsausführungen

Diese API hält bei der Ausführung die geplante Ausführung von Berichten an.

**Anforderungssyntax**

| Methode | Anforderungs-URI |
| ------------ | ------------- |
| PUT | `https://api.partnercenter.microsoft.com/insights/v1/cmp/ScheduledReport/pause/{Report ID}` |
|||

**Anforderungsheader**

| Header | type | BESCHREIBUNG |
| ------------ | ------------- | ------------- |
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken (Azure Active Directory) in der Form `Bearer <token>`. |
| Content-Type | Zeichenfolge | `Application/JSON` |
||||

**Pfadparameter**

Keine

**Query parameter (Abfrageparameter)**

| Parametername | Erforderlich | type | BESCHREIBUNG |
| ------------ | ------------- | ------------- | ------------- |
| `reportId` | Ja | Zeichenfolge | ID des Berichts, der geändert wird |
|||||

**Glossar**

Keine

**Antwort**

Die Antwortnutzdaten sind wie folgt im JSON-Format strukturiert:

Antwortcode: 200, 400, 401, 403, 404, 500 Antwortnutzdaten:

```json
{
  "Value": [
    {
      "ReportId": "string",
      "ReportName": "string",
      "Description": "string",
      "QueryId": "string",
      "Query": "string",
      "User": "string",
      "CreatedTime": "string",
      "ModifiedTime": "string",
      "StartTime": "string",
      "ReportStatus": "string",
      "RecurrenceInterval": 0,
      "RecurrenceCount": 0,
      "CallbackUrl": "string",
      "Format": "string"
    }
  ],
  "TotalCount": 0,
  "Message": "string",
  "StatusCode": 0
}
```

**Glossar**

| Parameter | Beschreibung |
| ------------ | ------------- |
| `ReportId` | Universell eindeutiger Bezeichner (Universally Unique Identifier, UUID) des gelöschten Berichts |
| `ReportName` | Name, der dem Bericht während der Erstellung gegeben wurde |
| `Description` | Beschreibung, die während der Erstellung des Berichts angegeben wurde |
| `QueryId` | Abfrage-ID, die zum Zeitpunkt der Erstellung des Berichts übergeben wurde |
| `Query` | Abfragetext, der für diesen Bericht ausgeführt wird |
| `User` | Benutzer-ID, die zum Erstellen des Berichts verwendet wurde |
| `CreatedTime` | Zeitpunkt, zu dem der Bericht erstellt wurde. Das Zeitformat lautet yyyy-MM-ddTHH:mm:ssZ |
| `ModifiedTime` | Uhrzeit der letzten Änderung des Berichts. Das Zeitformat lautet yyyy-MM-ddTHH:mm:ssZ |
| `StartTime` | Uhrzeit, zu der die Berichtsausführung beginnt. Das Zeitformat lautet yyyy-MM-ddTHH:mm:ssZ |
| `ReportStatus` | Status der Berichtsausführung. Die möglichen Werte sind „Angehalten“, „Aktiv“ und „Inaktiv“. |
| `RecurrenceInterval` | Wiederholungsintervall, das bei der Erstellung des Berichts angegeben wurde |
| `RecurrenceCount` | Anzahl der Wiederholungen, die bei der Erstellung des Berichts angegeben wurde |
| `CallbackUrl` | Rückruf-URL, die in der Anforderung angegeben wurde |
| `Format` | Format der Berichtsdateien |
|||
