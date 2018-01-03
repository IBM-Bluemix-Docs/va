---

copyright:
  years: 2015, 2017

lastupdated: "2017-03-07"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}

# IBM MQ Bluemix-Container-Image
{: #ibm_mq}

Das Image 'ibm-mq' wird für {{site.data.keyword.containershort}} zur Verfügung gestellt. Es beinhaltet {{site.data.keyword.IBM}} MQ Advanced for Developers (Programm ohne Gewährleistung).  

{:shortdesc}


## Funktionsweise
{: #ibm_mq_how}

{{site.data.keyword.IBM_notm}} MQ Advanced for Developers enthält alle erforderlichen Elemente, die Sie benötigen, um mit der Entwicklung eigener Messaging-Lösungen und {{site.data.keyword.IBM_notm}} MQ-Anwendungen zu beginnen.  

Wenn Messaging-Lösungen und {{site.data.keyword.IBM_notm}} MQ-Anwendungen in {{site.data.keyword.Bluemix_notm}} entwickelt werden sollen, können Sie das Docker-Image 'ibm-mq' in {{site.data.keyword.containershort}} einrichten. Anschließend können Sie Warteschlangenmanager erstellen und ausführen und über die Webbenutzerschnittstelle oder ein Terminal so konfigurieren, dass sie für die Anforderungen Ihrer Messaging-Lösung oder Anwendung ausgelegt sind. 

**Hinweis:** Das Image 'ibm-mq' ist ausschließlich für Entwicklungszwecke und Komponententests vorgesehen. Sie können das Image darüber hinaus dazu nutzen, das Produkt zu erkunden, Ihre Kenntnisse anhand von Lernprogrammen zu vertiefen und den Nutzwert von {{site.data.keyword.IBM_notm}} MQ für Ihre Organisation auszuloten. 

## Enthaltene Elemente
{: #ibm_mq_what}

Das Image 'ibm-mq' enthält das Softwarepaket für {{site.data.keyword.IBM_notm}} MQ Advanced for Developers Version 9.0.x. Dabei handelt es sich um eine Produktversion mit vollem Funktionsumfang, die für Entwicklung und Komponententests eingesetzt werden kann. Sie können diese Version kostenlos herunterladen und im Rahmen der Lizenzbedingungen verwenden. 

{{site.data.keyword.IBM_notm}} MQ Advanced for Developers ist für Windows- (64 Bit) und Linux-Betriebssysteme (x86-64) verfügbar. Alle vorausgesetzten Produkte sind im Downloadpaket enthalten. Weitere Informationen hierzu finden Sie im Abschnitt zu [IBM MQ-Downloads ![Symbol für externen Link](../../../../icons/launch-glyph.svg "Symbol für externen Link")](http://www-03.ibm.com/software/products/lv/ibm-mq-advanced-for-developers){: new_window}. 

Weitere Informationen zu den in {{site.data.keyword.IBM_notm}} MQ Advanced for Developers enthaltenen Komponenten finden Sie im Abschnitt [IBM MQ ![Symbol für externen Link](../../../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/support/knowledgecenter/SSFKSJ_9.0.0/com.ibm.mq.helphome.v90.doc/WelcomePagev9r0.htm){: new_window}. 



## Nutzungseinschränkungen
{: #ibm_mq_usage}

Die folgende Tabelle enthält die Einschränkungen, die bei der kostenlosen Nutzung des Images 'ibm-mq' in {{site.data.keyword.Bluemix_notm}} zu beachten sind: 

| Umgebung   	| Kostenlose Nutzung - Einschränkungen |
|-------------|-------------------------|
| Entwicklung | Uneingeschränkte kostenlose Nutzung des Images 'ibm-mq' für das Erstellen einzelner Container zu Entwicklungszwecken |
| Test        | Uneingeschränkte kostenlose Nutzung des Images 'ibm-mq' für das Erstellen einzelner Container für Komponententests. Informieren Sie sich anhand der [Lizenzinformationen](getstarted.html#ibm_mq_license ) über die zulässigen Tests. |
| Produktion  | Die {{site.data.keyword.IBM_notm}} MQ Advanced for Developer-Lizenz gestattet keinen Einsatz des Images für Produktionszwecke. |

**Hinweis:** Die Preisgestaltung für das Image 'ibm-mq' ist unabhängig von der Preisgestaltung für die Container, die Sie in {{site.data.keyword.Bluemix_notm}} verwenden. 


## Lizenzinformationen
{: #ibm_mq_license}

Informationen zu Lizenzen: 

*	Die Dockerfiles und zugehörige Scripts werden im Rahmen der Lizenz '[Apache License 2.0 ![Symbol für externen Link](../../../../icons/launch-glyph.svg "Symbol für externen Link")](http://www.apache.org/licenses/LICENSE-2.0.html){: new_window}' lizenziert. 
* [Lizenzinformationen für {{site.data.keyword.IBM_notm}} MQ Advanced for Developers ![Symbol für externen Link](../../../../icons/launch-glyph.svg "Symbol für externen Link")](http://www-03.ibm.com/software/sla/sladb.nsf/displaylis/973FA648A5DE42948525806E004CC757?OpenDocument){: new_window} 

**Hinweis:** Die Lizenz schließt eine Weitergabe aus. Die Bedingungen für {{site.data.keyword.IBM_notm}} MQ im Image beschränken die Nutzung auf Entwicklungszwecke und Komponententests. 


## Einführung
{: #ibm_mq_get_started}

Gehen Sie wie im Folgenden beschrieben vor, um {{site.data.keyword.IBM_notm}} MQ in einem Container auszuführen und zu verwalten, der unter {{site.data.keyword.Bluemix_notm}} gestartet wurde: 

1. [Richten Sie einen Container ein, der auf dem Image 'ibm-mq' basiert. ](ibm_mq.html#ibm_mq_provision)

    **Hinweis:** Der Container ist an den aktiven Status des enthaltenen Warteschlangenmanagers 'Queue Manager' gebunden. Wird Queue Manager über die Konsole oder die Befehlszeile gestoppt, wird auch der Container kurz darauf beendet.  

2. Verwalten Sie {{site.data.keyword.IBM_notm}} MQ-Ressourcen, die in einem Container ausgeführt werden. Die Verwaltung kann in der grafischen Benutzerschnittstelle der IBM MQ-Webkonsole oder über die Eingabe von Befehlen an einem Terminal erfolgen.  

    1. Wählen Sie eine der folgenden Optionen für die Verwaltung von {{site.data.keyword.IBM_notm}} MQ-Ressourcen im Container aus: 
    
        *	Starten Sie die {{site.data.keyword.IBM_notm}} MQ-Webkonsole und verwalten Sie {{site.data.keyword.IBM_notm}} MQ in der grafischen Benutzerschnittstelle. Weitere Informationen hierzu finden Sie im Abschnitt [IBM MQ-Webkonsole starten](ibm_mq.html#ibm_mq_webui). 
        *	Rufen Sie an einem Terminal die Docker-Befehlszeilenschnittstelle auf. Weitere Informationen hierzu finden Sie im Abschnitt [Terminal für Docker-Befehlszeilenschnittstelle einrichten](ibm_mq.html#ibm_mq_docker_commands). 
        * Rufen Sie an einem Terminal die IBM Bluemix Container Service-Befehlszeilenschnittstelle auf. Weitere Informationen hierzu finden Sie im Abschnitt [Terminal für IBM Bluemix Container Service-Befehlszeilenschnittstelle einrichten](ibm_mq.html#ibm_mq_containers_cli). 
        
    2. Konfigurieren Sie den Warteschlangenmanager '{{site.data.keyword.IBM_notm}} MQ Queue Manager', der im Container ausgeführt wird.  

        * Verwenden Sie das Programm *runmqsc*, wenn Queue Manager über die Befehlszeile verwaltet werden soll. Führen Sie den Befehl `runmqsc QM-Name` aus. Dabei ist *QM-Name* der Name Ihres Warteschlangenmanagers. Weitere Informationen hierzu finden Sie im Abschnitt [Container über Befehlszeile verwalten](ibm_mq.html#ibm_mq_access). 
    
        * Verwenden Sie für eine grafisch orientierte Verwaltung Ihres Warteschlangenmanagers die Webbenutzerschnittstelle. 
        
    3. Erstellen Sie für Ihren Warteschlangenmanager Nachrichtenwarteschlangen wie im [IBM MQ Knowledge Center ![Symbol für externen Link](../../../../icons/launch-glyph.svg "Symbol für externen Link")](http://www.ibm.com/support/knowledgecenter/en/SSFKSJ_9.0.0/com.ibm.mq.helphome.v90.doc/WelcomePagev9r0.htm){: new_window} beschrieben und senden und empfangen Sie Nachrichten über die Warteschlangen. 
    
        Führen Sie beispielsweise in `runmqsc` den folgenden Befehl zum Erstellen einer neuen lokalen Warteschlange aus: `DEFINE QLOCAL('MY.Q')`. Dabei ist *MY.Q* der Name der lokalen Warteschlange für das Terminal. Sie können die Warteschlange in der Webkonsole anzeigen oder die Warteschlange und die zugehörigen Eigenschaften anzeigen, indem Sie den folgenden Befehl im Befehl `runmqsc` ausführen: `DISPLAY QLOCAL('MY.Q')`. 
    
3. [Optional] Überwachen Sie {{site.data.keyword.IBM_notm}} MQ-Protokolle im Container. Weitere Informationen hierzu finden Sie im Abschnitt [Container über Befehlszeile verwalten](ibm_mq.html#ibm_mq_access). 

Weitere Informationen zur Einführung zu IBM MQ finden Sie auf der [Begrüßungsseite zu IBM MQ Version 9.0.x ![Symbol für externen Link](../../../../icons/launch-glyph.svg "Symbol für externen Link")](http://www.ibm.com/support/knowledgecenter/en/SSFKSJ_9.0.0/com.ibm.mq.helphome.v90.doc/WelcomePagev9r0.htm){: new_window}. 

Weitere Informationen zum Image und zum lokalen Erstellen eines Images in Docker finden Sie auf der [GitHub-Seite für MQ Docker![Symbol für externen Link](../../../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/ibm-messaging/mq-docker){: new_window}. 
    
Aktuelle Blogs und Informationen zu IBM MQ finden Sie im [Developerworks-Blog zu IBM MQ![Symbol für externen Link](../../../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/developerworks/community/blogs/messaging){: new_window}. 


### Auf dem Image 'ibm-mq' basierenden Container einrichten
{: #ibm_mq_provision}

Richten Sie einen Docker-Container in {{site.data.keyword.Bluemix_notm}}, der auf dem von {{site.data.keyword.IBM_notm}} bereitgestellten Image 'ibm-mq' basiert, wie folgt ein: 

1.	Melden Sie sich bei {{site.data.keyword.Bluemix_notm}} an. Verwenden Sie Ihre {{site.data.keyword.Bluemix_notm}}-ID für die Anmeldung. 
2.	Wählen Sie im Katalog **Container** aus und anschließend das Image *ibm-mq*. 
3.	Wählen Sie **Einzeln** aus, um eine einzelne Containerinstanz zu erstellen, die für Entwicklungs- und Testzwecke verwendet werden kann. 
4.	Geben Sie den Namen, z. B. 'mq', für den Container ein. 
5.	Wählen Sie den Containerumfang aus. 
6.	Wählen Sie im Feld für die öffentliche IP-Adresse **Öffentliche IP anfordern und binden** aus. 
7.	Geben Sie im Feld 'Öffentliche Ports' die Ports **1414/tcp, 9443/tcp** an. 
8.	Erweitern Sie die Optionen für 'Erweitert' und klicken Sie auf **Neue Umgebungsvariable hinzufügen**. Geben Sie die folgenden Umgebungsvariablen ein: LICENSE und MQ_QMGR_NAME. 

    * Stimmen Sie den Lizenzbedingungen für IBM MQ Advanced for Developers zu, indem Sie für **LICENSE** den Wert **accept** festlegen.  
    
        Sie können die Lizenzbedingungen einsehen, bevor Sie ihnen zustimmen, indem Sie den Wert 'view' für 'License' festlegen. Die Lizenzbedingungen werden anschließend in den Containerprotokollen ausgegeben.  
    
        Wenn Englisch nicht Ihre Muttersprache ist, können Sie die Lizenzdatei in einer anderen Sprache aufrufen, indem Sie eine weitere Umgebungsvariable namens **LANG** angeben und einen der folgenden Werte verwenden: *'zh_TW*' für traditionelles Chinesisch, *'zh*' für vereinfachtes Chinesisch, *'cs*' für Tschechisch, *'en*' für Englisch, *'fr*' für Französisch, *'de*' für Deutsch, *'el*' für Griechisch, *'id*' für Indonesisch, *'it*' für Italienisch, *'ja*' für Japanisch, *'ko*' für Koreanisch, *'lt*' für Litauisch, *'pl*' für Polnisch, *'pt*' für Portugiesisch, *'ru*' für Russisch, *'sl*' für Slowenisch, *'es*' für Spanisch oder *'tr*' für Türkisch. 

    * Legen Sie für **MQ_QMGR_NAME** den Wert *MYMQ* fest, wobei *MYMQ* der von Ihnen gewählte Name für den Warteschlangenmanager ist. 
 
        **Hinweis:** Wenn Sie keinen Wert für die Umgebungsvariablen LICENSE und MQ_QMGR_NAME angeben, wird der Container zwar erstellt, er startet in diesem Fall jedoch nicht.  
    
    * [Optional] Standardmäßig wird die Webkonsole von {{site.data.keyword.IBM_notm}} MQ im Container gestartet. Dieses Standardverhalten können Sie ändern, indem Sie für die Umgebungsvariable **MQ_DISABLE_WEB_CONSOLE** den Wert *true* angeben. 
    
        **Hinweis:** Wenn die Webkonsole verwendet werden soll, dürfen Sie die Umgebungsvariable *MQ_DISABLE_WEB_CONSOLE* nicht definieren. 

    * [Optional] Legen Sie zum Anzeigen der MQ-Protokolle über die {{site.data.keyword.Bluemix_notm}}-Benutzerschnittstelle für die Umgebungsvariable **LOG_LOCATIONS** den Wert `/var/mqm/qmgr/QM-Name/errors/AMQERR01.LOG` fest. Dabei ist *QM-Name* der Wert, den Sie für die Umgebungsvariable *MQ_QMGR_NAME* festgelegt haben. 
    
    *	[Optional] Standardmäßig werden die Standardobjekte von {{site.data.keyword.IBM_notm}} MQ Developer erstellt. Dieses Standardverhalten können Sie ändern, indem Sie für die Umgebungsvariable **MQ_DEV** den Wert *false* angeben und die Standardinstallationswerte für IBM MQ durch die von Ihnen gewünschten Werte ersetzen. Weitere Informationen hierzu finden Sie im Abschnitt [Standardinstallationswerte ändern](ibm_mq.html#ibm_mq_dev_default). 
 
9. [Optional] Ordnen Sie im Bereich mit den Optionen für 'Erweitert' einen Datenträger zu. Verwenden Sie {{site.data.keyword.Bluemix_notm}}-Datenträger, wenn {{site.data.keyword.IBM_notm}} MQ-Daten und die MQ-Konfiguration persistent gespeichert werden sollen, um auch bei Containermigrationen erhalten zu bleiben. {{site.data.keyword.IBM_notm}} MQ-Daten, die nicht auf einem Datenträger gespeichert sind, gehen beim Beenden eines Containers verloren. 

    Das Image 'ibm-mq' kann mit {{site.data.keyword.Bluemix_notm}}-Datenträgern verwendet werden. Die Datenträger müssen jedoch an **/mnt/mqm** angehängt sein, um von {{site.data.keyword.IBM_notm}} MQ erkannt und genutzt werden zu können. 

    Standardmäßig gehen IBM MQ-Daten in Containern, die ohne einen Datenträger erstellt wurden, beim Beenden der jeweiligen Container verloren. Wenn Sie dies verhindern möchten, müssen Sie das Messaging für persistente Nachrichten und {{site.data.keyword.Bluemix_notm}}-Datenträger verwenden. 

    1. Klicken Sie auf **Datenträger erstellen**, geben Sie einen Datenträgernamen ein und wählen Sie eine Dateifreigabe aus. 
    2. Klicken Sie auf **Vorhandenen Datenträger zuweisen** und wählen Sie den von Ihnen erstellten Datenträger aus. Geben Sie im Feld für den Pfad **/mnt/mqm** ein. Stellen Sie sicher, dass das Kontrollkästchen **Schreibgeschützt** nicht aktiviert ist. 

    **Hinweis:** Wenn Sie einen Datenträger anhängen, der als IBM MQ-Datenverzeichnis verwendet werden soll, müssen Sie den Datenträger an das Verzeichnis */mnt/mqm* anhängen. Durch das Anhängen an das Verzeichnis */mnt/mqm* wird der Datenträger vom Script 'ibm-mq' erkannt. Vom Script wird das IBM MQ-Dateisystem anschließend für die Verwendung des Datenträgers konfiguriert. 
     
10.	Klicken Sie auf **Erstellen** und warten Sie dann, bis der Container aktiv ist. 

    Nach dem Erstellen des Containers wird das Container-Dashboard geöffnet. Überprüfen Sie, ob der Container aktiv ist (Status **Running**).  

    Überprüfen Sie bei Bedarf die Protokolle für Ihren Container, indem Sie auf **Überwachung und Protokolle** klicken und die Registerkarte **Protokollierung** auswählen. Die Protokolle werden angezeigt. Klicken Sie für eine umfassendere Analyse der Protokolle auf **Erweiterte Ansicht**. Dadurch werden die Protokolle in Kibana angezeigt. 

Sie haben einen Warteschlangenmanager erstellt und gestartet. Jetzt können Sie mit den folgenden Verbindungsdetails eine Verbindung zu einer MQ-Anwendung herstellen: 

|    Verbindungseigenschaft         |  Wert |
|-------|-------|
|  IP-Adresse                       | Öffentliche IP-Adresse des Containers |
|     Port        |    1414         |
| Kanal           | DEV.APP.SVRCONN |


### Container über Befehlszeile verwalten
{: #ibm_mq_access}

Wenn der Container, in dem {{site.data.keyword.IBM_notm}} MQ ausgeführt wird, in {{site.data.keyword.Bluemix_notm}} eingesetzt wird, können Sie auf Ihren Container und den Containerstatus zugreifen und die {{site.data.keyword.IBM_notm}} MQ-Installation prüfen. 

Führen Sie die folgenden Schritte aus, um Setup und Konfiguration von {{site.data.keyword.IBM_notm}} MQ im Container zu überprüfen: 

1.	Richten Sie die IBM Containers-CLI ein. Weitere Informationen hierzu finden Sie auf der [Bluemix-CLI-Seite](https://plugins.ng.bluemix.net/ui/home.html). Folgen Sie den Anweisungen für die Installation des aktuellen Cloud Foundry Container Service-Plug-ins. 

2.	Melden Sie sich an einem Terminal bei Ihrer {{site.data.keyword.Bluemix_notm}}-Organisation und dem Bereich an, in dem der Container mit Ihrer {{site.data.keyword.Bluemix_notm}}-ID ausgeführt wird. Führen Sie die folgenden Befehle aus: 

    1. `bx login`
    2. `bx ic init`
    
3.	Überprüfen Sie den Status des Containers. Führen Sie den folgenden Befehl aus: `bx ic ps`. 

4.	Ordnen Sie Ihrem Container eine Bash-Sitzung zu: 

    `bx ic exec -it Containername /bin/bash`
    
    Dabei ist 'Containername' der Name des Containers. 
    
5.	Führen Sie verschiedene Befehle aus, um Ihr MQ-Setup zu verwalten. Beispiel: 

| Befehl   | Aktion  |
|---------|---------|
| dspmqver | IBM MQ-Version und Buildinformationen prüfen |
| dspmq    | Status des Warteschlangenmanagers, der im Container ausgeführt wird, überprüfen | 
| runmqsc  | MQ-Ressourcen verwalten |

Wählen Sie zum Überwachen der MQ-Protokolle eine der folgenden Optionen aus: 

* Führen Sie den folgenden Befehl aus, wenn Ihr Terminal für die Ausführung von {{site.data.keyword.containershort}}-CLI-Befehlen konfiguriert ist: `bx ic exec Container-ID tail -f /var/mqm/qmgr/QM-Name/errors/AMQERR01.LOG`. Dabei ist *Container-ID* der Name des Containers und *QM-Name* der Name des Warteschlangenmanagers. 
    
* Führen Sie den folgenden Befehl aus, wenn Ihr Terminal für die Ausführung von Docker-CLI-Befehlen konfiguriert ist: `docker exec Container-ID tail -f /var/mqm/qmgr/QM-Name/errors/AMQERR01.LOG`. Dabei ist *Container-ID* der Name des Containers und *QM-Name* der Name des Warteschlangenmanagers. 
    
    

### IBM MQ-Webkonsole starten
{: #ibm_mq_webui}

Führen Sie die folgenden Schritte aus, um die {{site.data.keyword.IBM_notm}} MQ-Webkonsole zu starten: 

1.	Wählen Sie im {{site.data.keyword.Bluemix_notm}}-Dashboard Ihren Container aus und vergewissern Sie sich, dass der Container den Status *Aktiv* aufweist. 
2.	Stellen Sie sicher, dass bei dem Container der öffentliche Port 9443 konfiguriert ist. 
3.	Suchen Sie in den Containerdetails nach der öffentlichen IP oder fordern Sie eine IP an und binden Sie sie, falls das noch nicht geschehen ist. 
4.	Starten Sie in einem Web-Browser die folgende URL: `https://Öffentliche_IP_für_Docker-Container':9443/ibmmq/console/`. Dabei ist 'Öffentliche_IP_für_Docker-Container' die öffentliche IP Ihres Containers. 

    Es wird eine Sicherheitswarnung angezeigt, da das vom Web-Server verwendete TLS-Zertifikat beim Hosten der MQ-Konsole als selbst signiertes Zertifikat generiert wird. Klicken Sie auf **Ausnahme hinzufügen** und bestätigen Sie Ihre Auswahl. 

    **Hinweis:** Verwenden Sie in einer Produktionsumgebung Ihr eigenes Zertifikat, das von Ihrem Browser als sicher anerkannt wird. Dieses Zertifikat sollte von einer Zertifizierungsstelle stammen.  
    
    Der Browser öffnet die Webbenutzerschnittstelle. 
    
5. Melden Sie sich bei der {{site.data.keyword.IBM_notm}} MQ-Konsole mit den folgenden Berechtigungsnachweisen an: 

    1.	Benutzer: admin
    2.	Kennwort: passw0rd
    
Sie können IBM MQ nun verwalten. 

### Terminal für IBM Bluemix Container Service-CLI einrichten
{: #ibm_mq_containers_cli}

Verwenden Sie die {{site.data.keyword.containershort}}-CLI, um {{site.data.keyword.IBM_notm}} MQ-Verwaltungsbefehle direkt in einem Container einzugeben. 

Führen Sie die folgenden Schritte aus, um ein Terminal für bx ic-Befehle zu konfigurieren, mit denen IBM MQ verwaltet werden kann: 

1.	Richten Sie die {{site.data.keyword.containershort}}-CLI ein. Weitere Informationen hierzu finden Sie auf der [Bluemix-CLI-Seite](https://plugins.ng.bluemix.net/ui/home.html). Folgen Sie den Anweisungen für die Installation des aktuellen Cloud Foundry Container Service-Plug-ins. 

2.	Melden Sie sich an einem Terminal bei {{site.data.keyword.Bluemix_notm}} an. Führen Sie die folgenden Befehle aus: 

    1. `bx login`
    2. `bx ic init`
    
3.	Ordnen Sie Ihrem Container eine Bash-Sitzung zu: 

    `bx ic exec -it Containername /bin/bash`
    
    Dabei ist 'Containername' der Name des Containers. 
    


### Terminal für Docker-CLI einrichten
{: #ibm_mq_docker_commands}

Verwenden Sie die Docker-CLI, um {{site.data.keyword.IBM_notm}} MQ-Verwaltungsbefehle direkt in einem Container einzugeben. 

Führen Sie die folgenden Schritte aus, um ein Terminal für Docker-Befehle zu konfigurieren, mit denen IBM MQ verwaltet werden kann: 

1.	Richten Sie die {{site.data.keyword.containershort}}-CLI ein. Weitere Informationen hierzu finden Sie auf der [Bluemix-CLI-Seite](https://plugins.ng.bluemix.net/ui/home.html). Folgen Sie den Anweisungen für die Installation des aktuellen Cloud Foundry Container Service-Plug-ins. 

2.	Melden Sie sich von einem Terminal aus bei {{site.data.keyword.Bluemix_notm}} an. Führen Sie die folgenden Befehle aus: 

    1. `bx login`
    2. `bx ic init`
    3. Kopieren Sie die Werte der folgenden Umgebungsvariablen: DOCKER_HOST, DOCKER_CERT_PATH und DOCKER_TLS_VERIFY. 
    4. Überschreiben Sie die lokale Docker-Umgebung, indem Sie die folgenden Variablen so definieren, dass die Verbindung zu {{site.data.keyword.containershort}} hergestellt wird: 
        
        `export DOCKER_HOST=<your_host_value>`
        
        `export DOCKER_CERT_PATH=<your_cert_path_value>`
        
        `export DOCKER_TLS_VERIFY=1`

3.	Ordnen Sie Ihrem Container eine Bash-Sitzung zu: 

    `bx ic exec -it Containername /bin/bash`
    
    Dabei ist 'Containername' der Name des Containers. 
    
**Hinweis:** Bei dieser Option werden nur einige Docker-Befehle unterstützt. 




### Standardinstallationswerte ändern
{: #ibm_mq_dev_default}

Der Warteschlangenmanager wird zunächst mit {{site.data.keyword.IBM_notm}} MQ Developer-Standardobjekten erstellt. Diese Standardobjekte erleichtern den Einstieg in das Entwickeln von Anwendungen und ermöglichen Ihnen sogar das Ausprobieren von {{site.data.keyword.IBM_notm}} MQ.  

Der Warteschlangenmanager wird mit den folgenden Standardobjekten erstellt: 

* Der MQ-Administrator **admin** mit dem Kennwort *passw0rd* 
*	Der MQ-Anwendungsbenutzer **app** ohne Kennwort 
*	Der für eine Bindung an den TCP-Port 1414 konfigurierte Listener **DEV.LISTENER.TCP** 
*	Der für Verbindungen zu MQ-Clientanwendungen konfigurierte Kanal **DEV.APP.SVRCONN** 
*	Der für MQ-Verwaltungsverbindungen konfigurierte Kanal **DEV.ADMIN.SVRCONN** 
*	Die drei Warteschlangen **DEV.QUEUE.1**, **DEV.QUEUE.2** und **DEV.QUEUE.3**, in die Nachrichten gestellt werden können 
*	Die Warteschlange **DEV.DEAD.LETTER.QUEUE** für unzustellbare Nachrichten 
*	Das Basistopic **DEV.BASE.TOPIC** mit der Topic-Zeichenfolge */dev* 
*	Ein Kanalauthentifizierungsdatensatz zum Blockieren von Clientverbindungen, bei denen die Verbindung nicht über den Kanal **DEV.APP.SVRCONN** oder **DEV.ADMIN.SVRCONN** hergestellt wird 
*	Ein Kanalauthentifizierungsdatensatz zum Blockieren von Benutzern mit Administratorberechtigung, bei denen die Verbindung nicht über den Kanal **DEV.ADMIN.SVRCONN** hergestellt wird 
*	Ein Kanalauthentifizierungsdatensatz zum ausschließlichen Zulassen von Benutzern mit Administratorberechtigung, bei denen die Verbindung über den Kanal **DEV.ADMIN.SVRCONN** hergestellt wird 
*	Eine Verbindungsauthentifizierung, die so konfiguriert ist, dass Verwaltungsanwendungen beim Herstellen einer Verbindung gültige Berechtigungsnachweise übergeben und die jeweilige Benutzer-ID für Berechtigungsprüfungen annehmen müssen 

Mit diesen Standardobjekten können Sie eine Verbindung zu einer Clientanwendung herstellen, die Nachrichten in eine Warteschlange stellen und aus einer Warteschlange abrufen oder das Topic abonnieren und Nachrichten zum Topic veröffentlichen kann. 

Sie können die Developer-Standardobjekte bei Bedarf anpassen, indem Sie beim Erstellen Ihres Containers über das Image 'ibm-mq' die folgenden Umgebungsvariablen hinzufügen: 

| Umgebungsvariable | Zweck      | Standardwert |
|----------------------|---------|---------|
| MQ_ADMIN_PASSWORD | Definieren Sie diese Variable, um das Standardkennwort des erstellten MQ-Administrators in ein Kennwort Ihrer Wahl zu ändern. Das Kennwort muss mindestens 8 Zeichen enthalten. Der Benutzername 'admin' für den Benutzer mit Administratorberechtigung ist fest vorgegeben. | passw0rd |
| MQ_APP_PASSWORD   | Definieren Sie diese Variable, um das Kennwort für den Anwendungsbenutzer festzulegen. Der Benutzername 'app' für den Anwendungsbenutzer ist fest vorgegeben. Wenn Sie diese Umgebungsvariable festlegen, müssen Sie die Benutzer-ID und das Kennwort beim Herstellen einer Verbindung zu MQ-Clients angeben. <br> Das Kennwort muss mindestens 8 Zeichen enthalten. |  |
| MQ_TLS_KEYSTORE   | Geben Sie für diese Umgebungsvariable die Position eines PKCS#12-Keystores mit dem Zertifikat an, das von der MQ-Webkonsole und IBM MQ Queue Manager für TLS-Operationen verwendet werden soll. <br> Wenn diese Umgebungsvariable definiert ist, können die erstellten Entwicklungskanäle TLS mit der Verschlüsselungsspezifikation ‘TLS_RSA_WITH_AES_256_GCM_SHA384’ nutzen. <br> **Hinweis:** Die Keystore-Datei muss im {{site.data.keyword.Bluemix_notm}}-Container zur Verfügung gestellt werden. Hängen Sie dazu einen Datenträger mit dem Keystore beim Erstellen des Containers an. |  |
| MQ_TLS_PASSPHRASE | Definieren Sie diese Umgebungsvariable, um das Kennwort für den Keystore festzulegen, der für die Umgebungsvariable MQ_TLS_KEYSTORE angegeben ist. |  |
| MQ_DEV            | Definieren Sie diese Umgebungsvariable, um steuern zu können, ob die {{site.data.keyword.IBM_notm}} MQ Developer-Standardobjekte erstellt werden. | true |
| MQ_DISABLE_WEB_CONSOLE | Definieren Sie diese Umgebungsvariable, um die Konfiguration und das Starten der Webkonsole zu inaktivieren, wenn die CPU/RAM-Auslastung Ihres Containers nicht zu hoch sein soll. |  |



