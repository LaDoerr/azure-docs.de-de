---
title: 'Tutorial: Erstellen über das Portal: Web Application Firewall'
description: In diesem Tutorial erfahren Sie, wie Sie mithilfe des Azure-Portals ein Anwendungsgateway mit einer Web Application Firewall erstellen.
services: web-application-firewall
author: vhorne
ms.service: web-application-firewall
ms.topic: tutorial
ms.date: 05/19/2021
ms.author: victorh
ms.openlocfilehash: f72706d4bb1d9470518fb3b14ee756a1fe1551db
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110480544"
---
# <a name="tutorial-create-an-application-gateway-with-a-web-application-firewall-using-the-azure-portal"></a>Tutorial: Erstellen eines Anwendungsgateways mit einer Web Application Firewall über das Azure-Portal

In diesem Tutorial erfahren Sie, wie Sie mithilfe des Azure-Portals ein Anwendungsgateway mit einer Web Application Firewall (WAF) erstellen. Die WAF verwendet [OWASP](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project)-Regeln, um Ihre Anwendung zu schützen. Diese Regeln beinhalten den Schutz vor Angriffen z.B. durch Einschleusung von SQL-Befehlen, siteübergreifendes Scripting und Sitzungsübernahmen. Nach der Erstellung wird das Anwendungsgateway getestet, um sicherzustellen, dass es ordnungsgemäß funktioniert. Mit Azure Application Gateway leiten Sie den Webdatenverkehr Ihrer Anwendungen an bestimmte Ressourcen weiter, indem Sie Ports Listener zuweisen, Regeln erstellen und Ressourcen zu einem Back-End-Pool hinzufügen. Der Einfachheit halber wird in diesem Tutorial ein einfaches Setup mit einer öffentlichen Front-End-IP-Adresse, einem grundlegenden Listener zum Hosten einer einzelnen Website auf diesem Anwendungsgateway, zwei virtuellen Computern für den Back-End-Pool und einer Routingregel für grundlegende Anforderungen verwendet.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines Anwendungsgateways mit aktivierter WAF
> * Erstellen der virtuellen Computer, die als Back-End-Server verwendet werden
> * Erstellen eines Speicherkontos und Konfigurieren der Diagnose
> * Testen des Anwendungsgateways

![Web Application Firewall – Beispiel](../media/application-gateway-web-application-firewall-portal/scenario-waf.png)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

<!---If you prefer, you can complete this tutorial using [Azure PowerShell](tutorial-restrict-web-traffic-powershell.md) or [Azure CLI](tutorial-restrict-web-traffic-cli.md).--->

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="create-an-application-gateway"></a>Erstellen eines Anwendungsgateways

1. Klicken Sie im Azure-Portal im Menü auf der linken Seite auf **Ressource erstellen**. Das Fenster **Neu** wird angezeigt.

2. Klicken Sie auf **Netzwerk**, und wählen Sie dann in der Liste **Ausgewählte** die Option **Application Gateway** aus.

### <a name="basics-tab"></a>Registerkarte „Grundlagen“

1. Geben Sie auf der Registerkarte **Grundlagen** die folgenden Werte für die Einstellungen des Anwendungsgateways ein:

   - **Ressourcengruppe**: Wählen Sie **myResourceGroupAG** als Ressourcengruppe aus. Falls diese Gruppe nicht vorhanden ist, wählen Sie **Neue erstellen** aus, um sie zu erstellen.
   - **Name des Anwendungsgateways**: Geben Sie *myAppGateway* als Namen des Anwendungsgateways ein.
   - **Ebene**: Wählen Sie die Option **WAF V2** aus.

     ![Erstellen eines neuen Anwendungsgateways: Grundlagen](../media/application-gateway-web-application-firewall-portal/application-gateway-create-basics.png)

2.  Für die Kommunikation in Azure zwischen den von Ihnen erstellten Ressourcen ist ein virtuelles Netzwerk erforderlich. Sie können ein neues virtuelles Netzwerk erstellen oder ein bereits vorhandenes virtuelles Netzwerk auswählen. In diesem Beispiel erstellen Sie gleichzeitig mit dem Anwendungsgateway ein virtuelles Netzwerk. Application Gateway-Instanzen werden in separaten Subnetzen erstellt. In diesem Beispiel erstellen Sie zwei Subnetze: eins für das Anwendungsgateway und eins für die Back-End-Server.

    Wählen Sie unter **Virtuelles Netzwerk konfigurieren** die Option **Neu erstellen** aus, um ein neues virtuelles Netzwerk zu erstellen. Geben Sie im Fenster **Virtuelles Netzwerk erstellen**, das geöffnet wird, die folgenden Werte ein,um das virtuelle Netzwerk und zwei Subnetze zu erstellen:

    - **Name**: Geben Sie *myVNet* als Namen des virtuellen Netzwerks ein.

    - **Subnetzname** (Application Gateway-Subnetz): Im Raster **Subnetze** wird ein Subnetz namens *Default* angezeigt. Ändern Sie den Namen dieses Subnetzes in *myAGSubnet*.<br>Das Subnetz für das Anwendungsgateway kann nur Anwendungsgateways enthalten. Andere Ressourcen sind nicht zulässig.

    - **Subnetzname** (Subnetz für Back-End-Server): Geben Sie in der zweiten Zeile des Rasters **Subnetze** in der Spalte *Subnetzname* den Namen **myBackendSubnet** ein.

    - **Adressbereich** (Subnetz für Back-End-Server): Geben Sie in der zweiten Zeile des Rasters **Subnetze** einen Adressbereich ein, der sich nicht mit dem Adressbereich des Subnetzes *myAGSubnet* überschneidet. Wenn der Adressbereich von *myAGSubnet* beispielsweise 10.21.0.0/24 lautet, geben Sie *10.21.1.0/24* als Adressbereich für *myBackendSubnet* ein.

    Wählen Sie **OK** aus, um das Fenster **Virtuelles Netzwerk erstellen** zu schließen und die Einstellungen für das virtuelle Netzwerk zu speichern.

     ![Erstellen eines neuen Anwendungsgateways: virtuelles Netzwerk](../media/application-gateway-web-application-firewall-portal/application-gateway-create-vnet.png)
    
3. Übernehmen Sie auf der Registerkarte **Grundlagen** die Standardwerte für die übrigen Einstellungen, und wählen Sie dann **Weiter: Front-Ends** aus.

### <a name="frontends-tab"></a>Registerkarte „Front-Ends“

1. Vergewissern Sie sich auf der Registerkarte **Front-Ends**, dass der **Typ der Front-End-IP-Adresse** auf **Öffentlich** festgelegt ist. <br>Je nach Anwendungsfall können Sie konfigurieren, dass die Front-End-IP-Adresse öffentlich oder privat ist. In diesem Beispiel verwenden Sie eine öffentliche Front-End-IP-Adresse.
   > [!NOTE]
   > Für die Application Gateway v2-SKU können Sie nur die **öffentliche** Front-End-IP-Konfiguration wählen. Die private Front-End-IP-Konfiguration ist derzeit für diese v2-SKU nicht aktiviert.

2. Wählen Sie **Neu hinzufügen** für **Öffentliche IP-Adresse** aus, und geben Sie *myAGPublicIPAddress* als Namen der öffentlichen IP-Adresse ein. Wählen Sie dann **OK** aus. 

     ![Erstellen eines neuen Anwendungsgateways: Front-Ends](../media/application-gateway-web-application-firewall-portal/application-gateway-create-frontends.png)

3. Klicken Sie auf **Weiter: Back-Ends**.

### <a name="backends-tab"></a>Registerkarte „Back-Ends“

Der Back-End-Pool wird zum Weiterleiten von Anforderungen an die Back-End-Server verwendet, die die Anforderung verarbeiten. Back-End-Pools können Netzwerkkarten, VM-Skalierungsgruppen, öffentliche IP-Adressen, interne IP-Adressen, vollqualifizierte Domänennamen (Fully Qualified Domain Names, FQDN) und Back-Ends mit mehreren Mandanten wie Azure App Service umfassen. In diesem Beispiel erstellen Sie einen leeren Back-End-Pool mit Ihrem Anwendungsgateway und fügen ihm später Back-End-Ziele hinzu.

1. Wählen Sie auf der Registerkarte **Back-Ends** die Option **Back-End-Pool hinzufügen** aus.

2. Geben Sie im Fenster **Back-End-Pool hinzufügen**, das geöffnet wird, die folgenden Werte ein, um einen leeren Back-End-Pool zu erstellen:

    - **Name**: Geben Sie *myBackendPool* als Name des Back-End-Pools ein.
    - **Back-End-Pool ohne Ziele hinzufügen**: Wählen Sie **Ja** aus, um einen Back-End-Pool ohne Ziele zu erstellen. Back-End-Ziele werden Sie nach dem Erstellen des Anwendungsgateways hinzufügen.

3. Wählen Sie im Fenster **Back-End-Pool hinzufügen** die Option **Hinzufügen** aus, um die Konfiguration des Back-End-Pools zu speichern und zur Registerkarte **Back-Ends** zurückzukehren.

     ![Erstellen eines neuen Anwendungsgateways: Back-Ends](../media/application-gateway-web-application-firewall-portal/application-gateway-create-backends.png)

4. Wählen Sie auf der Registerkarte **Back-Ends** die Option **Weiter: Konfiguration** aus.

### <a name="configuration-tab"></a>Registerkarte „Konfiguration“

Auf der Registerkarte **Konfiguration** verbinden Sie das Front-End und den von Ihnen erstellten Back-End-Pool mithilfe einer Routingregel.

1. Wählen Sie in der Spalte **Routingregeln** die Option **Routingregel hinzufügen** aus.

2. Geben Sie im Fenster **Routingregel hinzufügen**, das geöffnet wird, *myRoutingRule* als **Regelname** ein.

3. Eine Routingregel erfordert einen Listener. Geben Sie im Fenster **Routingregel hinzufügen** auf der Registerkarte **Listener** die folgenden Werte für den Listener ein:

    - **Name des Listeners**: Geben Sie *myListener* als Name für den Listener ein.
    - **Front-End-IP**: Wählen Sie **Öffentlich** aus, um die für das Front-End erstellte öffentliche IP-Adresse auszuwählen.
  
      Übernehmen Sie auf der Registerkarte **Listener** die Standardwerte für die übrigen Einstellungen. Wählen Sie dann die Registerkarte **Back-End-Ziele** aus, um den Rest der Routingregel zu konfigurieren.

   ![Erstellen eines neuen Anwendungsgateways: Listener](../media/application-gateway-web-application-firewall-portal/application-gateway-create-rule-listener.png)

4. Wählen Sie auf der Registerkarte **Back-End-Ziele** den Pool **myBackendPool** als **Back-End-Ziel** aus.

5. Wählen Sie für die **HTTP-Einstellung** die Option **Neu hinzufügen** aus, um eine neue HTTP-Einstellung zu erstellen. Die HTTP-Einstellung bestimmt das Verhalten der Routingregel. Geben Sie im Fenster **HTTP-Einstellung hinzufügen**, das geöffnet wird, *myHTTPSetting* als **Name der HTTP-Einstellung** ein. Übernehmen Sie im Fenster **HTTP-Einstellung hinzufügen** die Standardwerte für die übrigen Einstellungen, und wählen Sie dann **Hinzufügen** aus, um zum Fenster **Routingregel hinzufügen** zurückzukehren. 

     ![Erstellen eines neuen Anwendungsgateways: HTTP-Einstellung](../media/application-gateway-web-application-firewall-portal/application-gateway-create-httpsetting.png)

6. Wählen Sie im Fenster **Routingregel hinzufügen** die Option **Hinzufügen** aus, um die Routingregel zu speichern und zur Registerkarte **Konfiguration** zurückzukehren.

     ![Erstellen eines neuen Anwendungsgateways: Routingregel](../media/application-gateway-web-application-firewall-portal/application-gateway-create-rule-backends.png)

7. Klicken Sie auf **Weiter: Tags** und dann auf **Weiter: Überprüfen + erstellen**.

### <a name="review--create-tab"></a>Registerkarte „Überprüfen und erstellen“

Überprüfen Sie die Einstellungen auf der Registerkarte **Überprüfen und erstellen**, und wählen Sie dann **Erstellen** aus, um das virtuelle Netzwerk, die öffentliche IP-Adresse und das Anwendungsgateway zu erstellen. Die Erstellung des Anwendungsgateways in Azure kann einige Minuten in Anspruch nehmen. 

Warten Sie, bis die Bereitstellung erfolgreich abgeschlossen ist, bevor Sie mit dem nächsten Abschnitt fortfahren.

## <a name="add-backend-targets"></a>Hinzufügen von Back-End-Zielen

In diesem Beispiel verwenden Sie virtuelle Computer als Ziel-Back-End. Sie können entweder vorhandene virtuelle Computer verwenden oder neue erstellen. Sie erstellen zwei virtuelle Computer, die von Azure als Back-End-Server für das Anwendungsgateway verwendet werden.

Gehen Sie dazu wie folgt vor:

1. Erstellen von zwei neuen virtuellen Computern (*myVM* und *myVM2*) für die Verwendung als Back-End-Server
2. Installieren von IIS auf den virtuellen Computern, um zu überprüfen, ob die Application Gateway-Instanz erfolgreich erstellt wurde
3. Hinzufügen der Back-End-Server zum Back-End-Pool

### <a name="create-a-virtual-machine"></a>Erstellen eines virtuellen Computers

1. Klicken Sie im Azure-Portal auf **Ressource erstellen**. Das Fenster **Neu** wird angezeigt.
2. Wählen Sie in der Liste **Beliebt** die Option **Windows Server 2019 Datacenter** aus. Die Seite **Virtuellen Computer erstellen** wird angezeigt.<br>Application Gateway kann Datenverkehr an jeden beliebigen virtuellen Computer weiterleiten, der im Back-End-Pool verwendet wird. In diesem Beispiel verwenden Sie Windows Server 2019 Datacenter.
3. Geben Sie auf der Registerkarte **Grundlagen** die folgenden Werte für die VM-Einstellungen ein:

    - **Ressourcengruppe**: Wählen Sie **myResourceGroupAG** als Namen der Ressourcengruppe aus.
    - **Name des virtuellen Computers**: Geben Sie *myVM* als Namen der VM ein.
    - **Benutzername**: Geben Sie einen Benutzernamen für den Administrator ein.
    - **Kennwort**: Geben Sie ein Administratorkennwort ein.
    - **Öffentliche Eingangsports**: Wählen Sie **Keine** aus.
4. Übernehmen Sie für die anderen Einstellungen die Standardwerte, und klicken Sie auf **Weiter: Datenträger**.  
5. Übernehmen Sie auf der Registerkarte **Datenträger** die Standardwerte, und klicken Sie auf **Weiter: Netzwerk**.
6. Vergewissern Sie sich auf der Registerkarte **Netzwerk**, dass **myVNet** für **Virtuelles Netzwerk** ausgewählt und **Subnetz** auf **myBackendSubnet** festgelegt ist.
1. Wählen Sie unter **Öffentliche IP** die Option **Keine** aus.
1. Übernehmen Sie für die anderen Einstellungen die Standardwerte, und klicken Sie auf **Weiter: Verwaltung** aus.
1. Legen Sie auf der Registerkarte **Verwaltung** die Option **Startdiagnose** auf **Deaktivieren** fest. Übernehmen Sie für die anderen Einstellungen die Standardwerte, und klicken Sie auf **Bewerten + erstellen**.
1. Überprüfen Sie auf der Registerkarte **Bewerten + erstellen** die Einstellungen, beheben Sie alle Validierungsfehler, und wählen Sie dann **Erstellen** aus.
1. Warten Sie, bis die Erstellung des virtuellen Computers abgeschlossen ist, bevor Sie fortfahren.

### <a name="install-iis-for-testing"></a>Installieren von IIS für Testzwecke

In diesem Beispiel installieren Sie IIS auf den virtuellen Computern nur, um zu überprüfen, ob Azure das Anwendungsgateway erfolgreich erstellt hat.

1. Öffnen Sie [Azure PowerShell](../../cloud-shell/quickstart-powershell.md). Wählen Sie dazu in der oberen Navigationsleiste des Azure-Portals **Cloud Shell** und dann in der Dropdownliste **PowerShell** aus. 

    ![Installieren der benutzerdefinierten Erweiterung](../media/application-gateway-web-application-firewall-portal/application-gateway-extension.png)

2. Legen Sie den location-Parameter für Ihre Umgebung fest, und führen Sie anschließend den folgenden Befehl aus, um IIS auf dem virtuellen Computer zu installieren: 

    ```azurepowershell-interactive
    Set-AzVMExtension `
      -ResourceGroupName myResourceGroupAG `
      -ExtensionName IIS `
      -VMName myVM `
      -Publisher Microsoft.Compute `
      -ExtensionType CustomScriptExtension `
      -TypeHandlerVersion 1.4 `
      -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
      -Location EastUS
    ```

3. Erstellen Sie eine zweite VM, und installieren Sie IIS mithilfe der zuvor ausgeführten Schritte. Verwenden Sie *myVM2* für den VM-Namen und für die Einstellung **VMName** des Cmdlets **Set-AzVMExtension**.

### <a name="add-backend-servers-to-backend-pool"></a>Hinzufügen von Back-End-Servern zu Back-End-Pools

1. Wählen Sie **Alle Ressourcen** und dann **myAppGateway** aus.

2. Klicken Sie im Menü auf der linken Seite auf **Back-End-Pools**.

3. Wählen Sie **myBackendPool** aus.

4. Wählen Sie unter **Zieltyp** in der Dropdownliste die Option **Virtueller Computer** aus.

5. Wählen Sie unter **Ziel** in der Dropdownliste die zugehörige Netzwerkschnittstelle für **myVM** aus.
1. Wiederholen Sie den Schritt für **myVM2**.

   :::image type="content" source="../media/application-gateway-web-application-firewall-portal/application-gateway-backend.png" alt-text="Hinzufügen von Back-End-Servern":::


6. Wählen Sie **Speichern** aus.

7. Warten Sie, bis die Bereitstellung abgeschlossen ist, bevor Sie mit dem nächsten Schritt fortfahren.

   
## <a name="create-and-link-a-web-application-firewall-policy"></a>Erstellen und Verknüpfen einer Web Application Firewall-Richtlinie

Alle WAF-Anpassungen und -Einstellungen befinden sich in einem separaten Objekt, das als WAF-Richtlinie bezeichnet wird. Die Richtlinie muss Ihrem Anwendungsgateway zugeordnet werden. 

Erstellen Sie mithilfe des verwalteten Standardregelsatzes (Default Rule Set, DRS) eine einfache WAF-Richtlinie.

1. Wählen Sie links oben im Portal die Option **Ressource erstellen** aus. Suchen Sie nach **WAF**, wählen Sie **Web Application Firewall** aus, und wählen Sie dann **Erstellen** aus.
2. Geben Sie auf der Seite **WAF-Richtlinie erstellen** auf der Registerkarte **Grundlagen** die folgenden Informationen ein, oder wählen Sie sie aus. Übernehmen Sie die Standardwerte für die übrigen Einstellungen, und wählen Sie **Überprüfen + erstellen** aus:

   |Einstellung  |Wert  |
   |---------|---------|
   |Richtlinie für     |Regionale WAF (Application Gateway)|
   |Subscription     |Wählen Sie Ihren Abonnementnamen aus.|
   |Resource group     |Wählen Sie **myResourceGroupAG** aus.|
   |Richtlinienname     |Geben Sie einen eindeutigen Namen für Ihre WAF-Richtlinie ein.|
1. Wählen Sie **Weiter: Verwaltete Regeln** aus.
1. Übernehmen Sie die Standardeinstellungen, und wählen Sie **Weiter: Richtlinieneinstellungen** aus.
1. Übernehmen Sie die Standardeinstellungen, und wählen Sie **Weiter: Benutzerdefinierte Regeln** aus.
1. Wählen Sie **Weiter: Zuordnung** aus.
1. Wählen Sie **Zuordnung hinzufügen** und dann **Application Gateway** aus.
1. Aktivieren Sie das Kontrollkästchen **Web Application Firewall-Richtlinienkonfiguration auch dann anwenden, wenn sie von der aktuellen Konfiguration abweicht**.
1. Wählen Sie **Hinzufügen**.

   > [!NOTE]
   > Wenn Sie Ihrer Application Gateway-Instanz (oder Ihrem Listener) eine Richtlinie zuweisen, jedoch gilt dafür bereits eine Richtlinie, wird die ursprüngliche Richtlinie überschrieben und durch die neue Richtlinie ersetzt.
4. Klicken Sie auf **Überprüfen + erstellen** und dann auf **Erstellen**.

## <a name="test-the-application-gateway"></a>Testen des Anwendungsgateways

IIS ist für die Erstellung des Anwendungsgateways zwar nicht erforderlich, Sie haben die Installation aber ausgeführt, um zu überprüfen, ob Azure das Anwendungsgateway erfolgreich erstellt hat. Testen des Anwendungsgateways mit IIS:

1. Suchen Sie auf der Seite **Übersicht** des Anwendungsgateways nach der öffentlichen IP-Adresse für das Anwendungsgateway. ![Aufzeichnen der öffentlichen IP-Adresse des Anwendungsgateways](../media/application-gateway-web-application-firewall-portal/application-gateway-record-ag-address.png) 

   Alternativ können Sie **Alle Ressourcen** auswählen, *myAGPublicIPAddress* in das Suchfeld eingeben und den Eintrag dann in den Suchergebnissen auswählen. Azure zeigt die öffentliche IP-Adresse auf der Seite **Übersicht** an.
1. Kopieren Sie die öffentliche IP-Adresse, und fügen Sie sie in die Adressleiste des Browsers ein.
1. Überprüfen Sie die Antwort. Eine gültige Antwort bestätigt, dass das Anwendungsgateway erfolgreich erstellt wurde und eine Verbindung mit dem Back-End herstellen kann.

   ![Testen des Anwendungsgateways](../media/application-gateway-web-application-firewall-portal/application-gateway-iistest.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die mit dem Anwendungsgateway erstellten Ressourcen nicht mehr benötigen, entfernen Sie die Ressourcengruppe. Beim Entfernen der Ressourcengruppe werden auch das Anwendungsgateway und alle zugehörigen Ressourcen entfernt. 

So entfernen Sie die Ressourcengruppe:

1. Wählen Sie im Azure-Portal im Menü auf der linken Seite die Option **Ressourcengruppen** aus.
2. Suchen Sie auf der Seite **Ressourcengruppen** in der Liste nach **myResourceGroupAG**, und wählen Sie den Eintrag aus.
3. Wählen Sie auf der Seite für die **Ressourcengruppe** die Option **Ressourcengruppe löschen** aus.
4. Geben Sie *myResourceGroupAG* für **RESSOURCENGRUPPENNAMEN EINGEBEN** ein, und wählen Sie **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erfahren Sie mehr über die Web Application Firewall](../overview.md).