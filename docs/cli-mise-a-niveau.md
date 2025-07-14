<h1 class="rem">Mises à niveau scriptées</h1>

Ou : mettre à niveau plusieurs installations Grav à la fois.

Ceci est un guide pour faciliter la mise à niveau de plusieurs installations Grav, en utilisant [Deployer](https://deployer.org/). Par multiples, j'entends des installations séparées, pas une [installation multisite](avance-config-multisite.md). Nous utiliserons le chemin d'accès à chaque installation pour exécuter les commandes [CLI de Grav](cli-commandes-grav.md), mais sans avoir à les saisir manuellement.

L'un des avantages d'un gestionnaire de tâches tel que Deployer est qu'au fur et à mesure qu'il parcourt ses tâches, il vous permet de savoir exactement ce qu'il fait en cours de route. Il vous montrera également toute sortie du serveur à partir des commandes en cours d'exécution. Par exemple, voici une sortie de Deployer :

```console
Executing task packages

GPM Releases Configuration: Stable

Found 8 packages installed of which 1 need updating

01. Email           [v2.5.2 -> v2.5.3]

GPM Releases Configuration: Stable

Preparing to install Email [v2.5.3]
  |- Downloading package...   100%
  |- Checking destination...  ok
  |- Installing package...    ok
  '- Success!

Clearing cache

Cleared:  /home/username/public_html/deployer/grav/cache/twig/*
Cleared:  /home/username/public_html/deployer/grav/cache/doctrine/*
Cleared:  /home/username/public_html/deployer/grav/cache/compiled/*

Touched: /home/username/public_html/deployer/grav/user/config/system.yaml
```

Et aussi simple que cela, Deployer a dit à Grav de mettre à niveau tous les packages, ce qui a mis à niveau le plugin Email vers sa dernière version.

<h2 id="Conditions préalables">Conditions préalables.
<a href="#Conditions préalables" class="toc-anchor after"></a></h2>

Comme avec Grav, vous avez besoin de PHP **v7.3.6** ou supérieur. Cela s'applique également à la ligne de commande (CLI), donc si vous avez plusieurs versions installées, utilisez celle qui fait référence à la bonne version. Utilisez la commande `php -v` pour vérifier votre version par défaut, la mienne est PHP **7.2.34**.

Dans les environnements partagés, vérifiez auprès de votre hôte quelle commande utiliser pour la CLI. Dans mon cas, c'est `php74` qui avec `-v` renvoie PHP **7.4.12**. Cela signifie également préfixer chaque chemin comme ceci : `php74 vendor/bin/dep list`.

Certains hébergeurs vous permettent également de sélectionner votre version PHP par défaut à utiliser pour la CLI, vérifiez auprès de votre hébergeur comment procéder.

<h2 id="Réglages">Réglages.
<a href="#Réglages" class="toc-anchor after"></a></h2>

Dans la racine publique de vos serveurs (comme **public_html** ou **www**), créez un dossier nommé `deployer` et entrez dedans. Nous allons l'utiliser comme base pour le projet. Vous voudrez protéger ce répertoire par mot de passe (voir le [guide DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-set-up-password-authentication-with-apache-on-ubuntu-14-04) pour une approche manuelle, ou utiliser [CPanel](https://www.siteground.com/tutorials/cpanel/pass_protected_directories.htm) si disponible).

Vous devez avoir une installation fonctionnelle de Grav, ainsi que de [Composer](https://getcomposer.org/). Composer est déjà installé sur certains hôtes, ce que vous pouvez vérifier en exécutant `composer -v`. S'il n'est pas installé, vous pouvez l'installer via SSH -- à partir du répertoire racine -- avec ce qui suit :

```console
$ | export COMPOSERDIR=~/bin;mkdir bin
  | curl -sS https://getcomposer.org/installer | php -- --install-dir=$COMPOSERDIR --filename=composer
```

Ou, si vous préférez une [installation locale](https://getcomposer.org/download/), exécutez ce qui suit dans le dossier `public_html/deployer/-` :

```console
$ | php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
  | php composer-setup.php
  | php -r "unlink('composer-setup.php');"
```

Avec une installation locale, composer est exécuté via `composer.phar` plutôt que simplement `composer`. Maintenant, tout en restant dans le dossier `public_html/deployer/`, exécutez ce qui suit pour installer [Deployer](https://deployer.org/docs/installation) :

```console
$ | composer require deployer/deployer
```

Maintenant, toujours dans le même dossier, créez un fichier nommé `deploy.php`. Nous l'utiliserons pour exécuter chaque tâche avec Deployer. Copiez et collez ce qui suit dans le fichier :

```php
1  | <?php
2  | namespace Deployer;
3  | require 'vendor/autoload.php';
4  |
5  | // Configuration
6  | set('default_stage', 'production');
7  | set(php, 'php56');
8  | 
9  | // Servers
10 | localServer('site1')
11 |     ->stage('production')
12 |     ->set('deploy_path', '/home/username/public_html/deployer/grav');
13 |
14 | desc('Backup Grav installations');
15 | task('backup', function () {
16 |     $backup = run('{{php}} bin/grav backup');
17 |     writeln($backup);
18 | });
19 | desc('Upgrade Grav Core');
20 | task('core', function () {
21 |     $self_upgrade = run('{{php}} bin/gpm self-upgrade -y');
22 |     writeln($self_upgrade);
23 | });
24 | desc('Upgrade Grav Packages');
25 | [ task('packages', function () {
26 |     $upgrade = run('{{php}} bin/gpm update -y');
27 |     writeln($upgrade);
28 | });
29 | ?>
```

<h2 id="Configuration">Configuration.
<a href="#Configuration" class="toc-anchor after"></a></h2>

Étant donné que Deployer a besoin d'un environnement de transfert explicite, nous le définissons dans `production`. De plus, pour permettre une version php spécifique, nous définissons un exécutable par défaut à utiliser. Il peut s'agir d'un exécutable nommé ou du chemin vers une version spécifique de PHP. Notre configuration ressemble maintenant à ceci :

```console
1 | // Configuration
2 | set('default_stage', 'production');
3 | set(php, 'php56');
```

Si votre version par défaut de PHP CLI est **5.6*** ou supérieure, vous changez ceci en `set(php, 'php');`.

<h3 id="Les serveurs">Les serveurs.
<a href="#Les serveurs" class="toc-anchor after"></a></h3>

Nous pouvons configurer autant de serveurs/sites que nécessaire, le script sera exécuté pour chacun d'eux dans l'ordre. Il peut s'agir d'installations locales ou sur des serveurs externes, mais comme il s'agit d'une configuration locale, nous utilisons `localServer` (voir [Deployer/servers](https://deployer.org/docs/servers) pour plus de configurations). Voici un exemple avec plusieurs sites :

```console
1  | // Servers
2  | localServer('site1')
3  |     ->stage('production')
4  |     ->set('deploy_path', '/home/username/public_html/deployer/grav1');
5  | localServer('site2')
6  |     ->stage('production')
7  |     ->set('deploy_path', '/home/username/public_html/deployer/grav2');
8  | localServer('site3')
9  |     ->stage('production')
10 |     ->set('deploy_path', 'C:\caddy\grav1');
11 | localServer('site4')
12 |     ->stage('production')
13 |     ->set('deploy_path', 'C:\caddy\grav2');
```
 
 Notez que nous utilisons des chemins absolus vers les installations, mais ils sont relatifs à la façon dont le chemin est interprété par SSH. Cela signifie que sur le serveur, nous voulons le chemin complet car Deployer l'interprète correctement, mais nous pourrions également utiliser la clé tilde si un HOMEDIR est défini, comme ceci : `~/public_html/deployer/grav1`.
 
<h3 id="Tâches">Tâches.
<a href="#Tâches" class="toc-anchor after"></a></h3> 

Trois tâches sont actuellement configurées : `backup`, `core` et `packages`. L'exécution de `php vendor/bin/dep backup` crée une sauvegarde de chaque installation, disponible dans **deploy_path/backup/BACKUP.zip**, où `deploy_path` correspond aux chemins que vous avez configurés pour les serveurs.

L'exécution de `php vendor/bin/dep core` met à niveau Grav lui-même, et le fait avec l'option `--all-yes` pour ignorer toutes les questions Oui/Non posées. Il en va de même lors de l'exécution de `php vendor/bin/dep packages`, qui mettent à niveau tous les plugins et thèmes.

<h2 id="Conclusion">Conclusion.
<a href="#Conclusion" class="toc-anchor after"></a></h2> 

Nous pouvons désormais facilement mettre à niveau tous vos sites définis en exécutant les tâches dans l'ordre. Nous entrons d'abord dans le dossier `public_html/deployer/`, puis nous exécutons chaque commande selon les besoins :

```console
$ | php vendor/bin/dep backup
  | php vendor/bin/dep core
  | php vendor/bin/dep packages
```

Nous aurons maintenant fait une sauvegarde de chaque site, mis à jour Grav lui-même, ainsi que mis à jour tous les plugins et thèmes.   

