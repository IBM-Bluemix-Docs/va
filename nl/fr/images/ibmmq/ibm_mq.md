---

copyright:
  years: 2015, 2017

lastupdated: "2017-03-07"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}

# Image IBM MQ Bluemix Container
{: #ibm_mq}

L'image ibm-mq est fournie pour {{site.data.keyword.containershort}}. Elle inclut {{site.data.keyword.IBM}} MQ Advanced for Developers (non garanti). 

{:shortdesc}


## Fonctionnement
{: #ibm_mq_how}

{{site.data.keyword.IBM_notm}} MQ Advanced for Developers contient tout ce dont vous avez besoin pour commencer à développer vos propres solutions de messagerie et applications {{site.data.keyword.IBM_notm}} MQ. 

Pour développer des solutions de messagerie et des applications {{site.data.keyword.IBM_notm}} MQ dans {{site.data.keyword.Bluemix_notm}}, vous pouvez déployer l'image Docker ibm-mq dans {{site.data.keyword.containershort}}. Vous pouvez ensuite créer et exécuter des gestionnaires de files d'attente d'exécution, et utilisez l'interface utilisateur Web ou un terminal pour les configurer de manière à répondre aux besoins de votre application ou de votre solution de messagerie.

**Remarque :** Vous pouvez utiliser l'image ibm-mq image uniquement à des fins de développement et de test unitaire. Vous pouvez également utiliser l'image pour explorer le produit, dérouler des tutoriels et évaluer la contribution d'{{site.data.keyword.IBM_notm}} MQ à votre organisation.

## Eléments inclus
{: #ibm_mq_what}

Cette image ibm-mq contient le package logiciel pour {{site.data.keyword.IBM_notm}} MQ Advanced for Developers version 9.0.x, version complète du produit que vous pouvez utiliser pour le développement et les tests unitaires. Vous pouvez télécharger gratuitement cette version et l'utiliser conformément aux dispositions de la licence.

{{site.data.keyword.IBM_notm}} MQ Advanced for Developers est disponible sur les systèmes d'exploitation Windows 64 bits et Linux x86-64. Tous les services prérequis du produit sont inclus dans le package de téléchargement. Pour plus d'informations, voir le document [IBM MQ Downloads ![Icône de lien externe](../../../../icons/launch-glyph.svg "Icône de lien externe")](http://www-03.ibm.com/software/products/lv/ibm-mq-advanced-for-developers){: new_window}

Pour plus d'informations sur les fonctions incluses dans {{site.data.keyword.IBM_notm}} MQ Advanced for Developers, voir [IBM MQ ![Icône de lien externe](../../../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/support/knowledgecenter/SSFKSJ_9.0.0/com.ibm.mq.helphome.v90.doc/WelcomePagev9r0.htm){: new_window}



## Restrictions d'utilisation
{: #ibm_mq_usage}

Le tableau suivant présente les restrictions qui s'appliquent à l'utilisation gratuite de l'image ibm-mq dans {{site.data.keyword.Bluemix_notm}} :

| Environnement	| Restrictions quant à l'utilisation gratuite |
|-------------|-------------------------|
| Développement	| Utilisation gratuite illimitée de l'image ibm-mq pour créer des conteneurs uniques à des fins de développement et de test unitaire. |
| Test | Utilisation gratuite illimitée de l'image ibm-mq pour créer des conteneurs uniques à des fins de développement et de test unitaire. Consultez la rubrique [Informations sur la licence](getstarted.html#ibm_mq_license ) pour en savoir plus sur les tests autorisés. |
| Production | La licence {{site.data.keyword.IBM_notm}} MQ Advanced for Developer ne permet pas son utilisation à des fins de production. |

**Remarque :** La tarification de l'image ibm-mq est indépendante de celle des conteneurs que vous utilisez dans {{site.data.keyword.Bluemix_notm}}.


## Informations sur la licence
{: #ibm_mq_license}

Informations sur les licences :

*	Les documents Dockerfile et les scripts qui leur sont associés sous licence [Apache License 2.0 ![Icône de lien externe](../../../../icons/launch-glyph.svg "Icône de lien externe")](http://www.apache.org/licenses/LICENSE-2.0.html){: new_window}.
* [Informations sur la licence pour {{site.data.keyword.IBM_notm}} MQ Advanced for Developers ![Icône de lien externe](../../../../icons/launch-glyph.svg "Icône de lien externe")](http://www-03.ibm.com/software/sla/sladb.nsf/displaylis/973FA648A5DE42948525806E004CC757?OpenDocument){: new_window}.

**Remarque :** La licence ne permet pas de distribution supplémentaire. Les dispositions relatives à {{site.data.keyword.IBM_notm}} MQ dans l'image limitent l'utilisation à des fins de développement et de test unitaire.


## Premiers pas
{: #ibm_mq_get_started}

Procédez comme indiqué ci-après pour exécuter et gérer {{site.data.keyword.IBM_notm}} MQ dans un conteneur s'exécutant sous {{site.data.keyword.Bluemix_notm}} :

1. [Mise à disposition d'un conteneur basé sur l'image ibm-mq](ibm_mq.html#ibm_mq_provision)

    **Remarque :** Le conteneur est lié à l'état en cours du gestionnaire de file d'attente à l'intérieur de celui-ci. Si vous arrêtez le gestionnaire de file d'attente à l'aide de la console ou de la ligne de commande, le conteneur s'arrêtera peu de temps après. 

2. Gestion des ressources {{site.data.keyword.IBM_notm}} MQ qui s'exécutent dans un conteneur. Vous pouvez procéder de manière graphique à partir de la console Web IBM MQ ou à l'aide d'un programme à partir d'un terminal. 

    1. Choisissez l'une des options suivantes pour gérer les ressources {{site.data.keyword.IBM_notm}} MQ dans le conteneur :
    
        *	Lancez la console Web {{site.data.keyword.IBM_notm}} MQ et gérez {{site.data.keyword.IBM_notm}} MQ sous forme graphique. Pour plus d'informations, voir [Lancement de la console Web IBM MQ](ibm_mq.html#ibm_mq_webui).
        *	A partir d'un terminal, utilisez l'interface de ligne de commande Docker. Pour plus d'informations, voir [Configuration d'un terminal afin qu'il utilise l'interface de ligne de commande Docker](ibm_mq.html#ibm_mq_docker_commands).
        * A partir d'un terminal, utilisez l'interface de ligne de commande d'IBM Bluemix Container Service. Pour plus d'informations, voir [Configuration d'un terminal afin qu'il utilise l'interface de ligne de commande IBM Bluemix Container Service](ibm_mq.html#ibm_mq_containers_cli)
        
    2. Configurez le gestionnaire de file d'attente {{site.data.keyword.IBM_notm}} MQ qui s'exécute dans le conteneur. 

        * Pour gérer le gestionnaire de file d'attente à l'aide de la ligne de commande, utilisez le programme *runmqsc*. Exécutez la commande suivante : `runmqsc QM_Name` où *QM_Name* désigne le nom de votre gestionnaire de files d'attente. Pour plus d'informations, voir [Gestion de votre conteneur à l'aide de la ligne de commande](ibm_mq.html#ibm_mq_access).
    
        * Pour gérer le gestionnaire de files d'attente sous forme graphique, utilisez l'interface utilisateur Web.
        
    3. Suivez les instructions dans [IBM MQ Knowledge Center ![Icône de lien externe](../../../../icons/launch-glyph.svg "Icône de lien externe")](http://www.ibm.com/support/knowledgecenter/en/SSFKSJ_9.0.0/com.ibm.mq.helphome.v90.doc/WelcomePagev9r0.htm){: new_window} pour créer des files d'attente de messages sur votre gestionnaire de files d'attente et placer ou récupérer des messages dans ces files.
    
        Par exemple, exécutez dans `runmqsc` la commande suivante pour créer une file d'attente locale : `DEFINE QLOCAL('MY.Q')`, où *MY.Q* désigne le nom de la file d'attente locale sur le terminal. Vous pouvez consulter cette file d'attente dans la console Web ou vous pouvez afficher la file d'attente et ses propriétés en exécutant dans 'runmqsc` la commande suivante : `DISPLAY QLOCAL('MY.Q')`.
    
3. [Facultatif] Surveillance des journaux {{site.data.keyword.IBM_notm}} MQ dans le conteneur. Pour plus d'informations, voir [Gestion de votre conteneur à l'aide de la ligne de commande](ibm_mq.html#ibm_mq_access).

Pour en savoir plus sur l'introduction de mise en route d'IBM MQ, voir [IBM MQ Version 9.0.x Welcome page ![Icône de lien externe](../../../../icons/launch-glyph.svg "Icône de lien externe")](http://www.ibm.com/support/knowledgecenter/en/SSFKSJ_9.0.0/com.ibm.mq.helphome.v90.doc/WelcomePagev9r0.htm){: new_window}

Pour toute information sur l'image et la façon de créer votre propre image localement dans Docker, voir [la page MQ Docker GitHub ![Icône de lien externe](../../../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/ibm-messaging/mq-docker){: new_window}
    
Pour les informations et blogs les plus récents sur IBM MQ, voir [The MQ Developer works blog ![Icône de lien externe](../../../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/developerworks/community/blogs/messaging){: new_window}


### Mise à disposition d'un conteneur basé sur l'image ibm-mq
{: #ibm_mq_provision}

Procédez comme indiqué ci-après pour mettre à disposition un conteneur Docker dans {{site.data.keyword.Bluemix_notm}} basé sur l'image ibm-mq image fournie par {{site.data.keyword.IBM_notm}} :

1.	Connectez-vous à {{site.data.keyword.Bluemix_notm}}. Connectez-vous à votre ID {{site.data.keyword.Bluemix_notm}}.
2.	Dans le catalogue, sélectionnez **Conteneurs** et choisissez l'image *ibm-mq*.
3.	Sélectionnez **Unique** pour créer un conteneur d'instance unique, lequel peut être utilisé à des fins de développement et de test.
4.	Entrez le nom du conteneur, par exemple mq.
5.	Sélectionnez la taille du conteneur.
6.	Dans la zone Adresse IP publique, sélectionnez **Demander et lier une adresse IP publique**.
7.	Dans la zone Ports publics, spécifiez **1414/tcp, 9443/tcp**.
8.	Développez les options avancées, cliquez sur **Ajoutez une nouvelle variable d'environnement**, et entrez les variables d'environnement suivantes : LICENSE et MQ_QMGR_NAME.

    * Définissez la valeur de **LICENSE** sur **accept** pour accepter les dispositions du contrat de licence IBM MQ Advanced for Developers. 
    
        Si vous voulez consulter la licence avant d'en accepter les termes, définissez la variable LICENSE sur “view” pour que la licence soit imprimée dans les journaux du conteneur. 
    
        Si votre langue principale n'est pas l'anglais, vous pouvez consulter le fichier de licence dans la langue de votre choix, en définissant la variable d'environnement **LANG** sur l'une des valeurs suivantes : *zh_TW* pour le chinois traditionnel, *zh* pour le chinois, *cs* pour le tchèque, *en* pour l'anglais, *fr* pour le français, *de* pour l'allemand, *el* pour le grec, *id* pour l'indonésien, *it* pour l'italien, *ja* pour le japonais, *ko* pour le coréen, *lt* pour le lituanien, *pl* pour le polonais, *pt* pour le portugais, *ru* pour le russe, *sl* pour le slovène, *es* pour l'espagnol ou *tr* pour le turc.

    * Définissez la valeur **MQ_QMGR_NAME** sur *MYMQ* où *MYMQ* est le nom du gestionnaire de files d'attente sélectionné.
 
        **Remarque :** Si vous ne spécifiez pas de valeurs pour les variables d'environnement LICENSE et MQ_QMGR_NAME, le conteneur est créé, mais ne démarre pas. 
    
    * [Facultatif] Par défaut, la console Web {{site.data.keyword.IBM_notm}} MQ est lancée dans le conteneur. Pour modifier ce comportement par défaut, définissez la variable d'environnement **MQ_DISABLE_WEB_CONSOLE** sur *true*.
    
        **Remarque :** Pour utiliser une console Web, ne définissez pas la variable d'environnement *MQ_DISABLE_WEB_CONSOLE*.

    * [Facultatif] Pour visualiser les journaux MQ via l'interface utilisateur {{site.data.keyword.Bluemix_notm}}, définissez la variable d'environnement **LOG_LOCATIONS** en lui affectant la valeur `/var/mqm/qmgr/QM_Name/errors/AMQERR01.LOG`, où *QM_Name* est la valeur définie pour la variable d'environnement *MQ_QMGR_NAME*.
    
    *	[Facultatif] Par défaut, les objets {{site.data.keyword.IBM_notm}} MQ Developer par défaut sont créés. Pour modifier le comportement par défaut, définissez la variable d'environnement **MQ_DEV** sur *false*, et remplacez les valeurs d'installation par défaut d'IBM MQ par vos propres valeurs. Pour plus d'informations, voir [Modification des valeurs d'installation par défaut](ibm_mq.html#ibm_mq_dev_default)
 
9. [Facultatif] Dans la section Options avancées, affectez un volume. Utilisez des volumes {{site.data.keyword.Bluemix_notm}} pour le stockage des données {{site.data.keyword.IBM_notm}} MQ et de la configuration de manière durable pendant les migrations de conteneur. Les données {{site.data.keyword.IBM_notm}} MQ qui ne sont pas stockées dans un volume seront perdues en cas d'arrêt d'un conteneur.

    L'image ibm-mq peut être utilisée avec des volumes {{site.data.keyword.Bluemix_notm}} à condition que les volumes soient montés vers **/mnt/mqm** afin d'être détectés et utilisés par {{site.data.keyword.IBM_notm}} MQ.

    Par défaut, tous les conteneurs créés sans volume perdent leurs données IBM MQ en cas d'arrêt du conteneur. Pour que cela ne se produise pas, utilisez une messagerie persistante et des volumes {{site.data.keyword.Bluemix_notm}}.

    1. Cliquez sur **Créer un volume**, entrez un nom de volume et sélectionnez un partage de fichiers.
    2. Cliquez sur **Affecter un volume existant** et sélectionnez le volume que vous avez créé. Entrez **/mnt/mqm** dans la zone du chemin d'accès. Assurez-vous que la case **En lecture seule** n'est pas cochée.

    **Remarque :** Lorsque vous montez un volume à des fins d'utilisation avec le répertoire de données IBM MQ, montez-le vers */mnt/mqm*. En le montant vers */mnt/mqm*, le script ibm-mq détecte le volume et configure le système de fichiers IBM MQ de manière à l'utiliser.
     
10.	Cliquez sur **Créer**, puis attendez que le conteneur commence à s'exécuter.

    Une fois le conteneur créé, le tableau de bord du conteneur s'ouvre. Vérifiez que le statut du conteneur indique bien **En cours d'exécution**. 

    Pour consulter les journaux de votre conteneur, cliquez sur **Surveillance et journaux** et sélectionnez l'onglet **Journalisation**. Les journaux s'affichent. Pour une analyse avancée de vos journaux, cliquez sur **Vue avancée** afin d'afficher vos journaux dans Kibana.

Vous avez créé et démarré un gestionnaire de files d'attente. Vous pouvez maintenant connecter une application MQ avec les informations de connexion suivantes :

| Propriété de connexion | Valeur |
|-------|-------|
| Adresse IP   | Adresse IP publique du conteneur |
| Port | 1414 |
| Canal | DEV.APP.SVRCONN |


### Gestion de votre conteneur à l'aide de la ligne de commande
{: #ibm_mq_access}

Une fois que le conteneur qui exécute {{site.data.keyword.IBM_notm}} MQ est déployé dans {{site.data.keyword.Bluemix_notm}}, vous pouvez accéder à votre conteneur et à son statut et valider l'installation {{site.data.keyword.IBM_notm}} MQ.

Procédez comme suit pour vérifier l'installation et la configuration d'{{site.data.keyword.IBM_notm}} MQ dans le conteneur :

1.	Configurez l'interface de ligne de commande (CLI) d'IBM Containers. Pour plus d'informations, reportez-vous à la page [Initiation à l'interface de ligne de commande Bluemix](https://plugins.ng.bluemix.net/ui/home.html) et suivez les instructions pour installer le plug-in le plus récent du service Cloud Foundry Container.

2.	A partir d'un terminal, connectez-vous à votre organisation et espace {{site.data.keyword.Bluemix_notm}} où le conteneur s'exécute avec votre ID {{site.data.keyword.Bluemix_notm}}. Exécutez les commandes suivantes :

    1. `  bx login
  `
    2. `bx ic init`
    
3.	Vérifiez le statut du conteneur. Exécutez la commande suivante : `bx ic ps`

4.	Associez une session Bash à votre conteneur :

    `bx ic exec -it container_name /bin/bash`
    
    où container_name désigne le nom du conteneur
    
5.	Exécutez des commandes pour gérer votre installation MQ, par exemple :

| Commande | Action |
|---------|---------|
| dspmqver | Vérifie la version IBM MQ et les informations de génération. |
| dspmq | Vérifie le statut du gestionnaire de files d'attente qui s'exécute dans le conteneur. | 
| runmqsc | Gère les ressources MQ. |

Pour surveiller les journaux MQ, choisissez l'une des options suivantes :

* Si votre terminal est configuré pour exécuter des commandes d'interface de ligne de commande {{site.data.keyword.containershort}}, exécutez la commande suivante : `bx ic exec container_id tail -f /var/mqm/qmgr/QM_ Name/errors/AMQERR01.LOG` où *container_id* est le nom de votre conteneur et *QM_ Name* celui de votre gestionnaire de files d'attente.
    
* Si votre terminal est configuré pour exécuter des commandes d'interface de ligne de commande Docker, exécutez la commande suivante : `docker exec container_id tail -f /var/mqm/qmgr/QM_Name/errors/AMQERR01.LOG` où *container_id* est le nom de votre conteneur et *QM_Name* celui de votre gestionnaire de files d'attente.
    
    

### Lancement de la console Web IBM MQ
{: #ibm_mq_webui}

Procédez comme indiqué ci-après pour lancer la console Web {{site.data.keyword.IBM_notm}} MQ :

1.	Dans le tableau de bord {{site.data.keyword.Bluemix_notm}}, sélectionnez votre conteneur et vérifiez que son statut est défini sur *En cours d'exécution*.
2.	Vérifiez que votre conteneur est bien configuré sur le port public 9443.
3.	Recherchez l'adresse IP publique dans les détails du conteneur et demandez et liez une adresse si besoin.
4.	A partir d'un navigateur Web, entrez l'adresse URL suivante : `https://DockerContainerPublicIP:9443/ibmmq/console/` où DockerContainerPublicIP correspond à l'adresse IP publique de votre conteneur.

    Un avertissement de sécurité s'affiche, car le certificat TLS utilisé par le serveur Web lors de l'hébergement de la console MQ est généré et auto-signé. Cliquez sur **Ajouter une exception** et confirmez.

    **Remarque :** Dans un environnement de production, utilisez votre propre certificat qui est reconnu et considéré comme digne de confiance par votre navigateur. Ce certificat doit avoir été émis par une autorité de certification. 
    
    Le navigateur ouvre l'interface utilisateur Web.
    
5. Connectez-vous à la console {{site.data.keyword.IBM_notm}} MQ à l'aide des données d'identification suivantes :

    1.	Utilisateur : admin
    2.	Mot de passe : passw0rd
    
Vous êtes maintenant prêt à administrer IBM MQ.

### Configuration d'un terminal afin qu'il utilise l'interface de ligne de commande d'IBM Bluemix Container Service
{: #ibm_mq_containers_cli}

Utilisez l'interface de ligne de commande {{site.data.keyword.containershort}} pour exécuter les commandes d'administration {{site.data.keyword.IBM_notm}} MQ directement dans un conteneur.

Procédez comme suit pour configurer un terminal afin d'exécuter des commandes bx ic pour gérer IBM MQ :

1.	Installez l'interface de ligne de commande {{site.data.keyword.containershort}}. Pour plus d'informations, reportez-vous à la page [Initiation à l'interface de ligne de commande Bluemix](https://plugins.ng.bluemix.net/ui/home.html) et suivez les instructions pour installer le plug-in le plus récent du service Cloud Foundry Container.

2.	A partir d'un terminal, connectez-vous à {{site.data.keyword.Bluemix_notm}}. Exécutez les commandes suivantes :

    1. `  bx login
  `
    2. `bx ic init`
    
3.	Associez une session Bash à votre conteneur :

    `bx ic exec -it container_name /bin/bash`
    
    où container_name désigne le nom du conteneur
    


### Configuration d'un terminal afin qu'il utilise l'interface de ligne de commande Docker
{: #ibm_mq_docker_commands}

Utilisez l'interface de ligne de commande Docker pour exécuter des commandes d'administration {{site.data.keyword.IBM_notm}} MQ directement dans un conteneur.

Procédez comme indiqué ci-après pour configurer un terminal afin qu'il exécute des commandes Docker pour gérer IBM MQ :

1.	Installez l'interface de ligne de commande {{site.data.keyword.containershort}}. Pour plus d'informations, reportez-vous à la page [Initiation à l'interface de ligne de commande Bluemix](https://plugins.ng.bluemix.net/ui/home.html) et suivez les instructions pour installer le plug-in le plus récent du service Cloud Foundry Container.

2.	A partir d'un terminal, connectez-vous à {{site.data.keyword.Bluemix_notm}}. Exécutez les commandes suivantes :

    1. `  bx login
  `
    2. `bx ic init`
    3. Copiez les valeurs qui sont fournies pour les variables d'environnement suivantes : DOCKER_HOST, DOCKER_CERT_PATH et DOCKER_TLS_VERIFY.
    4. Supplantez l'environnement Docker local en définissant les variables suivantes pour vous connecter à {{site.data.keyword.containershort}} :
        
        `export DOCKER_HOST=<your_host_value>`
        
        `export DOCKER_CERT_PATH=<your_cert_path_value>`
        
        `export DOCKER_TLS_VERIFY=1`

3.	Associez une session Bash à votre conteneur :

    `bx ic exec -it container_name /bin/bash`
    
    où container_name désigne le nom du conteneur
    
**Remarque :** Cette option ne prend en charge que certaines commandes Docker.




### Modification des valeurs d'installation par défaut
{: #ibm_mq_dev_default}

Lorsque le gestionnaire de files d'attente est créé, il est créé avec les objets par défaut d'{{site.data.keyword.IBM_notm}} MQ Developer. Ces objets par défaut sont conçus pour vous aider à commencer rapidement à développer des applications ou à tester {{site.data.keyword.IBM_notm}} MQ. 

Le gestionnaire de files d'attente est créé avec les objets par défaut suivants :

* Un utilisateur administrateur MQ **admin** avec un mot de passe *passw0rd*.
*	Un utilisateur d'application MQ **app** sans mot de passe.
*	Un programme d'écoute **DEV.LISTENER.TCP** configuré pour se lier au port 1414/TCP.
*	Un canal **DEV.APP.SVRCONN** configuré pour gérer les connexions d'application client MQ.
*	Un canal **DEV.ADMIN.SVRCONN** configuré pour gérer les connexions admin MQ.
*	3 files d'attentes **DEV.QUEUE.1**, **DEV.QUEUE.2**, **DEV.QUEUE.3** dans lesquelles vous pouvez placer des messages.
*	Une file d'attente de rebut **DEV.DEAD.LETTER.QUEUE** dans laquelle sont placés les messages non livrés.
*	Une rubrique de base **DEV.BASE.TOPIC** avec la chaîne de rubrique */dev*.
*	Un enregistrement d'authentification de canal qui bloque toutes les connexions client qui ne se connectent pas via le canal **DEV.APP.SVRCONN** ou **DEV.ADMIN.SVRCONN**.
*	Un enregistrement d'authentification de canal qui bloque tous les administrateurs qui se connectent à l'exception de ceux qui se connectent via le canal **DEV.ADMIN.SVRCONN**.
*	Un enregistrement d'authentification de canal qui autorise les administrateurs à se connecter uniquement au canal **DEV.ADMIN.SVRCONN**.
*	Authentification de connexion configurée de manière à exiger que les applications admin fournissent des données d'identification valides au moment de la connexion et utilisent cet ID utilisateur pour la vérification des droits.

Grâce à ces objets par défaut, vous pouvez connecter une application client qui peut placer et récupérer des messages dans une file d'attente, ou vous abonner et publier dans une rubrique.

Si vous souhaitez personnaliser les objets par défaut, ajoutez les variables d'environnement suivantes lorsque vous créez votre conteneur à partir de l'image ibm-mq.

| Variable d'environnement | Fonction | Valeur par défaut |
|----------------------|---------|---------|
| MQ_ADMIN_PASSWORD | Définissez cette variable pour modifier le mot de passe de l'administrateur MQ créé par défaut sur une valeur de votre choix. Le mot de passe doit contenir au minimum 8 caractères. Le nom d'utilisateur de l'administrateur est admin. | passw0rd |
| MQ_APP_PASSWORD | Définissez cette variable pour configurer le mot de passe de l'utilisateur d'application. Le nom d'utilisateur de l'utilisateur d'application est app. Si vous définissez cette variable d'environnement, vous devez ensuite fournir l'ID utilisateur et le mot de passe correspondant lorsque vous connectez des clients MQ. <br> Le mot de passe doit contenir au minimum 8 caractères. |  |
| MQ_TLS_KEYSTORE | Définissez cette variable d'environnement sur l'emplacement d'un magasin de clés PKCS#12 qui contient le certificat que doit utiliser la console Web et le gestionnaire de files d'attente MQ pour les opérations TLS. <br> Lorsque cette variable d'environnement est définie, les canaux de développement créés sont activés pour le protocole TLS à l'aide de CipherSpec ‘TLS_RSA_WITH_AES_256_GCM_SHA384’. <br> **Remarque :** Vous devez rendre le fichier de clés disponible dans votre conteneur {{site.data.keyword.Bluemix_notm}}. Pour ce faire, vous pouvez monter un volume qui contient votre magasin de clés lorsque vous créez le conteneur. |  |
| MQ_TLS_PASSPHRASE | Définissez cette variable d'environnement pour configurer le mot de passe du magasin de clés spécifié dans la variable d'environnement MQ_TLS_KEYSTORE. |  |
| MQ_DEV | Définissez cette variable d'environnement pour vérifier que les objets {{site.data.keyword.IBM_notm}} MQ Developer par défaut ont bien été créés. | true |
| MQ_DISABLE_WEB_CONSOLE | Définissez cette variable d'environnement pour désactiver la configuration et le démarrage de la console Web si vous souhaitez conserver l'utilisation de l'unité centrale/la mémoire RAM de votre conteneur. |  |



