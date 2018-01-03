---

copyright:
  years: 2015, 2017

lastupdated: "2017-03-07"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}

# Immagine contenitore IBM MQ Bluemix
{: #ibm_mq}

L'immagine ibm-mq viene fornita per {{site.data.keyword.containershort}}. Include {{site.data.keyword.IBM}} MQ Advanced for Developers (non coperto da garanzia). 

{:shortdesc}


## Funzionamento
{: #ibm_mq_how}

{{site.data.keyword.IBM_notm}} MQ Advanced for Developers contiene tutto il necessario per iniziare a sviluppare le tue proprie soluzioni di messaggistica e le applicazioni {{site.data.keyword.IBM_notm}} MQ. 

Per sviluppare le soluzioni di messaggistica e le applicazioni {{site.data.keyword.IBM_notm}} MQ in {{site.data.keyword.Bluemix_notm}}, puoi distribuire l'immagine ibm-mq Docker in {{site.data.keyword.containershort}}. Successivamente, puoi creare ed eseguire i gestori code e utilizzare la IU web o un terminale in modo da configurarli per soddisfare i requisiti dell'applicazione o della soluzione di messaggistica.

**Nota:** puoi utilizzare l'immagine ibm-mq solo per lo sviluppo e la verifica di unità. Puoi anche utilizzare l'immagine per esplorare il prodotto, imparare dalle esercitazioni e valutare il contributo che {{site.data.keyword.IBM_notm}} MQ può apportare alla tua organizzazione.

## Elementi inclusi
{: #ibm_mq_what}

Questa immagine ibm-mq contiene il pacchetto software per {{site.data.keyword.IBM_notm}} MQ Advanced for Developers versione 9.0.x, che è una versione completa del prodotto che puoi utilizzare per lo sviluppo e la verifica di unità. Puoi scaricare questa versione gratuitamente e sei libero di utilizzarla nei termini della licenza.

{{site.data.keyword.IBM_notm}} MQ Advanced for Developers è disponibile per i sistemi operativi Windows a 64 bit e Linux su x86-64. Tutti i prerequisiti del prodotto sono inclusi nel pacchetto di download. Per ulteriori informazioni, consulta [IBM MQ Downloads ![Icona link esterno](../../../../icons/launch-glyph.svg "Icona link esterno")](http://www-03.ibm.com/software/products/lv/ibm-mq-advanced-for-developers){: new_window}

Per informazioni sulle funzioni incluse in {{site.data.keyword.IBM_notm}} MQ Advanced for Developers, consulta [IBM MQ ![Icona link esterno](../../../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/support/knowledgecenter/SSFKSJ_9.0.0/com.ibm.mq.helphome.v90.doc/WelcomePagev9r0.htm){: new_window}



## Restrizioni di utilizzo 
{: #ibm_mq_usage}

La seguente tabella mostra le restrizioni che si applicano all'utilizzo gratuito dell'immagine ibm-mq in {{site.data.keyword.Bluemix_notm}}: 

| Ambiente	 | Restrizioni di utilizzo gratuito |
|-------------|-------------------------|
| Sviluppo | Utilizzo gratuito illimitato dell'immagine ibm-mq per la creazione di singoli contenitori per lo sviluppo. |
| Verifica | Utilizzo gratuito illimitato dell'immagine ibm-mq per la creazione di singoli contenitori per la verifica di unità. Controlla le [Informazioni sulla licenza](getstarted.html#ibm_mq_license ) per ulteriori informazioni su quali verifiche sono consentite. |
| Produzione | La licenza di {{site.data.keyword.IBM_notm}} MQ Advanced for Developers non ne consente l'uso per la produzione. |

**Nota:** il prezzo dell'immagine ibm-mq è indipendente dal prezzo dei contenitori che utilizzi in {{site.data.keyword.Bluemix_notm}}. 


## Informazioni sulla licenza 
{: #ibm_mq_license}

Informazioni sulle licenze: 

*	I Dockerfile e gli script associati sono concessi in licenza con la  [Apache License 2.0 ![Icona link esterno](../../../../icons/launch-glyph.svg "Icona link esterno")](http://www.apache.org/licenses/LICENSE-2.0.html){: new_window}.
* [Informazioni sulla licenza per {{site.data.keyword.IBM_notm}} MQ Advanced for Developers ![Icona link esterno](../../../../icons/launch-glyph.svg "Icona link esterno")](http://www-03.ibm.com/software/sla/sladb.nsf/displaylis/973FA648A5DE42948525806E004CC757?OpenDocument){: new_window}.

**Nota:** la licenza non consente un'ulteriore distribuzione. I termini per {{site.data.keyword.IBM_notm}} MQ nell'immagine limitano l'utilizzo per lo sviluppo e la verifica di unità. 


## Introduzione
{: #ibm_mq_get_started}

Completa la seguente procedura per eseguire e gestire {{site.data.keyword.IBM_notm}} MQ in un contenitore in esecuzione su {{site.data.keyword.Bluemix_notm}}: 

1. [Esegui il provisioning di un contenitore basato sull'immagine ibm-mq](ibm_mq.html#ibm_mq_provision)

    **Nota:** il contenitore è legato allo stato di esecuzione del gestore code al suo interno. Se arresti il gestore code mediante la console o la riga di comando, il contenitore si arresterà poco dopo.  

2. Gestisci le risorse {{site.data.keyword.IBM_notm}} MQ in esecuzione in un contenitore. Puoi farlo graficamente dalla console web di IBM MQ o a livello di programmazione usando i comandi da un terminale.  

    1. Scegli una delle seguenti opzioni per gestire le risorse di {{site.data.keyword.IBM_notm}} MQ nel contenitore: 
    
        *	Avvia la console web di {{site.data.keyword.IBM_notm}} MQ e gestisci {{site.data.keyword.IBM_notm}} MQ graficamente. Per ulteriori informazioni, consulta [Avvia la console web di IBM MQ](ibm_mq.html#ibm_mq_webui).
        *	Da un terminale, utilizza la CLI Docker. Per ulteriori informazioni, consulta [Configurazione di un terminale per l'utilizzo della CLI Docker](ibm_mq.html#ibm_mq_docker_commands).
        * Da un terminale, utilizza la CLI del servizio IBM Bluemix Container. Per ulteriori informazioni, consulta [Configurazione di un terminale per l'utilizzo della CLI del servizio IBM Bluemix Container](ibm_mq.html#ibm_mq_containers_cli)
        
    2. Configura il gestore code {{site.data.keyword.IBM_notm}} MQ che viene eseguito nel contenitore.  

        * Per gestire il gestore code attraverso la riga di comando, utilizza il programma *runmqsc*. Esegui questo comando: `runmqsc QM_Name` dove *QM_Name* è il nome del tuo gestore code. Per ulteriori informazioni, consulta [Gestione del tuo contenitore attraverso la riga di comando](ibm_mq.html#ibm_mq_access).
    
        * Per gestire graficamente il gestore code, utilizza la IU web. 
        
    3. Segui le istruzioni riportate in [IBM MQ Knowledge Center ![Icona link esterno](../../../../icons/launch-glyph.svg "Icona link esterno")](http://www.ibm.com/support/knowledgecenter/en/SSFKSJ_9.0.0/com.ibm.mq.helphome.v90.doc/WelcomePagev9r0.htm){: new_window} per creare code di messaggi sul tuo gestore code dalle quali inserire o estrarre i messaggi.
    
        Ad esempio, in `runmqsc` immetti il seguente comando per creare una nuova coda locale: `DEFINE QLOCAL('MY.Q')`, dove *MY.Q* è il nome della coda locale dal terminale. Puoi visualizzare la coda nella console web o puoi visualizzare la coda e le sue proprietà eseguendo in 'runmqsc` il seguente comando: `DISPLAY QLOCAL('MY.Q')`. 
    
3. [Facoltativo] Monitora i log {{site.data.keyword.IBM_notm}} MQ nel contenitore. Per ulteriori informazioni, consulta [Gestione del tuo contenitore attraverso la riga di comando](ibm_mq.html#ibm_mq_access).

Per ulteriori informazioni sulla guida introduttiva di IBM MQ, vedi [Pagina di benvenuto di IBM MQ versione 9.0.x ![Icona link esterno](../../../../icons/launch-glyph.svg "Icona link esterno")](http://www.ibm.com/support/knowledgecenter/en/SSFKSJ_9.0.0/com.ibm.mq.helphome.v90.doc/WelcomePagev9r0.htm){: new_window}

Per ulteriori informazioni sull'immagine e su come creare la tua propria immagine localmente in Docker, vedi [la pagina MQ Docker GitHu ![Icona link esterno](../../../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/ibm-messaging/mq-docker){: new_window}
    
Per i blog e le informazioni più recenti su IBM MQ, vedi [il blog MQ DeveloperWorks ![Icona link esterno](../../../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/developerworks/community/blogs/messaging){: new_window}


### Provisioning di un contenitore basato sull'immagine ibm-mq 
{: #ibm_mq_provision}

Completa la seguente procedura per eseguire il provisioning di un contenitore Docker in {{site.data.keyword.Bluemix_notm}} basato sull'immagine ibm-mq fornita da {{site.data.keyword.IBM_notm}}: 

1.	Accedi a {{site.data.keyword.Bluemix_notm}}. Esegui l'accesso con il tuo ID {{site.data.keyword.Bluemix_notm}}. 
2.	Dal catalogo, seleziona **Contenitori** e scegli l'immagine *ibm-mq*. 
3.	Seleziona **Singolo** per creare un singolo contenitore dell'istanza, che può essere utilizzato per scopi di sviluppo e verifica. 
4.	Immetti il nome del contenitore, ad esempio mq. 
5.	Seleziona la dimensione del contenitore. 
6.	Nel campo Indirizzo IP pubblico, seleziona **Richiedi ed esegui il bind dell'IP pubblico**. 
7.	Nel campo Porte pubbliche, specifica **1414/tcp, 9443/tcp**. 
8.	Espandi le Opzioni avanzate, fai clic su **Aggiungi una nuova variabile di ambiente** e immetti le seguenti variabili di ambiente: LICENSE e MQ_QMGR_NAME. 

    * Imposta il valore di **LICENSE** su **accept** per accettare le condizioni della licenza di IBM MQ Advanced for Developers.  
    
        Se vuoi visualizzare la licenza prima di accettarla, puoi impostare LICENSE su “view” e la licenza verrà stampata nei log del contenitore.  
    
        Se l'inglese non è la tua prima lingua, puoi visualizzare il file di licenza in una lingua diversa impostando un'altra variabile di ambiente chiamata **LANG** su uno dei seguenti valori: *zh_TW* per Cinese_TW, *zh* per Cinese, *cs* per Ceco, *en* per Inglese, *fr* per Francese, *de* per Tedesco, *el* per Greco, *id* per Indonesiano, *it* per Italiano, *ja* per Giapponese, *ko* per Coreano, *lt* per Lituano, *pl* per Polacco, *pt* per Portoghese, *ru* per Russo, *sl* per Sloveno, *es* per Spagnolo o *tr* per Turco. 

    * Imposta il valore di **MQ_QMGR_NAME** con il valore *MYMQ* dove *MYMQ* è il nome gestore code che hai scelto. 
 
        **Nota:** se non specifichi i valori per le variabili di ambiente LICENSE e MQ_QMGR_NAME, il contenitore viene creato ma non avviato.  
    
    * [Facoltativo] Per impostazione predefinita, la console di {{site.data.keyword.IBM_notm}} MQ Web viene avviata nel contenitore. Per modificare il comportamento predefinito, imposta il valore della variabile di ambiente **MQ_DISABLE_WEB_CONSOLE** su *true*. 
    
        **Nota:** per utilizzare la console web, non impostare la variabile di ambiente *MQ_DISABLE_WEB_CONSOLE*. 

    * [Facoltativo] Per visualizzare i log MQ attraverso la IU {{site.data.keyword.Bluemix_notm}}, imposta la variabile di ambiente **LOG_LOCATIONS** sul valore `/var/mqm/qmgr/QM_Name/errors/AMQERR01.LOG` dove *QM_Name* è il valore che hai impostato per la variabile di ambiente *MQ_QMGR_NAME*.
    
    *	[Facoltativo] Per impostazione predefinita, vengono creati gli oggetti predefiniti di {{site.data.keyword.IBM_notm}} MQ Developer. Per modificare il comportamento predefinito, imposta la variabile di ambiente **MQ_DEV** su *false* e modifica i valori di installazione predefiniti per IBM MQ con i tuoi valori personalizzati. Per ulteriori informazioni, consulta [Modifica i valori di installazione predefiniti](ibm_mq.html#ibm_mq_dev_default)
 
9. [Facoltativo] Nella sezione Opzioni avanzate, assegna un volume. Utilizza i volumi {{site.data.keyword.Bluemix_notm}} per memorizzare i dati e la configurazione di {{site.data.keyword.IBM_notm}} MQ in modo permanente nel corso delle migrazioni del contenitore. I dati {{site.data.keyword.IBM_notm}} MQ che non sono memorizzati in un volume andranno persi se un contenitore viene chiuso. 

    L'immagine ibm-mq può essere utilizzata con i volumi {{site.data.keyword.Bluemix_notm}}, tuttavia i volumi devono essere montati su **/mnt/mqm** affinché possano essere rilevati e utilizzati da {{site.data.keyword.IBM_notm}} MQ. 

    Per impostazione predefinita, tutti i contenitori creati senza un volume perderanno i dati IBM MQ quando il contenitore viene arrestato. Per evitarlo, devi utilizzare la messaggistica persistente e i volumi {{site.data.keyword.Bluemix_notm}}. 

    1. Fai clic su **Crea un volume**, immetti un nome per il volume e seleziona una condivisione file. 
    2. Fai clic su **Assegna un volume esistente** e seleziona il volume che hai creato. Immetti **/mnt/mqm** nella casella del percorso. Assicurati che la casella **Sola lettura** non sia selezionata. 

    **Nota:** quando monti un volume da utilizzare come directory dei dati IBM MQ, devi montarlo nella posizione */mnt/mqm*. Montandolo in */mnt/mqm*, lo script ibm-mq rileverà il volume e configurerà il file system IBM MQ per utilizzarlo. 
     
10.	Fai clic su **Crea** e attendi che il contenitore avvii l'esecuzione.

    Una volta creato il contenitore, viene aperto il dashboard relativo. Controlla che lo stato del contenitore sia impostato su **In esecuzione**.  

    Per controllare i log del tuo contenitore, fai clic su **Monitoraggio e log** e seleziona la scheda **Registrazione**. Vengono visualizzati i log. Per un'analisi avanzata dei tuoi log, fai clic su **Vista avanzata** per visualizzare i tuoi log in Kibana. 

Hai creato e avviato un gestore code. Adesso, puoi connettere un'applicazione MQ utilizzando i seguenti dettagli di connessione: 

| Proprietà della connessione | Valore|
|-------|-------|
| Indirizzo IP | Indirizzo IP pubblico del contenitore |
| Porta | 1414 |
| Canale | DEV.APP.SVRCONN |


### Gestione del tuo contenitore attraverso la riga di comando 
{: #ibm_mq_access}

Dopo che il contenitore che esegue {{site.data.keyword.IBM_notm}} MQ viene distribuito in {{site.data.keyword.Bluemix_notm}}, puoi accedere al contenitore e controllarne lo stato e convalidare l'installazione di {{site.data.keyword.IBM_notm}} MQ.

Completa la seguente procedura per verificare il setup e la configurazione di {{site.data.keyword.IBM_notm}} MQ nel contenitore: 

1.	Configura la CLI IBM Containers. Per ulteriori informazioni, consulta [la pagina Bluemix CLI](https://plugins.ng.bluemix.net/ui/home.html) e segui le istruzioni per installare l'ultimo plugin del servizio Cloud Foundry Container.

2.	Da un terminale, accedi alla tua organizzazione e al tuo spazio {{site.data.keyword.Bluemix_notm}} in cui è in esecuzione il contenitore utilizzando il tuo ID {{site.data.keyword.Bluemix_notm}}. Esegui questi comandi: 

    1. `bx login`
    2. `bx ic init`
    
3.	Controlla lo stato del contenitore. Esegui il seguente comando: `bx ic ps`

4.	Allega una sessione bash al tuo contenitore:

    `bx ic exec -it container_name /bin/bash`
    
    dove container_name è il nome del contenitore. 
    
5.	Esegui i comandi per gestire la tua configurazione MQ, ad esempio: 

| Comando| Azione|
|---------|---------|
| dspmqver | Verificare la versione di IBM MQ e le informazioni di build. |
| dspmq | Controllare lo stato del gestore code in esecuzione nel contenitore. | 
| runmqsc | Gestire le risorse MQ. |

Per monitorare i log MQ, scegli una delle seguenti opzioni: 

* Se il tuo terminale è configurato per eseguire i comandi CLI {{site.data.keyword.containershort}}, immetti il seguente comando: `bx ic exec container_id tail -f /var/mqm/qmgr/QM_ Name/errors/AMQERR01.LOG` dove *container_id* è il nome del tuo contenitore e *QM_ Name* è il nome del tuo gestore code.
    
* Se il tuo terminale è configurato per eseguire i comandi della CLI Docker, esegui questo comando: `docker exec container_id tail -f /var/mqm/qmgr/QM_Name/errors/AMQERR01.LOG` dove *container_id* è il nome del tuo contenitore e *QM_Name* è il nome del tuo gestore code. 
    
    

### Avvio della console web di IBM MQ 
{: #ibm_mq_webui}

Completa la seguente procedura per avviare la console web di {{site.data.keyword.IBM_notm}} MQ: 

1.	Nel dashboard {{site.data.keyword.Bluemix_notm}}, seleziona il tuo contenitore e verifica che lo stato del contenitore sia impostato su *In esecuzione*. 
2.	Verifica che per il contenitore sia stata configurata la porta pubblica 9443. 
3.	Controlla i dettagli del contenitore per trovare l'IP pubblico o, se non lo hai già fatto, richiedi ed esegui il bind di un indirizzo. 
4.	Da un browser web, avvia il seguente URL: `https://DockerContainerPublicIP:9443/ibmmq/console/` dove DockerContainerPublicIP è l'IP pubblico del tuo contenitore. 

    Si apre un avviso di sicurezza perché viene generato e autofirmato il certificato TLS utilizzato dal server web per ospitare la console MQ. Fai clic su **aggiungi eccezione** e conferma. 

    **Nota:** in un ambiente di produzione, utilizza il tuo certificato che viene riconosciuto e considerato attendibile dal tuo browser. Questo certificato deve essere emesso da un'autorità di certificazione.  
    
    Il browser apre l'interfaccia utente web. 
    
5. Accedi alla console {{site.data.keyword.IBM_notm}} MQ con le credenziali: 

    1.	Utente: admin
    2.	Password: passw0rd
    
Sei ora pronto a gestire IBM MQ. 

### Configurazione di un terminale per l'utilizzo della CLI del servizio IBM Bluemix Container 
{: #ibm_mq_containers_cli}

Utilizza la CLI {{site.data.keyword.containershort}} per eseguire i comandi di gestione di {{site.data.keyword.IBM_notm}} MQ direttamente in un contenitore.

Completa la seguente procedura per configurare un terminale per eseguire i comandi bx ic per gestire IBM MQ:

1.	Configura la CLI {{site.data.keyword.containershort}}. Per ulteriori informazioni, consulta [la pagina Bluemix CLI](https://plugins.ng.bluemix.net/ui/home.html) e segui le istruzioni per installare l'ultimo plugin del servizio Cloud Foundry Container.

2.	Da un terminale, accedi a {{site.data.keyword.Bluemix_notm}}. Esegui questi comandi: 

    1. `bx login`
    2. `bx ic init`
    
3.	Allega una sessione bash al tuo contenitore:

    `bx ic exec -it container_name /bin/bash`
    
    dove container_name è il nome del contenitore. 
    


### Configurazione di un terminale per l'utilizzo della CLI Docker 
{: #ibm_mq_docker_commands}

Utilizza la CLI Docker per eseguire i comandi di amministrazione di {{site.data.keyword.IBM_notm}} MQ direttamente in un contenitore. 

Completa la seguente procedura per configurare un terminale per l'esecuzione dei comandi Docker per gestire IBM MQ: 

1.	Configura la CLI {{site.data.keyword.containershort}}. Per ulteriori informazioni, consulta [la pagina Bluemix CLI](https://plugins.ng.bluemix.net/ui/home.html) e segui le istruzioni per installare l'ultimo plugin del servizio Cloud Foundry Container.

2.	Da un terminale, accedi a {{site.data.keyword.Bluemix_notm}}. Esegui questi comandi: 

    1. `bx login`
    2. `bx ic init`
    3. Copia i valori forniti per le seguenti variabili di ambiente: DOCKER_HOST, DOCKER_CERT_PATH e DOCKER_TLS_VERIFY. 
    4. Sovrascrivi l'ambiente Docker locale configurando le seguenti variabili per il collegamento a {{site.data.keyword.containershort}}: 
        
        `export DOCKER_HOST=<your_host_value>`
        
        `export DOCKER_CERT_PATH=<your_cert_path_value>`
        
        `export DOCKER_TLS_VERIFY=1`

3.	Allega una sessione bash al tuo contenitore:

    `bx ic exec -it container_name /bin/bash`
    
    dove container_name è il nome del contenitore. 
    
**Nota:** questa opzione supporta solo alcuni comandi Docker. 




### Modifica i valori di installazione predefiniti 
{: #ibm_mq_dev_default}

Quando si crea il gestore code, questo viene creato con le impostazioni predefinite di {{site.data.keyword.IBM_notm}} MQ Developer. Questi oggetti predefiniti sono progettati per aiutarti a iniziare rapidamente a sviluppare applicazioni o a provare {{site.data.keyword.IBM_notm}} MQ.  

Il gestore code verrà creato con i seguenti oggetti predefiniti: 

* Un utente amministratore MQ **admin** con la password *passw0rd*. 
*	Un utente applicazione MQ **app** senza una password. 
*	Un listener **DEV.LISTENER.TCP** configurato per collegarsi alla porta 1414/TCP. 
*	Un canale **DEV.APP.SVRCONN** configurato per gestire le connessioni dell'applicazione client MQ. 
*	Un canale **DEV.ADMIN.SVRCONN** configurato per gestire le connessioni di amministratore MQ. 
*	3 code **DEV.QUEUE.1**, **DEV.QUEUE.2**, **DEV.QUEUE.3** dove puoi inserire i messaggi. 
*	Una coda di lettere non recapitate **DEV.DEAD.LETTER.QUEUE** in cui verranno inseriti i messaggi non recapitabili. 
*	Un argomento di base **DEV.BASE.TOPIC** con la stringa di argomento */dev*. 
*	Un record di autenticazione di canale per bloccare tutte le connessioni client a meno che non si stiano connettendo attraverso il canale **DEV.APP.SVRCONN** o **DEV.ADMIN.SVRCONN**. 
*	Un record di autenticazione di canale per bloccare tutte le connessioni degli utenti amministratore a meno che non si stiano connettendo attraverso il canale **DEV.ADMIN.SVRCONN**. 
*	Un record di autenticazione di canale per consentire solo all'utente amministratore di connettersi al canale **DEV.ADMIN.SVRCONN**. 
*	Autenticazione della connessione impostata per richiedere alle applicazioni di amministrazione di trasmettere credenziali valide durante la connessione e di adottare tale ID utente per i controlli di autorizzazione. 

Con questi oggetti predefiniti, puoi connettere un'applicazione client in grado di inserire e ricevere messaggi in una coda oppure sottoscrivere e pubblicare nell'argomento. 

Se vuoi personalizzare le impostazioni predefinite dello sviluppatore che vengono create, puoi aggiungere le seguenti variabili di ambiente quando crei il contenitore dall'immagine ibm-mq. 

| Variabile di ambiente | Scopo | Valore predefinito |
|----------------------|---------|---------|
| MQ_ADMIN_PASSWORD | Imposta questa variabile per modificare la password dell'amministratore MQ creato dall'impostazione predefinita a quella di tua scelta. La password deve contenere almeno 8 caratteri. Il nome utente per l'utente amministratore è fissato come admin. | passw0rd |
| MQ_APP_PASSWORD | Imposta questa variabile per impostare la password per l'utente dell'applicazione. Il nome utente per l'utente dell'applicazione è fissato come app. Se imposti questa variabile di ambiente, devi fornire l'ID utente e la password quando connetti i client MQ. <br> La password deve contenere almeno 8 caratteri. |  |
| MQ_TLS_KEYSTORE | Imposta questa variabile di ambiente sul percorso di un keystore PKCS#12 contenente il certificato che vuoi venga utilizzato dalla console web MQ e dal gestore code IBM MQ per le operazioni TLS. <br> Se questa variabile di ambiente è impostata, i canali dello sviluppatore creati verranno abilitati per TLS utilizzando la CipherSpec ‘TLS_RSA_WITH_AES_256_GCM_SHA384’. <br> **Nota:** devi rendere disponibile il file del keystore all'interno del tuo contenitore {{site.data.keyword.Bluemix_notm}}. Per fare ciò, puoi montare un volume contenente il tuo keystore quando crei il contenitore. |  |
| MQ_TLS_PASSPHRASE | Imposta questa variabile di ambiente per impostare la password per il keystore specificato nella variabile di ambiente MQ_TLS_KEYSTORE. |  |
| MQ_DEV | Imposta questa variabile di ambiente per controllare se vengono creati gli oggetti predefiniti di {{site.data.keyword.IBM_notm}} MQ Developer. | true |
| MQ_DISABLE_WEB_CONSOLE | Imposta questa variabile di ambiente per disabilitare la configurazione e l'avvio della console web se vuoi conservare l'utilizzo della CPU/RAM del tuo contenitore. |  |



