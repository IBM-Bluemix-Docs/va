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

# Gestion de la sécurité des images avec Vulnerability Advisor 
{: #va_index}

L'assistant de détection des vulnérabilités (Vulnerability Advisor) vérifie le statut de sécurité des images de conteneur avant le déploiement.
{:shortdesc}


## A propos de Vulnerability Advisor 
{: #about}

Vulnerability Advisor assure la gestion de la sécurité pour
{{site.data.keyword.containerlong}}. Vulnerability Advisor génère un rapport sur le statut de la sécurité, suggère
des correctifs et des pratiques recommandées, et propose des techniques pour empêcher l'exécution d'images non sécurisées. La correction des problèmes de sécurité et de configuration signalés par Vulnerability Advisor peut vous aider à sécuriser votre infrastructure {{site.data.keyword.cloud_notm}}.
{:shortdesc}

Vulnerability Advisor inclut les fonctions suivantes :

-   Analyse des images pour détection de vulnérabilités
-   Génération d'un rapport d'évaluation basé sur des normes de sécurité, telles que ISO 27002, [Center of Internet Security ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.cisecurity.org/), ainsi que sur des pratiques en matière de sécurité spécifiques à {{site.data.keyword.containerlong_notm}}
-   Détection de logiciels malveillants basés fichier
-   Soumission de recommandations pour sécuriser les fichiers de configuration d'un sous-ensemble de fichiers d'application
-   Soumission d'instructions pour correction d'une vulnérabilité ou d'un problème de configuration signalés dans ses rapports
   

    


**Packages vulnérables**

Vulnerability Advisor recherche des packages vulnérables dans les images basées sur des systèmes d'exploitation pris en charge et fournit un lien vers les éventuels bulletins de sécurité pertinents concernant la vulnérabilité concernée. 

Les packages où ont été détectés des problèmes de vulnérabilité connus sont affichés. Les vulnérabilités éventuelles sont mises à jour quotidiennement à partir des avis de sécurité publiés pour les images Docker répertoriées dans le tableau suivant. Généralement, pour qu'un package vulnérable réussisse l'examen, une version plus récente de ce package incluant un correctif pour la vulnérabilité est requise. Un même package peut recenser plusieurs vulnérabilités et, dans ce cas, une seule mise à jour du package peut résoudre plusieurs vulnérabilités. Les informations de la colonne **Action corrective** décrivent comment améliorer la sécurité.

Les images de base prises en charge sont décrites dans le tableau suivant.

  |Image de base Docker|Source des avis de sécurité|
  |-----------------|--------------------------|
  |Alpine|[Git - Alpine Linux ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://git.alpinelinux.org/) et [CIRCL CVE Search ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://cve.circl.lu/)|
  |CentOS| [CentOS announce archives ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://lists.centos.org/pipermail/centos-announce/) et [CentOS CR announce archives ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://lists.centos.org/pipermail/centos-cr-announce/)|
  |Debian|[Debian security announcements ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://lists.debian.org/debian-security-announce/)|
  |Red Hat Enterprise Linux (RHEL)|[Red Hat Product Errata ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://access.redhat.com/errata/#/)|
  |Ubuntu|[Ubuntu Security Notices ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ubuntu.com/usn/)|
  {: caption="Tableau 1. Images de base Docker vérifiées par Vulnerability Advisor pour détection de packages vulnérables" caption-side="top"}
  



**Configurations d'applications**

Recense les paramètres d'application non sécurisés de l'image. Les informations de la colonne **Action corrective** décrivent comment améliorer la sécurité.




**Rapport de sécurité**

Dans le tableau de bord Registre, la colonne **Rapport de sécurité** indique le statut de vos référentiels.

Vulnerability Advisor recherche des vulnérabilités connues dans les
paramètres de configuration pour les types d'application suivants :
-   MySQL
-   NGINX
-   Apache

Le rapport identifie les pratiques de sécurité de cloud souhaitables pour vos images. Vous pouvez accéder à la liste complète des problèmes de sécurité et de configuration vérifiés par Vulnerability Advisor.

Le tableau de bord Vulnerability Advisor fournit une présentation et une évaluation de la sécurité d'une image. 

Pour plus d'informations sur le tableau de bord Vulnerability Advisor, voir [Examen d'un rapport de vulnérabilité](#va_reviewing).





## Examen d'un rapport de vulnérabilité pour votre image
{: #va_reviewing}

Avant de déployer une image, vous pouvez examiner son rapport Vulnerability Advisor, lequel fournit des détails sur les éventuels packages vulnérables et les paramètres d'application non sécurisés.
{:shortdesc}

Les images de conteneur sont fournies par IBM, par des tiers, ou peuvent être ajoutées par votre organisation.

Examinez les problèmes potentiels relatifs à l'image ou à la configuration en procédant comme suit :

1.  Connectez-vous à {{site.data.keyword.Bluemix_notm}}. Vous devez être connecté pour que Vulnerability Advisor soit visible dans l'interface graphique.
2.  Cliquez sur **Catalogue**.
3.  Sous **Infrastructure**, cliquez sur **Conteneurs**. 
4.  Cliquez sur la vignette **Container Registry**.
5.  Développez **Vulnerability Advisor** et cliquez sur **Référentiels analysés**. 
6.  Pour visualiser le rapport de l'image indiquant `latest` (dernière), cliquez sur la ligne de ce référentiel. Le rapport présente le nombre total de problèmes et indique si des packages vulnérables ou des problèmes de configuration ont été détectés. Si aucune balise `latest` n'existe dans le référentiel, l'image la plus récente est utilisée.
7.  Pour afficher des informations sur chaque package vulnérable, dans le tableau **Packages vulnérables détectés**, cliquez sur la colonne **Vulnérabilités** pour ouvrir le rapport.
    1.  Pour visualiser plus d'informations, développez le récapitulatif.
    2.  Pour consulter la notice du distributeur du système d'exploitation, cliquez sur le lien dans la colonne **Notice officielle**.
8.  Pour afficher des informations sur chaque problème de configuration, dans le tableau **Problèmes de configuration détectés**, cliquez sur la ligne correspondant au problème. 
9.  Effectuez l'action corrective pour chaque problème mentionné dans le rapport et reconstruisez l'image. Certains problèmes dans le fichier Dockerfile peuvent être résolus en utilisant le code fourni dans la rubrique [Résolution des problèmes dans les images](#va_report).

Si des vulnérabilités existent sans que vous ne les corrigiez, ces problèmes peuvent affecter l'utilisation de l'image pour un conteneur. Vous pouvez continuer à utiliser dans un conteneur une image affectée par des problèmes de sécurité et de configuration.

 




## Examen à l'aide de l'interface CLI de la sécurité d'image des images Docker stockées dans un espace de nom 
{: #va_registry_cli}

Vous pouvez examiner à l'aide de l'interface CLI la sécurité des images Docker stockées dans vos espaces de nom dans {{site.data.keyword.registrylong_notm}} pour trouver des informations sur des vulnérabilités potentielles.
{:shortdesc}

Lorsque vous ajoutez au registre une image, celle-ci est automatiquement analysée par Vulnerability Advisor pour détecter des problèmes de sécurité et des vulnérabilités potentielles. Les conteneurs
déployés à partir d'images vulnérables risquent d'être attaqués et compromis. Les images ne sont analysées que si elles sont basées sur un système d'exploitation pris en charge par Vulnerability Advisor.

Si des problèmes de sécurité sont identifiés, des
instructions vous sont soumises afin de vous aider à résoudre la vulnérabilité signalée.

Pour vérifier le statut de vulnérabilité d'images dans votre compte {{site.data.keyword.Bluemix_notm}}, procédez comme suit.

1.  Recensez toutes les images dans votre compte {{site.data.keyword.Bluemix_notm}}. La commande suivante renvoie la liste de toutes les images sans considération de l'espace de nom où elles sont stockées.

    ```
    bx cr image-list
    ```
    {: pre}

2.  Vérifiez le statut des images dans la colonne **Statut de vulnérabilité**. L'un des statuts suivants est affiché :
    -   `OK` Ce statut indique qu'aucun problème de sécurité n'a été détecté.
    -   `Vulnérable` Ce statut indique qu'un problème de sécurité potentiel ou une vulnérabilité ont été détectés.
    -   `Inconnu` Ce statut est affiché pendant l'analyse de l'image jusqu'à ce que le statut de vulnérabilité final puisse être déterminé.
    -   `Système d'exploitation non pris en charge` Ce statut est affiché si l'analyse de l'image n'est pas prise en charge par Vulnerability Advisor.
4.  Pour plus d'informations sur le statut, consultez le rapport de Vulnerability Advisor.

    ```
    bx cr va registry.<region>/<my_namespace>/<my_image>:<tag>
    ```
    {: pre}

    Votre sortie CLI liste les packages vulnérables, soumet une description de la vulnérabilité détectée et un lien vers des instructions pour la corriger.


## Résolution de problèmes courants dans les images 
{: #va_report}

Exemples de correctifs pour les problèmes courants signalés par Vulnerability Advisor.
{:shortdesc}

Vulnerability Advisor propose des actions correctives
pour les problèmes de sécurité et de configuration signalés. Certains problèmes signalés peuvent être corrigés en mettant à jour votre Dockerfile. Afin de
résoudre plus rapidement des problèmes courants, utilisez les exemples de
code suivants pour implémenter la solution dans votre fichier Dockerfile.

### Age maximal du mot de passe, durée de vie minimale du mot de passe (en jours) et longueur minimale du mot de passe
{: #va_password}

**Problème** : une erreur ou toutes les erreurs suivantes sont signalées :

```
L'âge maximal du mot de passe doit être défini sur 90 jours.
```
{: screen}

```
La longueur minimale du mot de passe doit être de 8.
```
{: screen}

```
Le nombre de jours minimal qui doit s'écouler entre les changements de mot
de passe par l'utilisateur doit être 1.
```
{: screen}

**Correctif** : appliquez la conformité de mot de passe en ajoutant le code suivant à votre Dockerfile.

```
RUN \
    sed -i 's/^PASS_MAX_DAYS.*/PASS_MAX_DAYS    90/' /etc/login.defs && \
    sed -i 's/^PASS_MIN_DAYS.*/PASS_MIN_DAYS    1/' /etc/login.defs && \
    sed -i 's/sha512/sha512 minlen=8/' /etc/pam.d/common-password
```

### Vulnérabilité SSH 
{: #ssh}

**Problème** : l'erreur suivante est renvoyée :

```
Le serveur SSH ne doit pas être installé.
```
{: screen}

**Correctif** : au lieu d'utiliser SSH, utilisez `docker attach` ou `docker exec` pour accéder à votre conteneur. Vérifiez que votre fichier Dockerfile ne contient pas d'étapes d'installation de serveur SSH.

