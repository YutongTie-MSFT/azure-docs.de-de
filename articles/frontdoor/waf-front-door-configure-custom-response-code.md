---
title: Konfigurieren einer benutzerdefinierten Antwort für Web Application Firewall in Azure Front Door
description: Erfahren Sie, wie Sie einen benutzerdefinierten Antwortcode und eine Meldung für den Fall konfigurieren, dass eine Anforderung durch Web Application Firewall (WAF) blockiert wird.
services: frontdoor
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/08/2019
ms.author: tyao;kumud
ms.openlocfilehash: 2d16893420f27caf4f8b00dc32069e3296d7c236
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "59795745"
---
# <a name="configure-a-custom-response-for-azure-web-application-firewall"></a>Konfigurieren einer benutzerdefinierten Antwort für Azure Web Application Firewall

Wenn Azure Web Application Firewall (WAF) mit Azure Front Door eine Anforderung aufgrund einer übereinstimmenden Regel blockiert, wird standardmäßig ein 403-Statuscode mit der Meldung **Die Anforderung wird blockiert** zurückgegeben. In diesem Artikel wird beschrieben, wie Sie einen benutzerdefinierten Antwortstatuscode und eine Antwortmeldung für den Fall konfigurieren, dass eine Anforderung durch WAF blockiert wird.

## <a name="set-up-your-powershell-environment"></a>Einrichten Ihrer PowerShell-Umgebung
Azure PowerShell bietet eine Reihe von Cmdlets, die das [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)-Modell für die Verwaltung von Azure-Ressourcen verwenden. 

Sie können [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) auf Ihrem lokalen Computer installieren und in einer beliebigen PowerShell-Sitzung nutzen. Befolgen Sie die Anweisungen auf der Seite, um sich mit Ihren Azure-Anmeldeinformationen anzumelden und das Azure PowerShell-Modul zu installieren.

### <a name="connect-to-azure-with-an-interactive-dialog-for-sign-in"></a>Herstellen einer Verbindung mit Azure über einen interaktiven Anmeldedialog
```
Connect-AzAccount
Install-Module -Name Az
```
Vergewissern Sie sich, dass die aktuelle Version von PowerShellGet installiert ist. Führen Sie den folgenden Befehl aus, und öffnen Sie PowerShell erneut.

```
Install-Module PowerShellGet -Force -AllowClobber
``` 
### <a name="install-azfrontdoor-module"></a>Installieren des Moduls „Az.FrontDoor“ 

```
Install-Module -Name Az.FrontDoor
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

In Azure ordnen Sie verwandte Ressourcen einer Ressourcengruppe zu. In diesem Beispiel erstellen Sie mithilfe von [New-AzResourceGroup](/powershell/module/Az.resources/new-Azresourcegroup) eine Ressourcengruppe.

```azurepowershell-interactive
New-AzResourceGroup -Name myResourceGroupWAF
```

## <a name="create-a-new-waf-policy-with-custom-response"></a>Erstellen einer neuen WAF-Richtlinie mit benutzerdefinierter Antwort 

Im folgenden Beispiel wird eine neue WAF-Richtlinie erstellt, in der der Statuscode auf 405 und die Meldung auf **You are blocked** (Sie wurden blockiert) festgelegt wird. Dies erfolgt unter Verwendung von [New-AzFrontDoorFireWallPolicy](/powershell/module/az.frontdoor/new-azfrontdoorfirewallPolicy).

```azurepowershell
# WAF policy setting
New-AzFrontDoorFireWallPolicy `
-Name myWAFPolicy `
-ResourceGroupName myResourceGroupWAF `
-EnabledState enabled `
-Mode Detection `
-CustomBlockResponseStatusCode 405 `
-CustomBlockResponseBody "<html><head><title>You are blocked.</title></head><body></body></html>"
```

Mithilfe von [Set-AzFrontDoor](/powershell/module/az.frontdoor/set-azfrontdoor) können Sie den benutzerdefinierten Antwortcode oder die Einstellungen des Antworttext einer vorhandenen WAF-Richtlinie ändern.

```azurepowershell
# modify WAF response code
Set-AzFrontDoorFireWallPolicy `
-Name myWAFPolicy `
-ResourceGroupName myResourceGroupWAF `
-EnabledState enabled `
-Mode Detection `
-CustomBlockResponseStatusCode 403
```

```azurepowershell
# modify WAF response body
Set-AzFrontDoorFireWallPolicy `
-Name myWAFPolicy `
-ResourceGroupName myResourceGroupWAF `
-CustomBlockResponseBody "<html><head><title> Forbidden</title></head><body></body></html>"
```

## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zu [Front Door](front-door-overview.md)