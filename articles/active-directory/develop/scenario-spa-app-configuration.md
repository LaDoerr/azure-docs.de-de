---
title: Konfigurieren einer Single-Page-Webanwendung | Azure
titleSuffix: Microsoft identity platform
description: Informationen zum Erstellen einer Single-Page-Webanwendung (Codekonfiguration der App)
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 02/11/2020
ms.author: marsma
ms.custom: aaddev
ms.openlocfilehash: f049f49893b2ce1a895b9d175cbc308ee2868043
ms.sourcegitcommit: 82d82642daa5c452a39c3b3d57cd849c06df21b0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2021
ms.locfileid: "113357569"
---
# <a name="single-page-application-code-configuration"></a>Single-Page-Webanwendung: Codekonfiguration

Erfahren Sie, wie Sie den Code für Ihre Single-Page-Webanwendung (SPA) konfigurieren.

## <a name="microsoft-libraries-supporting-single-page-apps"></a>Microsoft-Bibliotheken zur Unterstützung von Single-Page-Apps

Die folgenden Microsoft-Bibliotheken unterstützen Single-Page-Apps:

[!INCLUDE [active-directory-develop-libraries-spa](../../../includes/active-directory-develop-libraries-spa.md)]

## <a name="application-code-configuration"></a>Codekonfiguration der Anwendung

In einer MSAL-Bibliothek werden die Anwendungsregistrierungsinformationen während der Initialisierung der Bibliothek als Konfiguration übergeben.

# <a name="javascript-msaljs-v2"></a>[JavaScript (MSAL.js v2)](#tab/javascript2)

```javascript
// Configuration object constructed.
const config = {
    auth: {
        clientId: 'your_client_id'
    }
};

// create PublicClientApplication instance
const publicClientApplication = new PublicClientApplication(config);
```

Weitere Informationen zu den konfigurierbaren Optionen finden Sie unter [Initialisieren von Clientanwendungen mithilfe von MSAL.js](msal-js-initializing-client-applications.md).

# <a name="javascript-msaljs-v1"></a>[JavaScript (MSAL.js v1)](#tab/javascript1)

```javascript
// Configuration object constructed.
const config = {
    auth: {
        clientId: 'your_client_id'
    }
};

// create UserAgentApplication instance
const userAgentApplication = new UserAgentApplication(config);
```

Weitere Informationen zu den konfigurierbaren Optionen finden Sie unter [Initialisieren von Clientanwendungen mithilfe von MSAL.js](msal-js-initializing-client-applications.md).

# <a name="angular-msaljs-v2"></a>[Angular (MSAL.js v2)](#tab/angular2)

```javascript
// In app.module.ts
import { MsalModule } from '@azure/msal-angular';
import { PublicClientApplication } from '@azure/msal-browser';

@NgModule({
    imports: [
        MsalModule.forRoot( new PublicClientApplication({
            auth: {
                clientId: 'Enter_the_Application_Id_Here',
            }
        }), null, null)
    ]
})
export class AppModule { }
```

# <a name="angular-msaljs-v1"></a>[Angular (MSAL.js v1)](#tab/angular1)

```javascript
// App.module.ts
import { MsalModule } from '@azure/msal-angular';
@NgModule({
    imports: [
        MsalModule.forRoot({
            auth: {
                clientId: 'your_app_id'
            }
        })
    ]
})
export class AppModule { }
```

# <a name="react"></a>[React](#tab/react)

```javascript
import { PublicClientApplication } from "@azure/msal-browser";
import { MsalProvider } from "@azure/msal-react";

// Configuration object constructed.
const config = {
    auth: {
        clientId: 'your_client_id'
    }
};

// create PublicClientApplication instance
const publicClientApplication = new PublicClientApplication(config);

// Wrap your app component tree in the MsalProvider component
ReactDOM.render(
    <React.StrictMode>
        <MsalProvider instance={publicClientApplication}>
            <App />
        </ MsalProvider>
    </React.StrictMode>,
    document.getElementById('root')
);
```

---

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit dem nächsten Artikel in diesem Szenario fort: [Anmelden und Abmelden](scenario-spa-sign-in.md).
