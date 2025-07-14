<h1 class="rem">Commandes Grav</h1>

Grav est livré avec une interface de ligne de commande (CLI) intégrée qui peut être trouvée sur `bin/grav`. La CLI est extrêmement utile pour exécuter des tâches récurrentes telles que **vider le cache**, effectuer des **sauvegardes**, etc.

L'accès à la CLI est un processus simple mais vous devez utiliser un **terminal**. Sur un Mac, cela s'appelle `Terminal`, sur Windows, cela s'appelle `cmd` et sur Linux, c'est juste un shell. Les commandes de style UNIX ne sont pas disponibles en mode natif dans Windows cmd. L'installation du package [msysgit](https://msysgit.github.io/) sur une machine Windows ajoute [Git](https://git-scm.com/) et Git BASH, qui est une invite de commande alternative qui rend les commandes UNIX disponibles. Si vous accédez à votre serveur à distance, vous utiliserez très probablement **SSH** pour vous connecter à distance à votre serveur. Consultez cet [excellent tutoriel pour plus d'informations sur SSH](http://code.tutsplus.com/tutorials/ssh-what-and-how--net-25138).

Bien que certaines opérations puissent être effectuées manuellement, en *s'appuyant* sur la CLI, ces tâches pourraient être automatisées via des *cronjobs* qui s'exécutent quotidiennement.

Pour obtenir une liste de toutes les commandes disponibles dans Grav, vous pouvez lancer la commande :

```console
 $ | bin/grav list
```

Cela devrait afficher quelque chose comme :

```console
Available commands:
  backup       Creates a backup of the Grav instance
  cache        [clearcache|cache-clear] Clears Grav cache
  clean        Handles cleaning chores for Grav distribution
  composer     Updates the composer vendor dependencies needed by Grav.
  help         Displays help for a command
  install      Installs the dependencies needed by Grav. Optionally can create symbolic links
  list         Lists commands
  logviewer    Display the last few entries of Grav log
  new-project  [newproject] Creates a new Grav project with all the dependencies installed
  sandbox      Setup of a base Grav system in your webroot, good for development, playing  
    around or starting fresh
  scheduler    Run the Grav Scheduler.  Best when integrated with system cron
  security     Capable of running various Security checks
```

Pour obtenir de l'aide sur une commande spécifique, vous pouvez ajouter help avant la commande :

```console
 $ | bin/grav help install
```

<h2 id="Sauvegarde">Sauvegarde.
<a href="#Sauvegarde" class="toc-anchor after"></a></h2>

Le système de sauvegarde Grav a été complètement remanié dans Grav 1.6 pour prendre en charge plusieurs profils de sauvegarde. Ces profils sont configurés dans le fichier `user/config/backups.yaml`. Si vous n'avez pas de fichier de configuration personnalisé, Grav utilisera celui par défaut fourni dans `system/config/backups.yaml`.

Si Grav détecte plusieurs profils de sauvegarde, la commande CLI vous demandera de choisir celui que vous souhaitez sauvegarder avec la commande CLI.

```console
$ | cd ~/workspace/portfolio
  | bin/grav backup
  |  
  | Grav Backup
  | ===========
  |  
  | Choose a backup?
  |   [0] Default Site Backup
  |   [1] Pages Backup
```

Alternativement, vous pouvez passer directement un index du profil :

```console
$ | cd ~/workspace/portfolio
  | bin/grav backup 1
  |  
  | Archiving 36 files [=======================================] 100% < 1 sec Done...
  |   
  | [OK] Backup Successfully Created: /users/joe/workspace/portfolio/backup/  
  |   pages_backup--20190227120510.zip
```

Vous trouverez plus d'informations sur la fonctionnalité de sauvegarde dans la section [Avancé -> Sauvegardes](./avance-sauvegardes.md).

<h2 id="Nettoyer">Nettoyer.
<a href="#Nettoyer" class="toc-anchor after"></a></h2>

Cette commande CLI est principalement utilisée pendant le processus de construction du package, car elle supprime les fichiers et dossiers superflus de Grav. Il est fortement recommandé **de ne pas l'utilise**r vous-même, sauf si vous l'utilisez pour créer vos propres packages Grav.

```console
 $ | bin/grav clean
```

<h2 id="Vider le cache">Vider le cache.
<a href="#Vider le cache" class="toc-anchor after"></a></h2>

Vous pouvez vider le cache en supprimant tous les fichiers et dossiers sous `cache/`.

La commande CLI équivalente est :

```console
 $ | cd ~/webroot/my-grav-project
 $ | bin/grav cache
```    

Il existe plusieurs alias pour la compatibilité (`cache`, `cache-clear`, `clearcache`, `clear`).

L'option par défaut est le processus standard d'effacement du cache, mais vous pouvez le contrôler davantage avec ces options :

```console
 --purge           If set purge old caches
 --all             If set will remove all including compiled, twig, doctrine caches
 --assets-only     If set will remove only assets/*
 --images-only     If set will remove only images/*
 --cache-only      If set will remove only cache/*
 --tmp-only        If set will remove only tmp/*
```

<h2 id="Composer">Composer.
<a href="#Composer" class="toc-anchor after"></a></h2>

Si vous avez installé Grav via GitHub et que vous avez installé manuellement des packages de fournisseurs basés sur composer, vous pouvez facilement mettre à jour avec :

```console
 $ | bin/grav composer
```

Vous pouvez également passer des options de composition telles que `install` :

```console
 $ | bin/grav composer --install
```

ou alors

```console
 $ | bin/grav composer --update
```

<div class="notice info">
Ceux-ci utilisent tous l'option <code>--no-dev</code> composer, donc pour pouvoir effectuer des tests, vous devez utiliser composer directement : <code>bin/composer.phar</code>.
</div>

<h2 id="Installer">Installer.
<a href="#Installer" class="toc-anchor after"></a></h2>

Pour installer les dépendances sur lesquelles Grav s'appuie (plugin **error**, plugin ** problems**, thème **antimatter**), lancez un **terminal** ou une **console** et accédez au dossier grav où vous souhaitez installer les dépendances et exécutez la commande CLI.

```console
 $ | cd ~/webroot/my-grav-project
 $ | bin/grav install
```

Vous devriez maintenant avoir les dépendances installées sous :

* ~/webroot/my-grav-project/user/plugins/error
* ~/webroot/my-grav-project/user/plugins/problems
* ~/webroot/my-grav-project/user/themes/antimatter

<h2 id="Visionneuse de journal">Visionneuse de journal.
<a href="#Visionneuse de journal" class="toc-anchor after"></a></h2>

Dans le cadre de Grav 1.6, une nouvelle commande CLI logviewer a été créée pour permettre une visualisation rapide des journaux Grav.

La façon la plus simple d'utiliser cette commande est de taper simplement :

```console
$ | cd ~/webroot/my-grav-project
$ | bin/grav logviewer
```

Cela affichera les 20 dernières entrées de journal du fichier `logs/grav.log`. Il y a quelques options :

```txt
1 | -f, --file[=FILE]     custom log file location (default = grav.log)
2 | -l, --lines[=LINES]   number of lines (default = 10)
3 | -v, --verbose         verbose output including a stack trace if available
```

par exemple.

```console
$ | bin/grav logviewer --lines=4
  | 
  | Log Viewer
  | ==========
  |
  | viewing last 4 entries in grav.log
  |
  | 2019-02-27 12:00:30 [WARNING] Plugin 'foo-plugin' enabled but not found! Try clearing cache with `bin/grav cache`
  | 2019-02-27 12:04:57 [NOTICE] Backup Created: /Users/joe/my-grav-project/backup/default_site_backup--20190227120450.zip
  | 2019-02-27 12:05:10 [NOTICE] Backup Created: /Users/joe/my-grav-project/backup/pages_backup--20190227120510.zip
  | 2019-02-27 12:26:00 [NOTICE] Backup Created: /Users/joe/my-grav-project/backup/pages_backup--20190227122600.zip
```
Et une sortie détaillée avec des traces de pile :

```console
$ | bin/grav logviewer -v
  |
  | Log Viewer
  | ==========
  |
  | viewing last 20 entries in grav.log
  |
  | 2019-03-14 05:52:44 [WARNING] Plugin 'simplesearch.bak' enabled but not found! Try clearing cache with `bin/grav clear-cache`
  | 2019-03-14 05:52:44 [CRITICAL] A function must be an instance of \Twig_FunctionInterface or \Twig_SimpleFunction.
  | 0 /Users/joe/my-grav-project/plugins/acme-twig-filters/acme-twig-filters.php(52): Twig\Environment->addFunction(Object(Twig\TwigFilter))
  | 1 /Users/joe/my-grav-project/vendor/symfony/event-dispatcher/EventDispatcher.php(212): Grav\Plugin\ACMETwigFiltersPlugin->onTwigInitialized(Object(RocketTheme\Toolbox\Event\Event), 'onTwigInitializ...', Object(RocketTheme\Toolbox\Event\EventDispatcher))
  | 2 /Users/joe/my-grav-project/vendor/symfony/event-dispatcher/EventDispatcher.php(44): Symfony\Component\EventDispatcher\EventDispatcher->doDispatch(Array, 'onTwigInitializ...', Object(RocketTheme\Toolbox\Event\Event))
  | 3 /Users/joe/my-grav-project/vendor/rockettheme/toolbox/Event/src/EventDispatcher.php(23): Symfony\Component\EventDispatcher\EventDispatcher->dispatch('onTwigInitializ...', Object(RocketTheme\Toolbox\Event\Event))
  | 4 /Users/joe/my-grav-project/system/src/Grav/Common/Grav.php(365): RocketTheme\Toolbox\Event\EventDispatcher->dispatch('onTwigInitializ...', Object(RocketTheme\Toolbox\Event\Event))
  | 5 /Users/joe/my-grav-project/system/src/Grav/Common/Twig/Twig.php(175): Grav\Common\Grav->fireEvent('onTwigInitializ...')
  | 6 /Users/joe/my-grav-project/system/src/Grav/Common/Processors/TwigProcessor.php(24): Grav\Common\Twig\Twig->init()
  | 7 /Users/joe/my-grav-project/system/src/Grav/Framework/RequestHandler/Traits/RequestHandlerTrait.php(45): Grav\Common\Processors\TwigProcessor->process(Object(Nyholm\Psr7\ServerRequest), Object(Grav\Framework\RequestHandler\RequestHandler))
  | 8 /Users/joe/my-grav-project/system/src/Grav/Framework/RequestHandler/Traits/RequestHandlerTrait.php(57): Grav\Framework\RequestHandler\RequestHandler->handle(Object(Nyholm\Psr7\ServerRequest))
  | 9 /Users/joe/my-grav-project/system/src/Grav/Common/Processors/AssetsProcessor.php(28): Grav\Framework\RequestHandler\RequestHandler->handle(Object(Nyholm\Psr7\ServerRequest))
  | 
  | 2019-03-14 05:52:46 [WARNING] Plugin 'simplesearch.bak' enabled but not found! Try clearing cache with `bin/grav clear-cache`
  | ...
```

<h2 id="Nouveau projet">Nouveau projet.
<a href="#Nouveau projet" class="toc-anchor after"></a></h2>

Chaque fois que vous souhaitez démarrer un nouveau projet avec Grav, vous devez commencer avec une instance Grav propre. Grâce à la CLI, ce processus est très simple et ne prend que quelques secondes.

1.Lancez un **terminal** ou une **console** et accédez au dossier grav (pour les besoins de ce document, nous supposerons qu'il réside sous `~/Projects/grav`).

```console
 $ | cd ~/Projects/grav
```

2.Exécutez Grav CLI pour créer un nouveau projet, la destination étant l'emplacement où votre projet résidera (généralement la [racine](https://en.wikipedia.org/wiki/Webroot) Web de votre serveur Web). Supposons que nous créons un **portfolio** et que nous le voulons à `~/Webroot/portfolio`.

```console
 $ | bin/grav new-project ~/webroot/portfolio
```

Cela créera une nouvelle instance Grav et téléchargera toutes les dépendances requises.

<h2 id="Bac à sable">Bac à sable.
<a href="#Bac à sable" class="toc-anchor after"></a></h2>

Grav a un utilitaire astucieux appelé `sandbox`, qui peut rapidement créer une copie <a href="../cli-intro">symbolique</a> de l'installation de Grav. En termes simples, l'exécution de `bin/grav sandbox -s DESTINATION` - où "DESTINATION" est le chemin d'accès au dossier dans lequel vous souhaitez copier l'installation - recrée l'installation Grav dans un autre dossier.

Par exemple, en exécutant :

```console
 $ | bin/grav sandbox -s ../copy
```

À partir de votre dossier Grav actuel, créez un dossier frère nommé `copy`, où les dossiers suivants sont des copies virtuelles : `/bin`, `/system`, `/vendor`, `/webserver-configs`, ainsi que des fichiers standard qui résident généralement dans le dossier racine de Grav. Tout le contenu de `/user` sera des copies carbone, et non virtuelles, vous pouvez donc facilement commencer à personnaliser la nouvelle installation sans avoir créé de surcharge à partir des fichiers principaux.

<h2 id="Planificateur">Planificateur.
<a href="#Planificateur" class="toc-anchor after"></a></h2>

Comme indiqué dans la section <a href="../avance-planificateur">Avancé -> Planificateur</a>, le planificateur peut être surveillé via la commande CLI.

La commande de base exécutera manuellement les tâches du planificateur qui sont dues :

```console
 $ | bin/grav scheduler
``` 

Pour obtenir plus de détails, vous pouvez utiliser l'option facultative `-v` :

```console
$ | bin/grav scheduler -v
  |
  | Running Scheduled Jobs
  | ======================
  |
  | [2019-02-27T12:34:07-07:00] Success: Grav\Common\Cache::purgeJob
  | [2019-02-27T12:34:07-07:00] Success: Grav\Common\Cache::clearJob
  | [2019-02-27T12:34:07-07:00] Success: ls -lah
``` 

Les autres options incluent :

```console
 -i, --install         Show Install Command
 -j, --jobs            Show Jobs Summary
 -d, --details         Show Job Details
```

Veuillez vous référer à la section <a href="../avance-planificateur">Avancé -> Planificateur</a>, pour des informations plus détaillées sur ces options.

<h2 id="Sécurité">Sécurité.
<a href="#Sécurité" class="toc-anchor after"></a></h2>

Ajouté dans Grav 1.5 est une nouvelle commande CLI du scanner de sécurité. Vous pouvez l'exécuter pour analyser rapidement votre contenu par rapport aux <a href="../configuration/#Securité">paramètres de sécurité configurés</a>.

```console
$ | bin/grav security
  | 
  | Grav Security Check
  | ===================
  |
  | Scanning 11 pages [===================================================] 100% < 1 sec
  |
  | [OK] Security Scan complete: No issues found...
```

<h2 id="Informations PHP CGI-FCGI">Informations PHP CGI-FCGI.
<a href="#Informations PHP CGI-FCGI" class="toc-anchor after"></a></h2>

Pour déterminer si votre serveur exécute `cgi-fcgi` sur la ligne de commande, tapez ce qui suit :

```console
$ | $ php -v
  | PHP 5.5.17 (cgi-fcgi) (built: Sep 19 2014 09:49:55)
  | Copyright (c) 1997-2014 The PHP Group
  | Zend Engine v2.5.0, Copyright (c) 1998-2014 Zend Technologies
  |    with the ionCube PHP Loader v4.6.1, Copyright (c) 2002-2014, by ionCube Ltd.
```

Si vous voyez une référence à `(cgi-fcgi)`, vous devrez préfixer toutes les commandes `bin/grav` avec `php-cli`. Alternativement, vous pouvez configurer un alias dans votre shell avec quelque chose comme : `alias php="php-cli"` qui garantira que la version **CLI** de PHP s'exécute à partir de la ligne de commande.

