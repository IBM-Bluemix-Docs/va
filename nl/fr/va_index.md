---

copyright:
  years: 2017
lastupdated: "2017-10-26"

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
-   Génération d'un rapport d'évaluation basé sur des normes de sécurité, telles que ISO 27002, ainsi que sur des pratiques en matière de sécurité spécifiques à {{site.data.keyword.containerlong_notm}}
-   Détection de logiciels malveillants basés fichier
-   Soumission de recommandations pour sécuriser les fichiers de configuration d'un sous-ensemble de fichiers d'application
-   Soumission d'instructions pour correction d'une vulnérabilité ou d'un problème de configuration signalés dans ses rapports

<dl>
  <dt><strong>Packages vulnérables</strong></dt>
  <dd>Vulnerability Advisor recherche des packages vulnérables dans les images basées sur des systèmes d'exploitation pris en charge et fournit un lien vers les éventuels bulletins de sécurité pertinents concernant la vulnérabilité concernée. Vulnerability
Advisor met à jour quotidiennement sa liste interne d'après ces avis de sécurité. Les images de base prises en charge sont décrites dans le tableau suivant.</dd>
</dl>

  |Image de base Docker|Source des avis de sécurité|
  |-----------------|--------------------------|
  |Alpine|[Git - Alpine Linux ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://git.alpinelinux.org/) et [CIRCL CVE Search ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cve.circl.lu/)|
  |CentOS| [CentOS announce archives ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://lists.centos.org/pipermail/centos-announce/) et [CentOS CR announce archives ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://lists.centos.org/pipermail/centos-cr-announce/)|
  |Debian|[Debian security announcements ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://lists.debian.org/debian-security-announce/)|
  |Red Hat Enterprise Linux (RHEL)|[Red Hat Product Errata ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://access.redhat.com/errata/#/)|
  |Ubuntu|[Ubuntu Security Notices ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ubuntu.com/usn/)|
  {: caption="Tableau 1. Images de base Docker vérifiées par Vulnerability Advisor pour détection de packages vulnérables" caption-side="top"}










## Examen de la sécurité des images Docker stockées dans un espace de nom dans {{site.data.keyword.registrylong_notm}} à l'aide de l'interface de ligne de commande 
{: #va_registry_cli}

Vous pouvez examiner la sécurité des images Docker stockées dans un espace de nom pour consulter des informations sur des vulnérabilités potentielles.
{:shortdesc}

Lorsque vous ajoutez une image à {{site.data.keyword.registrylong_notm}}, celle-ci est automatiquement analysée par Vulnerability Advisor pour détecter des problèmes de sécurité et des vulnérabilités potentielles. Les conteneurs
déployés à partir d'images vulnérables risquent d'être attaqués et compromis. Les images ne sont analysées que si elles sont basées sur un système d'exploitation pris en charge par Vulnerability Advisor.

Vulnerability Advisor vérifie la présence des vulnérabilités suivantes. Si des problèmes de sécurité sont identifiés, des
instructions vous sont soumises afin de vous aider à résoudre la vulnérabilité signalée.

Pour vérifier le statut de vulnérabilité d'images dans votre compte {{site.data.keyword.Bluemix_notm}}, procédez comme suit.

1.  Recensez toutes les images dans votre compte {{site.data.keyword.Bluemix_notm}}. La commande suivante renvoie la liste de toutes les images sans considération de l'espace de nom où elles sont stockées.

    ```
    bx cr image-list
    ```
    {: pre}

2.  Vérifiez le statut dans la colonne Statut de vulnérabilité. L'un des statuts suivants est affiché :
    -   OK. Ce statut indique qu'aucun problème de sécurité n'a été détecté.
    -   Vulnérable. Ce statut indique qu'un problème de sécurité potentiel, ou une vulnérabilité, ont été détectés.
    -   Inconnu. Ce statut reste affiché pendant que l'image est analysée jusqu'à ce que son statut final de vulnérabilité puisse être déterminé.
    -   Système d'exploitation non pris en charge. Ce statut est affiché si l'analyse de l'image n'est pas prise en charge par Vulnerability Advisor.
4.  Pour plus d'informations sur le statut, consultez le rapport de Vulnerability Advisor.

    ```
    bx cr va registry.<region>/<my_namespace>/<my_image>:<tag>
    ```
    {: pre}

    Votre sortie CLI liste les packages vulnérables, soumet une description de la vulnérabilité détectée et un lien vers des instructions pour la corriger.


## Résolution des problèmes dans les images 
{: #va_report}

Exemples de correctifs pour les problèmes courants signalés par Vulnerability Advisor.
{:shortdesc}

Vulnerability Advisor propose des actions correctives
pour les problèmes de sécurité et de configuration signalés. Certains problèmes signalés peuvent être corrigés en mettant à jour votre Dockerfile. Afin de
résoudre plus rapidement des problèmes courants, utilisez les exemples de
code suivants pour implémenter la solution dans votre fichier Dockerfile.

### Age maximal du mot de passe, durée de vie minimale du mot de passe (en jours) et longueur minimale du mot de passe
{: #va_password}

Problème : Vous recevez une erreur ou toutes les erreurs suivantes :

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

Correctif : Appliquez la conformité du mot de passe en ajoutant le code suivant à votre fichier Dockerfile :

```
RUN \
    sed -i 's/^PASS_MAX_DAYS.*/PASS_MAX_DAYS    90/' /etc/login.defs && \
    sed -i 's/^PASS_MIN_DAYS.*/PASS_MIN_DAYS    1/' /etc/login.defs && \
    sed -i 's/sha512/sha512 minlen=8/' /etc/pam.d/common-password
```

### Vulnérabilité SSH 
{: #ssh}

Problème : L'erreur suivante est renvoyée :

```
Le serveur SSH ne doit pas être installé.
```
{: screen}

Correctif : Au lieu d'utiliser SSH, utilisez `docker attach` ou
`docker exec` pour accéder à votre conteneur. Vérifiez que votre fichier Dockerfile ne contient pas d'étapes d'installation de serveur SSH.

