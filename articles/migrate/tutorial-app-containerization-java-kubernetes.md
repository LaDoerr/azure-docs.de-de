---
title: 'Azure App-Containerisierung: Java; Containerisierung und Migration von Java-Anwendungen zu Azure Kubernetes.'
description: 'Tutorial: Containerisieren und Migrieren von Java-Anwendungen zu Azure Kubernetes Service'
services: ''
author: rahugup
manager: bsiva
ms.topic: tutorial
ms.date: 6/30/2021
ms.author: rahugup
ms.openlocfilehash: 415a6c5ae8ff9083dc26ce36d92a771be3ad01f6
ms.sourcegitcommit: 6ea4d4d1cfc913aef3927bef9e10b8443450e663
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2021
ms.locfileid: "113295957"
---
# <a name="java-web-app-containerization-and-migration-to-azure-kubernetes-service"></a>Containerisieren und Migrieren von Java-Web-Apps zu Azure Kubernetes Service

In diesem Artikel erfahren Sie, wie Sie unter Apache Tomcat ausgeführte Java-Web-Apps mit dem App-Containerisierungstool von Azure Migrate containerisieren und zu [Azure Kubernetes Service (AKS)](https://azure.microsoft.com/services/kubernetes-service/) migrieren können. Für den Containerisierungsprozess ist kein Zugriff auf Ihre Codebasis erforderlich, zudem ist die Containerisierung vorhandener Apps mühelos möglich. Das Tool ermittelt anhand des Ausführungsstatus der Apps auf dem Server die App-Komponenten und packt diese in ein Containerimage. Die containerisierte Anwendung kann dann in Azure Kubernetes Service (AKS) bereitgestellt werden.

Das App-Containerisierungstool von Azure Migrate unterstützt derzeit die folgenden Vorgänge:


- Containerisieren von Java-Web-Apps unter Apache Tomcat (auf Linux-Servern) und Bereitstellen dieser Apps in Linux-Containern in AKS
- Containerisieren von Java-Web-Apps unter Apache Tomcat (auf Linux-Servern) und Bereitstellen dieser Apps in Linux-Containern in App Service. [Weitere Informationen](./tutorial-app-containerization-java-app-service.md)
- Containerisieren von ASP.NET-Apps und Bereitstellen dieser Apps in Windows-Containern in AKS [Weitere Informationen](./tutorial-app-containerization-aspnet-kubernetes.md)
- Containerisieren von ASP.NET-Apps und Bereitstellen dieser Apps in Windows-Containern in App Service. [Weitere Informationen](./tutorial-app-containerization-aspnet-app-service.md)

Das App-Containerisierungstool von Azure Migrate unterstützt Sie bei den folgenden Vorgängen:

- **App analysieren:** Das Tool stellt eine Remoteverbindung mit den Anwendungsservern her, auf denen Ihre Java-Web-App ausgeführt wird (Apache Tomcat), und ermittelt die App-Komponenten. Dann erstellt das Tool ein Dockerfile, das zum Erstellen eines Containerimages für die App verwendet werden kann.
- **Containerimage erstellen:** Sie können das Dockerfile ansehen, an die Anforderungen der App anpassen und es anschließend verwenden, um das Containerimage für die App zu erstellen. Das Containerimage für die App wird an eine von Ihnen festgelegte Azure Container Registry-Instanz gepusht.
- **In Azure Kubernetes Service bereitstellen:** Das Tool generiert anschließend die YAML-Dateien für die Kubernetes-Ressourcendefinition, die benötigt werden, um die containerisierte Anwendung in Ihrem Azure Kubernetes Service-Cluster bereitzustellen. Die YAML-Dateien können angepasst und für die Bereitstellung der App in AKS verwendet werden.

> [!NOTE]
> Das App-Containerisierungstool von Azure Migrate ermittelt die App-Typen (ASP.NET- und Java-Web-Apps unter Apache Tomcat) und die App-Komponenten auf einem Anwendungsserver. Verwenden Sie die Ermittlungs- und Bewertungsfunktion von Azure Migrate, um Server sowie den Bestand an Apps, Rollen und Features zu ermitteln, die auf lokalen Computern ausgeführt werden. [Weitere Informationen](./tutorial-discover-vmware.md)

Nicht alle Apps profitieren von einer Umstellung auf Container ohne erhebliche Überarbeitungen. Unter anderem ergeben sich durch die Verschiebung vorhandener Apps in Container ohne Überarbeitung jedoch folgende Vorteile:

- **Verbesserte Infrastrukturauslastung:** Mithilfe von Containern können mehrere Apps dieselben Ressourcen nutzen und in derselben Infrastruktur gehostet werden. Dadurch können Sie die Infrastruktur konsolidieren und die Auslastung verbessern.
- **Vereinfachte Verwaltung**: Indem Sie Ihre Anwendungen auf einer modernen verwalteten Plattform wie AKS und App Service hosten, können Sie Ihre Verwaltungspraktiken vereinfachen. Das können Sie erreichen, indem Sie Wartungs- und Verwaltungsvorgänge für die Infrastruktur einstellen oder reduzieren, die Sie für Ihre eigene Infrastruktur normalerweise durchführen würden.
- **Anwendungsportabilität**: Durch die zunehmende Nutzung und Standardisierung von Containerspezifikationsformaten und -plattformen stellt die Anwendungsportabilität keine Herausforderungen mehr dar.
- **Moderne Verwaltung mit DevOps**: Dies ermöglicht Ihnen die Einführung und Standardisierung moderner Verwaltungs- und Sicherheitsverfahren sowie den Übergang zu DevOps.


In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Einrichten eines Azure-Kontos
> * Installieren des App-Containerisierungstools von Azure Migrate
> * Analysieren Ihrer Java-Web-Apps
> * Erstellen des Containerimages
> * Bereitstellen der containerisierten Anwendung in AKS

> [!NOTE]
> In den Tutorials wird der einfachste Bereitstellungspfad für ein Szenario erläutert, damit Sie schnell einen Proof of Concept einrichten können. Die Tutorials verwenden nach Möglichkeit Standardoptionen und zeigen nicht alle möglichen Einstellungen und Pfade.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit diesem Tutorial beginnen, sollten folgende Voraussetzungen erfüllt sein:

**Anforderung** | **Details**
--- | ---
**Computer für die Installation des Tools** | Sie benötigen einen Windows-Computer, auf dem das App-Containerisierungstool von Azure Migrate installiert und ausgeführt werden kann. Auf diesem Windows-Computer kann ein Server- (Windows Server 2016 oder höher) oder ein Clientbetriebssystem (Windows 10) ausgeführt werden. Das Tool ist also für Desktopcomputer geeignet. <br/><br/> Der Windows-Computer, auf dem das Tool ausgeführt wird, sollte über eine Netzwerkverbindung mit den Servern bzw. VMs verfügen, auf denen die zu containerisierenden Java-Web-Apps gehostet werden.<br/><br/> Auf dem Windows-Computer, auf dem das App-Containerisierungstool von Azure Migrate ausgeführt wird, muss mindestens 6 GB freier Speicherplatz für die Anwendungsartefakte vorhanden sein. <br/><br/> Der Windows-Computer sollte direkt oder über einen Proxy auf das Internet zugreifen können.
**Anwendungsserver** | – Ermöglichen SSH-Verbindungen (Secure Shell) über Port 22 der Server, auf denen zu containerisierende Java-Apps ausgeführt werden <br/>
**Java-Webanwendung** | Das Tool unterstützt zurzeit: <br/><br/> – Apps, die unter Tomcat 8 oder höher ausgeführt werden<br/> – Anwendungsserver mit Ubuntu Linux 16.04, 18.04 und 20.04, Debian 7 und 8, CentOS 6 und 7 oder Red Hat Enterprise Linux 5, 6 und 7 <br/> – Apps, für die Java 7 oder höher eingesetzt wird  <br/><br/> Das Tool unterstützt zurzeit nicht: <br/><br/> – Anwendungsserver, auf denen mehrere Tomcat-Instanzen ausgeführt werden <br/>  


## <a name="prepare-an-azure-user-account"></a>Vorbereiten eines Azure-Benutzerkontos

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen, bevor Sie beginnen.

Sobald Ihr Abonnement eingerichtet ist, benötigen Sie ein Azure-Benutzerkonto mit den folgenden Berechtigungen:
- Besitzerberechtigungen für das Azure-Abonnement
- Berechtigungen zum Registrieren von Azure Active Directory-Apps

Wenn Sie gerade erst ein kostenloses Azure-Konto erstellt haben, sind Sie der Besitzer Ihres Abonnements. Wenn Sie nicht der Besitzer des Abonnements sind, müssen Sie mit dem Besitzer zusammenarbeiten, um die Berechtigungen wie folgt zuzuweisen:

1. Suchen Sie im Azure-Portal nach „Abonnements“, und wählen Sie unter **Dienste** die Option **Abonnements** aus.

    ![Suchfeld, mit dem nach dem Azure-Abonnement gesucht wird](./media/tutorial-discover-vmware/search-subscription.png)

2. Wählen Sie auf der Seite **Abonnements** das Abonnement aus, in dem Sie ein Azure Migrate-Projekt erstellen möchten.
3. Wählen Sie im Abonnement die Option **Zugriffssteuerung (IAM)**  > **Zugriff überprüfen** aus.
4. Suchen Sie unter **Zugriff überprüfen** nach dem entsprechenden Benutzerkonto.
5. Klicken Sie unter **Rollenzuweisung hinzufügen** auf **Hinzufügen**.

    ![Suche nach einem Benutzerkonto, um den Zugriff zu überprüfen und eine Rolle zuzuweisen](./media/tutorial-discover-vmware/azure-account-access.png)

6. Wählen Sie unter **Rollenzuweisung hinzufügen** die Rolle „Besitzer“ und das Konto aus (in diesem Beispiel „azmigrateuser“). Klicken Sie anschließend auf **Speichern**.

    ![Die Seite „Rollenzuweisung“ wird geöffnet, auf der Sie dem Konto eine Rolle zuweisen können.](./media/tutorial-discover-vmware/assign-role.png)

7. Ihr Azure-Konto benötigt zudem **Berechtigungen zum Registrieren von Azure Active Directory-Apps**.
8. Navigieren Sie im Azure-Portal zu **Azure Active Directory** > **Benutzer** > **Benutzereinstellungen**.
9. Vergewissern Sie sich unter **Benutzereinstellungen**, dass Azure AD-Benutzer Anwendungen registrieren können (standardmäßig auf **Ja** festgelegt).

      ![Überprüfung in den Benutzereinstellungen, ob Benutzer Active Directory-Apps registrieren können](./media/tutorial-discover-vmware/register-apps.png)

10. Wenn die Einstellungen für „App-Registrierungen“ auf „Nein“ festgelegt sind, fordern Sie den Mandantenadministrator/globalen Administrator auf, die erforderliche Berechtigung zuzuweisen. Alternativ kann der Mandantenadministrator bzw. der globale Administrator einem Konto die Rolle **Anwendungsentwickler** zuweisen, um die Registrierung von Azure Active Directory-Apps zuzulassen. [Weitere Informationen](../active-directory/fundamentals/active-directory-users-assign-role-azure-portal.md)

## <a name="download-and-install-azure-migrate-app-containerization-tool"></a>Herunterladen und Installieren des App-Containerisierungstools von Azure Migrate

1. Laden Sie das [Installationsprogramm](https://go.microsoft.com/fwlink/?linkid=2134571) für das App-Containerisierungstool von Azure Migrate auf einen Windows-Computer herunter.
2. Starten Sie PowerShell im Administratormodus, und ändern Sie das PowerShell-Verzeichnis in den Ordner, der das Installationsprogramm enthält.
3. Führen Sie das Installationsskript mithilfe des folgenden Befehls aus:

   ```powershell
   .\AppContainerizationInstaller.ps1
   ```

## <a name="launch-the-app-containerization-tool"></a>Starten des App-Containerisierungstools

1. Öffnen Sie auf einem Computer, der eine Verbindung mit dem Windows-Computer herstellen kann, auf dem das Anwendungscontainerisierungstool ausgeführt wird, in einem Browser die URL für das Tool: **https://*Computername oder IP-Adresse* 44369**.

   Alternativ können Sie auch auf dem Desktop auf die App-Verknüpfung klicken, um die App zu öffnen.

2. Wenn eine Warnung angezeigt wird, dass die Verbindung nicht privat ist, klicken Sie auf „Erweitert“ und wählen Sie die Option aus, dennoch zur Website zu wechseln. Diese Warnung wird angezeigt, da die Webschnittstelle ein selbstsigniertes TLS/SSL-Zertifikat verwendet.
3. Melden Sie sich mit dem lokalen Administratorkonto auf dem Computer an.
4. Wählen Sie **Java-Web-Apps unter Tomcat** als Anwendungstyp aus, der containerisiert werden soll.
5. Für den Azure-Zieldienst wählen Sie **Container in Azure Kubernetes Service** aus.
    ![Standard-Load-up für das App-Containerisierungstool](./media/tutorial-containerize-apps-aks/tool-home.png)

### <a name="complete-tool-pre-requisites"></a>Erfüllen der Voraussetzungen für das Tool
1. Akzeptieren Sie die **Lizenzbedingungen**, und lesen Sie die Drittanbieterinformationen.
6. Gehen Sie in der Web-App für das Tool unter **Erforderliche Komponenten einrichten** wie folgt vor:
   - **Konnektivität**: Das Tool überprüft, ob der Windows-Computer über Internetzugriff verfügt. Falls der Computer einen Proxy verwendet:
     - Klicken Sie auf **Proxy einrichten**, um die Proxyadresse (im IP-Adressformat oder als FQDN) und den lauschenden Port anzugeben.
     - Geben Sie die Anmeldeinformationen an, wenn der Proxy eine Authentifizierung erfordert.
     - Es werden nur HTTP-Proxys unterstützt.
     - Wenn Sie Proxydetails hinzugefügt oder den Proxy und/oder die Authentifizierung deaktiviert haben, klicken Sie auf **Speichern**, um die Konnektivitätsprüfung noch mal auszulösen.
   - **Updates installieren**: Das Tool sucht automatisch nach den aktuellsten Updates und installiert diese. Sie können die aktuelle Version des Tools auch [hier](https://go.microsoft.com/fwlink/?linkid=2134571) herunterladen und manuell installieren.
   - **Secure Shell (SSH) aktivieren**: Das Tool zeigt eine Meldung an, damit Sie sich vergewissern, dass Secure Shell (SSH) auf den Anwendungsservern aktiviert ist, auf denen die zu containerisierenden Java-Web-Apps ausgeführt werden.


## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Klicken Sie auf **Anmelden**, um sich bei Ihrem Azure-Konto anzumelden.

1. Für die Authentifizierung bei Azure benötigen Sie einen Gerätecode. Wenn Sie auf „Anmelden“ klicken, wird ein modales Dialogfeld mit dem Gerätecode angezeigt.
2. Klicken Sie auf **Code kopieren und anmelden**, um den Gerätecode zu kopieren und eine Azure-Anmeldeaufforderung auf einer neuen Registerkarte im Browser zu öffnen. Sollte keine Aufforderung angezeigt werden, vergewissern Sie sich, dass Sie den Popupblocker im Browser deaktiviert haben.

    ![Modales Dialogfeld mit Gerätecode](./media/tutorial-containerize-apps-aks/login-modal.png)

3. Fügen Sie auf der neuen Registerkarte den Gerätecode ein, und melden Sie sich mit den Anmeldeinformationen für Ihr Azure-Konto an. Sobald Sie angemeldet sind, können Sie die Registerkarte im Browser schließen und zur Weboberfläche des App-Containerisierungstools zurückkehren.
4. Wählen Sie den **Azure-Mandanten** aus, den Sie verwenden möchten.
5. Geben Sie das gewünschte **Azure-Abonnement** an.

## <a name="discover-java-web-applications"></a>Ermitteln von Java-Web-Apps

Das App-Containerisierungstool stellt unter Verwendung der angegebenen Anmeldeinformationen eine Remoteverbindung mit den Anwendungsservern her und versucht, unter Apache Tomcat ausgeführte Java-Web-Apps zu ermitteln, die auf den Anwendungsservern gehostet werden.

1. Geben Sie die **IP-Adresse oder den FQDN und die Anmeldeinformationen** für den Server an, auf dem die Java-Web-App ausgeführt wird. Diese Daten sollen verwendet werden, um eine Remoteverbindung mit dem Server zur Anwendungsermittlung herzustellen.
    - Unter Linux müssen die angegebenen Anmeldeinformationen einem Rootkonto auf dem Anwendungsserver zugeordnet sein.
    - Bei Domänenkonten (der Benutzer muss Administrator auf dem Anwendungsserver sein) müssen Sie dem Domänennamen den Benutzernamen voranstellen ( *<Domäne\Benutzername>* ).
    - Sie können die Anwendungsermittlung für bis zu fünf Server gleichzeitig ausführen.

2. Klicken Sie auf **Überprüfen**, um sich zu vergewissern, dass der Anwendungsserver von dem Computer aus erreichbar ist, auf dem das Tool ausgeführt wird, und dass die Anmeldeinformationen gültig sind. Bei erfolgreicher Überprüfung wird der Status in der entsprechenden Spalte als **Zugeordnet** angezeigt.  

    ![Screenshot: IP-Adresse und Anmeldeinformationen für den Server](./media/tutorial-containerize-apps-aks/discovery-credentials.png)

3. Klicken Sie auf **Weiter**, um die Anwendungsermittlung auf den ausgewählten Anwendungsservern zu starten.

4. Nach erfolgreichem Abschluss der Anwendungsermittlung können Sie die Liste der zu containerisierenden Anwendungen auswählen.

    ![Screenshot: ermittelte Java-Web-App](./media/tutorial-containerize-apps-aks/discovered-app.png)


4. Wählen Sie die zu containerisierenden Anwendungen mithilfe der Kontrollkästchen aus.
5. **Containername angeben**: Legen Sie Namen für die Zielcontainer der ausgewählten Anwendungen fest. Der Containername sollte im Format <*Name:Tag*> angegeben werden, wobei „Tag“ für das Containerimage verwendet wird. Sie können den Zielcontainernamen beispielsweise auf *appname:v1* festlegen.   

### <a name="parameterize-application-configurations"></a>Parametrisieren von Anwendungskonfigurationen
Durch das Parametrisieren der Konfiguration steht diese zur Bereitstellungszeit als Parameter zur Verfügung. Das bedeutet, dass Sie diese Einstellung während der Anwendungsbereitstellung konfigurieren können, anstatt sie in einem bestimmten Wert im Containerimage hartzucodieren. Diese Option ist beispielsweise für Parameter wie Datenbank-Verbindungszeichenfolgen nützlich.
1. Klicken Sie auf **App-Konfigurationen**, um erkannte Konfigurationen anzuzeigen.
2. Aktivieren Sie das Kontrollkästchen, um die erkannten Anwendungskonfigurationen zu parametrisieren.
3. Wählen Sie die zu parametrisierenden Konfigurationen aus, und klicken Sie dann auf **Anwenden**.

   ![Screenshot: Parametrisierung der Konfiguration einer ASP.NET-App](./media/tutorial-containerize-apps-aks/discovered-app-configs.png)

### <a name="externalize-file-system-dependencies"></a>Externalisieren von Dateisystemabhängigkeiten

 Sie können weitere Ordner hinzufügen, die von der Anwendung verwendet werden. Legen Sie fest, ob diese Teil des Containerimages werden oder mithilfe persistenter Volumes in Azure-Dateifreigabe externalisiert werden sollen. Persistente Volumes sind für zustandsbehaftete Anwendungen ideal, die den Zustand außerhalb des Containers speichern oder statische Inhalte im Dateisystem gespeichert haben. [Weitere Informationen](../aks/concepts-storage.md)

1. Klicken Sie unter „App-Ordner“ auf **Bearbeiten**, um die erkannten Anwendungsordner anzuzeigen. Bei den erkannten Anwendungsordnern handelt es sich um für die Anwendung erforderliche Artefakte, deshalb werden sie ins Containerimage kopiert.

2. Klicken Sie auf **Ordner hinzufügen**, und geben Sie die Pfade der Ordner an, die hinzugefügt werden sollen.
3. Wenn Sie mehrere Ordner zum selben Volume hinzufügen möchten, müssen Sie diese mit einem Trennzeichen (`,`) trennen.
4. Wählen Sie **Persistentes Volume** als Speicheroption aus, wenn die Ordner außerhalb des Containers in einem persistenten Volume gespeichert werden sollen.
5. Klicken Sie nach der Überprüfung der Anwendungsordner auf **Speichern**.
   ![Screenshot: Speicherauswahl für App-Volumes](./media/tutorial-containerize-apps-aks/discovered-app-volumes.png)

6. Klicken Sie auf **Weiter**, um zur Buildphase des Containerimages zu wechseln.

## <a name="build-container-image"></a>Erstellen des Containerimage


1. **Azure Container Registry-Instanz auswählen**: Wählen Sie über das Dropdownmenü eine [Azure Container Registry-Instanz](../container-registry/index.yml) aus, die zum Erstellen und Speichern der Containerimages für die App verwendet werden soll. Sie können eine vorhandene Azure Container Registry-Instanz wählen oder über die Option „Create new registry“ (Neue Registrierung erstellen) eine neue erstellen.

    ![Screenshot: ACR-Auswahl für die App](./media/tutorial-containerize-apps-aks/build-java-app.png)


2. **Dockerfile überprüfen**: Das Dockerfile, das für die Erstellung der Containerimage für die ausgewählten Apps benötigt wird, wird zu Beginn des Buildschritts generiert. Klicken Sie auf **Überprüfen**, um das Dockerfile zu öffnen. Im Überprüfungsschritt können Sie auch sämtliche notwendigen Anpassungen am Dockerfile vornehmen und die Änderung speichern, bevor Sie den Buildprozess einleiten.

3. **Konfigurieren von Application Insights**: Sie können die Überwachung für Ihre Java-Apps aktivieren, die in App Service ausgeführt werden, ohne Ihren Code zu instrumentieren. Das Tool installiert den eigenständigen Java-Agent als Teil des Containerimages. Nach der Konfiguration während der Bereitstellung erfasst der Java-Agent automatisch eine Vielzahl von Anforderungen, Abhängigkeiten, Protokollen und Metriken für Ihre Anwendung, die für die Überwachung mit Application Insights verwendet werden können. Diese Option ist standardmäßig für alle Java-Anwendungen aktiviert.  

4. **Buildprozess auslösen**: Wählen Sie die Apps aus, für die Images erstellt werden sollen, und klicken Sie auf **Erstellen**. Wenn Sie auf „Erstellen“ klicken, werden die Containerimages für die einzelnen Apps erstellt. Das Tool überwacht den Buildstatus kontinuierlich. Wenn der Buildprozess abgeschlossen ist, können Sie mit dem nächsten Schritt fortfahren.

5. **Buildstatus nachverfolgen**: Sie können den Fortschritt des Buildschritts überwachen, indem Sie unter der Statusspalte auf **Buildvorgang wird ausgeführt** klicken. Der Link ist erst einige Minuten nach Beginn des Buildprozesses verfügbar.  

6. Wenn der Build abgeschlossen ist, klicken Sie auf **Weiter**, um die Bereitstellungseinstellungen festzulegen.

    ![Screenshot: Abschluss des Buildprozesses für App-Containerimages](./media/tutorial-containerize-apps-aks/build-java-app-completed.png)

## <a name="deploy-the-containerized-app-on-aks"></a>Bereitstellen der containerisierten App in AKS

Nachdem das Containerimage erstellt wurde, müssen Sie die App als Container in [Azure Kubernetes Service (AKS)](https://azure.microsoft.com/services/kubernetes-service/) bereitstellen.

1. **Azure Kubernetes Service-Cluster auswählen**: Geben Sie den AKS-Cluster an, in dem die App bereitgestellt werden soll.

     - Der ausgewählte AKS-Cluster muss über einen Linux-Knotenpool verfügen.
     - Der Cluster muss so konfiguriert werden, dass Images aus der Azure Container Registry-Instanz gepullt werden können, in der diese gespeichert werden.
         - Führen Sie den folgenden Befehl in der Azure CLI aus, um den AKS-Cluster an die ACR-Instanz anzufügen.
           ``` Azure CLI
           az aks update -n <cluster-name> -g <cluster-resource-group> --attach-acr <acr-name>
           ```  
     - Wenn Sie keinen AKS-Cluster besitzen oder einen neuen AKS-Cluster erstellen möchten, in dem die App bereitgestellt werden soll, können Sie hierzu im Tool auf **Create new AKS cluster** (Neuen AKS-Cluster erstellen) klicken.      
          - Das Tool erstellt den AKS-Cluster innerhalb eines Linux-Knotenpools. Der Cluster wird so konfiguriert, dass Images aus der Azure Container Registry-Instanz gepullt werden können, die zuvor erstellt wurde (falls diese Option ausgewählt wurde).
     - Wählen Sie den AKS-Cluster aus, und klicken Sie auf **Weiter**.
2. **Angeben des Geheimnisspeichers und Überwachungsarbeitsbereichs**: Wenn Sie sich für die Parametrisierung von Anwendungskonfigurationen entschieden haben, geben Sie den Geheimnisspeicher an, der für die Anwendung verwendet werden soll. Sie können Azure Key Vault oder Kubernetes-Geheimnisse für die Verwaltung Ihrer Anwendungsgeheimnisse auswählen.

     - Wenn Sie Kubernetes-Geheimnisse für die Verwaltung von Geheimnissen ausgewählt haben, klicken Sie auf **Weiter**.
     - Wenn Sie eine Azure Key Vault-Instanz zum Verwalten Ihrer Anwendungsgeheimnisse verwenden möchten, geben Sie die gewünschte Azure Key Vault-Instanz an.     
         - Wenn Sie nicht über eine Azure Key Vault-Instanz verfügen oder eine neue Key Vault-Instanz erstellen möchten, können Sie diese über das Tool erstellen, indem Sie auf **Neu erstellen** klicken.
         - Das Tool weist automatisch die erforderlichen Berechtigungen zum Verwalten von Geheimnissen über die Key Vault-Instanz zu.
    - **Überwachungsarbeitsbereich**: Wenn Sie die Überwachung mit Application Insights aktiviert haben, geben Sie die Application Insights-Ressource an, die Sie verwenden möchten. Diese Option ist nicht sichtbar, wenn Sie die Überwachungsintegration deaktiviert haben.
         - Wenn Sie nicht über eine Application Insights-Ressource verfügen oder eine neue Application Insights-Ressource erstellen möchten, können Sie diese über das Tool erstellen, indem Sie auf **Neu erstellen** klicken.

2. **Azure-Dateifreigabe angeben**: Wenn Sie mehrere Ordner hinzugefügt und die Option „Persistentes Volume“ ausgewählt haben, geben Sie die Azure-Dateifreigabe an, die das App-Containerisierungstool von Azure Migrate während des Bereitstellungsprozesses verwenden soll. Das Tool erstellt neue Verzeichnisse in dieser Azure-Dateifreigabe, um die Anwendungsordner zu kopieren, die für persistenten Volumespeicher konfiguriert sind. Sobald die Anwendung bereitgestellt wurde, bereinigt das Tool die Azure-Dateifreigabe, indem die erstellten Verzeichnisse gelöscht werden.

     - Wenn Sie keine Azure-Dateifreigabe besitzen oder eine neue erstellen möchten, können Sie hierzu im Tool auf **Create new Storage Account and file share** (Neues Speicherkonto und neue Dateifreigabe erstellen) klicken.  

3. **Konfiguration für die Anwendungsbereitstellung**: Nachdem Sie die obigen Schritte abgeschlossen haben, müssen Sie die Bereitstellungskonfiguration für die App festlegen. Klicken Sie auf **Konfigurieren**, um die Bereitstellung der App anzupassen. Im Konfigurationsschritt können Sie die folgenden Anpassungen vornehmen:
     - **Präfixzeichenfolge**: Geben Sie eine Präfixzeichenfolge an, die dem Namen aller Ressourcen vorangestellt werden soll, die für die containerisierte Anwendung im AKS-Cluster erstellt werden.
     - **Replikatgruppen**: Geben Sie an, wie viele Anwendungsinstanzen (Pods) innerhalb der Container ausgeführt werden sollen.
     - **Lastenausgleich**: Wählen Sie *Extern* aus, wenn die containerisierte Anwendung von öffentlichen Netzwerken aus erreichbar sein soll.
     - **Anwendungskonfiguration**: Geben Sie für parametrisierte Anwendungskonfigurationen die Werte an, die für die aktuelle Bereitstellung verwendet werden sollen.
     - **Speicher**: Machen Sie für sämtliche Anwendungsordner, die für den persistenten Volumespeicher konfiguriert wurden, eine Angabe, ob das Volume anwendungsinstanzübergreifend freigegeben oder einzeln mit jeder Instanz im Container initialisiert werden soll. Standardmäßig sind alle Anwendungsordner in persistenten Volumes als freigegeben konfiguriert.
     - Klicken Sie auf **Anwenden**, um die Bereitstellungskonfiguration zu speichern.
     - Klicken Sie auf **Weiter**, um die App bereitzustellen.

    ![Screenshot: Bereitstellungskonfiguration für die App](./media/tutorial-containerize-apps-aks/deploy-java-app-config-aks.png)

4. **App bereitstellen**: Sobald die Bereitstellungskonfiguration für die App gespeichert wurde, generiert das Tool die Kubernetes-Bereitstellungsdatei (YAML) für diese.
     - Klicken Sie auf **Bearbeiten**, um die Kubernetes-Bereitstellungsdatei (YAML) für die Anwendungen zu überprüfen und anzupassen.
     - Wählen Sie die bereitzustellende App aus.
     - Klicken Sie auf **Bereitstellen**, um mit der Bereitstellung der ausgewählten Apps zu beginnen.

         ![Screenshot: Bereitstellungskonfiguration für die App](./media/tutorial-containerize-apps-aks/deploy-java-app-deploy-aks.png)

     - Nachdem die App bereitgestellt wurde, können Sie auf die Spalte *Bereitstellungsstatus* klicken, um die Ressourcen nachzuverfolgen, die für die App bereitgestellt wurden.

## <a name="download-generated-artifacts"></a>Herunterladen generierter Artefakte

Sämtliche für die Erstellung und Bereitstellung der App in AKS verwendeten Artefakte (einschließlich des Dockerfile und der YAML-Spezifikationsdateien für Kubernetes) werden auf dem Computer gespeichert, auf dem das Tool ausgeführt wird. Die Artefakte befinden sich unter *C:\ProgramData\Microsoft Azure Migrate App Containerization*.

Für jeden Anwendungsserver wird ein separater Ordner erstellt. Sie können alle im Containerisierungsprozess verwendeten Zwischenartefakte anzeigen und herunterladen, indem Sie zu diesem Ordner navigieren. Die Ordner für Anwendungsserver werden immer dann bereinigt, wenn das Tool für einen bestimmten Server gestartet wird.

## <a name="troubleshoot-issues"></a>Behandeln von Problemen

Wenn Sie Probleme mit dem Tool behandeln müssen, sollten Sie sich die Protokolldateien auf dem Windows-Computer ansehen, auf dem das App-Containerisierungstool ausgeführt wird. Die Protokolldateien für das Tool befinden sich im Ordner *C:\ProgramData\Microsoft Azure Migrate App Containerization\Logs*.

## <a name="next-steps"></a>Nächste Schritte

- Containerisieren von Java-Web-Apps unter Apache Tomcat (auf Linux-Servern) und Bereitstellen dieser Apps in Linux-Containern in App Service. [Weitere Informationen](./tutorial-app-containerization-java-app-service.md)
- Containerisieren von ASP.NET-Web-Apps und Bereitstellen dieser Apps in Windows-Containern in AKS. [Weitere Informationen](./tutorial-app-containerization-aspnet-kubernetes.md)
- Containerisieren von ASP.NET-Web-Apps und Bereitstellen dieser Apps in Windows-Containern in Azure App Service. [Weitere Informationen](./tutorial-app-containerization-aspnet-app-service.md)
