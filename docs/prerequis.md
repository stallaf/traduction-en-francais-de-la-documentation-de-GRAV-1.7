<h1 class="rem">Prérequis</h1>

Grav est volontairement conçu avec peu de prérequis. Vous pouvez facilement exécuter Grav sur votre ordinateur local, ainsi que chez 99% des fournisseurs d'hébergement Web. Si vous avez un stylo à portée de main, notez les exigences suivantes du système Grav :

   1. Serveur Web (Apache, Nginx, LiteSpeed, Lightly, IIS, etc.)
   2. PHP 7.3.6 ou supérieur  
   3. hmm... c'est vraiment ça, (mais veuillez regarder les exigences PHP pour une expérience fluide) !

Grav est construit avec des fichiers texte brut pour votre contenu. Aucune base de données n'est nécessaire.

<div class = "notice info">Un cache utilisateur PHP tel que APCu, Memcached ou Redis est fortement recommandé pour des performances optimales. Ne vous inquiétez pas cependant, ceux-ci font généralement déjà partie de votre pack d'hébergement !
</div>

<h2 id="Serveurs Web">Serveurs Web
<a href="#Serveurs Web" class="toc-anchor after"></a></h2>

Grav est si simple et polyvalent que vous n'avez même pas besoin d'un serveur Web pour l'exécuter. Vous pouvez l'exécuter directement à partir du serveur Web PHP intégré, tant que vous exécutez **PHP 7.3.6** ou une version ultérieure.

Tester avec les serveurs Web intégrés est un moyen utile de vérifier une installation de Grav et d'effectuer un bref développement, mais il n'est pas recommandé pour un site en direct ou même pour des tâches de développement avancées. Nous avons décrit comment dans notre [guide d'Installation](./installation.md).

Même si techniquement vous n'avez pas besoin d'un serveur Web autonome, il est préférable d'en exécuter un, même pour le développement local. Il existe de nombreuses options intéressantes :

<h3 id="Mac">Mac
<a href="#Mac" class="toc-anchor after"></a></h3>

   * MacOS 10.14 Mojave est déjà livré avec le serveur Web Apache et PHP 7.1, alors le travail est fait !
   * [MAMP/MAMP Pro](https://www.mamp.info/en/mac/) est livré avec Apache, MySQL et bien sûr PHP. C'est un excellent moyen de mieux contrôler la version de PHP que vous utilisez, de configurer des hôtes virtuels, ainsi que d'autres fonctionnalités utiles telles que la gestion automatique du DNS dynamique.
   * [AMPPS](http://www.ampps.com/downloads) est une pile logicielle de Softaculous permettant d'utiliser Apache, PHP, Perl, Python, .. Cela inclut tout ce dont vous avez besoin (et plus) pour le développement GRAV.
   * [Brew Apache/PHP](https://getgrav.org/blog/macos-mojave-apache-multiple-php-versions) est une approche alternative qui permet une installation entièrement configurable avec différentes versions de PHP.

<h3 id="Windows">Windows
<a href="#Windows" class="toc-anchor after"></a></h3>

   * [Laragon](https://laragon.org/), environnement de développement universel portable, isolé, rapide et puissant pour PHP, Node.js, etc. Il est rapide, léger, facile à utiliser et à étendre.
   * [XAMPP](https://www.apachefriends.org/index.html) fournit Apache, PHP et MySQL dans un package simple.
   * [EasyPHP](http://www.easyphp.org/) fournit un pack d'hébergement Web personnel ainsi qu'une version développeur plus puissante.
   * [MAMP pour Windows](https://mamp.info/) est un favori Mac de longue date, mais maintenant disponible pour Windows.
   * [IIS avec PHP](http://php.iis.net/) est un moyen rapide d'exécuter PHP sous Windows.
   * [AMPPS](https://www.ampps.com/downloads/) est une pile logicielle de Softaculous permettant d'utiliser Apache, PHP, Perl, Python, .. Cela inclut tout ce dont vous avez besoin (et plus) pour le développement GRAV.
   * [Linux Subsystem](https://medium.freecodecamp.org/setup-a-php-development-environment-on-windows-subsystem-for-linux-wsl-9193ff28ae83) est un excellent moyen d'exécuter un environnement de type Linux sous Windows

<h3 id="Linux">Linux
<a href="#Linux" class="toc-anchor after"></a></h3>

   * De nombreuses distributions de Linux sont déjà livrées avec Apache et PHP intégrés. Si ce n'est pas le cas, la distribution fournit généralement un gestionnaire de packages à travers lequel vous pouvez les installer sans trop de tracas. Des configurations plus avancées doivent être étudiées à l'aide d'un bon moteur de recherche.

<h3 id="Prérequis pour Apache">Prérequis pour Apache
<a href="#Prérequis pour Apache" class="toc-anchor after"></a></h3>

Même si la plupart des distributions d'Apache sont livrées avec tout le nécessaire, par souci d'exhaustivité, voici une liste des modules Apache requis :

   * `mod_rewrite`
   * `mod_ssl` (si vous souhaitez exécuter Grav sous SSL).
   * `mod_mpm_itk_module` (si vous souhaitez exécuter Grav sous son propre compte utilisateur).

Vous devez également vous assurer que `AllowOverride All` est défini dans les blocs `<Directory>` et/ou `<VirtualHost>` afin que le fichier `.htaccess` soit traité correctement et que les règles de réécriture prennent effet.

<h3 id="Prérequis pour IIS">Prérequis pour IIS
<a href="#Prérequis pour IIS" class="toc-anchor after"></a></h3>

Bien qu'IIS soit considéré comme un serveur Web prêt à fonctionner et "prêt à l'emploi", certaines modifications doivent être apportées.

Pour faire fonctionner **Grav** sur un serveur IIS, vous devez installer **URL Rewrite**. Cela peut être accompli à l'aide de **Microsoft Web Platform Installer** à partir d'IIS. Vous pouvez également installer URL Rewrite en allant sur [iis.net](https://www.iis.net/downloads/microsoft/url-rewrite).

<h3 id="Prérequis pour PHP">Prérequis pour PHP
<a href="#Prérequis pour PHP" class="toc-anchor after"></a></h3>

La plupart des fournisseurs d'hébergement et même les configurations LAMP locales ont PHP préconfiguré avec tout ce dont vous avez besoin pour que Grav fonctionne "prêt à l'emploi". Cependant, certaines configurations Windows, et même les distributions Linux locales ou sur VPS (je vous laisse regarder Debian !) - sont livrées avec une compilation PHP très minimale. Par conséquent, vous devrez peut-être installer ou activer ces modules PHP :

   * `curl` (client pour la gestion des URL utilisé par GPM).
   * `ctype` (utilisé par symfony/yaml/Inline).
   * `dom` (utilisé par le fil d'actualité grav/admin).
   * `gd` (une bibliothèque graphique utilisée pour manipuler des images).
   * `json` (utilisé par Symfony/Composer/GPM).
   * `mbstring` (prise en charge des chaînes multioctets).
   * `openssl` (bibliothèque de sockets sécurisés utilisée par GPM).
   * `session` (utilisé par toolbox).
   * `simplexml` (utilisé par le fil d'actualité grav/admin).
   * `xml` (prise en charge XML).
   * `zip` prise en charge de l'extension zip (utilisée par GPM).

Pour activer le support `openssl` et (un)zip, vous devrez trouver dans le fichier `php.ini` de votre distribution Linux des lignes comme :

```php
1 | ;extension=openssl.so
2 | ;extension=zip.so
```

et supprimez le point-virgule initial.

<h4 id="Modules optionnels">Modules optionnels
<a href="#Modules optionnels" class="toc-anchor after"></a></h4>

   * `apcu` pour des performances de cache accrues.
   * `opcache` pour des performances PHP accrues.
   * `yaml` PECL Yaml fournit un traitement yaml natif et peut augmenter considérablement les performances.
   * `xdebug` utile pour le débogage dans un environnement de développement.

<h3 id="Autorisations">Autorisations
<a href="#Autorisations" class="toc-anchor after"></a></h3>

Pour que Grav fonctionne correctement, votre serveur Web doit disposer des **autorisations de fichier**s appropriées pour écrire les journaux, des caches, etc. Lors de l'utilisation de la [CLI](./cli-commandes-grav.md)   (Interface en Ligne de Commande) ou du [GPM](./cli-commandes-gpm.md) (Gestionnaire de Package Grav), l'utilisateur exécutant PHP à partir de la ligne de commande doit également disposer des autorisations appropriées pour modifier les fichiers.

Par défaut, Grav s'installera, respectivement, avec les autorisations `644` et `755` pour les fichiers et les dossiers. La plupart des fournisseurs d'hébergement ont des configurations qui garantissent qu'un serveur Web exécutant PHP vous permettra de créer et de modifier des fichiers dans votre compte d'utilisateur. Cela signifie que Grav est **prêt à l'emploi** sur la grande majorité des fournisseurs d'hébergement.

Toutefois, si vous utilisez un serveur dédié ou même votre environnement local, vous devrez peut-être ajuster les autorisations pour vous assurer que votre **utilisateur** et votre **serveur Web** peuvent modifier les fichiers selon les besoins. Il existe plusieurs approches que vous pouvez adopter.

   1. Dans un **environnement de développement local**, vous pouvez généralement configurer votre serveur Web pour qu'il s'exécute sous votre profil utilisateur. Ainsi, le serveur Web vous permettra toujours de créer et de modifier des fichiers.

   2. Modifiez les **autorisations de groupe** sur tous les fichiers et dossiers afin que le groupe du serveur Web ait un accès en écriture aux fichiers et dossiers tout en conservant les autorisations standard. Cela nécessite quelques commandes pour que cela fonctionne.

Tout d'abord, vérifiez avec quel utilisateur Apache s'exécute en exécutant la commande suivante :

```bash
$ | ps aux | grep -v root | grep apache | cut -d\ -f1 | sort | uniq
```

Maintenant, découvrez à quel groupe appartient cet utilisateur en exécutant cette commande (remarque : ajustez USERNAME avec le nom d'utilisateur apache que vous avez trouvé dans la commande précédente)

```bash
$ | groups USERNAME
```

(remarque : ajustez `GROUP` pour qu'il corresponde au groupe sous lequel votre apache s'exécute, trouvé dans la commande précédente. [`www-data, apache, nobody,` etc.]) :

``` bash
1 | chgrp -R GROUP .
2 | find . -type f | xargs chmod 664
3 | find ./bin -type f | xargs chmod 775
4 | find . -type d | xargs chmod 775
5 | find . -type d | xargs chmod +s
6 | umask 0002
```

Si vous avez besoin d'invoquer des autorisations de superutilisateur, vous exécuterez `find … | sudo xargs chmod …` à la place.

<h2 id="Outils recommandés">Outils recommandés
<a href="#Outils recommandés" class="toc-anchor after"></a></h2>

<h3 id="PhpStorm">PhpStorm
<a href="#PhpStorm" class="toc-anchor after"></a></h3>

Grav est développé à l'aide de [PhpStorm](https://www.jetbrains.com/phpstorm/), ce qui en fait le meilleur IDE pour Grav. Cependant, cela ne vient pas comme çà.

PhpStorm est le mieux adapté aux développeurs PHP, y compris les personnes qui écrivent des plugins Grav compliqués. Il offre une compilation de code automatisée pour Grav (il vous suffit d'ajouter Grav et tout plugin que vous utilisez dans les inclusions), et de nombreux autres outils pour aider au développement du code. Il prend également en charge le formatage de twig, yaml, html, js, scss et tailwind.

<h3 id="Éditeurs de texte">Éditeurs de texte
<a href="#Éditeurs de texte" class="toc-anchor after"></a></h3>

Bien que vous puissiez vous en sortir avec le Bloc-notes, Textedit, Vi ou tout autre éditeur de texte par défaut fourni avec votre plateforme, nous vous recommandons d'utiliser un bon éditeur de texte avec coloration syntaxique pour faciliter les choses. Voici quelques options recommandées :

   1. [Visual Studio Code](https://code.visualstudio.com/) - Semblable à Atom, il est construit à l'aide d'Electron, de Node, ainsi que de HTML/CSS. Il est assez léger et dispose de nombreux plugins, dont un très bon support pour PHP et JavaScript. Il s'agit de l'éditeur actuellement recommandé pour développer pour Grav.
   2. [Atom](https://atom.io/) - MacOS/Windows/Linux - Un nouvel éditeur développé par Github. C'est gratuit et open source. Il est similaire à Sublime, mais n'a pas encore la profondeur des plugins disponibles.
   3. [SublimeText](https://www.sublimetext.com/) - MacOS/Windows/Linux - Un éditeur de développeur commercial, mais qui vaut bien le prix. Très puissant surtout combiné avec des plugins tels que [Markdown Extended](https://sublime.wbond.net/packages/Markdown%20Extended), [Pretty YAML](https://sublime.wbond.net/packages/Pretty%20YAML) et [PHP-Twig](https://sublime.wbond.net/packages/PHP-Twig).
   4. [Notepad++](https://notepad-plus-plus.org/) - Windows - Un éditeur gratuit et très populaire pour Windows.
   5. [Bluefish](http://bluefish.openoffice.nl/index.html) - MacOS/Windows/Linux - Un éditeur de texte open source gratuit destiné aux programmeurs et aux développeurs Web.
   6. [Kate](https://kate-editor.org/about-kate/) - MacOS/Windows/Linux - Un éditeur de texte et outil de programmation open source léger mais puissant et polyvalent, prenant en charge la surbrillance dans plus de 300 langues (y compris Markdown).

<h3 id="Éditeurs Markdown">Éditeurs Markdown
<a href="#Éditeurs Markdown" class="toc-anchor after"></a></h3>

Une autre option si vous travaillez principalement avec la création de contenu consiste à utiliser un **éditeur Markdown** dédié. Ceux-ci sont souvent très centrés sur le contenu et fournissent généralement un aperçu en direct de votre contenu rendu au format HTML. Il y en a littéralement des centaines, mais certaines bonnes options incluent :

   1. [MacDown](http://macdown.uranusjr.com/) - MacOS - Gratuit, un éditeur Markdown open source simple et léger.
   2. [LightPaper](https://getlightpaper.com/) - MacOS - 17,99 $, propre, puissant. Notre éditeur Markdown de choix sur Mac. **Obtenez 25 % de réduction avec le code de réduction : GET_GRAV_25**
   3. [MarkDrop](http://culturezoo.com/markdrop/) - MacOS - 5 $, mais super propre et prise en charge Droplr intégrée.
   4. [MarkdownPad](http://markdownpad.com/) - Windows - Versions gratuites et Pro. Posséde même la prise en charge du front-matter YAML. Une excellente solution pour les utilisateurs de Windows.
   5. [Mark Text](https://marktext.app/) - Éditeur Markdown gratuit et open source pour Windows / Linux / MacOS.

<h3 id="Client FTP">Client FTP
<a href="#Client FTP" class="toc-anchor after"></a></h3>

Bien qu'il existe de nombreuses façons de déployer **Grav**, il vous suffit fondamentalement de copier votre site local sur votre hébergeur. La façon la plus simple de le faire est d'utiliser un [client FTP](https://en.wikipedia.org/wiki/File_Transfer_Protocol). Il en existe de nombreux, mais certains sont recommandés :

   1. [Transmit](https://panic.com/transmit/) - MacOS - Le client FTP/SFTP de facto sur MacOS. Facile à utiliser, rapide, synchronisation des dossiers et à peu près tout ce que vous pourriez demander.
   2. [FileZilla](https://filezilla-project.org/) - MacOS/Windows/Linux - Probablement la meilleure option pour les utilisateurs Windows et Linux. Gratuit et très puissant (mais très moche sur Mac !).
   3. [Cyberduck](https://cyberduck.io/) - MacOS/Windows - Une option gratuite décente pour les utilisateurs MacOS et Windows. Pas aussi complet que les autres.
   4. [ForkLift](http://www.binarynights.com/forklift/) - MacOS - Une alternative solide à Transmit, et légèrement moins chère au démarrage.

<h3 id="Git">Git
<a href="#Git" class="toc-anchor after"></a></h3>

Si vous exécutez le système de contrôle de version distribué [Git](https://git-scm.com/) sur vos environnements de développement et de serveur, vous pouvez configurer un flux de travail simple via un service Git hébergé tel que [Github](https://github.com/) ou [GitLab](https://about.gitlab.com/). C'est un peu plus de travail à configurer, mais cela fournit un flux de travail plus propre et plus robuste qui s'occupe des sauvegardes pour vous. N'essayez ceci que si vous êtes à l'aise avec Git et ses outils clients.

<div class = "notice tip">Nous fournissons plus de détails sur l'utilisation de Git dans votre flux de travail ultérieurement dans la section <a href="../server-deploy-git">Déploiement avec Git</a> du chapitre <a href="../web-nginx">Serveurs Web et hébergement</a>.
</div>

