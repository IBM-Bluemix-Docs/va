---

copyright:
  years: 2017, 2019
lastupdated: "2019-08-05"

keywords: IBM Cloud Kubernetes Service, IBM Cloud Container Registry, security status of container images, image security, Vulnerability Advisor, security, registry, vulnerabilities, containers, security issues, configuration issues,

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

Vulnerability Advisor überprüft den Sicherheitsstatus von Container-Images, die von {{site.data.keyword.IBM}} oder anderen Anbietern bereitgestellt oder die zum Registrynamensbereich Ihrer Organisation hinzugefügt werden.
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
- Bereitstellen eines Bewertungsberichts auf der Basis von Sicherheitsverfahren, die für {{site.data.keyword.containerlong_notm}} spezifisch sind
- Bereitstellen von Empfehlungen zur Sicherung von Konfigurationsdateien für eine Untergruppe von Anwendungstypen
- Bereitstellen von Anweisungen zum Korrigieren eines gemeldeten [Pakets mit potenziellen Sicherheitslücken](#packages) oder zum Beheben von [Konfigurationsproblemen](#app_configurations) auf der Basis des entsprechenden Berichts
- Bereitstellen von Urteilen für [Container Image Security Enforcement](/docs/services/Registry?topic=registry-security_enforce#security_enforce)
- Anwenden von Ausnahmen für Berichte auf Konto-, Namensbereichs-, Repository- oder Tagebene, um anzugeben, wann entsprechend gekennzeichnete Probleme für einen bestimmten Anwendungsfall nicht zutreffen

Im Registry-Dashboard wird in der Spalte **Richtlinienstatus** der Status Ihrer Repositorys angezeigt. Im Bericht mit dem entsprechenden Link werden Maßnahmen empfohlen, mit denen die Cloudsicherheit für Ihre Images erhöht werden kann.

Das Vulnerability Advisor-Dashboard enthält eine Übersicht und eine Beurteilung zu der Sicherheit eines Images. Weitere Informationen zu Vulnerability Advisor finden Sie im Abschnitt [Sicherheitslückenbericht für Ihr Image überprüfen](#va_reviewing).

### Datenschutz
{: #about_data_protection}

Für das Scannen von Images und Containern in Ihrem Konto zur Ermittlung potenzieller Sicherheitsprobleme werden von Vulnerability Advisor die folgenden Informationen erfasst, gespeichert und verarbeitet:

- Felder mit freiem Format, einschließlich IDs, Beschreibungen und Imagenamen (Registry, Namensbereich, Repositoryname und Image-Tag)
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

Pakete, die bekannten Probleme im Hinblick auf Sicherheitslücken aufweisen, werden in den Scanergebnissen angezeigt. Die Liste der potenziellen Sicherheitslücken wird täglich anhand der veröffentlichten Sicherheitshinweise für die Docker-Imagetypen in der nachfolgenden Tabelle aktualisiert. In der Regel wird zum Bestehen der Sicherheitsprüfung bei einem potenziell gefährdeten Paket eine neue Version des Pakets benötigt, die eine Korrektur für die Sicherheitslücke enthält. Für ein Paket können mehrere Sicherheitslücken aufgelistet werden. In diesem Fall können durch ein einzelnes Update für das Paket unter Umständen auch mehrere Probleme behoben werden.

  |Docker-Basisimage|Quelle der Sicherheitshinweise|
  |-----------------|--------------------------|
  |Alpine|[Git - Alpine Linux ![Symbol für externen Link](../../icons/launch-glyph.svg "External link icon")](https://git.alpinelinux.org/) und [CIRCL CVE Search ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.circl.lu/).|
  |CentOS| [CentOS-announce-Archive ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://lists.centos.org/pipermail/centos-announce/) und [CentOS CR-announce-Archive ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://lists.centos.org/pipermail/centos-cr-announce/). Weitere Informationen zu Sicherheitslücken finden Sie im Abschnitt [Sicherheitslücken in Paketen in CentOS](#va_centos).|
  |Debian|[Debian-Sicherheitshinweise ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://lists.debian.org/debian-security-announce/).|
  |Red Hat Enterprise Linux (RHEL)|[Red Hat-Sicherheitsdaten-API ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://access.redhat.com/labsinfo/securitydataapi).|
  |Ubuntu|[Ubuntu-Sicherheitshinweise ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://usn.ubuntu.com/).|
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

### Sicherheitslückenbericht über die grafische Benutzerschnittstelle überprüfen
{: #va_reviewing_gui}

Sie können die Sicherheit von Docker-Images, die in Ihren Namensbereichen in {{site.data.keyword.registrylong_notm}} gespeichert sind, über die GUI überprüfen.
{:shortdesc}

1. Melden Sie sich bei {{site.data.keyword.cloud_notm}} an.
2. Klicken Sie auf das Symbol **Navigationsmenü** und anschließend auf **Kubernetes**.
3. Klicken Sie auf **Registry** und anschließend auf die Kachel **Images**. Eine Liste der Images und der Sicherheitsstatus der einzelnen Images werden in der Tabelle **Images** angezeigt.
4. Zeigen Sie den Bericht für das aktuelle Image (mit dem Tag `latest` markiert) an, indem Sie auf die Zeile für das betreffende Image klicken. Die Registerkarte mit den **Imagedetails** wird geöffnet und enthält die Daten für dieses Image. Enthält das Repository kein Image mit dem Tag `latest`, wird das neueste Image verwendet.
5. Wenn der Sicherheitsstatus Probleme aufweist, klicken Sie auf die Registerkarte **Probleme nach Typ**, um Informationen zu den Problemen anzuzeigen. Die Tabellen **Sicherheitslücken** und **Konfigurationsprobleme** werden geöffnet.

   - Tabelle mit **Sicherheitslücken**. Diese Tabelle enthält die Sicherheitslücken-ID für die einzelnen Probleme, den Richtlinienstatus für das jeweilige Problem, die betroffenen Pakete und Maßnahmen zur Behebung des Problems. Erweitern Sie die Zeile, um weitere Informationen zum jeweiligen Problem anzuzeigen. Eine Zusammenfassung dieses Problems wird angezeigt, die einen Link zum Sicherheitshinweis des Anbieters für das betreffende Problem enthält. Pakete, die bekannten Probleme im Hinblick auf Sicherheitslücken aufweisen, werden aufgeführt.
  
     Die Liste wird täglich anhand der veröffentlichten Sicherheitshinweise für die unter [Typen von Sicherheitslücken](#types) aufgelisteten Docker-Imagetypen aktualisiert. In der Regel wird zum Bestehen der Sicherheitsprüfung bei einem potenziell gefährdeten Paket eine neue Version des Pakets benötigt, die eine Korrektur für die Sicherheitslücke enthält. Für ein Paket können mehrere Sicherheitslücken aufgelistet werden. In diesem Fall können durch ein einzelnes Update für das Paket unter Umständen auch mehrere Probleme behoben werden. Klicken Sie auf den Code des Sicherheitshinweises, um weitere Informationen zum Paket und Schritte zur Aktualisierung des Pakets anzuzeigen.

   - Tabelle mit **Konfigurationsproblen**. Diese Tabelle enthält die Konfigurationsproblem-ID für die einzelnen Probleme, den Richtlinienstatus für das jeweilige Problem sowie das entsprechende Sicherheitsverfahren. Erweitern Sie die Zeile, um weitere Informationen zum jeweiligen Problem anzuzeigen. Eine Zusammenfassung dieses Problems wird angezeigt, die einen Link zum Sicherheitshinweis für das betreffende Problem enthält.
  
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
    - `Keine Probleme`: Es wurden keine Sicherheitsprobleme gefunden.
    - `<X> Probleme` Die Anzahl der gefundenen potenziellen Sicherheitsprobleme oder Sicherheitslücken. `<X>` gibt die Anzahl der Probleme an.
    - `Scan wird durchgeführt`: Das Image wird zurzeit gescannt und der abschließende Sicherheitslückenstatus liegt nicht vor.

3. Über Details zum Status können Sie sich im Vulnerability Advisor-Bericht informieren:

   ```
   ibmcloud cr va registry.<region>/<my_namespace>/<my_image>:<tag>
   ```
   {: pre}

   In der Ausgabe der Befehlszeilenschnittstelle können Sie die folgenden Informationen zu den Konfigurationsproblemen anzeigen.
      - `Sicherheitsverfahren`: Beschreibung der gefundenen Sicherheitslücke.
      - `Fehlerbehebungsmaßnahme`: Details zur Vorgehensweise zur Behebung der Sicherheitslücke.

### Sicherheitslücken in Paketen in CentOS
{: #va_centos}

Wenn Sie CentOS verwenden, kann Ihr Bericht möglicherweise Fehlalarme enthalten, das bedeutet, dass Sicherheitslücken gemeldet werden, die nicht vorhanden sind. Dieser Fall tritt ein, wenn von Red Hat ein Sicherheitshinweis veröffentlicht wird, dieser aber nicht für CentOS gilt oder der Fix noch nicht in CentOS portiert wurde.
{:shortdesc}

Wenn Sie einen Bericht empfangen, laut dem Ihr Paket Sicherheitslücken aufweist, führen Sie die folgenden Schritte aus:

1. Klicken Sie auf den Code des Sicherheitshinweises, um die Schritte zum Aktualisieren des Pakets anzuzeigen.
2. Aktualisieren Sie das Paket, indem Sie die entsprechenden Schritte ausführen.
3. Wenn das Paket aktualisiert wird, war das Ergebnis kein Fehlalarm und die erforderliche Aktion ist abgeschlossen.
4. Wenn das Paket nicht aktualisiert wird, da keine neueren Versionen für die Installation verfügbar waren, war das Ergebnis ein Fehlalarm. Sie können für diesen Sicherheitshinweis eine Ausnahme für eine Richtlinie hinzufügen. Siehe hierzu [Ausnahmen für Richtlinien für Organisationen festlegen](#va_managing_policy).

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
