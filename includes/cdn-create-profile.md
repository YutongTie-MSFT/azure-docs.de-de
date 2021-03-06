---
title: Includedatei
description: Includedatei
services: cdn
author: SyntaxC4
ms.service: cdn
ms.topic: include
ms.date: 05/24/2018
ms.author: cfowler
ms.custom: include file
ms.openlocfilehash: e431e7c45dc2cb41db5b9fa762abc908cc76b375
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34665289"
---
## <a name="create-a-new-cdn-profile"></a>Erstellen eines neuen CDN-Profils

Ein CDN-Profil ist ein Container für CDN-Endpunkte und gibt einen Tarif an.

1. Klicken Sie links oben im Azure-Portal auf **Ressource erstellen**. 
    
    Der Bereich **Neu** wird angezeigt.
   
2. Klicken Sie auf **Web + Mobil** und anschließend auf **CDN**.
   
    ![Klicken auf die Ressource „CDN“](./media/cdn-create-profile/cdn-new-resource.png)

    Der Bereich **CDN-Profil** wird angezeigt.

3. Verwenden Sie für die CDN-Profileinstellungen die Werte aus der folgenden Tabelle:
   
    | Einstellung  | Wert |
    | -------- | ----- |
    | **Name** | Geben Sie *my-cdn-profile-123* als Profilname ein. Dieser Name muss global eindeutig sein. Sollte er bereits verwendet werden, können Sie einen anderen Namen eingeben. |
    | **Abonnement** | Wählen Sie in der Dropdownliste ein Azure-Abonnement aus. |
    | **Ressourcengruppe** | Klicken Sie auf **Neu erstellen**, und geben Sie *my-resource-group-123* als Ressourcengruppennamen ein. Dieser Name muss global eindeutig sein. Sollte er bereits verwendet werden, können Sie die Option **Vorhandenes Element verwenden** und anschließend **my-resource-group-123** in der Dropdownliste auswählen. | 
    | **Ressourcengruppenstandort** | Wählen Sie in der Dropdownliste die Option **USA, Mitte** aus. |
    | **Preisstufe** | Wählen Sie in der Dropdownliste die Option **Verizon Standard** aus. |
    | **Jetzt neuen CDN-Endpunkt erstellen** | Lassen Sie das Kontrollkästchen deaktiviert. |  
   
    ![Neues CDN-Profil](./media/cdn-create-profile/cdn-new-profile.png)

4. Aktivieren Sie das Kontrollkästchen **An Dashboard anheften**, um das erstellte Profil in Ihrem Dashboard zu speichern.
    
5. Wählen Sie **Erstellen**, um das Profil zu erstellen. 

    Bei Profilen vom Typ **Azure CDN Standard von Microsoft** ist die Profilerstellung in der Regel in zwei Stunden abgeschlossen. 

