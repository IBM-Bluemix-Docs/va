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

# Gestione della sicurezza delle immagini con il Controllo vulnerabilità 
{: #va_index}

Il Controllo vulnerabilità verifica lo stato di sicurezza delle immagini del contenitore prima della distribuzione.
{:shortdesc}


## Informazioni sul Controllo vulnerabilità 
{: #about}

Il Controllo vulnerabilità fornisce la gestione sicura per {{site.data.keyword.containerlong}}. Il Controllo vulnerabilità genera un report di stato, suggerisce correzioni e procedure consigliate e la gestione per limitare le immagini non sicure dall'essere eseguite. La correzione dei problemi di configurazione e sicurezza notificati dal Controllo vulnerabilità può aiutarti a proteggere la tua infrastruttura {{site.data.keyword.cloud_notm}}.
{:shortdesc}

Il Controllo vulnerabilità include le seguenti funzioni: 

-   Scansione delle immagini per le vulnerabilità 
-   Fornisce un report di valutazione basato su standard di sicurezza, come ISO 27002, così come le procedure specifiche per {{site.data.keyword.containerlong_notm}}
-   Individua i malware basati sui file
-   Fornisce raccomandazioni per proteggere i file di configurazione per una serie secondaria di tipi di applicazione 
-   Fornisce istruzioni su come risolvere una vulnerabilità riportata o un problema di configurazione nei propri report 

<dl>
  <dt><strong>Pacchetti vulnerabili</strong></dt>
  <dd>Il Controllo vulnerabilità verifica i pacchetti vulnerabili nelle immagini basate sui sistemi operativi supportati e fornisce un link per tutte le informazioni particolari sulla sicurezza rilevanti sulla vulnerabilità.  Il Controllo vulnerabilità aggiorna il proprio elenco interno con queste informazioni particolari di sicurezza giornalmente. Le immagini di base supportate sono descritte nella seguente tabella. </dd>
</dl>

  |Immagine di base Docker |Origine delle informazioni particolari di sicurezza |
  |-----------------|--------------------------|
  |Alpine|[Git - Alpine Linux ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://git.alpinelinux.org/) e [CIRCL CVE Search ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://cve.circl.lu/)|
  |CentOS| [CentOS announce archives ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://lists.centos.org/pipermail/centos-announce/) e [CentOS CR announce archives ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://lists.centos.org/pipermail/centos-cr-announce/)|
  |Debian|[Debian security announcements ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://lists.debian.org/debian-security-announce/)|
  |Red Hat Enterprise Linux (RHEL)|[Red Hat Product Errata ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://access.redhat.com/errata/#/)|
  |Ubuntu|[Ubuntu Security Notices ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ubuntu.com/usn/)|
  {: caption="Tabella 1. Immagini di base Docker in cui il Controllo vulnerabilità verifica i pacchetti vulnerabili " caption-side="top"}










## Riesame della sicurezza delle immagini Docker memorizzate in uno spazio dei nomi in {{site.data.keyword.registrylong_notm}} utilizzando la CLI 
{: #va_registry_cli}

Puoi esaminare la sicurezza delle immagini Docker memorizzate in uno spazio dei nomi per trovare informazioni sulle potenziali vulnerabilità.
{:shortdesc}

Quando aggiungi un'immagine a {{site.data.keyword.registrylong_notm}}, viene automaticamente scansionata dal controllo vulnerabilità per individuare problemi di sicurezza e vulnerabilità potenziali. I contenitori che vengono distribuiti da immagini vulnerabili potrebbero essere attaccati e compromessi. Le immagini vengono scansionate solo se si basano su un sistema operativo supportato dal controllo vulnerabilità. 

Il controllo vulnerabilità controlla le seguenti vulnerabilità. Se si riscontrano dei problemi di sicurezza, vengono fornite le istruzioni per aiutare a correggere le vulnerabilità segnalate. 

Per controllare lo stato di vulnerabilità delle immagini nel tuo account, {{site.data.keyword.Bluemix_notm}} completa i seguenti passi.

1.  Elenco di tutte le immagini nel tuo account {{site.data.keyword.Bluemix_notm}}. Il seguente comando restituisce un elenco di tutte le immagini indipendentemente dallo spazio dei nomi in cui sono memorizzate. 

    ```
    bx cr image-list
    ```
    {: pre}

2.  Controlla lo stato nella colonna STATO DI VULNERABILITÀ. Viene visualizzato uno dei seguenti stati: 
    -   OK. Questo stato significa che non sono stati trovati problemi di sicurezza. 
    -   Vulnerabile. Questo stato significa che è stato trovato un potenziale problema di sicurezza o vulnerabilità. 
    -   Sconosciuto. Questo stato viene visualizzato durante la scansione dell'immagine finché non viene determinato lo stato finale della vulnerabilità. 
    -   SO non supportato. Questo stato viene visualizzato se la scansione dell'immagine non è supportata dal Controllo vulnerabilità. 
4.  Per ulteriori informazioni sullo stato, esamina il report del Controllo vulnerabilità. 

    ```
    bx cr va registry.<region>/<my_namespace>/<my_image>:<tag>
    ```
    {: pre}

    Nell'output della CLI, puoi trovare l'elenco dei pacchetti vulnerabili, una descrizione della vulnerabilità riscontrata e un link alle istruzioni su come correggerla. 


## Risoluzione dei problemi nelle immagini  
{: #va_report}

Esempio di correzioni di problemi comuni riportati dal Controllo vulnerabilità. {:shortdesc}

Il Controllo vulnerabilità fornisce le azioni correttive con i problemi di sicurezza o configurazione segnalati. Alcuni dei problemi segnalati possono essere corretti aggiornando il tuo Dockerfile. Per aiutarti a risolvere i problemi comuni più velocemente, utilizza i seguenti esempi di codice per implementare la soluzione nel tuo Dockerfile. 

### Validità massima, giorni minimi e lunghezza minima della password. 
{: #va_password}

Problema: hai ricevuto uno o tutti i seguenti errori: 

```
Maximum password age must be set to 90 days.
```
{: screen}

```
Minimum password length must be 8.
```
{: screen}

```
Minimum days that must elapse between user-initiated password changes should be 1.
```
{: screen}

Correzione: Imposta la conformità della password aggiungendo il seguente codice al tuo Dockerfile. 

```
RUN \
    sed -i 's/^PASS_MAX_DAYS.*/PASS_MAX_DAYS    90/' /etc/login.defs && \
    sed -i 's/^PASS_MIN_DAYS.*/PASS_MIN_DAYS    1/' /etc/login.defs && \
    sed -i 's/sha512/sha512 minlen=8/' /etc/pam.d/common-password
```

### Vulnerabilità SSH 
{: #ssh}

Problema: viene restituito il seguente errore: 

```
SSH server should not be installed.
```
{: screen}

Correzione: invece di utilizzare SSH, utilizza `docker attach` o `docker exec` per accedere al tuo contenitore. Assicurarsi che il tuo Dockerfile non contenga alcun passo per l'installazione di un server SSH. 

