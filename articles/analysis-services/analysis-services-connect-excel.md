---
title: Verbinden von Azure Analysis Services mit Excel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mithilfe von Excel eine Verbindung mit einem Azure Analysis Services-Server herstellen.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 5c46d4e4d23744cf07ccf7857a33990bf405a6a1
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/10/2019
ms.locfileid: "54189013"
---
# <a name="connect-with-excel"></a>Herstellen einer Verbindung mit Excel

Nachdem Sie einen Server erstellt und ein tabellarisches Modell dafür bereitgestellt haben, können Clients eine Verbindung herstellen und mit dem Durchsuchen von Daten beginnen. 

## <a name="before-you-begin"></a>Voraussetzungen

Das zur Anmeldung verwendete Konto muss einer Modelldatenbankrolle angehören, die mindestens über Leseberechtigungen verfügt. Weitere Informationen finden Sie unter [Authentifizierung und Benutzerberechtigungen](analysis-services-manage-users.md). 

## <a name="connect-in-excel"></a>Eine Verbindung in Excel herstellen

Das Herstellen einer Verbindung mit einem Server in Excel wird mithilfe der Funktion „Daten abrufen“ in Excel 2016 und höher unterstützt. Das Herstellen einer Verbindung mithilfe des Import Table Wizard (Assistent „Tabelle importieren“) in Power Pivot wird nicht unterstützt. 

1. Klicken Sie in Excel im Menüband **Daten** auf **Get External Data (Externe Daten abrufen)** > **Aus anderen Quellen** > **Aus Analysis Services**.

2. Geben Sie im Datenverbindungs-Assistenten in **Servername** den Servernamen samt Protokoll und URI ein. Beispiel: asazure://westcentralus.asazure.windows.net/advworks. Wählen Sie dann unter **Anmeldeinformationen** die Option **Benutzername und Kennwort verwenden** aus, und geben Sie den Benutzernamen der Organisation, z.B. nancy@adventureworks.com, und das Kennwort ein.

    > [!IMPORTANT]
    > Wenn Sie sich über ein entsprechendes Konto (Microsoft-Konto, Live ID, Yahoo, Gmail usw.) anmelden, oder wenn Sie sich mit mehrstufiger Authentifizierung anmelden müssen, belassen Sie das Kennwortfeld leer. Nachdem Sie auf „Weiter“ geklickt haben, werden Sie zur Eingabe des Kennworts aufgefordert. 

    ![Herstellen einer Verbindung über die Excel-Anmeldung](./media/analysis-services-connect-excel/aas-connect-excel-logon.png)

3. Wählen Sie unter **Datenbank und Tabelle wählen** die Datenbank und das Modell oder die Perspektive aus, und klicken Sie dann auf **Fertig stellen**.
   
    ![Herstellen einer Verbindung über die Excel-Modellauswahl](./media/analysis-services-connect-excel/aas-connect-excel-select.png)


## <a name="see-also"></a>Weitere Informationen

[Clientbibliotheken](analysis-services-data-providers.md)   
[Manage your server (Verwalten des Servers)](analysis-services-manage.md)     


