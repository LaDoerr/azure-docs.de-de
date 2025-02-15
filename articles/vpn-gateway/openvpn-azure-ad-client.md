---
title: 'Konfigurieren von VPN-Clients für P2S-OpenVPN-Protokollverbindungen: Azure AD-Authentifizierung'
description: Erfahren Sie, wie Sie einen VPN-Client konfigurieren, um über VPN Gateway Point-to-Site-VPN und Azure Active Directory-Authentifizierung eine Verbindung mit einem virtuellen Netzwerk herzustellen.
titleSuffix: Azure VPN Gateway
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 08/20/2021
ms.author: cherylmc
ms.openlocfilehash: a004e8d7fd87cc7abdf881b355b40b59ac874ba1
ms.sourcegitcommit: 28cd7097390c43a73b8e45a8b4f0f540f9123a6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/24/2021
ms.locfileid: "122779736"
---
# <a name="azure-active-directory-authentication-configure-a-vpn-client-for-p2s-openvpn-protocol-connections"></a>Azure Active Directory-Authentifizierung: Konfigurieren eines VPN-Clients für P2S-OpenVPN-Protokollverbindungen

Dieser Artikel unterstützt Sie beim Konfigurieren eines VPN-Clients für die Verbindung zu einem virtuellen Netzwerk mit einem Point-to-Site-VPN und Azure Active Directory-Authentifizierung. Damit Sie mit Azure AD eine Verbindung herstellen und eine Authentifizierung durchführen können, müssen Sie zuerst Ihren Azure AD-Mandanten konfigurieren. Weitere Informationen finden Sie unter [Konfigurieren eines Azure AD-Mandanten](openvpn-azure-ad-tenant.md). Weitere Informationen zu Point-to-Site-VPN-Verbindungen finden Sie unter [Informationen zu Point-to-Site-VPN](point-to-site-about.md).

[!INCLUDE [OpenVPN note](../../includes/vpn-gateway-openvpn-auth-include.md)]

## <a name="working-with-client-profiles"></a><a name="profile"></a>Arbeiten mit Clientprofilen

Für jeden Computer, der über den VPN-Client eine Verbindung mit dem VNet herstellen möchte, müssen Sie den Azure VPN Client für den Computer herunterladen und auch ein VPN-Clientprofil konfigurieren. Wenn Sie mehrere Computer konfigurieren möchten, können Sie ein Clientprofil auf einem Computer erstellen, exportieren und dann auf andere Computer importieren.

### <a name="to-download-the-azure-vpn-client"></a>So laden Sie Azure VPN Client herunter

[!INCLUDE [Download Azure VPN client](../../includes/vpn-gateway-download-vpn-client.md)]

### <a name="to-create-a-certificate-based-client-profile"></a><a name="cert"></a>So erstellen Sie ein zertifikatbasiertes Clientprofil

Bei Verwendung eines zertifikatbasierten Profils müssen die entsprechenden Zertifikate auf dem Clientcomputer installiert sein. Weitere Informationen zu Zertifikaten finden Sie unter [Installieren von Clientzertifikaten](point-to-site-how-to-vpn-client-install-azure-cert.md).

![Screenshot der Zertifikatauthentifizierung.](./media/openvpn-azure-ad-client/create/create-cert1.jpg)

### <a name="to-create-a-radius-client-profile"></a><a name="radius"></a>So erstellen Sie ein RADIUS-Clientprofil

![Screenshot der RADIUS-Authentifizierung.](./media/openvpn-azure-ad-client/create/create-radius1.jpg)
  
> [!NOTE]
> Der geheime Serverschlüssel kann in das P2S-VPN-Clientprofil exportiert werden.  Anweisungen zum Exportieren eines Clientprofils finden Sie [hier](about-vpn-profile-download.md).
>

### <a name="to-export-and-distribute-a-client-profile"></a><a name="export"></a>So exportieren und verteilen Sie ein Clientprofil

Sobald Sie über ein Arbeitsprofil verfügen und es an andere Benutzer verteilen müssen, können Sie es mit den folgenden Schritten exportieren:

1. Markieren Sie das VPN-Clientprofil, das Sie exportieren möchten, wählen Sie **...** und dann **Exportieren** aus.

    ![Screenshot mit der Seite „Azure-VPN-Client“, auf der die Auslassungspunkte ausgewählt sind und „Exportieren“ hervorgehoben ist](./media/openvpn-azure-ad-client/export/export1.jpg)

2. Wählen Sie den Speicherort aus, an dem Sie dieses Profil speichern möchten, lassen Sie den Dateinamen unverändert, und wählen Sie **Speichern** aus, um die XML-Datei zu speichern.

    ![Export](./media/openvpn-azure-ad-client/export/export2.jpg)

### <a name="to-import-a-client-profile"></a><a name="import"></a>So importieren Sie ein Clientprofil

1. Wählen Sie auf der Seite **Importieren** aus.

    ![Screenshot mit ausgewählter Schaltfläche „Hinzufügen“ und hervorgehobener „Importieren“-Aktion unten links im Fenster](./media/openvpn-azure-ad-client/import/import1.jpg)

2. Navigieren Sie zur XML-Datei für das Profil und wählen Sie sie aus. Wählen Sie bei ausgewählter Datei **Öffnen** aus.

    ![Screenshot, in dem eine ausgewählte Profil-XML-Datei gezeigt ist](./media/openvpn-azure-ad-client/import/import2.jpg)

3. Geben Sie den Namen des Profils an und wählen Sie **Speichern** aus.

    ![Screenshot mit hervorgehobenem „Verbindungsname“ und ausgewählter Schaltfläche „Speichern“](./media/openvpn-azure-ad-client/import/import3.jpg)

4. Wählen Sie **Verbinden** aus, um die Verbindung zum VPN herzustellen.

    ![Screenshot mit ausgewähltem VPN und ausgewählter Schaltfläche „Verbinden“](./media/openvpn-azure-ad-client/import/import4.jpg)

5. Nachdem die Verbindung hergestellt wurde, wird das Symbol grün und **Verbunden** angezeigt.

    ![Import](./media/openvpn-azure-ad-client/import/import5.jpg)

### <a name="to-delete-a-client-profile"></a><a name="delete"></a>So löschen Sie ein Clientprofil

1. Wählen Sie die Auslassungszeichen neben dem Clientprofil aus, das Sie löschen möchten. Wählen Sie dann **Entfernen** aus.

    ![Screenshot mit ausgewählten Auslassungspunkten und ausgewählter Option „Entfernen“](./media/openvpn-azure-ad-client/delete/delete1.jpg)

2. Wählen Sie **Entfernen** aus, um den Löschvorgang durchzuführen.

    ![delete](./media/openvpn-azure-ad-client/delete/delete2.jpg)

## <a name="create-a-connection"></a><a name="connection"></a>Erstellen einer Verbindung

1. Wählen Sie auf der Seite **+** aus und klicken Sie dann auf **+ Hinzufügen**.

    ![Screenshot mit ausgewählter Schaltfläche „Hinzufügen“](./media/openvpn-azure-ad-client/create/create1.jpg)

2. Füllen Sie die Verbindungsinformationen aus. Wenden Sie sich an Ihren Administrator, wenn Sie die Werte nicht kennen. Wählen Sie nach Eingabe der Werte **Speichern** aus.

    ![Screenshot mit den hervorgehobenen Eigenschaften der VPN-Verbindung und ausgewählter Schaltfläche „Speichern“](./media/openvpn-azure-ad-client/create/create2.jpg)

3. Wählen Sie **Verbinden** aus, um die Verbindung zum VPN herzustellen.

    ![Screenshot mit ausgewählter Schaltfläche „Verbinden“](./media/openvpn-azure-ad-client/create/create3.jpg)

4. Wählen Sie die richtigen Anmeldeinformationen und dann **Weiter** aus.

    ![Screenshot mit hervorgehobenen Beispielanmeldeinformationen und ausgewählter Schaltfläche „Weiter“](./media/openvpn-azure-ad-client/create/create4.jpg)

5. Nachdem die Verbindung erfolgreich hergestellt wurde, wird das Symbol grün und **Verbunden** angezeigt.

    ![connection](./media/openvpn-azure-ad-client/create/create5.jpg)

### <a name="to-connect-automatically"></a><a name="autoconnect"></a>So stellen Sie die Verbindung automatisch her

Diese Schritte unterstützen Sie beim Konfigurieren der Verbindung, sodass sie automatische hergestellt wird und immer aktiv ist.

1. Wählen Sie auf der Homepage für den VPN-Client **VPN-Einstellungen** aus.

    ![Screenshot der VPN-Startseite mit ausgewählter Option „VPN-Einstellungen“](./media/openvpn-azure-ad-client/auto/auto1.jpg)

2. Wählen Sie im Dialogfeld zum Wechseln von Apps **Ja** aus.

    ![Screenshot des Dialogfelds „Wollten Sie die App wechseln?“ mit ausgewählter Schaltfläche „Ja“](./media/openvpn-azure-ad-client/auto/auto2.jpg)

3. Vergewissern Sie sich, dass die Verbindung, die Sie festlegen möchten, noch nicht hergestellt ist, markieren Sie dann das Profil und aktivieren Sie das Kontrollkästchen **Automatisch verbinden**.

    ![Screenshot des Fensters „Einstellungen“ mit aktiviertem Kontrollkästchen „Automatisch verbinden“](./media/openvpn-azure-ad-client/auto/auto3.jpg)

4. Wählen Sie **Verbinden** aus, um die VPN-Verbindung zu initiieren.

    ![auto](./media/openvpn-azure-ad-client/auto/auto4.jpg)

## <a name="diagnose-connection-issues"></a><a name="diagnose"></a>Diagnose von Verbindungsproblemen

1. Zum Diagnostizieren von Verbindungsproblemen können Sie das **Diagnose**-Tool verwenden. Wählen Sie die **...** neben der VPN-Verbindung aus, die Sie diagnostizieren möchten, um das Menü einzublenden. Wählen Sie dann **Diagnose** aus.

    ![Screenshot, in dem die Auslassungspunkte und „Diagnose“ ausgewählt sind](./media/openvpn-azure-ad-client/diagnose/diagnose1.jpg)

2. Wählen Sie auf der Seite **Verbindungseigenschaften** die Option **Diagnose ausführen** aus.

    ![Screenshot der Seite „Verbindungseigenschaften“ mit ausgewählter Option „Diagnose ausführen“](./media/openvpn-azure-ad-client/diagnose/diagnose2.jpg)

3. Melden Sie sich mit Ihren Anmeldeinformationen an.

    ![Screenshot des Dialogfelds „Melden Sie sich an“ mit einem ausgewählten „Geschäfts-, Schul- oder Unikonto“](./media/openvpn-azure-ad-client/diagnose/diagnose3.jpg)

4. Zeigen Sie die Diagnoseergebnisse an.

    ![Diagnose](./media/openvpn-azure-ad-client/diagnose/diagnose4.jpg)

## <a name="faq"></a>Häufig gestellte Fragen

### <a name="is-the-azure-vpn-client-supported-with-windows-fips-mode"></a>Wird der Azure VPN Client im Windows-FIPS-Modus unterstützt?

Ja, mit dem Hotfix [KB4577063](https://support.microsoft.com/help/4577063/windows-10-update-kb4577063).

### <a name="how-do-i-add-dns-suffixes-to-the-vpn-client"></a>Wie füge ich DNS-Suffixe zum VPN-Client hinzu?

Sie können die heruntergeladene XML-Datei für das Profil ändern und die Tags **\<dnssuffixes>\<dnssufix> \</dnssufix>\</dnssuffixes>** hinzufügen.

```
<azvpnprofile>
<clientconfig>

    <dnssuffixes>
          <dnssuffix>.mycorp.com</dnssuffix>
          <dnssuffix>.xyz.com</dnssuffix>
          <dnssuffix>.etc.net</dnssuffix>
    </dnssuffixes>
    
</clientconfig>
</azvpnprofile>
```

### <a name="how-do-i-add-custom-dns-servers-to-the-vpn-client"></a>Wie füge ich dem VPN-Client benutzerdefinierte DNS-Server hinzu?

Sie können die heruntergeladene XML-Datei für das Profil ändern und die Tags **\<dnsservers>\<dnsserver> \</dnsserver>\</dnsservers>** hinzufügen.

```
<azvpnprofile>
<clientconfig>

    <dnsservers>
        <dnsserver>x.x.x.x</dnsserver>
        <dnsserver>y.y.y.y</dnsserver>
    </dnsservers>
    
</clientconfig>
</azvpnprofile>
```

> [!NOTE]
> Der OpenVPN-Azure AD-Client verwendet DNS-Einträge der Richtlinientabelle für die Namensauflösung (Name Resolution Policy Table, NRPT). Dies bedeutet, dass in der Ausgabe von `ipconfig /all` keine DNS-Server aufgeführt werden. Verwenden Sie [Get-DnsClientNrptPolicy](/powershell/module/dnsclient/get-dnsclientnrptpolicy) in PowerShell, um die von Ihnen derzeit verwendeten DNS-Einstellungen zu überprüfen.
>

### <a name="can-i-configure-split-tunneling-for-the-vpn-client"></a><a name="split"></a>Kann ich für den VPN-Client getrenntes Tunneln konfigurieren?

Getrenntes Tunneln ist standardmäßig für den VPN-Client konfiguriert.

### <a name="how-do-i-direct-all-traffic-to-the-vpn-tunnel-forced-tunneling"></a><a name="forced-tunnel"></a>Wie leite ich den gesamten Netzwerkdatenverkehr an den VPN-Tunnel weiter (Tunnel erzwingen)?

Sie können „Tunnel erzwingen“ mit zwei verschiedenen Methoden konfigurieren: entweder durch Ankündigung benutzerdefinierter Routen oder durch Ändern der XML-Profildatei.    

> [!NOTE]
> Internetkonnektivität wird nicht über das VPN-Gateway bereitgestellt. Daher wird der für das Internet gebundene Datenverkehr verworfen.
>

* **Ankündigung benutzerdefinierter Routen:** Sie können benutzerdefinierte Routen 0.0.0.0/1 und 128.0.0.0/1 ankündigen. Weitere Informationen finden Sie unter [Ankündigung benutzerdefinierter Routen für P2S-VPN-Clients](vpn-gateway-p2s-advertise-custom-routes.md).

* **Profil-XML:** Sie können die heruntergeladene XML-Profildatei und die Tags **\<includeroutes>\<route>\<destination>\<mask> \</destination>\</mask>\</route>\</includeroutes>** hinzufügen.


    ```
    <azvpnprofile>
    <clientconfig>
          
        <includeroutes>
            <route>
                <destination>0.0.0.0</destination><mask>1</mask>
            </route>
            <route>
                <destination>128.0.0.0</destination><mask>1</mask>
            </route>
        </includeroutes>
           
    </clientconfig>
    </azvpnprofile>
    ```


### <a name="how-do-i-add-custom-routes-to-the-vpn-client"></a>Wie füge ich dem VPN-Client benutzerdefinierte Routen hinzu?

Sie können die heruntergeladene XML-Datei für das Profil ändern und die Tags **\<includeroutes>\<route>\<destination>\<mask> \</destination>\</mask>\</route>\</includeroutes>** hinzufügen.

```
<azvpnprofile>
<clientconfig>

    <includeroutes>
        <route>
            <destination>x.x.x.x</destination><mask>24</mask>
        </route>
    </includeroutes>
    
</clientconfig>
</azvpnprofile>
```

### <a name="how-do-i-block-exclude-routes-from-the-vpn-client"></a>Wie blockiere ich Routen vom VPN-Client (Ausschließen von Routen)?

Sie können die heruntergeladene XML-Datei für das Profil ändern und die Tags **\<excluderoutes>\<route>\<destination>\<mask> \</destination>\</mask>\</route>\</excluderoutes>** hinzufügen.

```
<azvpnprofile>
<clientconfig>

    <excluderoutes>
        <route>
            <destination>x.x.x.x</destination><mask>24</mask>
        </route>
    </excluderoutes>
    
</clientconfig>
</azvpnprofile>
```

### <a name="can-i-import-the-profile-from-a-command-line-prompt"></a>Kann ich das Profil über eine Eingabeaufforderung importieren?

Sie können das Profil über eine Eingabeaufforderung importieren, indem Sie die heruntergeladene Datei **azurevpnconfig.xml** im Ordner **%userprofile%\AppData\Local\Packages\Microsoft.AzureVpn_8wekyb3d8bbwe\LocalState** ablegen und den folgenden Befehl ausführen:

```
azurevpn -i azurevpnconfig.xml 
```
Um den Import zu erzwingen, verwenden Sie den Schalter **-f**.


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Erstellen eines Azure Active Directory-Mandanten für P2S Open VPN-Verbindungen, die die Azure AD-Authentifizierung verwenden](openvpn-azure-ad-tenant.md).