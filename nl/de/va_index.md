---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-01"

keywords: IBM Cloud Kubernetes Service, IBM Cloud Container Registry, security status of container images, image security, Vulnerability Advisor, security, registry, vulnerabilities, container scanner, containers, security issues, configuration issues,

subcollection: va

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}

# Imagesicherheit mit Vulnerability Advisor verwalten
{: #va_index}

Vulnerability Advisor überprüft den Sicherheitsstatus von Container-Images, die von {{site.data.keyword.IBM}} oder anderen Anbietern bereitgestellt oder die zum Registrynamensbereich Ihrer Organisation hinzugefügt werden. Wenn Container Scanner in jedem Cluster installiert ist, überprüft Vulnerability Advisor auch den Status von aktiven Containern.
{:shortdesc}

Wenn Sie ein Image zu einem Namensbereich hinzufügen, wird das Image automatisch von Vulnerability Advisor auf Sicherheitsprobleme und potenzielle Sicherheitslücken überprüft. Wenn Sicherheitsprobleme gefunden werden, werden Anweisungen zur Verfügung gestellt, um die gemeldete Sicherheitslücke zu beheben.

Vulnerability Advisor bietet Sicherheitsmanagementfunktionalität für [{{site.data.keyword.registrylong_notm}}](/docs/services/Registry?topic=registry-getting-started#getting-started) durch die Generierung eines Sicherheitsstatusberichts, der empfohlene Fixes und bewährte Verfahren enthält.

Wenn Vulnerability Advisor Probleme findet, führt dies zu dem Urteil, dass eine Bereitstellung des betreffenden Images nicht empfehlenswert ist. Wenn Sie sich entscheiden, das Image bereitzustellen, weisen alle Container, die auf der Basis des betreffenden Images bereitgestellt werden, bekannte Probleme auf, die möglicherweise für Angriffe auf den Container genutzt werden oder den Container auf andere Weise beeinträchtigen. Das Urteil wird entsprechend den von Ihnen angegebenen Ausnahmen angepasst. Dieses Urteil kann von Container Image Security Enforcement dazu verwendet werden, die Bereitstellung nicht sicherer Images in {{site.data.keyword.containerlong_notm}} zu verhindern.

Durch das Beheben der von Vulnerability Advisor gemeldeten Sicherheits- und Konfigurationsprobleme können Sie die {{site.data.keyword.cloud_notm}}-Infrastruktur schützen.

## Informationen zu Vulnerability Advisor
{: #about}

Vulnerability Advisor stellt Funktionen für den Schutz von Images bereit.
{:shortdesc}

Die folgenden Funktionen sind verfügbar:

- Scannen von Images auf Probleme
- Scans, die ausgeführt werden und Probleme in Containern ermitteln, wenn [Container Scanner](#va_install_container_scanner) in jedem Cluster installiert ist.
- Bereitstellen eines Bewertungsberichts auf der Basis von Sicherheitsverfahren, die für {{site.data.keyword.containerlong_notm}} spezifisch sind
- Bereitstellen von Empfehlungen zur Sicherung von Konfigurationsdateien für eine Untergruppe von Anwendungstypen
- Bereitstellen von Anweisungen zum Korrigieren eines gemeldeten [Pakets mit potenziellen Sicherheitslücken](#packages) oder zum Beheben von [Konfigurationsproblemen](#app_configurations) auf der Basis des entsprechenden Berichts
- Bereitstellen von Urteilen für [Container Image Security Enforcement](/docs/services/Registry?topic=registry-security_enforce#security_enforce)
- Anwenden von Ausnahmen für Berichte auf Konto-, Namensbereichs-, Repository- oder Tagebene, um anzugeben, wann entsprechend gekennzeichnete Probleme für einen bestimmten Anwendungsfall nicht zutreffen
- Bereitstellen von Links zu zugehörigen Containern in der Ansicht **Tag** der grafischen Benutzerschnittstelle von {{site.data.keyword.registrylong_notm}}. Die aktiven Container, die das betreffende Image verwenden, können in einem Cluster, in dem Container Scanner installiert ist, aufgelistet werden.

Im Registry-Dashboard wird in der Spalte **Richtlinienstatus** der Status Ihrer Repositorys angezeigt. Im Bericht mit dem entsprechenden Link werden Maßnahmen empfohlen, mit denen die Cloudsicherheit für Ihre Images erhöht werden kann.

Das Vulnerability Advisor-Dashboard enthält eine Übersicht und eine Beurteilung zu der Sicherheit eines Images. Wenn Container Scanner installiert ist, stehen auch Links zu aktiven Containern zur Verfügung. Weitere Informationen zu Vulnerability Advisor finden Sie im Abschnitt [Sicherheitslückenbericht für Ihr Image überprüfen](#va_reviewing).

**Datenschutz**

Für das Scannen von Images und Containern in Ihrem Konto zur Ermittlung potenzieller Sicherheitsprobleme werden von Vulnerability Advisor die folgenden Informationen erfasst, gespeichert und verarbeitet:

- Felder mit freiem Format, einschließlich IDs, Beschreibungen und Imagenamen (Registry, Namensbereich, Repositoryname und Image-Tag)
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

Vulnerability Advisor überprüft Images, die unterstützte Betriebssysteme verwenden, auf Pakete mit potenziellen Sicherheitslücken und stellt einen Link bereit, über den relevante Sicherheitshinweise zur jeweiligen Sicherheitslücke aufgerufen werden können.
{:shortdesc}

Pakete, die bekannten Probleme im Hinblick auf Sicherheitslücken aufweisen, werden in den Scanergebnissen angezeigt. Die Liste der potenziellen Sicherheitslücken wird täglich anhand der veröffentlichten Sicherheitshinweise für die Docker-Imagetypen in der nachfolgenden Tabelle aktualisiert. In der Regel wird zum Bestehen der Sicherheitsprüfung bei einem potenziell gefährdeten Paket eine neue Version des Pakets benötigt, die eine Korrektur für die Sicherheitslücke enthält. Für ein Paket können mehrere Sicherheitslücken aufgelistet werden. In diesem Fall können durch ein einzelnes Upgrade für das Paket unter Umständen auch mehrere Probleme behoben werden.

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

Images werden nur dann überprüft, wenn sie ein Betriebssystem verwenden, das von Vulnerability Advisor unterstützt wird. Vulnerability Advisor überprüft die Konfigurationseinstellungen für die folgenden App-Typen:

- MySQL
- NGINX
- Apache

## Sicherheitslückenbericht überprüfen
{: #va_reviewing}

Vor dem Bereitstellen eines Images können Sie sich anhand des zugehörigen Vulnerability Advisor-Berichts über Details zu Paketen mit potenziellen Sicherheitslücken und zu nicht sicheren Container- oder App-Einstellungen informieren. Darüber hinaus können Sie prüfen, ob das Image den Richtlinien Ihrer Organisation entspricht.
{:shortdesc}

Wenn festgestellte Sicherheitslücken nicht behoben werden, können diese Probleme die Sicherheit von Containern beeinträchtigen, die mithilfe dieses Images erstellt werden. Falls Container Image Security Enforcement nicht bereitgestellt ist, ist es möglich, ein Image mit Sicherheits- und Konfigurationsproblemen weiterhin in einem Container zu verwenden. Ist Container Image Security Enforcement für das Image bereitgestellt und aktiv, müssen für alle festgestellten Probleme Ausnahmen für die Richtlinie festgelegt werden, damit Container auf der Basis dieses Images bereitgestellt werden können.

Informationen zum Konfigurieren des Umsetzungsrahmens im Zusammenhang mit Vulnerability Advisor-Problemen in Container Image Security Enforcement finden Sie in [Richtlinien anpassen](/docs/services/Registry?topic=registry-security_enforce#customize_policies).
{:tip}

Wenn das Image nicht den Anforderungen entspricht, die durch die Richtlinie Ihrer Organisation festgelegt sind, müssen Sie das Image so konfigurieren, dass diese Anforderungen erfüllt sind, bevor Sie es bereitstellen können. Weitere Informationen zum Anzeigen und Ändern der Organisationsrichtlinie finden Sie in [Ausnahmen für Richtlinien für Organisationen festlegen](#va_managing_policy).
{:tip}

Falls Container Scanner bereitgestellt ist, sucht Vulnerability Advisor nach der Bereitstellung des Images weiter nach Sicherheits- und Konfigurationsproblemen im Container. Festgestellte Probleme können Sie anhand der Schritte beheben, die in [Containerbericht überprüfen](#va_reviewing_container) beschrieben sind.

### Sicherheitslückenbericht über die grafische Benutzerschnittstelle überprüfen
{: #va_reviewing_gui}

Sie können die Sicherheit von Docker-Images, die in Ihren Namensbereichen in {{site.data.keyword.registrylong_notm}} gespeichert sind, über die GUI überprüfen.
{:shortdesc}

1. Melden Sie sich bei {{site.data.keyword.cloud_notm}} an.
2. Klicken Sie auf das Symbol **Navigationsmenü** und anschließend auf **Kubernetes**.
3. Klicken Sie auf **Registry** und anschließend auf die Kachel **Images**. Eine Liste der Images und der Sicherheitsstatus der einzelnen Images werden in der Tabelle **Images** angezeigt.
4. Zeigen Sie den Bericht für das aktuelle Image (mit dem Tag `latest` markiert) an, indem Sie auf die Zeile für das betreffende Image klicken. Die Registerkarte mit den **Imagedetails** wird geöffnet und enthält die Daten für dieses Image. Enthält das Repository kein Image mit dem Tag `latest`, wird das neueste Image verwendet.
5. Wenn der Sicherheitsstatus Probleme aufweist, klicken Sie auf die Registerkarte **Probleme nach Typ**, um Informationen zu den Problemen anzuzeigen. Die Tabellen **Sicherheitslücken** und **Konfigurationsprobleme** werden geöffnet.

   - **Sicherheitslücken**: Diese Tabelle enthält die Sicherheitslücken-ID für die einzelnen Probleme, den Richtlinienstatus für das jeweilige Problem, die betroffenen Pakete und Maßnahmen zur Behebung des Problems. Erweitern Sie die Zeile, um weitere Informationen zum jeweiligen Problem anzuzeigen. Eine Zusammenfassung dieses Problems wird angezeigt, die einen Link zum Sicherheitshinweis des Anbieters für das betreffende Problem enthält. Pakete, die bekannten Probleme im Hinblick auf Sicherheitslücken aufweisen, werden aufgeführt.
  
     Die Liste wird täglich anhand der veröffentlichten Sicherheitshinweise für die unter [Typen von Sicherheitslücken](#types) aufgelisteten Docker-Imagetypen aktualisiert. In der Regel wird zum Bestehen der Sicherheitsprüfung bei einem potenziell gefährdeten Paket eine neue Version des Pakets benötigt, die eine Korrektur für die Sicherheitslücke enthält. Für ein Paket können mehrere Sicherheitslücken aufgelistet werden. In diesem Fall können durch ein einzelnes Upgrade für das Paket unter Umständen auch mehrere Probleme behoben werden. Klicken Sie auf den Code des Sicherheitshinweises, um weitere Informationen zum Paket und Schritte zur Aktualisierung des Pakets anzuzeigen.

   - **Konfigurationsprobleme**: Diese Tabelle enthält die Konfigurationsproblem-ID für die einzelnen Probleme, den Richtlinienstatus für das jeweilige Problem sowie das entsprechende Sicherheitsverfahren. Erweitern Sie die Zeile, um weitere Informationen zum jeweiligen Problem anzuzeigen. Eine Zusammenfassung dieses Problems wird angezeigt, die einen Link zum Sicherheitshinweis für das betreffende Problem enthält.
  
     Die Liste enthält Vorschläge für Aktionen, mit denen Sie die Sicherheit des Containers und aller nicht sicheren Anwendungseinstellungen des Containers erhöhen können. Erweitern Sie die Zeile, um den Lösungsvorschlag für das jeweilige Problem anzuzeigen.

6. Führen Sie die im Bericht angegebenen Maßnahmen zum Beheben der einzelnen Probleme aus und erstellen Sie das Image neu.

### Sicherheitslückenbericht über die Befehlszeilenschnittstelle überprüfen
{: #va_registry_cli}

Sie können die Sicherheit von Docker-Images, die in Ihren Namensbereichen in {{site.data.keyword.registrylong_notm}} gespeichert sind, über die Befehlszeilenschnittstelle überprüfen.
{:shortdesc}

1. Rufen Sie eine Liste der Images in Ihrem {{site.data.keyword.cloud_notm}}-Konto auf. Eine Liste aller Images wird zurückgegeben, unabhängig vom Namensbereich, in dem sie gespeichert sind.

   ```
   ibmcloud cr image-list
   ```
   {: pre}

2. Überprüfen Sie den Status in der Spalte **Sicherheitsstatus**.
    - **Keine Probleme**: Es wurden keine Sicherheitsprobleme gefunden.
    - **`<X>` Probleme** `<X>` Es wurden potenzielle Sicherheitsprobleme oder Sicherheitslücken gefunden. `<X>` gibt die Anzahl der Probleme an. 
    - **Scan wird durchgeführt**: Das Image wird zurzeit gescannt und der abschließende Sicherheitslückenstatus liegt noch nicht vor.

3. Über Details zum Status können Sie sich im Vulnerability Advisor-Bericht informieren:

   ```
   ibmcloud cr va registry.<region>/<my_namespace>/<my_image>:<tag>
   ```
   {: pre}

   In der Ausgabe der Befehlszeilenschnittstelle können Sie die folgenden Informationen zu den Konfigurationsproblemen anzeigen.
      - **Sicherheitsverfahren**: Beschreibung der gefundenen Sicherheitslücke.
      - **Fehlerbehebungsmaßnahme**: Details zur Vorgehensweise zur Behebung der Sicherheitslücke.

## Ausnahmen für Richtlinien für Organisationen festlegen
{: #va_managing_policy}

Bei der Verwaltung der Sicherheit einer {{site.data.keyword.cloud_notm}}-Organisation können Sie mithilfe der Richtlinieneinstellungen bestimmen, ob für ein Problem eine Ausnahme besteht. Sie können mithilfe von Container Image Security Enforcement sicherstellen, dass die Bereitstellung nur für Images zulässig ist, die außer den durch Ihre Richtlinie ausgenommenen Problemen keine Sicherheitsprobleme aufweisen.
{:shortdesc}

Sie können Container aus jedem beliebigen Image unabhängig vom Sicherheitsstatus bereitstellen, es sei denn, Container Image Security Enforcement ist im Cluster bereitgestellt. Informationen zur Bereitstellung von Container Image Security Enforcement finden Sie in [Security Enforcement installieren](/docs/services/Registry?topic=registry-security_enforce#security_enforce).

Wenn Container Image Security Enforcement verwendet wird, verhindert jedes von Vulnerability Advisor festgestellte Sicherheitsproblem, dass ein Container auf der Basis eines Images bereitgestellt wird. Damit ein Image mit festgestellten Problemen bereitgestellt werden kann, müssen Ausnahmen zur Richtlinie hinzugefügt werden.

### Ausnahmen für Richtlinien für Organisationen über die grafische Benutzerschnittstelle festlegen
{: #va_managing_policy_gui}

Führen Sie die folgenden Schritte aus, um Ausnahmen für die Richtlinie über die grafische Benutzerschnittstelle festzulegen:

1. Melden Sie sich bei {{site.data.keyword.cloud_notm}} an. Sie müssen angemeldet sein, um Vulnerability Advisor in der GUI anzuzeigen.
2. Klicken Sie auf das Symbol **Navigationsmenü** und anschließend auf **Kubernetes**.
3. Klicken Sie unter **Vulnerability Advisor** auf **Richtlinieneinstellungen**.
4. Klicken Sie auf **Ausnahme erstellen**.
5. Wählen Sie den Problemtyp aus.
6. Geben Sie die Problem-ID ein.

   Diese Informationen finden Sie im [Sicherheitslückenbericht](#va_reviewing). Die Spalte **Sicherheitslücken-ID** enthält die ID für CVE-Probleme oder Probleme im Zusammenhang mit Sicherheitshinweisen; die Spalte **Konfigurationsproblem-ID** enthält die ID für Konfigurationsprobleme.
   {: tip}

7. Wählen Sie den Registry-Namensbereich, das Repository und den Tag aus, für die die Ausnahme gelten soll.
8. Klicken Sie auf **Speichern**.

Sie können Ausnahmen auch bearbeiten und entfernen, indem Sie den Mauszeiger über die betreffende Zeile bewegen und auf das Symbol **Optionsliste öffnen und schließen** klicken.

### Ausnahmen für Richtlinien für Organisationen über die Befehlszeilenschnittstelle festlegen
{: #va_managing_policy_cli}

Führen Sie die folgenden Befehle aus, um Ausnahmen für die Richtlinie über die Befehlszeilenschnittstelle festzulegen:

- Zum Erstellen einer Ausnahme für ein Sicherheitsproblem führen Sie den Befehl [`ibmcloud cr exemption-add`](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_exemption_add) aus.
- Zum Auflisten Ihrer Ausnahmen für Sicherheitsprobleme führen Sie den Befehl [`ibmcloud cr exemption-list`](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_exemption_list) aus.
- Zum Auflisten der Sicherheitsproblemtypen, für die eine Ausnahme festgelegt werden kann, führen Sie den Befehl [`ibmcloud cr exemption-types`](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_exemption_types) aus.
- Zum Löschen einer Ausnahme für ein Sicherheitsproblem führen Sie den Befehl [`ibmcloud cr exemption-rm`](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_exemption_rm) aus.

Wenn Sie weitere Informationen zu Befehlen aufrufen möchten, können Sie das Flag `--help` bei der Ausführung des jeweiligen Befehls verwenden.

## Container Scanner installieren
{: #va_install_container_scanner}

Container Scanner ermöglicht es Vulnerability Advisor, in aktiven Containern festgestellte Probleme zu melden, die im Basisimage des Containers nicht vorhanden sind. Wenn Sie keine Laufzeitänderungen am Container vornehmen, ist Container Scanner nicht erforderlich, da im Imagebericht dieselben Probleme aufgeführt sind.
{:shortdesc}

Wenn Sie den Sicherheitsstatus von Live-Containern überprüfen möchten, die in Ihrem Cluster ausgeführt werden, können Sie Container Scanner installieren. Zum Schutz Ihrer App scannt Container Scanner regelmäßig Ihre aktiven Container, sodass Sicherheitslücken festgestellt und neu festgestellte Sicherheitslücken behoben werden können.

Sie können Container Scanner so einrichten, dass die Container, die Pods in allen Ihren Kubernetes-Namensbereichen zugewiesen sind, auf Sicherheitslücken überwacht werden. Werden Sicherheitslücken festgestellt, müssen Sie Probleme im Image beheben und die App anschließend erneut bereitstellen. Container Scanner unterstützt nur Container, die auf der Basis von Images erstellt werden, die in {{site.data.keyword.registrylong_notm}} gespeichert sind.

Zur Verwendung von Container Scanner müssen Sie Berechtigungen einrichten. Anschließend müssen Sie ein [Helm-Diagramm ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.helm.sh/developing_charts) einrichten und es dem Cluster zuordnen, in dem es verwendet werden soll.

### Serviceberechtigungen für Container Scanner einrichten
{: #va_install_container_scanner_permissions}

Container Scanner erfordert das Einrichten von Berechtigungen für die Ausführung des Service.
{:shortdesc}

Führen Sie die folgenden Schritte aus, um Serviceberechtigungen einzurichten:

1. Melden Sie sich beim {{site.data.keyword.cloud_notm}}-CLI-Client an. Wenn Sie über ein eingebundenes Konto verfügen, verwenden Sie `--sso`.
2. [Rufen Sie in der `kubectl`-Befehlszeilenschnittstelle](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure) den Cluster auf, für den Sie ein Helm-Diagramm verwenden möchten.
3. Erstellen Sie eine Service-ID und einen API-Schlüssel für Container Scanner und ordnen Sie einen Namen zu:
    1. Führen Sie den folgenden Befehl aus, um eine Service-ID zu erstellen. Dabei ist `<scanner_serviceID>` ein von Ihnen ausgewählter Name für die Service-ID. Beachten Sie den entsprechenden **CRN** (Cloud Resource Name).

       ```
       ibmcloud iam service-id-create <scanner_serviceID>
       ```
       {: codeblock}

    2. Erstellen Sie eine API-Schlüssel für den Service. Dabei ist `<scanner_serviceID>` die Service-ID, die Sie im vorherigen Schritt erstellt haben, und `<scanner_APIkey_name>` ist ein von Ihnen ausgewählter Name für den Scanner-API-Schlüssel. 

       ```
       ibmcloud iam service-api-key-create <scanner_APIkey_name> <scanner_serviceID>
       ```
       {: codeblock}
       Der API-Schlüssel des Scanners wird zurückgegeben.

       Stellen Sie sicher, dass der Scanner-API-Schlüssel sicher gespeichert wird, da er zu einem späteren Zeitpunkt nicht erneut abgerufen werden kann. Stellen Sie darüber hinaus sicher, dass Sie für jeden Cluster, in dem der Scanner installiert ist, über einen separaten API-Schlüssel verfügen.
       {: important}

    3. Erstellen Sie eine Servicerichtlinie, mit der die Rolle eines `Schreibberechtigten` erteilt wird.

       ```
       ibmcloud iam service-policy-create --resource-type scaningress --service-name container-registry --roles Writer <scanner_serviceID>
       ```
       {: codeblock}

### Helm-Diagramm konfigurieren
{: #va_install_container_scanner_helm}

Konfigurieren Sie ein Helm-Diagramm und ordnen Sie es dem Cluster zu, in dem es verwendet werden soll.
{:shortdesc}

Führe Sie die folgenden Schritte aus, um ein Helm-Diagramm zu konfigurieren:

1. [Richten Sie Helm im IBM Cloud Kubernetes-Service ein](/docs/containers?topic=containers-helm#helm). Wenn Sie eine RBAC-Richtlinie (rollenbasierte Zugriffssteuerung) zum Erteilen des Zugriffs auf Tiller verwenden, müssen Sie sicherstellen, dass die Tiller-Rolle über Zugriff auf alle Namensbereiche verfügt. Indem Sie der Tiller-Rolle die Zugriffsberechtigung für alle Namensbereiche erteilen, stellen Sie sicher, dass Container Scanner die Container in allen Namensbereichen überwachen kann.

2. Fügen Sie das IBM Diagrammrepository zu Helm hinzu, z. B. `ibm`.

   ```
   helm repo add ibm https://icr.io/helm/ibm
   ```
   {: pre}

3. Speichern Sie die Standardkonfigurationseinstellungen für das Helm-Diagramm von Container Scanner in einer lokalen YAML-Datei. Beziehen Sie das Diagrammrepository, z. B. `ibm`, in den Helm-Diagrammpfad ein.

   ```
   helm inspect values ibm/ibmcloud-container-scanner > config.yaml
   ```
   {: pre}

4. Bearbeiten Sie die Datei `config.yaml`.

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
   <caption>Tabelle 2. Erläuterung der YAML-Dateikomponenten</caption>
   <thead>
   <th>Feld</th>
   <th>Wert</th>
   </thead>
   <tbody>
   <tr>
   <td><code>EmitURL</code></td>
   <td>Ersetzen Sie <code>&lt;regional_emit_URL&gt;</code> durch die URL des regionalen Endpunkts für Vulnerability Advisor. Führen Sie zum Abrufen der URL <code>ibmcloud cr info</code> aus und rufen Sie die Adresse der <strong>Container-Registry</strong> ab. Beispiel: <code>https<span comment="make the link not a link">://us.</span>icr.io</code>. <code>/va</code> an das Ende dieser Adresse anfügen. Beispiel: <code>https<span comment="make the link not a link">://us.</span>icr.io/va</code>. Weitere Informationen zu Regionen finden Sie in [Lokale Regionen](/docs/services/Registry?topic=registry-registry_overview#registry_regions_local).</td>
   </tr>
   <tr>
   <td><code>AccountID</code></td>
   <td>Ersetzen Sie <code>&lt;IBM_Cloud_account_ID&gt;</code> durch die ID des {{site.data.keyword.cloud_notm}}-Kontos, in dem sich der Cluster befindet. Zum Abrufen der Konto-ID führen Sie <code>ibmcloud account list</code> aus.</td>
   </tr>
   <tr>
   <td><code>ClusterID</code></td>
   <td>Ersetzen Sie <code>&lt;cluster_ID&gt;</code> durch den Kubernetes-Cluster, in dem Container Scanner installiert werden soll. Zum Abrufen einer Liste mit Cluster-IDs führen Sie <code>ibmcloud ks clusters</code> aus. <br> **Tipp**: Verwenden Sie die ID des Clusters, nicht den Namen.
   </td>
   </tr>
   <tr>
   <td><code>APIKey</code></td>
   <td>Ersetzen Sie <code>&lt;scanner_APIkey&gt;</code> durch den API-Schlüssel des Scanners, der in einem der vorhergehenden Schritte erstellt wurde.</td>
   </tr>
   </tbody></table>

5. Installieren Sie das Helm-Diagramm mit der aktualisierten Datei `config.yaml` im Cluster. Die aktualisierten Eigenschaften werden in einer Konfigurationsübersicht (ConfigMap) für das Diagramm gespeichert. Ersetzen Sie `<myscanner>` durch den gewünschten Namen für das Helm-Diagramm. Beziehen Sie das Diagrammrepository, z. B. `ibm`, in den Helm-Diagrammpfad ein.

   ```
   helm install -f config.yaml --name=<myscanner> ibm/ibmcloud-container-scanner
   ```
   {: pre}

   Container Scanner wird im Namensbereich `kube-system` installiert, scannt jedoch Container aller Namensbereiche.
   {:tip}

6. Überprüfen Sie den Bereitstellungsstatus des Diagramms. Wenn das Diagramm bereit ist, wird im Feld **Status** der Wert `Bereitgestellt` angezeigt.

   ```
   helm status <myscanner>
   ```
   {: pre}

7. Vergewissern Sie sich nach der Bereitstellung des Diagramms, dass die aktualisierten Einstellungen in `config.yaml` verwendet wurden.

   ```
   helm get values <myscanner>
   ```
   {: pre}

Container Scanner ist nun installiert und der Agent ist als [Dämongruppe ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) im Cluster bereitgestellt. Container Scanner wird zwar im Namensbereich `kube-system` bereitgestellt, scannt jedoch alle Container, die Pods in allen Ihren Kubernetes-Namensbereichen zugewiesen sind, wie z. B. `default`.

## Container Scanner hinter einer Firewall ausführen
{: #va_firewall}

Wenn Ihre Firewall abgehende Verbindungen blockiert, müssen Sie sie so konfigurieren, dass Workerknoten auf den Container Scanner am TCP-Port `443` über die entsprechenden IP-Adressen zugreifen können. Informationen hierzu finden Sie in Schritt 3 unter [Zugriff des Clusters auf Infrastrukturressourcen und andere Services über eine öffentliche Firewall zulassen](/docs/containers?topic=containers-firewall#firewall_outbound) in der {{site.data.keyword.containerlong_notm}}-Dokumentation.
{:shortdesc}

## Containerbericht überprüfen
{: #va_reviewing_container}

Im Dashboard können Sie den Status eines Containers anzeigen, um festzustellen, ob seine Sicherheit den Richtlinien Ihrer Organisation entspricht. Darüber hinaus können Sie den Sicherheitsbericht eines Containers überprüfen, in dem alle gefährdeten Pakete und nicht sicheren Container- oder Anwendungseinstellungen aufgeführt sind und der angibt, ob der Container den Richtlinien Ihrer Organisation entspricht.
{:shortdesc}

Vergewissern Sie sich, dass in Ihrem Bereich ausgeführte Container weiterhin den Organisationsrichtlinien entsprechen, indem Sie das Feld **Richtlinienstatus** überprüfen. Der angezeigte Status entspricht einer der folgenden Bedingungen:

- **Entspricht der Richtlinie** - Es wurden keine Sicherheits- oder Konfigurationsprobleme festgestellt.
- **Entspricht der Richtlinie nicht** - Vulnerability Advisor hat potenzielle Sicherheits- oder Konfigurationsprobleme festgestellt, die dazu führen, dass der Container die Vorgaben der Richtlinie nicht erfüllt. Wenn Ihre Organisationsrichtlinie die Bereitstellung von Images mit Sicherheitslücken zulässt, kann das Image mit dem Status `Mit entsprechenden Vorsichtsmaßnahmen bereitstellen` bereitgestellt werden und an den Benutzer, der es bereitstellt, wird eine Warnung gesendet.
- **Beurteilung unvollständig** - Der Scan wurde nicht abgeschlossen. Der Scan wird möglicherweise noch ausgeführt oder das Betriebssystem der betreffenden Containerinstanz ist möglicherweise nicht kompatibel.

Vergewissern Sie sich, dass der Container so sicher wie möglich ist, indem Sie den zugehörigen Sicherheitsbericht anzeigen und für alle gemeldeten Sicherheits- oder Konfigurationsprobleme die entsprechenden Maßnahmen durchführen. Führen Sie hierzu die folgenden Schritte aus:

1. Wählen Sie den Container aus, für den ein Bericht angezeigt werden soll:
    1. Klicken Sie auf das Symbol **Navigationsmenü** und anschließend auf **Kubernetes**.
    2. Klicken Sie auf **Registry** und anschließend auf die Kachel **Repositorys**. Erweitern Sie dann die Zeile für das gewünschte Repository.
    3. Wählen Sie die Zeile für das gewünschte Image aus.
    4. Wählen Sie die Registerkarte **Zugehörige Container** und anschließend die Zeile für den gewünschten Container aus. Der Sicherheitsbericht wird geöffnet.
2. Überprüfen Sie die jeweiligen Abschnitte, um die potenziellen Sicherheits- und Konfigurationsprobleme für die einzelnen Pakete im Image anzuzeigen:

    - **Sicherheitslücken**: Listet die Pakete auf, die bekannten Probleme im Hinblick auf Sicherheitslücken aufweisen. Die Liste wird täglich anhand der veröffentlichten Sicherheitshinweise für die unter [Typen von Sicherheitslücken](#types) aufgelisteten Docker-Imagetypen aktualisiert. In der Regel wird zum Bestehen der Sicherheitsprüfung bei einem potenziell gefährdeten Paket eine neue Version des Pakets benötigt, die eine Korrektur für die Sicherheitslücke enthält. Für ein Paket können mehrere Sicherheitslücken aufgelistet werden. In diesem Fall können durch ein einzelnes Upgrade für das Paket unter Umständen auch mehrere Probleme behoben werden. Klicken Sie auf den Code des Sicherheitshinweises, um weitere Informationen zum Paket und Schritte zur Aktualisierung des Pakets anzuzeigen.

    - **Konfigurationsprobleme**: Listet Vorschläge auf, mit denen Sie die Sicherheit des Containers und aller nicht sicheren Anwendungseinstellungen des Containers erhöhen können. Erweitern Sie die Zeile, um den Lösungsvorschlag für das jeweilige Problem anzuzeigen.

   Problembehebungsmaßnahmen oder -vorschläge werden zu jedem aufgeführten Punkt angegeben.

3. Überprüfen Sie den Richtlinienstatus für die einzelnen Sicherheitsprobleme. Der Richtlinienstatus gibt an, ob für dieses Problem eine Ausnahme gilt.

    - **Aktiv**: Es liegt ein Problem vor, für das keine Ausnahme gilt und das sich auf den Sicherheitsstatus auswirkt.
    - **Ausgenommen**: Für dieses Problem ist in den Richtlinieneinstellungen eine Ausnahme festgelegt.
    - **Teilweise ausgenommen**: Dieses Problem ist mehreren Sicherheitshinweisen zugeordnet. Nicht für alle Sicherheitshinweise gilt eine Ausnahme.

4. Entscheiden Sie, wie der Container aktualisiert werden soll, damit die Probleme behoben werden können.

    **Wichtig**: Um Probleme im Container-Image zu beheben, müssen Sie die alte Instanz löschen und eine erneute Bereitstellung durchführen, wodurch alle Daten im vorhandenen Container verloren gehen. Stellen Sie sicher, dass Sie mit der Containerarchitektur vertraut sind, damit Sie die geeignete Methode zur erneuten Bereitstellung des Containers auswählen können.

    **Beispiel**

    - Wenn der Container von den Daten, die er berechnet, entkoppelt ist, können Sie den Container stoppen und löschen, die erforderlichen Änderungen am Image vornehmen und eine erneute Bereitstellung durchführen, ohne dass Daten verloren gehen.
    - Sie können einen {{site.data.keyword.cloud_notm}}-Service, wie z. B. [Delivery Pipeline](/docs/services/ContinuousDelivery?topic=ContinuousDelivery-deliverypipeline_about#deliverypipeline_about), zur Unterstützung bei der Aktualisierung der gefährdeten Containerinstanz verwenden.
    - In einer Mikroservicearchitektur können Sie den Datenverkehr an eine andere Containerinstanz weiterleiten, während Sie Sicherheits- oder Konfigurationsprobleme beheben, und das neue Image in einer Red/Black-Bereitstellung mithilfe einer Push-Operation übertragen.

5. Wenn Sie das Problem zum gegenwärtigen Zeitpunkt nicht beheben können, können Sie in den Richtlinieneinstellungen eine Ausnahme für das Problem festlegen; auf diese Weise kann das Problem die Bereitstellung des Containers nicht blockieren. Wenn Sie eine Ausnahme für das Problem festlegen möchten, klicken Sie auf das Symbol **Optionsliste öffnen und schließen** und anschließend auf **Ausnahme erstellen**. Informationen hierzu finden Sie in [Ausnahmen für Richtlinien für Organisationen festlegen](#va_managing_policy).

6. Beheben Sie die im **Sicherheitsbericht** beschriebenen Probleme und erstellen Sie das Image erneut bzw. stellen Sie den Container erneut bereit - je nach ausgewählter Methode.
