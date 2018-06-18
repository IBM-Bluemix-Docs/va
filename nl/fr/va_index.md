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


# Gestion de la sécurité des images avec Vulnerability Advisor
{: #va_index}

L'assistant de détection des vulnérabilités (Vulnerability Advisor) vérifie le statut de sécurité des images de conteneur fournies par IBM, des tiers, ou qui sont ajoutées aux espaces de nom de registre de votre organisation.
{:shortdesc}


Lorsque vous ajoutez une image à un espace de nom, cette image est automatiquement analysée par Vulnerability Advisor pour détecter des problèmes de sécurité et des vulnérabilités potentielles. Si des problèmes de sécurité sont identifiés, des
instructions vous sont soumises afin de vous aider à résoudre la vulnérabilité signalée. Vous pouvez toujours déployer des conteneurs à partir d'images vulnérables, mais vous ne devez pas oublier que ces conteneurs risquent d'être attaqués ou compromis.


## A propos de Vulnerability Advisor
{: #about}

Vulnerability Advisor assure la gestion de la sécurité pour
{{site.data.keyword.containerlong}}. Vulnerability Advisor génère un rapport sur le statut de la sécurité, suggère
des correctifs et des pratiques recommandées, et propose des techniques pour empêcher l'exécution d'images non sécurisées. La correction des problèmes de sécurité et de configuration signalés par Vulnerability Advisor peut vous aider à sécuriser votre infrastructure {{site.data.keyword.cloud_notm}}.
{:shortdesc}

Vulnerability Advisor inclut les fonctions suivantes :

-   Analyse les images pour détection de vulnérabilités
-   Fournit un rapport d'évaluation basé sur les pratiques de sécurité spécifiques à {{site.data.keyword.containerlong_notm}}
-   Soumet des recommandations pour sécuriser les fichiers de configuration d'un sous-ensemble de fichiers d'application
-   Fournit des instructions pour la correction d'un [package vulnérable](#packages) ou d'un [problème de configuration](#app_configurations) signalé dans son rapport.

Dans le tableau de bord Registre, la colonne **Rapport de sécurité** indique le statut de vos référentiels. Le rapport identifie les pratiques de sécurité de cloud souhaitables pour vos images. 

Le tableau de bord Vulnerability Advisor fournit une présentation et une évaluation de la sécurité d'une image. Pour plus d'informations sur le tableau de bord Vulnerability Advisor, voir [Examen d'un rapport de vulnérabilité](#va_reviewing).
	
	
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
{: #va_install_livescan}

Avant de commencer :

1.  Connectez-vous au client d'interface CLI {{site.data.keyword.Bluemix_notm}}. Si vous disposez d'un compte fédéré, utilisez `--sso`.
2.  [Ciblez votre interface CLI `kubectl`](../../containers/cs_cli_install.html#cs_cli_configure) sur le cluster qui doit utiliser une charte Helm.
3.  Créez un ID service et une clé d'API pour le scanner de conteneur et donnez-lui un nom :
    1.  Créez un ID service en exécutant la commande suivante, en remplaçant `<scanner_serviceID>` par le nom de votre choix pour l'ID service. Notez son **CRN**.
    
        ```
    	bx iam service-id-create <IDservice_scanner>
    	```
        {: codeblock}

    2.  Créez une clé d'API de service, où `<scanner_serviceID>` est l'ID service créé à l'étape précédente et qui remplace `<scanner_APIkey_name>` par le nom de votre choix pour la clé d'API de scanner. 
    
        ```
    	bx iam service-api-key-create <nom_cléAPI_scanner> <IDservice_scanner>
    	```
        {: codeblock}
	
	    La clé d'API de scanner est renvoyée.
	
	    Veillez à stocker votre clé d'API de scanner de manière sécurisée car elle ne pourra pas être extraite ultérieurement.
	    {: tip}
	
    3.  Créez une règle de service accordant le rôle `Writer` (auteur).
    		
        ```
    	bx iam service-policy-create <IDservice_scanner> --resource-type scaningress --service-name container-registry --roles Writer
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
    EmitURL: <URL_régionale_émise>
    AccountID: <ID_compte_IBM_Cloud>
    ClusterID: <ID_cluster>
    APIKey: <cléAPI_scanner>
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
    <td>Entrez l'URL de point d'extrémité régionale de Vulnerability Advisor. Pour obtenir cette URL, exécutez <code>bx cr info</code> et extrayez l'adresse de <strong>Container Registry</strong> (registre de conteneur). Remplacez <code>registry</code> par <code>va</code>. Exemple : <code>https<span comment="make the link not a link">://va.</span>eu-gb.bluemix.net</code></td>
    </tr>
    <tr>
    <td><code>AccountID</code></td>
    <td>Remplacez par l'ID compte {{site.data.keyword.Bluemix_notm}} dans lequel se trouve votre cluster. Pour obtenir l'ID compte, exécutez <code>bx account list</code>.</td>
    </tr>
    <tr>
    <td><code>ClusterID</code></td>
    <td>Remplacez par le cluster Kubernetes dans lequel installer le scanner de conteneur. Pour répertorier les ID cluster, exécutez <code>bx cs clusters</code>.</td>
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
    
    **Remarque** : Le scanner de conteneur est installé dans l'espace de nom `kube-system`, mais il analyse les conteneurs de tous les espaces de nom.

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


IBM Container Scanner est à présent installé, et l'agent livescan est déployé en tant que [DaemonSet ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) dans votre cluster. Bien que le scanner soit déployé dans l'espace de nom `kube-system`, il analyse tous les conteneurs affectés à des pods pour l'ensemble de vos espaces de nom Kubernetes, par exemple `default`. 



## Examen d'un rapport de vulnérabilité
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
    bx cr image-list
    ```
    {: pre}

2.  Vérifiez le statut dans la colonne du **STATUT DE SECURITE**.
    -   `No Issues` : aucun problème de sécurité détecté.
    -   `X Issues` : problèmes de sécurité ou vulnérabilités potentiels détectés.
    -   `Scanning` : l'image est en cours d'analyse et le statut de vulnérabilité final n'est pas encore déterminé.
4.  Pour afficher les détails du statut, passez en revue le rapport Vulnerability Advisor.

    ```
    bx cr va registry.<region>/<my_namespace>/<my_image>:<tag>
    ```
    {: pre}

    Dans la sortie de l'interface CLI, vous pouvez consulter les informations suivantes relatives aux problèmes de configuration.
      - Security practice (mesure ou pratique de sécurité) : une description de la vulnérabilité trouvée
      - Corrective action (action corrective) : détails permettant de corriger la vulnérabilité


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
