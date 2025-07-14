<h1 class="rem">Sous-système Windows pour Linux</h1>

Le sous-système Windows pour Linux permet aux développeurs d'exécuter l'environnement GNU/Linux - y compris la plupart des outils, utilitaires et applications en ligne de commande - directement sur Windows, sans modification, sans la surcharge d'une machine virtuelle.

Tu peux:

* Choisissez vos distributions GNU/Linux préférées dans le Windows Store.
* Exécutez des logiciels gratuits en ligne de commande courants tels que grep, sed, awk ou d'autres binaires ELF-64.
* Exécutez des scripts shell Bash et des applications de ligne de commande GNU/Linux, notamment :
    * Outils : vim, emacs, tmux
    * Langages : Javascript/node.js, Ruby, Python, C/C++, C# & F#, Rust, Go, etc.
    * Services : sshd, MySQL, Apache, lighttpd
* Installez des logiciels supplémentaires à l'aide de votre propre gestionnaire de packages de distribution GNU/Linux.
* Invoquez des applications Windows à l'aide d'un shell de ligne de commande de type Unix.
* Appelez les applications GNU/Linux sous Windows.

Pour plus d'informations, visitez : [Windows Subsustem for Linux Documentation](https://docs.microsoft.com/en-us/windows/wsl/about).

<h2 id="Installation du sous-système Windows pour Linux">Installation du sous-système Windows pour Linux.
<a href="#Installation du sous-système Windows pour Linux" class="toc-anchor after"></a></h2>

L'installation du sous-système Windows pour Linux est bien décrite par le propre document de Microsoft [Installer le sous-système Windows pour Linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10). Au lieu de la distribution Ubuntu standard mentionnée dans le guide d'installation, recherchez et choisissez la dernière version d'Ubuntu 18.04 LTS.

Pour initialiser et mettre à jour l'installation d'Ubuntu, suivez [Initialisation d'une distribution nouvellement installée](https://docs.microsoft.com/en-us/windows/wsl/initialize-distro). Cette étape peut être ignorée si vous avez déjà initialisé la distribution Ubuntu à l'étape précédente.

<div class = "notice note">
Un aspect important de WSL est que les <strong>outils Windows</strong> ne peuvent <strong>pas</strong> accéder aux fichiers stockés dans Ubuntu. Cependant, Ubuntu peut (presque) librement lire/écrire le système de fichiers Windows. Par conséquent, les fichiers auxquels les outils Windows doivent accéder (par exemple, votre IDE, votre sauvegarde) doivent être stockés sur le système de fichiers Windows.
<p></p>
Lorsque vous accédez au système de fichiers Windows à partir du shell bash, vous devez faire précéder le chemin de <code>/mnt/c/</code>. Bien que cela ne soit pas obligatoire, il est préférable d'utiliser exactement la même casse de chemin de fichier lors de la création de liens symboliques.
</div>

<h2 id="Installation d'Apache">Installation d'Apache.
<a href="#Installation d'Apache" class="toc-anchor after"></a></h2>

Utilisez la commande suivante dans le shell bash pour installer Apache :

    $ | sudo apt install apache2

<div class = "notice tip">
Le terminal utilisé par WSL ne prend pas en charge le collage de texte comme vous en avez l'habitude. Utilisez le <strong>clic droit</strong> pour coller.
</div>

Créez un dossier de projet pour vos sites Web. Pour les raisons mentionnées ci-dessus, ce dossier doit être en dehors du système de fichiers WSL. Vous pouvez utiliser par exemple : `C:/Users/<Username>/Documents/Development/Web/webroot, ou simplement C:/webroot`.

Dans Ubuntu, créez un lien symbolique vers le dossier `webroot`.

    $ | sudo ln -s /mnt/c/your/path/to/webroot /var/www/webroot

Ouvrez le fichier de configuration de l'hôte virtuel par défaut d'Apache :

    $ | sudo nano /etc/apache2/sites-available/000-default.conf

<div class = "notice tip">
Supprimez le contenu existant en maintenant la touche <code>Maj</code> enfoncée et faites défiler vers le bas à l'aide de la touche ↓. Appuyez ensuite sur <code>Ctrl+K</code> pour couper la sélection.
</div>

Insérez la configuration VirtualHost suivante :

```configuration
1  | <VirtualHost *:80>
2  | 
3  |     ServerName localhost
4  | 
5  |     ServerAdmin webmaster@localhost
6  |     DocumentRoot  /var/www/webroot
7  | 
8  |     <Directory /var/www/>
9  |         Options Indexes FollowSymLinks
10 |         AllowOverride All
11 |         Require all granted
12 |     </Directory>
13 | 
14 |     ErrorLog ${APACHE_LOG_DIR}/error.log
15 |     CustomLog ${APACHE_LOG_DIR}/access.log combined
16 | 
17 | </VirtualHost>
```

<div class = "notice tip">
Enregistrez le fichier en appuyant sur <code>Ctrl + O</code> et appuyez sur <code>Entrée</code> pour confirmer. Quittez avec <code>Ctrl+X</code>.
(Dans la barre de commandes : <code>^</code> signifie <code>Ctrl</code> et <code>M</code> signifie <code>Alt</code>)
</div>

Ouvrez votre éditeur/IDE Windows préféré et créez un fichier `index.html` dans votre dossier webroot avec le contenu suivant :

```html
1  | <!DOCTYPE html>
2  | <html lang="en">
3  | <head>
4  |   <meta charset="utf-8">
5  |   <title>It works!</title>
6  | </head>
7  | <body>
8  |   <h1>It works!</h1>
9  | </body>
10 | </html>
```

Démarrez le service Apache :

    $ | sudo service apache2 start

<div class="notice info">
Vous obtiendrez probablement le message d'erreur connu suivant <a href="https://github.com/Microsoft/WSL/issues/1953">que vous pouvez ignorer</a> :
<em>(92) Protocole non disponible : AH00076 : échec de l'activation de APR_TCP_DEFER_ACCEPT</em>
</div>

Ouvrez [http://localhost](http://localhost/) dans votre navigateur et vous devriez voir le texte « Ça marche ! ».

Pour que vos futurs sites Grav fonctionnent correctement, le module Apache `rewrite` doit être activée.

    $ | sudo a2enmod rewrite

<h2 id="Installation de PHP">Installation de PHP.
<a href="#Installation de PHP" class="toc-anchor after"></a></h2>

Utilisez la commande suivante pour installer la dernière version de PHP :

    $ | sudo apt install php

Pour vérifier que PHP est installé et vérifier sa version, exécutez la commande suivante :

    $ | php-v

Vous devriez obtenir une réponse semblable à celle-ci :

```console
PHP 7.2.7-0ubuntu0.18.04.2 (cli) (construit : 4 juillet 2018 16:55:24) ( NTS )
Copyright (c) 1997-2018 Le Groupe PHP
Zend Engine v3.2.0, Copyright (c) 1998-2018 Zend Technologies
```

Pour répondre aux exigences PHP de Grav, quelques extensions PHP supplémentaires doivent être installées :

    $ | sudo apt install php-mbstring php-gd php-curl php-xml php-zip

Redémarrez Apache pour récupérer les modifications :

    $ | sudo service apache2 restart

<h2 id="Installation de Grav">Installation de Grav.
<a href="#Installation de Grav" class="toc-anchor after"></a></h2>

Vous pouvez installer Grav depuis Windows ou depuis Ubuntu.

<h3 id="Option 1 : Windows">Option 1 : Windows.
<a href="#Option 1 : Windows" class="toc-anchor after"></a></h3>

Installez Grav en téléchargeant le package ZIP et en l'extrayant :

1. Téléchargez le dernier et le meilleur package[ Grav](https://getgrav.org/download/core/grav/latest) ou [Grav + Admin](https://getgrav.org/download/core/grav-admin/latest).
2. Extrayez le fichier ZIP dans la racine Web que vous avez créée auparavant.
3. Renommez le dossier extrait en `mysite`.
4. Ouvrez [http://localhost/mysite](http://localhost/mysite) dans le navigateur et vous devriez avoir une installation Grav fonctionnelle.

<h3 id="Option 2 : Ubuntu">Option 2 : Ubuntu.
<a href="#Option 2 : Ubuntu" class="toc-anchor after"></a></h3>

Exécutez les commandes suivantes pour installer Grav dans la racine Web par défaut d'Apache :

```console
$ | wget -O grav.zip https://getgrav.org/download/core/grav/latest
$ | sudo apt install unzip  # unzip is not installed by default on WSL/Ubuntu
$ | unzip grav.zip -d /var/www/webroot
$ | mv /var/www/webroot/grav /var/www/webroot/mysite
```

Ouvrez [http://localhost/mysite](http://localhost/mysite) dans le navigateur et vous devriez avoir une installation Grav fonctionnelle.

Pour d'autres options d'installation, visitez la documentation [d'installation](/installation) de Grav.

<h2 id="Installation de XDebug (facultatif)">Installation de XDebug (facultatif).
<a href="#Installation de XDebug (facultatif)" class="toc-anchor after"></a></h2>

Si vous êtes développeur et que vous souhaitez développer vos propres plugins et thèmes, vous devrez ~~probablement~~ inévitablement déboguer votre code à un moment donné...

Installez XDebug à l'aide de la commande suivante :

    $ | sudo apt install php-xdebug

XDebug doit être activé dans `php.ini`.
Ouvrez l'éditeur :

    $ | sudo nano /etc/php/7.2/apache2/php.ini

Et ajoutez les lignes suivantes à la fin du fichier :

    [XDebug]
    xdebug.remote_enable = 1

<div class = "notice tip">
Dans Nano, vous pouvez utiliser <code>Alt+/</code> pour accéder au bas du fichier.
</div>

Redémarrez Apache :

    $ | sudo service apache2 restart

<h3 id="Activation du débogueur">Activation du débogueur.
<a href="#Activation du débogueur" class="toc-anchor after"></a></h3>

Pour commencer le débogage, vous devez d'abord activer le débogueur sur le serveur. Pour cela, vous devez définir un paramètre spécial GET/POST ou COOKIE. Vous pouvez le faire [manuellement](https://xdebug.org/docs/remote#starting), mais il est beaucoup plus pratique d'utiliser une extension de navigateur. Il vous permet d'activer le débogueur en un clic. Lorsque l'extension est active, elle envoie directement le cookie XDEBUG_SESSION, au lieu de passer par XDEBUG_SESSION_START. Vous trouverez ci-dessous un tableau avec le lien vers l'extension appropriée pour votre navigateur.

| **Navigateur**   | **Extension d'aide**
| :----------- | :------------
| Chrome       | [Xdebug Helper](https://chrome.google.com/extensions/detail/eadndfjplgieldjbigjakmdgkmoaaaoc)
| Firefox      | [Xdebug Helper](https://addons.mozilla.org/en-US/firefox/addon/xdebug-helper-for-firefox/) ou Le [Xdebug le plus simple](https://addons.mozilla.org/en-US/firefox/addon/the-easiest-xdebug/)
| Opera        | [Xdebug launcher](https://addons.opera.com/addons/extensions/details/xdebug-launcher/)

Lorsque vous souhaitez activer/désactiver le débogage d'un site Web, activez simplement "Débogage" dans l'extension du navigateur.

<h3 id="Lancement du débogueur dans Visual Studio Code (facultatif)">Lancement du débogueur dans Visual Studio Code (facultatif).
<a href="#Lancement du débogueur dans Visual Studio Code (facultatif)" class="toc-anchor after"></a></h3>

Lorsque vous utilisez Visual Studio Code, les lanceurs de débogage PHP par défaut ne fonctionnent pas lorsque Apache/PHP s'exécute dans WSL, en raison des mappages de fichiers.

Insérez la configuration suivante dans une [configuration de lancement](https://code.visualstudio.com/docs/editor/debugging#_launch-configurations) PHP déjà créée dans `.vscode/launch.json` :

```yaml
1 | {
2 |     "name": "LSW Listen for XDebug",
3 |     "type": "php",
4 |     "request": "launch",
5 |     "port": 9000,
6 |     "pathMappings": {
7 |         "/mnt/c": "c:/",
8 |     }
9 | }
```

<h2 id="Ajout d'hôtes virtuels supplémentaires (facultatif)">Ajout d'hôtes virtuels supplémentaires (facultatif).
<a href="#Ajout d'hôtes virtuels supplémentaires (facultatif)" class="toc-anchor after"></a></h2>

Au cours des différentes étapes du cycle de vie de notre site (développement, test, production) différentes configurations Grav peuvent être nécessaires. Prenons par exemple la mise en cache ou les pipelines d'actifs. Vous voudrez peut-être les désactiver pendant le développement et les activer lors des tests de performances. Pour plus d'informations, consultez la documentation sur la [configuration automatique de l'environnement](https://learn.getgrav.org/advanced/environment-config#automatic-environment-configuration).

* Démarrez un éditeur en tant qu'administrateur et ouvrez le fichier `C:/Windows/System32/drivers/etc/hosts`. Vous pouvez, par exemple, ajouter les hôtes suivants :

```configuration
    127.0.0.1 mysite-dev
    127.0.0.1 mysite-prod
```

Les hôtes définis dans le fichier d'hôtes Windows seront automatiquement disponibles dans `/etc/hosts` dans WSL/Ubuntu.

* Créez de nouveaux fichiers de configuration VirtualHost dans le dossier `/etc/apache2/sites-available`.

    $ | sudo nano /etc/apache2/sites-available/mysite-dev.conf

Collez ce qui suit dans l'éditeur :

```configuration
1  | <VirtualHost *:80>
2  | 
3  |     ServerName mysite-dev
4  | 
5  |     ServerAdmin webmaster@localhost
6  |     DocumentRoot  /var/www/webroot/mysite
7  | 
8  |     <Directory /var/www/>
9  |         Options Indexes FollowSymLinks
10 |         AllowOverride All
11 |         Require all granted
12 |     </Directory>
13 | 
14 |     ErrorLog ${APACHE_LOG_DIR}/error.log
15 |     CustomLog ${APACHE_LOG_DIR}/access.log combined
16 | 
17 | </VirtualHost>
```

Répétez les commandes ci-dessus pour `mysite-prod.conf` et utilisez `ServerName mysite-prod` comme serveur.

Activez les nouveaux VirtualHosts dans la configuration Apache :

```console
$ | sudo a2ensite mysite-*
$ | sudo service apache2 reload
$ | sudo service apache2 restart
```

Vous pouvez maintenant pointer le navigateur vers [http://mysite-dev](http://mysite-dev/) et il ouvrira l'installation de Grav sur `C:/your/path/to/webroot/mysite` en utilisant les fichiers de configuration dans le dossier `/user/mysite-dev/config/`.

<h2 id="Démarrer automatiquement Apache (facultatif)">Démarrer automatiquement Apache (facultatif).
<a href="#Démarrer automatiquement Apache (facultatif)" class="toc-anchor after"></a></h2>

Pour démarrer et arrêter Apache, des privilèges élevés sont requis. Et pour bénéficier des privilèges élevés, un mot de passe est demandé. Pour empêcher Ubuntu de demander un mot de passe, vous pouvez vous accorder des privilèges élevés permanents pour certains services.

Démarrez l'éditeur [visudo](https://manpages.ubuntu.com/manpages/trusty/man8/visudo.8.html) pour modifier le fichier sudoer :

    $ | sudo visudo -f /etc/sudoers.d/services

Copiez les lignes suivantes dans l'éditeur :

```configuration
%sudo ALL=(root) NOPASSWD: /usr/sbin/service *
%wheel ALL=(root) NOPASSWD: /usr/sbin/service *
```

Apache peut maintenant être démarré avec des privilèges élevés sans fournir de mot de passe.

Pour démarrer Apache à chaque démarrage d'un shell Ubuntu, la commande `sudo service apache2 start` doit être ajoutée au script de démarrage `.bashrc`. Ce script est exécuté chaque fois que vous démarrez un terminal WSL.

    $ | nano .bashrc

Ajoutez le script suivant à la fin du fichier :

```configuration
1 | ## Start apache2 if not running
2 | status=`service apache2 status`
3 | if [[ $status == *"apache2 is not running" ]]
4 | then
5 |   sudo service apache2 start
6 | fi
```

Et ajoutez ce qui suit à `.bash_logout` pour arrêter Apache lors de la fermeture du shell bash.

```configuration
1 | ## Stop apache2 if running
2 | status=`service apache2 status`
3 | if [[ $status == *"apache2 is running" ]]
4 | then
5 |   sudo service apache2 stop
6 | fi
```

<h2 id="Trucs et astuces">Trucs et astuces.
<a href="#Trucs et astuces" class="toc-anchor after"></a></h2>

<h3 id="Émulateur de terminal Linux avec interface graphique">Émulateur de terminal Linux avec interface graphique.
<a href="#Émulateur de terminal Linux avec interface graphique" class="toc-anchor after"></a></h3>

Si vous n'êtes pas fan de l'expérience de terminal par défaut et que vous souhaitez installer un terminal graphique Linux "natif", vous pouvez consulter l'article [Configuration d'un émulateur de terminal joli et utilisable pour WSL](https://blog.ropnop.com/configuring-a-pretty-and-usable-terminal-emulator-for-wsl/).

<h3 id="Plusieurs sites Web, une seule base de code Grav">Plusieurs sites Web, une seule base de code Grav.
<a href="#Plusieurs sites Web, une seule base de code Grav" class="toc-anchor after"></a></h3>

Si vous êtes comme moi et que plusieurs sites Web Grav sont déployés pour des projets distincts, vous voudrez peut-être lire la documentation sur les [liens symboliques](https://learn.getgrav.org/cli-console/command-line-intro#symbolic-links) et sur la [copie d'un projet](https://learn.getgrav.org/cli-console/grav-cli#copying-a-project) pour créer une copie symbolique d'un seul noyau Grav.

