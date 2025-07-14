<h1 class = "rem">Structure des dossiers</h1>

Étant donné que Grav est un **CMS basé sur des fichiers plats**, ce qui signifie qu'aucune base de données n'est utilisée, la structure des dossiers de votre site est très importante. Au niveau supérieur de votre installation Grav, la structure des dossiers ressemble à :

```console
1  | /assets
2  | /backup
3  | /bin
4  | /cache
5  | /images
6  | /logs
7  | /system
8  | /tmp
9  | /vendor
10 | /user
```

Alors approfondissons un peu chacun de ces dossiers de niveau supérieur et expliquons à quoi ils servent :

<h2 id="/assets">/assets
<a href="#/assets" class="toc-anchor after"></a></h2>

Le dossier `assets` est utilisé par le nouveau système de gestion des assets dans Grav pour stocker les fichiers `.css` et `.js` traités.

<div class = "notice info">
Ce dossier ne doit pas être utilisé pour stocker des données utilisateur, car il est systématiquement vidé de toutes les données.
</div>

<h2 id="/backup">/backup
<a href="#/backup" class="toc-anchor after"></a></h2>

Le dossier `backup` est l'emplacement par défaut des sauvegardes Grav.

<h2 id="/bin">/bin
<a href="#/bin" class="toc-anchor after"></a></h2>

Le dossier `bin` contient [l'application Grav CLI](cli-commandes-grav.md) qui peut être utilisée pour effectuer certaines tâches pratiques à partir de la ligne de commande. Il s'agit d'une fonctionnalité relativement avancée principalement destinée aux développeurs, nous allons donc laisser ce sujet de côté pour une discussion ultérieure.

<h2 id="/cache">/cache
<a href="#/cache" class="toc-anchor after"></a></h2>

Le dossier `cache` est utilisé pour stocker des fichiers temporaires en cache qui sont automatiquement générés par Grav pour améliorer les performances. Par défaut, Grav gère automatiquement la mise en cache, en sélectionnant la meilleure option disponible pour votre environnement d'hébergement afin de garantir que votre site fonctionne le plus rapidement possible.

Si Grav décide que le **système de fichiers** est la meilleure méthode de mise en cache, les fichiers mis en cache qu'il génère seront stockés ici. Le moteur de template Twig utilise également cet emplacement pour stocker ses fichiers de template pré-compilés. Encore une fois, cela est fait pour s'assurer que Grav fonctionne à sa vitesse optimale.

<div class = "notice info">
Ce dossier ne doit pas être utilisé pour stocker des données utilisateur, car il est systématiquement vidé de toutes les données.
</div>

<h2 id="/images">/images
<a href="#/images" class="toc-anchor after"></a></h2>

Grav est livré avec une bibliothèque de manipulation d'images puissante mais [très facile à utiliser](./media.md). Cela signifie que vous pouvez facilement redimensionner une image à la volée à partir de votre contenu ou même d'un plugin. Ces images sont stockées dans le dossier `images` afin de pouvoir être réutilisées si la même image avec la même taille est à nouveau demandée.

Ce dossier agit comme un cache d'images et est destiné aux fichiers générés automatiquement. Les médias fournis par l'utilisateur doivent être stockés dans le dossier `user/pages/`, `user/themes/` ou même un dossier personnalisé `user/images/`.

<div class = "notice info">
Ce dossier ne doit pas être utilisé pour stocker des données utilisateur, car il est systématiquement vidé de toutes les données.
</div>

<h2 id="/logs">/logs
<a href="#/logs" class="toc-anchor after"></a></h2>

Lorsque Grav détecte une erreur, ou si vous avez activé la journalisation ou le profilage supplémentaire, il stocke les fichiers journaux pertinents dans le dossier `logs`.

<h2 id="/system">/system
<a href="#/system" class="toc-anchor after"></a></h2>

Le dossier `system` est l'endroit où les fichiers qui font fonctionner Grav en direct. Vous ne devez rien modifier dans ce dossier car une mise à jour de Grav pourrait écraser vos modifications. Si vous avez besoin de changer quelque chose lié au fonctionnement de Grav, vous pouvez utiliser des plugins comme indiqué dans les chapitres suivants.

<h2 id="/tmp">/tmp
<a href="#/tmp" class="toc-anchor after"></a></h2>

Le dossier `tmp` est utilisé par Grav et les plugins pour stocker les fichiers temporaires.

<div class = "notice info">
Ce dossier ne doit pas être utilisé pour stocker des données utilisateur, car il est systématiquement vidé de toutes les données.
</div>

<h2 id="/vendor">/vendor
<a href="#/vendor" class="toc-anchor after"></a></h2>

Le dossier `vendor` contient des bibliothèques importantes sur lesquelles Grav s'appuie. Ce dossier est similaire au dossier `system` en ce sens que son contenu ne doit pas être modifié à moins que vous ne soyez absolument certain de ce que vous faites.

Si vous avez [installé](./installation.md) Grav depuis GitHub, le dossier `vendor` n'aura pas été installé avec. Afin de créer et de remplir le dossier du fournisseur, vous devrez exécuter l'installation de `bin/grav install` ou `composer install` à partir de la racine de votre instance Grav. Plus de détails peuvent être trouvés dans la section [d'installation](./installation.md).

<h2 id="/user">/user
<a href="#/user" class="toc-anchor after"></a></h2>

C'est le dossier le plus important pour la majorité des utilisateurs de Grav. Ce dossier est l'endroit où vous passerez votre temps à créer du contenu, à utiliser des plugins et à éditer vos thèmes. Fouillons un peu plus loin dans ce dossier :

```console
1 | /user/accounts
2 | /user/blueprints
3 | /user/config
4 | /user/data
5 | /user/images
6 | /user/languages
7 | /user/pages
8 | /user/plugins
9 | /user/themes
```

<h2 id="/user/accounts">/user/accounts
<a href="#/user/accounts" class="toc-anchor after"></a></h2>

Le dossier `accounts` est l'endroit où vous définirez les comptes d'utilisateurs si des restrictions d'accès sont requises pour certaines parties de votre site.

<h2 id="/user/blueprints">/user/blueprints
<a href="#/user/blueprints" class="toc-anchor after"></a></h2>

Le dossier `blueprints` contient vos blueprints personnalisés pour le site.

<h2 id="/user/config">/user/config
<a href="#/user/config" class="toc-anchor after"></a></h2>

Les [fichiers du répertoire config](./configuration.md) sont utilisés pour configurer le site Web et ont été abordés dans le chapitre précédent.

<h2 id="/user/data">/user/data
<a href="#/user/data" class="toc-anchor after"></a></h2>

Le dossier `data` peut être utilisé par des plugins pour stocker des données que vous pourrez référencer ultérieurement. Un bon exemple de plugin qui utilise ce dossier est le plugin **Forms** qui peut prendre un formulaire Web et stocker les données soumises dans un fichier texte dans ce dossier. Vous pouvez également stocker d'autres éléments tels que les téléchargements d'utilisateurs ou tout ce que vous souhaitez vraiment.

<div class = "notice info">
Ce dossier n'est pas accessible via un navigateur par défaut.
</div>

<h2 id="/user/images">/user/images
<a href="#/user/images" class="toc-anchor after"></a></h2>

Le dossier `images` peut être utilisé pour stocker vos images. Il est accessible en utilisant `image://` stream.

<h2 id="/user/languages">/user/languages
<a href="#/user/languages" class="toc-anchor after"></a></h2>

Le dossier `languages` contient les [substitutions de traduction](./multi-langue.md).

<h2 id="/user/pages">/user/pages
<a href="#/user/pages" class="toc-anchor after"></a></h2>

C'est le cœur de Grav. Le dossier `pages` est l'endroit où vous créez et modifiez votre contenu. Nous entrerons beaucoup plus en profondeur dans le chapitre suivant.

<h2 id="/user/plugins">/user/plugins
<a href="#/user/plugins" class="toc-anchor after"></a></h2>

Un plugin peut étendre le noyau rapide de Grav avec des fonctionnalités particulières dont vous pourriez avoir besoin pour votre site Web. Les plugins peuvent être téléchargés depuis [GetGrav.org/downloads/plugins](https://getgrav.org/downloads/plugins), ou vous pouvez [développer les vôtres](./plugin-tutoriel.md).

<h2 id="/user/themes">/user/themes
<a href="#/user/themes" class="toc-anchor after"></a></h2>

Un thème transforme votre contenu en un véritable site Web. Il convertit le contenu que vous avez construit en HTML qu'un navigateur comprend et affiche à votre public. Un thème de base est fourni avec Grav, mais vous pouvez également en télécharger d'autres depuis [GetGrav.org/downloads/themes](https://getgrav.org/downloads/themes) ou même créer le vôtre. Le chapitre [Thèmes](themes-base.md) en parlera plus en détail.

