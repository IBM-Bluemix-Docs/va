---

copyright:
  years: 2017
lastupdated: "2017-10-26"

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

Vulnerability Advisor überprüft den Sicherheitsstatus von Container-Images vor der Bereitstellung.
{:shortdesc}


## Informationen zu Vulnerability Advisor 
{: #about}

Mit Vulnerability Advisor stehen Sicherheitsmanagementfunktionen für {{site.data.keyword.containerlong}} zur Verfügung. Vulnerability Advisor generiert einen Sicherheitsstatusbericht, liefert Vorschläge für Korrekturen und bewährte Verfahren und stellt die Managementfunktionalität bereit, mit der die Ausführung nicht sicherer Images verhindert werden kann. Durch das Beheben der von Vulnerability Advisor gemeldeten Sicherheits- und Konfigurationsprobleme können Sie die {{site.data.keyword.cloud_notm}}-Infrastruktur schützen.
{:shortdesc}

Vulnerability Advisor umfasst die folgenden Features:

-   Scannen von Images auf Sicherheitslücken
-   Bereitstellen eines Auswertungsberichts basierend auf Sicherheitsstandards wie ISO 27002 sowie auf für {{site.data.keyword.containerlong_notm}} spezifischen Sicherheitsverfahren
-   Erkennen dateibasierter Malware
-   Bereitstellen von Empfehlungen zur Sicherung von Konfigurationsdateien für eine Untergruppe von Anwendungstypen
-   Bereitstellen von Anweisungen zum Beheben von gemeldeten Sicherheitslücken oder Konfigurationsproblemen in entsprechenden Berichten

<dl>
  <dt><strong>Pakete mit potenziellen Sicherheitslücken</strong></dt>
  <dd>Vulnerability Advisor überprüft Images, die auf unterstützten Betriebssystemen basieren, auf Pakete mit potenziellen Sicherheitslücken und stellt einen Link bereit, über den relevante Sicherheitshinweise zur jeweiligen Sicherheitslücke aufgerufen werden können. Vulnerability Advisor aktualisiert die interne Liste des Programms täglich anhand dieser Sicherheitshinweise. Unterstützte Basisimages werden in der folgenden Tabelle beschrieben.</dd>
</dl>

  |Docker-Basisimage|Quelle der Sicherheitshinweise|
  |-----------------|--------------------------|
  |Alpine|[Git - Alpine Linux ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://git.alpinelinux.org/) und [CIRCL CVE Search ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.circl.lu/)|
  |CentOS| [CentOS-announce Archives ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://lists.centos.org/pipermail/centos-announce/) und [CentOS CR-announce Archives ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://lists.centos.org/pipermail/centos-cr-announce/)|
  |Debian|[Debian Security Announcements ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://lists.debian.org/debian-security-announce/)|
  |Red Hat Enterprise Linux (RHEL)|[Red Hat Product Errata ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://access.redhat.com/errata/#/)|
  |Ubuntu|[Ubuntu Security Notices ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ubuntu.com/usn/)|
  {: caption="Tabelle 1. Docker-Basisimages, die von Vulnerability Advisor auf Pakete mit potenziellen Sicherheitslücken überprüft werden" caption-side="top"}










## Imagesicherheit für Docker-Images, die in einem Namensbereich in {{site.data.keyword.registrylong_notm}} gespeichert sind, über die Befehlszeilenschnittstelle überprüfen 
{: #va_registry_cli}

Sie können die Sicherheit von Docker-Images überprüfen, die in einem Namensbereich gespeichert sind, um Informationen zu möglichen Sicherheitslücken zu erhalten.
{:shortdesc}

Wenn Sie ein Image zu {{site.data.keyword.registrylong_notm}} hinzufügen, wird das Image automatisch durch Vulnerability Advisor gescannt, um Sicherheitsprobleme und potenzielle Sicherheitslücken zu ermitteln. Container, die von gefährdeten Images implementiert werden, könnten angegriffen und beschädigt werden. Images werden nur dann überprüft, wenn sie auf einem Betriebssystem basieren, das von Vulnerability Advisor unterstützt wird.

Die Überprüfung durch Vulnerability Advisor umfasst die folgenden Sicherheitslücken. Wenn Sicherheitsprobleme gefunden werden, werden Anweisungen zur Verfügung gestellt, um die gemeldete Sicherheitslücke zu beheben.

Führen Sie die folgenden Schritte aus, um den Sicherheitslückenstatus von Images in Ihrem {{site.data.keyword.Bluemix_notm}}-Konto zu überprüfen.

1.  Rufen Sie eine Liste aller Images in Ihrem {{site.data.keyword.Bluemix_notm}}-Konto auf. Der folgende Befehl gibt eine Liste aller Images ungeachtet des Namensbereichs zurück, in dem sie gespeichert sind.

    ```
    bx cr image-list
    ```
    {: pre}

2.  Überprüfen Sie den Status in der Spalte SICHERHEITSLÜCKE - STATUS. Einer der folgenden Statuswerte wird angezeigt: 
    -   OK. Dieser Status bedeutet, dass keine Sicherheitsprobleme festgestellt wurden.
    -   Gefährdet. Dieser Status bedeutet, dass ein mögliches Sicherheitsproblem oder eine potenzielle Sicherheitslücke festgestellt wurde.
    -   Unbekannt. Dieser Status wird angezeigt, während das Image gescannt wird, bis der endgültige Sicherheitslückenstatus bestimmt werden kann.
    -   Nicht unterstütztes Betriebssystem. Dieser Status wird angezeigt, wenn das Scannen des Image durch Vulnerability Advisor nicht unterstützt wird.
4.  Weitere Informationen zum Status finden Sie im Vulnerability Advisor-Bericht.

    ```
    bx cr va registry.<Region>/<eigener Namensbereich>/<eigenes Image>:<Tag>
    ```
    {: pre}

    Die Ausgabe der Befehlszeilenschnittstelle enthält eine Liste der Pakete mit potenziellen Sicherheitslücken, eine Beschreibung der festgestellten Sicherheitslücke sowie einen Link zu Anweisungen, mit denen Sie diese Lücke schließen können.


## Probleme in Images beheben 
{: #va_report}

Beispielkorrekturen für häufig auftretende Probleme, die von Vulnerability Advisor gemeldet werden.
{:shortdesc}

Vulnerability Advisor stellt Fehlerbehebungsmaßnahmen für die gemeldeten Sicherheits- oder Konfigurationsprobleme bereit. Einige der gemeldeten Probleme können durch die Aktualisierung der Dockerfile behoben werden. Mithilfe der folgenden Codebeispiele können Sie die Lösung in der Dockerfile implementieren und so häufig auftretende Probleme schneller beheben.

### Maximale Gültigkeitsdauer des Kennworts, Kennwortmindestgültigkeit in Tagen und minimale Kennwortlänge
{: #va_password}

Problem: Angezeigt wird/werden einer oder alle der folgenden Fehler:

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

Korrektur: Stellen Sie sicher, dass die Handhabung von Kennwörtern mit den entsprechenden Vorgaben konform ist, indem Sie den folgenden Code zu Ihrer Dockerfile hinzufügen.

```
RUN \
    sed -i 's/^PASS_MAX_DAYS.*/PASS_MAX_DAYS    90/' /etc/login.defs && \
    sed -i 's/^PASS_MIN_DAYS.*/PASS_MIN_DAYS    1/' /etc/login.defs && \
    sed -i 's/sha512/sha512 minlen=8/' /etc/pam.d/common-password
```

### SSH-Sicherheitslücke 
{: #ssh}

Problem: Der folgende Fehler wird zurückgegeben:

```
Der SSH-Server darf nicht installiert sein.
```
{: screen}

Korrektur: Verwenden Sie für den Zugriff auf den Container `docker attach` oder `docker exec` anstelle von SSH. Stellen Sie sicher, dass Ihre Dockerfile keine Schritte für die Installation eines SSH-Servers enthält.

