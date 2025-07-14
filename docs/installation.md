<h1 class="rem">Installation</h1>

L'installation de Grav est un processus trivial. En fait, il n'y a pas vraiment d'installation. Vous avez plusieurs options pour installer Grav. La première - et la plus simple - consiste à télécharger l'archive **zip** et à l'extraire. La deuxième méthode consiste à installer avec **Composer**. La troisième méthode consiste à cloner le projet source directement à partir de **GitHub**, puis à exécuter une commande de script incluse pour installer les dépendances nécessaires. Il existe <a href="#Options supplémentaires">d'autres façons</a> d'exécuter des scripts groupés.

<h2 id="Vérifier la version PHP">Vérifier la version PHP
<a href="#Vérifier la version PHP" class="toc-anchor after"></a></h2>

Grav est incroyablement facile à installer et à faire fonctionner. Assurez-vous d'avoir au moins la version PHP 7.3.6+ en allant sur le terminal et en tapant `php -v` :

```bash
$ | php-v
  | PHP 7.3.18 (cli) (built : Jun 5 2020 11:06:30) ( NTS )
  | Copyright (c) 1997-2018 The PHP Group
  | Zend Engine v3.3.18, Copyright (c) 1998-2018 Zend Technologies
  |     with Zend OPcache v7.3.18, Copyright (c) 1999-2018, by Zend Technologies
```

<h2 id="Option 1 : Installer à partir du package ZIP">Option 1 : Installer à partir du package ZIP
<a href="#Option 1 : Installer à partir du package ZIP" class="toc-anchor after"></a></h2>

Le moyen le plus simple d'installer Grav est de télécharger le package ZIP et de l'extraire :

   1. Téléchargez le dernier package [Grav](https://getgrav.org/download/core/grav/latest) ou [Grav + Admin](https://getgrav.org/download/core/grav-admin/latest).
   2. Extrayez le fichier ZIP dans le [webroot](https://www.wordnik.com/words/webroot) de votre serveur Web, par ex. `~/webroot/grav`

<div class = "notice tip">Il existe des packages <a href = "https://getgrav.org/downloads/skeletons">Skeleton</a> disponibles, qui incluent le système principal Grav, des exemples de pages, des plugins et la configuration. Ils sont un excellent moyen de commencer; tout ce que vous avez à faire est de <a href = "https://getgrav.org/downloads/skeletons">télécharger le Skeleton</a> que vous préférez et de suivre les étapes ci-dessus.
</div>

Vous pouvez également télécharger n'importe quelle installation pré-emballée d'une [version étiquetée](https://github.com/getgrav/grav/tags) à partir de getgrav.org. Utilisez le format `https://getgrav.org/download/TYPE/PACKAGE/VERSION.`

   * [getgrav.org/download/core/grav/1.7.0](https://getgrav.org/download/core/grav/1.7.0) téléchargements Grav Core v1.7.0
   * [getgrav.org/download/core/grav/1.7.0-rc.10?testing=true](https://getgrav.org/download/core/grav/1.7.0-rc.10?testing=true) télécharge Grav Core v1.7.0-rc.10, une version de test
   * [getgrav.org/download/core/grav-admin/1.7.0](https://getgrav.org/download/core/grav-admin/1.7.0) télécharge Grav Core avec le plugin Admin, à Core v1.7.0
   * [getgrav.org/download/core/grav-admin/1.7.0-rc.10?testing=true](https://getgrav.org/download/core/grav-admin/1.7.0-rc.10?testing=true) télécharge Grav Core v1.7.0-rc.10 avec le plugin Admin, une version de test
   * [getgrav.org/download/core/grav-update/1.7.0](https://getgrav.org/download/core/grav-update/1.7.0) télécharge le package de mise à jour pour Grav Core
   * [getgrav.org/download/plugins/flex-objects-json/0.1.0](https://getgrav.org/download/plugins/flex-objects-json/0.1.0) télécharge le plugin Flex Objects JSON à v0.1.0
   * [getgrav.org/download/themes/quark/2.0.3](https://getgrav.org/download/themes/quark/2.0.3) télécharge le thème Quark en v2.0.3

<div class = "notice warning">Si vous avez téléchargé le fichier ZIP et que vous prévoyez ensuite de le déplacer vers votre racine Web, veuillez déplacer le <strong>DOSSIER ENTIER</strong> car il contient plusieurs fichiers cachés (tels que .htaccess) qui ne seront pas sélectionnés par défaut. L'omission de ces fichiers cachés peut causer des problèmes lors de l'exécution de Grav.
</div>

<h2 id="Option 2 : Installer avec Composer">Option 2 : Installer avec Composer
<a href="#Option 2 : Installer avec Composer" class="toc-anchor after"></a></h2>

La méthode alternative consiste à installer Grav avec [composer](https://getcomposer.org/doc/00-intro.md#installation-linux-unix-osx) :

```bash
$ | composer create-project getgrav/grav ~/webroot/grav
```

Si vous souhaitez découvrir la version à la pointe de la technologie de Grav, ajoutez `1.x-dev` comme paramètre supplémentaire :

```bash
$ | composer create-project getgrav/grav ~/webroot/grav 1.x-dev
```

<h2 id="Installer depuis GitHub">Installer depuis GitHub
<a href="#Installer depuis GitHub" class="toc-anchor after"></a></h2>

Une autre méthode consiste à cloner Grav à partir du référentiel GitHub, puis à exécuter un simple script d'installation de dépendance :

   1. Clonez le référentiel Grav de [GitHub](https://github.com/getgrav/grav) dans un dossier de la racine Web de votre serveur, par ex. `~/webroot/grav`. Lancez un **terminal** ou une **console** et accédez au dossier Webroot :

```bash
$ | cd ~/webroot
$ | git clone -b master https://github.com/getgrav/grav.git
```

   2. Installez les **dépendances du fournisseur** via [composer](https://getcomposer.org/doc/00-intro.md#installation-linux-unix-osx) :

```bash
$ | cd ~/webroot/grav
$ | composer install --no-dev -o
```

   3. Installez les **dépendances du plugin** et du **thème** en utilisant [l'application Grav CLI](./cli-commandes-grav.md) `bin/grav` 

```bash
$ | cd ~/webroot/grav
$ | bin/grav install
```

Cela **clonera** automatiquement les dépendances requises de GitHub directement dans cette installation Grav.

<h2 id="Options supplémentaires">Options supplémentaires
<a href="#Options supplémentaires" class="toc-anchor after"></a></h2>

<h3 id="Installer avec Docker">Installer avec Docker
<a href="#Installer avec Docker" class="toc-anchor after"></a></h3>

[Docker](https://en.wikipedia.org/wiki/Docker_(software)) est un moyen extrêmement efficace de démarrer des platesformes et des services sur les serveurs et les environnements locaux. Si vous configurez plusieurs environnements qui doivent être identiques ou si vous travaillez en collaboration, il offre un moyen simple d'assurer la cohérence entre les installations. Si vous développez plusieurs sites Grav, vous pouvez rationaliser leur configuration à l'aide de Docker.

Des images Docker stables sont disponibles qui utilisent les serveurs Web [Apache](https://github.com/getgrav/docker-grav) (l'image officielle), [Nginx](https://github.com/dsavell/docker-grav) et [Caddy](https://github.com/hughbris/grav-daddy). Si vous recherchez, vous trouverez plus que vous pouvez essayer. Avec n'importe quelle image, assurez-vous de créer des volumes pour conserver les dossiers `user`, `backups` et `logs` de Grav. (Les sauvegardes et les journaux sont facultatifs si vous n'avez pas besoin de conserver ces données.)

<h3 id="Installer sur Cloudron">Installer sur Cloudron
<a href="#Installer sur Cloudron" class="toc-anchor after"></a></h3>

Cloudron est une solution complète pour exécuter des applications sur votre serveur et les maintenir à jour et sécurisées. Sur votre Cloudron, vous pouvez installer Grav en quelques clics. Si vous hébergez plusieurs sites, vous pouvez les installer complètement isolés les uns des autres sur le même serveur.

<a href="https://cloudron.io/store/org.getgrav.cloudronapp.html"><img src="https://cloudron.io/img/button.svg" class="img"></a>

Le code source du paquet peut être trouvé [ici](https://git.cloudron.io/cloudron/grav-app).

<h3 id="Installer via Linode Marketplace">Installer via Linode Marketplace
<a href="#Installer via Linode Marketplace" class="toc-anchor after"></a></h3>

Si vous utilisez des serveurs Linode, il existe une [méthode simple et documentée utilisant Linode Marketplace](https://www.linode.com/docs/guides/grav-marketplace-app/). Cela mettra en place un nouveau site Grav sur un nouveau serveur virtuel Linode dédié. Le serveur virtuel encourra des frais périodiques.

<h2 id="Serveurs Web">Serveurs Web
<a href="#Serveurs Web" class="toc-anchor after"></a></h2>

<h3 id="Apache/IIS/Nginx">Apache/IIS/Nginx
<a href="#Apache/IIS/Nginx" class="toc-anchor after"></a></h3>

Utiliser Grav avec un serveur Web tel qu'Apache, IIS ou Nginx est aussi simple que d'extraire Grav dans un dossier sous [webroot](https://www.wordnik.com/words/webroot). Tout ce dont il a besoin pour fonctionner est PHP 7.3.6 ou supérieur, vous devez donc vous assurer que votre instance de serveur répond à cette exigence. Vous trouverez plus d'informations sur les prérequis de Grav dans le chapitre des [prérequis](./prerequis.md) de ce guide.

Si votre webroot est, par exemple, `~/public_html`, vous pouvez l'extraire dans ce dossier et y accéder via `http://localhost`. Si vous l'avez extrait dans `~/public_html/grav`, vous y accéderez via `http://localhost/grav`.

<div class = "notice tip">
Chaque serveur Web doit être configuré. Grav est livré avec .htaccess par défaut, pour Apache, et est livré avec certains <a href = "https://github.com/getgrav/grav/tree/master/webserver-configs">fichiers de configuration de serveur par défaut</a>, pour <code>nginx</code>, <code>caddy server</code>, <code>iis</code> et <code>lighttpd</code>. Utilisez-les en conséquence si nécessaire.
</div>

<h3 id="Exécuter Grav avec le serveur Web PHP intégré">Exécuter Grav avec le serveur Web PHP intégré
<a href="#Exécuter Grav avec le serveur Web PHP intégré" class="toc-anchor after"></a></h3>

Vous pouvez exécuter Grav à l'aide d'une simple commande depuis Terminal / Invite de commandes en utilisant le serveur PHP intégré disponible tant que PHP est installé.

Tout ce que vous avez à faire est d'accéder à la racine de votre installation Grav à l'aide du terminal ou de l'invite de commande et de taper `bin/grav server`.

<div class = "notice info">
Bien que techniquement tout ce dont vous avez besoin est PHP installé, si vous installez <a href = "https://symfony.com/download">l'application Symfony CLI</a>, le serveur fournira un certificat SSL afin que vous puissiez utiliser <code>https://</code> et utiliser PHP-FPM pour de meilleures performances.
</div>

La saisie de cette commande vous présentera une sortie semblable à la suivante :

```bash
$ | bin/grav server
  |
  | Grav Web Server
  | ===============
  |
  |Tailing Web Server log file (/Users/joeblow/.symfony/log/ 96e710135f52930318e745e901e4010d0907cec3.log)
  | Tailing PHP-FPM log file (/Users/joeblow/.symfony/log/96e710135f52930318e745e901e4010d0907cec3/53fb8ec204547646acb3461995e4da5a54cc7575.log)
  | Tailing PHP-FPM log file (/Users/joeblow/.symfony/log/96e710135f52930318e745e901e4010d0907cec3/53fb8ec204547646acb3461995e4da5a54cc7575.log)
  | 
  | [OK] Web server listening
  | The Web server is using PHP FPM 8.0.8
  | https://127.0.0.1:8000
  | 
  | [Web Server ] Jul 30 14:54:53 |DEBUG  | PHP    Reloading PHP versions
  | [Web Server ] Jul 30 14:54:54 |DEBUG  | PHP    Using PHP version 8.0.8 (from default version in $PATH)
  | [PHP-FPM    ] Jul  6 14:40:17 |NOTICE | FPM    fpm is running, pid 64992
  | [PHP-FPM    ] Jul  6 14:40:17 |NOTICE | FPM    ready to handle connections
  | [PHP-FPM    ] Jul  6 14:40:17 |NOTICE | FPM    fpm is running, pid 64992
  | [PHP-FPM    ] Jul  6 14:40:17 |NOTICE | FPM    ready to handle connections
  | [Web Server ] Jul 30 14:54:54 |INFO   | PHP    listening path="/usr/local/Cellar/php/8.0.8_2/sbin/php-fpm" php="8.0.8" port=65140
  | [PHP-FPM    ] Jul 30 14:54:54 |NOTICE | FPM    fpm is running, pid 73709
  | [PHP-FPM    ] Jul 30 14:54:54 |NOTICE | FPM    ready to handle connections
  | [PHP-FPM    ] Jul 30 14:54:54 |NOTICE | FPM    fpm is running, pid 73709
  | [PHP-FPM    ] Jul 30 14:54:54 |NOTICE | FPM    ready to handle connections
```

Votre terminal vous donnera également des mises à jour en temps réel de toute activité sur ce serveur. Vous pouvez copier l'URL fournie dans la ligne `[OK] Web server listening` et la coller dans le navigateur de votre choix pour accéder à votre site, y compris l'administrateur.

`https://127.0.0.1:8000`

<div class = "notice warning">
Il s'agit d'un outil utile pour un développement rapide et <strong>ne doit pas</strong> être utilisé à la place d'un serveur Web dédié tel qu'Apache ou Nginx.
</div>

<h2 id="Installation réussie">Installation réussie
<a href="#Installation réussie" class="toc-anchor after"></a></h2>

La première fois qu'il se charge, Grav pré-compile certains fichiers. Si vous actualisez maintenant votre navigateur, vous obtiendrez une version en cache plus rapide.

![installation réussie](https://learn.getgrav.org/user/pages/01.basics/03.installation/install.png)

<div class = "notice info">
Dans les exemples précédents, <strong>$</strong> représente l'invite de commande. Cela peut sembler différent sur différentes plates-formes.
</div>

Par défaut, Grav est livré avec des exemples de pages pour vous donner un point de départ. Votre site est déjà entièrement fonctionnel et vous pouvez le configurer, ajouter du contenu, l'étendre ou le personnaliser autant que vous le souhaitez.

<h2 id="Problèmes d'installation et de configuration">Problèmes d'installation et de configuration
<a href="#Problèmes d'installation et de configuration" class="toc-anchor after"></a></h2>

Si des problèmes sont découverts lors du chargement initial de la page (ou après un événement de vidage du cache), une page d'erreur peut s'afficher :

![problème](https://learn.getgrav.org/user/pages/01.basics/03.installation/problems.png)

Veuillez consulter la section [Dépannage](./depannage-404.md) pour obtenir de l'aide concernant des problèmes spécifiques.

<div class = "notice note">
Si vous rencontrez des problèmes avec les autorisations de fichiers, veuillez consulter la <a href="/depannage-autorisations">documentation de dépannage des autorisations</a>. Vous pouvez également consulter la <a href="/web-nginx">documentation des guides d'hébergement"</a> qui contient des instructions spécifiques pour divers environnements d'hébergement.
</div>

<h2 id="Mises à jour Grav">Mises à jour Grav
<a href="#Mises à jour Grav" class="toc-anchor after"></a></h2>

Pour garder votre site à jour, veuillez lire [Mise à jour de Grav & Plugins](./maj-grav.md).

