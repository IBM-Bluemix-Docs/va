---

copyright:
  years: 2017
lastupdated: "2017-12-05"

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

-   Überprüfen von Images auf Sicherheitslücken
-   Bereitstellen eines Berichts mit einer Bewertung anhand von Sicherheitsstandards wie ISO 27002, [Center of Internet Security ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.cisecurity.org/) und von Sicherheitsverfahren, die für {{site.data.keyword.containerlong_notm}} relevant sind. 
-   Erkennen dateibasierter Malware
-   Bereitstellen von Empfehlungen zur Sicherung von Konfigurationsdateien für eine Untergruppe von Anwendungstypen
-   Bereitstellen von Anweisungen zum Beheben von gemeldeten Sicherheitslücken oder Konfigurationsproblemen in entsprechenden Berichten
   

    


**Pakete mit potenziellen Sicherheitslücken**

Vulnerability Advisor überprüft Images, die auf unterstützten Betriebssystemen basieren, auf Pakete mit potenziellen Sicherheitslücken und stellt einen Link bereit, über den relevante Sicherheitshinweise zur jeweiligen Sicherheitslücke aufgerufen werden können. 

Pakete mit bekannten Problemen im Hinblick auf Sicherheitslücken werden angezeigt. Die Liste der potenziellen Sicherheitslücken wird täglich anhand veröffentlichter Sicherheitshinweise für die Docker-Imagetypen in der nachfolgenden Tabelle aktualisiert. In der Regel wird zum Bestehen der Sicherheitsprüfung bei einem potenziell gefährdeten Paket eine neue Version des Pakets benötigt, die eine Korrektur für die Sicherheitslücke enthält. Für ein Paket können mehrere Sicherheitslücken aufgelistet werden. In diesem Fall können durch ein einzelnes Upgrade für das Paket unter Umständen auch mehrere Probleme behoben werden. In der Spalte **CORRECTIVE ACTION** wird beschrieben, wie die Sicherheit erhöht werden kann. 

Unterstützte Basisimages werden in der folgenden Tabelle beschrieben.

  |Docker-Basisimage|Quelle der Sicherheitshinweise|
  |-----------------|--------------------------|
  |Alpine|[Git - Alpine Linux ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://git.alpinelinux.org/) und [CIRCL CVE Search ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://cve.circl.lu/)|
  |CentOS| [CentOS-announce Archives ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://lists.centos.org/pipermail/centos-announce/) und [CentOS CR-announce Archives ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://lists.centos.org/pipermail/centos-cr-announce/)|
  |Debian|[Debian Security Announcements ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://lists.debian.org/debian-security-announce/)|
  |Red Hat Enterprise Linux (RHEL)|[Red Hat Product Errata ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://access.redhat.com/errata/#/)|
  |Ubuntu|[Ubuntu Security Notices ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ubuntu.com/usn/)|
  {: caption="Tabelle 1. Docker-Basisimages, die von Vulnerability Advisor auf Pakete mit potenziellen Sicherheitslücken überprüft werden" caption-side="top"}
  



**Anwendungskonfigurationen**

Listet Anwendungseinstellungen für das Image auf, die nicht sicher sind. In der Spalte **CORRECTIVE ACTION** wird beschrieben, wie die Sicherheit erhöht werden kann. 




**Sicherheitsbericht**

Im Registry-Dashboard wird in der Spalte **SECURITY REPORT** der Status Ihrer Repositorys angezeigt. 

Die Konfigurationseinstellungen für die folgenden Anwendungstypen werden von Vulnerability Advisor auf bekannte Sicherheitslücken überprüft: 
-   MySQL
-   NGINX
-   Apache

Im Bericht werden Maßnahmen empfohlen, mit denen die Cloudsicherheit für Ihre Images erhöht werden kann. Sie können auf eine vollständige Liste der Sicherheits- und Konfigurationsprobleme zugreifen, die von Vulnerability Advisor überprüft werden. 

Das Vulnerability Advisor-Dashboard enthält eine Übersicht und eine Beurteilung zu der Sicherheit eines Images.  

Weitere Informationen zu Vulnerability Advisor finden Sie im Abschnitt [Sicherheitslückenbericht für Ihr Image überprüfen](#va_reviewing). 





## Sicherheitslückenbericht für Ihr Image überprüfen
{: #va_reviewing}

Vor dem Bereitstellen eines Images können Sie sich anhand des zugehörigen Vulnerability Advisor-Berichts über Details zu gefährdeten Paketen und problematischen Anwendungseinstellungen informieren.
{:shortdesc}

Container-Images werden von IBM oder anderen Anbietern bereitgestellt oder von Ihrer Organisation hinzugefügt. 

Verschaffen Sie sich wie im Folgenden beschrieben einen Einblick in potenzielle Sicherheits- und Konfigurationsprobleme eines Images: 

1.  Melden Sie sich bei {{site.data.keyword.Bluemix_notm}} an. Sie müssen angemeldet sein, um die grafische Benutzerschnittstelle von Vulnerability Advisor aufrufen zu können. 
2.  Klicken Sie auf **Katalog**. 
3.  Klicken Sie unter **Infrastruktur** auf **Container**.  
4.  Klicken Sie auf die Kachel **Container-Registry**. 
5.  Erweitern Sie den Eintrag **Vulnerability Advisor** und klicken Sie auf **Geprüfte Repositories**.  
6.  Zeigen Sie den Bericht für das aktuelle Image (mit dem Tag `latest` markiert) an, indem Sie auf die Zeile für das betreffende Repository klicken. Im Bericht wird die Gesamtzahl der Probleme angezeigt und angegeben, ob gefährdete Pakete oder Konfigurationsprobleme vorliegen. Enthält das Repository kein Image mit dem Tag `latest`, wird das neueste Image verwendet. 
7.  Zeigen Sie Informationen zu den einzelnen gefährdeten Paketen in der Tabelle **Vulnerable Packages Found** an, indem Sie auf den Link in der Spalte **VULNERABILITIES** klicken. Der zugehörige Bericht wird geöffnet. 
    1.  Zeigen Sie bei Bedarf weitere Informationen an, indem Sie die Zusammenfassung erweitern. 
    2.  Zeigen Sie die Hinweise der Firma an, von der Sie das Betriebssystem bezogen haben, indem Sie auf den Link in der Spalte **OFFICIAL NOTICE** klicken. 
8.  Zeigen Sie Informationen zu den einzelnen Konfigurationsproblemen in der Tabelle **Configuration Issues Found** an, indem Sie jeweils auf die entsprechenden Zeilen klicken.  
9.  Führen Sie die im Bericht angegebenen Maßnahmen zum Beheben der einzelnen Probleme aus und erstellen Sie das Image neu. Einige Probleme in der Dockerfile können mit dem im Abschnitt [Probleme in Images beheben](#va_report) angegebenen Code behoben werden. 

Wenn Sicherheitslücken vorliegen und nicht behoben werden, können sich diese Probleme auf die Verwendung des Images für einen Container auswirken. Sie können ein Image, das Sicherheits- und Konfigurationsproblem aufweist, weiterhin in einem Container verwenden. 

 




## Sicherheit von Docker-Images, die in einem Namensbereich gespeichert sind, über die Befehlszeilenschnittstelle überprüfen 
{: #va_registry_cli}

Sie können die Sicherheit von Docker-Images, die in Ihren Namensbereichen in {{site.data.keyword.registrylong_notm}} gespeichert sind, über die Befehlszeilenschnittstelle überprüfen, um Informationen zu potenziellen Sicherheitslücken zu erhalten.
{:shortdesc}

Wenn Sie ein Image zur Registry hinzufügen, wird das Image automatisch von Vulnerability Advisor auf Sicherheitsprobleme und potenzielle Sicherheitslücken überprüft. Bei Containern, die aus gefährdeten Images bereitgestellt werden, besteht das Risiko, dass sie ein Ziel unbefugter Zugriffe und infiltriert werden. Images werden nur dann überprüft, wenn sie auf einem Betriebssystem basieren, das von Vulnerability Advisor unterstützt wird.

Werden Sicherheitsprobleme entdeckt, werden entsprechende Anweisungen zur Verfügung gestellt, um das Beheben der gemeldeten Sicherheitslücke zu erleichtern. 

Führen Sie die folgenden Schritte aus, um den Sicherheitslückenstatus von Images in Ihrem {{site.data.keyword.Bluemix_notm}}-Konto zu überprüfen.

1.  Rufen Sie eine Liste aller Images in Ihrem {{site.data.keyword.Bluemix_notm}}-Konto auf. Der folgende Befehl gibt eine Liste aller Images ungeachtet des Namensbereichs zurück, in dem sie gespeichert sind.

    ```
    bx cr image-list
    ```
    {: pre}

2.  Überprüfen Sie den Status in der Spalte **SICHERHEITSLÜCKE - STATUS**. Einer der folgenden Statuswerte wird angezeigt:
    -   `OK` Dieser Status bedeutet, dass keine Sicherheitsprobleme festgestellt wurden. 
    -   `Gefährdet` Dieser Status bedeutet, dass ein mögliches Sicherheitsproblem oder eine potenzielle Sicherheitslücke festgestellt wurde. 
    -   `Unbekannt` Dieser Status wird beim Überprüfen des Images angezeigt, bis der auf Sicherheitslücken bezogene Status endgültig bestimmt werden kann. 
    -   `Nicht unterstütztes Betriebssystem` Dieser Status wird angezeigt, wenn das Überprüfen des Images durch Vulnerability Advisor nicht unterstützt wird. 
4.  Weitere Informationen zum Status finden Sie im Vulnerability Advisor-Bericht. 

    ```
    bx cr va registry.<Region>/<eigener Namensbereich>/<eigenes Image>:<Tag>
    ```
    {: pre}

    Die Ausgabe der Befehlszeilenschnittstelle enthält eine Liste der Pakete mit potenziellen Sicherheitslücken, eine Beschreibung der festgestellten Sicherheitslücke sowie einen Link zu Anweisungen, mit denen Sie diese Lücke schließen können.


## Häufig auftretende Probleme in Images beheben 
{: #va_report}

Beispielkorrekturen für häufig auftretende Probleme, die von Vulnerability Advisor gemeldet werden.
{:shortdesc}

Vulnerability Advisor stellt Fehlerbehebungsmaßnahmen für die gemeldeten Sicherheits- oder Konfigurationsprobleme bereit. Einige der gemeldeten Probleme können durch die Aktualisierung der Dockerfile behoben werden. Mithilfe der folgenden Codebeispiele können Sie die Lösung in der Dockerfile implementieren und so häufig auftretende Probleme schneller beheben.

### Maximale Gültigkeitsdauer des Kennworts, Kennwortmindestgültigkeit in Tagen und minimale Kennwortlänge
{: #va_password}

**Problem**: Sie empfangen einen der folgenden Fehler oder alle angegebenen Fehler. 

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

### SSH-Sicherheitslücke 
{: #ssh}

**Problem**: Es wird der folgende Fehler zurückgegeben. 

```
Der SSH-Server darf nicht installiert sein.
```
{: screen}

**Korrektur**: Verwenden Sie für den Zugriff auf den Container den Befehl `docker attach` oder `docker exec` anstelle von SSH. Stellen Sie sicher, dass Ihre Dockerfile keine Schritte für die Installation eines SSH-Servers enthält.

