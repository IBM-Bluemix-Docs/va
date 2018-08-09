---

copyright:
  years: 2017, 2018
lastupdated: "2018-07-24"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Imagesicherheit mit Vulnerability Advisor verwalten
{: #va_index}

Vulnerability Advisor überprüft den Sicherheitsstatus von Container-Images, die von {{site.data.keyword.IBM}} oder anderen Anbietern bereitgestellt oder die zum Registrynamensbereich Ihrer Organisation hinzugefügt werden. Wenn in den einzelnen Clustern der Container-Scanner installiert ist, wird auch der Status aktiver Container überprüft.
{:shortdesc}

Wenn Sie ein Image zu einem Namensbereich hinzufügen, wird das Image automatisch von Vulnerability Advisor auf Sicherheitsprobleme und potenzielle Sicherheitslücken überprüft. Wenn Sicherheitsprobleme gefunden werden, werden Anweisungen zur Verfügung gestellt, um die gemeldete Sicherheitslücke zu beheben. 

Vulnerability Advisor bietet Sicherheitsmanagementfunktionalität für {{site.data.keyword.registrylong_notm}} durch die Generierung eines Sicherheitsstatusberichts, der empfohlene Fixes und bewährte Verfahren enthält. 

Werden Probleme gefunden, führt dies zu dem Urteil, dass eine Bereitstellung des betreffenden Images nicht empfehlenswert ist. Wenn Sie sich entscheiden, das Image bereitzustellen, weisen alle Container, die auf der Basis des betreffenden Images bereitgestellt werden, bekannte Probleme auf, die möglicherweise für Angriffe auf den Container genutzt werden oder den Container auf andere Weise beeinträchtigen. Vulnerability Advisor passt das Urteil anhand der von Ihnen angegebenen Ausnahmen an. Dieses Urteil kann bei der Umsetzung der Imagesicherheit dazu verwendet werden, die Bereitstellung nicht sicherer Images in {{site.data.keyword.containerlong_notm}} zu verhindern. 

Durch das Beheben der von Vulnerability Advisor gemeldeten Sicherheits- und Konfigurationsprobleme können Sie die {{site.data.keyword.cloud_notm}}-Infrastruktur schützen.


## Informationen zu Vulnerability Advisor
{: #about}

Vulnerability Advisor stellt Funktionen für den Schutz von Images bereit.

{:shortdesc}

 Die folgenden Funktionen sind verfügbar:

-   Scannen von Images auf Probleme
-   Scannen von aktiven Containern auf Probleme, falls der [Container-Scanner](#va_install_container_scanner) in jedem Cluster installiert ist
-   Bereitstellen eines Bewertungsberichts auf der Basis von Sicherheitsverfahren, die für {{site.data.keyword.containerlong_notm}} spezifisch sind
-   Bereitstellen von Empfehlungen zur Sicherung von Konfigurationsdateien für eine Untergruppe von Anwendungstypen
-   Bereitstellen von Anweisungen zum Korrigieren eines gemeldeten [Pakets mit potenziellen Sicherheitslücken](#packages) oder zum Beheben von [Konfigurationsproblemen](#app_configurations) auf der Basis des entsprechenden Berichts
-   Bereitstellen von Urteilen für die [Umsetzung der Imagesicherheit](../Registry/registry_security_enforce.html#security_enforce)
-   Anwenden von Ausnahmen für Berichte auf Konto-, Namensbereichs-, Repository- oder Tagebene, um anzugeben, wann entsprechend gekennzeichnete Probleme für einen bestimmten Anwendungsfall nicht zutreffen
-   Bereitstellen von Links zu zugehörigen Containern in der Ansicht **Tag** der grafischen Benutzerschnittstelle von {{site.data.keyword.registrylong_notm}}. Die aktiven Container, die das betreffende Image verwenden, können in einem Cluster, in dem der Container-Scanner installiert ist, aufgelistet werden.


Im Registry-Dashboard wird in der Spalte **Richtlinienstatus** der Status Ihrer Repositorys angezeigt. Im Bericht mit dem entsprechenden Link werden Maßnahmen empfohlen, mit denen die Cloudsicherheit für Ihre Images erhöht werden kann. 

Das Vulnerability Advisor-Dashboard enthält eine Übersicht und eine Beurteilung zu der Sicherheit eines Images. Wenn der Container-Scanner installiert ist, stehen auch Links zu aktiven Containern zur Verfügung. Weitere Informationen zu Vulnerability Advisor finden Sie im Abschnitt [Sicherheitslückenbericht für Ihr Image überprüfen](#va_reviewing).
	
	
**Datenschutz**

Für das Scannen von Images und Containern in Ihrem Konto zur Ermittlung potenzieller Sicherheitsprobleme werden von Vulnerability Advisor die folgenden Informationen erfasst, gespeichert und verarbeitet:
- Textfelder mit freiem Format, einschließlich IDs, Beschreibungen und Imagenamen (Registry, Namensbereich, Repositoryname und Image-Tag)
- Kubernetes-Metadaten, einschließlich Namen von Kubernetes-Ressourcen, wie z. B. Pod-, Replikatgruppen- und Bereitstellungsnamen
- Metadaten zu den Dateimodi und Erstellungszeitmarken der Konfigurationsdateien
- Inhalt der System- und Anwendungskonfigurationsdateien in Images und Containern
- Installierte Pakete und Bibliotheken (einschließlich der entsprechenden Versionsinformationen)

Geben Sie keine personenbezogenen Daten in Feldern oder an Positionen an, die von Vulnerability Advisor verarbeitet werden. Die betreffenden Felder und Positionen sind in der vorhergehenden Liste aufgeführt.

Scanergebnisse, die auf Rechenzentrumsebene aggregiert werden, werden so verarbeitet, dass anonymisierte Metriken generierte werden, die zur Ausführung und Verbesserung des Service dienen.

Scanergebnisse werden 30 Tage nach ihrer Generierung gelöscht.


## Typen von Sicherheitslücken
{: #types}


### Pakete mit potenziellen Sicherheitslücken
{: #packages}

Vulnerability Advisor überprüft Images, die auf unterstützten Betriebssystemen basieren, auf Pakete mit potenziellen Sicherheitslücken und stellt einen Link bereit, über den relevante Sicherheitshinweise zur jeweiligen Sicherheitslücke aufgerufen werden können.
{:shortdesc}

Pakete mit bekannten Problemen im Hinblick auf Sicherheitslücken werden in den Scanergebnissen angezeigt. Die Liste der potenziellen Sicherheitslücken wird täglich anhand veröffentlichter Sicherheitshinweise für die Docker-Imagetypen in der nachfolgenden Tabelle aktualisiert. In der Regel wird zum Bestehen der Sicherheitsprüfung bei einem potenziell gefährdeten Paket eine neue Version des Pakets benötigt, die eine Korrektur für die Sicherheitslücke enthält. Für ein Paket können mehrere Sicherheitslücken aufgelistet werden. In diesem Fall können durch ein einzelnes Upgrade für das Paket unter Umständen auch mehrere Probleme behoben werden.


  |Docker-Basisimage|Quelle der Sicherheitshinweise|
  |-----------------|--------------------------|
  |Alpine|[Git - Alpine Linux ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://git.alpinelinux.org/) und [CIRCL CVE Search ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.circl.lu/)|
  |CentOS| [CentOS-announce-Archive ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://lists.centos.org/pipermail/centos-announce/) und [CentOS CR-announce-Archive ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://lists.centos.org/pipermail/centos-cr-announce/)|
  |Debian|[Debian-Sicherheitshinweise ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://lists.debian.org/debian-security-announce/)|
  |Red Hat Enterprise Linux (RHEL)|[Korrekturliste für Red Hat-Produkte ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://access.redhat.com/errata/#/)|
  |Ubuntu|[Ubuntu-Sicherheitshinweise ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://usn.ubuntu.com/)|
  {: caption="Tabelle 1. Unterstützte Docker-Basisimages, die von Vulnerability Advisor auf Pakete mit potenziellen Sicherheitslücken überprüft werden" caption-side="top"}


### Konfigurationsprobleme
{: #app_configurations}

Bei Konfigurationsproblemen handelt es sich um potenzielle Sicherheitsprobleme im Zusammenhang mit der Konfiguration einer App. Viele der gemeldeten Probleme können durch die Aktualisierung der Dockerfile behoben werden.
{:shortdesc}

Images werden nur dann überprüft, wenn sie auf einem Betriebssystem basieren, das von Vulnerability Advisor unterstützt wird. Vulnerability Advisor überprüft die Konfigurationseinstellungen für die folgenden App-Typen:
-   MySQL
-   NGINX
-   Apache




## Container-Scanner installieren
{: #va_install_container_scanner}

Vorbereitungen:

1.  Melden Sie sich beim {{site.data.keyword.Bluemix_notm}}-CLI-Client an. Wenn Sie über ein eingebundenes Konto verfügen, verwenden Sie `--sso`.
2.  [Rufen Sie in der `kubectl`-Befehlszeilenschnittstelle](../../containers/cs_cli_install.html#cs_cli_configure) den Cluster auf, für den Sie ein Helm-Diagramm verwenden möchten.
3.  Erstellen Sie eine Service-ID und einen API-Schlüssel für den Container-Scanner und ordnen Sie einen Namen zu:
    1.  Erstellen Sie eine Service-ID mithilfe des folgenden Befehls. Ersetzen Sie dabei `<scanner_serviceID>` durch den gewünschten Namen für die Service-ID. Beachten Sie den entsprechenden **CRN**.
    
        ```
    	ibmcloud iam service-id-create <scanner_serviceID>
    	```
        {: codeblock}

    2.  Erstellen Sie einen API-Schlüssel für den Service. Dabei ist `<scanner_serviceID>` die Service-ID, die Sie im vorherigen Schritt erstellt haben. Ersetzen Sie `<scanner_APIkey_name>` durch den gewünschten Namen für den Scanner-API-Schlüssel. 
    
        ```
    	ibmcloud iam service-api-key-create <scanner_APIkey_name> <scanner_serviceID>
    	```
        {: codeblock}
	
	    Der API-Schlüssel des Scanners wird zurückgegeben.
	
	    Stellen Sie sicher, dass der Scanner-API-Schlüssel sicher gespeichert wird, da er zu einem späteren Zeitpunkt nicht erneut abgerufen werden kann.
	    {: tip}
	
    3.  Erstellen Sie eine Servicerichtlinie, mit der die Rolle eines `Schreibberechtigten` erteilt wird.
    		
        ```
    	ibmcloud iam service-policy-create <scanner_serviceID> --resource-type scaningress --service-name container-registry --roles Writer
    	```
        {: codeblock}

Gehen Sie wie folgt vor, um das Helm-Diagramm zu konfigurieren:

1.  [Richten Sie Helm im Cluster ein](../../containers/cs_integrations.html#helm). Wenn Sie eine RBAC-Richtlinie zum Erteilen des Helm-Tiller-Zugriffs verwenden, müssen Sie sicherstellen, dass die Tiller-Rolle über Zugriff auf alle Namensbereiche verfügt, damit der Scanner Container in allen Namensbereich überwachen kann.

2.  Fügen Sie das IBM Diagrammrepository zu Helm hinzu, z. B. `ibm-incubator`.

    ```
    helm repo add ibm-incubator https://registry.bluemix.net/helm/ibm-incubator
    ```
    {: pre}

3.  Speichern Sie die Standardkonfigurationseinstellungen für das Helm-Diagramm des Container-Scanners in einer lokalen YAML-Datei. Beziehen Sie das Diagrammrepository, z. B. `ibm-incubator`, in den Helm-Diagrammpfad ein.

    ```
    helm inspect values ibm-incubator/ibmcloud-container-scanner > config.yaml
    ```
    {: pre}

4.  Bearbeiten Sie die Datei `config.yaml`.

    ```yaml
    EmitURL: <regional_emit_URL>
    AccountID: <IBM_Cloud_account_ID>
    ClusterID: <cluster_ID>
    APIKey: <scanner_APIkey>
    ...
    ```
    {: pre}

    <table>
    <col width="22%">
    <col width="78%">
    <caption>Erläuterung der YAML-Dateikomponenten</caption>
    <thead>
    <th>Feld</th>
    <th>Wert</th>
    </thead>
    <tbody>
    <tr>
    <td><code>EmitURL</code></td>
    <td>Geben Sie die regionale Endpunkt-URL für Vulnerability Advisor ein. Führen Sie zum Abrufen der URL <code>ibmcloud cr info</code> aus und rufen Sie die Adresse der <strong>Container-Registry</strong> ab. Ersetzen Sie <code>registry</code> durch <code>va</code>. Beispiel: <code>https<span comment="make the link not a link">://va.</span>eu-gb.bluemix.net</code></td>
    </tr>
    <tr>
    <td><code>AccountID</code></td>
    <td>Diese Angabe durch die ID des {{site.data.keyword.Bluemix_notm}}-Kontos ersetzen, in dem sich der Cluster befindet. Zum Abrufen der Konto-ID führen Sie <code>ibmcloud account list</code> aus.</td>
    </tr>
    <tr>
    <td><code>ClusterID</code></td>
    <td>Diese Angabe durch den Kubernetes-Cluster ersetzen, in dem der Container-Scanner installiert werden soll. Zum Abrufen einer Liste mit Cluster-IDs führen Sie <code>ibmcloud ks clusters</code> aus.<br> **Tipp**: Verwenden Sie die ID des Clusters, nicht den Namen.
    </td>
    </tr>
    <tr>
    <td><code>APIKey</code></td>
    <td>Diese Angabe durch den API-Schlüssel des Scanners ersetzen, der in einem der vorhergehenden Schritte erstellt wurde.</td>
    </tr>
    </tbody></table>

5.  Installieren Sie das Helm-Diagramm mit der aktualisierten Datei `config.yaml` im Cluster. Die aktualisierten Eigenschaften werden in einer Konfigurationsübersicht für das Diagramm gespeichert. Ersetzen Sie `<myscanner>` durch den gewünschten Namen für das Helm-Diagramm. Beziehen Sie das Diagrammrepository, z. B. `ibm-incubator`, in den Helm-Diagrammpfad ein.

    ```
    helm install -f config.yaml --name=<myscanner> ibm-incubator/ibmcloud-container-scanner
    ```
    {: pre}
    
    Der Container-Scanner wird im Namensbereich `kube-system` installiert, scannt jedoch Container aller Namensbereiche.
    {:tip}

6.  Überprüfen Sie den Bereitstellungsstatus des Diagramms. Wenn das Diagramm bereit ist, wird im Feld **Status** am Anfang der Ausgabe der Wert `Bereitgestellt` angezeigt.

    ```
    helm status <myscanner>
    ```
    {: pre}

7.  Vergewissern Sie sich nach der Bereitstellung des Diagramms, dass die aktualisierten Einstellungen in `config.yaml` verwendet wurden.

    ```
    helm get values <myscanner>
    ```
    {: pre}


IBM Container Scanner ist nun installiert und der Agent ist als [Dämongruppe ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) im Cluster bereitgestellt. Der Scanner wird zwar im Namensbereich `kube-system` bereitgestellt, scannt jedoch alle Container, die Pods in allen Ihren Kubernetes-Namensbereichen zugewiesen sind, wie z. B. `default`. 


## Container-Scanner hinter einer Firewall ausführen
{: #va_firewall}

Wenn Ihre Firewall abgehende Verbindungen blockiert, müssen Sie sie so konfigurieren, dass Workerknoten auf den Container-Scanner am TCP-Port <code>443</code> über die in der folgenden Tabelle aufgeführten IP-Adressen zugreifen kann.
{:shortdesc}

 

<p>
  <table summary=" Die Zeilen sind von links nach rechts zu lesen, wobei die Serverzone in Spalte 1 und die zugehörigen IP-Adressen in Spalte 2 angegeben sind.">
  <caption>IP-Adressen, die für abgehenden Datenverkehr geöffnet werden müssen</caption>
      <thead>
      <th>Region</th>
      <th>IP-Adresse</th>
      </thead>
    <tbody>
      <tr>
         <td>Asien/Pazifik (Süden)</td>
         <td><code>168.1.40.158</code><br><code>130.198.65.182</code></td>
      </tr>
      <tr>
         <td>EU (Mitte)</td>
         <td><code>159.8.220.182</code><br><code>158.177.74.102</code></td>
        </tr>
      <tr>
        <td>Vereinigtes Königreich (Süden)</td>
        <td><code>158.175.71.134</code><br><code>5.10.111.190</code></td>
      </tr>
      <tr>
        <td>USA (Osten)</td>
         <td><code>169.60.73.158</code><br><code>169.61.84.102</code></td>
      </tr>
      <tr>
        <td>USA (Süden)</td>
        <td><code>169.47.103.118</code><br><code>169.48.165.6</code></td>
      </tr>
      </tbody>
    </table>
</p>


## Sicherheitslückenbericht über die grafische Benutzerschnittstelle überprüfen
{: #va_reviewing}

Vor dem Bereitstellen eines Images können Sie sich anhand des zugehörigen Vulnerability Advisor-Berichts über Details zu Paketen mit potenziellen Sicherheitslücken und zu nicht sicheren App-Einstellungen informieren.
{:shortdesc}



1.  Melden Sie sich bei {{site.data.keyword.Bluemix_notm}} an.
2.  Klicken Sie auf **Katalog**.
3.  Klicken Sie unter **Infrastruktur** auf **Container**.
4.  Klicken Sie auf die Kachel **Container-Registry**.
5.  Erweitern Sie den Eintrag **Vulnerability Advisor** und klicken Sie auf **Gescannte Repositories**.
6.  Zeigen Sie den Bericht für das aktuelle Image (mit dem Tag `latest` markiert) an, indem Sie auf die Zeile für das betreffende Repository klicken. Im Bericht wird die Gesamtzahl der Probleme angezeigt und angegeben, ob gefährdete Pakete oder Konfigurationsprobleme vorliegen. Enthält das Repository kein Image mit dem Tag `latest`, wird das neueste Image verwendet.
7.  Zum Anzeigen von Informationen zu den einzelnen Paketen mit potenziellen Sicherheitslücken für das ausgewählte Image klicken Sie in der Tabelle **Gefundene Pakete mit potenziellen Sicherheitslücken** auf den Link in der Spalte **Sicherheitslücken**, um den Bericht zu öffnen.
    1.  Zeigen Sie bei Bedarf weitere Informationen an, indem Sie die Zusammenfassung erweitern.
    2.  Wenn ein Hinweis des Betriebssystemdistributors vorhanden ist, klicken Sie auf den Link in der Spalte **Offizieller Hinweis**.
8.  Zeigen Sie Informationen zu den einzelnen Konfigurationsproblemen in der Tabelle **Gefundene Konfigurationsprobleme** an, indem Sie jeweils auf die entsprechenden Zeilen klicken.
9.  Führen Sie die im Bericht angegebenen Maßnahmen zum Beheben der einzelnen Probleme aus und erstellen Sie das Image neu. Einige Probleme in der Dockerfile können mit dem im Abschnitt [Probleme in Images beheben](#va_report) angegebenen Code behoben werden.

Wenn Sicherheitslücken vorhanden sind und diese nicht behoben werden, können diese Probleme die Sicherheit von Containern beeinträchtigen, die auf der Basis dieses Images erstellt werden. Es ist jedoch möglich, ein Image mit Sicherheits- und Konfigurationsproblemen weiterhin in einem Container zu verwenden.

 


## Sicherheitslückenbericht über die Befehlszeilenschnittstelle überprüfen
{: #va_registry_cli}

Sie können die Sicherheit von Docker-Images, die in Ihren Namensbereichen in {{site.data.keyword.registrylong_notm}} gespeichert sind, über die Befehlszeilenschnittstelle überprüfen.
{:shortdesc}

1.  Rufen Sie eine Liste der Images in Ihrem {{site.data.keyword.Bluemix_notm}}-Konto auf. Eine Liste aller Images wird zurückgegeben, unabhängig vom Namensbereich, in dem sie gespeichert sind.

    ```
    ibmcloud cr image-list
    ```
    {: pre}

2.  Überprüfen Sie den Status in der Spalte **Sicherheitsstatus**.
    -   `Keine Probleme`: Es wurden keine Sicherheitsprobleme gefunden.
    -   `X Probleme`: Es wurden potenzielle Sicherheitsprobleme oder Sicherheitslücken gefunden.
    -   `Scan wird durchgeführt`: Das Image wird zurzeit gescannt und der abschließende Sicherheitslückenstatus liegt noch nicht vor.
4.  Über Details zum Status können Sie sich im Vulnerability Advisor-Bericht informieren.

    ```
    ibmcloud cr va registry.<region>/<my_namespace>/<my_image>:<tag>
    ```
    {: pre}

    In der Ausgabe der Befehlszeilenschnittstelle können Sie die folgenden Informationen zu den Konfigurationsproblemen anzeigen.
      - Sicherheitsverfahren: Beschreibung der gefundenen Sicherheitslücke.
      - Fehlerbehebungsmaßnahme: Details zur Vorgehensweise zur Behebung der Sicherheitslücke.


## Containerbericht überprüfen
{: #va_reviewing_container}

Im Dashboard können Sie den Status eines Containers anzeigen, um festzustellen, ob seine Sicherheit den Richtlinien Ihrer Organisation entspricht. Darüber hinaus können Sie den Sicherheitsbericht eines Containers überprüfen, in dem alle gefährdeten Pakete und nicht sicheren Container- oder Anwendungseinstellungen aufgeführt sind und der angibt, ob der Container den Richtlinien Ihrer Organisation entspricht.
{:shortdesc}

Vergewissern Sie sich, dass der Container so sicher wie möglich ist, indem Sie den zugehörigen Sicherheitsbericht anzeigen und für alle gemeldeten Sicherheits- oder Konfigurationsprobleme die entsprechenden Maßnahmen durchführen. Führen Sie hierzu die folgenden Schritte aus:

1.  Wählen Sie den Container aus, für den ein Bericht angezeigt werden soll:
    1.  Wählen Sie im Katalog **Container** aus und klicken Sie auf **Container-Registry**.
    2.  Wählen Sie die Registerkarte **Private Repositorys** und anschließend die Zeile für das gewünschte Repository aus.
    3.  Wählen Sie die Zeile für das gewünschte Image aus.
    4.  Wählen Sie die Registerkarte **Zugehörige Container** und anschließend die Zeile für den gewünschten Container aus. Der Sicherheitsbericht wird geöffnet.
2.  Überprüfen Sie die jeweiligen Abschnitte, um die potenziellen Sicherheits- und Konfigurationsprobleme für die einzelnen Pakete im Image anzuzeigen:

      -   **Sicherheitslücken**: Listet Pakete mit bekannten Problemen aufgrund von Sicherheitslücken auf. Diese werden täglich auf der Basis veröffentlichter Sicherheitshinweise für die Docker-Imagetypen aktualisiert, die in [Imagesicherheit mit Vulnerability Advisor verwalten](va_index.html) aufgeführt sind. In der Regel wird zum Bestehen der Sicherheitsprüfung bei einem potenziell gefährdeten Paket eine neue Version des Pakets benötigt, die eine Korrektur für die Sicherheitslücke enthält. Für ein Paket können mehrere Sicherheitslücken aufgelistet werden. In diesem Fall können durch ein einzelnes Upgrade für das Paket gegebenenfalls auch mehrere Probleme behoben werden. Klicken Sie auf den Code des Sicherheitshinweises, um weitere Informationen zum Paket und Schritte zur Aktualisierung des Pakets anzuzeigen.

    -   **Konfigurationsprobleme**: Listet Vorschläge auf, mit denen Sie die Sicherheit des Containers und aller nicht sicheren Anwendungseinstellungen des Containers erhöhen können. Erweitern Sie die Zeile, um den Lösungsvorschlag für das jeweilige Problem anzuzeigen.

   Problembehebungsmaßnahmen oder -vorschläge werden zu jedem aufgeführten Punkt angegeben.
   
3.  Überprüfen Sie den Richtlinienstatus für die einzelnen Sicherheitsprobleme. Der Richtlinienstatus gibt an, ob für dieses Problem eine Ausnahme gilt.

    -  **Aktiv**: Es liegt ein Problem vor, für das keine Ausnahme gilt und das sich auf den Sicherheitsstatus auswirkt.
    -  **Ausgenommen**: Für dieses Problem ist in den Richtlinieneinstellungen eine Ausnahme festgelegt.
    -  **Teilweise ausgenommen**: Dieses Problem ist mehreren Sicherheitshinweisen zugeordnet. Nicht für alle Sicherheitshinweise gilt eine Ausnahme.

4.  Entscheiden Sie, wie der Container aktualisiert werden soll, damit die Probleme behoben werden können.

    **Wichtig:** Zur Behebung von Problemen im Container-Image müssen Sie die alte Instanz löschen und eine erneute Bereitstellung durchführen, wodurch alle Daten im vorhandenen Container verloren gehen. Stellen Sie sicher, dass Sie mit der Containerarchitektur vertraut sind, damit Sie die geeignete Methode zur erneuten Bereitstellung des Containers auswählen können.

    Beispiel:

    -   Wenn der Container von den Daten, die er berechnet, entkoppelt ist, können Sie den Container stoppen und löschen, die erforderlichen Änderungen am Image vornehmen und eine erneute Bereitstellung durchführen, ohne dass Daten verloren gehen.
    -   Sie können einen {{site.data.keyword.Bluemix_notm}}-Service, wie z. B. [Delivery Pipeline](../ContinuousDelivery/pipeline_about.html), zur Unterstützung bei der Aktualisierung der gefährdeten Containerinstanz verwenden.
    -   In einer Mikroservicearchitektur können Sie den Datenverkehr an eine andere Containerinstanz weiterleiten, während Sie Sicherheits- oder Konfigurationsprobleme beheben, und das neue Image in einer Red/Black-Bereitstellung mithilfe einer Push-Operation übertragen.

5.  Falls eine Behebung des Problems zum gegenwärtigen Zeitpunkt nicht möglich ist, können Sie in den Richtlinieneinstellungen eine Ausnahme für das Problem festlegen; auf diese Weise kann das Problem die Bereitstellung des Containers nicht blockieren. Wenn Sie eine Ausnahme für das Problem festlegen möchten, klicken Sie auf das Symbol **Optionsliste öffnen und schließen** und anschließend auf **Ausnahme erstellen**.

6.  Beheben Sie die im **Sicherheitsbericht** beschriebenen Probleme und erstellen Sie das Image erneut bzw. stellen Sie den Container erneut bereit - je nach ausgewählter Methode. Einige Probleme in der Dockerfile können mit dem im Abschnitt [Probleme in Images beheben](/docs/services/va/va_index.html#va_report) angegebenen Code behoben werden.


## Häufig auftretende Probleme in Images beheben
{: #va_report}

Lesen Sie die Beispielkorrekturen für häufig auftretende Probleme, die vom Vulnerability Advisor gemeldet werden können. Einige Probleme können durch eine Aktualisierung der Dockerfile behoben werden.
{:shortdesc}


### Maximale Gültigkeitsdauer des Kennworts, Kennwortmindestgültigkeit in Tagen und minimale Kennwortlänge
{: #va_password}

**Problem**: Eine oder mehrere der folgenden Sicherheitslücken werden gemeldet:

```
Die maximale Gültigkeitsdauer des Kennworts muss auf 90 Tage begrenzt sein.
```
{: screen}

```
Die Mindestkennwortlänge muss mit 8 angegeben sein.
```
{: screen}

```
Die Mindestanzahl der Tage, die zwischen Kennwortänderungen durch den Benutzer vergehen müssen, beträgt 1.
```
{: screen}

**Korrektur**: Stellen Sie sicher, dass die Regeln für Kennwörter eingehalten werden, indem Sie den folgenden Code zu Ihrer Dockerfile hinzufügen.

```
RUN \
    sed -i 's/^PASS_MAX_DAYS.*/PASS_MAX_DAYS    90/' /etc/login.defs && \
    sed -i 's/^PASS_MIN_DAYS.*/PASS_MIN_DAYS    1/' /etc/login.defs && \
    sed -i 's/sha512/sha512 minlen=8/' /etc/pam.d/common-password
```
{: codeblock}


### SSH-Sicherheitslücke
{: #ssh}

**Problem**: Die folgende Sicherheitslücke wird zurückgegeben:

```
Der SSH-Server darf nicht installiert sein.
```
{: screen}

**Korrektur**: Verwenden Sie für den Zugriff auf den Container den Befehl `docker attach` oder `docker exec` anstelle von SSH. Stellen Sie sicher, dass Ihre Dockerfile keine Schritte für die Installation eines SSH-Servers enthält.
