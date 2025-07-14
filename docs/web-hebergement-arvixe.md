<h1 class="rem">Arvixe</h1>

[Arvixe](https://www.arvixe.com/) est une société d'hébergement primée qui est fière de fournir un hébergement Web de qualité, **abordable** et d'une **fiabilité** inégalée. Avec d'excellentes fonctionnalités et une position **conviviale pour les développeurs**, l'hébergement partagé Arvixe est une excellente option pour un site basé sur Grav.

![Arvixe](https://learn.getgrav.org/user/pages/09.webservers-hosting/01.shared/05.arvixe/arvixe.png)

Dans ce guide : Nous couvrirons l'essentiel pour configurer un compte d'hébergement mutualisé Arvixe assez standard pour fonctionner de manière optimale avec Grav.

<h2 id="Choisir votre plan d'hébergement">Choisir votre plan d'hébergement.
<a href="#Choisir votre plan d'hébergement" class="toc-anchor after"></a></h2>

Au moment de la rédaction, Arvixe propose [deux options d'hébergement partagé basées sur Linux](https://www.arvixe.com/linux_web_hosting) qui coûtent 4,00 $ / mois pour la **PersonalClass** et 7,00 $ pour le plan **PersonalClass Pro**. Les deux plans sont presque identiques, mais le plan Pro offre des domaines simultanés illimités. L'un ou l'autre fonctionnera bien avec Grav.

<h2 id="Configurer PHP">Configurer PHP.
<a href="#Configurer PHP" class="toc-anchor after"></a></h2>

Arvixe fournit un panneau de contrôle **cPanel** très complet. L'URL correspondante est fournie avec votre e-mail de bienvenue.

Parce que la version PHP par défaut d'Arvixe est **5.3**, la première chose à faire est de changer la version par défaut de PHP avec laquelle votre site fonctionne.

Sur la page d'accueil principale de cPanel, il y a une section intitulée **Logiciels/Services**. Vous trouverez ici le **ntPHSSelector**. Lorsque vous cliquez dessus, vous serez confronté à une arborescence de dossiers dans laquelle vous pouvez définir un dossier spécifique ou définir la version à l'échelle du site en cliquant sur `public_html`. Lorsque vous choisissez un dossier, vous pouvez sélectionner la version de PHP, sélectionnez simplement **5.5** et cliquez sur Soumettre.

![PHP chez Arvixe](https://learn.getgrav.org/user/pages/09.webservers-hosting/01.shared/05.arvixe/php-selector.png)

Cliquez sur **Enregistrer** pour que cela prenne effet.

Vous devriez pouvoir accéder à votre site via l'URL de votre site Web avec `phpinfo.php` ajouté à la fin. Par exemple : `http://myarvixe.com/phpinfo.php`.

![Version PHP Arvixe](https://learn.getgrav.org/user/pages/09.webservers-hosting/01.shared/05.arvixe/phpinfo.png)

Vous pouvez vous assurer que vous utilisez la bonne version de PHP.

<div class = "notice info">
PHP 5.5 d'Arvixe inclut déjà Zend OPcache activé, il n'y a donc aucune étape supplémentaire requise pour obtenir cette configuration.
</div>

<h2 id="Activation de SSH">Activation de SSH.
<a href="#Activation de SSH" class="toc-anchor after"></a></h2>

Tout d'abord, vous devrez ouvrir l'option **Accès SSH/Shell** dans la section **Sécurité** de cPanel. Sur cette page d'accès SSH, vous devez cliquer sur le bouton **Gérer les clés SSH**.

Il y a deux options à ce stade. **Générez une nouvelle clé** ou **importez une clé**. Il est plus simple de créer votre paire de clés publique/privée localement sur votre ordinateur, puis d'importer simplement la clé publique DSA.

<div class = "notice info">
Les utilisateurs de Windows devront d'abord installer <a href = "https://www.cygwin.com/">Cygwin</a> pour fournir de nombreux outils GNU et open source utiles disponibles sur les plates-formes Mac et Linux. Lorsque vous êtes invité à choisir des packages, assurez-vous de cocher l'option SSH. Après l'installation, lancez <code>Cygwin Terminal</code>
</div>

Lancez une fenêtre de terminal et tapez :

    $ | ssh-keygen -t dsa

Ce script de génération de clé vous invitera à remplir certaines valeurs, ou vous pouvez simplement appuyer sur `[return]` pour accepter les valeurs par défaut. Cela créera un `id_dsa` (clé privée) et un `id_dsa.pub` (clé publique) dans un dossier appelé `.ssh/` dans votre répertoire personnel. Il est important de vous assurer que vous ne donnez **JAMAIS** votre clé privée, ni ne la téléchargez nulle part, **uniquement votre clé publique**.

Une fois généré, vous pouvez coller le contenu de votre clé publique `id_dsa.pub` dans le champ `Public Key` de la section **Import SSH key** de la page **SSH Access** :

![Import clef SSH](https://learn.getgrav.org/user/pages/09.webservers-hosting/01.shared/05.arvixe/ssh-public-key.png)

Après le téléchargement, vous devriez voir la clé répertoriée dans la section **Clés publiques** de la page Gérer les clés SSH. Vous devez ensuite cliquer sur **Gérer** pour vous assurer que la clé est autorisée :

![Gérer clef SSH](https://learn.getgrav.org/user/pages/09.webservers-hosting/01.shared/05.arvixe/authorized-keys.png)

Cela signifie que vous êtes prêt à tester ssh sur votre serveur.

    $ | ssh arvixe_username@arvixe_servername

Évidemment, vous devrez entrer votre nom d'utilisateur fourni par Arvixe pour `arvixe_username` et le nom de serveur fourni par Arvixe pour `arvixe_servername`.

<h2 id="Configurer l'interface de ligne de commande PHP">Configurer l'interface de ligne de commande PHP.
<a href="#Configurer l'interface de ligne de commande PHP" class="toc-anchor after"></a></h2>

Au moment d'écrire ces lignes, la version PHP par défaut d'Arvixe est **5.3**. Étant donné que Grav nécessite PHP **5.5+**, nous devons nous assurer que Grav utilise une version plus récente de PHP sur la ligne de commande (CLI). Pour ce faire, vous devez utiliser SSH pour accéder à votre serveur et modifier votre fichier `.bash_profile` et modifier le chemin afin qu'il référence le chemin PHP approprié avant le chemin normal :

```bash
# .bash_profile

# Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi

# User specific environment and startup programs

PATH=/opt/ntphp/php55/bin:$PATH:$HOME/bin

export PATH
```

Vous aurez besoin de sourcer le profil : `$ source ~/.bash_profile` ou de vous reconnecter à votre terminal pour que votre changement de chemin prenne effet, mais après cela, vous devriez pouvoir taper `php -v` et voir :

```console
$ | php -v
$ | PHP 5.5.18 (cli) (built: Nov 19 2014 14:29:20)
$ | Copyright (c) 1997-2014 The PHP Group
$ | Zend Engine v2.5.0, Copyright (c) 1998-2014 Zend Technologies
$ |     with Zend OPcache v7.0.4-dev, Copyright (c) 1999-2014, by Zend Technologies
```

<h2 id="Installer et tester Grav">Installer et tester Grav.
<a href="#Installer et tester Grav" class="toc-anchor after"></a></h2>

En utilisant vos nouvelles capacités SSH trouvées, connectons-nous en SSH à votre serveur Arvixe (si vous n'y êtes pas déjà) et téléchargez la dernière version de Grav, décompressez-la et testez-la !

Nous allons extraire Grav dans un sous-dossier `/grav`, mais vous pouvez décompresser directement à la racine de votre dossier `~/public_html/` pour vous assurer que Grav est accessible directement.

```console
$ | $ cd ~/public_html
$ | [~/public_html]$ curl -L -O https://github.com/getgrav/grav/releases/download/1.7.28/grav-v1.7.28.zip
$ | [~/public_html]$ unzip grav-v1.7.28.zip
```

Vous devriez maintenant pouvoir pointer votre navigateur vers `http://myarvixe.com/grav` en utilisant bien sûr l'URL appropriée.

Parce que vous avez suivi ces instructions avec diligence, vous pourrez également utiliser les commandes [Grav CLI](/cli-commandes-grav) et [Grav GPM](/cli-commandes-gpm) telles que :

```console
$ | $ cd ~/public_html/grav
$ | $ bin/grav clear-cache
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

