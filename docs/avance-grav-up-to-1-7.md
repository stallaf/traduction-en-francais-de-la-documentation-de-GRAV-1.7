<h1 class="rem">Mise à niveau vers Grav 1.7</h1>

Grav 1.7 introduit quelques nouvelles fonctionnalités, améliorations, corrections de bogues et fournit de nombreux changements architecturaux qui ouvrent la voie vers Grav 2.0. Voici quelques faits saillants :

* **Objets flexibles** : une nouvelle façon de créer vos propres types de données.
* **Symfony Server** : Exécutez Grav sans avoir à installer de serveur Web.
* **Multilingue amélioré** : meilleures solutions de remplacement de langue, meilleure prise en charge de l'administration.
* **Multi-site amélioré** : l'administrateur a amélioré la prise en charge multi-site.
* **Amélioration de l'ACL d'administration** : prise en charge complète de CRUD pour les utilisateurs et les pages.
* **Prise en charge multimédia améliorée** : prend en charge le format d'image `webp`, le chargement paresseux et plus encore.
* **Mise en cache améliorée** : nouvelle balise `{% cache %}` et performances améliorées, en particulier dans l'administration.
* **Détection XSS dans les formulaires** : les formulaires ne seront pas soumis si un potentiel XSS y est détecté. Consultez la [documentation](/formulaires-ref-options-formulaires/#XSS Checks) pour savoir comment désactiver les vérifications.
* **Meilleurs outils de débogage** : intégration <a href = "https://underground.works/clockwork/">Clockwork</a>, profilage Twig et prise en charge de l'extension PHP <a href = "https://github.com/tideways/php-xhprof-extension">Tideways XHProf</a> pour le profilage des performances.

<div class = "notice warning">
I<strong>MPORTANT</strong> : Pour la plupart des gens, Grav 1.7 devrait être une simple mise à niveau sans aucun problème, mais comme toute mise à niveau, il est recommandé <strong>de faire une sauvegarde</strong> de votre site et de tester <strong>la mise à niveau dans un environnement</strong> de test avant de mettre à niveau votre site en ligne.
</div>

<h3 id="Problèmes les plus courants">Problèmes les plus courants.
<a href="#Problèmes les plus courants" class="toc-anchor after"></a></h3>

1. ######Le HTML est affiché sous forme de code sur votre site plutôt que d'être rendu sous forme de HTML comme prévu.

    Ce comportement est le résultat de la nouvelle valeur par défaut de l'**auto-escaping** étant vrai dans Grav 1.7. Il s'agit d'une amélioration de la sécurité, et si vous effectuez une mise à niveau à partir d'une version antérieure à 1.7, nous activons automatiquement le paramètre de **compatibilité Twig** dans la configuration du système pour garantir que votre ancien code Twig continuera à fonctionner. Si vous effectuez une mise à jour manuelle vers la version 1.7 ou une mise à niveau d'une manière qui ne passe pas par le processus de mise à niveau automatique de GPM, vous devez définir ce paramètre vous-même.

    Consultez la [section Twig](#Twig) de ce guide pour plus de détails...
    
2. ######Obtention d'erreurs à propos de YAML non valide.

    Comme nous avons mis à niveau vers une version plus récente du framework Symfony, l'analyseur YAML est plus strict qu'il ne l'était dans les versions antérieures à 1.7. Pour gérer cela, nous avons inclus une ancienne version de l'analyseur qui est disponible lors de l'activation de la compatibilité Yaml. Ceci est automatiquement géré pour vous si vous mettez à niveau vers Grav 1.7 via GPM, mais si vous avez mis à niveau manuellement, vous devrez définir cette valeur vous-même.

    Consultez la [section YAML](#YAML) de ce guide pour plus de détails...
    
3. ######L'administrateur apparaît avec des chaînes non traduites

    Si votre administrateur affiche des chaînes non traduites dans l'interface, c'est probablement parce que vous avez précédemment désactivé les **traductions linguistiques**. C'était bogué dans les versions précédentes de Grav et sa désactivation ne désactivait pas réellement les traductions dans l'administration comme prévu. Ceci est **corrigé** dans Grav 1.7 et ce paramètre fait ce qu'il est censé faire, afficher les codes de traduction en majuscules plutôt que les chaînes traduites elles-mêmes.

    Consultez la section [Dépannage](#Dépannages des problèmes) pour le correctif.
    
4. ######Erreurs lors de l'enregistrement ou des plugins d'administration non fonctionnels

    Dans Grav 1.7, nous avons introduit **Flex Pages** comme nouvelle interface utilisateur de gestion de page par défaut. De plus, pour optimiser les performances, nous avons arrêté d'initialiser les pages à chaque appel de l'administrateur. Revenir aux **pages Grav** normales peut temporairement résoudre votre problème. Cela se fait en éditant le plugin **FlexObjects** et en désactivant **Pages (Admin)**.

    Pour résoudre correctement le problème, les plugins personnalisés doivent être mis à jour pour prendre en charge à la fois les **pages Grav** et les **pages Flex** en utilisant `PageInterface` et doivent également explicitement Pages si nécessaire.

    Consultez la [section Pages](#Pages) et la [section Admin](#Administration)</span> de ce guide pour plus de détails...

    Il y a également eu des problèmes de plugins spécifiques qui ont déjà été découverts. Consultez la [section Dépannage](#Dépanages) de cette page pour des problèmes spécifiques avec les plugins.
    
5. ######Les plans de page cessent de fonctionner ou génèrent une erreur à propos d'une boucle

    **Grav 1.7.8** ajoute le support pour définir n'importe quel **blueprint** dans votre thème. Cela signifie que si vous avez des plans de page dans le dossier `blueprints/pages/`, les emplacements de plans standard sont utilisés, tout comme dans les plugins. Malheureusement, certains thèmes plus anciens peuvent avoir un mélange de fichiers dans `blueprints/` et `blueprint/pages`, ce qui interrompt la détection et provoque soit des champs manquants dans l'administrateur lors de l'édition des pages, soit une erreur fatale : `boucle détectée lors de l'extension du fichier blueprint`.

    Si l'une de ces erreurs se produit, consultez la [section Dépannage](#Dépannag) pour le correctif.

<h3 id="Guide de mise à jour rapide">Guide de mise à jour rapide.
<a href="#Guide de mise à jour rapide" class="toc-anchor after"></a></h3>

<div class = "notice note">
<strong>Grav 1.7</strong> nécessite <strong>PHP 7.3.6</strong> ou une version ultérieure. La version recommandée est la dernière version de <strong>PHP 7.4</strong>.
</div>

<h2 id="YAML">YAML.
<a href="#YAML" class="toc-anchor after"></a></h2>

<div class = "notice warning">
<strong>IMPORTANT</strong> : l'analyseur YAML Grav 1.7 est plus strict et votre site peut tomber en panne si vous avez des erreurs de syntaxe dans vos fichiers de configuration ou vos en-têtes de page. Cependant, si vous mettez à jour votre site existant à l'aide de `bin/gpm` ou du processus de mise à niveau de `Admin Plugin`, la majeure partie de la syntaxe YAML endommagée fonctionne.
</div>

Pour revenir à l'ancien comportement, vous devez vous assurer que vous disposez des paramètres suivants dans `user/config/system.yaml` :

```yaml
strict_mode:
  yaml_compat: true
```

ou dans Admin sous **Configuration → Advanced -> Yaml Compability**

![Compatibilité YAML](https://learn.getgrav.org/user/pages/08.advanced/09.grav-development/03.grav-17-upgrade-guide/yaml-compat.png)

<div class = "notice tip">
<strong>ASTUCE : Le guide de mise à jour Grav 1.6</strong> a une section <a href="../avance-grav-up-to-1-6/#Analyse YAML">YAML Parsing</a> intégrée. Il est très important de le lire avant !
</div>

Par défaut, Grav 1.7 utilise un **analyseur Symfony 4.4 YAML**, qui suit plus étroitement la [spécification standard YAML](https://yaml.org/spec) que les anciennes versions de Grav. Cela signifie que les fichiers YAML qui fonctionnaient correctement auparavant peuvent provoquer des erreurs résultant d'un YAML non valide. Cependant, Grav reviendra par défaut à l'ancienne version 3.4 de l'analyseur pour que votre site reste opérationnel.

<div class = "notice tip">
<strong>ASTUCE</strong> : vous devez exécuter la <strong>commande CLI</strong> <code>bin/grav yamllinter</code> ou visiter dans <strong>Admin > Tools > Reports</strong> avant et après la mise à niveau et corriger tous les avertissements et erreurs liés à YAML.
</div>

<h3 id="Twig">Twig.
<a href="#Twig" class="toc-anchor after"></a></h3>

<div class = "notice warning">
<strong>IMPORTANT</strong> : Grav 1.7 active <strong>Twig Auto-Escaping</strong> par défaut. Cependant, si vous mettez à jour votre site existant à l'aide de <code>bin/gpm</code> ou le processus de mise à niveau de <code>Admin Plugin</code> conserve les paramètres d'échappement automatique existants.
</div>

Pour revenir à l'ancien comportement, vous devez vous assurer que vous disposez des paramètres suivants dans `user/config/system.yaml` :

```yaml
twig:
  autoescape: false
strict_mode:
  twig_compat: true
```

ou dans Admin sous **Configuration → Advanced -> Yaml Compability**

Et n'oubliez pas de vider le cache après avoir fait cela !

![Compatibilité Twing](https://learn.getgrav.org/user/pages/08.advanced/09.grav-development/03.grav-17-upgrade-guide/twig-compat.png)

<div class = "notice tip">
<strong>ASTUCE</strong> : Le guide de mise à niveau Grav 1.6 comporte une section <a href="../avance-grav-up-to-1-6/#Twig"><strong>Twig</strong></a> dédiée. Il est très important de la lire d'abord !
</div>

Le moteur de template Twig a été mis à jour vers la version 1.43, mais il prend également en charge Twig 2.13. Afin de prendre en charge cette nouvelle version de Twig, vous devez mettre à jour toute ancienne syntaxe dans vos modèles Twig. **Grav 1.6 Upgrade Guide** vous aide à le faire.

Les changements supplémentaires dans les modèles sont :

* Ajout d'une nouvelle balise Twig `{% cache %}` éliminant le besoin d'extension twigcache.
* Ajout de la fonction twig `array_diff()`.
* Ajout de la fonction twig `template_from_string()`.
* Ajout d'une nouvelle fonction twig `svg_image()` pour faciliter l'inclusion de la source SVG dans Twig.
* Amélioration de la fonction `url()` twig pour prendre le troisième paramètre `(true)` pour renvoyer l'URL sur un fichier inexistant au lieu de renvoyer false.
* Amélioration du filtre `|array` twig pour travailler avec les itérateurs et les objets avec la méthode `toArray()`.
* Amélioration de la fonction twig `authorize()` pour mieux fonctionner avec les paramètres de règle imbriqués.
* Amélioration du filtre de twig `|yaml_serialize` : prise en charge ajoutée des objets `JsonSerializable` et d'autres objets de type tableau.
* Ajout de modèles par défaut pour `external.html.twig`, `default.html.twig` et `modular.html.twig`.
* **RUPTURE DE COMPATIBILITÉ ARRIÈRE** : Utilisez `{% script 'file.js' at 'bottom' %}` au lieu de `in 'bottom'` qui est cassé.

<h2 id="Formulaires">Formulaires.
<a href="#Formulaires" class="toc-anchor after"></a></h2>

<div class = "notice warning">
<strong>IMPORTANT</strong> : Grav 1.7 modifie le comportement de <strong>Strict Validation</strong>. Toutefois, si vous mettez à jour votre site existant à l'aide de <code>bin/gpm</code> ou du processus de mise à niveau de <code>Admin Plugin</code>, le comportement en mode strict existant est conservé.
</div>

**Améliorations du mode strict** : Dans les formulaires, déclaration de `validation : strict` n'était pas aussi strict que nous l'espérions à cause d'un bogue. Le mode strict devrait empêcher les formulaires d'envoyer des champs supplémentaires et cela a été corrigé dans Grav 1.7. Malheureusement, bon nombre des anciens formulaires étaient déclarés stricts même s'ils contenaient des données supplémentaires.

Pour revenir à l'ancien comportement, vous devez vous assurer que vous disposez du paramètre suivant dans `user/config/system.yaml` :

```yaml
strict_mode:
  blueprint_compat: true
```

**La détection d'injection XSS** est désormais activée par défaut dans tous les formulaires frontaux. Consultez la [documentation](/formulaires-ref-options-formulaires/#XSS Checks) pour savoir comment désactiver ou personnaliser les vérifications par formulaire et par champ.

Pour cette raison, nous avons ajouté une nouvelle option de configuration `system.strict_mode.blueprint_compat: true` pour conserver l'ancien comportement de `validation :  strict`. Il est recommandé de désactiver ce paramètre pour améliorer la sécurité du site, mais avant cela, veuillez rechercher dans tous vos formulaires si vous utilisiez la fonctionnalité  `validation : strict`. Si tel était le cas, supprimez la ligne ou testez si le formulaire fonctionne toujours.

<div class = "notice note">
<strong>REMARQUE</strong> : Ce mécanisme de secours de rétrocompatibilité sera supprimé dans Grav 2.0.
</div>

<h3 id="Environnements et Multi-Sites">Environnements et Multi-Sites.
<a href="#Environnements et Multi-Sites" class="toc-anchor after"></a></h3>

<div class = "notice warning">
<strong>Important</strong> : Grav 1.7 déplace les <a href="../avance-config-environnement">environnements</a> dans le dossier <code>user://env</code>. L'ancien emplacement fonctionne toujours, mais il est préférable de déplacer les environnements dans un seul emplacement, les futures fonctionnalités pourraient en dépendre.
</div>

Grav 1.7 ajoute également la prise en charge de la [configuration de l'environnement basé sur le serveur](/avance-config-environnement/#Configuration de l'environnement basé sur le serveur) et de la [configuration multisite basée sur le serveur](/avance-config-multisite/#Configuration multisite basée sur serveur). Cette fonctionnalité est pratique si vous souhaitez utiliser par exemple des conteneurs Docker et que vous souhaitez les rendre indépendants du domaine que vous utilisez. Ou si vous ne souhaitez pas stocker de secrets dans la configuration, mais les stocker dans la configuration de votre serveur.

De plus, le fichier `setup.php` peut désormais se trouver dans `GRAV_ROOT/setup.php` ou `GRAV_ROOT/GRAV_USER_PATH/setup.php`. Le deuxième emplacement facilite l'utilisation des environnements avec des référentiels git contenant uniquement le dossier utilisateur.

<h3 id="Comptes utilisateur">Comptes utilisateur.
<a href="#Comptes utilisateur" class="toc-anchor after"></a></h3>

L'administrateur dispose désormais d'une nouvelle [administration de comptes](/comptes) à l'aide **d'utilisateurs flexibles** :

* [Gestionnaire de comptes utilisateurs](/compte-utilisateur)
* [Gestionnaire de groupes d'utilisateurs](/groupe-utilisateur)

<div class = "notice tip">
<strong>REMARQUE</strong> : la fonctionnalité Utilisateurs flexibles n'est pas encore utilisée dans l'interface de votre site.
</div>

<h3 id="Pages">Pages.
<a href="#Pages" class="toc-anchor after"></a></h3>

[L'administration des pages](/dashboard-pages)</span> existantes a été grandement améliorée avec les **pages flexibles** :

* Affichage de liste retravaillé : bien meilleur support pour les grands sites
* Meilleur contrôle d'accès : prise en charge [CRUD ACL](/dashboard-pages-permissions/) avec les propriétaires de pages
* Meilleure prise en charge multilingue

<div class = "notice info">
<strong>RUPTURE DE COMPATIBILITÉ ARRIÈRE</strong> : Nous avons corrigé la page d'erreur 404 lorsque vous accédez à une page non routable avec des pages enfants routables et visibles en dessous. Maintenant, vous êtes redirigé vers la première page enfant routable et visible à la place. C'est probablement ce que vous vouliez en premier lieu.
</div>

<div class = "notice note">
<strong>REMARQUE</strong> : la fonctionnalité Flex Pages n'est pas encore utilisée dans l'interface de votre site.
</div>

<h3 id="Multilingue">Multilingue.
<a href="#Multilingue" class="toc-anchor after"></a></h3>

Grav 1.7 a changé le comportement du fonctionnement des replis multilingues pour les pages.

Auparavant, si la page n'existait pas avec la langue demandée, l'ancienne implémentation recherchait la prochaine langue prise en charge. Cela signifiait que la page non traduite était toujours affichée, mais la page pouvait utiliser une langue inconnue du lecteur.

Le nouveau comportement consiste à se rabattre uniquement sur la langue par défaut du site. Ce comportement par défaut peut être remplacé en définissant des langues de secours par langue à l'aide de l'option de configuration `system.languages.content_fallback`.

Si la page n'existe dans aucune des langues de secours, **404 Not Found** s'affichera à la place.

<div class = "notice info">
<strong>RUPTURE DE COMPATIBILITÉ ARRIÈRE</strong>  : Veuillez ajouter les langues de secours correctes pour le contenu de la page dans <code>system.yaml</code> ou admin : <strong>Configuration > System > Languages > Content Language Fallback</strong>
</div>

<h3 id="Médias">Médias.
<a href="#Médias" class="toc-anchor after"></a></h3>

La gestion des médias a été grandement améliorée dans Grav 1.7. Certains faits saillants sont :

* Prise en charge du format d'image `webp`.
* Markdown : Ajout de la prise en charge des attributs natifs `loading=lazy` sur les images. Peut être défini dans `system.images.defaults` ou par image md avec `?loading=lazy`.
* Ajout de la possibilité de ne pas traiter des éléments spécifiques `noprocess` uniquement dans les extraits de lien/d'image, par ex. `http://foo.com/page?id=foo&target=_blank&noprocess=id`.

<h2 id="CLI">CLI.
<a href="#CLI" class="toc-anchor after"></a></h2>

Certains faits marquants sont :

 *   Toutes les commandes CLI acceptent maintenant les paramètres `--env` et `--lang` pour définir respectivement l'environnement et la langue utilisée (`-e` ne fonctionne plus).
* Ajout d'une nouvelle commande CLI `bin/grav server` pour exécuter facilement les serveurs Web intégrés Symfony ou PHP.
* Vérification améliorée de la commande cron du `Scheduler` et informations CLI plus utiles.
* Ajout de la nouvelle option `-r <job-id>` pour la commande CLI du planificateur pour forcer l'exécution d'un travail.
* Amélioration de la commande CLI `bin/grav yamllinter` en ajoutant une option pour rechercher les problèmes de YAML Linting à partir de l'ensemble du site ou du dossier personnalisé.
* Les échecs de la commande CLI/GPM renvoient désormais un code non nul (permettant la détection d'erreur si la commande échoue).

<h3 id="Configuration">Configuration.
<a href="#Configuration" class="toc-anchor after"></a></h3>

Ajout d'une nouvelle option de configuration pour conserver la langue par défaut dans les fichiers `.md` si elle est définie sur `false`.

* system.yaml : `langues.include_default_lang_file_extension` : **true**|false
* Admin : **Configuration > Systèmes > Langues > Inclure la langue par défaut dans l'extension de fichier**.

Ajout d'une nouvelle option de configuration pour définir les langues de contenu de secours individuellement pour chaque langue.

* system.yaml : `languages.content_fallback` : voir [Configuration de la langue](/multi-langue/)
* Admin : **Configuration > Système > Langues > Langue de remplacement du contenu**.

Ajout d'une nouvelle option de configuration pour choisir entre la barre de débogage et le mouvement d'horloge.

* system.yaml : `debugger.provider` :  **clockwork**|debugbar.
*Admin : **Configuration > Système > Débogueur > Fournisseur de débogueur**.

Ajout d'une nouvelle option de configuration pour masquer les informations potentiellement sensibles.

* system.yaml : `debugger.censored` : **false**|true.
* Admin : **Configuration > Système > Débogueur > Données sensibles au censeur**.

Ajout d'une nouvelle option de configuration pour conserver l'ancienn comportement `validation: strict`.

* system.yaml : `strict_mode.blueprint_compat` : **true**|false.
* Admin : **Configuration > Système > Avancé > Compatibilité Blueprint**.

Ajout de la prise en charge de la configuration système pour les en-têtes` HTTP_X_FORWARDED` (hôte désactivé par défaut).

* system.yaml : `http_x_forwarded.protocol` : **true**|false.
* Admin : **Configuration > Système > Avancé > HTTP_X_FORWARDED_PROTO activé**.
* system.yaml : `http_x_forwarded.host` : true|**false**.
* Admin : **Configuration > Système > Avancé > HTTP_X_FORWARDED_HOST activé**.
* system.yaml : `http_x_forwarded.port : **true**|false.
* Admin : **Configuration > Système > Avancé > HTTP_X_FORWARDED_PORT activé**.
* system.yaml : `http_x_forwarded.ip : true|**false**.
* Admin : **Configuration > Système > Avancé > IP HTTP_X_FORWARDED activé**.

Ajout d'une nouvelle option de configuration `security.sanitize_svg` pour supprimer le code potentiellement dangereux des fichiers SVG.

* security.yaml : `sanitize_svg` : **true**|false.
* Admin : **Configuration > Sécurité > Nettoyer SVG**.

<h2 id="DÉVELOPPEURS">DÉVELOPPEURS.
<a href="#DÉVELOPPEURS" class="toc-anchor after"></a></h2>

<h3 id="Débogage">Débogage.
<a href="#Débogage" class="toc-anchor after"></a></h3>

* Ajout de la prise en charge des outils de développement [Clockwork](https://underground.works/clockwork) (maintenant débogueur par défaut).
* Ajout de la prise en charge de l'extension PHP [Tideways XHProf](https://github.com/tideways/php-xhprof-extension) pour les appels de méthode de profilage.
* Ajout du profilage Twig pour le débogueur Clockwork.

<h3 id="Utiliser le chargeur automatique de compositeur">Utiliser le chargeur automatique de compositeur.
<a href="#Utiliser le chargeur automatique de compositeur" class="toc-anchor after"></a></h3>

* Mise à jour de `bin/composer.phar` vers `2.0.2` qui est tout nouveau et beaucoup plus rapide.

* Veuillez ajouter le fichier `composer.json` à votre plugin et exécuter `composer update --no-dev` (et n'oubliez pas de le maintenir à jour) :

    compositeur.json

```json
{
    "name": "getgrav/grav-plugin-example",
    "type": "grav-plugin",
    "description": "Example plugin for Grav CMS",
    "keywords": ["example", "plugin"],
    "homepage": "https://github.com/getgrav/grav-plugin-example",
    "license": "MIT",
    "authors": [
        {
            "name": "...",
            "email": "...",
            "homepage": "...",
            "role": "Developer"
        }
    ],
    "support": {
        "issues": "https://github.com/getgrav/grav-plugin-example/issues",
        "docs": "https://github.com/getgrav/grav-plugin-example/blob/master/README.md"
    },
    "require": {
        "php": ">=7.1.3"
    },
    "autoload": {
        "psr-4": {
            "Grav\\Plugin\\Example\\": "classes/",
            "Grav\\Plugin\\Console\\": "cli/"
        },
        "classmap":  [
            "example.php"
        ]
    },
    "config": {
        "platform": {
            "php": "7.1.3"
        }
    }
}
```

Voir le [schéma Composer](https://getcomposer.org/doc/04-schema.md)

* Veuillez utiliser autoloader au lieu de `require` dans le code :

    exemple.php

```php
/**
 * @return array
 */
public static function getSubscribedEvents(): array
{
   return [
      'onPluginsInitialized' => [
         // This is only required in Grav 1.6. Grav 1.7 automatically calls $plugin->autolaod() method.
         ['autoload', 100000],
      ]
   ];
 }

/**
 * Composer autoload.
 *
 * @return \Composer\Autoload\ClassLoader
 */
public function autoload(): \Composer\Autoload\ClassLoader
{
   return require __DIR__ . '/vendor/autoload.php';
}
```

* Plugins & Thèmes : Appelez `$plugin->autoload()` et `$theme->autoload()` automatiquement lorsque l'objet est initialisé.

*  Assurez-vous que votre code n'utilise pas `require` ou `include` pour charger des classes.

<h3 id="Blueprints de plugins/thèmes <code>(blueprints.yaml</code>)">Blueprints de plugins/thèmes <code>(blueprints.yaml</code>).
<a href="#Blueprints de plugins/thèmes <code>(blueprints.yaml</code>)" class="toc-anchor after"></a></h3>

* S'il-vous-plait ajoutez:

```yaml
slug: folder-name
type: plugin|theme
```

* Assurez-vous de mettre à jour vos dépendances. Je recommande de régler Grav sur 1.6 ou 1.7 et de mettre à jour votre code/fournisseur vers PHP 7.1.

```yaml
dependencies:
    - { name: grav, version: '>=1.6.0' }
```

* `themes` ajoutés aux plans et à la configuration mis en cache.

* **Grav 1.7.8** ajoute le support pour définir n'importe quel **blueprint** dans votre thème. Déplacez tous les fichiers et dossiers de `blueprints/` dans `blueprints/pages/` pour que votre thème reste compatible. N'oubliez pas également de mettre à jour la dépendance Grav minimale à `> = 1.7.8.`.

<h3 id="Sessions">Sessions.
<a href="#Sessions" class="toc-anchor after"></a></h3>

* L'ID de session change maintenant lors de la connexion pour éviter les problèmes de fixation de session.
* Ajout de la méthode `Session :: regenerateId()` pour éviter correctement les problèmes de fixation de session.

<h3 id="ACL">ACL.
<a href="#ACL" class="toc-anchor after"></a></h3>

* `user.authorize()` exige maintenant que l'utilisateur soit autorisé (vérification 2FA réussie), à ​​moins que la règle ne contienne `login` dans son nom.

* Ajout de la prise en charge d'ACL plus avancées (CRUD)

* **BC BREAK** `user.authorize()` et Flex `object.isAuthorized()` ont maintenant deux états de refus : `false` et `null`..

    Assurez-vous que vous n'avez pas de contrôles stricts contre false : `$user->authorize($action) === false` (PHP) ou `user.authorize(action) is same as(false)` (Twig).

    Pour les vérifications négatives, vous devez utiliser `!user->authorize($action)` (PHP) ou `not user.authorize(action) `(Twig).

    Le changement a été fait pour permettre des règles de refus fortes en enchaînant les actions si les précédentes ne correspondent pas : `user.authorize(action1) ?? user.authorize(action2) ?? user.authorize(action3)`.

    Notez que la fonction Twig `authorize ()` **conservera** toujours l'ancien comportement !

<h3 id="Pages">Pages.
<a href="#Pages" class="toc-anchor after"></a></h3>

* Ajout de modèles par défaut pour `external.html.twig`, `default.html.twig` et `modular.html.twig`.

* L'administrateur utilise `Flex Pages` par défaut (peut être désactivé à partir du plugin `Flex-Objects`).

![Désactiver les pages flexibles](https://learn.getgrav.org/user/pages/08.advanced/09.grav-development/03.grav-17-upgrade-guide/disable-flex-pages.png)

* Ajout de la prise en charge des autorisations d'administrateur spécifiques à la page pour les `Flex Pages`.

* Ajout de la prise en charge de la page racine pour les `Flex Pages`.

* Correction des mauvais appels `Pages::dispatch()` (avec redirection) alors que nous voulions vraiment appeler `Pages::find()`.

* Ajout de la méthode : `Pages::getCollection()`.

* Déplacement de la logique de `collection()` et `evaluate()` de la classe `Page` vers la classe `Pages`.

* **OBSOLETE** `$page->modular()` en faveur de `$page->isModule()`.

* **OBSOLETE**  `PageCollectionInterface::nonModular()` en faveur de `PageCollectionInterface::pages()`.

* **OBSOLETE**  `PageCollectionInterface::modular()` en faveur de `PageCollectionInterface::modules()`.

* **BC BREAK** Fixed `Page::modular()` et `Page::modularTwig()` retournant `null` pour les dossiers et autres pages non initialisées. Ne devrait pas affecter votre code à moins que vous ne vérifiiez contre `false` ou `null`.

* **BC BREAK** Toujours utiliser `\Grav\Common\Page\Interfaces\PageInterface` au lieu de `\Grav\Common\Page\Page` dans les signatures de méthode.

* L'administrateur utilise désormais les `Flex Pages` par défaut, la collecte se comportera de manière légèrement différente.

* **BC BREAK** `$page->topParent()` peut renvoyer la page elle-même au lieu de `null`.

* **BC BREAK* `$page->header()` peut maintenant `\Grav\Common\Page\Header` renvoyer un objet au lieu de `stdClass`, vous devez gérer les deux (Flex vs regular).

<h3 id="Médias">Médias.
<a href="#Médias" class="toc-anchor after"></a></h3>

* Ajout de la méthode `MediaTrait :: freeMedia ()` pour libérer les médias (et la mémoire).
* Ajout de la prise en charge du téléchargement et de la suppression d'images directement dans `Media` en utilisant PSR-7.
* Types d'actifs ajustés pour permettre l'extension des actifs dans la classe.
* **BC BREAK** Media n'étend plus les `Getters`, l'accès à `$media->$filename` ne fonctionne plus, utilisez plutôt `$media[$filename]` !

<h3 id="Markdown">Markdown.
<a href="#Markdown" class="toc-anchor after"></a></h3>

* **BC BREAK** Mise à niveau de Parsedown vers 1.7 pour Parsedown-Extra 0.8. Les plugins qui étendent Parsedown peuvent avoir besoin d'un correctif pour s'afficher en HTML.
* Ajout de la nouvelle méthode `Excerpts :: processLinkHtml()`.

<h3 id="Utilisateurs">Utilisateurs.
<a href="#Utilisateurs" class="toc-anchor after"></a></h3>

* Ajout d'un support expérimental pour `Flex Users` dans le frontend (pas encore recommandé d'utiliser).
 * L'administrateur utilise `Flex Users` par défaut (peut être désactivé à partir du plug-in `Flex-Objects`).
* `Flex Users` améliorés : respectez les plans et autorisez l'utilisation de Flex uniquement en mode administrateur.
* `Flex Users` améliorés : l'ACL des utilisateurs et des groupes prend désormais en charge les autorisations de refus.
* `UserInterface ::authorize()` modifié pour renvoyer `null` ayant la même signification que `false` si l'accès est refusé en raison de l'absence de règle correspondante.
* **OBSOLETE** `\Grav\Common\User\Group` en faveur de `$grav['user_groups']`, qui contient la collection Flex UserGroup.
* **BC BREAK** Toujours utiliser \`Grav\Common\User\Interfaces\UserInterface` au lieu de `\Grav\Common\User\User` dans les signatures de méthode.

<h3 id="Flex">Flex.
<a href="#Flex" class="toc-anchor after"></a></h3>

* N'utilisez pas directement les classes Flex `Framework`, il est préférable d'utiliser ou d'étendre les classes sous l'espace de noms `Grav\Common\Flex\Types\Generic`.
* Ajout de `$grav['flex']` pour accéder à tous les répertoires Flex enregistrés
    * Ajout `de FlexRegisterEvent` qui se déclenche lorsque `$grav['flex'] `est accédé pour la première fois.
* Ajout de la méthode `hasFlexFeature()` pour tester si `FlexObject` ou `FlexCollection` implémente une fonctionnalité donnée.
* Ajout de la méthode `getFlexFeatures()` pour renvoyer toutes les fonctionnalités implémentées par `FlexObject` ou `FlexCollection`.
* Ajout de la méthode `FlexObject :: refresh ()` pour s'assurer que l'objet est à jour.
* Ajout de `FlexStorage :: getMetaData()` pour obtenir des méta-informations d'objet mises à jour pour les clés répertoriées.
* Ajout de l'interface `FlexDirectoryInterface`.
* Ajout de l'option de recherche `same_as` aux Flex Objects.
* La méthode `Flex Pages $page->header()` renvoie l'objet `\Grav\Common\Page\Header`, l'ancienne classe `Page` renvoie toujours `stdClass`.
* Renommé `PageCollectionInterface::nonModular()` en `PageCollectionInterface::pages()` et obsolète l'ancienne méthode
    Renommé PageCollectionInterface::modular() en PageCollectionInterface::modules() et l'ancienne méthode est obsolète.
* `FlexDirectory::getObject()` peut maintenant être appelé sans aucun paramètre pour créer un nouvel objet.
* Implémentation d'une configuration personnalisable par type de répertoire flexible.
* **OBSOLETE** `FlexDirectory::update()` et `FlexDirectory::remove()`.
* **BC BREAK** Déplacement de toutes les classes de type `Flex sous Grav\Common\Flex`.
* **BC BREAK** `FlexStorageInterface::getStoragePath()` et `getMediaPath() `peuvent maintenant renvoyer `null`.
* **BC BREAK** Les objets Flex ne renvoient plus de clé temporaire s'ils n'en ont pas ; la clé vide est renvoyée à la place.
* **BC BREAK** Argument de rechargement ajouté à `FlexStorageInterface::getMetaData()`.
* Vous pouvez ajouter le fichier `edit_list.html.twig` à un champ de formulaire afin de personnaliser l'apparence dans la vue de la liste.

<h3 id="Multilingue">Multilingue.
<a href="#Multilingue" class="toc-anchor after"></a></h3>

* Prise en charge améliorée de la langue pour la classe `Route`.
* Traductions : renommer MODULAIRE en MODULE partout.
* Ajout de `Language::getPageExtensions()` pour obtenir la liste complète des extensions de langue de page prises en charge.
* **BC BREAK** Fixed `Language::getFallbackPageExtensions()` pour revenir uniquement à la langue par défaut au lieu de parcourir toutes les langues.

<h3 id="Multi-sites">Multi-sites.
<a href="#Multi-sites" class="toc-anchor after"></a></h3>

* Ajout du support pour avoir tous les sites/environnements sous le dossier `user/env`.

<h3 id="Sérialisation">Sérialisation.
<a href="#Sérialisation" class="toc-anchor after"></a></h3>

* Toutes les classes utilisent maintenant la sérialisation PHP 7.4. Les anciennes méthodes `Serializable` sont désormais définitives et ne peuvent pas être remplacées.

<h3 id="Blueprints">Blueprints.
<a href="#Blueprints" class="toc-anchor after"></a></h3>

* Ajout du filtre `flatten_array` pour la validation des champs de formulaire. 
* Ajout de la prise en charge de `security@: ou : [admin.super, admin.pages]` dans les plans (prise en charge du mode ET/OU imbriqué).
* Validation du Blueprint : Ajout de `validate : value_type : bool|int|float|string|trim` to `array` pour filtrer toutes les valeurs à l'intérieur du tableau.
* Si vos plugins ont un dossier blueprints, l'initialiser dans l'événement sera trop tard. Faites ceci à la place :

```yaml
class MyPlugin extends Plugin
{
    /** @var array */
    public $features = [
        'blueprints' => 0, // Use priority 0
    ];
}
```

<h3 id="Événements">Événements.
<a href="#Événements" class="toc-anchor after"></a></h3>

* Utilisez `Symfony EventDispatcher` directement au lieu de `rockettheme/toolbox wrapper`.
* Ajout de la méthode `$grav->dispatchEvent()` pour les événements PSR-14.
* Ajout de `PluginsLoadedEvent` qui se déclenche après que les plugins ont été chargés mais pas encore initialisés.
* Ajout de `SessionStartEvent` qui se déclenche au démarrage de la session.
* Ajout de `FlexRegisterEvent` qui se déclenche lorsque `$grav['flex']` est accédé pour la première fois.
* Ajout de `PermissionsRegisterEvent` qui se déclenche lors du premier accès à `$grav['permissions']`.
* Ajout de l'événement `onAfterCacheClear`.
* Vérifiez l'événement `onAdminTwigTemplatePaths`, il ne doit PAS être :

```yaml
public function onAdminTwigTemplatePaths($event)
{
    // This code breaks all the other plugins in admin, including Flex Objects
    $event['paths'] = [__DIR__ . '/admin/themes/grav/templates'];
}
```

mais:

```yaml
public function onAdminTwigTemplatePaths($event)
{
    // Add plugin template path for admin.
    $paths = $event['paths'];
    $paths[] = __DIR__ . '/admin/themes/grav/templates';
    $event['paths'] = $paths;
}
```

<h3 id="Javascript">Javascript.
<a href="#Javascript" class="toc-anchor after"></a></h3>

* Mise à jour de JQuery groupé vers la dernière version `3.5.1`.

<h3 id="Divers">Divers.
<a href="#Divers" class="toc-anchor after"></a></h3>

* Ajout de `Utils ::functionExists()` : compatible avec PHP 8 `function_exists()`.
* Ajout des méthodes d'assistance `Utils::isAssoc()` et `Utils::isNegative()`.
* Ajout de la méthode `Utils :: simpleTemplate()` pour une modélisation très simple des variables.
* Ajout de `Utils ::fullPath()` pour obtenir le chemin complet d'un fichier, qu'il s'agisse d'un flux, d'un relatif, etc.
* Prend en charge le remplacement personnalisable des caractères nuls dans `CSVFormatter :: decode ()`.
* Ajout de la nouvelle fonction `Security :: sanitizeSVG()`.
* Ajout de la méthode `$grav->close()` pour terminer correctement la requête avec une réponse.
* Ajout de la méthode `Folder::countChildren()` pour déterminer si un dossier a des dossiers enfants.
* Prise en charge des liens symboliques lors de l'enregistrement du fichier.
* Ajout de la méthode `Route :: getBase()`.
* **BC BREAK** Rend les objets `Route` immuables. Cela signifie que vous devez faire : `{% set route = route.withExtension('.html') %}` (pour toutes les méthodes `withX`) pour conserver la version mise à jour.
* Meilleure gestion de `Content-Encoding` dans Apache lorsque la compression de contenu est désactivée.
* Ajout d'une fonction de compatibilité `Uri::getAllHeaders()`.
* Autoriser le passage des options `JsonFormatter` sous forme de chaîne.

<h3 id="CLI">CLI.
<a href="#CLI" class="toc-anchor after"></a></h3>

* **BC BREAK** De nombreux plugins initialisent Grav d'une mauvaise manière, il n'est pas sûr d'initialiser les plugins et le thème par vous-même
    * Les appels suivants nécessitent Grav 1.6.21 ou une version ultérieure, il est donc recommandé de définir la dépendance Grav sur cette version
    * Dans la méthode `serve()` :
    * Appelez `$this->setLanguage($langCode)`; avant de faire quoi que ce soit d'autre si vous voulez définir la langue (ou utiliser par défaut).
    * Appelez l'un des suivants :
        * `$this->initializeGrav()`; Déjà appelé si vous êtes dans bin/plugin, sinon vous devrez peut-être appeler celui-ci :
        * `$this->initializePlugins()`; Cela initialise grav, plugins (jusqu'à onPluginsInitialized).
        * `$this->initializeThemes()`; Cela initialise grav, plugins et thème.
        * `$this->initializePages()`; Cela initialise grav, plugins, thème et tout ce dont les pages ont besoin.
* C'est une bonne idée de préfixer vos classes de commandes CLI avec le nom de votre plugin, sinon il peut y avoir des conflits de noms (nous en avons déjà !).

<h3 id="Bibliothèques utilisées">Bibliothèques utilisées.
<a href="#Bibliothèques utilisées" class="toc-anchor after"></a></h3>

* Mise à jour des composants Symfony vers la version 4.4, veuillez mettre à jour toutes les fonctionnalités obsolètes dans votre code.
* **BC BREAK** Veuillez exécuter `bin/grav yamllinter` pour trouver toute erreur d'analyse YAML sur votre site (y compris vos plugins et thèmes).

<h2 id="PLUGINS">PLUGINS.
<a href="#PLUGINS" class="toc-anchor after"></a></h2>

<h3 id="Administrateur">Administrateur.
<a href="#Administrateur" class="toc-anchor after"></a></h3>

* Ajout de l'option `Content Editor` au plan de compte d'utilisateur.
* **BC BREAK** Admin n'initialisera plus les pages frontend, cela a été fait pour accélérer considérablement le plugin Admin.
    Veuillez appeler `$grav['admin']->enablePages()` ou `{% do admin.enablePages() %}` si vous avez besoin d'accéder aux pages frontales. Cet appel peut être effectué plusieurs fois en toute sécurité.
    Si vous utilisez `Flex Pages`, veuillez utiliser Flex Directory à la place, cela rendra votre code beaucoup plus rapide.
* L'administrateur utilise désormais Flex pour modifier `Accounts` et `Pages`. Si votre plugin s'accroche à l'un de ceux-ci, assurez-vous qu'il fonctionne toujours.
* Le cache administrateur est activé par défaut, assurez-vous que votre plugin efface le cache si nécessaire. Veuillez éviter de vider tout le cache !

<h3 id="Noyau de code court">Noyau de code court.
<a href="#Noyau de code court" class="toc-anchor after"></a></h3>

* **OBSOLETE** Chaque shortcode doit avoir la méthode `init()`, les classes sans elle cesseront de fonctionner à l'avenir.

<h2 id="Dépannage des problèmes">Dépannage des problèmes.
<a href="#Dépannage des problèmes" class="toc-anchor after"></a></h2>

`ERREUR : modèle flex-objects.html.twig introuvable pour la page`.

Si vous obtenez cette erreur après la mise à niveau vers Grav 1.7, cela peut être lié à un plugin appelé `content-edit`. Si vous désactivez ce plugin, l'erreur devrait se résoudre d'elle-même. [Grav Numéro #3169](https://github.com/getgrav/grav/issues/3169)

<h3 id="Administrateur non traduit">Administrateur non traduit.
<a href="#Administrateur non traduit" class="toc-anchor after"></a></h3>

Si votre plugin d'administration ressemble à ceci :

![Administrateur non traduit](https://learn.getgrav.org/user/pages/08.advanced/09.grav-development/03.grav-17-upgrade-guide/untranslated.png)

La correction est très simple et peut être effectuée même lorsqu'elle n'est pas entièrement traduite. Accédez simplement à `PLUGIN_ADMIN.CONFIGURATION`, puis dans `PLUGIN_ADMIN.LANGUAGES`, définissez `PLUGIN_ADMIN.LANGUAGE_TRANLATIONS` sur `PLUGIN_ADMIN.YES` :

![Corriger les traductions](https://learn.getgrav.org/user/pages/08.advanced/09.grav-development/03.grav-17-upgrade-guide/fix-translations.png)

<h3 id="Les plans de page cessent de fonctionner dans l'administration">Les plans de page cessent de fonctionner dans l'administration.
<a href="#Les plans de page cessent de fonctionner dans l'administration" class="toc-anchor after"></a></h3>

Si vous ne pouvez pas voir vos champs personnalisés lors de la modification de la page, votre thème utilise deux emplacements en conflit pour les plans de page.

Si le thème n'a pas été créé par vous, veuillez signaler un bogue à l'auteur du thème.

Pour corriger le bogue, vous devez déplacer tous les fichiers et dossiers de votre thème de `blueprints/` vers `blueprints/pages/` (nécessite **Grav 1.7.8+**). Sinon, si le thème doit prendre en charge les anciennes versions de Grav, faites l'inverse.

<h3 id="Erreur : boucle détectée lors de l'extension du fichier Blueprint">Erreur : boucle détectée lors de l'extension du fichier Blueprint.
<a href="#Erreur : boucle détectée lors de l'extension du fichier Blueprint" class="toc-anchor after"></a></h3>

La solution la plus simple pour une erreur de boucle consiste à déplacer les fichiers à leur emplacement approprié, veuillez consulter le problème ci-dessus.

Vous pouvez également résoudre le problème en modifiant le plan de la page cassée à partir de :

```yaml
extends@:
    type: [NAME]
    context: 'blueprints://pages'
```

où `[NAME`] est le nom de fichier (sans l'extension de fichier) du plan lui-même, pour :

```yaml
extends@: self@
```

<h3 id="Style CSS manquant dans l'administration">Style CSS manquant dans l'administration.
<a href="#Style CSS manquant dans l'administration" class="toc-anchor after"></a></h3>

Il a été signalé qu'après la mise à niveau vers les derniers Grav 1.7 et Admin 1.10, certaines pages d'administration semblent cassées et pas entièrement stylées. Cela pourrait être lié au plugin `imagecreate`. Désactiver ce plugin ne suffit pas, vous devez supprimer complètement le plugin, puis l'erreur devrait se résoudre d'elle-même. [Problème d'administration #2035](https://github.com/getgrav/grav-plugin-admin/issues/2035)

<h2 id="Revenir à Grav 1.6">Revenir à Grav 1.6.
<a href="#Revenir à Grav 1.6" class="toc-anchor after"></a></h2>

Bien que nous vous recommandons de résoudre tous les problèmes que vous pourriez avoir pour vous assurer que Grav 1.7 et les futures mises à jour seront une mise à niveau facile, il y aura des scénarios où vous avez une fonctionnalité de plug-in personnalisée, ou n'avez pas les ressources de développement à portée de main, et avez juste besoin de revenir rapidement à Grav 1.6.

Si vous avez un accès CLI au site, cela peut être fait en exécutant ces commandes à partir de la **racine de votre site Grav 1.7** :

```console
wget -q https://getgrav.org/download/core/grav-update/1.6.31 -O tmp/grav-update-v1.6.31.zip
wget -q https://getgrav.org/download/plugins/admin/1.9.19 -O tmp/grav-plugin-admin-v1.9.19.zip
unzip tmp/grav-update-v1.6.31.zip -d tmp
unzip tmp/grav-plugin-admin-v1.9.19.zip -d tmp
cp -rf tmp/getgrav-grav-plugin-admin-5d86394/* user/plugins/admin/
cp -rf tmp/grav-update/* ./
```

Fondamentalement, il effectue une **installation directe** de la dernière version de Grav 1.6 et Admin 1.9 en plus de votre installation actuelle. Il ne touche pas le dossier `user/` afin que votre contenu et vos plugins ne soient pas impactés.

Pour ceux qui n'ont pas accès à la CLI, téléchargez les fichiers [grav-update-v1.6.31.zip](https://github.com/getgrav/grav/releases/download/1.6.31/grav-update-v1.6.31.zip) et [grav-plugin-admin-1.9.19.zip](https://github.com/getgrav/grav-plugin-admin/archive/1.9.19.zip) en utilisant les liens donnés ici. Décompressez les fichiers dans votre système de fichiers. Utilisez ensuite votre client FTP/SFTP préféré pour copier tous les fichiers Grav dans vos fichiers `WEBROOT` et Admin dans `WEBROOT/user/plugins/admin`.

