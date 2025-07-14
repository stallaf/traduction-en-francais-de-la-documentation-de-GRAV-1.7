<h1 class="rem">Variables de thème</h1>

Lorsque vous concevez un thème, Grav vous donne accès à toutes sortes d'objets et de variables à partir de vos modèles Twig. Le cadre de modélisation Twig fournit des moyens puissants pour lire et manipuler ces objets et variables. Ceci est [entièrement expliqué dans leur propre documentation](https://twig.symfony.com/doc/1.x/templates.html) ainsi que [résumé succinctement dans notre propre documentation](./bases-twig.md).

<div class="notice warning">
Dans Twig, vous pouvez appeler des méthodes qui ne prennent aucun paramètre en appelant simplement le nom de la méthode et en omettant les parenthèses <code>()</code>. Si vous devez transmettre des paramètres, vous devez également les fournir après le nom de la méthode. <code>page.content</code> est équivalent à <code>page.content()</code>.
</div>

<h2 id="Objets de base">Objets de base
<a href="#Objets de base" class="toc-anchor after"></a></h2> 

Plusieurs **objets principaux **sont disponibles pour un modèle Twig, et chaque objet possède un ensemble de **variables** et de **fonctions**.

<h3 id="variable base_dir">variable base_dir
<a href="#variable base_dir" class="toc-anchor after"></a></h3> 

La variable `{{ base_dir }}` renvoie le répertoire du fichier de base de l'installation de Grav.

<h3 id="variable base_url">variable base_url
<a href="#variable base_url" class="toc-anchor after"></a></h3> 

Le `{{ base_url }}` renvoie l'URL de base au site Grav, que cela affiche ou non l'URL complète dépend de l'option absolute_urls dans [la configuration du système](./configuration-systeme.md).

<h3 id="variable base_url_relative">variable base_url_relative
<a href="#variable base_url_relative" class="toc-anchor after"></a></h3> 

Le `{{ base_url_relative }}` renvoie l'URL de base au site Grav, sans les informations sur l'hôte.

<h3 id="variable base_url_absolute">variable base_url_absolute
<a href="#variable base_url_absolute" class="toc-anchor after"></a></h3> 

Le `{{ base_url_absolute }}` renvoie l'URL de base au site Grav, y compris les informations sur l'hôte.

<h3 id="variable base_url_simple">variable base_url_simple
<a href="#variable base_url_simple" class="toc-anchor after"></a></h3> 

Le `{{ base_url_simple }}` renvoie l'URL de base au site Grav, sans le code de langue.

<h3 id="variable home_url">variable home_url
<a href="#variable home_url" class="toc-anchor after"></a></h3> 

Le `{{ home_url }}` est particulièrement utile pour renvoyer vers la page d'accueil de votre site. Il est similaire à base_url mais prend en compte la langue actuellement active.

<h3 id="variable html_lang">variable html_lang
<a href="#variable html_lang" class="toc-anchor after"></a></h3> 

Le `{{ html_lang }}` renvoie la langue active.

Le `{{ theme_url }}` renvoie l'URL relative au thème actuel.

Cela renverra la langue active actuelle si elle est fournie, sinon utilisez l'option configurée `site.default_lang`, sinon revenez à `en`.

<h3 id="variable theme_dir">variable theme_dir
<a href="#variable theme_dir" class="toc-anchor after"></a></h3> 

La variable `{{ theme_dir }} `renvoie le dossier du répertoire de fichiers du thème actuel.

<div class="notice info">
Lors de la création de liens vers des ressources telles que des images ou des fichiers JavaScript et CSS, il est recommandé d'utiliser la fonction <code>url()</code> en combinaison avec le flux <code>theme://</code> comme décrit sur la page <a href ="../resume-twig">Balises Filtres & Functions TWIG</a>. Pour JavaScript et CSS, <a href="../asset-manager">Asset Manager</a> est encore plus simple à utiliser mais dans certains cas comme le chargement dynamique ou conditionnel des assets, cela ne fonctionnera pas.
</div>

<h3 id="variable language_codes">variable language_codes
<a href="#variable language_codes" class="toc-anchor after"></a></h3> 

Le `{{ language_codes }}` renvoie la liste des langues disponibles du site.

<h3 id="objet assets">objet assets
<a href="#objet assets" class="toc-anchor after"></a></h3> 

**Asset Manager** ajoute un moyen simple de gérer CSS et JavaScript dans votre site.

```twig
1 | {% do assets.addCss('theme://css/foo.css') %}
2 | {% do assets.addInlineCss('a { color: red; }') %}
3 | {% do assets.addJs('theme://js/something.js') %}
4 | {% do assets.addInlineJs('alert("Warming!");') %}
```

En savoir plus sur [Asset Manager](./asset-manager.md).

<div class="notice note">
<strong>ASTUCE</strong> : Il est recommandé d'utiliser à la place la <a href="../balises-twig">balise styles</a> et la <a href="../balises-twig#script"></a>.
</div>

<h3 id="objet config">objet config
<a href="#objet config" class="toc-anchor after"></a></h3> 

Vous pouvez accéder à n'importe quel paramètre de configuration Grav défini dans les fichiers YAML dans `/user/config via` l'objet `config`. Par example:

    {{ config.system.pages.theme }}{# returns the currently configured theme #}

<h3 id="objet site">objet site
<a href="#objet site" class="toc-anchor after"></a></h3> 

Un alias de l'objet `config.site`. Ceci représente la configuration définie dans le fichier `site.yaml`.

<h3 id="objet system">objet system
<a href="#objet system" class="toc-anchor after"></a></h3> 

Un alias de l'objet `config.system`. Ceci représente la configuration dans le fichier principal `system.yaml`.

<h3 id="objet theme">objet theme
<a href="#objet theme" class="toc-anchor after"></a></h3> 

Un alias de l'objet `config.theme`. Cela représente la configuration du thème actif actuel. Les paramètres du plugin sont disponibles via `config.plugins`.

<h3 id="objet page">objet page
<a href="#objet page" class="toc-anchor after"></a></h3> 

Parce que Grav est construit en utilisant la structure définie dans le dossier `pages/` , chaque page est représentée par un objet page.

L'**objet page** est probablement l'objet _le_ plus important avec lequel vous travaillerez car il contient toutes les informations sur la page actuelle sur laquelle vous vous trouvez.

<div class="notice info">
La liste complète des méthodes de l'objet Page est disponible sur le site API (PAGE NON CONSTRUITE). Voici une liste des méthodes que vous trouverez les plus utiles.
</div>

<h4 id="summary()">summary() 
<a href="#summary()" class="toc-anchor after"></a></h4> 

Cela renvoie une version tronquée ou raccourcie de votre contenu. Vous pouvez fournir un paramètre `size` facultatif pour spécifier la longueur maximale du résumé, en caractères. Alternativement, si aucune taille n'est fournie, la valeur peut être obtenue via la variable à l'échelle du site `summary.size` à partir de votre configuration `site.yaml`.

    {{ page.summary|brut }}

ou alors

    {{ page.summary(50)|raw }}

Une troisième option consiste à utiliser un délimiteur manuel de `===` dans votre contenu. Tout ce qui précède le délimiteur sera utilisé pour le résumé.

<h4 id="content()">content() 
<a href="#content()" class="toc-anchor after"></a></h4> 

Cela renvoie l'intégralité du contenu HTML de votre page.

    {{ page.content|raw }}

<h4 id="header()">header() 
<a href="#header()" class="toc-anchor after"></a></h4> 

Cela renvoie les en-têtes de page tels que définis dans le front-matter YAML de la page. Par exemple une page avec les en-têtes suivants :

```twig
1 | title: My Page
2 | author: Joe Bloggs
```

peut être utilisé:

```console
The author of this page is: {{ page.header.author|e }}
```

<h4 id="media()">media() 
<a href="#media()" class="toc-anchor after"></a></h4> 

Cela renvoie un objet **Media** contenant tous les médias associés à une page. Ceux-ci incluent des **images**, des** vidéos** et d'autres **fichiers**. Vous pouvez accéder aux méthodes multimédias comme décrit dans la [documentation multimédia](./media.md) pour le contenu. Parce qu'il agit comme un tableau, les filtres et fonctions Twig peuvent être utilisés. Remarque : les images SVG sont traitées comme des fichiers, et non comme des images, car elles ne peuvent pas être manipulées à l'aide de filtres d'image twig.

Obtenir un fichier ou une image spécifique :

    {% set my_pdf = page.media['myfile.pdf'] %}

Obtenez la première image :

    {% set first_image = page.media.images|premier %}

Bouclez sur toutes les images et affichez la balise HTML pour l'afficher :

    {% for image in page.media.images %}
       {{ image.html|raw }}
    {% endfor %}

<h4 id="title()">title() 
<a href="#title()" class="toc-anchor after"></a></h4> 

Cela renvoie le titre de la page tel qu'il est défini dans la variable `title` des en-têtes YAML de la page elle-même.

    title: My Page

<h4 id="menu()">menu() 
<a href="#menu()" class="toc-anchor after"></a></h4> 

Cela renvoie la valeur de la variable `menu` des en-têtes YAML de la page. Si aucun n'est fourni, il s'agit par défaut du titre.

```twig
1 | title: My Page
2 | menu: my-page
```

<h4 id="visible()">visible() 
<a href="#visible()" class="toc-anchor after"></a></h4> 

Ceci renvoie si la page est visible ou non. Par défaut, les pages avec une valeur numérique suivie d'un point sont visibles par défaut `(01.somefolder1)` tandis que celles sans `(subfolder2)` ne sont pas considérées comme visibles. Cela peut être remplacé dans les en-têtes de page :

```twig
1 | title: My Page
2 | visible: true
```

<h4 id="routable()">routable() 
<a href="#routable()" class="toc-anchor after"></a></h4> 

Ceci renvoie si oui ou non une page est routable par Grav. Cela signifie que si vous pouvez pointer votre navigateur vers la page et recevoir le contenu en retour. Les pages non routables peuvent être utilisées dans des modèles, des plugins, etc., mais ne peuvent pas être atteintes directement. Cela peut être défini dans les en-têtes de page :

```twig
1 | title: My Page
2 | routable: true
```

<h4 id="slug()">slug() 
<a href="#slug()" class="toc-anchor after"></a></h4> 

Cela renvoie le nom direct tel qu'il est affiché dans l'URL de cette page, par exemple` my-blog-post`.

<h4 id="URL([include_host = false])">URL([include_host = false]) 
<a href="#URL([include_host = false])" class="toc-anchor after"></a></h4> 

Cela renvoie l'URL de la page, par exemple :

    {{ page.url|e }} {# could return /my-section/my-category/my-blog-post #}

ou alors

    {{ page.url(true)|e }} {# could return http://mysite.com/my-section/my-category/my-blog-post #}

<h4 id="permalink()">permalink() 
<a href="#permalink()" class="toc-anchor after"></a></h4> 

Cela renvoie l'URL avec les informations sur l'hôte. Particulièrement utile lorsque vous avez besoin d'un lien rapide accessible de n'importe où.

<h4 id="canonical()">canonical() 
<a href="#canonical()" class="toc-anchor after"></a></h4> 

Cela renvoie l'URL qui est la version "préférée" ou le lien vers une page particulière. Cette valeur sera par défaut l'URL normale à moins que la page n'ait remplacé l'option en-tête de page `canonical:`.
 
 <h4 id="route()">route() 
<a href="#route()" class="toc-anchor after"></a></h4> 

Cela renvoie le routage interne d'une page. Ceci est principalement utilisé pour le routage interne et la répartition des pages.

<h4 id="home()">home() 
<a href="#home()" class="toc-anchor after"></a></h4> 

Cela renvoie `true` ou `false` selon que cette page est configurée ou non comme page **d'accueil**. Ce paramètre se trouve dans le fichier `system.yaml`.

<h4 id="root()">root() 
<a href="#root()" class="toc-anchor after"></a></h4> 

Cela renvoie `true` ou `false` selon que cette page est ou non la page racine de la hiérarchie de l'arborescence.

<h4 id="active()">active() 
<a href="#active()" class="toc-anchor after"></a></h4> 

Cela renvoie `true` ou `false` selon que cette page est ou non actuellement la page à laquelle votre navigateur accède. Ceci est particulièrement utile en navigation pour savoir si la page sur laquelle vous vous trouvez est la page active.

<h4 id="modular()">modular() 
<a href="#modular()" class="toc-anchor after"></a></h4> 

Cela renvoie`true` ou `false` selon que cette page est modulaire ou non.

<h4 id="activeChild()">activeChild() 
<a href="#activeChild()" class="toc-anchor after"></a></h4> 

Ceci renvoie si oui ou non l'URL de cet URI contient l'URL de la page active. Ou en d'autres termes, est l'URL de cette page dans l'URL actuelle. Encore une fois, cela est utile lors de la construction de votre navigation et vous souhaitez savoir si la page sur laquelle vous parcourez est le parent d'une page enfant active.

<h4 id="find(url)">find(url)
<a href="#find(url)" class="toc-anchor after"></a></h4> 

Cela renvoie un objet de page tel que spécifié par une URL de route.

    {% include 'modular/author-detail.html.twig' with {'page': page.find('/authors/billy-bloggs')} %}

<h4 id="collection()">collection() 
<a href="#collection()" class="toc-anchor after"></a></h4> 

Cela renvoie la collection de pages pour ce contexte tel que déterminé par les en-têtes de page de collection.

```twig
1 | {% for child in page.collection %}
2 |   {% include 'partials/blog_item.html.twig' with {'page':child, 'truncate':true} %}
3 | {% endfor %}
```

<h4 id="currentPosition()">currentPosition() 
<a href="#currentPosition()" class="toc-anchor after"></a></h4> 

Cela renvoie l'index de la page en cours par rapport à ses frères et sœurs.

<h4 id="isFirst()">isFirst() 
<a href="#isFirst()" class="toc-anchor after"></a></h4> 

Cela renvoie `true` ou `false` selon que cette page est la première de ses frères et sœurs.

<h4 id="isLast()">isLast() 
<a href="#isLast()" class="toc-anchor after"></a></h4> 

Cela renvoie `true` ou `false` selon que cette page est la dernière de ses frères et sœurs.

<h4 id="nextSibling()">nextSibling() 
<a href="#nextSibling()" class="toc-anchor after"></a></h4> 

Cela renvoie la page suivante du tableau de frères et sœurs en fonction de la position actuelle.

<h4 id="prevSibling()">prevSibling() 
<a href="#prevSibling()" class="toc-anchor after"></a></h4> 

Cela renvoie la page précédente du tableau de frères et sœurs en fonction de la position actuelle.

<div class="notice info">
nextSibling() et prevSibling() ordonnent les pages dans une structure semblable à une pile. Cela fonctionne mieux dans une situation de blog, où le premier article de blog a nextSibling null et prevSibling est l'article de blog précédent. Si ce sens de commande vous embrouille, nous vous suggérons d'utiliser page.adjacentSibling(-1) pour pointer vers la page suivante au lieu de page.nextSibling() afin de réduire la confusion que la terminologie pourrait créer. Vous pouvez également définir une constante dans le thème et l'utiliser pour une meilleure lisibilité, comme page.adjacentSibling(NEXT_PAGE)
</div>

<h4 id="children()">children() 
<a href="#children()" class="toc-anchor after"></a></h4> 

Cela renvoie un tableau de pages enfants pour la page, tel que défini dans la structure de contenu des pages.

<h4 id="orderBy()">orderBy() 
<a href="#orderBy()" class="toc-anchor after"></a></h4>

Cela renvoie le type de commande pour tous les enfants triés de la page. Les valeurs incluent généralement : `default`, `title`, `date` et `folder`. Cette valeur est généralement configurée dans les en-têtes de page.

<h4 id="orderDir()">orderDir() 
<a href="#orderDir()" class="toc-anchor after"></a></h4> 

Cela renvoie la direction de l'ordre pour tous les enfants triés de la page. Les valeurs peuvent être `asc` pour l'ordre croissant ou `desc` pour l'ordre décroissant. Cette valeur est généralement configurée dans les en-têtes de page.

<h4 id="orderManual()">orderManual() 
<a href="#orderManual()" class="toc-anchor after"></a></h4> 

Cela renvoie un tableau de classement manuel des pages pour tous les enfants de la page. Cette valeur est généralement configurée dans les en-têtes de page.

<h4 id="maxCount()">maxCount() 
<a href="#maxCount()" class="toc-anchor after"></a></h4> 

Cela renvoie le nombre maximal de pages enfants pouvant être renvoyées. Cette valeur est généralement configurée dans les en-têtes de page.

<h4 id="children.count()">children.count() 
<a href="#children.count()" class="toc-anchor after"></a></h4> 

Cela renvoie le nombre de pages enfants de la page.

<h4 id="children.current()">children.current() 
<a href="#children.current()" class="toc-anchor after"></a></h4> 

Cela renvoie l'élément enfant actuel. Peut être utilisé lors de l'itération sur les enfants.

<h4 id="children.next()">children.next() 
<a href="#children.next()" class="toc-anchor after"></a></h4> 

Cela renvoie l'enfant suivant dans le tableau d'enfants.

<h4 id="children.prev()">children.prev() 
<a href="#children.prev()" class="toc-anchor after"></a></h4> 

Cela renvoie l'enfant précédent dans le tableau d'enfants.

<h4 id="children.nth(position)">children.nth(position) 
<a href="#children.nth(position)" class="toc-anchor after"></a></h4> 

Cela renvoie l'enfant identifié par `position` qui est un entier de `0` à `children.count() - 1` dans le tableau des enfants.

<h4 id="children.sort(orderBy, orderDir)">children.sort(orderBy, orderDir)
<a href="#children.sort(orderBy, orderDir)" class="toc-anchor after"></a></h4> 

Réorganise les enfants en fonction d'un **orderBy** (`default`, `title`, `date` et `folder`) et **orderDir** (`asc` ou `desc`)

<h4 id="parent()">parent() 
<a href="#parent()" class="toc-anchor after"></a></h4> 

Cela renvoie l'objet de page parent pour cette page. Ceci est très utile lorsque vous avez besoin de remonter dans l'arborescence imbriquée des pages.

<h4 id="isPage()">isPage() 
<a href="#isPage()" class="toc-anchor after"></a></h4> 

Cela renvoie `true` ou `false` selon que cette page est associée à un fichier `.md` réel plutôt qu'à un simple dossier de routage.

<h4 id="isDir()">isDir() 
<a href="#isDir()" class="toc-anchor after"></a></h4> 

Cela renvoie `true` ou `false` selon que cette page est uniquement un dossier pour le routage.

<h4 id="id()">id() 
<a href="#id()" class="toc-anchor after"></a></h4> 

Cela renvoie un identifiant unique pour la page.

<h4 id="modified()">modified() 
<a href="#modified()" class="toc-anchor after"></a></h4> 

Cela renvoie un horodatage de la dernière modification de la page.

<h4 id="date()">date() 
<a href="#date()" class="toc-anchor after"></a></h4> 

Cela renvoie l'horodatage de la date de la page. Généralement, cela est défini dans les en-têtes pour représenter la date d'une page ou d'un article. Si aucune valeur n'est définie explicitement, l'horodatage de modification du fichier est utilisé.

<h4 id="template()">template() 
<a href="#template()" class="toc-anchor after"></a></h4> 

Cela renvoie le nom du modèle de page sans l'extension `.md`. Par exemple `default`.

<h4 id="filePath()">filePath() 
<a href="#filePath()" class="toc-anchor after"></a></h4> 

Cela renvoie le chemin d'accès complet au fichier de la page. Par exemple `/Users/votrenom/sites/grav/user/pages/01.home/default.md`.

<h4 id="filePathClean()">filePathClean() 
<a href="#filePathClean()" class="toc-anchor after"></a></h4> 

Cela renvoie le chemin relatif à partir de la racine du site Grav. Par exemple `user/pages/01.home/default.md`.

<h4 id="path()">path() 
<a href="#path()" class="toc-anchor after"></a></h4> 

Cela renvoie le chemin complet du répertoire contenant la page. Par exemple `/Users/votrenom/sites/grav/user/pages/01.home`.

<h4 id="folder()">folder() 
<a href="#folder()" class="toc-anchor after"></a></h4> 

Cela renvoie le nom du dossier de la page. Par exemple `01.home`.

<h4 id="taxonomy()">taxonomy() 
<a href="#taxonomy()" class="toc-anchor after"></a></h4> 

Cela renvoie un tableau de la taxonomie associée à une page. Celles-ci peuvent être itérées. Ceci est particulièrement utile pour afficher des éléments tels que des balises :

```twig
1 | {% for tag in page.taxonomy.tag %}
2 |    <a href="search/tag:{{ tag }}">{{ tag }}</a>
3 | {% endfor %}
```

<h2 id="Objet pages">Objet pages 
<a href="#Objet pages" class="toc-anchor after"></a></h2> 

**L'objet pages** est la **page racine** qui représente un arbre imbriqué de chaque **objet de page** que Grav connaît. Ceci est particulièrement utile pour la création d'un **sitemap**, la **navigation** ou si vous souhaitez trouver une **page** en particulier.

<div class="notice info">
Cet objet n'est pas le même que <code>grav.pages</code> qui est une instance de la classe <code>Pages</code>.
</div>

<h4 id="children method">children method
<a href="#children method" class="toc-anchor after"></a></h4> 

Cela renvoie les pages enfants immédiates sous la forme d'un tableau **d'objets de page**. Comme l'objet pages représente l'arborescence entière, vous pouvez parcourir entièrement chaque page du dossier Grav pages/.

Obtenez les pages de niveau supérieur pour un menu simple :

```twig
1 | <ul class="navigation">
2 |     {% for page in pages.children %}
3 |         {% if page.visible %}
4 |             <li><a href="{{ page.url }}">{{ page.menu }}</a></li>
5 |         {% endif %}
6 |     {% endfor %}
7 |</ul>
```

<h2 id="Objet media)">Objet media 
<a href="#Objet media" class="toc-anchor after"></a></h2> 

Il existe un nouvel objet qui vous permet d'accéder aux médias qui se trouvent en dehors des objets Page via les flux PHP de Twig. Cela fonctionne de la même manière que la liaison d'images dans le contenu en utilisant des flux pour accéder aux images et un traitement multimédia pour manipuler le thème.

    {{ media['user://media/bird.png'].resize(50, 50).rotate(90).html()|raw }}

<h2 id="Objet uri">Objet uri 
<a href="#Objet uri" class="toc-anchor after"></a></h2> 

<div class="notice info">
La liste complète des méthodes de l'objet Uri est disponible sur le site de l'API(PAGE NON CONSTRUITE) . Voici une liste des méthodes que vous trouverez les plus utiles.
</div>

L'objet Uri a plusieurs méthodes pour accéder à des parties de l'URI actuel. Pour l'URL complète `http://mysite.com/grav/section/category/page.json/param1:foo/param2:bar/?query1=baz&query2=qux :`

<h4 id="path()">path() 
<a href="#path()" class="toc-anchor after"></a></h4> 

Cela renvoie la partie chemin de l'URL : (par exemple,` uri.path = /section/category/page)`

<h4 id="paths()">paths() 
<a href="#paths()" class="toc-anchor after"></a></h4> 

Cela renvoie le tableau des éléments de chemin : (par exemple, `uri.paths = [section, catégorie, page])`

<h4 id="route([absolute = false][, domain = false])">route([absolute = false][, domain = false]) 
<a href="#route([absolute = false][, domain = false])" class="toc-anchor after"></a></h4> 

Cela renvoie la route sous forme d'URL absolue ou relative. (par exemple `uri.route(true)` = `http://mysite.com/grav/section/category/page` ou `uri.route() = /section/category/page`)

<h4 id="params()">params() 
<a href="#params()" class="toc-anchor after"></a></h4> 

Cela renvoie la partie params de l'URL : (par exemple, `uri.params` = `/param1:foo/param2:bar`)

<h4 id="param(id)">param(id)
<a href="#param(id)" class="toc-anchor after"></a></h4> 

Cela renvoie la valeur d'un paramètre particulier. (par exemple, `uri.param('param1')` = `foo`)

<h4 id="query()">query() 
<a href="#query()" class="toc-anchor after"></a></h4> 

Cela renvoie la partie requête de l'URL : (par exemple, `uri.query` = `query1=bar&query2=qux`)

<h4 id="quer(id)">quer(id) 
<a href="#quer(id)" class="toc-anchor after"></a></h4> 

Vous pouvez également récupérer des éléments de requête spécifiques : (par exemple, `uri.query('query1')` = `bar`)

<h4 id="URL([include_host = true])">URL([include_host = true])
<a href="#URL([include_host = true])" class="toc-anchor after"></a></h4> 

Cela renvoie l'URL complète avec ou sans l'hôte. (par exemple `uri.url(false)` = `grav/section/category/page/param:foo?query=bar`)

<h4 id="extension()">extension() 
<a href="#extension()" class="toc-anchor after"></a></h4> 

Cela renvoie l'extension, ou renverra `html` s'il n'est pas fourni : (par exemple, `uri.extension` = `json`)

<h4 id="host()">host() 
<a href="#host()" class="toc-anchor after"></a></h4> 

Cela renvoie la partie hôte de l'URL. (par exemple, `uri.host` = `mysite.com`)

<h4 id="base()">base() 
<a href="#base()" class="toc-anchor after"></a></h4> 

Cela renvoie la partie de base de l'URL. (par exemple, `uri.base ` = `http://monsite.com`)

<h4 id="rootUrl([include_host = false])">rootUrl([include_host = false]) 
<a href="#rootUrl([include_host = false])" class="toc-anchor after"></a></h4> 

Cela renvoie l'URL racine à l'instance grav. (par exemple `uri.rootUrl()` = `http://monsite.com/grav`)

<h4 id="referrer()">referrer() 
<a href="#referrer()" class="toc-anchor after"></a></h4> 

Cela renvoie les informations de référence pour cette page.

<h2 id="Objet header">Objet header 
<a href="#Objet header" class="toc-anchor after"></a></h2> 

L'objet d'en-tête est un alias pour `page.header()` de la page d'origine. C'est un moyen pratique d'accéder aux en-têtes de page d'origine lorsque vous parcourez d'autres objets `page ` de pages enfants ou de collections.

<h2 id="content string">content string 
<a href="#content string" class="toc-anchor after"></a></h2> 

L'objet de contenu est un alias pour la `page.content()` de la page d'origine.

Pour afficher le contenu de la page, vous devez :

    {{ content|raw }}

<h2 id="Objet de taxonomie">Objet de taxonomie
<a href="#Objet de taxonomie" class="toc-anchor after"></a></h2> 

L'objet Taxonomy global qui contient toutes les informations de taxonomie pour le site. Pour plus d'informations, voir Taxonomy.

<h2 id="Objet navigateur">Objet navigateur
<a href="#Objet navigateur" class="toc-anchor after"></a></h2> 

<div class="notice info">
La liste complète des méthodes de l'objet Browser est disponible sur le site API. Voici une liste des méthodes que vous trouverez les plus utiles.
</div>

Grav a un support intégré pour déterminer par programmation la plate-forme, le navigateur et la version de l'utilisateur.

```twig
1 | {{ browser.platform|e }}   # macintosh
2 | {{ browser.browser|e }}    # chrome
3 | {{ browser.version|e }}    # 41
```

<h2 id="Objet utilisateur">Objet utilisateur
<a href="#Objet utilisateur" class="toc-anchor after"></a></h2> 

Vous pouvez accéder indirectement à l'objet utilisateur actuellement connecté via l'objet Grav. Cela vous permet d'accéder à des données telles que `username`, `fullname`, `title`, et `email`.

```twig
1 | {{ grav.user.username|e }}  # admin
2 | {{ grav.user.fullname|e }}  # Billy Bloggs
3 | {{ grav.user.title|e }}     # Administrator
4 | {{ grav.user.email|e }}     # billy@bloggs.com
```

<h2 id="Ajout de variables personnalisées">Ajout de variables personnalisées
<a href="#Ajout de variables personnalisées" class="toc-anchor after"></a></h2>

Vous pouvez facilement ajouter des variables personnalisées de différentes manières. Si la variable est une variable à l'échelle du site, vous pouvez placer la variable dans votre fichier `user/config/site.yaml` puis y accéder via :

    {{ site.my_variable|e }}

Alternativement, si la variable n'est nécessaire que pour une page particulière, vous pouvez ajouter la variable au front-matter YAML de votre page et y accéder via l'objet `page.header`. Par example:

```yaml
title: My Page
author: Joe Bloggs
```

pourrait être utilisé comme :

    The author of this page is: {{ page.header.author|e }}

<h2 id="Ajout d'objets personnalisés">Ajout d'objets personnalisés
<a href="#Ajout d'objets personnalisés" class="toc-anchor after"></a></h2>

Un moyen avancé d'ajouter des objets personnalisés consiste à utiliser un plugin pour ajouter des objets à l'objet Twig. Ceci est un sujet avancé et est couvert plus en détail dans le [chapitre plugins](./crochets-evenements.md).

