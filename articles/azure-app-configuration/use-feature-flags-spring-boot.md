---
title: 'Tutorial zur Verwendung von Featureflags in einer Spring Boot-App: Azure App Configuration | Microsoft-Dokumentation'
description: In diesem Tutorial erfahren Sie, wie Sie Featureflags in Spring Boot-Apps implementieren.
services: azure-app-configuration
documentationcenter: ''
author: mrm9084
manager: zhenlan
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.devlang: java
ms.topic: tutorial
ms.date: 06/25/2021
ms.author: mametcal
ms.custom: mvc, devx-track-java
ms.openlocfilehash: dc101eac5d3829d8f4bcb84c79481b2053f82965
ms.sourcegitcommit: 40dfa64d5e220882450d16dcc2ebef186df1699f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2021
ms.locfileid: "113037650"
---
# <a name="tutorial-use-feature-flags-in-a-spring-boot-app"></a>Tutorial: Verwenden von Featureflags in einer Spring Boot-App

Die Spring Boot Core-Featureverwaltungsbibliotheken bieten Unterstützung für die Implementierung von Featureflags in einer Spring Boot-Anwendung. Dank dieser Bibliotheken können Sie Ihrem Code Featureflags auf deklarative Weise hinzufügen.

Die Featureverwaltungsbibliotheken verwalten darüber hinaus Featureflag-Lebenszyklen im Hintergrund. Die Bibliotheken können Flagzustände aktualisieren und zwischenspeichern, garantieren, dass ein Flagzustand während eines Anforderungsaufrufs unveränderlich ist, und Ähnliches. Darüber hinaus bietet die Spring Boot-Bibliothek Integrationen, einschließlich MVC-Controlleraktionen, Routen und Middleware.

Unter [Schnellstart: Hinzufügen von Featureflags zu einer Spring Boot-App](./quickstart-feature-flag-spring-boot.md) werden mehrere Methoden gezeigt, mit denen Sie Featureflags in einer Spring Boot-Anwendung hinzufügen können. Diese Methoden werden im vorliegenden Tutorial näher erläutert.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Hinzufügen von Featureflags in wichtigen Teilen Ihrer Anwendung, um die Verfügbarkeit von Features zu steuern
> * Integrieren in App Configuration, wenn Sie damit Featureflags verwalten

## <a name="set-up-feature-management"></a>Einrichten der Featureverwaltung

Der Spring Boot-Feature-Manager `FeatureManager` ruft Featureflags aus dem nativen Konfigurationssystem des Frameworks ab. Dadurch können Sie die Featureflags Ihrer Anwendung mit einer beliebigen, von Spring Boot unterstützten Konfigurationsquelle konfigurieren – unter anderem mit der lokalen Datei *bootstrap.yml* oder mit Umgebungsvariablen. `FeatureManager` basiert auf der Abhängigkeitsinjektion. Die Featureverwaltungsdienste können unter Verwendung von Standardkonventionen registriert werden:

```java
private FeatureManager featureManager;

public HelloController(FeatureManager featureManager) {
    this.featureManager = featureManager;
}
```

Featureflags sollten sich außerhalb der Anwendung befinden und separat verwaltet werden. Dadurch können Sie Flagzustände jederzeit ändern, und die Änderungen werden in der Anwendung sofort wirksam. Mit App Configuration können Sie alle Ihre Featureflags über eine dedizierte Portalbenutzeroberfläche an einem zentralen Ort organisieren und steuern. App Configuration übermittelt darüber hinaus die Flags über seine Spring Boot-Clientbibliotheken direkt an Ihre Anwendung.

Die Verbindung zwischen Ihrer Spring Boot-Anwendung und App Configuration lässt sich am einfachsten über den Konfigurationsanbieter herstellen:

```xml
<dependency>
    <groupId>com.azure.spring</groupId>
    <artifactId>azure-spring-cloud-feature-management-web</artifactId>
    <version>2.0.0</version>
</dependency>
```

> [!NOTE]
> Wenn Sie Unterstützung für eine ältere Version von Spring Boot benötigen, sehen Sie sich unsere [alte Bibliothek](https://github.com/Azure/azure-sdk-for-java/blob/spring-cloud-starter-azure-appconfiguration-config_1.2.9/sdk/appconfiguration/spring-cloud-azure-feature-management/README.md) an.

## <a name="feature-flag-declaration"></a>Featureflagdeklaration

Jedes Featureflag besteht aus zwei Teilen: einem Namen und einer Filterliste, anhand der ausgewertet wird, ob ein Feature *aktiviert* ist (der Wert also `True` lautet). Ein Filter definiert einen Anwendungsfall für die Aktivierung eines Features.

Verfügt ein Featureflag über mehrere Filter, wird die Filterliste in der angegebenen Reihenfolge durchlaufen, bis einer der Filter angibt, dass das Feature aktiviert werden soll. Daraufhin ist das Featureflag *aktiviert*, und alle weiteren Filterergebnisse werden übersprungen. Falls durch keinen Filter angegeben wird, dass das Feature aktiviert werden soll, ist das Featureflag *deaktiviert*.

Der Feature-Manager unterstützt *application.yml* als Konfigurationsquelle für Featureflags. Das folgende Beispiel zeigt die Einrichtung von Featureflags in einer YAML-Datei:

```yml
feature-management:
  feature-set:
    feature-a: true
    feature-b: false
    feature-c:
      enabled-for:
        -
          name: Percentage
          parameters:
            value: 50
```

Der Abschnitt `feature-management` dieses YML-Dokuments wird konventionsgemäß für Featureflageinstellungen verwendet. Das vorherige Beispiel enthält drei Featureflags, deren Filter in der Eigenschaft `EnabledFor` definiert sind:

* `feature-a` ist *aktiviert*.
* `feature-b` ist *deaktiviert*.
* `feature-c` gibt einen Filter namens `Percentage` mit einer Eigenschaft vom Typ `parameters` an. `Percentage` ist ein konfigurierbarer Filter. In diesem Beispiel gibt `Percentage` an, dass das Flag `feature-c` mit einer Wahrscheinlichkeit von 50 Prozent *aktiviert* ist.

## <a name="feature-flag-checks"></a>Überprüfen von Featureflags

Bei der Featureverwaltung wird grundsätzlich zunächst geprüft, ob ein Featureflag *aktiviert* ist. Ist dies der Fall, führt der Feature-Manager die im Feature enthaltenen Aktionen aus. Beispiel:

```java
private FeatureManager featureManager;
...
if (featureManager.isEnabledAsync("feature-a").block()) {
    // Run the following code
}
```

## <a name="dependency-injection"></a>Abhängigkeitsinjektion

In Spring Boot kann auf den Feature-Manager `FeatureManager` mittels Abhängigkeitsinjektion zugegriffen werden:

```java
@Controller
@ConfigurationProperties("controller")
public class HomeController {
    private FeatureManager featureManager;

    public HomeController(FeatureManager featureManager) {
        this.featureManager = featureManager;
    }
}
```

## <a name="controller-actions"></a>Controlleraktionen

In MVC-Controllern steuern Sie mithilfe eines Attributs vom Typ `@FeatureGate`, ob eine spezifische Aktion aktiviert wird. Für die folgende Aktion `Index` muss `feature-a`*aktiviert* sein, damit sie ausgeführt werden kann:

```java
@GetMapping("/")
@FeatureGate(feature = "feature-a")
public String index(Model model) {
    ...
}
```

Ist ein MVC-Controller oder eine Aktion blockiert, weil das steuernde Featureflag *deaktiviert* ist, wird eine registrierte Schnittstelle (`IDisabledFeaturesHandler`) aufgerufen. Die Standardschnittstelle `IDisabledFeaturesHandler` gibt den Statuscode 404 ohne Antworttext an den Client zurück.

## <a name="mvc-filters"></a>MVC-Filter

Sie können MVC-Filter so einrichten, dass sie abhängig vom Zustand eines Featureflags aktiviert werden. Im folgenden Code wird ein MVC-Filter namens `FeatureFlagFilter` hinzugefügt. Dieser Filter wird innerhalb der MVC-Pipeline nur ausgelöst, wenn `feature-a` aktiviert ist.

```java
@Component
public class FeatureFlagFilter implements Filter {

    @Autowired
    private FeatureManager featureManager;

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
            throws IOException, ServletException {
        if(!featureManager.isEnabledAsync("feature-a").block()) {
            chain.doFilter(request, response);
            return;
        }
        ...
        chain.doFilter(request, response);
    }
}
```

## <a name="routes"></a>Routen

Sie können Featureflags zum Umleiten von Routen verwenden. Mit dem folgenden Code wird ein Benutzer von `feature-a` umgeleitet:

```java
@GetMapping("/redirect")
@FeatureGate(feature = "feature-a", fallback = "/getOldFeature")
public String getNewFeature() {
    // Some New Code
}

@GetMapping("/getOldFeature")
public String getOldFeature() {
    // Some New Code
}
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie Featureflags in Ihrer Spring Boot-Anwendung unter Verwendung der Bibliotheken vom Typ `azure-spring-cloud-feature-management-web` implementieren. Die folgenden Ressourcen enthalten weitere Informationen zur Unterstützung der Featureverwaltung in Spring Boot und App Configuration:

* [Schnellstart: Hinzufügen von Featureflags zu einer Spring Boot-App](./quickstart-feature-flag-spring-boot.md)
* [Verwalten von Featureflags](./manage-feature-flags.md)