---

copyright:
  years: 2017, 2018
lastupdated: "2018-05-31"

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

Il Controllo vulnerabilità verifica lo stato di sicurezza delle immagini del contenitore fornite da IBM, da terze parti oppure aggiunte allo spazio dei nomi del registro della tua organizzazione.
{:shortdesc}


Quando aggiungi un'immagine a uno spazio dei nomi, il Controllo vulnerabilità ne esegue automaticamente la scansione per individuare problemi di sicurezza e potenziali vulnerabilità. Se si riscontrano dei problemi di sicurezza, vengono fornite le istruzioni per aiutare a correggere le vulnerabilità segnalate. Puoi ancora distribuire contenitori da immagini vulnerabili ma tieni presente che tali contenitori potrebbero essere attaccati o compromessi.


## Informazioni sul Controllo vulnerabilità
{: #about}

Il Controllo vulnerabilità fornisce la gestione sicura per {{site.data.keyword.containerlong}}. Il Controllo vulnerabilità genera un report di stato, suggerisce correzioni e procedure consigliate e la gestione per limitare le immagini non sicure dall'essere eseguite. La correzione dei problemi di configurazione e sicurezza notificati dal Controllo vulnerabilità può aiutarti a proteggere la tua infrastruttura {{site.data.keyword.cloud_notm}}.
{:shortdesc}

Il Controllo vulnerabilità include le seguenti funzioni:

-   Scansione delle immagini per le vulnerabilità
-   Fornisce un report di valutazione basato sulle prassi di sicurezza specifiche per {{site.data.keyword.containerlong_notm}}
-   Fornisce raccomandazioni per proteggere i file di configurazione per una serie secondaria di tipi di applicazione
-   Fornisce istruzioni su come correggere un [pacchetto vulnerabile](#packages) o un [problema di configurazione](#app_configurations) segnalato nei relativi report

Nel dashboard del registro, la colonna **REPORT DI SICUREZZA** visualizza lo stato dei tuoi repository. Il report identifica le buone procedure di sicurezza cloud per le tue immagini. 

Il dashboard del Controllo vulnerabilità fornisce una panoramica e una valutazione della sicurezza di un'immagine. Per ulteriori informazioni sul dashboard del Controllo vulnerabilità, consulta [Controllo di un report di vulnerabilità ](#va_reviewing).
	
	
**Protezione dei dati**

Per eseguire la scansione di immagini e contenitori nel tuo account per rilevare eventuali problemi di sicurezza, il Controllo vulnerabilità raccoglie, memorizza ed elabora le seguenti informazioni:
- campi di testo in formato libero, compresi ID, descrizioni e nomi immagine (registro, spazio dei nomi, nome repository e tag immagine)
- metadati Kubernetes, compresi i nomi delle risorse Kubernetes quali pod, serie di repliche e nomi distribuzione
- metadati relativi alle modalità file e alle date/ore di creazione dei file di configurazione
- contenuto dei file di configurazione di sistema e applicazioni in immagini e contenitori
- pacchetti e librerie installati (comprese le loro versioni)

Non inserire informazioni personali nei campi o nelle ubicazioni elaborati dal Controllo vulnerabilità, così come identificati nell'elenco precedente.

I risultati della scansione, aggregati a un livello di data center, vengono elaborati per produrre delle metriche anonimizzate per effettuare e migliorare il servizio.

I risultati della scansione vengono eliminati 30 giorni dopo essere stati generati.



## Tipi di vulnerabilità
{: #types}

### Pacchetti vulnerabili
{: #packages}

Il Controllo vulnerabilità verifica i pacchetti vulnerabili nelle immagini basate sui sistemi operativi supportati e fornisce un link per tutte le informazioni particolari sulla sicurezza rilevanti sulla vulnerabilità.
{:shortdesc}

I pacchetti con problemi di vulnerabilità noti vengono visualizzati nei risultati della scansione. Le vulnerabilità possibili vengono aggiornate dagli avvisi di sicurezza pubblicati giornalmente per i tipi di immagine Docker elencati nella seguente tabella. Di norma, per fare in modo che un pacchetto vulnerabile superi con esito positivo la scansione, è necessaria una sua versione più recente che includa una correzione per la vulnerabilità in questione. Lo stesso pacchetto potrebbe elencare più vulnerabilità e in questo caso, un solo aggiornamento del pacchetto potrebbe risolvere più vulnerabilità.


  |Immagine di base Docker|Origine delle informazioni particolari di sicurezza|
  |-----------------|--------------------------|
  |Alpine|[Git - Alpine Linux ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://git.alpinelinux.org/) e [CIRCL CVE Search ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://cve.circl.lu/)|
  |CentOS| [CentOS announce archives ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://lists.centos.org/pipermail/centos-announce/) e [CentOS CR announce archives ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://lists.centos.org/pipermail/centos-cr-announce/)|
  |Debian|[Debian security announcements ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://lists.debian.org/debian-security-announce/)|
  |Red Hat Enterprise Linux (RHEL)|[Red Hat Product Errata ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://access.redhat.com/errata/#/)|
  |Ubuntu|[Ubuntu Security Notices ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://usn.ubuntu.com/)|
  {: caption="Tabella 1. Immagini di base Docker supportate verificate dal Controllo vulnerabilità per rilevare eventuali pacchetti vulnerabili" caption-side="top"}




### Problemi di configurazione
{: #app_configurations}

I problemi di configurazione sono dei potenziali problemi di sicurezza correlati al modo in cui è configurata un'applicazione. Molti dei problemi segnalati possono essere corretti aggiornando il tuo Dockerfie.
{:shortdesc}

Le immagini vengono scansionate solo se si basano su un sistema operativo supportato dal controllo vulnerabilità. Il Controllo vulnerabilità controlla le impostazioni di configurazione per i seguenti tipi di applicazioni:
-   MySQL
-   NGINX
-   Apache



## Installazione dello scanner contenitori
{: #va_install_livescan}

Prima di iniziare:

1.  Accedi al client CLI {{site.data.keyword.Bluemix_notm}}. Se hai un account federato, usa `--sso`.
2.  [Indirizza la tua CLI `kubectl`](../../containers/cs_cli_install.html#cs_cli_configure) al cluster dove vuoi usare un grafico Helm.
3.  Crea un ID servizio e una chiave API per lo scanner contenitori e assegna ad esso un nome:
    1.  Crea un ID servizio eseguendo questo comando, sostituendo a `<scanner_serviceID>` un nome a tua scelta per l'ID servizio. Prendi nota del relativo **CRN**.
    
        ```
    	bx iam service-id-create <scanner_serviceID>
    	```
        {: codeblock}

    2.  Crea una chiave API di servizio, dove `<scanner_serviceID>` è l'ID servizio che hai creato nel passo precedente e sostituendo a `<scanner_APIkey_name>` un nome a tua scelta per la chiave API dello scanner. 
    
        ```
    	bx iam service-api-key-create <scanner_APIkey_name> <scanner_serviceID>
    	```
        {: codeblock}
	
	    Viene restituita la chiave API dello scanner.
	
	    Assicurati di conservare la tua chiave API dello scanner in modo sicuro perché non sarà possibile recuperarla successivamente.
	    {: tip}
	
    3.  Crea una politica di servizio che conceda il ruolo di `Writer` (Scrittore).
    		
        ```
    	bx iam service-policy-create <scanner_serviceID> --resource-type scaningress --service-name container-registry --roles Writer
    	```
        {: codeblock}

Per configurare il grafico Helm:

1.  [Configura Helm nel tuo cluster](../../containers/cs_integrations.html#helm). Se usi una politica RBAC per concedere l'accesso tiller Helm, assicurati che il ruolo di tiller abbia accesso a tutti gli spazi dei nomi in modo che lo scanner possa controllare tutti i contenitori in tutti gli spazi dei nomi.

2.  Aggiungi il repository di grafici IBM al tuo Helm, come ad esempio `ibm-incubator`.

    ```
    helm repo add ibm-incubator https://registry.bluemix.net/helm/ibm-incubator
    ```
    {: pre}

3.  Salva le impostazioni di configurazione predefinite per il grafico Helm dello scanner di contenitori in un file YAML locale. Includi il repository dei grafici, come ad esempio `ibm-incubator`, nel percorso del grafico Helm.

    ```
    helm inspect values ibm-incubator/ibmcloud-container-scanner > config.yaml
    ```
    {: pre}

4.  Modifica il file `config.yaml`.

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
    <caption>Informazioni sui componenti file YAML</caption>
    <thead>
    <th>Campo</th>
    <th>Valore</th>
    </thead>
    <tbody>
    <tr>
    <td><code>EmitURL</code></td>
    <td>Immetti l'URL dell'endpoint regionale del Controllo vulnerabilità. Per ottenere l'URL, esegui <code>bx cr info</code> e recupera l'indirizzo <strong>Registro contenitore</strong>. Sostituisci <code>registry</code> con <code>va</code>. Ad esempio, <code>https<span comment="make the link not a link">://va.</span>eu-gb.bluemix.net</code></td>
    </tr>
    <tr>
    <td><code>AccountID</code></td>
    <td>Sostituisci con l'ID account {{site.data.keyword.Bluemix_notm}} in cui si trova il tuo cluster. per ottenere l'ID account, esegui <code>bx account list</code>.</td>
    </tr>
    <tr>
    <td><code>ClusterID</code></td>
    <td>Sostituisci con il cluster Kubernetes in cui vuoi installare lo scanner contenitori. Per elencare gli ID cluster, esegui <code>bx cs clusters</code>.</td>
    </tr>
    <tr>
    <td><code>APIKey</code></td>
    <td>Sostituisci con la chiave API dello scanner che hai creato in precedenza.</td>
    </tr>
    </tbody></table>

5.  Installa il grafico Helm nel tuo cluster con il file `config.yaml` aggiornato. Le proprietà aggiornate sono memorizzate in una mappa di configurazione per il tuo grafico. Sostituisci a `<myscanner>` un nome a tua scelta per il tuo grafico Helm. Includi il repository dei grafici, come ad esempio `ibm-incubator`, nel percorso del grafico Helm.

    ```
    helm install -f config.yaml --name=<myscanner> ibm-incubator/ibmcloud-container-scanner
    ```
    {: pre}
    
    **Note**: lo scanner contenitori viene installato nello spazio dei nomi `kube-system` ma esegue la scansione dei contenitori da tutti gli spazi dei nomi.

6.  Controlla lo stato di distribuzione del grafico. Quando il grafico è pronto, il campo **STATO** vicino alla parte superiore dell'output ha un valore di `DEPLOYED`.

    ```
    helm status <myscanner>
    ```
    {: pre}

7.  Dopo che il grafico è stato distribuito, verifica che siano state utilizzate le impostazioni aggiornate nel file `config.yaml`.

    ```
    helm get values <myscanner>
    ```
    {: pre}


Lo scanner contenitori IBM è ora installato e l'agent livescan viene distribuito come una [serie di daemon (DaemonSet) ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) nel tuo cluster. Anche se viene distribuito allo spazio dei nomi `kube-system`, lo scanner esegue la scansione di tutti i contenitori assegnati ai pod in tutti i tuoi spazi dei nomi Kubernetes, come ad esempio `default`. 



## Riesame di un report di vulnerabilità
{: #va_reviewing}

Prima che tu distribuisca un'immagine, puoi riesaminare il relativo report del Controllo vulnerabilità per i dettagli relativi ad eventuali pacchetti vulnerabili e impostazioni delle applicazioni non sicure.
{:shortdesc}



1.  Accedi a {{site.data.keyword.Bluemix_notm}}.
2.  Fai clic su **Catalogo**.
3.  In **Infrastruttura**, fai clic su **Contenitori**.
4.  Fai clic sul tile **Container Registry**.
5.  Espandi **Controllo vulnerabilità** e fai clic su **Repository scansionati**.
6.  Per visualizzare il report per l'immagine contrassegnata come `latest`, fai clic sulla riga di tale repository. Il report mostra il numero totale di problemi e se sono pacchetti vulnerabili o problemi di configurazione. Se non esiste alcuna tag `latest` nel repository, viene utilizzata l'immagine più recente.
7.  Per visualizzare le informazioni su ogni pacchetto vulnerabile per l'immagine da te selezionata, nella tabella **Pacchetti vulnerabili trovati**, fai clic sul link nella colonna **VULNERABILITÀ** per aprire il report.
    1.  Per visualizzare ulteriori informazioni, espandi il riepilogo.
    2.  Se viene fornito un avviso del distributore del sistema operativo, fai clic sul link nella colonna **AVVISO UFFICIALE**.
8.  Per visualizzare le informazioni su ogni problema di configurazione, nella colonna **Problemi di configurazione trovati**, fai clic sulla riga del problema.
9.  Esegui l'azione correttiva per ogni problema mostrato nel report e ricrea l'immagine. Alcuni problemi nel Dockerfile possono essere risolti utilizzando il codice fornito in [Risoluzione dei problemi nelle immagini](#va_report).

Se esistono delle vulnerabilità e non le correggi, esse potrebbero avere delle ripercussioni sulla sicurezza dei contenitori creati con tale immagine. Puoi tuttavia continuare a usare un'immagine che ha problemi di sicurezza e configurazione in un contenitore.

 



## Riesame di un report di vulnerabilità utilizzando la CLI
{: #va_registry_cli}

Puoi riesaminare la sicurezza delle immagini Docker memorizzate nei tuoi spazi dei nomi in {{site.data.keyword.registrylong_notm}} utilizzando la CLI.
{:shortdesc}

1.  Elenca le immagini nel tuo account {{site.data.keyword.Bluemix_notm}}. Viene restituito un elenco di tutte le immagini, indipendentemente dallo spazio dei nomi in cui sono memorizzate.

    ```
    bx cr image-list
    ```
    {: pre}

2.  Controlla lo stato nella colonna **STATO SICUREZZA**.
    -   `No Issues`: non è stato rilevato alcun problema di sicurezza.
    -   `X Issues`: sono stati rilevati dei potenziali problemi di sicurezza o delle potenziali vulnerabilità.
    -   `Scanning`: è in corso la scansione dell'immagine e lo stato di vulnerabilità finale non è stato ancora determinato.
4.  Per visualizzare i dettagli per lo stato, riesaminare il report del Controllo vulnerabilità.

    ```
    bx cr va registry.<region>/<my_namespace>/<my_image>:<tag>
    ```
    {: pre}

    Nell'output della CLI, puoi visualizzare le seguenti informazioni sui problemi di sicurezza.
      - Prassi di sicurezza: una descrizione della vulnerabilità che è stata rilevata
      - Azione correttiva: i dettagli su come correggere la vulnerabilità


## Risoluzione dei problemi comuni nelle immagini
{: #va_report}

Riesamina le correzioni di esempio per i problemi comuni che potrebbero essere segnalati dal Controllo vulnerabilità. Alcuni problemi possono essere corretti aggiornando il tuo Dockerfile.
{:shortdesc}

### Validità massima, giorni minimi e lunghezza minima della password.
{: #va_password}

**Problema**: ricevi una o più delle seguenti vulnerabilità:

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
{: codeblock}

### Vulnerabilità SSH
{: #ssh}

**Problema**: viene restituita la seguente vulnerabilità:

```
SSH server should not be installed.
```
{: screen}

**Correzione**: invece di utilizzare SSH, utilizza `docker attach` o `docker exec` per accedere al tuo contenitore. Assicurarsi che il tuo Dockerfile non contenga alcun passo per l'installazione di un server SSH.
