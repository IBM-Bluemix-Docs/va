---

copyright:
  years: 2017, 2019
lastupdated: "2019-08-05"

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


# Gestion de la sécurité des images avec Vulnerability Advisor
{: #va_index}

L'assistant de détection des vulnérabilités (Vulnerability Advisor) vérifie le statut de sécurité des images de conteneur fournies par {{site.data.keyword.IBM}}, des tiers, ou ajoutées aux espaces de nom de registre de votre organisation.
{:shortdesc}

Lorsque vous ajoutez une image à un espace de nom, cette image est automatiquement analysée par Vulnerability Advisor pour détecter des problèmes de sécurité et des vulnérabilités potentielles. Si des problèmes de sécurité sont identifiés, des instructions vous sont soumises afin de vous aider à résoudre la vulnérabilité signalée.

Vulnerability Advisor fournit la gestion de la sécurité pour [{{site.data.keyword.registrylong_notm}}](/docs/services/Registry?topic=registry-getting-started#getting-started) en générant un rapport de statut de sécurité qui inclut des correctifs suggérés et les meilleures pratiques.

Les problèmes détectés par Vulnerability Advisor génèrent un verdict stipulant qu'il n'est pas recommandé de déployer cette image. Si vous choisissez de déployer l'image, les conteneurs qui sont déployés à partir de celle-ci comportent des problèmes connus qui peuvent être exploités pour attaquer ou compromettre le conteneur. Le verdict est ajusté en fonction des exemptions que vous avez spécifiées. Ce verdict peut être utilisé par le programme Image Security Enforcement afin d'empêcher le déploiement d'images non sécurisées dans {{site.data.keyword.containerlong_notm}}.

La correction des problèmes de sécurité et de configuration signalés par Vulnerability Advisor peut vous aider à sécuriser votre infrastructure {{site.data.keyword.cloud_notm}}.

## A propos de Vulnerability Advisor
{: #about}

Vulnerability Advisor fournit des fonctions vous permettant de sécuriser vos images.
{:shortdesc}

Les fonctions disponibles sont les suivantes :

- Analyse les images pour détection d'images
- Fournit un rapport d'évaluation basé sur les pratiques de sécurité spécifiques à {{site.data.keyword.containerlong_notm}}
- Soumet des recommandations pour sécuriser les fichiers de configuration d'un sous-ensemble de fichiers d'application
- Fournit des instructions pour la correction d'un [package vulnérable](#packages) ou d'un [problème de configuration](#app_configurations) signalé dans son rapport.
- Fournit des verdicts au programme [Container Image Security Enforcement](/docs/services/Registry?topic=registry-security_enforce#security_enforce)
- Applique des exemptions à des rapports au niveau compte, espace de nom, référentiel ou balise afin de signaler que les problèmes qui sont marqués ne s'appliquent pas à votre cas d'utilisation

Dans le tableau de bord Registre, la colonne **Statut de règle** indique le statut de vos référentiels. Le rapport lié identifie les pratiques de sécurité de cloud souhaitables pour vos images.

Le tableau de bord Vulnerability Advisor fournit une présentation et une évaluation de la sécurité d'une image. Pour plus d'informations sur le tableau de bord Vulnerability Advisor, voir [Examen d'un rapport de vulnérabilité](#va_reviewing).

### Protection des données
{: #about_data_protection}

Pour analyser des images et des conteneurs de votre compte afin de détecter les problèmes de sécurité, Vulnerability Advisor collecte, stocke et traite les informations suivantes :

- Zones à format libre, notamment ID, descriptions et noms d'image (registre, espace de nom, nom de référentiel et étiquette de l'image)
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
  |Alpine|[Git - Alpine Linux ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://git.alpinelinux.org/) rt [CIRCL CVE Search ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://cve.circl.lu/).|
  |CentOS| [CentOS announce archives ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://lists.centos.org/pipermail/centos-announce/) et [CentOS CR announce archives ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://lists.centos.org/pipermail/centos-cr-announce/). Pour plus d'informations sur les vulnérabilités, voir [Vulnérabilités dans les packages sur CentOS](#va_centos).|
  |Debian|[Debian security announcements ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://lists.debian.org/debian-security-announce/).|
  |Red Hat Enterprise Linux (RHEL)|[Red Hat Security Data API ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://access.redhat.com/labsinfo/securitydataapi).|
  |Ubuntu|[Ubuntu Security Notices ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://usn.ubuntu.com/).|
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

### Examen d'un rapport de vulnérabilité via l'interface graphique
{: #va_reviewing_gui}

Vous pouvez examiner à l'aide de l'interface graphique la sécurité des images Docker stockées dans vos espaces de nom dans {{site.data.keyword.registrylong_notm}}.
{:shortdesc}

1. Connectez-vous à {{site.data.keyword.cloud_notm}}.
2. Cliquez sur l'icône **Menu de Navigation** puis sur **Kubernetes**.
3. Cliquez sur **Registre**, puis sur la vignette **Images**. Une liste de vos images s'affiche dans le tableau **Images** avec le statut de sécurité de chaque image.
4. Pour afficher le rapport de l'image indiquant `latest` (dernière), cliquez sur la ligne de cette image. L'onglet **Détails de l'image** s'ouvre et affiche les données de cette image. Si aucune balise `latest` n'existe dans le référentiel, l'image la plus récente est utilisée.
5. Si le statut de sécurité affiche des problèmes, cliquez sur l'onglet **Problèmes par type** pour en connaître les détails. Les tableaux **Vulnérabilités** et **Problèmes de configuration** s'ouvrent.

   - Tableau **Vulnérabilités**. Affiche l'ID de vulnérabilité de chaque problème, le statut de règle de ce problème ainsi que les packages affectés et indique comment résoudre le problème. Pour afficher plus d'informations sur ce problème, développez la ligne. Ceci affichera un récapitulatif du problème ainsi qu'un lien vers l'avis de sécurité correspondant du fournisseur. Une liste des packages s'affiche avec les problèmes de vulnérabilité connus.
  
     La liste est mise à jour quotidiennement d'après les avis de sécurité publiés pour les images Docker répertoriées dans [Types de vulnérabilité](#types). Généralement, pour qu'un package vulnérable réussisse l'examen, une version plus récente de ce package incluant un correctif pour la vulnérabilité est requise. Un même package peut recenser plusieurs vulnérabilités et, dans ce cas, une seule mise à jour du package peut résoudre plusieurs problèmes. Cliquez sur le code de l'avis de sécurité pour afficher plus d'informations sur le package et sur la procédure à suivre pour le mettre à jour.

   - Tableau **Problèmes de configuration**. Affiche l'ID de problème de configuration de chaque problème, le statut de règle de ce problème ainsi que la pratique de sécurité. Pour afficher plus d'informations sur ce problème, développez la ligne. Ceci affichera un récapitulatif de ce problème ainsi qu'un lien vers l'avis de sécurité correspondant.
  
     La liste propose des actions que vous pouvez effectuer pour augmenter la sécurité du conteneur et répertorie les paramètres d'application du conteneur qui ne sont pas sécurisés. Développez la ligne pour savoir comment résoudre le problème.

6. Effectuez l'action corrective pour chaque problème mentionné dans le rapport et reconstruisez l'image.

### Examen d'un rapport de vulnérabilité via l'interface CLI
{: #va_registry_cli}

Vous pouvez examiner à l'aide de l'interface CLI la sécurité des images Docker stockées dans vos espaces de nom dans {{site.data.keyword.registrylong_notm}}.
{:shortdesc}

1. Recensez les images dans votre compte {{site.data.keyword.cloud_notm}}. La liste de toutes les images est renvoyée, sans considération de l'espace de nom où elles sont stockées.

   ```
   ibmcloud cr image-list
   ```
   {: pre}

2. Vérifiez le statut dans la colonne du **STATUT DE SECURITE**.
    - `Aucun problème :` aucun problème de sécurité n'a été détecté.
    - `<X> problèmes :` nombre de vulnérabilités ou problèmes de sécurité potentiels détectés, où `<X>` correspondant au nombre de problèmes.
    - `En cours d'analyse :` l'image est en cours d'analyse et le statut de vulnérabilité final n'est pas déterminé.

3. Pour afficher les détails des statuts, passez en revue le rapport de Vulnerability Advisor :

   ```
   ibmcloud cr va registry.<region>/<my_namespace>/<my_image>:<tag>
   ```
   {: pre}

   Dans la sortie de l'interface CLI, vous pouvez consulter les informations suivantes relatives aux problèmes de configuration.
      - `Pratique de sécurité :` description de la vulnérabilité détectée
      - `Action corrective :` informations expliquant comment corriger la vulnérabilité

### Vulnérabilités dans les packages sur CentOS
{: #va_centos}

Si vous utilisez CentOS, vous pouvez obtenir des faux positifs dans votre rapport, c'est-à-dire que le rapport peut signaler une vulnérabilité qui n'en est pas une. Cette situation se produit lorsqu'un avis de sécurité est publié par Red Hat mais qu'il n'est pas applicable à Centos ou que le correctif n'a pas encore été appliqué à CentOS.
{:shortdesc}

Si vous recevez un rapport signalant que votre package comporte des vulnérabilités, procédez comme suit :

1. Affichez les étapes pour mettre à jour le package en cliquant sur le code de l'avis de sécurité.
2. Mettez à jour le package en effectuant les étapes appropriées.
3. Si le package est mis à jour, le résultat n'était pas un faux positif et l'action requise est terminée.
4. Si le package n'est pas mis à jour car aucune nouvelle version n'est disponible à l'installation, le résultat était un faux positif. Vous pouvez ajouter une règle d'exemption pour cet avis de sécurité (voir [Définition de règles d'exemption de l'organisation](#va_managing_policy)).

## Définition de règles d'exemption de l'organisation
{: #va_managing_policy}

Si vous désirez gérer la sécurité d'une organisation {{site.data.keyword.cloud_notm}}, vous pouvez utiliser votre paramètre de règle pour déterminer si un problème est exempt ou non. Vous pouvez utiliser Container Image Security Enforcement pour garantir que le déploiement n'est permis qu'à partir d'images ne présentant pas de problèmes de sécurité une fois que ceux exemptés par votre règle ont été pris en compte.
{:shortdesc}

Vous pouvez déployer des conteneurs depuis n'importe quelle image quel soit son statut de sécurité, sauf si Container Image Security Enforcement est déployé dans votre cluster. Pour savoir comment déployer Container Image Security Enforcement, voir [Installation de Security Enforcement](/docs/services/Registry?topic=registry-security_enforce#security_enforce).

Lorsque vous utilisez Container Image Security Enforcement, tout problème de sécurité détecté par Vulnerability Advisor empêche le déploiement d'un conteneur depuis l'image concernée. Pour permettre le déploiement d'une image dans laquelle des problèmes ont été détectés, vous devez ajouter des exemptions à votre règle.

### Définition de règles d'exemption de l'organisation depuis l'interface graphique
{: #va_managing_policy_gui}

Si vous désirez définir des exemptions à la règle à l'aide de l'interface graphique, procédez comme suit :

1. Connectez-vous à {{site.data.keyword.cloud_notm}}. Vous devez être connecté pour que Vulnerability Advisor soit visible dans l'interface graphique.
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
