<h1 class="rem">Développement local avec ddev</h1>

[ddev](https://ddev.readthedocs.io/) est un outil de développement PHP open source, basé sur Docker. Il peut facilement créer des environnements d'hébergement locaux et ses configurations de serveur peuvent être contrôlées par version. Initialement destiné au développement Drupal, ddev peut facilement héberger des sites Drupal, WordPress et GravCMS. Puisqu'il est basé sur Docker, ddev est compatible avec Windows, Mac et Linux.

<h2 id="Installation de ddev">Installation de ddev.
<a href="#Installation de ddev" class="toc-anchor after"></a></h2>

Veuillez consulter [la documentation officielle de ddev](https://ddev.readthedocs.io/en/latest/) pour les instructions les plus récentes sur l'installation de ddev.

<h2 id="Configuration">Configuration.
<a href="#Configuration" class="toc-anchor after"></a></h2>

* Placez les fichiers Grav dans un dossier sur la machine hôte (/home/USER/projects/grav).
* Dans votre terminal, cd dans ce dossier `cd /home/USER/projects/grav`
* Tapez `ddev config`. Les invites suivantes s'afficheront :
    * Nom du projet (par défaut, le nom du dossier de [GRAV_ROOT]
    * Chemin Docroot (par défaut, [GRAV_ROOT])
    * Type de projet (utilisez le type `php` pour cette option)
* lancez `ddev start` depuis le dossier [GRAV_ROOT].
* Laissez ddev construire les conteneurs dont il a besoin. Les informations d'identification racine/Sudo peuvent être requises pour apporter des modifications aux hôtes locaux.

<h2 id="Remarque sur ddev et le plugin Feed">Remarque sur ddev et le plugin Feed.
<a href="#Remarque sur ddev et le plugin Feed" class="toc-anchor after"></a></h2>

ddev utilise par défaut nginx et la configuration par défaut au 2020-09-18 est suffisante pour la plupart des cas d'utilisation. Toutefois, si vous prévoyez d'utiliser le [plug-in ffed](https://github.com/getgrav/grav-plugin-feed), vous devrez apporter les modifications de configuration suivantes :

* Modifier [GRAV_ROOT]/.ddev/nginx_full/nginx-site.conf
* Supprimez la ligne 3 pour rendre les modifications permanentes (`#ddev-generated`)
* Supprimez les lignes 58-62 qui forçaient la mise en cache statique rss et atom (`# Expire rules for static content ...`)
* Exécutez `ddev restart` pour charger la nouvelle configuration nginx.

Si ces modifications ne sont pas apportées, l'erreur HTTP 404 sera générée lors des tentatives de chargement de flux RSS ou Atom.

<h2 id="Utiliser ddev">Utiliser ddev.
<a href="#Utiliser ddev" class="toc-anchor after"></a></h2>

Exécutez ces commandes à partir de [GRAV_ROOT] sur la machine hôte :

* `ddev describe` - Affiche tous les services disponibles.
* `ddev ssh` - Connecte un shell au serveur Web au docroot.
* `ddev exec 'params'` - Exécute les paramètres au niveau du docroot (par exemple, `ddev exec 'bin/grav clear'` pour vider le cache). 

<div class="notice defaut">
Je dois installer [insert plugin/ theme here]. Comment accéder à `bin/gpm` ?
</div>

À partir de [GRAV_ROOT], tapez `ddev ssh` et vous serez connecté au serveur Web au docroot. À partir de là, vous pouvez exécuter n'importe quelle commande php (composer, bin/gpm, bin/grav, etc.).

<div class="notice defaut">
Où puis-je modifier mes fichiers ?
</div>

Un éditeur sur la machine hôte peut modifier les fichiers sur [GRAV_ROOT]. Les modifications seront automatiquement répercutées dans le conteneur ddev. Les modifications effectuées dans le conteneur (c'est-à-dire `bin/gpm install admin`) seront répercutées sur la machine hôte.

