---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-21"

keywords: security status of container images, image security, Vulnerability Advisor, security, registry, vulnerabilities

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

# Gestion de la sécurité des images avec Vulnerability Advisor
{: #va_index}

L'assistant de détection des vulnérabilités (Vulnerability Advisor) vérifie le statut de sécurité des images de conteneur fournies par {{site.data.keyword.IBM}}, des tiers, ou ajoutées aux espaces de nom de registre de votre organisation. Si l'outil Container Scanner est installé dans chaque cluster, Vulnerability Advisor vérifie également le statut des conteneurs qui sont en cours d'exécution.
{:shortdesc}

Lorsque vous ajoutez une image à un espace de nom, cette image est automatiquement analysée par Vulnerability Advisor pour détecter des problèmes de sécurité et des vulnérabilités potentielles. Si des problèmes de sécurité sont identifiés, des instructions vous sont soumises afin de vous aider à résoudre la vulnérabilité signalée.

Vulnerability Advisor fournit la gestion de la sécurité pour [{{site.data.keyword.registrylong_notm}}](/docs/services/Registry?topic=registry-index#index) en générant un rapport de statut de sécurité qui inclut des correctifs suggérés et les meilleures pratiques.

Les problèmes détectés par Vulnerability Advisor génèrent un verdict stipulant qu'il n'est pas recommandé de déployer cette image. Si vous choisissez de déployer l'image, les conteneurs qui sont déployés à partir de celle-ci comportent des problèmes connus qui peuvent être exploités pour attaquer ou compromettre le conteneur. Le verdict est ajusté en fonction des exemptions que vous avez spécifiées. Ce verdict peut être utilisé par le programme Image Security Enforcement afin d'empêcher le déploiement d'images non sécurisées dans {{site.data.keyword.containerlong_notm}}.

La correction des problèmes de sécurité et de configuration signalés par Vulnerability Advisor peut vous aider à sécuriser votre infrastructure {{site.data.keyword.cloud_notm}}.

## A propos de Vulnerability Advisor
{: #about}

Vulnerability Advisor fournit des fonctions vous permettant de sécuriser vos images.
{:shortdesc}

Les fonctions disponibles sont les suivantes :

- Analyse les images pour détection d'images
- Analyse les problèmes dans les conteneurs qui s'exécutent si un outil [Container Scanner](#va_install_container_scanner) est installé dans chaque cluster
- Fournit un rapport d'évaluation basé sur les pratiques de sécurité spécifiques à {{site.data.keyword.containerlong_notm}}
- Soumet des recommandations pour sécuriser les fichiers de configuration d'un sous-ensemble de fichiers d'application
- Fournit des instructions pour la correction d'un [package vulnérable](#packages) ou d'un [problème de configuration](#app_configurations) signalé dans son rapport.
- Fournit des verdicts au programme [Container Image Security Enforcement](/docs/services/Registry?topic=registry-security_enforce#security_enforce)
- Applique des exemptions à des rapports au niveau compte, espace de nom, référentiel ou balise afin de signaler que les problèmes qui sont marqués ne s'appliquent pas à votre cas d'utilisation
- Fournit des liens vers des conteneurs associés dans la vue **Balise** de l'interface graphique {{site.data.keyword.registrylong_notm}}. Vous pouvez répertorier les conteneurs qui sont en cours d'exécution et qui utilisent cette image dans un cluster sur lequel l'outil Container Scanner est installé.

Dans le tableau de bord Registre, la colonne **Statut de règle** indique le statut de vos référentiels. Le rapport lié identifie les pratiques de sécurité de cloud souhaitables pour vos images.

Le tableau de bord de Vulnerability Advisor fournit une présentation et une évaluation de la sécurité d'une image et, si l'outil Container Scanner est installé, comporte des liens vers les conteneurs en cours d'exécution. Pour plus d'informations sur le tableau de bord Vulnerability Advisor, voir [Examen d'un rapport de vulnérabilité](#va_reviewing).

**Protection des données**

Pour analyser des images et des conteneurs de votre compte afin de détecter les problèmes de sécurité, Vulnerability Advisor collecte, stocke et traite les informations suivantes :

- Zones à format libre, notamment ID, descriptions et noms d'image (registre, espace de nom, nom de référentiel et étiquette de l'image)
- Métadonnées Kubernetes, notamment les noms de ressources Kubernetes telles que pod, jeu de répliques (ReplicaSet) et noms de déploiement (deployment)
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

Vulnerability Advisor recherche des packages vulnérables dans les images qui utilisent des systèmes d'exploitation pris en charge et fournit un lien vers les éventuels bulletins de sécurité pertinents concernant la vulnérabilité concernée.
{:shortdesc}

Les packages comportant des problèmes de vulnérabilité connus sont affichés dans les résultats d'analyse. Les vulnérabilités éventuelles sont mises à jour quotidiennement d'après les avis de sécurité publiés pour les images Docker répertoriées dans le tableau suivant. Généralement, pour qu'un package vulnérable réussisse l'examen, une version plus récente de ce package incluant un correctif pour la vulnérabilité est requise. Un même package peut recenser plusieurs vulnérabilités et, dans ce cas, une seule mise à jour du package peut résoudre plusieurs vulnérabilités.

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

Les images ne sont analysées que si elles utilisent un système d'exploitation pris en charge par Vulnerability Advisor. Vulnerability Advisor vérifie les paramètres de configuration pour les types d'application suivants :

- MySQL
- NGINX
- Apache

## Examen d'un rapport de vulnérabilités
{: #va_reviewing}

Avant de déployer une image, vous pouvez examiner son rapport Vulnerability Advisor pour plus d'informations sur les éventuels packages vulnérables et paramètres de conteneur ou d'application non sécurisés. Vous pouvez également vérifier si l'image est conforme aux règles de l'organisation.
{:shortdesc}

Si vous ne corrigez pas les problèmes détectés, ceux-ci peuvent nuire à la sécurité des conteneurs générés à l'aide de cette image. Si Container Image Security Enforcement n'est pas déployé, vous pouvez continuer à utiliser dans un conteneur une image affectée par des problèmes de sécurité et de configuration. Si Container Image Security Enforcement est déployé et actif pour l'image, tous les problèmes détectés doivent être exemptés par votre règle pour que des conteneurs puissent être déployés depuis l'image.

Pour déterminer la portée d'application dans Container Image Security Enforcement de problèmes détectés par Vulnerability Advisor, voir [Personnalisation de règles](/docs/services/Registry?topic=registry-security_enforce#customize_policies).
{:tip}

Si votre image ne répond pas aux exigences définies par la règle de votre organisation, vous devez configurer l'image pour qu'elle se conforme à ces exigences avant de pouvoir la déployer. Pour plus d'informations sur la consultation et la modification de la règle de l'organisation, voir [Définition de règles d'exemption de l'organisation](#va_managing_policy).
{:tip}

Si vous déployez Container Scanner après avoir déployé votre image, Vulnerability Advisor continue à analyser le conteneur pour détecter des problèmes de sécurité et de configuration. Vous pouvez résoudre les problèmes éventuels détectés en suivant la procédure décrite dans [Examen d'un rapport de conteneur](#va_reviewing_container).

### Examen d'un rapport de vulnérabilité via l'interface graphique
{: #va_reviewing_gui}

Vous pouvez examiner à l'aide de l'interface graphique la sécurité des images Docker stockées dans vos espaces de nom dans {{site.data.keyword.registrylong_notm}}.
{:shortdesc}

1. Connectez-vous à {{site.data.keyword.Bluemix_notm}}.
2. Cliquez sur l'icône **Menu de Navigation** puis sur **Kubernetes**.
3. Cliquez sur **Registre**, puis sur la vignette **Images**. Une liste de vos images s'affiche dans le tableau **Images** avec le statut de sécurité de chaque image.
4. Pour afficher le rapport de l'image indiquant `latest` (dernière), cliquez sur la ligne de cette image. L'onglet **Détails de l'image** s'ouvre et affiche les données de cette image. Si aucune balise `latest` n'existe dans le référentiel, l'image la plus récente est utilisée.
5. Si le statut de sécurité affiche des problèmes, cliquez sur l'onglet **Problèmes par type** pour en connaître les détails. Les tableaux **Vulnérabilités** et **Problèmes de configuration** s'ouvrent.

   - **Vulnérabilités :** ce tableau affiche l'ID de vulnérabilité de chaque problème, le statut de règle de ce problème ainsi que les packages affectés et indique comment résoudre le problème. Pour afficher plus d'informations sur ce problème, développez la ligne. Ceci affichera un récapitulatif du problème ainsi qu'un lien vers l'avis de sécurité correspondant du fournisseur. Une liste des packages s'affiche avec les problèmes de vulnérabilité connus.
  
     La liste est mise à jour quotidiennement d'après les avis de sécurité publiés pour les images Docker répertoriées dans [Types de vulnérabilité](#types). Généralement, pour qu'un package vulnérable réussisse l'examen, une version plus récente de ce package incluant un correctif pour la vulnérabilité est requise. Un même package peut recenser plusieurs vulnérabilités et, dans ce cas, une seule mise à niveau du package peut résoudre plusieurs problèmes. Cliquez sur le code de l'avis de sécurité pour afficher plus d'informations sur le package et sur la procédure à suivre pour le mettre à jour.

   - **Problèmes de configuration : ** ce tableau affiche l'ID de problème de configuration de chaque problème, le statut de règle de ce problème ainsi que la pratique de sécurité. Pour afficher plus d'informations sur ce problème, développez la ligne. Ceci affichera un récapitulatif de ce problème ainsi qu'un lien vers l'avis de sécurité correspondant.
  
     La liste propose des actions que vous pouvez effectuer pour augmenter la sécurité du conteneur et répertorie les paramètres d'application du conteneur qui ne sont pas sécurisés. Développez la ligne pour savoir comment résoudre le problème.

6. Effectuez l'action corrective pour chaque problème mentionné dans le rapport et reconstruisez l'image.

### Examen d'un rapport de vulnérabilité via l'interface CLI
{: #va_registry_cli}

Vous pouvez examiner à l'aide de l'interface CLI la sécurité des images Docker stockées dans vos espaces de nom dans {{site.data.keyword.registrylong_notm}}.
{:shortdesc}

1. Recensez les images dans votre compte {{site.data.keyword.Bluemix_notm}}. La liste de toutes les images est renvoyée, sans considération de l'espace de nom où elles sont stockées.

   ```
   ibmcloud cr image-list
   ```
   {: pre}

2. Vérifiez le statut dans la colonne du **STATUT DE SECURITE**.
    - **Aucun problème :** aucun problème de sécurité n'a été détecté.
    - **`<X>`Problèmes : ** `<X>` des vulnérabilités ou des problèmes de sécurité potentiels ont été détectés, où `<X>` indique le nombre de problèmes.
    - **En cours d'analyse :** l'image est en cours d'analyse et le statut de vulnérabilité final n'est pas encore déterminé.

3. Pour afficher les détails des statuts, passez en revue le rapport de Vulnerability Advisor :

   ```
   ibmcloud cr va registry.<region>/<my_namespace>/<my_image>:<tag>
   ```
   {: pre}

   Dans la sortie de l'interface CLI, vous pouvez consulter les informations suivantes relatives aux problèmes de configuration.
      - **Pratique de sécurité :** description de la vulnérabilité détectée
      - **Action corrective :** informations expliquant comment corriger la vulnérabilité

## Définition de règles d'exemption de l'organisation
{: #va_managing_policy}

Si vous désirez gérer la sécurité d'une organisation {{site.data.keyword.Bluemix_notm}}, vous pouvez utiliser votre paramètre de règle pour déterminer si un problème est exempt ou non. Vous pouvez décider d'utiliser Container Image Security Enforcement pour garantir que le déploiement n'est permis qu'à partir d'images ne présentant pas de problèmes de sécurité une fois que ceux exemptés par votre règle ont été pris en compte.
{:shortdesc}

Vous pouvez déployer des conteneurs depuis n'importe quelle image quel soit son statut de sécurité, sauf si Container Image Security Enforcement est déployé dans votre cluster. Pour savoir comment déployer Container Image Security Enforcement, voir [Installation de Security Enforcement](/docs/services/Registry?topic=registry-security_enforce#security_enforce).

Lorsque vous utilisez Container Image Security Enforcement, tout problème de sécurité détecté par Vulnerability Advisor empêche le déploiement d'un conteneur depuis l'image concernée. Pour permettre le déploiement d'une image dans laquelle des problèmes ont été détectés, vous devez ajouter des exemptions à votre règle.

### Définition de règles d'exemption de l'organisation depuis l'interface graphique
{: #va_managing_policy_gui}

Si vous désirez définir des exemptions à la règle à l'aide de l'interface graphique, procédez comme suit :

1. Connectez-vous à {{site.data.keyword.Bluemix_notm}}. Vous devez être connecté pour que Vulnerability Advisor soit visible dans l'interface graphique.
2. Cliquez sur l'icône **Menu de Navigation** puis sur **Kubernetes**.
3. Sous **Vulnerability Advisor**, cliquez sur **Paramètres de règle**.
4. Cliquez sur **Créer une exemption**.
5. Sélectionnez le type de problème.
6. Entre l'ID du problème.

   Vous pouvez trouver cette information dans votre [rapport de vulnérabilités](#va_reviewing). La colonne **ID de vulnérabilité** contient l'ID à utiliser pour CVE ou pour les problèmes associés à des avis de sécurité. La colonne **ID du problème de configuration** contient l'ID à utiliser pour les problèmes de configuration.
   {: tip}

7. Sélectionnez l'espace de nom du registre, le référentiel et l'étiquette auxquels appliquer l'exemption.
8. Cliquez sur **Sauvegarder**.

Vous pouvez également modifier et supprimer des exemptions en survolant la ligne concernée et en cliquant sur l'icône **Ouvrir et fermer la liste des options**.

### Définition de règles d'exemption de l'organisation depuis l'interface de ligne de commande
{: #va_managing_policy_cli}

Si vous désirez définir des exemptions à la règle à l'aide de l'interface de ligne de commande, procédez comme suit :

- Pour créer une exemption pour un problème de sécurité, exécutez la commande [`ibmcloud cr exemption-add`](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_exemption_add).
- Pour recenser vos exemptions pour des problèmes de sécurité, exécutez la commande [`ibmcloud cr exemption-list`](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_exemption_list).
- Pour répertorier les types de problèmes de sécurité que vous pouvez exempter, exécutez la commande [`ibmcloud cr exemption-types`](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_exemption_types).
- Pour supprimer une exemption pour un problème de sécurité, exécutez la commande [`ibmcloud cr exemption-rm`](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_exemption_rm).

Pour plus d'informations sur les commandes, utilisez l'indicateur `--help` lorsque vous exécutez la commande.

## Installation de l'outil Container Scanner
{: #va_install_container_scanner}

Container Scanner active Vulnerability Advisor pour signaler tout problème lié à l'exécution de conteneurs ne se trouvant pas dans l'image de base du conteneur. Si vous n'apportez aucune modification d'exécution à votre conteneur, Container Scanner n'est pas nécessaire car le rapport de l'image signalera les mêmes problèmes.
{:shortdesc}

Pour vérifier le statut de sécurité de conteneurs de production s'exécutant dans votre cluster, vous pouvez installer l'outil Container Scanner. Pour protéger votre application, Container Scanner analyse régulièrement vos conteneurs en cours d'exécution pour que vous puissiez détecter et corriger toutes les éventuelles vulnérabilités qui viennent d'être détectées.

Vous pouvez configurer Container Scanner pour qu'il contrôle les vulnérabilités dans les conteneurs affectés à des pods dans tous vos espaces de nom Kubernetes. Lorsque des vulnérabilités sont trouvées, vous devez corriger tous les problèmes liés à l'image, puis redéployer votre application. Container Scanner ne prend en charge que les conteneurs qui sont créés à partir d'images stockées dans {{site.data.keyword.registrylong_notm}}.

Pour utiliser Container Scanner, vous devez configurer les droits et ensuite configurer une [charte Helm ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://docs.helm.sh/developing_charts) et l'associer au cluster dans lequel vous souhaitez l'utiliser.

### Configuration des droits du service pour Container Scanner
{: #va_install_container_scanner_permissions}

Container Scanner requiert que des droits soient configurés pour que le service puisse fonctionner.
{:shortdesc}

Pour configurer les droits du service, procédez comme suit :

1. Connectez-vous au client d'interface CLI {{site.data.keyword.Bluemix_notm}}. Si vous disposez d'un compte fédéré, utilisez `--sso`.
2. [Ciblez votre interface CLI `kubectl`](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure) sur le cluster qui doit utiliser une charte Helm.
3. Créez un ID de service et une clé d'API pour l'outil Container Scanner et attribuez-lui un nom :
    1. Pour créer un ID de service, exécutez la commande suivante, où `<scanner_serviceID>` est le nom de votre choix pour l'ID de service. Notez son **CRN**.

       ```
       ibmcloud iam service-id-create <scanner_serviceID>
       ```
       {: codeblock}

    2. Créez une clé d'API de service, où `<scanner_serviceID>` est l'ID service créé à l'étape précédente et `<scanner_APIkey_name>` est le nom de votre choix pour la clé d'API de scanner.

       ```
       ibmcloud iam service-api-key-create <scanner_APIkey_name> <scanner_serviceID>
       ```
       {: codeblock}
       La clé d'API de scanner est renvoyée.

       Veillez à stocker votre clé d'API de scanner de manière sécurisée car elle ne pourra pas être extraite ultérieurement. Vérifiez également que vous disposez d'une clé d'API distincte pour chaque cluster sur lequel est installé le scanner.
       {: tip}

    3. Créez une règle de service accordant le rôle `Writer` (auteur).

       ```
       ibmcloud iam service-policy-create --resource-type scaningress --service-name container-registry --roles Writer <scanner_serviceID>
       ```
       {: codeblock}

### Configuration de la charte Helm
{: #va_install_container_scanner_helm}

Configurez une charte Helm et associez-la au cluster dans laquelle vous souhaitez l'utiliser.
{:shortdesc}

Pour configurer une charte Helm, procédez comme suit :

1. [Configurez Helm dans IBM Cloud Kubernetes Service](/docs/containers?topic=containers-integrations#helm). Si vous utilisez une règle de contrôle d'accès à base de rôles (RBAC) pour accorder les droits d'accès à Tiller, assurez-vous que le rôle de Tiller a accès à tous les espaces de nom. En accordant au rôle de Tiller l'accès à tous les espaces de nom, Container Scanner peut surveiller les conteneurs dans tous ces espaces de nom.

2. Ajoutez le référentiel de charte IBM à votre charte Helm, par exemple `ibm`.

   ```
   helm repo add ibm https://registry.bluemix.net/helm/ibm
   ```
   {: pre}

3. Sauvegardez les paramètres de configuration par défaut pour la charte Helm de Container Scanner dans un fichier YAML local. Incluez le référentiel de charte, par exemple `ibm`, au chemin d'accès à la charte Helm.

   ```
   helm inspect values ibm/ibmcloud-container-scanner > config.yaml
   ```
   {: pre}

4. Editez le fichier `config.yaml`.

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
   <caption>Tableau 2. Description des composants du fichier YAML</caption>
   <thead>
   <th>Zone</th>
   <th>Valeur</th>
   </thead>
   <tbody>
   <tr>
   <td><code>EmitURL</code></td>
   <td>Entrez l'URL de point d'extrémité régionale de Vulnerability Advisor. Pour obtenir l'URL, exécutez <code>ibmcloud cr info</code> et extrayez l'adresse de <strong>Container Registry</strong> (registre de conteneur). Par exemple, <code>https<span comment="make the link not a link">://registry.</span>eu-gb.bluemix.net</code>. Remplacez <code>registry</code> par <code>va</code>. Exemple, <code>https<span comment="make the link not a link">://va.</span>eu-gb.bluemix.net</code></td>
   </tr>
   <tr>
   <td><code>AccountID</code></td>
   <td>Remplacez <code>AccountID</code> par l'ID de compte {{site.data.keyword.Bluemix_notm}} dans lequel se trouve votre cluster. Pour obtenir l'ID compte, exécutez <code>ibmcloud account list</code>.</td>
   </tr>
   <tr>
   <td><code>ClusterID</code></td>
   <td>Remplacez <code>ClusterID</code> par le cluster Kubernetes dans lequel vous voulez installer l'outil Container Scanner. Pour répertorier les ID de cluster, exécutez <code>ibmcloud ks clusters</code>. <br> **Astuce :** n'utilisez pas le nom du cluster, mais son ID.
   </td>
   </tr>
   <tr>
   <td><code>APIKey</code></td>
   <td>Remplacez <code>APIKey</code> par la clé d'API de scanner créée précédemment.</td>
   </tr>
   </tbody></table>

5. Installez la charte Helm sur votre cluster avec le fichier `config.yaml` mis à jour. Les propriétés mises à jour sont stockées dans un objet configmap pour votre charte. Remplacez `<myscanner>` par le nom choisi pour votre charte Helm. Incluez le référentiel de charte, par exemple `ibm`, au chemin d'accès à la charte Helm.

   ```
   helm install -f config.yaml --name=<myscanner> ibm/ibmcloud-container-scanner
   ```
   {: pre}

   L'outil Container Scanner est installé dans l'espace de nom `kube-system`, mais il analyse les conteneurs de tous les espaces de nom .
   {:tip}

6. Vérifiez le statut de déploiement du diagramme. Lorsque la charte est prête, la zone **STATUS** indique `DEPLOYED`.

   ```
   helm status <myscanner>
   ```
   {: pre}

7. Une fois la charte déployée, vérifiez que les paramètres mis à jour dans le fichier `config.yaml` ont bien été utilisés.

   ```
   helm get values <myscanner>
   ```
   {: pre}

L'outil Container Scanner est maintenant installé et l'agent est déployé en tant que [DaemonSet ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) dans votre cluster. Bien que l'outil Container Scanner soit déployé sur l'espace de nom `kube-system`, il analyse tous les conteneurs affectés à des pods dans tous vos espaces de nom Kubernetes, par exemple `default`.

## Exécution de l'outil Container Scanner au travers d'un pare-feu
{: #va_firewall}

Si votre pare-feu bloque les connexions sortantes, vous devez le configurer pour qu'il autorise les noeuds worker à accéder à l'outil Container Scanner sur le port TCP `443` sur les adresses IP répertoriées dans le tableau suivant.
{:shortdesc}

 

<p>
  <table summary=" Les lignes doivent être lues de la gauche vers la droite, l'emplacement du serveur se trouvant dans la colonne 1 et les adresses IP pour la mise en correspondance dans la colonne 2.">
  <caption>Tableau 3. Adresses IP à ouvrir pour le trafic sortant</caption>
    <thead>
      <th>Emplacement</th>
      <th>Adresse IP</th>
    </thead>
    <tbody>
      <tr>
        <td>Dallas</td>
        <td><code>169.47.103.118</code><br><code>169.48.165.6</code></td>
      </tr>
      <tr>
         <td>Francfort</td>
         <td><code>159.8.220.182</code><br><code>158.177.74.102</code></td>
      </tr>
      <tr>
        <td>Londres</td>
        <td><code>158.175.71.134</code><br><code>5.10.111.190</code></td>
      </tr>
      <tr>
         <td>Sydney</td>
         <td><code>168.1.40.158</code><br><code>130.198.65.182</code></td>
      </tr>
      <tr>
        <td>Washington DC</td>
         <td><code>169.60.73.158</code><br><code>169.61.84.102</code></td>
      </tr>
    </tbody>
  </table>
</p>

## Examen d'un rapport de conteneur
{: #va_reviewing_container}

Dans votre tableau de bord, vous pouvez examiner le statut d'un conteneur pour déterminer si sa sécurité est conforme aux règles de votre organisation. Vous pouvez également examiner le rapport de sécurité d'un conteneur, qui décrit en détail les packages vulnérables et les paramètres de conteneur ou d'application non sécurisés et spécifie si le conteneur est conforme aux règles de l'organisation.
{:shortdesc}

Vérifiez en examinant la zone **Statut de la règle** que les conteneurs s'exécutant dans votre espace de nom continuent à se conformer à la règle de l'organisation. Le statut affiche l'une des situations suivantes :

- **Conforme à la règle :** aucun problème de configuration ou de sécurité n'a été détecté.
- **Non conforme à la règle :** Vulnerability Advisor a détecté des problèmes de configuration ou de sécurité potentiels qui entraînent la non-conformité du conteneur avec la règle. Si la règle de votre organisation autorise le déploiement d'images vulnérables, l'image peut être déployée avec l'état `A déployer avec précaution` et un avertissement est envoyé à l'utilisateur qui l'a déployée.
- **Evaluation incomplète :** l'analyse n'est pas terminée. Il se peut que l'analyse soit encore en cours ou que le système d'exploitation pour cette instance de conteneur ne soit pas compatible.

Assurez-vous que votre conteneur est aussi sécurisé que possible en consultant son rapport de sécurité et remédiez aux problèmes de sécurité ou de configuration signalés en procédant comme suit :

1. Sélectionnez le conteneur dont vous souhaitez consulter le rapport :
    1. Cliquez sur l'icône **Menu de Navigation** puis sur **Kubernetes**.
    2. Cliquez sur **Registre**, puis sur la vignette **Référentiels** et développez la ligne correspondant au référentiel de votre choix.
    3. Sélectionnez la ligne de l'image souhaitée.
    4. Sélectionnez l'onglet **Conteneurs associés**, puis sélectionnez la ligne correspondant au conteneur souhaité. Le rapport de sécurité s'ouvre.
2. Passez en revue les différentes sections pour identifier les problèmes de sécurité et de configuration potentiels pour chaque package de l'image :

    - **Vulnérabilités :** répertorie les packages contenant les problèmes de vulnérabilité connus. La liste est mise à jour quotidiennement d'après les avis de sécurité publiés pour les images Docker répertoriées dans [Types de vulnérabilité](#types). Généralement, pour qu'un package vulnérable réussisse l'examen, une version plus récente de ce package incluant un correctif pour la vulnérabilité est requise. Un même package peut recenser plusieurs vulnérabilités et, dans ce cas, une seule mise à niveau du package peut résoudre plusieurs problèmes. Cliquez sur le code de l'avis de sécurité pour afficher plus d'informations sur le package et sur la procédure à suivre pour le mettre à jour.

    - **Problèmes de configuration :** énumère des mesures que vous pouvez prendre pour augmenter la sécurité du conteneur et répertorie les paramètres d'application du conteneur qui ne sont pas sécurisés. Développez la ligne pour savoir comment résoudre le problème.

   Des actions correctives ou des suggestions sont indiquées pour chaque problème répertorié.

3. Passez en revue le statut de règle pour chaque problème de sécurité. Le statut de règle indique si ce problème est exempté.

    - **Actif :** vous avez un problème qui n'est pas exempté et qui affecte votre statut de sécurité.
    - **Exempté :** ce problème est exempté par vos paramètres de règle.
    - **Exempté partiellement :** ce problème est associé à plusieurs avis de sécurité. Les avis de sécurité ne sont pas tous exemptés.

4. Décidez comment mettre à jour le conteneur en vue de résoudre les problèmes.

    **Important :** pour corriger les problèmes liés à l'image de conteneur, vous devez supprimer l'ancienne instance et redéployer, ce qui signifie que vous allez perdre les données du conteneur existant. Vous devez avoir une connaissance satisfaisante de l'architecture de votre conteneur pour pouvoir choisir la méthode appropriée pour le redéployer.

    **Exemple**

    - Si votre conteneur est dissocié des données qu'il calcule, vous pouvez l'arrêter et le supprimer, apporter les modifications requises à l'image et redéployer, sans perdre aucune donnée.
    - Vous pouvez utiliser un service {{site.data.keyword.Bluemix_notm}}, tel que [Delivery Pipeline](/docs/services/ContinuousDelivery?topic=ContinuousDelivery-deliverypipeline_about#deliverypipeline_about), pour vous aider à mettre à jour l'instance de conteneur vulnérable.
    - Dans une architecture de microservices, vous pouvez acheminer le trafic vers une autre instance de conteneur pendant que vous corrigez les problèmes de sécurité ou de configuration, et envoyer la nouvelle image dans un déploiement rouge-noir.

5. Si vous n'arrivez pas à corriger le problème maintenant, vous pouvez l'exempter dans vos paramètres de règle pour qu'il ne bloque pas le déploiement du conteneur. Pour exempter le problème, cliquez sur l'icône **Ouvrir et fermer la liste des options**, puis sur **Créer une exemption**. Voir [Définition de règles d'exemption de l'organisation](#va_managing_policy).

6. Corrigez les problèmes qui sont décrits dans le rapport **Sécurité** et régénérez l'image ou redéployez le conteneur selon la méthode que vous avez choisie.
