<h1 class="rem">SiteGround</h1>

Le slogan de [SiteGround](http://www.siteground.com/) est **Web Hosting Crafted With Care**, et c'est pour cette raison qu'il s'est avéré être une solution d'hébergement populaire pour les membres des communautés Joomla et WordPress. C'est également une bonne option pour héberger un site Web basé sur Grav.

![SiteGround](https://learn.getgrav.org/user/pages/09.webservers-hosting/01.shared/04.siteground/siteground.png)

Dans ce guide, nous couvrirons les éléments essentiels pour configurer un compte d'hébergement partagé SiteGround assez standard pour fonctionner de manière optimale avec Grav.

<h2 id="Choisir votre plan d'hébergement">Choisir votre plan d'hébergement.
<a href="#Choisir votre plan d'hébergement" class="toc-anchor after"></a></h2>

Au moment de la rédaction de cet article, SiteGround propose [trois options d'hébergement partagé](http://www.siteground.com/web-hosting.htm) allant de 3,95 $/mois bas de gamme à 14,95 $/mois pour ce qu'ils appellent le plan **GoGeek**. Nous vous suggérons fortement d'opter pour le plan GoGeek haut de gamme mais toujours très bon marché. Cela fournit un meilleur matériel de serveur et moins d'utilisateurs sur le serveur.

<h2 id="Configuration">Configuration.
<a href="#Configuration" class="toc-anchor after"></a></h2>

SiteGround fournit un panneau de contrôle **cPanel** très complet. Celui-ci est directement accessible via l'onglet **Mes comptes**.

<h2 id="Activation de SSH">Activation de SSH.
<a href="#Activation de SSH" class="toc-anchor after"></a></h2>

Tout d'abord, vous devrez ouvrir l'option **Accès SSH/Shell** dans la section **AVANCÉ** de cPanel.

SiteGround fournit un didacticiel très complet pour l'[utilisation de SSH](http://www.siteground.com/tutorials/ssh/), mais il est plus simple de créer votre paire de clés publique/privée localement sur votre ordinateur, puis de simplement télécharger la clé publique DSA.

<div class = "notice info">
Les utilisateurs de Windows devront d'abord installer <a href = "https://www.cygwin.com/">Cygwin</a> pour fournir de nombreux outils GNU et open source utiles disponibles sur les plates-formes Mac et Linux. Lorsque vous êtes invité à choisir des packages, assurez-vous de cocher l'option SSH. Après l'installation, lancez le <code>Cygwin Terminal</code>.
</div>

Lancez une fenêtre de terminal et tapez :

    $ | $ ssh-keygen -t dsa

Ce script de génération de clé vous invitera à remplir certaines valeurs, ou vous pouvez simplement appuyer sur `[return]` pour accepter les valeurs par défaut. Cela créera un `id_dsa` (clé privée) et un `id_dsa.pub` (clé publique) dans un dossier appelé `.ssh/` dans votre répertoire personnel. Il est important de vous assurer que vous ne donnez **JAMAIS** votre clé privée, ni ne la téléchargez nulle part, **uniquement votre clé publique**.

Une fois généré, vous pouvez coller le contenu de votre clé publique `id_dsa.pub` dans le champ `Public Key` de la section **Upload SSH key** de la page **SSH/Shell Access** :

![clef SSH](https://learn.getgrav.org/user/pages/09.webservers-hosting/01.shared/04.siteground/ssh-public-key.png)

Après le téléchargement, vous devriez voir la clé répertoriée au bas de cette page. Cela signifie que vous êtes prêt à tester SSH sur votre serveur.

    $ | $ ssh siteground_username@siteground_servername -p18765

Évidemment, vous devrez entrer votre nom d'utilisateur fourni par SiteGround pour `siteground_username` et le nom de serveur fourni par SiteGround pour `siteground_servername`. Le `-p18765` est important car il s'agit du port non standard sur lequel SiteGround exécute SSH.

<h2 id="Activer PHP OPcache">Activer PHP OPcache.
<a href="#Activer PHP OPcache" class="toc-anchor after"></a></h2>

<div class = "notice tip">
Mise à jour [2016-03] : le support de Siteground a indiqué qu'OPCache est disponible à partir de PHP7 et non de 5.5. OPCache était alors activé par défaut, et donc aucune configuration supplémentaire n'était requise à cette étape de la configuration, de sorte que certaines des instructions ci-dessous peuvent ne plus être nécessaires.
</div>

Par défaut, l'hébergement SiteGround **prend en charge Zend OPcache**, mais il n'est **pas activé**. Vous devez l'activer manuellement en créant un fichier `php.ini` dans votre dossier `public_html/` avec le contenu :

    zend_extension=opcache.so

Pour tester que vous avez la bonne version de PHP et que Zend OPcache fonctionne, vous pouvez créer un fichier temporaire : `public_html/info.php` et le mettre dans le contenu :

    <?php phpinfo();

Enregistrez le fichier et pointez votre navigateur vers ce fichier info.php sur votre site et vous devriez être accueilli avec une information PHP reflétant la version que vous avez sélectionnée précédemment :

![version PHP](https://learn.getgrav.org/user/pages/09.webservers-hosting/01.shared/04.siteground/phpinfo-1.png)

Vous devriez également pouvoir faire défiler vers le bas et voir une section appelée **Zend OPcache** :

![ZendEngine](https://learn.getgrav.org/user/pages/09.webservers-hosting/01.shared/04.siteground/phpinfo-2.png)

<h2 id="Installer et tester Grav">Installer et tester Grav.
<a href="#Installer et tester Grav" class="toc-anchor after"></a></h2>

En utilisant vos nouvelles capacités SSH trouvées, connectons-nous en SSH à votre serveur SiteGround (si vous n'y êtes pas déjà) et téléchargez la dernière version de Grav, décompressez-la et testez-la !

Nous allons extraire Grav dans un sous-dossier /grav, mais vous pouvez décompresser directement à la racine de votre dossier `~/public_html/` pour vous assurer que Grav est accessible directement.

```console
$ | cd ~/public_html
$ | [~/public_html]$ curl -L -O https://github.com/getgrav/grav/releases/download/1.7.28/grav-v1.7.28.zip
$ | [~/public_html]$ unzip grav-v1.7.28.zip
```

Vous devriez maintenant pouvoir pointer votre navigateur vers `http://mysiteground.com/grav` en utilisant bien sûr l'URL appropriée.

<div class = "notice tip">
Mise à jour [2016-03] : Le chemin d'accès à la CLI pour PHP 7 sur l'hébergement mutualisé Siteground semble actuellement être : <code>/usr/local/php70/bin/php-cli</code>, et donc pour l'utilisation en ligne de commande de gpm/grav vous pourrait créer un alias, puis référencer le php-cli directement via le terminal, par ex. <code>alias php-cli="/usr/local/php70/bin/php-cli</code>". Ensuite, vous pouvez l'utiliser comme : <code>$php-cli bin/grav list</code>.
</div>

Parce que vous avez suivi ces instructions avec diligence, vous pourrez également utiliser les commandes [Grav CLI](/cli-commandes-grav) et [Grav GPM](/cli-commandes-gpm) telles que :

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

Pour utiliser le gestionnaire de paquets Grav (gpm), vous devrez le définir comme exécutable en exécutant cette commande dans votre dossier Grav.

    $ chmod +x bin/gpm
 
