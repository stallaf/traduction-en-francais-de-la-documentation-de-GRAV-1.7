<h1 class="rem">Commande GPM</h1>

Depuis la sortie de la version **0.9.3**, Grav inclut un GPM (Grav Package Manager) qui vous permet d'installer, de mettre à jour, de désinstaller et de lister tous les thèmes et plugins disponibles sur le référentiel Grav, ainsi que de mettre à jour Grav lui-même vers la dernière version .

Comme le [Grav CLI](./cli-commandes-grav.md), le *GPM* est un outil de ligne de commande qui oblige l'utilisateur à exécuter des commandes via une interface de ligne de commande, telle que **Terminal** sous MacOS. Les commandes de style UNIX ne sont pas disponibles en mode natif dans Windows cmd. L'installation du package [msysgit](https://msysgit.github.io/) sur une machine Windows ajoute [Git](https://git-scm.com/) et Git BASH, qui est une invite de commande alternative qui rend les commandes UNIX disponibles.

Pour démarrer avec *GPM*, vous pouvez exécuter la commande suivante pour recevoir une liste de toutes les commandes actuellement disponibles :

```console
$ | bin/gpm list
```

Pour recevoir de l'aide pour une commande spécifique, vous pouvez ajouter de l'aide à la ligne avant la commande :

```console
$ | bin/gpm help install
```

<div class="notice info">
Pour pouvoir effectuer <strong>l'installation</strong>, <strong>la mise à niveau</strong> et <strong>l'auto-mise à niveau</strong>, PHP doit avoir l'extension <code>php_openssl</code> activée. Si vous obtenez une erreur fatale lors du téléchargement, c'est probablement la cause.
</div>

<h3 id="Informations PHP CGI-FCGI">Informations PHP CGI-FCGI.
<a href="#Informations PHP CGI-FCGI" class="toc-anchor after"></a></h3>

Pour déterminer si votre serveur exécute `cgi-fcgi` sur la ligne de commande, tapez ce qui suit :

```console
$ | php -v
  | PHP 5.5.17 (cgi-fcgi) (built: Sep 19 2014 09:49:55)
  | Copyright (c) 1997-2014 The PHP Group
  | Zend Engine v2.5.0, Copyright (c) 1998-2014 Zend Technologies
  |  with the ionCube PHP Loader v4.6.1, Copyright (c) 2002-2014, by ionCube Ltd.
```

Si vous voyez une référence à `(cgi-fcgi)`, vous devrez préfixer toutes les commandes `bin/gpm` avec `php-cli`. Alternativement, vous pouvez configurer un alias dans votre shell avec quelque chose comme : `alias php="php-cli"` qui garantira que la version **CLI** de PHP s'exécute à partir de la ligne de commande.

<h2 id="Comment ça marche ?">Comment ça marche ?
<a href="#Comment ça marche ?" class="toc-anchor after"></a></h2>

*GPM* télécharge les métadonnées du référentiel à partir de **GetGrav.org**. Le référentiel contient tous les détails sur les packages disponibles et GPM est également capable de déterminer si l'un de ces packages est déjà installé et s'il doit être mis à jour.

Le référentiel lui-même est mis en cache localement, sur la machine d'instance Grav exécutant la commande, pendant 24 heures. Toute autre demande après la génération du cache ne contactera pas le serveur **GetGrav.org**, mais servira plutôt à partir du référentiel stocké localement. Cette approche garantit une réponse beaucoup plus rapide.

La plupart des commandes (listées ci-dessous) sont livrées avec l'option `--force (-f)` qui permet de forcer une nouvelle récupération du référentiel. Cela pourrait être extrêmement utile dans le cas où une mise à jour est connue et que l'utilisateur ne veut pas attendre un cycle complet de 24 heures avant que le cache ne soit effacé.

<h2 id="Commandes">Commandes.
<a href="#Commandes" class="toc-anchor after"></a></h2>

Ci-dessous, nous avons décomposé toutes les commandes disponibles pour *GPM*. Pour exécuter une commande, lancez votre application de terminal préférée et depuis la racine de votre instance Grav, vous pouvez taper `bin/gpm <command>`.

<h2 id="Index">Index.
<a href="#Index" class="toc-anchor after"></a></h2>

La commande `index` affiche une liste de toutes les ressources disponibles dans le référentiel Grav, organisées par thèmes et plugins.

<img class="img" src="https://learn.getgrav.org/user/pages/07.cli-console/04.grav-cli-gpm/index.jpg" alt="Commande GPM 1">

Chaque ligne affiche le **nom**, le **slug**, la **version** et s'il est déjà installé ou non.

Dans cette vue, vous pouvez également déterminer rapidement s'il existe une nouvelle version de l'une des ressources que vous avez déjà installées.

Par exemple, si nous avions une très ancienne version d'Antimatière (v1.1.1), mais que la dernière version était la v1.1.3, elle apparaîtra dans l'index comme indiqué ci-dessous.

<img class="img" src="https://learn.getgrav.org/user/pages/07.cli-console/04.grav-cli-gpm/index-outdated.jpg" alt="Commande GPM 2">

<div class="notice info">
Vous pouvez utiliser l'option --installed-only` pour afficher <strong>uniquement</strong> l'état de <strong>vos</strong> plugins et thèmes <strong>installés</strong>.
</div>

<h2 id="Info">Info.
<a href="#Info" class="toc-anchor after"></a></h2>

La commande `info` affiche les détails du package souhaité, tels que la description, l'auteur, la page d'accueil, etc.

<img class="img" src="https://learn.getgrav.org/user/pages/07.cli-console/04.grav-cli-gpm/info.jpg" alt="Commande GPM 3">

<div class="notice info">
Vous serez également invité à consulter le <strong>journal des modification</strong>s du plugin/thème via cette option.
</div>

<h2 id="Installer">Installer.
<a href="#Installer" class="toc-anchor after"></a></h2>

La commande `install` fait exactement ce qu'elle indique. Elle installe une ressource du référentiel sur votre instance Grav actuelle avec une simple commande.

La commande détectera également si une ressource est déjà installée, ou si elle est symboliquement liée, et vous demandera quoi faire.

Vous pouvez également installer plusieurs ressources à la fois en séparant les slugs par un espace.

<iframe width="700" height="400" src="https://www.youtube.com/embed/SUUtcYl2xrE" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<br>
<div class="notice info">
Vous pouvez utiliser l'option <code>--all-yes (-y)</code> pour ignorer les invites. Les ressources existantes seront remplacées et s'il s'agit de liens symboliques, elles seront automatiquement ignorées.
</div>

<h2 id="Mise à jour">Mise à jour.
<a href="#Mise à jour" class="toc-anchor after"></a></h2>

La commande `update` affiche une liste de ressources pouvant être mises à jour et fonctionne de la même manière que pour `install`.

<img class="img" src="https://learn.getgrav.org/user/pages/07.cli-console/04.grav-cli-gpm/update.jpg" alt="Commande GPM 4">

<iframe width="700" height="400" src="https://www.youtube.com/embed/jkxk2xBr5TM" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Vous pouvez également limiter les mises à jour à des ressources spécifiques uniquement.

<img class="img" src="https://learn.getgrav.org/user/pages/07.cli-console/04.grav-cli-gpm/update-limit.jpg" alt="Commande GPM 5">

<iframe width="700" height="400" src="https://www.youtube.com/embed/rSWdmdx9TDA" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<br>
<h2 id="Désinstaller">Désinstaller.
<a href="#Désinstaller" class="toc-anchor after"></a></h2>

La commande `uninstall` supprime un thème ou un plug-in installé et efface le cache. Parce que Grav est purement un système de fichiers, la désinstallation d'un thème ou d'un plugin signifie la suppression physique du dossier.

La commande détectera également si une ressource est symboliquement liée et vous demandera quoi faire.

Vous pouvez également désinstaller plusieurs ressources à la fois en séparant les slugs par un espace.

<div class="notice info">
Vous pouvez utiliser l'option <code>--all-yes (-y)</code> pour ignorer les invites. Si une ressource est détectée comme lien symbolique, elle sera automatiquement ignorée.
</div>

<h2 id="Auto-mise à niveau">Auto-mise à niveau.
<a href="#Auto-mise à niveau" class="toc-anchor after"></a></h2>

Le `self-upgrade` (ou selfupgrade) vous permet de mettre à jour Grav vers la dernière version disponible. Si aucune mise à niveau n'est nécessaire, un message vous le dira, notant également la version que vous utilisez actuellement et la date de publication de la version.

Il est fortement conseillé de toujours faire une sauvegarde avant d'effectuer une auto-mise à jour (voir *Création d'une sauvegarde* dans la [section CLI](cli-commandes-grav.md).

<div class="notice info">
L'auto-mise à niveau ne met à niveau que des parties de votre instance Grav, comme le dossier <code>system/</code>, <code>vendor</code>, <code>index.php</code>, et autres. Vos dossiers <code>user</code> et <code>images</code> ne seront jamais touchés.
</div>

<img class="img" src="https://learn.getgrav.org/user/pages/07.cli-console/04.grav-cli-gpm/upgrade.jpg" alt="Commande GPM 6">

<iframe width="700" height="400" src="https://www.youtube.com/embed/15-E8l5aaUo" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<br>
<h2 id="Informations importantes pour les développeurs">Informations importantes pour les développeurs.
<a href="#Informations importantes pour les développeurs" class="toc-anchor after"></a></h2>

<h3 id="Plans">Plans.
<a href="#Plans" class="toc-anchor after"></a></h3>

Avec l'introduction de *GPM*, nous avons maintenant des règles strictes concernant les `blueprints` valides. Qu'il s'agisse d'un thème ou d'un plugin que vous développez, vous devez toujours vous assurer que les `blueprints` sont correctement formatés.

Un blueprint peut servir plusieurs objectifs différents, y compris la définition de l'identité de votre ressource. Veuillez vous référer aux [Blueprints](formulaires-blueprints.md) pour une documentation plus détaillée sur ce que sont les Blueprints et comment ils doivent être compilés.

<h3 id="Releases">Releases.
<a href="#Releases" class="toc-anchor after"></a></h3>

Le référentiel Grav est actualisé toutes les heures et détecte automatiquement les nouvelles versions, cela implique qu'en tant que développeur, vous avez suivi nos exigences de [contribution](https://github.com/getgrav/grav#contributing).

De votre côté, tout ce que vous avez à faire est de vous assurer que vous avez mis à jour les plans avec la nouvelle version, et que vous avez marqué et publié la nouvelle version. Le référentiel Grav fera le reste pour vous et dès que votre version sera récupérée, elle sera disponible pour tout le monde via le site Web de Grav ou via *GPM*.

<h3 id="Ajouter votre ressource au dépôt.">Ajouter votre ressource au dépôt.
<a href="#Ajouter votre ressource au dépôt." class="toc-anchor after"></a></h3>

<a href="#ndt2" class="ndt">(1)</a> Suivez les instructions de la section [Thème/Plugin Release Process](avance-grav-developpement.md#Processus de publication de thème/plugin).

Pour ajouter votre nouveau plugin/thème au référentiel Grav, veuillez ouvrir un problème Grav sur GitHub. Vous pouvez également [utiliser ce lien précompilé](https://github.com/getgrav/grav/issues/new?title=%5Badd-resource%5D%20New%20Plugin%2FTheme&body=I%20would%20like%20to%20add%20my%20new%20plugin%2Ftheme%20to%20the%20Grav%20Repository.%0AHere%20are%20the%20project%20details%3A%20%2A%2Auser%2Frepository%2A%2A). Assurez-vous de mettre à jour le corps vers `user/repository` approprié.

Plus de détails sur ce que fait le plugin/thème sont les bienvenus et peuvent être placés dans le numéro.

Sachez également qu'avant d'ajouter un référentiel, l'équipe Grav inspectera votre plugin/thème en s'assurant qu'il correspond aux normes Grav. L'équipe peut également répondre avec des demandes d'informations supplémentaires, suggérer des améliorations mineures, etc. avant de fermer le problème et d'ajouter le plugin/thème.

- - -

<div id="ndt2">NDT : J'ai supprimé à cet endroit ce qui semble être une erreur d'affichage de la documentation d'origine <a href ="https://learn.getgrav.org/17/cli-console/grav-cli-gpm#add-your-resource-to-the-repository">advanced/grav-development#themeplugin-release-process</a>.</div>

- - -

