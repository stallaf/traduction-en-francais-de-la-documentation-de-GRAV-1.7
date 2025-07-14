<h1 class="rem">Recettes de plugins</h1>

Cette page contient un assortiment de problèmes et leurs solutions respectives liées aux plugins Grav.

<h2 id="Générer du code PHP dans un modèle Twig">Générer du code PHP dans un modèle Twig.
<a href="#Générer du code PHP dans un modèle Twig" class="toc-anchor after"></a></h2>

<h3 id="Objectif-1">Objectif :
<a href="#Objectif-1" class="toc-anchor after"></a></h3>

Vous souhaitez traiter du code PHP personnalisé et rendre le résultat disponible dans une page.

<h3 id="La solution-1 :">La solution :
<a href="#La solution-1 :" class="toc-anchor after"></a></h3>

Vous créez un nouveau plugin qui crée une extension Twig et met à disposition du contenu PHP dans vos templates Twig.

Créez un nouveau dossier de plugins dans `user/plugins/example` et ajoutez ces fichiers :

`user/plugins/example/example.php` `user/plugins/example/example.yaml`
`user/plugins/example/twig/ExampleTwigExtension.php`

Dans `twig/ExampleTwigExtension.php`, vous effectuerez votre traitement personnalisé et le renverrez sous forme de chaîne dans `exampleFunction()`.

Ensuite, dans votre fichier de modèle Twig (ou dans un fichier Markdown de page si vous avez activé le traitement Twig dans Pages), affichez la sortie en utilisant : `{{ example() }}`.

La vue d'ensemble est terminée, voyons le code réel :

`exemple.php` :

```php
1  | <?php
2  | namespace Grav\Plugin;
3  | use \Grav\Common\Plugin;
4  | class ExamplePlugin extends Plugin
5  | {
6  |     public static function getSubscribedEvents()
7  |     {
8  |        return [
9  |             'onTwigExtensions' => ['onTwigExtensions', 0]
10 |         ];
11 |     }
12 |     public function onTwigExtensions()
13 |     {
14 |         require_once(__DIR__ . '/twig/ExampleTwigExtension.php');
15 |         $this->grav['twig']->twig->addExtension(new ExampleTwigExtension());
16 |     }
17 | }
```

`ExempleTwigExtension.php` :

```php
1  | <?php
2  | namespace Grav\Plugin;
3  | use Grav\Common\Twig\Extension\GravExtension;
4  |
5  | class ExampleTwigExtension extends GravExtension
6  | {
7  |     public function getName()
8  |     {
9  |         return 'ExampleTwigExtension';
10 |     }
11 |     public function getFunctions(): array
12 |     {
13 |         return [
14 |             new \Twig_SimpleFunction('example', [$this, 'exampleFunction'])
15 |         ];
16 |     }
17 |     public function exampleFunction()
18 |     {
19 |         return 'something';
20 |     }
21 | }
```

`exemple.yaml` :

    enabled: true

Le plugin est maintenant installé et activé, et tout devrait fonctionner.

<h2 id="Filtrer les taxonomies à l'aide du plugin taxonomylist">Filtrer les taxonomies à l'aide du plugin taxonomylist.
<a href="#Filtrer les taxonomies à l'aide du plugin taxonomylist" class="toc-anchor after"></a></h2>

<h3 id="Objectif-2">Objectif :
<a href="#Objectif-2" class="toc-anchor after"></a></h3>

Vous souhaitez utiliser le [plugin Grav de liste de taxonomie](https://github.com/getgrav/grav-plugin-taxonomylist) pour répertorier les balises utilisées dans vos articles de blog, mais au lieu de les lister toutes, vous souhaitez uniquement répertorier les éléments les plus utilisés dans une taxonomie donnée (comme les cinq premières balises, par exemple).

<h3 id="La solution-2 :">La solution :
<a href="#La solution-2 :" class="toc-anchor after"></a></h3>

C'est un exemple où la flexibilité des plugins Grav est vraiment pratique. La première étape consiste à vous assurer que le [plugin Grav de la liste de taxonomie](https://github.com/getgrav/grav-plugin-taxonomylist) est installé dans votre package Grav. Après l'installation, assurez-vous de copier `/yoursite/user/plugins/taxonomylist/templates/partials/taxonomylist.html.twig` vers `/yoursite/user/themes/yourtheme/templates/partials/taxonomylist.html.twig` car nous apportera des modifications à ce fichier.

Pour que cela fonctionne, nous allons introduire trois nouvelles variables : `filter`, `filterstart` et `filterend` où

* **filter** est un booléen, qui sera défini sur `true` si nous voulons pouvoir répertorier uniquement les principales balises (ou toute autre taxonomie que vous souhaitez utiliser).
* **filterstart** est un entier arbitraire, mais doit généralement être défini sur zéro. Il s'agit de l'index du tableau de taxonomie par lequel vous souhaitez commencer.
* **filterend** est un entier arbitraire et est l'index dans le tableau de taxonomie auquel vous voulez vous terminer. Notez que si vous souhaitez répertorier les cinq premiers éléments de votre taxonomie, vous devez le définir sur 5 car notre boucle itérera jusqu'à `filterend -1`.

La prochaine étape consistera à appeler `taxonomylist.html.twig` dans le modèle dans lequel nous souhaitons répertorier les principaux éléments de notre taxonomie. Comme d'habitude, nous le ferons en utilisant `{% include %}` comme le montre l'exemple d'extrait de code suivant :

```html
1 | {% if config.plugins.taxonomylist.enabled %}
2 | <div class="sidebar-content">
3 |     <h4>Popular Tags</h4>
4 |     {% include 'partials/taxonomylist.html.twig' with {'taxonomy':'tag', filter: true, filterstart: 0, filterend: 5} %}
5 | </div>
6 | {% endif %}
```

Dans cet exemple, nous allons lister les cinq premières balises.

Maintenant, tournons notre attention vers `taxonomylist.html.twig`. Pour référence, voici le code par défaut de ce fichier lors de son installation initiale :

```html
1 | {% set taxlist = taxonomylist.get() %}
2 | 
3 | {% if taxlist %}
4 |     <span class="tags">
5 |         {% for tax,value in taxlist[taxonomy] %}
6 |             <a href="{{ base_url }}/{{ taxonomy }}{{ config.system.param_sep }}{{ tax|e('url') }}">{{ tax }}</a>
7 |         {% endfor %}
8 |     </span>
9 | {% endif %}
```

Pour que cela fonctionne avec nos nouvelles variables (c'est-à-dire `filter`, `filterstart` et `filterend`), nous devrons les inclure dans ce fichier comme suit :

```html
1  | {% set taxlist = taxonomylist.get %}
2  | 
3  | {% if taxlist %}
4  |     {% set taxlist_taxonomy = taxlist[taxonomy] %}
5  | 
6  |     {% if filter %}
7  |         {% set taxlist_taxonomy = taxlist_taxonomy|slice(filterstart,filterend) %}
8  |     {% endif %}
9  | 
10 |     <span class="tags">
11 |         {% for tax,value in taxlist_taxonomy %}
12 |             <a href="{{ base_url }}/{{ taxonomy }}{{ config.system.param_sep }}{{ tax|e('url') }}">{{ tax }}</a>
13 |         {% endfor %}
14 |     </span>
15 | {% endif %}
```

Ici, le fichier rassemble tous les éléments de la taxonomie par défaut, dans une variable appelée `taxlist_taxonomy`.

Si `filter` a été défini, la taxonomie utilise le filtre `slice` de Twig. Ce filtre va, dans notre cas, extraire un sous-ensemble d'un tableau depuis l'index de début (dans notre cas, `filterstart`) jusqu'à l'index de fin (dans notre cas, `filterend`).

La boucle for est exécutée comme dans la `taxonomylist.html.twig` d'origine avec le contenu de `taxlist_taxonomy,` filtré ou non.

<h2 id="Ajout d'un bouton de recherche au plugin SimpleSearch">Ajout d'un bouton de recherche au plugin SimpleSearch.
<a href="#Ajout d'un bouton de recherche au plugin SimpleSearch" class="toc-anchor after"></a></h2>

<h3 id="Objectif-3">Objectif :
<a href="#Objectif-3" class="toc-anchor after"></a></h3>

Vous aimez vraiment le [plugin Grav SimpleSearch](https://github.com/getgrav/grav-plugin-simplesearch), mais vous souhaitez ajouter un bouton de recherche en plus du champ de texte. L'une des raisons d'ajouter ce bouton est qu'il peut ne pas être évident pour l'utilisateur qu'il doit appuyer sur la touche `Enter` pour lancer sa demande de recherche.

<h3 id="La solution-3 :">La solution :
<a href="#La solution-3 :" class="toc-anchor after"></a></h3>

Tout d'abord, assurez-vous que vous avez installé le [plugin Grav SimpleSearch](V). Ensuite, assurez-vous de copier `/yoursite/user/plugins/simplesearch/templates/partials/simplesearch-searchbox.html.twig` vers `/yoursite/user/themes/yourtheme/templates/partials/simplesearch-searchbox.html.twig` car nous devra apporter des modifications à ce fichier.

Avant d'aller plus loin, examinons ce que fait ce fichier :

```html
1  | <input type="text" placeholder="Search..." value="{{ query }}" data-search-input="{{ base_url }}{{ config.plugins.simplesearch.route}}/query" />
2  | <script>
3  | jQuery(document).ready(function($){
4  |     var input = $('[data-search-input]');
5  |   input.on('keypress', function(event) {
6  |         if (event.which == 13 && input.val().length > 3) {
7  |             event.preventDefault();
8  |             window.location.href = input.data('search-input') + '{{ config.system.param_sep }}' + input.val();
9  |         }
10 |     });
11 | });
12 | </script>
```

La première ligne intègre simplement un champ de saisie de texte dans votre modèle Twig. L'attribut `data-search-input` stocke l'URL de base de la page de requête résultante. La valeur par défaut est `http://votresite/recherche/requête`.

Passons maintenant au jQuery en dessous. Ici, la balise contenant l'attribut `data-search-input` est affectée à une entrée variable. Ensuite, la méthode `jQuery .on()` est appliquée à l'entrée. La méthode `.on()` applique des gestionnaires d'événements aux éléments sélectionnés (dans ce cas, le champ de texte `<input>`). Ainsi, lorsque l'utilisateur appuie (`keypress`) sur une touche pour lancer la recherche, l'instruction `if` vérifie que les éléments suivants sont `true` :

1. La touche `Enter` a été enfoncée : `event.which == 13` où 13 est la valeur numérique de la touche `Enter` du clavier.
2. Le nombre de caractères saisis dans la zone de recherche est supérieur à trois. Vous voudrez peut-être ajuster cela à votre goût, car votre organisation peut avoir de nombreux acronymes de trois caractères ou moins.

S'ils sont vrais, alors `event.preventDefault()`; s'assure que l'action par défaut du navigateur pour la touche `Enter` est ignorée car cela empêcherait notre recherche de se produire. Enfin, l'URL complète de la requête de recherche est construite. La valeur par défaut est `http://votresite/recherche/requête:votrerequête`. À partir de là, `/yoursite/user/plugins/simplesearch/simplesearch.php` effectue la recherche proprement dite et les autres fichiers Twig du plugin répertorient les résultats.

Pas de retour à notre solution ! Si nous souhaitons ajouter un bouton de recherche, nous devons :

1. Ajouter le bouton
2. Assurez-vous d'appliquer la méthode `.on()` au bouton, mais cette fois, en utilisant `click` au lieu de `keypress`

Ceci est réalisé avec le code suivant utilisant le [Turret CSS Framework](https://bigfishtv.github.io/turret/docs/index.html). Des extraits de code pour d'autres frameworks seront listés à la fin.

```html
1  | <div class="input-group input-group-search">
2  |     <input type="search" placeholder="Search" value="{{ query }}" data-search-input="{{ base_url }}{{ config.plugins.simplesearch.route}}/query" >
3  |     <span class="input-group-button">
4  |         <button class="button" type="submit">Search</button>
5  |     </span>
6  |</div>
7  |
8  | <script>
9  | jQuery(document).ready(function($){
10 |     var input = $('[data-search-input]');
11 |     var searchButton = $('.button.search');
12 |
13 |     input.on('keypress', function(event) {
14 |         if (event.which == 13 && input.val().length > 3) {
15 |             event.preventDefault();
16 |             window.location.href = input.data('search-input') + '{{ config.system.param_sep }}' + input.val();
17 |         }
18 |     });
19 |
20 |     searchButton.on('click', function(event) {
21 |         if (input.val().length > 3) {
22 |             event.preventDefault();
23 |             window.location.href = input.data('search-input') + '{{ config.system.param_sep }}' + input.val();
24 |         }
25 |     });
26 | });
27 | </script>
```

Les attributs HTML et class sont spécifiques à Turret, mais le résultat final [ressemblera à ceci](https://bigfishtv.github.io/turret/docs/index.html#input-group). Nous pouvons également voir que la méthode `.on()` a également été affectée au bouton de recherche, mais elle vérifie uniquement que le nombre de caractères saisis dans la zone de recherche est supérieur à trois avant d'exécuter le code dans l'instruction `if`.

Voici le code HTML par défaut pour le champ de texte plus un bouton de recherche pour quelques autres frameworks :

[Bootstrap](https://getbootstrap.com/)

```html
1 | <div class="input-group">
2 |     <input type="text" class="form-control" placeholder="Search for...">
3 |     <span class="input-group-btn">
4 |         <button class="btn btn-default" type="button">Go!</button>
5 |    </span>
6 |</div>
```

[Materialize](http://materializecss.com/)

```html
1 | <div class="champ d'entrée">
2 |      <input id="search" type="search" requis>
3 |      <label for="search"><i class="material-icons">rechercher</i></label>
4 | </div>
```

[Pure CSS](http://purecss.io/)

```html
1 | <form class="pure-form">
2 |      <input type="text" class="pure-input-rounded">
3 |      <button type="submit" class="pure-button">Rechercher</button>
4 | </form>
```

[Semantic UI](http://semantic-ui.com/)

```html
1 | <div class="entrée d'action ui">
2 |    <input type="text" placeholder="Rechercher...">
3 |    <button class="ui button">Rechercher</button>
4 | </div>
```

<h2 id="Itérer à travers les pages et les médias">Itérer à travers les pages et les médias.
<a href="#Itérer à travers les pages et les médias" class="toc-anchor after"></a></h2>

<h3 id="Objectif-4">Objectif :
<a href="#Objectif-4" class="toc-anchor after"></a></h3>

Vous souhaitez accéder à toutes les pages et aux médias associés à chaque page via PHP et/ou Twig, afin qu'ils puissent être bouclés ou autrement manipulés par le plugin.

<h3 id="La solution-4 :">La solution :
<a href="#La solution-4 :" class="toc-anchor after"></a></h3>

Utilisez les capacités de collection de Grav pour construire un index récursif de toutes les pages, et lors de l'indexation, rassemblez également les fichiers multimédias pour chaque page. Le plugin [DirectoryListing](https://github.com/OleVik/grav-plugin-directorylisting/blob/v2.0.0-rc.2/Utilities.php#L64-L105) fait exactement cela et construit une liste HTML en utilisant l'arborescence produite. Pour ce faire, nous allons créer une fonction récursive - ou une méthode, comme cela peut être le cas dans la classe d'un plugin - qui parcourt chaque page et la stocke dans un tableau. La méthode est récursive, car elle s'appelle à nouveau pour chaque page qu'elle trouve qui a des enfants.

Tout d'abord, cependant, la méthode prend trois paramètres : le premier est le `$route` vers la page, qui indique à Grav où le trouver ; le second est le `$mode`, qui indique à la méthode s'il faut parcourir la page elle-même ou ses enfants ; le troisième est le `$depth`, qui garde une trace du niveau de la page. La méthode instancie initialement l'objet Page, puis traite de la profondeur et du mode, et construit la collection. Par défaut, nous classons les pages par Date, Décroissant, mais vous pouvez rendre cela configurable. Ensuite, nous construisons un tableau, `$paths`, pour contenir chaque page. Comme les routes sont uniques dans Grav, elles sont utilisées comme clés dans ce tableau pour identifier chaque page.

Maintenant, nous parcourons les pages, en ajoutant la profondeur, le titre et l'itinéraire (également conservé comme valeur pour la facilité d'accès). Dans la boucle foreach, nous essayons également de récupérer les pages enfants et de les ajouter si elles sont trouvées. Aussi, nous trouvons tous les médias associés à la page, et les ajoutons. Étant donné que la méthode est récursive, elle continuera à rechercher des pages et des pages enfants jusqu'à ce qu'il n'en trouve plus.

Les données renvoyées sont une structure arborescente, ou un tableau multidimensionnel dans le langage PHP, contenant toutes les pages et leurs médias. Cela peut être transmis à Twig ou utilisé dans le plugin lui-même. Notez qu'avec de très grandes structures de dossiers, PHP peut expirer ou échouer en raison des limites de récursivité, par exemple. dossiers de 100 niveaux ou plus.

```php
1  | /**
2  |  * Creates page-structure recursively
3  |  * @param string $route Route to page
4  |  * @param integer $depth Reserved placeholder for recursion depth
5  |  * @return array Page-structure with children and media
6  |  */
7  | public function buildTree($route, $mode = false, $depth = 0)
8  | {
9  |     $page = Grav::instance()['page'];
10 |     $depth++;
11 |     $mode = '@page.self';
12 |     if ($depth > 1) {
13 |         $mode = '@page.children';
14 |     }
15 |     $pages = $page->evaluate([$mode => $route]);
16 |     $pages = $pages->published()->order('date', 'desc');
17 |     $paths = array();
18 |     foreach ($pages as $page) {
19 |         $route = $page->rawRoute();
20 |         $path = $page->path();
21 |         $title = $page->title();
22 |         $paths[$route]['depth'] = $depth;
23 |         $paths[$route]['title'] = $title;
24 |         $paths[$route]['route'] = $route;
25 |         if (!empty($paths[$route])) {
26 |             $children = $this->buildTree($route, $mode, $depth);
27 |             if (!empty($children)) {
28 |                 $paths[$route]['children'] = $children;
29 |             }
30 |         }
31 |         $media = new Media($path);
32 |         foreach ($media->all() as $filename => $file) {
33 |             $paths[$route]['media'][$filename] = $file->items()['type'];
34 |         }
35 |     }
36 |     if (!empty($paths)) {
37 |         return $paths;
38 |     } else {
39 |         return null;
40 |     }
41 | }
```

<h2 id="Plugin de modèles Twig personnalisés">Plugin de modèles Twig personnalisés.
<a href="#Plugin de modèles Twig personnalisés" class="toc-anchor after"></a></h2>

<h3 id="Objectif-5">Objectif :
<a href="#Objectif-5" class="toc-anchor after"></a></h3>

Plutôt que d'utiliser l'héritage de thème, il est possible de créer un plugin très simple qui vous permet d'utiliser un emplacement personnalisé pour fournir des modèles Twig personnalisés.

<h3 id="La solution-5 :">La solution :
<a href="#La solution-5 :" class="toc-anchor after"></a></h3>

La seule chose dont vous avez besoin dans ce plugin est un événement pour fournir un emplacement pour vos modèles. La façon la plus simple de créer le plugin est d'utiliser le plugin `devtools`. 

Alors installez ça avec :

    $ | $ bin/gpm install devtools

Une fois installé, créez un nouveau plugin avec la commande :

    $ | $ bin/plugin devtools newplugin

Remplissez les détails pour le nom, l'auteur, etc. Disons que nous l'appelons `Custom Templates`, et le plugin sera créé dans `/user/plugins/custom-templates`. Il ne vous reste plus qu'à éditer le fichier `custom-templates.php` et y mettre ce code :

```php
1  | <?php
2  | namespace Grav\Plugin;
3  |
4  | use \Grav\Common\Plugin;
5  |
6  | class CustomTemplatesPlugin extends Plugin
7  | {
8  |     /**
9  |      * Subscribe to required events
10 |      * 
11 |      * @return array
12 |      */
13 |     public static function getSubscribedEvents()
14 |     {
15 |         return [
16 |             'onTwigTemplatePaths' => ['onTwigTemplatePaths', 0]
17 |         ];
18 |     }
19 | 
20 |     /**
21 |      * Add current directory to twig lookup paths.
22 |      */
23 |     public function onTwigTemplatePaths()
24 |     {
25 |         $this->grav['twig']->twig_paths[] = __DIR__ . '/templates';
26 |     }
27 | }
```

Ce plugin s'abonne simplement à l'événement `onTwigTemplatePaths()`, puis dans cette méthode d'événement, il ajoute le dossier `user/plugins/custom-templates/templates` à celui des chemins que Twig vérifiera.

Cela vous permet de déposer un modèle Twig appelé `foo.html.twig`, puis toute page appelée `foo.md` pourra utiliser ce modèle.

<div class = "notice note">
REMARQUE : Cela ajoutera le chemin du modèle personnalisé du plug-in à la fin du tableau de chemin du modèle Twig. Cela signifie que le thème (qui est toujours le premier) aura priorité sur les modèles du plugin du même nom. Pour résoudre ce problème, placez simplement le chemin du modèle du plug-in devant le tableau en modifiant la méthode de l'événement :
</div>

```php
1 | /**
2 |      * Add current directory to twig lookup paths.
3 |      */
4 |     public function onTwigTemplatePaths()
5 |     {
6 |         array_unshift($this->grav['twig']->twig_paths, __DIR__ . '/templates');
7 |     }
```

<h2 id="Utiliser Cache dans vos propres plugins">Utiliser Cache dans vos propres plugins.
<a href="#Utiliser Cache dans vos propres plugins" class="toc-anchor after"></a></h2>

<h3 id="Objectif-6">Objectif :
<a href="#Objectif-6" class="toc-anchor after"></a></h3>

Lors du développement de vos propres plugins, il est souvent utile d'utiliser le cache de Grav pour mettre en cache les données afin d'améliorer les performances. Heureusement, c'est un processus très simple pour utiliser le cache dans votre propre code.

<h3 id="La solution-6 :">La solution :
<a href="#La solution-6 :" class="toc-anchor after"></a></h3>

Voici un code de base qui vous montre comment fonctionne la mise en cache :

```php
1  | $cache = Grav::instance()['cache'];
2  |     $id = 'myplugin-data'
3  |     $list = [];
4  | 
5  |     if ($data = $cache->fetch($id)) {
6  |         return $data;
7  |     } else {
8  |         $data = $this->gatherData();
9  |         $cache->save($hash, $data);
10 |         return $data;
11 |     }
```

Tout d'abord, nous obtenons l'objet cache de Grav, puis nous essayons de voir si nos données existent déjà dans le cache `($data = $cache->fetch($id))`. Si $data existe, renvoyez-le simplement sans travail supplémentaire nécessaire.

Cependant, si la récupération du cache renvoie null, ce qui signifie qu'il n'est pas mis en cache, faites un peu de travail et récupérez les données (`$data = $this->gatherData()`), puis enregistrez simplement les données pour la prochaine fois (`$cache->save( $hachage, $data)`).

<h2 id="Apprendre par l'exemple">Apprendre par l'exemple.
<a href="#Apprendre par l'exemple" class="toc-anchor after"></a></h2>

Avec l'abondance de plugins actuellement disponibles, il y a de fortes chances que vous trouviez vos réponses quelque part dans leur code source. Le problème est de savoir lesquels regarder. Cette page tente de répertorier les problèmes de plugin courants, puis répertorie les plugins spécifiques qui montrent comment les résoudre.

Avant de continuer, assurez-vous de vous être familiarisé avec [la documentation de base](/plugin-bases), en particulier le [Grav Lifecycle](/cycle-vie) !

<h2 id="Comment lire et écrire des données dans le système de fichiers ?">Comment lire et écrire des données dans le système de fichiers ?
<a href="#Comment lire et écrire des données dans le système de fichiers ?" class="toc-anchor after"></a></h2>

Grav peut être un fichier plat, mais un fichier plat ≠ statique ! Il existe de nombreuses façons de lire et d'écrire des données dans le système de fichiers.

* Si vous avez juste besoin d'un accès en lecture aux données YAML, consultez le [plugin d'importation](https://github.com/Deester4x4jr/grav-plugin-import).

* interface préférée est via l'interface intégrée <span class = "ancre">RocketTheme\Toolbox\File</span>.

* Rien ne vous empêche non plus d'utiliser [SQLite](https://sqlite.org/).

* L'exemple le plus simple est probablement le plugin [Comments](https://github.com/getgrav/grav-plugin-comments).

* D'autres incluent

    * [Table Imporer](https://github.com/Perlkonig/grav-plugin-table-importer)

    * [Thumb Ratings](https://github.com/iusvar/grav-plugin-thumb-ratings)

    * [Webmention](https://github.com/Perlkonig/grav-plugin-webmention)

<h2 id="Comment rendre les données d'un plugin disponibles pour Twig ?">Comment rendre les données d'un plugin disponibles pour Twig ?
<a href="#Comment rendre les données d'un plugin disponibles pour Twig ?" class="toc-anchor after"></a></h2>

Une façon est via l'espace de noms `config.plugins.X`. Faites simplement un `$this->config->set()` comme on le voit dans les exemples suivants :

* [ipLocate](https://github.com/Perlkonig/grav-plugin-iplocate/blob/master/iplocate.php#L82)

* [Count Views](https://github.com/Perlkonig/grav-plugin-count-views/blob/master/count-views.php#L88)

Vous pouvez ensuite y accéder dans un modèle Twig via `{{ config.plugins.X.whatever.variable }}`.

Alternativement, vous pouvez passer des variables via `grav['twig']` :

* [Blogroll](https://github.com/Perlkonig/grav-plugin-blogroll/blob/master/blogroll.php#L43), auquel vous pouvez ensuite accéder directement [dans votre modèle](https://github.com/Perlkonig/grav-plugin-blogroll/blob/master/templates/partials/blogroll.html.twig#L32).

Enfin, vous pouvez injecter des données directement dans l'en-tête de la page, comme on le voit dans le [plugin Import](https://github.com/Deester4x4jr/grav-plugin-import).

<h2 id="Comment injecter Markdown dans une page ?">Comment injecter Markdown dans une page ?
<a href="#Comment injecter Markdown dans une page ?" class="toc-anchor after"></a></h2>

Selon [Grav Lifecycle](/cycle-vie), le dernier hook d'événement où vous pouvez injecter du Markdown brut est `onPageContentRaw`. Le plus ancien est probablement `onPageInitialized`. Vous pouvez simplement saisir `$this->grav['page']->rawMarkdown()`, seulement çà, puis le réécrire avec`$this->grav['page']->setRawContent()`. Les plugins suivants le démontrent :

* [Page Inject](https://github.com/getgrav/grav-plugin-page-inject)

* [Table Importer](https://github.com/Perlkonig/grav-plugin-table-importer)

<h2 id="Comment puis-je injecter du HTML dans la sortie finale ?">Comment puis-je injecter du HTML dans la sortie finale ?
<a href="#Comment puis-je injecter du HTML dans la sortie finale ?" class="toc-anchor after"></a></h2>

La dernière fois que vous pouvez injecter du HTML tout en gardant votre sortie en cache, c'est lors de l'événement `onOutputGenerated`. Vous pouvez simplement saisir et modifier `$this->grav->output`.

* De nombreuses tâches courantes peuvent être accomplies à l'aide de l'infrastructure [Shortcode Core](https://github.com/getgrav/grav-plugin-shortcode-core).

* Les plugins [Pubmed](https://github.com/Perlkonig/grav-plugin-pubmed) et [Tablesorter](https://github.com/Perlkonig/grav-plugin-tablesorter) adoptent une approche plus brutale.

<h2 id="Comment puis-je injecter des ressources telles que des fichiers JavaScript et CSS ?">Comment puis-je injecter des ressources telles que des fichiers JavaScript et CSS ?
<a href="#Comment puis-je injecter des ressources telles que des fichiers JavaScript et CSS ?" class="toc-anchor after"></a></h2>

Cela se fait via l'interface <span class = "ancre"> Grav\Common\Assets</a>.

* [Google Analytics](https://github.com/escopecz/grav-ganalytics)

* [Bootstrapper](https://github.com/getgrav/grav-plugin-bootstrapper)

* [Gravstrap](https://github.com/giansi/gravstrap)

* [Tablesorter](https://github.com/Perlkonig/grav-plugin-tablesorter)

<h2 id="Comment affecter les en-têtes de réponse et les codes de réponse ?">Comment affecter les en-têtes de réponse et les codes de réponse ?
<a href="#Comment affecter les en-têtes de réponse et les codes de réponse ?" class="toc-anchor after"></a></h2>

Vous pouvez utiliser la commande `header() `de PHP pour définir les en-têtes de réponse. La dernière chose que vous pouvez faire est pendant l'événement `onOutputGenerated`, après quoi la sortie est réellement envoyée au client. Le code de réponse lui-même ne peut être défini que dans l'en-tête YAML de la page en question (`http_response_code`).

* Le plugin [Graveyard](https://github.com/Perlkonig/grav-plugin-graveyard) remplace les réponses `404 NOT FOUND` par `410 GONE` via l'en-tête YAML.

* La [Webmention](https://github.com/Perlkonig/grav-plugin-webmention) définit l'en-tête `Location` sur une réponse `201 CREATED`.

<h2 id="Comment intégrer des bibliothèques tierces dans mon plugin ?">Comment intégrer des bibliothèques tierces dans mon plugin ?
<a href="#Comment intégrer des bibliothèques tierces dans mon plugin ?" class="toc-anchor after"></a></h2>

Habituellement, vous incorporeriez d'autres bibliothèques complètes dans un sous-dossier `vendor` et exigeriez son `autoload.php` le cas échéant dans votre plugin. (Si vous utilisez Git, envisagez d'utiliser des [subtrees](https://help.github.com/articles/about-git-subtree-merges/).)

* [Shortcode Core](https://github.com/getgrav/grav-plugin-shortcode-core)

* [Table Importer](https://github.com/Perlkonig/grav-plugin-table-importer)

<h2 id="Comment interagir avec les API externes ?">Comment interagir avec les API externes ?
<a href="#Comment interagir avec les API externes ?" class="toc-anchor after"></a></h2>

Grav fournit l'objet <span class = "ancre">Grav\Common\GPM\Response</span>, mais rien ne vous empêche de le faire directement si vous le souhaitez.

* [ipLocate](https://github.com/Perlkonig/grav-plugin-iplocate)

* [Pubmed](https://github.com/Perlkonig/grav-plugin-pubmed)

