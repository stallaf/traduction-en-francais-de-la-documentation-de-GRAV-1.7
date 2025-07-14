<h1 class="rem">Hébergement Web Rochen</h1>

[Rochen Web Hosting](http://www.rochen.com/?utm_source=RocketTheme&utm_medium=Showcase&utm_campaign=Promotions) est le partenaire d'hébergement de longue date de **GetGrav.org** et de **RocketTheme.com**. Rochen propose désormais une nouvelle offre d'hébergement mutualisé premium qui utilise des **disques SSD**, les serveurs Web **Litespeed** avec les derniers **processeurs Intel XEON** garantissent que Grav fonctionne de manière optimale. Ils offrent également le choix de serveurs basés aux États-Unis ou au Royaume-Uni, afin que vous puissiez choisir la meilleure option pour vos utilisateurs.

![ROCHEN](https://learn.getgrav.org/user/pages/09.webservers-hosting/01.shared/01.rochen/rochen.png)

<h2 id="Choisir votre plan d'hébergement">Choisir votre plan d'hébergement.
<a href="#Choisir votre plan d'hébergement" class="toc-anchor after"></a></h2>

[Rochen Web Hosting](http://www.rochen.com/?utm_source=RocketTheme&utm_medium=Showcase&utm_campaign=Promotions) propose deux options en matière d'hébergement : l'hébergement **partagé** et l'hébergement en **rafale**. Rochen recommande l'option Burst pour les sites plus fréquentés et plus exigeants. Pour les besoins de ce guide, nous utiliserons l'option Partagé de base.

L'hébergement partagé varie de 7,95 $/mois à 13,95 $/mois selon la durée de l'engagement.

<h2 id="Activation de SSH">Activation de SSH.
<a href="#Activation de SSH" class="toc-anchor after"></a></h2>

Tout d'abord, vous devrez ouvrir l'option **Basculer l'accès SSH** dans la section **Sécurité** de cPanel. Sur cette page d'accès SSH, vous devez cliquer sur le bouton **Activer l'accès SSH**.

Ensuite, dans la **section Sécurité**, cliquez à nouveau sur l'option **Gérer les clés SSH**.

![activer SSH](https://learn.getgrav.org/user/pages/09.webservers-hosting/01.shared/01.rochen/manage-ssh-keys.png)

Il y a deux options à ce stade. **Générez une nouvelle clé** ou **importez une clé**. Il est plus simple de créer votre paire de clés publique/privée localement sur votre ordinateur, puis d'importer simplement la clé publique DSA.

<div class = "notice info">
Les utilisateurs de Windows devront d'abord installer <a href = "https://www.cygwin.com/">Cygwin</a> pour fournir de nombreux outils GNU et open source utiles disponibles sur les plates-formes Mac et Linux. Lorsque vous êtes invité à choisir des packages, assurez-vous de cocher l'option SSH. Après l'installation, lancez <code>Cygwin Terminal</code>.
</div>

Lancez une fenêtre de terminal et tapez :

```console
$ | ssh-keygen -t dsa
```

Ce script de génération de clé vous invitera à remplir certaines valeurs, ou vous pouvez simplement appuyer sur `[return]` pour accepter les valeurs par défaut. Cela créera un `id_dsa` (clé privée) et un `id_dsa.pub` (clé publique) dans un dossier appelé `.ssh/` dans votre répertoire personnel. Il est important de vous assurer que vous ne donnez **JAMAIS** votre clé privée, ni ne la téléchargez nulle part, **uniquement votre clé publique**.

Une fois généré, vous pouvez coller le contenu de votre clé publique `id_dsa.pub` dans le champ `Public Key` de la section **Import SSH key** de la page **SSH Access** :

![import clé SSH](https://learn.getgrav.org/user/pages/09.webservers-hosting/01.shared/01.rochen/ssh-public-key.png)

Après le téléchargement, vous devriez voir la clé répertoriée dans la section **Clés publiques** de la page Gérer les clés SSH. Vous devez ensuite cliquer sur **Gérer** pour vous assurer que la clé est autorisée :

![gérer clé SSH](https://learn.getgrav.org/user/pages/09.webservers-hosting/01.shared/01.rochen/authorized-keys.png)

Pour **activer l'accès SSH** pour votre compte, accédez simplement à la section **Services gérés** sur le portail **my.rochen.com** et cliquez sur les informations de votre compte **d'hébergement mutualisé**. À côté de l'option **SSH**, cliquez sur le lien **Désactivé** et confirmez que vous souhaitez activer SSH.

Cela signifie que vous êtes prêt à tester ssh sur votre serveur.

```console
$ | ssh rochen_username@rochen_servername
```

Évidemment, vous devrez entrer votre nom d'utilisateur fourni par Rochen pour `rochen_username` et le nom de serveur fourni par rochen pour `rochen_servername`.

<h2 id="Configuration de PHP et de la mise en cache">Configuration de PHP et de la mise en cache.
<a href="#Configuration de PHP et de la mise en cache" class="toc-anchor after"></a></h2>

Rochen utilise PHP **5.4** par défaut, mais vous avez la possibilité d'utiliser une version **5.5** ou **5.6** plus récente qui est requise pour Grav.

La première chose à faire est de changer la version par défaut de PHP avec laquelle votre site fonctionne. Cliquez donc sur le lien **Sélectionner la version PHP** dans la section **Logiciels et services**.

Vous verrez une page indiquant la version actuelle de PHP. Vous trouverez ci-dessous une liste déroulante qui vous permet de choisir des versions alternatives. Choisissez **5.6** et cliquez sur le bouton `Set as current`.

![version PHP](https://learn.getgrav.org/user/pages/09.webservers-hosting/01.shared/01.rochen/php-settings.png)

Rochen est une race rare dans le monde des hébergeurs, en ce sens qu'ils fournissent des extensions de mise en cache sophistiquées pour PHP. Pour en profiter, activez l'extension de mise en cache `apcu`, ainsi que l'extension Zend `opcache`. Ensuite, cliquez sur `Save` en bas de ces options.

Une optimisation que vous devriez faire est de **désactiver** l'extension `xdebug` qui est activée par défaut, mais pas nécessaire dans un environnement de production, en fait cela ne fait que ralentir les choses.

Pour tester que vous avez la **bonne version de PHP**, **Zend OPcache** et **APCu** en cours d'exécution, vous pouvez créer un fichier temporaire : `public_html/info.php` et le mettre dans le contenu :

    <?php phpinfo();

Enregistrez le fichier et pointez votre navigateur vers ce fichier info.php sur votre site, et vous devriez être accueilli avec des informations PHP reflétant la version que vous avez sélectionnée précédemment :

![PHP info](https://learn.getgrav.org/user/pages/09.webservers-hosting/01.shared/01.rochen/php-info1.png)

Vous devriez également pouvoir faire défiler vers le bas et voir **Zend OPcache** répertorié dans le **bloc moteur zend**, et une section **APCu** en dessous : 

![Zend Engine](https://learn.getgrav.org/user/pages/09.webservers-hosting/01.shared/01.rochen/php-info2.png)

<h2 id="Installer et tester Grav">Installer et tester Grav.
<a href="#Installer et tester Grav" class="toc-anchor after"></a></h2>

En utilisant vos nouvelles capacités SSH trouvées, connectons-nous en SSH à votre serveur Rochen (si vous n'y êtes pas déjà) et téléchargez la dernière version de Grav, décompressez-la et testez-la !

Nous allons extraire Grav dans un sous-dossier `/grav`, mais vous pouvez décompresser directement à la racine de votre dossier `~/www/` pour vous assurer que Grav est accessible directement.

```console
$ | cd ~/www
$ | wget https://getgrav.org/download/core/grav/latest
$ |unzip grav-v1.7.28.zip
```

Vous devriez maintenant pouvoir pointer votre navigateur vers `http://myrochenserver.com/grav` en utilisant l'URL appropriée bien sûr.

Parce que vous avez suivi ces instructions avec diligence, vous pourrez également utiliser les commandes [Grav CLI](/cli-commandes-grav) et [Grav GPM](/cli-commandes-gpm) telles que :

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

