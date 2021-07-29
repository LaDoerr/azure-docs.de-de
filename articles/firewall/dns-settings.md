---
title: DNS-Einstellungen für Azure Firewall
description: Sie können Azure Firewall mit DNS-Server- und DNS-Proxyeinstellungen konfigurieren.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: how-to
ms.date: 05/26/2021
ms.author: victorh
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: e4543af78b173632e3374567e9a199f182679e8f
ms.sourcegitcommit: 20acb9ad4700559ca0d98c7c622770a0499dd7ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2021
ms.locfileid: "110701722"
---
# <a name="azure-firewall-dns-settings"></a>DNS-Einstellungen für Azure Firewall

Sie können einen benutzerdefinierten DNS-Server konfigurieren und den DNS-Proxy für Azure Firewall aktivieren. Konfigurieren Sie diese Einstellungen, wenn Sie die Firewall bereitstellen, oder konfigurieren Sie die Einstellungen über die Seite **DNS-Einstellungen** zu einem späteren Zeitpunkt. Standardmäßig verwendet Azure Firewall Azure DNS, und der DNS-Proxy ist deaktiviert.

## <a name="dns-servers"></a>DNS-Server

Ein DNS-Server verwaltet Domänennamen und löst diese in IP-Adressen auf. Azure Firewall verwendet standardmäßig Azure DNS für die Namensauflösung. Mit der Einstellung **DNS-Server** können Sie eigene DNS-Server für die Namensauflösung von Azure Firewall konfigurieren. Sie können einen einzelnen Server oder mehrere Server konfigurieren.

> [!NOTE]
> Bei über Azure Firewall Manager verwalteten Azure Firewall-Instanzen werden die DNS-Einstellungen in der zugehörigen Azure Firewall-Richtlinie konfiguriert.

### <a name="configure-custom-dns-servers---azure-portal"></a>Konfigurieren benutzerdefinierter DNS-Server: Azure-Portal

1. Wählen Sie unter Azure Firewall die Option **Einstellungen** und danach **DNS-Einstellungen** aus.
2. Unter **DNS-Server** können Sie vorhandene DNS-Server eingeben oder hinzufügen, die zuvor in Ihrem virtuellen Netzwerk angegeben wurden.
3. Wählen Sie **Übernehmen**.

Die Firewall leitet nun DNS-Datenverkehr zur Namensauflösung an die angegebenen DNS-Server weiter.

:::image type="content" source="media/dns-settings/dns-servers.png" alt-text="Screenshot, der Einstellungen für DNS-Server zeigt.":::

### <a name="configure-custom-dns-servers---azure-cli"></a>Konfigurieren benutzerdefinierter DNS-Server: Azure CLI

Das folgende Beispiel aktualisiert Azure Firewall über die Azure CLI mit benutzerdefinierten DNS-Servern.

```azurecli-interactive
az network firewall update \
    --name fwName \ 
    --resource-group fwRG \
    --dns-servers 10.1.0.4 10.1.0.5
```

> [!IMPORTANT]
> Der Befehl `az network firewall` erfordert die Installation der Azure CLI-Erweiterung `azure-firewall`. Sie können sie mithilfe des Befehls `az extension add --name azure-firewall` installieren. 

### <a name="configure-custom-dns-servers---azure-powershell"></a>Konfigurieren benutzerdefinierter DNS-Server: Azure PowerShell

Das folgende Beispiel aktualisiert Azure Firewall über Azure PowerShell mit benutzerdefinierten DNS-Servern.

```azurepowershell
$dnsServers = @("10.1.0.4", "10.1.0.5")
$azFw = Get-AzFirewall -Name "fwName" -ResourceGroupName "fwRG"
$azFw.DNSServer = $dnsServers

$azFw | Set-AzFirewall
```

## <a name="dns-proxy"></a>DNS-Proxy

Sie haben auch die Möglichkeit, Azure Firewall als DNS-Proxy zu konfigurieren. Ein DNS-Proxy ist ein Vermittler für DNS-Anforderungen von virtuellen Clientcomputern an einen DNS-Server.

Wenn Sie die FQDN-Filterung (Fully Qualified Domain Name, vollqualifizierter Domänenname) in Netzwerkregeln aktivieren möchten, aktivieren Sie den DNS-Proxy, und aktualisieren Sie die Konfiguration des virtuellen Computers, um die Firewall als DNS-Proxy zu verwenden.

:::image type="content" source="media/dns-settings/dns-proxy-2.png" alt-text="D N S-Proxykonfiguration mit einem benutzerdefinierten D N S-Server.":::

Wenn Sie die FQDN-Filterung in Netzwerkregeln aktivieren und virtuelle Clientcomputer nicht für die Verwendung der Firewall als DNS-Proxy konfigurieren, werden DNS-Anforderungen von diesen Clients möglicherweise zu einem anderen Zeitpunkt an einen DNS-Server gesendet, oder die DNS-Anforderungen geben eine andere Antwort als die Firewall zurück. Der DNS-Proxy fügt Azure Firewall in den Pfad der Clientanforderungen ein, um Inkonsistenzen zu vermeiden.


Wenn Azure Firewall ein DNS-Proxy ist, können zwei Arten von Zwischenspeicherungsfunktionen auftreten:

- **Positiver Cache**: Die DNS-Auflösung ist erfolgreich. Die Firewall verwendet den TTL-Wert (Time-To-Live) des Pakets oder Objekts. 

- **Negativer Cache**: Die DNS-Auflösung führt zu keiner Antwort oder keiner Auflösung. Die Firewall speichert diese Information eine Stunde lang zwischen.

Der DNS-Proxy speichert alle aufgelösten IP-Adressen von FQDNs in Netzwerkregeln. Als Best Practice sollten Sie FQDNs verwenden, die in eine einzige IP-Adresse aufgelöst werden.

### <a name="policy-inheritance"></a>Richtlinienvererbung

 DNS-Einstellungen einer Richtlinie, die auf eine eigenständige Firewall angewendet werden, setzen die DNS-Einstellungen der eigenständigen Firewall außer Kraft. Eine untergeordnete Richtlinie erbt alle DNS-Einstellungen der übergeordneten Richtlinie, kann jedoch die übergeordnete Richtlinie außer Kraft setzen.

Um beispielsweise FQDNs in der Netzwerkregel zu verwenden, sollte der DNS-Proxy aktiviert sein. Wenn für eine übergeordnete Richtlinie jedoch **kein** DNS-Proxy aktiviert ist, unterstützt die untergeordnete Richtlinie keine FQDNs in Netzwerkregeln, es sei denn, Sie setzen diese Einstellung lokal außer Kraft.

### <a name="dns-proxy-configuration"></a>Konfiguration des DNS-Proxys

Für die Konfiguration des DNS-Proxy sind drei Schritte erforderlich:
1. Aktivieren Sie den DNS-Proxy in den DNS-Einstellungen von Azure Firewall.
2. Konfigurieren Sie optional den benutzerdefinierten DNS-Server, oder verwenden Sie die bereitgestellten Standardwerte.
3. Konfigurieren Sie die private IP-Adresse von Azure Firewall als benutzerdefinierte DNS-Adresse in den DNS-Servereinstellungen Ihres virtuellen Netzwerks. Durch diese Einstellung wird sichergestellt, dass DNS-Datenverkehr an Azure Firewall weitergeleitet wird.

#### <a name="configure-dns-proxy---azure-portal"></a>Konfigurieren des DNS-Proxys: Azure-Portal

Zum Konfigurieren des DNS-Proxys müssen Sie die DNS-Servereinstellung des virtuellen Netzwerks so konfigurieren, dass die private IP-Adresse der Firewall verwendet wird. Aktivieren Sie dann den DNS-Proxy in den **DNS-Einstellungen** von Azure Firewall.

##### <a name="configure-virtual-network-dns-servers"></a>Konfigurieren der DNS-Server für das virtuelle Netzwerk 

1. Wählen Sie das virtuelle Netzwerk aus, an das der DNS-Datenverkehr über die Azure Firewall-Instanz weitergeleitet werden soll.
2. Wählen Sie unter **Einstellungen** die Option **DNS-Server** aus.
3. Wählen Sie unter **DNS-Server** die Option **Benutzerdefiniert** aus.
4. Geben Sie die private IP-Adresse der Firewall ein.
5. Wählen Sie **Speichern** aus.
6. Starten Sie die mit dem virtuellen Netzwerk verbundenen VMs, damit ihnen die neuen DNS-Servereinstellungen zugewiesen werden. VMs nutzen ihre aktuellen DNS-Einstellungen weiter, bis sie neu gestartet werden.

##### <a name="enable-dns-proxy"></a>Aktivieren des DNS-Proxys

1. Wählen Sie Ihre Azure Firewall-Instanz aus.
2. Klicken Sie unter **Einstellungen** auf die Option **DNS-Einstellungen**.
3. Der **DNS-Proxy** ist standardmäßig deaktiviert. Wenn diese Einstellung aktiviert ist, lauscht die Firewall an Port 53 und leitet DNS-Anforderungen an die konfigurierten DNS-Server weiter.
4. Überprüfen Sie die Konfiguration der **DNS-Server**, um sicherzustellen, dass die Einstellungen zu Ihrer Umgebung passen.
5. Wählen Sie **Speichern** aus.

:::image type="content" source="media/dns-settings/dns-proxy.png" alt-text="Screenshot, der Einstellungen für den DNS-Proxy zeigt.":::

#### <a name="configure-dns-proxy---azure-cli"></a>Konfigurieren des DNS-Proxys: Azure CLI

Sie können die Azure CLI verwenden, um DNS-Proxyeinstellungen in Azure Firewall zu konfigurieren. Sie können sie auch verwenden, um virtuelle Netzwerke für die Verwendung von Azure Firewall als DNS-Server zu aktualisieren.

##### <a name="configure-virtual-network-dns-servers"></a>Konfigurieren der DNS-Server für das virtuelle Netzwerk

Im folgenden Beispiel wird das virtuelle Netzwerk so konfiguriert, dass Azure Firewall als DNS-Server verwendet wird.
 
```azurecli-interactive
az network vnet update \
    --name VNetName \ 
    --resource-group VNetRG \
    --dns-servers <firewall-private-IP>
```

##### <a name="enable-dns-proxy"></a>Aktivieren des DNS-Proxys

Das folgende Beispiel aktiviert das DNS-Proxyfeature in Azure Firewall.

```azurecli-interactive
az network firewall update \
    --name fwName \ 
    --resource-group fwRG \
    --enable-dns-proxy true
```

#### <a name="configure-dns-proxy---azure-powershell"></a>Konfigurieren des DNS-Proxys: Azure PowerShell

Sie können Azure PowerShell verwenden, um DNS-Proxyeinstellungen in Azure Firewall zu konfigurieren. Sie können sie auch verwenden, um virtuelle Netzwerke für die Verwendung von Azure Firewall als DNS-Server zu aktualisieren.

##### <a name="configure-virtual-network-dns-servers"></a>Konfigurieren der DNS-Server für das virtuelle Netzwerk

Im folgenden Beispiel wird das virtuelle Netzwerk so konfiguriert, dass Azure Firewall als ein DNS-Server verwendet wird.

```azurepowershell
$dnsServers = @("<firewall-private-IP>")
$VNet = Get-AzVirtualNetwork -Name "VNetName" -ResourceGroupName "VNetRG"
$VNet.DhcpOptions.DnsServers = $dnsServers

$VNet | Set-AzVirtualNetwork
```

##### <a name="enable-dns-proxy"></a>Aktivieren des DNS-Proxys

Das folgende Beispiel aktiviert das DNS-Proxyfeature in Azure Firewall.

```azurepowershell
$azFw = Get-AzFirewall -Name "fwName" -ResourceGroupName "fwRG"
$azFw.DNSEnableProxy = $true

$azFw | Set-AzFirewall
```

## <a name="next-steps"></a>Nächste Schritte

- [Details zu Azure Firewall-DNS-Proxys](dns-details.md)
- [FQDN-Filterung in Netzwerkregeln](fqdn-filtering-network-rules.md)
