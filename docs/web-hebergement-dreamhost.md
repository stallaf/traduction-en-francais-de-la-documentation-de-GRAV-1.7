<h1 class="rem">Dreamhost</h1>

[Dreamhost](https://dreamhost.com/) est un célèbre fournisseur d'hébergement qui propose différents niveaux de service allant de l'hébergement partagé alimenté par SSD aux serveurs dédiés.

![Dreamhost](https://learn.getgrav.org/user/pages/09.webservers-hosting/01.shared/06.dreamhost/dreamhost.png)

Concentrons-nous sur l'offre bas de gamme, l'hébergement mutualisé. Il est livré avec un excellent panneau d'administration, pas le cPanel habituel, mais un panneau personnalisé que vous pouvez utiliser pour tout configurer, de la gestion des utilisateurs SSH au choix de la version PHP que vous exécutez.

<h2 id="Configurer PHP">Configurer PHP.
<a href="#Configurer PHP" class="toc-anchor after"></a></h2>

Vous pouvez configurer chaque (sous-)domaine pour qu'il ait sa propre version de PHP. Au moment de la rédaction, la version PHP par défaut pour les nouveaux sites est 7.4. Vous pouvez choisir d'utiliser une version ultérieure (8.0 disponible), et nous vous recommandons de le faire car PHP 7.3.6+ est requis pour Grav.

![configuration de PHP](https://learn.getgrav.org/user/pages/09.webservers-hosting/01.shared/06.dreamhost/php-version.png)

<h2 id="Activation de SSH">Activation de SSH.
<a href="#Activation de SSH" class="toc-anchor after"></a></h2>

Ouvrez le panneau Utilisateurs. Chaque utilisateur Dreamhost peut avoir différents niveaux d'accès. Définissez votre utilisateur sur Shell User.

Au moment de la rédaction, la version par défaut de PHP CLI est 8.0.8, vous n'avez donc rien à faire pour que les outils Grav CLI fonctionnent correctement.

<h2 id="Installer et tester Grav">Installer et tester Grav.
<a href="#Installer et tester Grav" class="toc-anchor after"></a></h2>

Lorsque vous ajoutez un nouveau domaine, Dreamhost crée un dossier pour celui-ci sous le dossier de votre compte.

Accédez au serveur en utilisant SSH et allez dans ce dossier, puis téléchargez Grav dedans :

    $ | wget https://github.com/getgrav/grav/releases/download/1.7.28/grav-v1.7.28.zip

(Veuillez vérifier la dernière version disponible)

Décompressez avec `unzip grav-v1.7.28.zip`. Cela créera un dossier `grav`, nous devons donc déplacer les fichiers vers le dossier actuel. Tapez simplement :

    $ | mv grav/* grav/.htaccess ./; rmdir grav

Vous pouvez maintenant également supprimer le fichier zip :

    $ | rm grav-v1.7.28.zip

Grav a maintenant été installé avec succès. Essayez d'accéder au site à partir du navigateur, vous devriez voir un message de bienvenue Grav.

Vous pouvez maintenant installer des plugins et des thèmes, par exemple tapez ceci pour installer le plugin Grav Admin :

    $ | bin/gpm install admin
    
![installation Admin de Grav](https://learn.getgrav.org/user/pages/09.webservers-hosting/01.shared/06.dreamhost/install-plugin.png)    

<h2 id="Activer OPCache">Activer OPCache.
<a href="#Activer OPCache" class="toc-anchor after"></a></h2>

<h3 id="Sur les plans d'hébergement mutualisé Dreamhost">Sur les plans d'hébergement mutualisé Dreamhost.
<a href="#Sur les plans d'hébergement mutualisé Dreamhost" class="toc-anchor after"></a></h3>

OPCache est activé par défaut.

<h3 id="Sur les forfaits VPS Dreamhost">Sur les forfaits VPS Dreamhost.
<a href="#Sur les forfaits VPS Dreamhost" class="toc-anchor after"></a></h3>

OPCache est pris en charge mais n'est pas activé par défaut. Vous devez l'activer manuellement en créant un fichier phprc sous votre dossier utilisateur, sous `.php/7.4/phprc` (changez le numéro en fonction de votre version de PHP). Dans ce fichier, mettez le code suivant :

    zend_extension=opcache.so

Vous pouvez personnaliser davantage OPCache dans ce fichier en fonction de vos besoins.

