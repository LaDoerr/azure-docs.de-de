---
title: Behandeln von Aktivierungsfehlern bei Azure Stack Edge Pro mit GPU im Azure-Portal | Microsoft-Dokumentation
description: Hier wird beschrieben, wie Sie Probleme bei der Aktivierung von Azure Stack Edge Pro mit GPU und Schlüsseltresorprobleme behandeln können.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: troubleshooting
ms.date: 06/09/2021
ms.author: alkohli
ms.openlocfilehash: abb941fc84a0d4473ce03969e8c5718e8aeb41ce
ms.sourcegitcommit: e39ad7e8db27c97c8fb0d6afa322d4d135fd2066
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111982356"
---
# <a name="troubleshoot-activation-issues-on-your-azure-stack-edge-pro-gpu-device"></a>Behandeln von Aktivierungsproblemen auf Ihrem „Azure Stack Edge Pro mit GPU“-Gerät 

[!INCLUDE [applies-to-GPU-and-pro-r-and-mini-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-mini-r-sku.md)]

In diesem Artikel wird beschrieben, wie Sie Aktivierungsprobleme auf Ihrem „Azure Stack Edge Pro mit GPU“-Gerät behandeln können. 


## <a name="activation-errors"></a>Aktivierungsfehler

In der folgenden Tabelle werden die Fehler im Zusammenhang mit der Geräteaktivierung und die jeweils empfohlene Lösung zusammengefasst.

| Fehlermeldung| Empfohlene Lösung |
|------------------------------------------------------|--------------------------------------|
| Wenn die zur Aktivierung verwendete Azure Key Vault-Instanz gelöscht wird, bevor das Gerät mit dem Aktivierungsschlüssel aktiviert wird, wird diese Fehlermeldung angezeigt. <br> ![Schlüsseltresorfehler 1](./media/azure-stack-edge-gpu-troubleshoot-activation/key-vault-error-1.png)  | Wenn der Schlüsseltresor gelöscht wurde, können Sie ihn wiederherstellen, wenn sich der Tresor in der Löschschutzphase befindet. Führen Sie die Schritte in [Wiederherstellen eines Schlüsseltresors](../key-vault/general/key-vault-recovery.md#list-recover-or-purge-soft-deleted-secrets-keys-and-certificates) aus. <br>Wenn die Löschschutzphase abgelaufen ist, kann der Schlüsseltresor nicht mehr wiederhergestellt werden. Erkundigen Sie sich beim Support von Microsoft nach den weiteren Schritten. |
| Wenn die Azure Key Vault-Instanz gelöscht wird, nachdem das Gerät aktiviert wurde, und Sie dann versuchen, Vorgänge auszuführen, die eine Verschlüsselung umfassen , z. B. **Benutzer hinzufügen,** **Freigabe hinzufügen** oder **Compute konfigurieren**, erhalten Sie diesen Fehler. <br> ![Schlüsseltresorfehler 2](./media/azure-stack-edge-gpu-troubleshoot-activation/key-vault-error-2.png)    | Wenn der Schlüsseltresor gelöscht wurde, können Sie ihn wiederherstellen, wenn sich der Tresor in der Löschschutzphase befindet. Führen Sie die Schritte in „Wiederherstellen eines Schlüsseltresors“ aus. <br>Wenn die Löschschutzphase abgelaufen ist, kann der Schlüsseltresor nicht mehr wiederhergestellt werden. Erkundigen Sie sich beim Support von Microsoft nach den weiteren Schritten. |
| Wenn der Kanalintegritätsschlüssel in der Azure Key Vault-Instanz gelöscht wurde, und Sie versuchen, Vorgänge auszuführen, die eine Verschlüsselung umfassen, z. B. **Benutzer hinzufügen,** **Freigabe hinzufügen** oder **Compute konfigurieren**, erhalten Sie diesen Fehler. <br> ![Schlüsseltresorfehler 3](./media/azure-stack-edge-gpu-troubleshoot-activation/key-vault-error-3.png) | Wenn der Kanalintegritätsschlüssel im Schlüsseltresor gelöscht wurde, aber noch innerhalb der Löschschutzphase liegt, führen Sie die Schritte unter [Schlüsseltresor-Schlüsselentfernung rückgängig machen](/powershell/module/az.keyvault/undo-azkeyvaultkeyremoval) aus. <br>Wenn die Löschschutzphase abgelaufen ist und Sie den Schlüssel gesichert haben, können Sie den Schlüssel aus der Sicherung wiederherstellen. Andernfalls können Sie den Schlüssel nicht wiederherstellen. Erkundigen Sie sich beim Support von Microsoft nach den weiteren Schritten. |
| Wenn bei der Generierung des Aktivierungsschlüssels ein Fehler auftritt, wird dieser Fehler gemeldet. Die Benachrichtigung enthält weitere Details. <br> ![Schlüsseltresorfehler 4](./media/azure-stack-edge-gpu-troubleshoot-activation/key-vault-error-4.png)   | Stellen Sie sicher, dass die in [Zugreifen auf Azure Key Vault hinter einer Firewall](../key-vault/general/access-behind-firewall.md) angegebenen Ports und URLs in Ihrer Firewall geöffnet sind, damit der Zugriff auf den Schlüsseltresor möglich ist. Warten Sie ein paar Minuten, bevor Sie den Vorgang wiederholen. Wenden Sie sich an den Microsoft Support, wenn das Problem weiterhin besteht. |
| Wenn der Benutzer über Nur-Lese-Berechtigungen verfügt, ist er nicht berechtigt, einen Aktivierungsschlüssel zu generieren, und dieser Fehler wird gemeldet. <br> ![Schlüsseltresorfehler 5](./media/azure-stack-edge-gpu-troubleshoot-activation/key-vault-error-5.png) | Dies liegt möglicherweise daran, dass Sie nicht über die richtigen Zugriffsrechte verfügen oder *Microsoft.KeyVault* nicht registriert ist.<li>Stellen Sie sicher, dass Sie auf der Ressourcengruppenebene, die für Ihre Azure Stack Edge-Ressource verwendet wird, über Besitzer- oder Mitwirkendenzugriff verfügen.</li><li>Überprüfen Sie, ob der Ressourcenanbieter Microsoft.KeyVault registriert ist. Um einen Ressourcenanbieter zu registrieren, navigieren Sie zu dem Abonnement, das für die Azure Stack Edge-Ressource verwendet wird. Wechseln Sie zu **Ressourcenanbieter**, suchen Sie nach *Microsoft.KeyVault*, und wählen Sie **Registrieren** aus.</li> |

## <a name="next-steps"></a>Nächste Schritte

- [Installieren von Azure Stack Edge Pro mit GPU](azure-stack-edge-gpu-deploy-install.md)
