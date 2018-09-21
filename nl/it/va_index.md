---

copyright:
  years: 2017, 2018
lastupdated: "2018-08-23"

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

Il Controllo vulnerabilità verifica lo stato di sicurezza delle immagini del contenitore fornite da {{site.data.keyword.IBM}}, da terze parti oppure aggiunte allo spazio dei nomi del registro della tua organizzazione. Se lo Scanner contenitori è installato in ciascun cluster, il Controllo vulnerabilità controlla anche lo stato dei contenitori in esecuzione.
{:shortdesc}

Quando aggiungi un'immagine a uno spazio dei nomi, il Controllo vulnerabilità ne esegue automaticamente la scansione per individuare problemi di sicurezza e potenziali vulnerabilità. Se si riscontrano dei problemi di sicurezza, vengono fornite le istruzioni per aiutare a correggere le vulnerabilità segnalate. 

Il Controllo vulnerabilità fornisce la gestione della sicurezza per {{site.data.keyword.registrylong_notm}}, generando un report sullo stato della sicurezza che include le correzioni e le prassi ottimali consigliate. 

Qualsiasi problema rilevato dal controllo vulnerabilità determina un verdetto che indica che non è consigliabile distribuire questa immagine. Se scegli di distribuire l'immagine, qualsiasi contenitore distribuito dall'immagine include dei problemi noti che potrebbero essere utilizzati per attaccarlo o comprometterlo in altro modo. Il verdetto viene modificato in base a tutte le esenzioni che hai specificato. Questo verdetto può essere utilizzato da Container Image Security Enforcement per evitare la distribuzione di immagini non sicure in {{site.data.keyword.containerlong_notm}}. 

La correzione dei problemi di configurazione e sicurezza notificati dal Controllo vulnerabilità può aiutarti a proteggere la tua infrastruttura {{site.data.keyword.cloud_notm}}.


## Informazioni sul Controllo vulnerabilità
{: #about}

Il Controllo vulnerabilità fornisce funzioni che ti aiutano a proteggere le tue immagini.

{:shortdesc}

 Sono disponibili le seguenti funzioni:

-   Scansione delle immagini per rilevare eventuali problemi
-   Scansione per problemi nei contenitori che sono in esecuzione se uno [Scanner contenitori](#va_install_container_scanner) è installato in ogni cluster
-   Fornisce un report di valutazione basato sulle prassi di sicurezza specifiche per {{site.data.keyword.containerlong_notm}}
-   Fornisce raccomandazioni per proteggere i file di configurazione per una serie secondaria di tipi di applicazione
-   Fornisce istruzioni su come correggere un [pacchetto vulnerabile](#packages) o un [problema di configurazione](#app_configurations) segnalato nei relativi report
-   Fornisce i verdetti a [Container Image Security Enforcement](/docs/services/Registry/registry_security_enforce.html#security_enforce)
-   Applica esenzioni ai report a un livello di account, spazio dei nomi, repository o tag per indicare quando dei problemi contrassegnati non si applicano al tuo caso d'uso.
-   Fornisce dei link ai contenitori associati nella vista **Tag** della GUI (graphical user interface) {{site.data.keyword.registrylong_notm}}. Puoi elencare i contenitori che sono in esecuzione e che stanno utilizzando tale immagine in un cluster su cui è installato lo Scanner contenitori.


Nel dashboard Registro, la colonna **Stato politica** visualizza lo stato dei tuoi repository. Il report collegato identifica le procedure consigliate per la sicurezza cloud per le tue immagini. 

Il dashboard Controllo vulnerabilità fornisce una panoramica e una valutazione della sicurezza per un'immagine e, se lo Scanner contenitori è installato, dei link ai contenitori in esecuzione. Se desideri ulteriori informazioni sul dashboard Controllo vulnerabilità, consulta [Riesame di un report di vulnerabilità](#va_reviewing).
	
	
**Protezione dei dati**

Per eseguire la scansione di immagini e contenitori nel tuo account per rilevare eventuali problemi di sicurezza, il Controllo vulnerabilità raccoglie, memorizza ed elabora le seguenti informazioni:
- Campi in formato libero, compresi ID, descrizioni e nomi immagine (registro, spazio dei nomi, nome repository e tag immagine)
- Metadati Kubernetes, compresi i nomi delle risorse Kubernetes quali pod, serie di repliche e nomi distribuzione
- Metadati relativi alle modalità file e alle date/ore di creazione dei file di configurazione
- Il contenuto dei file di configurazione di sistema e applicazioni in immagini e contenitori
- Pacchetti e librerie installati (comprese le loro versioni)

Non inserire informazioni personali nei campi o nelle ubicazioni elaborati dal Controllo vulnerabilità, così come identificati nell'elenco precedente.

I risultati della scansione, aggregati a un livello di data center, vengono elaborati per produrre delle metriche anonimizzate per effettuare e migliorare il servizio.

I risultati della scansione vengono eliminati 30 giorni dopo essere stati generati.


## Tipi di vulnerabilità
{: #types}


### Pacchetti vulnerabili
{: #packages}

Il Controllo vulnerabilità verifica i pacchetti vulnerabili nelle immagini che stanno utilizzando i sistemi operativi supportati e fornisce un link per tutte le informazioni particolari sulla sicurezza rilevanti sulla vulnerabilità.
{:shortdesc}

I pacchetti che contengono problemi di vulnerabilità noti vengono visualizzati nei risultati della scansione. Le vulnerabilità possibili vengono aggiornate dagli avvisi di sicurezza pubblicati giornalmente per i tipi di immagine Docker elencati nella seguente tabella. Di norma, per fare in modo che un pacchetto vulnerabile superi con esito positivo la scansione, è necessaria una sua versione più recente che includa una correzione per la vulnerabilità in questione. Lo stesso pacchetto può elencare più vulnerabilità e in questo caso, un solo upgrade del pacchetto può risolvere più vulnerabilità.


  |Immagine di base Docker|Origine delle informazioni particolari di sicurezza|
  |-----------------|--------------------------|
  |Alpine|[Git - Alpine Linux ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://git.alpinelinux.org/) e [CIRCL CVE Search ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://cve.circl.lu/)|
  |CentOS| [CentOS announce archives ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://lists.centos.org/pipermail/centos-announce/) e [CentOS CR announce archives ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://lists.centos.org/pipermail/centos-cr-announce/)|
  |Debian|[Debian security announcements ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://lists.debian.org/debian-security-announce/)|
  |RHEL (Red Hat Enterprise Linux)|[Red Hat Product Errata ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://access.redhat.com/errata/#/)|
  |Ubuntu|[Ubuntu Security Notices ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://usn.ubuntu.com/)|
  {: caption="Tabella 1. Immagini di base Docker supportate verificate dal Controllo vulnerabilità per rilevare eventuali pacchetti vulnerabili" caption-side="top"}


### Problemi di configurazione
{: #app_configurations}

I problemi di configurazione sono dei potenziali problemi di sicurezza correlati al modo in cui è configurata un'applicazione. Molti dei problemi segnalati possono essere corretti aggiornando il tuo Dockerfie.
{:shortdesc}

Le immagini vengono scansionate solo se stanno utilizzando un sistema operativo supportato dal controllo vulnerabilità. Il Controllo vulnerabilità controlla le impostazioni di configurazione per i seguenti tipi di applicazioni:
-   MySQL
-   NGINX
-   Apache


## Installazione dello Scanner contenitori
{: #va_install_container_scanner}

**Prima di iniziare**

1.  Accedi al client CLI {{site.data.keyword.Bluemix_notm}}. Se hai un account federato, usa `--sso`.
2.  [Indirizza la tua CLI `kubectl`](/docs/containers/cs_cli_install.html#cs_cli_configure) al cluster dove vuoi usare un grafico Helm.
3.  Crea un ID servizio e una chiave API per lo Scanner contenitori e assegna ad esso un nome:
    1.  Per creare un ID servizio, immetti il seguente comando, dove `<scanner_serviceID>` è il nome di tua scelta per l'ID servizio. Prendi nota del relativo **CRN**.
    
        ```
    	ibmcloud iam service-id-create <scanner_serviceID>
    	```
        {: codeblock}

    2.  Crea una chiave API di servizio, dove `<scanner_serviceID>` è l'ID servizio che hai creato nel passo precedente e `<scanner_APIkey_name>` è un nome di tua scelta per la chiave API dello scanner. 
    
        ```
    	ibmcloud iam service-api-key-create <scanner_APIkey_name> <scanner_serviceID>
    	```
        {: codeblock}
	
	Viene restituita la chiave API dello scanner.
	
	Assicurati di conservare la tua chiave API dello scanner in modo sicuro perché non sarà possibile recuperarla successivamente.
	{: tip}
	
    3.  Crea una politica di servizio che conceda il ruolo di `Writer` (Scrittore).
    		
        ```
    	ibmcloud iam service-policy-create <scanner_serviceID> --resource-type scaningress --service-name container-registry --roles Writer
    	```
        {: codeblock}

Per configurare il grafico Helm, completa la seguente procedura:

1.  [Configura Helm nel tuo cluster](/docs/containers/cs_integrations.html#helm). Se usi una politica RBAC per concedere l'accesso tiller Helm, assicurati che il ruolo di tiller abbia accesso a tutti gli spazi dei nomi. Fornendo l'accesso al ruolo di tiller ti assicuri che lo Scanner contenitori possa controllare tutti i contenitori in tutti gli spazi dei nomi.

2.  Aggiungi il repository di grafici IBM al tuo Helm, come ad esempio `ibm-incubator`.

    ```
    helm repo add ibm-incubator https://registry.bluemix.net/helm/ibm-incubator
    ```
    {: pre}

3.  Salva le impostazioni di configurazione predefinite per il grafico Helm dello Scanner contenitori in un file YAML locale. Includi il repository dei grafici, come ad esempio `ibm-incubator`, nel percorso del grafico Helm.

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
    <td>Immetti l'URL dell'endpoint regionale del Controllo vulnerabilità. Per ottenere l'URL, esegui <code>ibmcloud cr info</code> e recupera l'indirizzo <strong>Registro contenitore</strong>. Sostituisci <code>registry</code> con <code>va</code>. Ad esempio, <code>https<span comment="make the link not a link">://va.</span>eu-gb.bluemix.net</code></td>
    </tr>
    <tr>
    <td><code>AccountID</code></td>
    <td>Sostituisci <code>AccountID</code> con l'ID account {{site.data.keyword.Bluemix_notm}} in cui si trova il tuo cluster. Per ottenere l'ID account, esegui <code>ibmcloud account list</code>.</td>
    </tr>
    <tr>
    <td><code>ClusterID</code></td>
    <td>Sostituisci <code>ClusterID</code> con il cluster Kubernetes in cui vuoi installare lo Scanner contenitori. Per elencare gli ID cluster, esegui <code>ibmcloud ks clusters</code>. <br> **Suggerimento**: utilizza l'ID del cluster, non il nome.
    </td>
    </tr>
    <tr>
    <td><code>APIKey</code></td>
    <td>Sostituisci <code>APIKey</code> con la chiave API dello scanner che hai creato in precedenza.</td>
    </tr>
    </tbody></table>

5.  Installa il grafico Helm nel tuo cluster con il file `config.yaml` aggiornato. Le proprietà aggiornate sono memorizzate in una mappa di configurazione per il tuo grafico. Sostituisci a `<myscanner>` un nome a tua scelta per il tuo grafico Helm. Includi il repository dei grafici, come ad esempio `ibm-incubator`, nel percorso del grafico Helm.

    ```
    helm install -f config.yaml --name=<myscanner> ibm-incubator/ibmcloud-container-scanner
    ```
    {: pre}
    
    Lo Scanner contenitori viene installato nello spazio dei nomi `kube-system` ma esegue la scansione dei contenitori da tutti gli spazi dei nomi.
    {:tip}

6.  Controlla lo stato di distribuzione del grafico. Quando il grafico è pronto, il campo **STATO** ha un valore di `DEPLOYED`.

    ```
    helm status <myscanner>
    ```
    {: pre}

7.  Dopo che il grafico è stato distribuito, verifica che siano state utilizzate le impostazioni aggiornate nel file `config.yaml`.

    ```
    helm get values <myscanner>
    ```
    {: pre}


Lo Scanner contenitori è ora installato e l'agent viene distribuito come una [serie di daemon (DaemonSet) ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) nel tuo cluster. Anche se viene distribuito allo spazio dei nomi `kube-system`, lo Scanner contenitori esegue la scansione di tutti i contenitori assegnati ai pod in tutti i tuoi spazi dei nomi Kubernetes, come ad esempio `default`.


## Esecuzione dello Scanner contenitori da dietro un firewall
{: #va_firewall}

Se il tuo firewall blocca le connessioni in uscita, devi configurare il tuo firewall per consentire ai nodi di lavoro di accedere allo Scanner contenitori sulla porta TCP <code>443</code> sugli indirizzi IP nella seguente tabella.
{:shortdesc}

 

<p>
  <table summary=" Le righe devono essere lette da sinistra a destra, con la posizione del server nella prima colonna e gli indirizzi IP corrispondenti nella seconda colonna.">
  <caption>Indirizzi IP da aprire per il traffico in uscita</caption>
      <thead>
      <th>Regione</th>
      <th>Indirizzo IP</th>
      </thead>
    <tbody>
      <tr>
         <td>Asia Pacifico Sud</td>
         <td><code>168.1.40.158</code><br><code>130.198.65.182</code></td>
      </tr>
      <tr>
         <td>Europa Centrale</td>
         <td><code>159.8.220.182</code><br><code>158.177.74.102</code></td>
        </tr>
      <tr>
        <td>Regno Unito Sud</td>
        <td><code>158.175.71.134</code><br><code>5.10.111.190</code></td>
      </tr>
      <tr>
        <td>Stati Uniti Est</td>
         <td><code>169.60.73.158</code><br><code>169.61.84.102</code></td>
      </tr>
      <tr>
        <td>Stati Uniti Sud</td>
        <td><code>169.47.103.118</code><br><code>169.48.165.6</code></td>
      </tr>
      </tbody>
    </table>
</p>


## Impostazione delle politiche di esenzione organizzative
{: #va_managing_policy}

Se vuoi gestire la sicurezza di un'organizzazione {{site.data.keyword.Bluemix_notm}}, puoi utilizzare la tua impostazione della politica per determinare se un problema è esente o meno. Puoi scegliere di utilizzare Container Image Security Enforcement per garantire che la distribuzione sia consentita solo da immagini che non contengono alcun problema di sicurezza, una volta ignorati quelli esentati dalla tua politica.
{:shortdesc}

Puoi distribuire i contenitori da qualsiasi immagine indipendentemente dallo stato della sicurezza, a meno che nel tuo cluster non sia distribuito Container Image Security Enforcement. Per informazioni su come distribuire Container Image Security Enforcement, vedi [Installazione di Security Enforcement](/docs/services/Registry/registry_security_enforce.html#security_enforce).

Quando utilizzi Container Image Security Enforcement, qualsiasi problema di sicurezza rilevato dal Controllo vulnerabilità impedisce la distribuzione di un contenitore dall'immagine. Per consentire la distribuzione di un'immagine con dei problemi rilevati, è necessario aggiungere delle esenzioni alla tua politica.

### Impostazione delle politiche di esenzione organizzative utilizzando la GUI
{: #va_managing_policy_gui}

Se vuoi impostare delle esenzioni per la politica utilizzando la GUI, completa la seguente procedura:

1.  Accedi a {{site.data.keyword.Bluemix_notm}}. Devi aver eseguito l'accesso per visualizzare il Controllo vulnerabilità nella GUI.
2.  Fai clic sull'icona **Menu** e quindi fai clic su **Containers**.
3.  In **Controllo vulnerabilità**, fai clic su **Policy Settings**.
4.  Fai clic su **Create Exemption**.
5.  Seleziona il tipo di problema.
6.  Immetti l'ID problema. 

    Puoi trovare queste informazioni nel tuo [report di vulnerabilità](#va_reviewing). La colonna **Vulnerability ID** contiene l'ID da utilizzare per i problemi di avvisi di sicurezza o CVE; la colonna **Configuration Issue ID** contiene l'ID da utilizzare per i problemi di configurazione.
    {: tip}


7.  Seleziona lo spazio dei nomi del registro, il repository e la tag a cui vuoi applicare l'esenzione.
8.  Fai clic su **Save**.

Puoi anche modificare e rimuovere le esenzioni passando il puntatore del mouse sulla riga pertinente e facendo clic sull'icona **apri e chiudi elenco delle opzioni**.

### Impostazione delle politiche di esenzione organizzative utilizzando la CLI
{: #va_managing_policy_cli}

Se vuoi impostare delle esenzioni per la politica utilizzando la CLI, puoi eseguire questi comandi:

-  Per creare un'esenzione per un problema di sicurezza, esegui il comando [`ibmcloud cr exemption-add`](/docs/services/Registry/registry_cli.html#bx_cr_exemption_add).
-  Per elencare le eccezioni per i problemi di sicurezza, esegui il comando [`ibmcloud cr exemption-list`](/docs/services/Registry/registry_cli.html#bx_cr_exemption_list) .
-  Per elencare i tipi di problemi di sicurezza che puoi esentare, esegui il comando [`ibmcloud cr exemption-types`](/docs/services/Registry/registry_cli.html#bx_cr_exemption_types).
-  Per eliminare un'esenzione per un problema di sicurezza, esegui il comando [`ibmcloud cr exemption-rm`](/docs/services/Registry/registry_cli.html#bx_cr_exemption_rm).

Per ulteriori informazioni sui comandi, puoi utilizzare l'indicatore `-- help` quando esegui il comando.


## Riesame di un report di vulnerabilità
{: #va_reviewing}

Prima che tu distribuisca un'immagine, puoi riesaminare il relativo report del Controllo vulnerabilità per i dettagli relativi ad eventuali pacchetti vulnerabili e impostazioni delle applicazioni o del contenitore non sicure. Puoi inoltre controllare se l'immagine è conforme alle politiche organizzative.
{:shortdesc}

Se non te ne occupi, i problemi rilevati potrebbero avere delle ripercussioni sulla sicurezza dei contenitori creati utilizzando tale immagine. Se Container Image Security Enforcement non viene distribuito, puoi continuare a utilizzare un'immagine che ha problemi di sicurezza e configurazione in un contenitore. Se Container Image Security Enforcement è distribuito e attivo per l'immagine, tutti i problemi rilevati devono essere esentati dalla tua politica perché i contenitori possano essere distribuibili da questa immagine. 

Per configurare l'ambito di implementazione dei problemi del Controllo vulnerabilità in Container Image Security Enforcement, vedi [Personalizzazione delle politiche](/docs/services/Registry/registry_security_enforce.html#customize_policies).
{:tip}

Se la tua immagine non soddisfa i requisiti impostati dalla politica della tua organizzazione, devi configurare l'immagine in modo che soddisfi tali requisiti prima di poterla distribuire. Per ulteriori informazioni su come visualizzare e modificare la politica dell'organizzazione, consulta  [Impostazione delle politiche di esenzione organizzative](#va_managing_policy).
{:tip}

Dopo che hai distribuito la tua immagine, se viene distribuito lo Scanner contenitori, il Controllo vulnerabilità continua ad eseguire scansioni per rilevare eventuali problemi di sicurezza e configurazione nel contenitore. Puoi risolvere qualsiasi problema rilevato attenendosi alla procedura descritta in [Riesame di un report del contenitore](#va_reviewing_container).

### Riesame di un report di vulnerabilità utilizzando la GUI
{: #va_reviewing_gui}

Puoi riesaminare la sicurezza delle immagini Docker memorizzate nei tuoi spazi dei nomi in {{site.data.keyword.registrylong_notm}} utilizzando la GUI.
{:shortdesc}

1.  Accedi a {{site.data.keyword.Bluemix_notm}}.
2.  Nel catalogo, in **Infrastruttura**, fai clic su **Contenitori**.
3.  Fai clic sul tile **Registro contenitore**.
4.  Espandi **Controllo vulnerabilità** e fai clic su **Repository scansionati**.
5.  Per visualizzare il report per l'immagine contrassegnata come `latest`, fai clic sulla riga di tale repository. Il report mostra il numero totale di problemi e se sono pacchetti vulnerabili o problemi di configurazione. Se non esiste alcuna tag `latest` nel repository, viene utilizzata l'immagine più recente.
6.  Per visualizzare le informazioni su ogni pacchetto vulnerabile per l'immagine da te selezionata, nella tabella **Pacchetti vulnerabili trovati**, fai clic sul link nella colonna **VULNERABILITÀ** per aprire il report.
    1.  Per visualizzare ulteriori informazioni, espandi il riepilogo.
    2.  Se viene fornito un avviso del distributore del sistema operativo, fai clic sul link nella colonna **AVVISO UFFICIALE**.
7.  Per visualizzare le informazioni su ogni problema di configurazione, nella colonna **Problemi di configurazione trovati**, fai clic sulla riga del problema.
8.  Completa l'azione correttiva per ogni problema mostrato nel report e ricrea l'immagine. 


### Riesame di un report di vulnerabilità utilizzando la CLI
{: #va_registry_cli}

Puoi riesaminare la sicurezza delle immagini Docker memorizzate nei tuoi spazi dei nomi in {{site.data.keyword.registrylong_notm}} utilizzando la CLI.
{:shortdesc}

1.  Elenca le immagini nel tuo account {{site.data.keyword.Bluemix_notm}}. Viene restituito un elenco di tutte le immagini, indipendentemente dallo spazio dei nomi in cui sono memorizzate.

    ```
    ibmcloud cr image-list
    ```
    {: pre}

2.  Controlla lo stato nella colonna **STATO SICUREZZA**.
    -   `No Issues`: non è stato rilevato alcun problema di sicurezza.
    -   `<X> Issues`: `<X>` sono stati rilevati dei potenziali problemi di sicurezza o delle potenziali vulnerabilità, dove `<X>` è il numero di problemi.
    -   `Scanning`: è in corso la scansione dell'immagine e lo stato di vulnerabilità finale non è stato ancora determinato.
    
3.  Per visualizzare i dettagli per lo stato, riesaminare il report del Controllo vulnerabilità.

    ```
    ibmcloud cr va registry.<region>/<my_namespace>/<my_image>:<tag>
    ```
    {: pre}

    Nell'output della CLI, puoi visualizzare le seguenti informazioni sui problemi di sicurezza.
      - **Prassi di sicurezza**: una descrizione della vulnerabilità che è stata rilevata
      - **Azione correttiva**: i dettagli su come correggere la vulnerabilità


## Riesame di un report del contenitore
{: #va_reviewing_container}

Nel tuo dashboard, puoi visualizzare lo stato di un contenitore per determinare se la sua sicurezza soddisfa la tua politica dell'organizzazione. Puoi anche riesaminare il report della sicurezza di un contenitore, che indica in modo dettagliato gli eventuali pacchetti vulnerabilità e le impostazioni di contenitore o applicazione non sicure e se il contenitore è conforme alle politiche organizzative.
{:shortdesc}

Controlla che i contenitori che stai eseguendo nel tuo spazio continuino a essere conformi alla politica organizzativa riesaminando il campo **Stato politica**. Lo stato viene visualizzato come una delle seguenti condizioni:

-   Conforme alla politica -  non sono stati rilevati problemi di sicurezza o configurazione.
-   Non conforme alla politica - il Controllo vulnerabilità ha rilevato dei potenziali problemi di sicurezza o configurazione che hanno causato la non conformità del contenitore alla politica. Se la tua politica organizzativa consente la distribuzione di immagini vulnerabili, l'immagine potrebbe essere distribuita nello stato `Deploy with Caution` e all'utente che ne ha eseguito la distribuzione viene inviata un'avvertenza.
-   Valutazione non completa - La scansione non è completa. La scansione potrebbe essere ancora in esecuzione o il sistema operativo per tale istanza del contenitore potrebbe non essere compatibile.

Controlla che il tuo contenitore sia il più sicuro possibile visualizzando il relativo report di sicurezza e intervieni su qualsiasi problema di sicurezza o configurazione notificato completando la seguente procedura:

1.  Seleziona il contenitore per cui vuoi visualizzare un report:
    1.  Nel catalogo, seleziona **Contenitori** e fai clic su **Registro contenitore**.
    2.  Seleziona la scheda **Private Repositories** e seleziona la riga per il repository che desideri.
    3.  Seleziona la riga per la tag di immagine che desideri.
    4.  Seleziona la scheda **Associated Containers** e seleziona quindi la riga per il contenitore che desideri. Il report di sicurezza viene aperto.
2.  Riesamina le sezioni per vedere i potenziali problemi di sicurezza e configurazione per ogni pacchetto nell'immagine.

      -   **Vulnerabilità**: elenca i pacchetti che contengono problemi di vulnerabilità noti. L'elenco viene aggiornato giornalmente utilizzando gli avvisi di sicurezza pubblicati per i tipi di immagine Docker elencati in [Tipi di vulnerabilità](#types). Di norma, per fare in modo che un pacchetto vulnerabile superi con esito positivo la scansione, è necessaria una sua versione più recente che includa una correzione per la vulnerabilità in questione. Lo stesso pacchetto può elencare più vulnerabilità e in questo caso, un solo upgrade del pacchetto può risolvere più problemi. Fai clic sul codice dell'avviso di sicurezza per visualizzare maggiori informazioni sul pacchetto e i passi per aggiornare il pacchetto.

    -   **Configuration Issues**: elenca i consigli che puoi applicare per aumentare la sicurezza del contenitore e le eventuali impostazioni dell'applicazione per il contenitore che non sono sicure. Espandi la riga per vedere come risolvere il problema.

   I suggerimenti o le azioni correttive sono forniti per ogni elemento che viene elencato.
   
3.  Riesamina lo stato della politica per ogni problema di sicurezza. Lo stato della politica indica se questo problema è esente.

    -  **Active**: hai un problema che non è esentato e che sta interessando il tuo stato di sicurezza.
    -  **Exempt**: questo problema è esentato dalle tue impostazioni della politica.
    -  **Partially exempt**: questo problema è associato a più di un avviso di sicurezza. Gli avvisi di sicurezza non sono tutti esenti.

4.  Decidi come aggiornare il contenitore in modo da poter risolvere i problemi.

    **Importante**: per correggere i problemi con l'immagine del contenitore, devi eliminare la vecchia istanza ed eseguire nuovamente la distribuzione, il che significa perdere gli eventuali dati all'interno del contenitore esistente. Assicurati di avere una buona comprensione dell'architettura del tuo contenitore per scegliere il metodo appropriato di ridistribuzione del contenitore.

    **Esempio**

    -   Se il tuo contenitore è disaccoppiato dai dati che calcola, puoi arrestarlo ed eliminarlo, eseguire le modifiche necessarie all'immagine ed eseguire la ridistribuzione senza perdere dati.
    -   Puoi utilizzare un servizio {{site.data.keyword.Bluemix_notm}}, come ad esempio [Delivery Pipeline](/docs/services/ContinuousDelivery/pipeline_about.html#deliverypipeline_about), per assistenza nell'aggiornamento dell'istanza del contenitore vulnerabile.
    -   In un'architettura dei microservizi, puoi instradare il traffico a un'altra istanza del contenitore mentre correggi i problemi di sicurezza o di configurazione ed esegui il push della nuova immagine in una distribuzione red/black.

5.  Se non puoi correggere il problema ora, puoi esentare il problema nelle tue impostazioni della politica, il che impedisce al problema di bloccare la distribuzione del contenitore. Per esentare il problema, fai clic sull'icona **apri e chiudi elenco di opzioni** e fai clic su **Create Exemption**; vedi [Impostazione delle politiche di esenzione organizzative](#va_managing_policy).

6.  Correggi i problemi che sono descritti nel report **Sicurezza** e crea nuovamente l'immagine oppure ridistribuisci il contenitore in base al metodo da te scelto.
