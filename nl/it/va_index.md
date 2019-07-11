---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-03"

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


# Gestione della sicurezza delle immagini con il Controllo vulnerabilità
{: #va_index}

Il Controllo vulnerabilità verifica lo stato di sicurezza delle immagini del contenitore fornite da {{site.data.keyword.IBM}}, da terze parti oppure aggiunte allo spazio dei nomi del registro della tua organizzazione.
{:shortdesc}

Quando aggiungi un'immagine a uno spazio dei nomi, il Controllo vulnerabilità ne esegue automaticamente la scansione per individuare problemi di sicurezza e potenziali vulnerabilità. Se si riscontrano dei problemi di sicurezza, vengono fornite le istruzioni per aiutare a correggere le vulnerabilità segnalate.

Il Controllo vulnerabilità fornisce la gestione della sicurezza per [{{site.data.keyword.registrylong_notm}}](/docs/services/Registry?topic=registry-getting-started#getting-started), generando un report sullo stato della sicurezza che include le correzioni e le prassi ottimali consigliate.

Qualsiasi problema rilevato dal controllo vulnerabilità determina un verdetto che indica che non è consigliabile distribuire questa immagine. Se scegli di distribuire l'immagine, qualsiasi contenitore distribuito dall'immagine include dei problemi noti che potrebbero essere utilizzati per attaccarlo o comprometterlo in altro modo. Il verdetto viene modificato in base a tutte le esenzioni che hai specificato. Questo verdetto può essere utilizzato da Container Image Security Enforcement per evitare la distribuzione di immagini non sicure in {{site.data.keyword.containerlong_notm}}.

La correzione dei problemi di configurazione e sicurezza notificati dal Controllo vulnerabilità può aiutarti a proteggere la tua infrastruttura {{site.data.keyword.cloud_notm}}.

## Informazioni sul Controllo vulnerabilità
{: #about}

Il Controllo vulnerabilità fornisce funzioni che ti aiutano a proteggere le tue immagini.
{:shortdesc}

Sono disponibili le seguenti funzioni:

- Scansione delle immagini per rilevare eventuali problemi
- Fornisce un report di valutazione basato sulle prassi di sicurezza specifiche per {{site.data.keyword.containerlong_notm}}
- Fornisce raccomandazioni per proteggere i file di configurazione per una serie secondaria di tipi di applicazione
- Fornisce istruzioni su come correggere un [pacchetto vulnerabile](#packages) o un [problema di configurazione](#app_configurations) segnalato nei relativi report
- Fornisce i verdetti a [Container Image Security Enforcement](/docs/services/Registry?topic=registry-security_enforce#security_enforce)
- Applica esenzioni ai report a un livello di account, spazio dei nomi, repository o tag per indicare quando dei problemi contrassegnati non si applicano al tuo caso d'uso.

Nel dashboard Registro, la colonna **Stato politica** visualizza lo stato dei tuoi repository. Il report collegato identifica le procedure consigliate per la sicurezza cloud per le tue immagini.

Il dashboard del Controllo vulnerabilità fornisce una panoramica e una valutazione della sicurezza di un'immagine. Se desideri ulteriori informazioni sul dashboard Controllo vulnerabilità, consulta [Riesame di un report di vulnerabilità](#va_reviewing).

**Protezione dei dati**

Per eseguire la scansione di immagini e contenitori nel tuo account per rilevare eventuali problemi di sicurezza, il Controllo vulnerabilità raccoglie, memorizza ed elabora le seguenti informazioni:

- Campi in formato libero, compresi ID, descrizioni e nomi immagine (registro, spazio dei nomi, nome repository e tag immagine)
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

I pacchetti che contengono problemi di vulnerabilità noti vengono visualizzati nei risultati della scansione. Le vulnerabilità possibili vengono aggiornate dagli avvisi di sicurezza pubblicati giornalmente per i tipi di immagine Docker elencati nella seguente tabella. Di norma, per fare in modo che un pacchetto vulnerabile superi con esito positivo la scansione, è necessaria una sua versione più recente che includa una correzione per la vulnerabilità in questione. Lo stesso pacchetto può elencare più vulnerabilità e in questo caso, un solo aggiornamento del pacchetto può risolvere più vulnerabilità.

  |Immagine di base Docker|Origine delle informazioni particolari di sicurezza|
  |-----------------|--------------------------|
  |Alpine|[Git - Alpine Linux ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://git.alpinelinux.org/) e [CIRCL CVE Search ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://cve.circl.lu/).|
  |CentOS| [CentOS announce archives ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://lists.centos.org/pipermail/centos-announce/) e [CentOS CR announce archives ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://lists.centos.org/pipermail/centos-cr-announce/). Per ulteriori informazioni sulle vulnerabilità, vedi [Vulnerabilità nei pacchetti su CentOS](#va_centos).|
  |Debian|[Debian security announcements ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://lists.debian.org/debian-security-announce/).|
  |RHEL (Red Hat Enterprise Linux)|[Red Hat Security Data API ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://access.redhat.com/labsinfo/securitydataapi).|
  |Ubuntu|[Ubuntu Security Notices ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://usn.ubuntu.com/).|
  {: caption="Tabella 1. Immagini di base Docker supportate verificate dal Controllo vulnerabilità per rilevare eventuali pacchetti vulnerabili" caption-side="top"}

### Problemi di configurazione
{: #app_configurations}

I problemi di configurazione sono dei potenziali problemi di sicurezza correlati al modo in cui è configurata un'applicazione. Molti dei problemi segnalati possono essere corretti aggiornando il tuo Dockerfie.
{:shortdesc}

Le immagini vengono scansionate solo se stanno utilizzando un sistema operativo supportato dal controllo vulnerabilità. Il Controllo vulnerabilità controlla le impostazioni di configurazione per i seguenti tipi di applicazioni:

- MySQL
- NGINX
- Apache

## Riesame di un report di vulnerabilità
{: #va_reviewing}

Prima che tu distribuisca un'immagine, puoi riesaminare il relativo report del Controllo vulnerabilità per i dettagli relativi ad eventuali pacchetti vulnerabili e impostazioni delle applicazioni o del contenitore non sicure. Puoi inoltre controllare se l'immagine è conforme alle politiche organizzative.
{:shortdesc}

Se non te ne occupi, i problemi rilevati potrebbero avere delle ripercussioni sulla sicurezza dei contenitori creati utilizzando tale immagine. Se Container Image Security Enforcement non viene distribuito, puoi continuare a utilizzare un'immagine che ha problemi di sicurezza e configurazione in un contenitore. Se Container Image Security Enforcement è distribuito e attivo per l'immagine, tutti i problemi rilevati devono essere esentati dalla tua politica perché i contenitori possano essere distribuibili da questa immagine.

Per configurare l'ambito di implementazione dei problemi del Controllo vulnerabilità in Container Image Security Enforcement, vedi [Personalizzazione delle politiche](/docs/services/Registry?topic=registry-security_enforce#customize_policies).
{:tip}

Se la tua immagine non soddisfa i requisiti impostati dalla politica della tua organizzazione, devi configurare l'immagine in modo che soddisfi tali requisiti prima di poterla distribuire. Per ulteriori informazioni su come visualizzare e modificare la politica dell'organizzazione, consulta  [Impostazione delle politiche di esenzione organizzative](#va_managing_policy).
{:tip}

### Riesame di un report di vulnerabilità utilizzando la GUI
{: #va_reviewing_gui}

Puoi riesaminare la sicurezza delle immagini Docker memorizzate nei tuoi spazi dei nomi in {{site.data.keyword.registrylong_notm}} utilizzando la GUI.
{:shortdesc}

1. Accedi a {{site.data.keyword.cloud_notm}}.
2. Fai clic sull'icona relativa al menu di navigazione (**Navigation Menu**) e fai quindi clic su **Kubernetes**.
3. Fai clic su **Registry** e fai quindi clic sul tile **Images**. Viene visualizzato un elenco delle tue immagini e dello stato di sicurezza di ogni immagine nella tabella **Images**.
4. Per visualizzare il report per l'immagine contrassegnata come `latest`, fai clic sulla riga di tale immagine. Si apre la scheda **Image Details** che mostra i dati di tale immagine. Se non esiste alcuna tag `latest` nel repository, viene utilizzata l'immagine più recente.
5. Se lo stato di sicurezza mostra un problema, per trovare informazioni sui problemi, fai clic sulla scheda **Issues by Type**. Si aprono le tabelle **Vulnerabilities** e **Configuration Issues**.

   - Tabella **Vulnerabilities**. Mostra l'ID di vulnerabilità di ogni problema, lo stato della politica di tale problema, i pacchetti interessati e come risolvere il problema. Per visualizzare ulteriori informazioni su tale problema, espandi la riga. Viene visualizzato un riepilogo di tale problema, che contiene un link all'avviso di sicurezza del fornitore per tale problema. Elenca i pacchetti che contengono problemi di vulnerabilità noti.
  
     L'elenco viene aggiornato giornalmente utilizzando gli avvisi di sicurezza pubblicati per i tipi di immagine Docker elencati in [Tipi di vulnerabilità](#types). Di norma, per fare in modo che un pacchetto vulnerabile superi con esito positivo la scansione, è necessaria una sua versione più recente che includa una correzione per la vulnerabilità in questione. Lo stesso pacchetto può elencare più vulnerabilità e in questo caso, un solo aggiornamento del pacchetto può risolvere più problemi. Fai clic sul codice dell'avviso di sicurezza per visualizzare maggiori informazioni sul pacchetto e i passi per aggiornare il pacchetto.

   - Tabella **Configuration Issues**. Mostra l'ID del problema di configurazione di ogni problema, lo stato della politica per tale problema e la prassi di sicurezza. Per visualizzare ulteriori informazioni su tale problema, espandi la riga. Viene visualizzato un riepilogo di tale problema, che contiene un link all'avviso di sicurezza per tale problema.
  
     L'elenco contiene i consigli per le azioni che puoi applicare per aumentare la sicurezza del contenitore e le eventuali impostazioni dell'applicazione per il contenitore che non sono sicure. Espandi la riga per vedere come risolvere il problema.

6. Completa l'azione correttiva per ogni problema mostrato nel report e ricrea l'immagine.

### Riesame di un report di vulnerabilità utilizzando la CLI
{: #va_registry_cli}

Puoi riesaminare la sicurezza delle immagini Docker memorizzate nei tuoi spazi dei nomi in {{site.data.keyword.registrylong_notm}} utilizzando la CLI.
{:shortdesc}

1. Elenca le immagini nel tuo account {{site.data.keyword.cloud_notm}}. Viene restituito un elenco di tutte le immagini, indipendentemente dallo spazio dei nomi in cui sono memorizzate.

   ```
   ibmcloud cr image-list
   ```
   {: pre}

2. Controlla lo stato nella colonna **SECURITY STATUS**.
    - `No Issues` non è stato rilevato alcun problema di sicurezza.
    - `<X> Issues` il numero rilevato di potenziali problemi di sicurezza o di potenziali vulnerabilità, dove `<X>` è il numero di problemi.
    - `Scanning` è in corso la scansione dell'immagine e lo stato di vulnerabilità finale non è determinato.

3. Per visualizzare i dettagli per lo stato, esamina il report del Controllo vulnerabilità:

   ```
   ibmcloud cr va registry.<region>/<my_namespace>/<my_image>:<tag>
   ```
   {: pre}

   Nell'output della CLI, puoi visualizzare le seguenti informazioni sui problemi di sicurezza.
      - `Security practice` una descrizione della vulnerabilità che è stata rilevata
      - `Corrective action` i dettagli su come correggere la vulnerabilità

### Vulnerabilità nei pacchetti su CentOS
{: #va_centos}

Se stai utilizzando CentOS, potresti ottenere dei falsi positivi nel tuo report, ossia, il report potrebbe segnalare una vulnerabilità quando non è così. Questa situazione si verifica quando un avviso di sicurezza viene rilasciato da Red Hat ma non è applicabile a CentOS, oppure la correzione non è ancora stata riportata in CentOS.
{:shortdesc}

Se ricevi un report che indica che il tuo pacchetto ha delle vulnerabilità, completa la seguente procedura:

1. Visualizza la procedura per l'aggiornamento del pacchetto facendo clic sul codice dell'avviso di sicurezza.
2. Aggiorna il pacchetto completando la procedura per aggiornare il pacchetto.
3. Se il pacchetto viene aggiornato, il risultato non era un falso positivo e l'azione richiesta è stata completata.
4. Se il pacchetto non viene aggiornato perché non sono disponibili delle versioni più recenti per l'installazione, il risultato era un falso positivo. Puoi aggiungere una politica di esenzione per questo avviso di sicurezza, vedi [Impostazione delle politiche di esenzione organizzative](#va_managing_policy).

## Impostazione delle politiche di esenzione organizzative
{: #va_managing_policy}

Se vuoi gestire la sicurezza di un'organizzazione {{site.data.keyword.cloud_notm}}, puoi utilizzare la tua impostazione della politica per determinare se un problema è esente o meno. Puoi utilizzare Container Image Security Enforcement per garantire che la distribuzione sia consentita solo da immagini che non contengono alcun problema di sicurezza, una volta ignorati quelli esentati dalla tua politica.
{:shortdesc}

Puoi distribuire i contenitori da qualsiasi immagine indipendentemente dallo stato della sicurezza, a meno che nel tuo cluster non sia distribuito Container Image Security Enforcement. Per informazioni su come distribuire Container Image Security Enforcement, vedi [Installazione di Security Enforcement](/docs/services/Registry?topic=registry-security_enforce#security_enforce).

Quando utilizzi Container Image Security Enforcement, qualsiasi problema di sicurezza rilevato dal Controllo vulnerabilità impedisce la distribuzione di un contenitore dall'immagine. Per consentire la distribuzione di un'immagine con dei problemi rilevati, è necessario aggiungere delle esenzioni alla tua politica.

### Impostazione delle politiche di esenzione organizzative utilizzando la GUI
{: #va_managing_policy_gui}

Se vuoi impostare delle esenzioni per la politica utilizzando la GUI, completa la seguente procedura:

1. Accedi a {{site.data.keyword.cloud_notm}}. Devi aver eseguito l'accesso per visualizzare il Controllo vulnerabilità nella GUI.
2. Fai clic sull'icona relativa al menu di navigazione (**Navigation Menu**) e fai quindi clic su **Kubernetes**.
3. In **Vulnerability Advisor**, fai clic su **Policy Settings**.
4. Fai clic su **Create Exemption**.
5. Seleziona il tipo di problema.
6. Immetti l'ID problema.

   Puoi trovare queste informazioni nel tuo [report di vulnerabilità](#va_reviewing). La colonna **Vulnerability ID** contiene l'ID da utilizzare per i problemi di avvisi di sicurezza o CVE; la colonna **Configuration Issue ID** contiene l'ID da utilizzare per i problemi di configurazione.
   {: tip}

7. Seleziona lo spazio dei nomi del registro, il repository e la tag a cui vuoi applicare l'esenzione.
8. Fai clic su **Save**.

Puoi anche modificare e rimuovere le esenzioni passando il puntatore del mouse sulla riga pertinente e facendo clic sull'icona **apri e chiudi elenco delle opzioni**.

### Impostazione delle politiche di esenzione organizzative utilizzando la CLI
{: #va_managing_policy_cli}

Se vuoi impostare delle esenzioni per la politica utilizzando la CLI, puoi eseguire questi comandi:

- Per creare un'esenzione per un problema di sicurezza, esegui il comando [`ibmcloud cr exemption-add`](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_exemption_add).
- Per elencare le eccezioni per i problemi di sicurezza, esegui il comando [`ibmcloud cr exemption-list`](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_exemption_list) .
- Per elencare i tipi di problemi di sicurezza che puoi esentare, esegui il comando [`ibmcloud cr exemption-types`](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_exemption_types).
- Per eliminare un'esenzione per un problema di sicurezza, esegui il comando [`ibmcloud cr exemption-rm`](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_exemption_rm).

Per ulteriori informazioni sui comandi, puoi utilizzare l'indicatore `-- help` quando esegui il comando.
