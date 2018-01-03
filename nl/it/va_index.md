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
-   Fornisce un report di valutazione basato su standard di sicurezza, come ISO 27002, [Center of Internet Security ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.cisecurity.org/) e le procedure di sicurezza specifiche per {{site.data.keyword.containerlong_notm}}
-   Individua i malware basati sui file
-   Fornisce raccomandazioni per proteggere i file di configurazione per una serie secondaria di tipi di applicazione
-   Fornisce istruzioni su come risolvere una vulnerabilità riportata o un problema di configurazione nei propri report
   

    


**Pacchetti vulnerabili**

Il Controllo vulnerabilità verifica i pacchetti vulnerabili nelle immagini basate sui sistemi operativi supportati e fornisce un link per tutte le informazioni particolari sulla sicurezza rilevanti sulla vulnerabilità. 

Vengono visualizzati i pacchetti con problemi di vulnerabilità noti.  Le vulnerabilità possibili vengono aggiornate dagli avvisi di sicurezza pubblicati giornalmente per i tipi di immagine Docker elencati nella seguente tabella. Di norma, per fare in modo che un pacchetto vulnerabile superi con esito positivo la scansione, è necessaria una sua versione più recente che includa una correzione per la vulnerabilità in questione. Lo stesso pacchetto potrebbe elencare più vulnerabilità e in questo caso, un solo aggiornamento del pacchetto potrebbe risolvere più vulnerabilità. Le informazioni nella colonna **AZIONI CORRETTIVE** descrivono come migliorare la sicurezza.

Le immagini di base supportate sono descritte nella seguente tabella.

  |Immagine di base Docker|Origine delle informazioni particolari di sicurezza|
  |-----------------|--------------------------|
  |Alpine|[Git - Alpine Linux ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://git.alpinelinux.org/) e [CIRCL CVE Search ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://cve.circl.lu/)|
  |CentOS| [CentOS announce archives ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://lists.centos.org/pipermail/centos-announce/) e [CentOS CR announce archives ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://lists.centos.org/pipermail/centos-cr-announce/)|
  |Debian|[Debian security announcements ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://lists.debian.org/debian-security-announce/)|
  |Red Hat Enterprise Linux (RHEL)|[Red Hat Product Errata ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://access.redhat.com/errata/#/)|
  |Ubuntu|[Ubuntu Security Notices ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ubuntu.com/usn/)|
  {: caption="Tabella 1. Immagini di base Docker in cui il Controllo vulnerabilità verifica i pacchetti vulnerabili" caption-side="top"}
  



**Configurazioni dell'applicazione **

Elenca le configurazioni di sicurezza per l'immagine che non sono sicure. Le informazioni nella colonna **AZIONI CORRETTIVE** descrivono come migliorare la sicurezza 




**Report Sicurezza**

Nel dashboard del registro, la colonna **REPORT DI SICUREZZA** visualizza lo stato dei tuoi repository.

Il Controllo vulnerabilità controlla le vulnerabilità note nelle impostazioni di configurazione per
i seguenti tipi di applicazione:
-   MySQL
-   NGINX
-   Apache

Il report identifica le buone procedure di sicurezza cloud per le tue immagini. Puoi accedere a un elenco completo dei problemi di configurazione e sicurezza verificati dal Controllo vulnerabilità.

Il dashboard del Controllo vulnerabilità fornisce una panoramica e una valutazione della sicurezza di un'immagine.  

Per ulteriori informazioni sul dashboard del Controllo vulnerabilità, consulta [Controllo di un report di vulnerabilità ](#va_reviewing).





## Controllo di un report di vulnerabilità dell'immagine 
{: #va_reviewing}

Prima di distribuire un'immagine, puoi controllarne il report del Controllo vulnerabilità, che ti fornisce i dettagli su tutti i pacchetti vulnerabili e sulle impostazioni dell'applicazione non sicure.
{:shortdesc}

Le immagini del contenitore sono fornite da IBM, terze parti o possono essere aggiunte dalla tua organizzazione.

Riesamina i problemi di configurazione e sicurezza dell'immagine potenziali completando la seguente procedura: 

1.  Accedi a {{site.data.keyword.Bluemix_notm}}. Devi aver eseguito l'accesso al Controllo vulnerabilità nell'interfaccia utente grafica 
2.  Fai clic su **Catalogo**.
3.  In **Infrastruttura**, fai clic su **Contenitori**. 
4.  Fai clic sul tile **Container Registry**.
5.  Espandi **Controllo vulnerabilità** e fai clic su **Repository scansionati**. 
6.  Per visualizzare il report per l'immagine contrassegnata come `latest`, fai clic sulla riga di tale repository. Il report mostra il numero totale di problemi e se sono pacchetti vulnerabili o problemi di configurazione. Se non esiste alcuna tag `latest` nel repository, viene utilizzata l'immagine più recente.
7.  Per visualizzare le informazioni su ogni pacchetto vulnerabile, nella tabella **Pacchetti vulnerabili trovati**, fai clic sul link nella colonna **VULNERABILITÀ** per aprire il report.
    1.  Per visualizzare ulteriori informazioni, espandi il riepilogo.
    2.  Per visualizzare le informazioni particolari del distributore del sistema operativo, fai clic sul link nella colonna **AVVISO UFFICIALE**.
8.  Per visualizzare le informazioni su ogni problema di configurazione, nella colonna **Problemi di configurazione trovati**, fai clic sulla riga del problema. 
9.  Esegui l'azione correttiva per ogni problema mostrato nel report e ricrea l'immagine. Alcuni problemi nel Dockerfile possono essere risolti utilizzando il codice fornito in [Risoluzione dei problemi nelle immagini](#va_report).

Se sono presenti delle vulnerabilità e non le risolvi, questi problemi possono influire sull'utilizzo dell'immagine del contenitore. Puoi continuare ad utilizzare un'immagine con problemi di configurazione e sicurezza in un contenitore.

 




## Riesame della sicurezza delle immagini Docker memorizzate in uno spazio dei nomi utilizzando la CLI 
{: #va_registry_cli}

Puoi esaminare la sicurezza delle immagini Docker memorizzate nei tuoi spazi dei nomi in {{site.data.keyword.registrylong_notm}} utilizzando la CLI per trovare informazioni sulle potenziali vulnerabilità.
{:shortdesc}

Quando aggiungi un'immagine al registro, viene automaticamente scansionata dal controllo vulnerabilità per individuare problemi di sicurezza e vulnerabilità potenziali. I contenitori che vengono distribuiti da immagini vulnerabili potrebbero essere attaccati e compromessi. Le immagini vengono scansionate solo se si basano su un sistema operativo supportato dal controllo vulnerabilità.

Se si riscontrano dei problemi di sicurezza, vengono fornite le istruzioni per aiutare a correggere le vulnerabilità segnalate.

Per controllare lo stato di vulnerabilità delle immagini nel tuo account, {{site.data.keyword.Bluemix_notm}} completa i seguenti passi.

1.  Elenco di tutte le immagini nel tuo account {{site.data.keyword.Bluemix_notm}}. Il seguente comando restituisce un elenco di tutte le immagini indipendentemente dallo spazio dei nomi in cui sono memorizzate.

    ```
    bx cr image-list
    ```
    {: pre}

2.  Controlla lo stato nella colonna **STATO DI VULNERABILITÀ**. Viene visualizzato uno dei seguenti stati:
    -   `OK` Questo stato significa che non sono stati trovati problemi di sicurezza.
    -   `Vulnerabile` Questo stato significa che è stato trovato un potenziale problema di sicurezza o vulnerabilità.
    -   `Sconosciuto` Questo stato viene visualizzato durante la scansione dell'immagine finché non viene determinato lo stato finale della vulnerabilità.
    -   `SO non supportato` Questo stato viene visualizzato se la scansione dell'immagine non è supportata dal Controllo vulnerabilità.
4.  Per ulteriori informazioni sullo stato, esamina il report del Controllo vulnerabilità.

    ```
    bx cr va registry.<region>/<my_namespace>/<my_image>:<tag>
    ```
    {: pre}

    Nell'output della CLI, puoi trovare l'elenco dei pacchetti vulnerabili, una descrizione della vulnerabilità riscontrata e un link alle istruzioni su come correggerla.


## Risoluzione dei problemi comuni nelle immagini  
{: #va_report}

Esempio di correzioni di problemi comuni riportati dal Controllo vulnerabilità.
{:shortdesc}

Il Controllo vulnerabilità fornisce le azioni correttive con i problemi di sicurezza o configurazione segnalati. Alcuni dei problemi segnalati possono essere corretti aggiornando il tuo Dockerfile. Per aiutarti a risolvere i problemi comuni più velocemente, utilizza i seguenti esempi di codice per implementare la soluzione nel tuo Dockerfile.

### Validità massima, giorni minimi e lunghezza minima della password.
{: #va_password}

**Problema**: hai ricevuto uno o tutti i seguenti errori:

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

**Correzione**: Imposta la conformità della password aggiungendo il seguente codice al tuo Dockerfile.

```
RUN \
    sed -i 's/^PASS_MAX_DAYS.*/PASS_MAX_DAYS    90/' /etc/login.defs && \
    sed -i 's/^PASS_MIN_DAYS.*/PASS_MIN_DAYS    1/' /etc/login.defs && \
    sed -i 's/sha512/sha512 minlen=8/' /etc/pam.d/common-password
```

### Vulnerabilità SSH 
{: #ssh}

**Problema**: viene restituito il seguente errore:

```
SSH server should not be installed.
```
{: screen}

**Correzione**: invece di utilizzare SSH, utilizza `docker attach` o `docker exec` per accedere al tuo contenitore. Assicurarsi che il tuo Dockerfile non contenga alcun passo per l'installazione di un server SSH.

