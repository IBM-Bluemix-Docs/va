---

copyright:
  years: 2017, 2018
lastupdated: "2018-07-24"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Gestion de la sécurité des images avec Vulnerability Advisor
{: #va_index}

L'assistant de détection des vulnérabilités (Vulnerability Advisor) vérifie le statut de sécurité des images de conteneur fournies par {{site.data.keyword.IBM}}, des tiers, ou qui sont ajoutées aux espaces de nom de registre de votre organisation.
{:shortdesc}

Lorsque vous ajoutez une image à un espace de nom, cette image est automatiquement analysée par Vulnerability Advisor pour détecter des problèmes de sécurité et des vulnérabilités potentielles. Si des problèmes de sécurité sont identifiés, desinstructions vous sont soumises afin de vous aider à résoudre la vulnérabilité signalée. 

Vulnerability Advisor fournit la gestion de la sécurité pour {{site.data.keyword.registrylong_notm}} en générant un rapport de statut de sécurité qui inclut des correctifs suggérés et les meilleures pratiques.  

Les problèmes détectés génèrent un verdict stipulant qu'il n'est pas recommandé de déployer cette image. Si vous choisissez de déployer l'image, les conteneurs qui sont déployés à partir de celle-ci comportent des problèmes connus qui peuvent être utilisés pour attaquer ou compromettre le conteneur. Vulnerability Advisor ajuste son verdict en fonction des exemptions que vous avez spécifiées. Ce verdict peut être utilisé par le programme Image Security Enforcement afin d'empêcher le déploiement d'images non sécurisées dans {{site.data.keyword.containerlong_notm}}. 

La correction des problèmes de sécurité et de configuration signalés par Vulnerability Advisor peut vous aider à sécuriser votre infrastructure {{site.data.keyword.cloud_notm}}.


## A propos de Vulnerability Advisor
{: #about}

Vulnerability Advisor fournit des fonctions vous permettant de sécuriser vos images. 

{:shortdesc}

 Les fonctions disponibles sont les suivantes :

-   Analyse les images pour détection d'images
-   Analyse les conteneurs actifs pour détection de problèmes si vous avez [installé le scanner de conteneur](#va_install_container_scanner) dans chaque cluster
-   Fournit un rapport d'évaluation basé sur les pratiques de sécurité spécifiques à {{site.data.keyword.containerlong_notm}}
-   Soumet des recommandations pour sécuriser les fichiers de configuration d'un sous-ensemble de fichiers d'application
-   Fournit des instructions pour la correction d'un [package vulnérable](#packages) ou d'un [problème de configuration](#app_configurations) signalé dans son rapport.
-   Fournit des verdicts au programme [Image Security Enforcement](../Registry/registry_security_enforce.html#security_enforce)
-   Applique des exemptions à des rapports au niveau compte, espace de nom, référentiel ou balise afin de signaler que les problèmes qui sont marqués ne s'appliquent pas à votre cas d'utilisation
-   Fournit des liens vers des conteneurs associés depuis la vue **Balise** de l'interface graphique {{site.data.keyword.registrylong_notm}}. Vous pouvez répertorier les conteneurs qui sont en cours d'exécution et qui utilisent cette image dans un cluster dans lequel le scanner de conteneur est installé.


Dans le tableau de bord Registre, la colonne **Statut de règle** indique le statut de vos référentiels. Le rapport lié identifie les pratiques de sécurité de cloud souhaitables pour vos images. 

Le tableau de bord Vulnerability Advisor fournit une présentation et une évaluation de la sécurité d'une image et, si le scanner de conteneur est installé, des liens vers des conteneurs en cours d'exécution. Pour plus d'informations sur le tableau de bord Vulnerability Advisor, voir [Examen d'un rapport de vulnérabilité](#va_reviewing).
	
	
**Protection des données**

Pour analyser des images et des conteneurs de votre compte afin de détecter les problèmes de sécurité, Vulnerability Advisor collecte, stocke et traite les informations suivantes :
- Zones de texte à format libre, notamment les ID, descriptions et noms d'image (registre, espace de nom, nom de référentiel et étiquettes d'image)
- Métadonnées Kubernetes, notamment les noms de ressources Kubernetes telles que pod, jeu de répliques (replicaset) et noms de déploiement (deployment)
- Métadonnées relatives aux modes de fichier et horodatages de création des fichiers de configuration
- Contenu des fichiers de configuration système et d'application dans les images et les conteneurs
- Packages et bibliothèques installés (y compris leurs versions)

Ne placez pas d'informations personnelles dans une zone ou un emplacement traité par Vulnerability Advisor, comme identifiées dans la liste précédente.

Les résultats d'analyse, agrégés au niveau d'un centre de données, sont traités afin de fournir des métriques anonymisées pour faire fonctionner et améliorer le service.

Les résultats d'analyse sont supprimés 30 jours après leur génération.


## Types de vulnérabilités
{: #types}


### Packages vulnérables
{: #packages}

Vulnerability Advisor recherche des packages vulnérables dans les images basées sur des systèmes d'exploitation pris en charge et fournit un lien vers les éventuels bulletins de sécurité pertinents concernant la vulnérabilité concernée.
{:shortdesc}

Les packages comportant des problèmes de vulnérabilité connus sont affichés dans les résultats d'analyse. Les vulnérabilités éventuelles sont mises à jour quotidiennement à partir des avis de sécurité publiés pour les images Docker répertoriées dans le tableau suivant. Généralement, pour qu'un package vulnérable réussisse l'examen, une version plus récente de ce package incluant un correctif pour la vulnérabilité est requise. Un même package peut recenser plusieurs vulnérabilités et, dans ce cas, une seule mise à jour du package peut résoudre plusieurs vulnérabilités.


  |Image de base Docker|Source des avis de sécurité|
  |-----------------|--------------------------|
  |Alpine|[Git - Alpine Linux ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://git.alpinelinux.org/) et [CIRCL CVE Search ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://cve.circl.lu/)|
  |CentOS| [CentOS announce archives ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://lists.centos.org/pipermail/centos-announce/) et [CentOS CR announce archives ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://lists.centos.org/pipermail/centos-cr-announce/)|
  |Debian|[Debian security announcements ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://lists.debian.org/debian-security-announce/)|
  |Red Hat Enterprise Linux (RHEL)|[Red Hat Product Errata ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://access.redhat.com/errata/#/)|
  |Ubuntu|[Ubuntu Security Notices ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://usn.ubuntu.com/)|
  {: caption="Tableau 1. Images de base Docker prises en charge et pour lesquelles Vulnerability Advisor recherche les packages vulnérables" caption-side="top"}


### Problèmes de configuration
{: #app_configurations}

Les problèmes de configuration peuvent constituer des risques de sécurité liés à la façon dont une application est configurée. Un grand nombre de problèmes signalés peuvent être corrigés en mettant à jour votre fichier Dockerfile.
{:shortdesc}

Les images ne sont analysées que si elles sont basées sur un système d'exploitation pris en charge par Vulnerability Advisor. Vulnerability Advisor vérifie les paramètres de configuration pour les types d'application suivants :
-   MySQL
-   NGINX
-   Apache




## Installation du scanner de conteneur
{: #va_install_container_scanner}

Avant de commencer :

1.  Connectez-vous au client d'interface CLI {{site.data.keyword.Bluemix_notm}}. Si vous disposez d'un compte fédéré, utilisez `--sso`.
2.  [Ciblez votre interface CLI `kubectl`](../../containers/cs_cli_install.html#cs_cli_configure) sur le cluster qui doit utiliser une charte Helm.
3.  Créez un ID service et une clé d'API pour le scanner de conteneur et donnez-lui un nom :
    1.  Créez un ID service en exécutant la commande suivante, en remplaçant `<scanner_serviceID>` par le nom de votre choix pour l'ID service. Notez son **CRN**.
    
        ```
    	ibmcloud iam service-id-create <scanner_serviceID>
    	```
        {: codeblock}

    2.  Créez une clé d'API de service, où `<scanner_serviceID>` est l'ID service créé à l'étape précédente et qui remplace `<scanner_APIkey_name>` par le nom de votre choix pour la clé d'API de scanner. 
    
        ```
    	ibmcloud iam service-api-key-create <scanner_APIkey_name> <scanner_serviceID>
    	```
        {: codeblock}
	
	    La clé d'API de scanner est renvoyée.
	
	    Veillez à stocker votre clé d'API de scanner de manière sécurisée car elle ne pourra pas être extraite ultérieurement.
	    {: tip}
	
    3.  Créez une règle de service accordant le rôle `Writer` (auteur).
    		
        ```
    	ibmcloud iam service-policy-create <scanner_serviceID> --resource-type scaningress --service-name container-registry --roles Writer
    	```
        {: codeblock}

Pour configurer la charte Helm :

1.  [Configurez Helm dans votre cluster](../../containers/cs_integrations.html#helm). Si vous utilisez une règle RBAC pour accorder l'accès au serveur Helm (Tiller), assurez-vous que le rôle tiller dispose d'un accès à l'ensemble des espaces de nom afin que le scanner puisse voir les conteneurs de tous les espaces de nom.

2.  Ajoutez le référentiel de charte IBM à Helm, par exemple `ibm-incubator`.

    ```
    helm repo add ibm-incubator https://registry.bluemix.net/helm/ibm-incubator
    ```
    {: pre}

3.  Sauvegardez les paramètres de configuration par défaut pour la charte Helm de scanner dans un fichier YAML local. Incluez le référentiel de charte, par exemple `ibm-incubator`, dans le chemin de la charte Helm.

    ```
    helm inspect values ibm-incubator/ibmcloud-container-scanner > config.yaml
    ```
    {: pre}

4.  Editez le fichier `config.yaml`.

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
    <caption>Description des composants du fichier YAML</caption>
    <thead>
    <th>Zone</th>
    <th>Valeur</th>
    </thead>
    <tbody>
    <tr>
    <td><code>EmitURL</code></td>
    <td>Entrez l'URL de point d'extrémité régionale de Vulnerability Advisor. Pour obtenir l'URL, exécutez <code>ibmcloud cr info</code> et extrayez l'adresse de <strong>Container Registry</strong> (registre de conteneur). Remplacez <code>registry</code> par <code>va</code>. Exemple : <code>https<span comment="make the link not a link">://va.</span>eu-gb.bluemix.net</code></td>
    </tr>
    <tr>
    <td><code>AccountID</code></td>
    <td>Remplacez par l'ID compte {{site.data.keyword.Bluemix_notm}} dans lequel se trouve votre cluster. Pour obtenir l'ID compte, exécutez <code>ibmcloud account list</code>.</td>
    </tr>
    <tr>
    <td><code>ClusterID</code></td>
    <td>Remplacez par le cluster Kubernetes dans lequel installer le scanner de conteneur. Pour répertorier les ID de cluster, exécutez <code>ibmcloud ks clusters</code>.<br> **Astuce** : n'utilisez pas le nom du cluster mais son ID.
</td>
    </tr>
    <tr>
    <td><code>APIKey</code></td>
    <td>Remplacez par la clé d'API de scanner créée précédemment.</td>
    </tr>
    </tbody></table>

5.  Installez la charte Helm sur votre cluster avec le fichier `config.yaml` mis à jour. Les propriétés mises à jour sont stockées dans un objet configmap pour votre charte. Remplacez `<myscanner>` par le nom choisi pour votre charte Helm. Incluez le référentiel de charte, par exemple `ibm-incubator`, dans le chemin de la charte Helm.

    ```
    helm install -f config.yaml --name=<myscanner> ibm-incubator/ibmcloud-container-scanner
    ```
    {: pre}
    
    Le scanner de conteneur est installé dans l'espace de nom `kube-system`, mais il analyse les conteneurs de tous les espaces de nom.{:tip}

6.  Vérifiez le statut de déploiement du diagramme. Quand la charte est prête, la zone de **STATUT** située le haut de la sortie indique la sortie `DEPLOYED` (déployé).

    ```
    helm status <myscanner>
    ```
    {: pre}

7.  Une fois la charte déployée, vérifiez que les paramètres mis à jour dans le fichier `config.yaml` ont bien été utilisés.

    ```
    helm get values <myscanner>
    ```
    {: pre}


IBM Container Scanner est à présent installé, et l'agent est déployé en tant que [DaemonSet ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) dans votre cluster. Bien que le scanner soit déployé dans l'espace de nom `kube-system`, il analyse tous les conteneurs affectés à des pods pour l'ensemble de vos espaces de nom Kubernetes, par exemple `default`. 


## Exécution d'un scanner de conteneur au travers d'un pare-feu
{: #va_firewall}

Si votre pare-feu bloque les connexions sortantes, vous devez le configurer pour qu'il autorise les noeuds worker à accéder au scanner de conteneur sur le port TCP <code>443</code> sur les adresses IP répertoriées dans le tableau suivant.
{:shortdesc}

 

<p>
  <table summary=" La lecture des lignes se fait de gauche à droite, avec la zone de serveur dans la colonne 1 et les adresses IP pour la mise en correspondance dans la colonne 2.">
  <caption>Adresses IP à ouvrir pour le trafic sortant</caption>
      <thead>
      <th>Région</th>
      <th>Adresse IP</th>
      </thead>
    <tbody>
      <tr>
         <td>Asie-Pacifique sud</td>
         <td><code>168.1.40.158</code><br><code>130.198.65.182</code></td>
      </tr>
      <tr>
         <td>Europe centrale</td>
         <td><code>159.8.220.182</code><br><code>158.177.74.102</code></td>
        </tr>
      <tr>
        <td>Sud du Royaume-Uni</td>
        <td><code>158.175.71.134</code><br><code>5.10.111.190</code></td>
      </tr>
      <tr>
        <td>Est des Etats-Unis</td>
         <td><code>169.60.73.158</code><br><code>169.61.84.102</code></td>
      </tr>
      <tr>
        <td>Etats-Unis du Sud</td>
        <td><code>169.47.103.118</code><br><code>169.48.165.6</code></td>
      </tr>
      </tbody>
    </table>
</p>


## Examen d'un rapport de vulnérabilité via l'interface graphique
{: #va_reviewing}

Avant de déployer une image, vous pouvez consulter son rapport Vulnerability Advisor afin de trouver des détails sur les éventuels packages vulnérables et les paramètres d'application non sécurisés.
{:shortdesc}



1.  Connectez-vous à {{site.data.keyword.Bluemix_notm}}.
2.  Cliquez sur **Catalogue**.
3.  Sous **Infrastructure**, cliquez sur **Conteneurs**.
4.  Cliquez sur la vignette **Container Registry**.
5.  Développez **Vulnerability Advisor** et cliquez sur **Référentiels analysés**.
6.  Pour visualiser le rapport de l'image indiquant `latest` (dernière), cliquez sur la ligne de ce référentiel. Le rapport présente le nombre total de problèmes et indique si des packages vulnérables ou des problèmes de configuration ont été détectés. Si aucune balise `latest` n'existe dans le référentiel, l'image la plus récente est utilisée.
7.  Pour afficher des informations sur chaque package vulnérable pour l'image sélectionnée, dans le tableau **Packages vulnérables détectés**, cliquez sur la colonne **Vulnérabilités** pour ouvrir le rapport.
    1.  Pour visualiser plus d'informations, développez le récapitulatif.
    2.  Si la notice du distributeur du système d'exploitation est fournie, cliquez sur le lien dans la colonne **Notice officielle**.
8.  Pour afficher des informations sur chaque problème de configuration, dans le tableau **Problèmes de configuration détectés**, cliquez sur la ligne correspondant au problème.
9.  Effectuez l'action corrective pour chaque problème mentionné dans le rapport et reconstruisez l'image. Certains problèmes dans le fichier Dockerfile peuvent être résolus en utilisant le code fourni dans la rubrique [Résolution des problèmes dans les images](#va_report).

Si des vulnérabilités existent et si vous ne les corrigez pas, ces problèmes risquent d'impacter la sécurité des conteneurs générés avec cette image. Vous pouvez néanmoins continuer à utiliser dans un conteneur une image affectée par des problèmes de sécurité et de configuration.

 


## Examen d'un rapport de vulnérabilité via l'interface CLI
{: #va_registry_cli}

Vous pouvez examiner à l'aide de l'interface CLI la sécurité des images Docker stockées dans vos espaces de nom dans {{site.data.keyword.registrylong_notm}}.
{:shortdesc}

1.  Recensez les images dans votre compte {{site.data.keyword.Bluemix_notm}}. La liste de toutes les images est renvoyée, sans considération de l'espace de nom où elles sont stockées.

    ```
    ibmcloud cr image-list
    ```
    {: pre}

2.  Vérifiez le statut dans la colonne du **STATUT DE SECURITE**.
    -   `No Issues` : aucun problème de sécurité détecté.
    -   `X Issues` : problèmes de sécurité ou vulnérabilités potentiels détectés.
    -   `Scanning` : l'image est en cours d'analyse et le statut de vulnérabilité final n'est pas encore déterminé.
4.  Pour afficher les détails du statut, passez en revue le rapport Vulnerability Advisor.

    ```
    ibmcloud cr va registry.<region>/<my_namespace>/<my_image>:<tag>
    ```
    {: pre}

    Dans la sortie de l'interface CLI, vous pouvez consulter les informations suivantes relatives aux problèmes de configuration.
      - Security practice (mesure ou pratique de sécurité) : une description de la vulnérabilité trouvée
      - Corrective action (action corrective) : détails permettant de corriger la vulnérabilité


## Examen d'un rapport de conteneur
{: #va_reviewing_container}

Dans votre tableau de bord, vous pouvez examiner le statut d'un conteneur pour déterminer si sa sécurité est conforme aux règles de votre organisation. Vous pouvez également examiner le rapport de sécurité d'un conteneur, qui décrit en détail les packages vulnérables et les paramètres de conteneur ou d'application non sécurisés et spécifie si le conteneur est conforme aux règles de l'organisation.
{:shortdesc}

Assurez-vous que votre conteneur est aussi sécurisé que possible en consultant son rapport de sécurité et remédiez aux problèmes de sécurité ou de configuration signalés en procédant comme suit :

1.  Sélectionnez le conteneur dont vous souhaitez consulter le rapport :
    1.  A partir du catalogue, sélectionnez **Conteneurs**, puis cliquez sur **Container Registry**.
    2.  Sélectionnez l'onglet **Référentiels privés** et sélectionnez la ligne correspondant au référentiel souhaité. 
    3.  Sélectionnez la ligne correspondant à l'image souhaitée. 
    4.  Sélectionnez l'onglet **Conteneurs associés**, puis sélectionnez la ligne correspondant au conteneur souhaité. Le rapport de sécurité s'ouvre. 
2.  Passez en revue les différentes sections pour identifier les problèmes de sécurité et de configuration potentiels pour chaque package de l'image :

      -   **Vulnérabilités** : répertorie les packages présentant des vulnérabilités connues, lesquelles sont mises à jour quotidiennement à partir des avis de sécurité publiés pour les types d'image Docker répertoriés dans [Gestion de la sécurité des images avec Vulnerability Advisor](va_index.html). Généralement, pour qu'un package vulnérable réussisse l'examen, une version plus récente de ce package incluant un correctif pour la vulnérabilité est requise. Un même package peut répertorier plusieurs vulnérabilités, auquel cas,
une même mise à jour peut corriger plusieurs problèmes. Cliquez sur le code de l'avis de sécurité pour consulter plus d'informations sur le
package et sur la procédure à suivre pour le mettre à jour.

    -   **Problèmes de configuration** : énumère des mesures que vous pouvez prendre pour augmenter la sécurité du conteneur et répertorie les paramètres d'application du conteneur qui ne sont pas sécurisés. Développez la ligne pour savoir comment résoudre le problème. 

   Des actions correctives ou des suggestions sont indiquées pour chaque problème répertorié.
   
3.  Passez en revue le statut de règle pour chaque problème de sécurité. Le statut de règle indique si ce problème est exempté. 

    -  **Actif** : vous avez un problème qui n'est pas exempté et qui affecte votre statut de sécurité. 
    -  **Exempté** : ce problème a été exempté par vos paramètres de règle. 
    -  **Exempté partiellement** : ce problème est associé à plus d'un avis de sécurité. Les avis de sécurité ne sont pas tous exemptés.

4.  Décidez comment mettre à jour le conteneur en vue de résoudre les problèmes. 

    **Important :** pour corriger les problèmes liés à l'image de conteneur, vous devez supprimer l'ancienne instance et redéployer, ce qui signifie que vous allez perdre les données du conteneur existant. Vous devez avoir une connaissance satisfaisante de l'architecture de votre conteneur pour pouvoir choisir la méthode appropriée pour le redéployer. 

    Exemple :

    -   Si votre conteneur est dissocié des données qu'il calcule, vous pouvez l'arrêter et le supprimer, apporter les modifications requises à l'image et redéployer, sans perdre aucune donnée. 
    -   Vous pouvez utiliser un service {{site.data.keyword.Bluemix_notm}}, par exemple, [Delivery Pipeline](../ContinuousDelivery/pipeline_about.html), pour vous aider à mettre à jour l'instance de conteneur vulnérable.  
    -   Dans une architecture de microservices, vous pouvez acheminer le trafic vers une autre instance de conteneur pendant que vous corrigez les problèmes de sécurité ou de configuration, et envoyer la nouvelle image dans un déploiement rouge/noir. 

5.  Si le problème ne peut pas être corrigé maintenant, vous pouvez l'exempter dans vos paramètres de règle, afin de l'empêcher de bloquer le déploiement du conteneur. Pour exempter le problème, cliquez sur l'icône **Ouvrir et fermer la liste des options**, puis sur **Créer une exemption**.

6.  Corrigez les problèmes qui sont décrits dans le rapport **Sécurité** et régénérez l'image ou redéployez le conteneur selon la méthode que vous avez choisie. Certains problèmes dans le fichier Dockerfile peuvent être résolus en utilisant le code fourni dans la rubrique [Résolution des problèmes dans les images](/docs/services/va/va_index.html#va_report).


## Résolution des problèmes courants dans les images
{: #va_report}

Examinez les exemples de correctif pour les problèmes courants pouvant être signalés par Vulnerability Advisor. Certains problèmes peuvent être corrigés en mettant à jour votre Dockerfile.
{:shortdesc}


### Age maximal du mot de passe, durée de vie minimale du mot de passe (en jours) et longueur minimale du mot de passe
{: #va_password}

**Problème** : Vous recevez une ou plusieurs vulnérabilité parmi les suivantes :

```
L'âge maximal du mot de passe doit être défini sur 90 jours.
```
{: screen}

```
La longueur minimale du mot de passe doit être de 8.
```
{: screen}

```
Le nombre de jours minimal qui doit s'écouler entre les changements de mot de passe par l'utilisateur doit être 1.
```
{: screen}

**Correctif** : Appliquez la conformité du mot de passe en ajoutant le code suivant à votre fichier Dockerfile :

```
RUN \
    sed -i 's/^PASS_MAX_DAYS.*/PASS_MAX_DAYS    90/' /etc/login.defs && \
    sed -i 's/^PASS_MIN_DAYS.*/PASS_MIN_DAYS    1/' /etc/login.defs && \
    sed -i 's/sha512/sha512 minlen=8/' /etc/pam.d/common-password
```
{: codeblock}


### Vulnérabilité SSH
{: #ssh}

**Problème** : La vulnérabilité suivante est renvoyée :

```
Le serveur SSH ne doit pas être installé.
```
{: screen}

**Correctif** : Au lieu d'utiliser SSH, utilisez `docker attach` ou `docker exec` pour accéder à votre conteneur. Vérifiez que votre fichier Dockerfile ne contient pas d'étapes d'installation de serveur SSH.
