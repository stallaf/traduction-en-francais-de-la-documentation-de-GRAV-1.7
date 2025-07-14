<h1 class="rem">Hébergement Web Crucial</h1>

[Crucial Web Hosting](http://www.crucialwebhost.com/promo/1421086/) est un autre des nouveaux pains des plates-formes d'hébergement Web modernes qui se concentre à la fois sur la vitesse et le support. L'utilisation de disques SSD et de serveurs Web Litespeed avec les derniers processeurs Intel XEON garantit que Grav fonctionne de manière fantastique. Crucial fournit également PHP jusqu'aux dernières versions de PHP 7.0.

![Crucial](https://learn.getgrav.org/user/pages/09.webservers-hosting/01.shared/03.crucial/crucial.png)

Dans ce guide, nous couvrirons les éléments essentiels pour configurer le package d'hébergement **Tier-1 Split-Shared** pour qu'il fonctionne de manière optimale avec Grav.

<h2 id="Choisir votre plan d'hébergement">Choisir votre plan d'hébergement.
<a href="#Choisir votre plan d'hébergement" class="toc-anchor after"></a></h2>

[L'hébergement Web Crucial](http://www.crucialwebhost.com/promo/1421086/) propose deux options principales en matière d'hébergement : l'hébergement **Spit-Shared** et **Split-Dedicated**. Selon Crucial, ces options basées sur le cloud sont supérieures aux configurations d'hébergement traditionnelles car elles offrent une meilleure isolation et de meilleures performances.

L'hébergement partagé partagé varie de 10 $/mois à 100 $/mois selon la mémoire et l'espace SSD. Split Dedicated varie de 150 $/mois à 650 $/mois en fonction du nombre de cœurs, de la mémoire, de l'espace SSD et de la bande passante. Nous n'utiliserons que l'option de base de 10 $/mois fournie avec 256 Mo de mémoire et 10 Go d'espace SSD.

<h2 id="Activation de SSH">Activation de SSH.
<a href="#Activation de SSH" class="toc-anchor after"></a></h2>

Tout d'abord, vous devrez ouvrir l'option **Basculer l'accès SSH** dans la section **Sécurité** de cPanel. Sur cette page d'accès SSH, vous devez cliquer sur le bouton **Activer l'accès SSH**.

Ensuite, dans la **section Sécurité**, cliquez à nouveau sur l'option **Gérer les clés SSH**.

![Gérer SSH](https://learn.getgrav.org/user/pages/09.webservers-hosting/01.shared/03.crucial/manage-ssh-keys.png)

Il y a deux options à ce stade. **Générez une nouvelle clé** ou **importez une clé**. Il est plus simple de créer votre paire de clés publique/privée localement sur votre ordinateur, puis d'importer simplement la clé publique DSA.

<div class = "notice info">
Les utilisateurs de Windows devront d'abord installer <a href = "https://www.cygwin.com/">Cygwin</a> pour fournir de nombreux outils GNU et open source utiles disponibles sur les plates-formes Mac et Linux. Lorsque vous êtes invité à choisir des packages, assurez-vous de cocher l'option SSH. Après l'installation, lancez le <code>Cygwin Terminal</code>.
</div>

Lancez une fenêtre de terminal et tapez :

    $ | ssh-keygen -t dsa

Ce script de génération de clé vous invitera à remplir certaines valeurs, ou vous pouvez simplement appuyer sur `[return]` pour accepter les valeurs par défaut. Cela créera un `id_dsa` (clé privée) et un `id_dsa.pub` (clé publique) dans un dossier appelé `.ssh/` dans votre répertoire personnel. Il est important de vous assurer que vous ne donnez **JAMAIS** votre clé privée, ni ne la téléchargez nulle part, **uniquement votre clé publique**.

Une fois généré, vous pouvez coller le contenu de votre clé publique `id_dsa.pub` dans le champ `Public Key` de la section **Import SSH key** de la page **SSH Access** :

![Accés SSH](https://learn.getgrav.org/user/pages/09.webservers-hosting/01.shared/03.crucial/ssh-public-key.png)

Après le téléchargement, vous devriez voir la clé répertoriée dans la section **Clés publiques** de la page Gérer les clés SSH. Vous devez ensuite cliquer sur **Gérer** pour vous assurer que la clé est autorisée :

Cela signifie que vous êtes prêt à tester ssh sur votre serveur.

    $ | ssh crucial_username@crucial_servername

Évidemment, vous devrez entrer votre nom d'utilisateur fourni par Crucial pour ``crucial_username`` et le nom de serveur fourni par crucial pour ``crucial_servername``.

<h2 id="Configurer PHP">Configurer PHP.
<a href="#Configurer PHP" class="toc-anchor after"></a></h2>

Actuellement, Crucial Web Hosting utilise par défaut **PHP 5.3**, ce qui n'est pas à la hauteur des exigences minimales pour Grav. Heureusement, Crucial prend en charge PHP jusqu'à la dernière version de PHP **7.0**, nous changeons donc la version de PHP pour quelque chose de plus actuel.

Pour ce faire, nous devons ajouter un appel de gestionnaire spécial dans le fichier `.htaccess` à la racine Web. Créez donc le fichier `~/www/.htaccess` et mettez ce qui suit :

    $ | Application AddHandler/x-httpd-php70 .php

Enregistrez le fichier. Pour tester que vous avez la **bonne version de PHP**, vous pouvez créer un fichier temporaire : `~/www/info.php` et le mettre dans le contenu :

    <?php phpinfo();

Enregistrez le fichier et pointez votre navigateur vers ce fichier info.php sur votre site, et vous devriez être accueilli avec des informations PHP reflétant la version que vous avez sélectionnée précédemment :

![version PHP](https://learn.getgrav.org/user/pages/09.webservers-hosting/01.shared/03.crucial/php-info.png)

<div class = "notice info">
Si vous installez Grav à la racine de votre compte d'hébergement, vous devrez ajouter la méthode <strong>AddHandler</strong> en haut du fichier <code>.htaccess</code> fourni avec Grav.
</div>

<div class = "notice tip">
Vous pouvez choisir une autre version de php pour exécuter Grav sous en utilisant comme PHP 5.6 avec <code>x-httpd-php56</code> par exemple.
</div>

<h2 id="Configurer l'interface de ligne de commande PHP">Configurer l'interface de ligne de commande PHP.
<a href="#Configurer l'interface de ligne de commande PHP" class="toc-anchor after"></a></h2>

Au moment d'écrire ces lignes, la version PHP par défaut de Crucial est la **5.3**. Étant donné que Grav nécessite PHP **5.5+**, nous devons nous assurer que Grav utilise une version plus récente de PHP sur la ligne de commande (CLI). Pour ce faire, vous devez utiliser SSH pour accéder à votre serveur et créer un nouveau lien symbolique vers une version plus récente de PHP dans le dossier `bin/` de votre utilisateur :

    $ | ln -s /usr/local/bin/php-70 ~/bin/php

Ensuite, modifiez votre fichier `.bash_profile` et ajoutez la référence `$HOME/bin` devant la chaîne `$PATH` normale :

```bash
1 | # .bash_profile
2 |
3 | # Get the aliases and functions
4 | if [ -f ~/.bashrc ]; then
5 |         . ~/.bashrc
6 | fi
7 |
8 | # User specific environment and startup programs
9 |
10| PATH=$HOME/bin:$PATH
11|
12| export PATH
```
Vous aurez besoin de sourcer le profil : `$ source ~/.bash_profile` ou de vous reconnecter à votre terminal pour que votre changement de chemin prenne effet, mais après cela, vous devriez pouvoir taper `php -v` et voir :

```console
$ | php -v
  | PHP 7.0.1 (cli) (built: Dec 28 2015 17:55:36) ( NTS )
  | Copyright (c) 1997-2015 The PHP Group
  | Zend Engine v3.0.0, Copyright (c) 1998-2015 Zend Technologies
  |    with Zend OPcache v7.0.6-dev, Copyright (c) 1999-2015, by Zend Technologies
```

<h2 id="Installer et tester Grav">Installer et tester Grav.
<a href="#Installer et tester Grav" class="toc-anchor after"></a></h2>

En utilisant vos nouvelles capacités SSH trouvées, connectons-nous en SSH à votre serveur Crucial (si vous n'y êtes pas déjà) et téléchargez la dernière version de Grav, décompressez-la et testez-la !

Nous allons extraire Grav dans un sous-dossier `/grav`, mais vous pouvez décompresser directement à la racine de votre dossier `~/www/` pour vous assurer que Grav est accessible directement.

```console
$ | cd ~/www
$ | wget --no-check-certificate https://getgrav.org/download/core/grav/latest
$ | unzip grav-v1.7.28.zip
```

Vous devriez maintenant pouvoir pointer votre navigateur vers `http://mycrucialserver.com/grav` en utilisant bien sûr l'URL appropriée.

Parce que vous avez suivi ces instructions avec diligence, vous pourrez également utiliser les commandes [Grav CLI](/cli-commandes-grav) et [Grav GPM](/cli-commanes-gpm) telles que :

```console
$ | cd ~/public_html/grav
$ | bin/grav clear-cache
  |
  | Clearing cache
  |
  | Cleared:  cache/twig/*
  | Cleared:  cache/doctrine/*
  | Cleared:  cache/compiled/*
  | Cleared:  cache/validated-*
  | Cleared:  images/*
  | Cleared:  assets/*
  |
  | Touched: /home/your_user/public_html/grav/user/config/system.yaml
```

