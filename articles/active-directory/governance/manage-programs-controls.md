---
title: Verwalten von Programmen und Steuerelementen für Zugriffsüberprüfungen – Azure Active Directory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie zusätzliche Programme für jede Governance-, Risikomanagement- und Konformitätsinitiative in Ihrer Organisation erstellen können, um Azure Active Directory-Zugriffsüberprüfungen als Steuerelemente zu erfassen und zu organisieren.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 06/21/2018
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 43c0f1c041bfed1b041a9926efd869d167c6f1e9
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2019
ms.locfileid: "58577262"
---
# <a name="manage-programs-and-controls-for-azure-ad-access-reviews"></a>Verwalten von Programmen und Steuerelementen für Azure AD-Zugriffsüberprüfungen

Azure Active Directory (Azure AD) enthält Zugriffsüberprüfungen von Gruppenmitgliedern und Anwendungszugriff. Diese Beispiele für Steuerelemente sichern den Überblick, wer auf Gruppenmitgliedschaften und Anwendungen Ihrer Organisation zugreifen kann. Diese Steuerelemente ermöglichen Organisationen das effiziente Verwalten ihrer Governance-, Risikomanagement- und Konformitätsanforderungen.

## <a name="create-and-manage-programs-and-their-controls"></a>Erstellen und Verwalten von Programmen und ihren Steuerelementen
Sie können die Nachverfolgung und das Erfassen von Zugriffsüberprüfungen für verschiedene Zwecke vereinfachen, indem Sie diese Aufgaben in Programmen organisieren. Jede Zugriffsüberprüfung kann mit einem Programm verknüpft werden. Wenn Sie dann Berichte für einen Auditor vorbereiten, können Sie sich auf die Zugriffsüberprüfungen im Bereich einer bestimmten Initiative konzentrieren.  Programme und Ergebnisse der Zugriffsüberprüfung werden Benutzern mit der Rolle „Globaler Administrator“, „Benutzeradministrator“, „Sicherheitsadministrator“ oder „Sicherheitsleseberechtigter“ angezeigt.

Um eine Liste der Programme anzuzeigen, navigieren Sie zur Seite [Zugriffsüberprüfungen](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/), und wählen Sie **Programme**.

**Standardprogramm** ist immer vorhanden. Globale Administratoren und Benutzeradministratoren können weitere Programme erstellen. Sie können z.B. ein Programm für jede Konformitätsinitiative oder für jedes Geschäftsziel verwenden.

Wenn ein Programm nicht mehr benötigt wird und keine Steuerelemente mit ihm verknüpft sind, können Sie es löschen.

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen einer Zugriffsüberprüfung von Gruppen oder Anwendungen](create-access-review.md)
- [Abrufen von Ergebnissen einer Gruppen oder Anwendungen betreffenden Zugriffsüberprüfung](retrieve-access-review.md)
