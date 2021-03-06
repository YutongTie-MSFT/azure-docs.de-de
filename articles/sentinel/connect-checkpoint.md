---
title: Erfassen von Check Point-Daten in Azure Sentinel Preview | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Check Point-Daten in Azure Sentinel erfassen.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 3229233d-400d-4971-8d76-eaa0d6591d75
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/6/2019
ms.author: rkarlin
ms.openlocfilehash: 1fb4f9165be03a7fc3cd055ef616dcfadb58ac9d
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2019
ms.locfileid: "58876495"
---
# <a name="connect-your-check-point-appliance"></a>Verbinden der Check Point-Appliance

> [!IMPORTANT]
> Azure Sentinel befindet sich momentan in der Public Preview.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Sie können Azure Sentinel mit jeder Check Point-Appliance verbinden, indem Sie die Protokolldateien im Format „Syslog CEF“ speichern. Die Integration mit Azure Sentinel ermöglicht Ihnen die einfache Ausführung von Analysen und Abfragen von Daten der Protokolldateien von Check Point. Weitere Informationen dazu, wie Azure Sentinel CEF-Daten erfasst, finden Sie unter [Connect CEF appliances (Verbinden von CEF-Appliances)](connect-common-event-format.md).

> [!NOTE]
> - Daten werden am geografischen Standort des Arbeitsbereichs gespeichert, in dem Sie Azure Sentinel ausführen.

## <a name="step-1-connect-your-check-point-appliance-using-an-agent"></a>Schritt 1: Verbinden der Check Point-Appliance mithilfe eines Agents

Sie müssen einen Agent auf einem dedizierten Computer (VM oder lokal) bereitstellen, um Ihre Check Point-Appliance mit Azure Sentinel zu verbinden und die Kommunikation zu ermöglichen. Sie können den Agent automatisch oder manuell bereitstellen. Die automatische Bereitstellung ist nur verfügbar, wenn es sich bei Ihrem dedizierten Computer um einen neuen virtuellen Computer handelt, den Sie in Azure erstellen. 

Alternativ hierzu können Sie den Agent manuell auf einem vorhandenen virtuellen Azure-Computer, einem virtuellen Computer in einer anderen Cloud oder einem lokalen Computer bereitstellen.

Netzwerkdiagramme zu beiden Optionen finden Sie unter [Herstellen einer Verbindung mit Datenquellen](connect-data-sources.md).

### <a name="deploy-the-agent-in-azure"></a>Bereitstellen des Agents in Azure

1. Klicken Sie im Azure Sentinel-Portal auf **Datensammlung**, und wählen Sie den Typ Ihrer Appliance aus. 

1. Gehen Sie unter **Linux Syslog agent configuration** (Linux-Syslog-Agent-Konfiguration) folgendermaßen vor:
   - Wählen Sie **Automatische Bereitstellung** (Automatic deployment) aus, wenn Sie einen neuen Computer erstellen möchten, auf dem der Azure Sentinel-Agent vorinstalliert ist und der die gesamte erforderliche Konfiguration (siehe Beschreibung oben) enthält. Wählen Sie die **automatische Bereitstellung** aus, und klicken Sie dann auf **Automatic agent deployment** (Automatische Bereitstellung des Agents). Daraufhin wird die Kaufseite für eine dedizierte VM geöffnet, die automatisch mit Ihrem Arbeitsbereich verbunden ist. Die VM gehört zur Serie **Standard D2s v3 (2 vCPUs, 8 GB Arbeitsspeicher)** und verfügt über eine öffentliche IP-Adresse.
     1. Geben Sie auf der Seite **Benutzerdefinierte Bereitstellung** Ihre Details an, legen Sie einen Benutzernamen und ein Kennwort fest, und erwerben Sie die VM, wenn Sie die Nutzungsbedingungen akzeptieren.
      
        1. Führen Sie die folgenden Befehle auf dem Syslog-Agent-Computer aus, um sicherzustellen, dass alle Check Point-Protokolle dem Azure Sentinel-Agent zugeordnet werden:
           - Wenn Sie „Syslog-ng“ verwenden, führen Sie die folgenden Befehle aus (beachten Sie, dass der Syslog-Agent neugestartet wird):
            
                sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };\n\nfilter f_msg_oms { match(\"Check Point\" value(\"MESSAGE\")); };\n  destination security_msg_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_msg_oms); destination(security_msg_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"

             Starten Sie den Syslog-Daemon neu: `sudo service syslog-ng restart`
           - Wenn Sie „rsyslog“ verwenden, führen Sie die folgenden Befehle aus (beachten Sie, dass der Syslog-Agent neugestartet wird):
                    
                 sudo bash -c "printf 'local4.debug  @127.0.0.1:25226\n\n:msg, contains, \"Check Point\"  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"
             Starten Sie den Syslog-Daemon neu: `sudo service rsyslog restart`

   - Wählen Sie **Manuelle Bereitstellung** (Manual deployment) aus, wenn Sie einen vorhandenen virtuellen Computer als dedizierten Linux-Computer verwenden möchten, auf dem der Azure Sentinel-Agent installiert werden soll. 
      1. Wählen Sie unter **Download and install the Syslog agent** (Syslog-Agent herunterladen und installieren) die Option **Azure Linux virtual machine** (Virtueller Azure-Linux-Computer) aus. 
      1. Wählen Sie in der **Virtuelle Computer**-Anzeige, die geöffnet wird, den Computer aus, den Sie verwenden möchten, und klicken Sie dann auf **Verbinden**.
      1. Legen Sie in der Connector-Anzeige unter **Configure and forward Syslog** (Syslog konfigurieren und weiterleiten) fest, ob Sie den Syslog-Daemon **rsyslog.d** oder **syslog-ng** verwenden. 
      1. Kopieren Sie die folgenden Befehle, und führen Sie sie auf der Appliance aus:
          - Wenn Sie „rsyslog.d“ ausgewählt haben:
              
            1. Weisen Sie den Syslog-Daemon an, die Einrichtung „local_4“ zu belauschen, „Check Point anzuwenden“ und die Syslog-Nachrichten an den Azure Sentinel-Agent über Port 25226 zu übermitteln. `sudo bash -c "printf 'local4.debug  @127.0.0.1:25226\n\n:msg, contains, \"Check Point\"  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"`
            
            2. Laden Sie die [Konfigurationsdatei „security_events“](https://aka.ms/asi-syslog-config-file-linux) herunter, mit der der Syslog-Agent zum Lauschen auf Port 25226 konfiguriert wird, und installieren Sie sie. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` Ersetzen Sie hier {0} durch die GUID Ihres Arbeitsbereichs.
           
            1. Starten Sie den Syslog-Daemon neu. `sudo service rsyslog restart`
             
          - Wenn Sie „syslog-ng“ ausgewählt haben:

              1. Weisen Sie den Syslog-Daemon an, die Einrichtung „local_4“ zu belauschen, „Check Point anzuwenden“ und die Syslog-Nachrichten an den Azure Sentinel-Agent über Port 25226 zu übermitteln. `sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };\n\nfilter f_msg_oms { match(\"Check Point\" value(\"MESSAGE\")); };\n  destination security_msg_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_msg_oms); destination(security_msg_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
              2. Laden Sie die [Konfigurationsdatei „security_events“](https://aka.ms/asi-syslog-config-file-linux) herunter, mit der der Syslog-Agent zum Lauschen auf Port 25226 konfiguriert wird, und installieren Sie sie. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` Ersetzen Sie hier {0} durch die GUID Ihres Arbeitsbereichs.

              3. Starten Sie den Syslog-Daemon neu. `sudo service syslog-ng restart`
      2. Starten Sie den Syslog-Agent mit dem folgenden Befehl neu:  `sudo /opt/microsoft/omsagent/bin/service_control restart [{workspace GUID}]`
      1. Vergewissern Sie sich, dass im Agent-Protokoll keine Fehler enthalten sind, indem Sie den folgenden Befehl ausführen: `tail /var/opt/microsoft/omsagent/log/omsagent.log`

### <a name="deploy-the-agent-on-an-on-premises-linux-server"></a>Bereitstellen des Agents auf einem lokalen Linux-Server

Wenn Sie nicht Azure verwenden, stellen Sie den Azure Sentinel-Agent für die Ausführung auf einem dedizierten Linux-Server manuell bereit.

1. Wählen Sie unter **Linux Syslog agent configuration** (Linux-Syslog-Agent-Konfiguration) die **manuelle Bereitstellung** aus, um eine dedizierte Linux-VM zu erstellen.
   1. Wählen Sie unter **Download and install the Syslog agent** (Syslog-Agent herunterladen und installieren) die Option **Non-Azure Linux machine** (Nicht von Azure stammender virtueller Computer) aus. 
   1. Wählen Sie in der **Direkt-Agent**-Anzeige, die geöffnet wird, die Option **Agent für Linux** aus, um den Agent herunterzuladen, oder führen Sie den folgenden Befehl aus, um den Agent auf Ihren Linux-Computer herunterzuladen: `wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w {workspace GUID} -s gehIk/GvZHJmqlgewMsIcth8H6VqXLM9YXEpu0BymnZEJb6mEjZzCHhZgCx5jrMB1pVjRCMhn+XTQgDTU3DVtQ== -d opinsights.azure.com`
      1. Legen Sie in der Connector-Anzeige unter **Configure and forward Syslog** (Syslog konfigurieren und weiterleiten) fest, ob Sie den Syslog-Daemon **rsyslog.d** oder **syslog-ng** verwenden. 
      1. Kopieren Sie die folgenden Befehle, und führen Sie sie auf der Appliance aus:
         - Wenn Sie **rsyslog** ausgewählt haben:
           1. Weisen Sie den Syslog-Daemon an, die Einrichtung „local_4“ zu belauschen, „Check Point anzuwenden“ und die Syslog-Nachrichten an den Azure Sentinel-Agent über Port 25226 zu übermitteln. `sudo bash -c "printf 'local4.debug  @127.0.0.1:25226\n\n:msg, contains, \"Check Point\"  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"`
            
           2. Laden Sie die [Konfigurationsdatei „security_events“](https://aka.ms/asi-syslog-config-file-linux) herunter, mit der der Syslog-Agent zum Lauschen auf Port 25226 konfiguriert wird, und installieren Sie sie. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` Ersetzen Sie hier {0} durch die GUID Ihres Arbeitsbereichs.
           3. Starten Sie den Syslog-Daemon neu. `sudo service rsyslog restart`
         - Wenn Sie **syslog-ng** ausgewählt haben:
            1. Weisen Sie den Syslog-Daemon an, die Einrichtung „local_4“ zu belauschen, „Check Point anzuwenden“ und die Syslog-Nachrichten an den Azure Sentinel-Agent über Port 25226 zu übermitteln. `sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };\n\nfilter f_msg_oms { match(\"Check Point\" value(\"MESSAGE\")); };\n  destination security_msg_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_msg_oms); destination(security_msg_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
            2. Laden Sie die [Konfigurationsdatei „security_events“](https://aka.ms/asi-syslog-config-file-linux) herunter, mit der der Syslog-Agent zum Lauschen auf Port 25226 konfiguriert wird, und installieren Sie sie. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` Ersetzen Sie hier {0} durch die GUID Ihres Arbeitsbereichs.
            3. Starten Sie den Syslog-Daemon neu. `sudo service syslog-ng restart`
      1. Starten Sie den Syslog-Agent mit dem folgenden Befehl neu:  `sudo /opt/microsoft/omsagent/bin/service_control restart [{workspace GUID}]`
      1. Vergewissern Sie sich, dass im Agent-Protokoll keine Fehler enthalten sind, indem Sie den folgenden Befehl ausführen: `tail /var/opt/microsoft/omsagent/log/omsagent.log`
 
## <a name="step-2-forward-check-point-logs-to-the-syslog-agent"></a>Schritt 2: Weiterleiten der Check Point-Protokolle an den Syslog-Agent

Konfigurieren Sie Ihre Check Point-Appliance, um Syslog-Nachrichten im CEF-Format über den Syslog-Agent an Ihren Azure-Arbeitsbereich weiterzuleiten.

1. Öffnen Sie die Seite [Check Point Log Export](https://aka.ms/asi-syslog-checkpoint-forwarding).
2. Scrollen Sie zum Abschnitt **Basic Deployment** (Grundlegende Bereitstellung), und führen Sie die Anweisungen zum Einrichten der Verbindung gemäß der folgenden Richtlinien durch:
   - Legen Sie den **Syslog-Port** auf **514** oder den Port fest, den Sie für den Agent festgelegt haben.
     - Ersetzen Sie den **Namen** und die **IP-Adresse des Zielservers** in der CLI durch den Namen und die IP-Adresse des Syslog-Agents.
     - Legen Sie das Format auf **CEF** fest.
3. Wenn Sie Version R77.30 oder R80.10 verwenden, scrollen Sie nach oben zum Abschnitt **Installation**, und führen Sie die Anweisungen aus, um das Protokollexportprogramm für Ihre Version zu installieren.
 
## <a name="step-3-validate-connectivity"></a>Schritt 3: Überprüfen der Konnektivität

Es kann bis zu 20 Minuten dauern, bis Ihre Protokolle in Log Analytics angezeigt werden. 

1. Stellen Sie sicher, dass für Ihre Protokolle der richtige Port im Syslog-Agent verwendet wird. Führen Sie den folgenden Befehl auf dem Syslog-Agent-Computer aus: `tcpdump -A -ni any  port 514 -vv` Durch diesen Befehl werden Ihnen die Protokolle angezeigt, die vom Gerät an den Syslog-Computer übermittelt werden. Stellen Sie sicher, dass die Protokolle über den richtigen Port und die richtige Einrichtung bei der Quellappliance eingehen.
2. Stellen Sie sicher, dass der Syslog-Daemon und der Agent kommunizieren. Führen Sie den folgenden Befehl auf dem Syslog-Agent-Computer aus: `tcpdump -A -ni any  port 25226 -vv` Durch diesen Befehl werden Ihnen die Protokolle angezeigt, die vom Gerät an den Syslog-Computer übermittelt werden. Stellen Sie sicher, dass die Protokolle auch vom Agent empfangen werden.
3. Wenn beide diese Befehle erfolgreiche Ergebnisse zurückgegeben haben, überprüfen Sie in Log Analytics, ob Ihre Protokolle eingehen. Alle Ereignisse, die von diesen Appliances übermittelt werden, werden in Log Analytics unformatiert unter dem Typ `CommonSecurityLog` angezeigt.

4. Stellen Sie sicher, dass Sie anschließend die folgenden Befehle ausführen:
  
   - Wenn Sie „Syslog-ng“ verwenden, führen Sie die folgenden Befehle aus (beachten Sie, dass der Syslog-Agent neugestartet wird):

         sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };\n\nfilter f_msg_oms { match(\"Check Point\" value(\"MESSAGE\")); };\n  destination security_msg_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_msg_oms); destination(security_msg_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"
        Starten Sie den Syslog-Daemon neu: `sudo service syslog-ng restart`

   - Wenn Sie „rsyslog“ verwenden, führen Sie die folgenden Befehle aus (beachten Sie, dass der Syslog-Agent neugestartet wird): 

         sudo bash -c "printf 'local4.debug @127.0.0.1:25226\n\n:msg, contains, "Check Point" @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"
     Starten Sie den Syslog-Daemon neu: `sudo service rsyslog restart`

1. Hier können Sie überprüfen, ob Fehler aufgetreten sind oder die Protokolle nicht eingehen: `tail /var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log`
4. Stellen Sie sicher, dass die Standardgröße der Syslog-Nachrichten auf 2048 Bytes (2 KB) beschränkt ist. Wenn die Protokolle zu lang sind, aktualisieren Sie die Konfigurationsdatei „security_events.conf“ mithilfe des folgenden Befehls: `message_length_limit 4096`



## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie gelernt, wie Sie Check Point-Appliances mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [ Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats.md).

