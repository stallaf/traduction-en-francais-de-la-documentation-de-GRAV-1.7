<h1 class="rem">Tutoriel sur les plugins</h1>

Les plugins sont généralement développés car il existe une tâche qui ne peut pas être complétée avec les fonctionnalités de base de Grav.

Dans ce tutoriel, nous allons créer un plugin qui aide Grav à fournir une page aléatoire à l'utilisateur. Vous avez probablement vu une fonctionnalité similaire sur les sites de blog comme un moyen de fournir un article de blog aléatoire lorsque vous cliquez sur un bouton.

<div class="notice note">
Puisqu'il existe déjà un plugin qui effectue ce travail nommé <code>Random</code>, nous appellerons ce plugin de test <code>Randomizer</code>.
</div>

Cette fonctionnalité n'est pas possible **prête à l'emploi**, mais est **facilement** fournie via un plugin. Comme c'est le cas avec un grand nombre d'aspects de Grav, il n'y a pas de moyen unique de le faire. Au lieu de cela, vous avez de nombreuses options. Nous ne couvrirons qu'une seule approche...

<h2 id="Présentation du plug-in Randomizer">Présentation du plug-in Randomizer
<a href="#Présentation du plug-in Randomizer" class="toc-anchor after"></a></h2> 

Pour notre plugin, nous adopterons l'approche suivante :

1. Activez le plugin si un URI correspond à notre "route de déclenchement" configurée. (par exemple `/random`)

2. Créez un filtre afin que seules les taxonomies configurées soient dans le pool de pages aléatoires. (ex. `catégorie : blog`)

3. Trouvez une page au hasard dans notre pool filtré et dites à Grav de l'utiliser pour le contenu de la page.

OK ! Cela semble assez simple, non ? Alors, laissez-nous craquer!

<h3 id="Étape 1 - Installer le plug-in DevTools">Étape 1 - Installer le plug-in DevTools
<a href="#Étape 1 - Installer le plug-in DevTools" class="toc-anchor after"></a></h3> 

<div class="notice tip">
Les versions précédentes de ce didacticiel nécessitaient la création manuelle d'un plugin. Tout ce processus peut être ignoré grâce à notre <strong>nouveau plugin DevTools</strong>.
</div>

La première étape de la création d'un nouveau plugin consiste à **installer le plugin DevTools**. Ceci peut être fait de deux façons.

<h3 id="Installer via CLI GPM">Installer via CLI GPM
<a href="#Installer via CLI GPM" class="toc-anchor after"></a></h3> 

Naviguez dans la ligne de commande jusqu'à la racine de votre installation Grav

```console
$ | bin/gpm install devtools
```

<h4 id="Installer via le plugin Admin">Installer via le plugin Admin
<a href="#Installer via le plugin Admin" class="toc-anchor after"></a></h4> 

* Une fois connecté, accédez simplement à la section **Plugins** depuis la barre latérale.
* Cliquez sur le bouton <span class="fa fa-plus"></span><strong>Add</strong> en haut à droite.
* Recherchez **DevTools** dans la liste et cliquez sur le bouton <span class="fa fa-plus"></span> <strong>Install</strong>.

<h3 id="Étape 2 - Créer le plugin Randomizer">Étape 2 - Créer le plugin Randomizer
<a href="#Étape 2 - Créer le plugin Randomizer" class="toc-anchor after"></a></h3> 

Pour cette prochaine étape, vous devez vraiment être dans la [ligne de commande](cli-intro.md) car les DevTools fournissent quelques commandes CLI pour faciliter le processus de création d'un nouveau plugin !

Depuis la racine de votre installation Grav saisissez la commande suivante :

```console
$ | bin/plugin devtools new-plugin
```

Ce processus vous posera quelques questions nécessaires à la création du nouveau plugin :

```console
$ | bin/plugin devtools new-plugin
  | Enter Plugin Name: Randomizer
  | Enter Plugin Description: Sends the user to a random page
  | Enter Developer Name: Acme Corp
  | Enter Developer Email: contact@acme.co
  | 
  | SUCCESS plugin Randomizer -> Created Successfully
  |
  | Path: /www/user/plugins/randomizer
  | 
  | Make sure to run `composer update` to initialize the autoloader
```

<div class="notice note">
À ce stade, vous devez <strong>exécuter la mise à jour</strong> de <code>composer update</code> dans le dossier du plug-in nouvellement créé.
</div>

La commande DevTools vous indique où ce nouveau plugin a été créé. Ce plugin créé est entièrement fonctionnel mais n'aura pas automatiquement la logique pour exécuter la fonction que nous souhaitons. Nous devrons le modifier en fonction de nos besoins.

<h3 id="Étape 3 - Bases du plugin">Étape 3 - Bases du plugin
<a href="#Étape 3 - Bases du plugin" class="toc-anchor after"></a></h3> 

Nous avons maintenant créé un nouveau plugin qui peut être modifié et développé. Décomposons-le et regardons ce qui constitue un plugin. Si vous regardez dans le dossier `user/plugins/randomizer` vous verrez :

<pre>
<div class="pre">
.
├── CHANGELOG.md
├── LICENSE
├── README.md
├── blueprints.yaml
├── randomizer.php
</pre>
</div>

Ceci est un exemple de structure mais certaines choses sont requises :

<h4 id="Éléments requis pour fonctionner">Éléments requis pour fonctionner
<a href="#Éléments requis pour fonctionner" class="toc-anchor after"></a></h4>


Ces éléments sont essentiels et votre plugin ne fonctionnera pas de manière fiable à moins que vous ne les incluiez dans votre plugin.

* `blueprints.yaml` - Le fichier de configuration utilisé par Grav pour obtenir des informations sur votre plugin. Il peut également définir un formulaire que l'administrateur peut afficher lors de l'affichage des détails du plugin. Ce formulaire vous permettra d'enregistrer les paramètres du plugin. [Ce fichier est documenté dans le chapitre Formulaires](formulaires-blueprints.md).
* `randomizer.php` - Ce fichier sera nommé en fonction de votre plugin, mais peut être utilisé pour héberger toute logique dont votre plugin a besoin. Vous pouvez utiliser n'importe quel [crochet d'événement de plugin](crochets-evenements.md) pour exécuter la logique à peu près n'importe quel moment du [cycle de vie de Grav](cycle-vie.md).
* `randomizer.yaml` - Il s'agit de la configuration utilisée par le plugin pour définir les options que le plugin pourrait utiliser. Celui-ci doit être nommé de la même manière que le fichier `.php`.

<br>
<h4 id="Éléments requis pour la release">Éléments requis pour la release
<a href="#Éléments requis pour la release" class="toc-anchor after"></a></h4>

Ces éléments sont nécessaires si vous souhaitez publier votre plugin via GPM.

* `CHANGELOG.md` - Un fichier qui suit le [format Grav Changelog](avance-grav-developpement.md) pour montrer les changements dans les versions.
* `LICENCE` - un fichier de licence, devrait probablement être MIT sauf si vous avez un besoin spécifique pour autre chose.
* `README.md` - Un 'Readme' qui devrait contenir toute documentation pour le plugin. Comment l'installer, le configurer et l'utiliser.

<br>
<h3 id="Étape 4 - Configuration du plugin">Étape 4 - Configuration du plugin
<a href="#Étape 4 - Configuration du plugin" class="toc-anchor after"></a></h3>

Comme nous l'avons décrit dans la **présentation du plugin**, nous avons besoin de quelques options de configuration pour notre plugin, donc le fichier `randomizer.yaml` devrait ressembler à ceci :

```yaml
1 | enabled: true
2 | active: true
3 | route: /random
4 | filters:
5 |     category: blog
```

Cela nous permet d'avoir plusieurs filtres si nous le souhaitons, mais pour l'instant, nous voulons juste que tout le contenu avec la taxonomie `category: blog` soit éligible pour la sélection aléatoire.

Tous les plugins doivent avoir l'option `enabled`. Si cela est `false` dans la configuration à l'échelle du site, votre plugin ne sera jamais initialisé par Grav. Tous les plugins ont également l'option `active`. Si cela est faux dans la configuration à l'échelle du site, chaque page devra activer votre plugin. Notez que plusieurs plugins prennent également en charge `enabled/active` dans le frontmatter de la page en utilisant `mergeConfig`, détaillé ci-dessous.

<div class="notice warming">
L'installation par défaut de Grav a une taxonomie définie pour la <code>category</code> et le <code>tag</code> par défaut. Cette configuration peut être modifiée dans votre fichier <code>user/config/site.yaml</code>.
</div>

Bien sûr, comme pour toutes les autres configurations dans Grav, il est conseillé de ne pas toucher à cette configuration par défaut pour le contrôle au jour le jour. Au lieu de cela, vous devez créer un remplacement dans un fichier appelé `/user/config/plugins/randomizer.yaml` pour héberger tous les paramètres personnalisés. Ce plugin `randomizer.yaml` fourni est vraiment destiné à définir des valeurs par défaut raisonnables pour votre plugin.

<h3 id="Étape 5 - Structure du plugin de base">Étape 5 - Structure du plugin de base
<a href="#Étape 5 - Structure du plugin de base" class="toc-anchor after"></a></h3>

La structure de classe du plugin de base ressemblera déjà à ceci :

```php
1  | <?php
2  | namespace Grav\Plugin;
3  |
4  | use Composer\Autoload\ClassLoader;
5  | use Grav\Common\Plugin;
6  | use RocketTheme\Toolbox\Event\Event;
7  |
8  | /**
9  |  * Class RandomizerPlugin
10 |  * @package Grav\Plugin
11 |  */
12 | class RandomizerPlugin extends Plugin
13 | {
14 |     /**
15 |      * Composer autoload.
16 |      *
17 |      * @return ClassLoader
18 |      */
19 |     public function autoload(): ClassLoader
20 |     {
21 |         return require __DIR__ . '/vendor/autoload.php';
22 |     }
23 | }
```

Nous devons ajouter quelques instructions `use` car nous allons utiliser ces classes dans notre plugin, et cela économise de l'espace et rend le code plus lisible si nous n'avons pas à mettre l'espace de noms complet pour chaque classe en ligne.

Modifiez les instructions `use` pour qu'elles ressemblent à ceci :
 
```console
1 | use Composer\Autoload\ClassLoader;
2 | use Grav\Common\Plugin;
3 | use Grav\Common\Page\Collection;
4 | use Grav\Common\Uri;
5 | use Grav\Common\Taxonomy;
```

Les deux éléments clés de cette structure de classe sont :

1. Les plugins doivent avoir un `namespace Grav\Plugin` en haut du fichier PHP.
2. Les plugins doivent être nommés en **casse de titre** en fonction du nom du plugin avec la chaîne `Plugin` ajoutée à la fin, et doivent étendre `Plugin`, d'où le nom de classe `RandomizerPlugin`.

<br>
<h3 id="Étape 6 - Événements auxquels vous êtes abonnés">Étape 6 - Événements auxquels vous êtes abonnés
<a href="#Étape 6 - Événements auxquels vous êtes abonnés" class="toc-anchor after"></a></h3> 

Grav utilise un système d'événements sophistiqué, et pour assurer des performances optimales, tous les plugins sont inspectés par Grav pour déterminer à quels événements le plugin est abonné.

```php
1 | public static function getSubscribedEvents(): array
2 | {
3 |     return [
4 |         'onPluginsInitialized' => [
5 |             ['autoload', 100000], // TODO: Remove when plugin requires Grav >=1.7
6 |            ['onPluginsInitialized', 0]
7 |         ]
8 |     ];
9 |}
```

Dans ce plugin, nous allons dire à Grav que nous nous abonnons à l'événement `onPluginsInitialized`. De cette façon, nous pouvons utiliser cet événement (qui est le premier événement disponible pour les plugins) pour déterminer si nous devons nous abonner à d'autres événements.

<div class="notice tip">
<strong>Remarque</strong> : Le premier écouteur d'événement de chargement <code>autoload</code> n'est nécessaire que dans Grav 1.6. Grav 1.7 appelle automatiquement la méthode.
</div>

<h3 id="Étape 7 - Déterminez si le plugin doit s'exécuter">Étape 7 - Déterminez si le plugin doit s'exécuter
<a href="#Étape 7 - Déterminez si le plugin doit s'exécuter" class="toc-anchor after"></a></h3>

L'étape suivante consiste à ajouter une méthode à notre classe `RandomizerPlugin` pour gérer l'événement `onPluginsInitialized` afin qu'il ne s'active que lorsqu'un utilisateur essaie d'accéder à la route que nous avons configurée dans notre fichier `randomizer.yaml`. Remplacez la logique actuelle du plug-in "exemple" par ce qui suit :

```php
1  | public function onPluginsInitialized(): void
2  | {
3  |     // Don't proceed if we are in the admin plugin
4  |     if ($this->isAdmin()) {
5  |         return;
6  |     }
7  | 
8  |     /** @var Uri $uri */
9  |     $uri = $this->grav['uri'];
10 |     $config = $this->config();
11 |
12 |     $route = $config['route'] ?? null;
13 |     if ($route && $route == $uri->path()) {
14 |         $this->enable([
15 |             'onPageInitialized' => ['onPageInitialized', 0]
16 |         ]);
17 |     }
18 | }
```

Tout d'abord, nous obtenons l'**objet Uri** du **conteneur d'injection de dépendance**. Celui-ci contient toutes les informations sur l'URI actuel, y compris les informations de route.

La **méthode config()** fait déjà partie du **Plugin** de base, nous pouvons donc simplement l'utiliser pour obtenir la valeur de configuration de notre `route` configurée.

Ensuite, nous comparons la route configurée au chemin URI actuel. S'ils sont égaux, nous indiquons au répartiteur que notre plugin écoutera également un nouvel événement : `onPageInitialized`.

En utilisant cette approche, nous nous assurons de ne pas exécuter de code supplémentaire si nous n'en avons pas besoin. Des pratiques comme celles-ci garantiront que votre site fonctionne aussi rapidement que possible.

<h3 id="Étape 8 - Afficher la page aléatoire">Étape 8 - Afficher la page aléatoire
<a href="#Étape 8 - Afficher la page aléatoire" class="toc-anchor after"></a></h3>

La dernière étape de notre plugin consiste à afficher la page au hasard, et nous pouvons le faire en ajoutant la méthode suivante :

```php
1  | /**
2  |  * Send user to a random page
3  |  */
4  | public function onPageInitialized(): void
5  | {
6  |     /** @var Taxonomy $uri */
7  |     $taxonomy_map = $this->grav['taxonomy'];
8  |     $config = $this->config();
9  |
10 |     $filters = (array)($config['filters'] ?? []);
11 |     $operator = $config['filter_combinator'] ?? 'and';
12 | 
13 |     if (count($filters) > 0) {
14 |         $collection = new Collection();
15 |         $collection->append($taxonomy_map->findTaxonomy($filters, $operator)->toArray());
16 |         if (count($collection) > 0) {
17 |             unset($this->grav['page']);
18 |             $this->grav['page'] = $collection->random()->current();
19 |         }
20 |     }
21 | }
```

Cette méthode est un peu plus compliquée, nous allons donc passer en revue ce qui se passe :

1. Tout d'abord, nous récupérons l'**objet Taxonomy** du **conteneur Grav DI** et l'affectons à une variable `$taxonomy_map`.

2. Ensuite, nous récupérons le tableau de filtres de notre configuration de plugin. Dans notre configuration, il s'agit d'un tableau avec 1 élément : ['category' => 'blog'].

3. Vérifiez que nous avons des filtres, puis créez une nouvelle `Collection` dans la variable `$collection` pour stocker nos pages.

4. Ajoutez toutes les pages qui correspondent au filtre à notre variable `$collection`.

5. Annulez la définition de l'objet de la `page` actuelle dont Grav a connaissance.

6. Définissez la `page` actuelle sur un élément aléatoire de la collection.

<h3 id="Étape 9 - Nettoyage">Étape 9 - Nettoyage
<a href="#Étape 9 - Nettoyage" class="toc-anchor after"></a></h3>

L'exemple de plugin créé avec le plugin **DevTool**s utilisait un événement appelé `onPageContentRaw()`. Cet événement n'est pas utilisé dans notre nouveau plugin, nous pouvons donc supprimer la fonction entière en toute sécurité.

<h3 id="Étape 10 - Classe finale du plugin">Étape 10 - Classe finale du plugin
<a href="#Étape 10 - Classe finale du plugin" class="toc-anchor after"></a></h3>

Et c'est tout ce qu'il y a à faire ! Le plugin est maintenant terminé. Votre classe de plugin complète devrait ressembler à ceci :

```php
1  | <?php
2  | namespace Grav\Plugin;
3  | 
4  | use Composer\Autoload\ClassLoader;
5  | use Grav\Common\Plugin;
6  | use Grav\Common\Page\Collection;
7  | use Grav\Common\Uri;
8  | use Grav\Common\Taxonomy;
9  | 
10 | /**
11 |  * Class RandomizerPlugin
12 |  * @package Grav\Plugin
13 |  */
14 | class RandomizerPlugin extends Plugin
15 | {
16 |     /**
17 |      * @return array
18 |      *
19 |      * The getSubscribedEvents() gives the core a list of events
20 |      *     that the plugin wants to listen to. The key of each
21 |      *     array section is the event that the plugin listens to
22 |      *     and the value (in the form of an array) contains the
23 |      *     callable (or function) as well as the priority. The
24 |      *     higher the number the higher the priority.
25 |      */
26 |     public static function getSubscribedEvents(): array
27 |     {
28 |     return [
29 |        'onPluginsInitialized' => [
30 |             ['autoload', 100000], // TODO: Remove when plugin requires Grav >=1.7
31 |             ['onPluginsInitialized', 0]
32 |         ]
33 |     ];
34 |     }
35 | 
36 |     /**
37 |      * Composer autoload.
38 |      *
40 |      * @return ClassLoader
41 |      */
42 |     public function autoload(): ClassLoader
43 |     {
44 |         return require __DIR__ . '/vendor/autoload.php';
45 |     }
46 |     public function onPluginsInitialized(): void
47 |     {
48 |         // Don't proceed if we are in the admin plugin
49 |         if ($this->isAdmin()) {
50 |             return;
51 |         }
52 | 
53 |         /** @var Uri $uri */
54 |         $uri = $this->grav['uri'];
55 |         $config = $this->config();
56 | 
57 |         $route = $config['route'] ?? null;
58 |         if ($route && $route == $uri->path()) {
59 |             $this->enable([
60 |                 'onPageInitialized' => ['onPageInitialized', 0]
61 |             ]);
62 |         }
63 |     }
64 | 
65 |     /**
66 |      * Send user to a random page
67 |      */
68 |     public function onPageInitialized(): void
69 |     {
70 |         /** @var Taxonomy $uri */
71 |         $taxonomy_map = $this->grav['taxonomy'];
72 |         $config = $this->config();
73 | 
74 |         $filters = (array)($config['filters'] ?? []);
75 |         $operator = $config['filter_combinator'] ?? 'and';
76 | 
77 |         if (count($filters) > 0) {
78 |             $collection = new Collection();
79 |             $collection->append($taxonomy_map->findTaxonomy($filters, $operator)->toArray());
80 |             if (count($collection) > 0) {
81 |                 unset($this->grav['page']);
82 |                 $this->grav['page'] = $collection->random()->current();
83 |             }
84 |         }
85 |     }
86 | }
```

Si vous avez suivi, vous devriez avoir un plugin **Randomizer** entièrement fonctionnel activé pour votre site. Pointez simplement votre navigateur sur `http://votresite.com/random`, et vous devriez voir une page au hasard. Vous pouvez également télécharger le plugin **Random** original directement depuis la section [Plugins Download](https://getgrav.org/downloads/plugins) du site [getgrav.org](https://getgrav.org/).

<h2 id="Fusion du plugin et de la configuration de la page">Fusion du plugin et de la configuration de la page
<a href="#Fusion du plugin et de la configuration de la page" class="toc-anchor after"></a></h2>

Une technique populaire utilisée dans une variété de plugins est le concept de fusion de la configuration du plugin (configuration utilisateur par défaut ou remplacée) avec la configuration au niveau de la page. Cela signifie que vous pouvez définir une configuration **à l'échelle du site**, puis disposer d'une configuration spécifique pour une page donnée selon vos besoins. Cela offre beaucoup de puissance et de flexibilité à votre plugin.

Dans les versions récentes de Grav, une méthode d'assistance a été ajoutée pour exécuter cette fonctionnalité automatiquement plutôt que de devoir coder cette logique vous-même. Le plugin **SmartyPants** fournit un bon exemple de cette fonctionnalité en action :

```php
1  | public function onPageContentProcessed(Event $event): void
2  | {
3  |     $page = $event['page'];
4  |     $config = $this->mergeConfig($page);
5  | 
6  |     if ($config->get('process_content')) {
7  |         $page->setRawContent(\Michelf\SmartyPants::defaultTransform(
8  |             $page->getRawContent(),
9  |             $config->get('options')
10 |         ));
11 |     }
12 | }
```

<h2 id="Implémentation de la CLI dans votre plugin">Implémentation de la CLI dans votre plugin
<a href="#Implémentation de la CLI dans votre plugin" class="toc-anchor after"></a></h2>

Les plugins ont également la capacité de s'intégrer à la ligne de commande `bin/plugin` pour exécuter des tâches. Vous pouvez suivre la documentation CLI du plug-in si vous souhaitez implémenter une telle fonctionnalité.

