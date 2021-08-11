---
title: Erstellen eines Clientzertifikats mit Microsoft Azure Confidential Ledger
description: Erstellen eines Clientzertifikats mit Microsoft Azure Confidential Ledger
services: confidential-ledger
author: msmbaldwin
ms.service: confidential-ledger
ms.topic: overview
ms.date: 04/15/2021
ms.author: mbaldwin
ms.openlocfilehash: 125f56a6c96e37394e4e9e2565093c847e592108
ms.sourcegitcommit: b5508e1b38758472cecdd876a2118aedf8089fec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2021
ms.locfileid: "113585931"
---
# <a name="creating-a-client-certificate"></a>Erstellen eines Clientzertifikats

Die Confidential Ledger-APIs erfordern eine clientzertifikatbasierte Authentifizierung. Nur die Zertifikate, die während der Ledgererstellung oder dem Ledgerupdate einer Positivliste hinzugefügt wurden, können verwendet werden, um die funktionalen Confidential Ledger-APIs aufzurufen.

Sie benötigen ein Zertifikat im PEM-Format. Sie können mehr als ein Zertifikat erstellen und mithilfe der Ledgerupdate-API hinzufügen oder löschen.

## <a name="openssl"></a>OpenSSL

Es wird empfohlen, OpenSSL zum Generieren von Zertifikaten zu verwenden. Wenn Sie Git installiert haben, können Sie OpenSSL in der Git-Shell ausführen. Andernfalls können Sie OpenSSL für Ihr Betriebssystem installieren.

- **Windows**: Installieren Sie [Chocolatey für Windows](https://chocolatey.org/install), öffnen Sie ein PowerShell-Terminalfenster im Administratormodus, und führen Sie `choco install openssl` aus. Alternativ können Sie OpenSSL für Windows über [diesen Link](http://gnuwin32.sourceforge.net/packages/openssl.htm) installieren.
- **Linux**: Führen Sie `sudo apt-get install openssl` aus.

Sie können anschließend ein Zertifikat generieren, indem Sie `openssl` in einem Bash- oder PowerShell-Terminalfenster ausführen.

```bash
openssl ecparam -out "privkey_name.pem" -name "secp384r1" -genkey
openssl req -new -key "privkey_name.pem" -x509 -nodes -days 365 -out "cert.pem" -"sha384" -subj=/CN="ACL Client Cert"
```

## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über Microsoft Azure Confidential Ledger](overview.md)