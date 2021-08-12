---
title: Datei einfügen
description: Datei einfügen
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 08/07/2019
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: f80efbac256871af073354f23317c447d6a85f1e
ms.sourcegitcommit: f3b930eeacdaebe5a5f25471bc10014a36e52e5e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/16/2021
ms.locfileid: "112234897"
---
Sie können jetzt mithilfe des Daten-Explorers Daten zu Ihrem neuen Container hinzufügen.

1. Erweitern Sie im **Daten-Explorer** die Datenbank **Tasks** und dann den Container **Items**. Wählen Sie **Elemente** und dann **Neues Element** aus.

   ![Neue Dokumente im Daten-Explorer im Azure-Portal erstellen](./media/cosmos-db-create-sql-api-add-sample-data/azure-cosmosdb-data-explorer-new-document.png)
  
2. Fügen Sie nun dem Container ein Dokument mit folgender Struktur hinzu.

     ```json
     {
         "id": "1",
         "category": "personal",
         "name": "groceries",
         "description": "Pick up apples and strawberries.",
         "isComplete": false
     }
     ```

3. Nachdem Sie den JSON-Code auf der Registerkarte **Dokumente** hinzugefügt haben, klicken Sie auf **Speichern**.

    ![Einfügen von JSON-Daten und Wählen von „Speichern“ im Daten-Explorer im Azure-Portal](./media/cosmos-db-create-sql-api-add-sample-data/azure-cosmosdb-data-explorer-save-document.png)

4.  Erstellen und speichern Sie ein weiteres Dokument, in das Sie einen eindeutigen Wert für die Eigenschaft `id` einfügen, und ändern Sie die anderen Eigenschaften nach Bedarf. Ihre neuen Dokumente können jede gewünschte Struktur aufweisen, da Azure Cosmos DB kein Schema für Ihre Daten vorgibt.