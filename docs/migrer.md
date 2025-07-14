<h1 class="rem">Migrer de Drupal 7 vers Grav</h1>

<h2 id="Conditions">Conditions.
<a href="#Conditions" class="toc-anchor after"></a></h2>

* PHP v7.1 ou supérieur pour les dépendances composer.
* Drush
* Site de travail Drupal 7 à partir duquel le contenu sera exporté.
 * Accès R/W à `public://` ainsi qu'au dossier des modules utilisateurs sur le site Drupal (typiquement `sites/all/modules/contrib)`.

<h2 id="Installation">Installation.
<a href="#Installation" class="toc-anchor after"></a></h2>

<iframe  width="560" 
         height="315" 
         src="https://www.youtube.com/embed/I6UVFUqZMOU" 
         title="Drupal to Grav Exporter (silent)" 
         frameborder="0" 
         allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" 
         allowfullscreen>
</iframe>

1. Téléchargez et déplacez le plugin [grav_export ](https://www.drupal.org/project/grav_export/) dans le dossier `sites/all/modules/contrib` de votre Drupal.
2. Exécutez `composer install` dans le dossier `grav_export` pour installer les dépendances.
3. Activez le module grav_export via  `drush en grav_export -y` ou l'interface graphique d'administration.
4. Exécutez `drush grav_export_all`, ou son alias `drush gravea`, pour exporter tous les éléments. Voir les autres options ci-dessous.
5. Les fichiers exportés se trouvent dans `[DRUPAL_ROOT]/sites/default/files/grav_export/EXPORT`
6. Le plugin Grav [https://github.com/david-szabo97/grav-plugin-admin-addon-user-manager](https://github.com/david-szabo97/grav-plugin-admin-addon-user-manager) est recommandé pour afficher et gérer les utilisateurs.
7. Suivez les étapes ci-dessous pour importer les données dans Grav.

<h2 id="Exporter des utilisateurs depuis Drupal">Exporter des utilisateurs depuis Drupal.
<a href="#Exporter des utilisateurs depuis Drupal" class="toc-anchor after"></a></h2>

<h3 id="Commande">Commande :
<a href="#Commande" class="toc-anchor after"></a></h3>

 `drush grav_export_users` ou `drush graveu` générera des fichiers de compte utilisateur Grav.
 
<h3 id="Résultats">Résultats :
<a href="#Résultats" class="toc-anchor after"></a></h3>

* Comptes d'utilisateurs dans le dossier d'exportation sous `EXPORT/accounts/`.
      * Les noms d'utilisateur seront remplis d'un minimum de 3 caractères, maximum de 16.
      * Si un nom d'utilisateur est tronqué ou rempli, le nom d'utilisateur aura également l'uid Drupal pour éviter les collisions.
       * Les mots de passe de chaque compte sont générés de manière aléatoire et n'ont aucun lien avec le compte Drupal respectif. Le mot de passe passe automatiquement à un hashed_password une fois que le compte s'authentifie pour la première fois.

<h2 id="Importer des utilisateurs dans Grav">Importer des utilisateurs dans Grav.
<a href="#Importer des utilisateurs dans Grav" class="toc-anchor after"></a></h2>

Copiez le dossier `EXPORT/accounts` dans votre répertoire `user` (par exemple, les fichiers username.yaml doivent être placés dans `user/accounts`).

<h2 id="Exportation des rôles d'utilisateur depuis Drupal">Exportation des rôles d'utilisateur depuis Drupal.
<a href="#Exportation des rôles d'utilisateur depuis Drupal" class="toc-anchor after"></a></h2>

<h3 id="Commande-1">Commande :
<a href="#Commande-1" class="toc-anchor after"></a></h3>

 `drush grav_export_roles` ou `drush graver` générera un fichier Grav groups.yaml.

<h3 id="Résultats-1">Résultats :
<a href="#Résultats-1" class="toc-anchor after"></a></h3>

Les rôles d'utilisateur Drupal sont exportés en tant que groupes Grav dans un fichier `groups.yaml` dans `config/groups.yaml`. Quelques notes sur l'exportation de rôle :

*Chaque rôle Drupal est converti en groupe Grav `drupal_<ROLE_WITH_UNDERSCORES>` (par exemple, `authentificated user` devient `drupal_authenticated_user`).
* Le groupe `drupal_administrator` reçoit un accès `admin.super` ainsi qu'un accès `admin.login`.
* Le groupe `drupal_authenticated_user` reçoit l'accès `admin.login`.
* Tous les comptes reçoivent le groupe "drupal_authenticated_user".
* Les utilisateurs Drupal avec des rôles d'administrateur reçoivent le groupe `drupal_administrator`.

<h3 id="Importation des rôles d'utilisateur">Importation des rôles d'utilisateur
<a href="#Importation des rôles d'utilisateur" class="toc-anchor after"></a></h3>

Copiez le dossier `EXPORT/confi`g dans `users/config`.

<h2 id="Exportation de types de contenu depuis Drupal">Exportation de types de contenu depuis Drupal.
<a href="#Exportation de types de contenu depuis Drupal" class="toc-anchor after"></a></h2>

<h3 id="Commande-2">Commande :
<a href="#Commande-2" class="toc-anchor after"></a></h3>

 `drush grav_export_content_types` ou `drush gravect` générera des plans Grav et des fichiers html.twig.

<h3 id="Résultats-2">Résultats :
<a href="#Résultats-2" class="toc-anchor after"></a></h3> 

Pour chaque type de champ défini, `drush gravect` tentera de créer un plan compatible et un fichier html.twig correspondant pour chaque type de contenu Drupal. Les fichiers seront exportés vers `EXPORT/themes/drupal_export/blueprints` et `EXPORT/themes/drupal_export/templates` respectivement.

<h3 id="Limites connues">Limites connues.
<a href="#Limites connues" class="toc-anchor after"></a></h3>

1. Champ "nombre_entier"

La cardinalité dans de nombreux champs Grav ne prend en charge qu'une seule valeur. Seule la première entrée Drupal est exportée.

2. Champ "champ d'adresse"

Grav n'a aucun concept de champ d'adresse. Les données de champ Drupal sont exportées sous forme de type de formulaire `array`.

<h3 id="Importation de types de contenu dans Grav">Importation de types de contenu dans Grav.
<a href="#Importation de types de contenu dans Grav" class="toc-anchor after"></a></h3>

Copiez les dossiers `EXPORT/themes/drupal_export/blueprints` et `XPORT/themes/drupal_export/templates` dans le thème actif dans Grav. Le plugin d'administration devrait maintenant fournir des options supplémentaires pour chaque type de contenu et les champs associés.

Remarque : Lorsque le contenu des champs est ajouté aux en-têtes de page Grav, l'affichage de ces champs n'est **pas** exporté depuis Drupal. Le fichier html.twig devra être modifié afin d'afficher tous les champs supplémentaires (en plus du contenu du corps principal).

<h2 id="Exporter des nœuds depuis Drupal">Exporter des nœuds depuis Drupal.
<a href="#Exporter des nœuds depuis Drupal" class="toc-anchor after"></a></h2>

<h3 id="Commande-3">Commande :
<a href="#Commande-3" class="toc-anchor after"></a></h3>

 `drush grav_export_nodes` ou `drush graven` générera des utilisateurs et des groupes Grav.
Résultats

<h3 id="Résultats-3">Résultats :
<a href="#Résultats-3" class="toc-anchor after"></a></h3>

* Chaque nœud est exporté vers sa propre structure de dossiers sous `EXPORT/pages`, en fonction de l'alias d'URL et du type de contenu de Drupal (c'est-à-dire `pages/content/my_first_page/page.yaml` ou `pages/content/cool_article/article.yaml`).
* Les données de champ Drupal sont stockées dans l'en-tête de la page.
* Les fichiers sont stockés dans `EXPORT/data/files/`, suivant un modèle de stockage de type Drupal.
* Dans la sortie drush, des termes de taxonomie supplémentaires peuvent être produits. Copiez-les dans le fichier `user/config/site.yaml` de Grav sous la clé de taxonomie.

<h2 id="Importation de nœuds dans des pages Grav">Importation de nœuds dans des pages Grav.
<a href="#Importation de nœuds dans des pages Grav" class="toc-anchor after"></a></h2>

Copiez les dossiers `EXPORT/data` et `EXPORT/pages` dans le répertoire `user dans Grav.

