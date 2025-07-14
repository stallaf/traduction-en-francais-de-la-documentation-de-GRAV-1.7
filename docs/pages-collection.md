<h1 class = "rem">Collections de pages</h1>

Dans Grav, le type de collection le plus courant est une liste de pages qui peut être définie soit dans le frontmatter de la page, soit dans le Twig lui-même. La plus courante consiste à définir une collection dans le frontmatter. Avec une collection définie, elle est disponible dans le Twig de la page pour en faire ce que vous voulez. En utilisant des méthodes de collecte de pages ou en parcourant chaque [objet de page](./variables-theme#objet page) et en utilisant les méthodes ou propriétés de page, vous pouvez faire des choses puissantes. Des exemples courants de ceci incluent l'affichage d'une liste d'articles de blog ou l'affichage de sous-pages modulaires pour rendre une conception de page complexe.

<h2 id="Collection d'objets">Collection d'objets
<a href="#Collection d'objets" class="toc-anchor after"></a></h2>

Lorsque vous définissez une collection dans l'en-tête de la page, vous créez dynamiquement une [Collection Grav](https://github.com/getgrav/grav/blob/develop/system/src/Grav/Common/Page/Collection.php) qui est disponible dans le Twig de la page. Cet Collection d'objets est **itérable** et peut être traité comme un **tableau** qui vous permet de faire des choses telles que :

    1 | {{ dump(page.collection[page.path]) }}

<h2 id="Exemple de définition de collection">Exemple de définition de collection
<a href="#Exemple de définition de collection" class="toc-anchor after"></a></h2>

Un exemple de collection définie dans le frontmatter de la page :

```yaml
1 | content:
2 |     items: '@self.children'
3 |     order:
4 |         by: date
5 |          dir: desc
6 |     limit: 10
7 |     pagination: true
```

La valeur `content.items` dans le frontmatter de la page indique à Grav de rassembler une collection d'éléments et les informations transmises à ceci définissent comment la collection doit être construite.

Cette définition crée une collection pour la page qui se compose de toutes les **pages enfants** triées par **date** dans l'ordre **décroissant** avec une **pagination** affichant **10 éléments** par page.

<h2 id="Accéder aux collections dans Twig">Accéder aux collections dans Twig
<a href="#Accéder aux collections dans Twig" class="toc-anchor after"></a></h2>

Lorsque cette collection est définie dans le header, Grav crée une collection **page.collection** à laquelle vous pouvez accéder dans un template twig avec :

```yaml
1 | {% for p in page.collection %}
2 | <h2>{{ p.title|e }}</h2>
3 | {{ p.summary|raw }}
4 | {% endfor %}
```

Cela boucle simplement sur les [pages](./variables-theme#objet page) de la collection affichant le titre et le résumé.

Vous pouvez également inclure un paramètre d'ordre pour modifier l'ordre par défaut des pages :

```yaml
1 | {% for p in page.collection.order('folder','asc') %}
2 | <h2>{{ p.title|e }}</h2>
3 | {{ p.summary|raw }}
4 | {% endfor %}
```

<h2 id="En-têtes de collection">En-têtes de collection
<a href="#En-têtes de collection" class="toc-anchor after"></a></h2>

Pour indiquer à Grav qu'une page spécifique doit être une page de liste et contenir des pages enfants, il existe un certain nombre de variables qui peuvent être utilisées :

<h3 id="Résumé des options de collection">Résumé des options de collection
<a href="#Résumé des options de collection" class="toc-anchor after"></a></h3>


| **CHAINE**                         | **RÉSULTAT**
| :-------------------------     | :---------------------
| `'@root.pages'`                | Récupère les pages de niveau supérieur.
| `'@root.descendants'`          | Récupère toutes les pages du site.
| `'@root.all'`                  | Récupère toutes les pages et modules du site.
|                                |
| `'@self.page'`                 | Obtenir une collection avec uniquement la page actuelle.
| `'@self.parent'`               | Récupère une collection avec uniquement le parent de la page actuelle.
| `'@self.siblings'`             | Obtenir les frères et sœurs de la page actuelle.
| `'@self.children'`             | Récupère les enfants de la page actuelle.
| `'@self.modules'`              | Récupère les modules de la page en cours.
| `'@self.all'`                  | Récupère à la fois les enfants et les modules de la page actuelle.
| `'@self.descendants'`          | Parcourir tous les enfants de la page en cours.
|                                |
| `'@page.page': '/fruit'`       | Obtenir une collection avec uniquement la page `/fruit`.
| `'@page.parent': '/fruit'`     | Récupère une collection avec uniquement le parent de la page `/fruit`.
| `'@page.siblings' : '/fruit'`  | Récupère les frères et sœurs de la page `/fruit`.
| `'@page.children': '/fruit'`   | Récupère les enfants de la page  `/fruit`.
| `'@page.modules' : '/fruit'`   | Récupère les modules de la page  `/fruit`.
| `'@page.all': '/fruit'`        | Récupère à la fois les enfants et les modules de la page `/fruit`.
| `'@page.descendants': '/fruit'`|Obtenir et parcourir tous les enfants de la page `/fruit`.
|                                |
| `'@taxonomy.tag'` :            | taxonomie de la photographie avec tag= `photography`.
| `'@taxonomy' : {tag: birds, category: blog}'` | taxonomie avec tag= `birds` && category= `blog`.

<div class="notice note">
Ce document décrit l'utilisation de <code>@page</code>, <code>@taxonomy.category</code>, etc., mais un format alternatif plus sûr pour YAML est <code>page@</code>, <code>taxonomy@.category</code>. Toutes les commandes <code>@</code> peuvent être écrites au format préfixe ou postfixe.
</div>

<div class="notice info">
Les options de collection ont été améliorées et modifiées depuis <strong>Grav 1.6.</strong>  Les anciennes versions fonctionneront toujours, mais leur utilisation n'est pas recommandée.
</div>

Nous allons les couvrir plus en détail.

<h2 id="Root Collections">Root Collections
<a href="#Root Collections" class="toc-anchor after"></a></h2>

<h5 id="@root.pages - Pages de niveau supérieur">@root.pages - Pages de niveau supérieur
<a href="#@root.pages - Pages de niveau supérieur" class="toc-anchor after"></a></h5>

Cela peut être utilisé pour récupérer les **pages publiées** de niveau supérieur/racine d'un site. Particulièrement utile pour récupérer les éléments qui composent la navigation principale par exemple :

```yaml
1 | content:
2 |     items: '@root.pages'
```

Un alias de `'@root.children'` est également valide. L'utilisation de `'@root'` est obsolète car sa signification peut changer à l'avenir.

<h5 id="@root.descendants - Toutes les pages">@root.descendants - Toutes les pages
<a href="#@root.descendants - Toutes les pages" class="toc-anchor after"></a></h5>

Cela permettra d'obtenir efficacement chaque page de votre site car il navigue de manière récursive à travers tous les enfants de la page racine vers le bas, et construit une collection de **toutes** les **pages publiées** d'un site :

```yaml
1 | content:
2 |     items: '@root.descendants'
```

<h5 id="@root.all - Toutes les pages et modules">@root.all - Toutes les pages et modules
<a href="#@root.all - Toutes les pages et modules" class="toc-anchor after"></a></h5>

Cela fera la même chose que ci-dessus, mais cela inclut **toutes** les **pages et modules publiés** du site.

```yaml
1 | content:
2 |     items: '@root.all'
```

<h2 id="Self-collections">Self-collections
<a href="#Self-collections" class="toc-anchor after"></a></h2>

<h5 id="@self.page - Page actuelle uniquement">@self.page - Page actuelle uniquement
<a href="#@self.page - Page actuelle uniquement" class="toc-anchor after"></a></h5>

Cela renverra une collection contenant uniquement la page actuelle.

```yaml
1 | content:
2 |     items: '@self.page'
```

Un alias de `'@self.self'` est également valide.

Une collection vide sera renvoyée si la page n'a pas été publiée.

<h5 id="@self.parent - La page parent de la page actuelle">@self.parent - La page parent de la page actuelle
<a href="#@self.parent - La page parent de la page actuelle" class="toc-anchor after"></a></h5>

Il s'agit d'une collection de cas particuliers car elle renverra toujours uniquement le **parent** de la page actuelle :

```yaml
1 | content:
2 |     items: '@self.parent'
```

Une collection vide sera retournée si la page est au niveau supérieur.

<h5 id="@self.siblings - Frères et sœurs de la page actuelle">@self.siblings - Frères et sœurs de la page actuelle
<a href="#@self.siblings - Frères et sœurs de la page actuelle" class="toc-anchor after"></a></h5>

Cette collection collectera toutes les **pages publiées** au même niveau de la page courante, à l'exclusion de la page courante :

```yaml
1 | content:
2 |     items: '@self.siblings'
```

<h5 id="@self.children - Enfants de la page actuelle">@self.children - Enfants de la page actuelle
<a href="#@self.children - Enfants de la page actuelle" class="toc-anchor after"></a></h5>

Ceci est utilisé pour lister les **enfants publiés** de la page actuelle :

```yaml
1 | content:
2 |     items: '@self.children'
```

Un alias de `'@self.pages'` est également valide. L'utilisation de `'@self'` est obsolète car sa signification peut changer à l'avenir.

<h5 id="@self.modules - Modules de la page courante">@self.modules - Modules de la page courante
<a href="#@self.modules - Modules de la page courante" class="toc-anchor after"></a></h5>

Cette méthode récupère uniquement les modules publiés de la page en cours (`_features`, `_showcase`, etc.) :

```yaml
1 | content:
2 |     items: '@self.modules'
```

L'utilisation de l'alias `@self.modular` est obsolète.

<h5 id="@self.all - Enfants et modules de la page actuelle">@self.all - Enfants et modules de la page actuelle
<a href="#@self.all - Enfants et modules de la page actuelle" class="toc-anchor after"></a></h5>

Cette méthode récupère uniquement les **enfants et modules publiés** de la page actuelle :

```yaml
1 | content:
2 |     items: '@self.all'
```

<h5 id="@self.descendants - Enfants + tous les descendants de la page actuelle">@self.descendants - Enfants + tous les descendants de la page actuelle
<a href="#@self.descendants - Enfants + tous les descendants de la page actuelle" class="toc-anchor after"></a></h5>

Semblable à `.children`, la collection `.descendants` récupérera tous les **enfants publiés** mais continuera à parcourir tous leurs enfants :

```yaml
1 | content:
2 |     items: '@self.descendants'
```

<h2 id="Collections de pages">Collections de pages
<a href="#Collections de pages" class="toc-anchor after"></a></h2>  
      
<h5 id="@page.page - Collection de la page spécifique uniquement">@page.page - Collection de la page spécifique uniquement
<a href="#@page.page - Collection de la page spécifique uniquement" class="toc-anchor after"></a></h5>

Cette collection prend une route slug d'une page comme argument et renverra la collection contenant cette page (s'il s'agit d'une **page publiée**):

```yaml
1 | content:
2 |     items:
3 |         '@page.page': '/blog'
```

Un alias de `'@page.self' : '/blog'` est également valide.

<div class="notice note">
Une collection vide sera renvoyée si la page n'a pas été publiée.
</div>

<h5 id="@page.parent - La page parent d'une page spécifique">@page.parent - La page parent d'une page spécifique
<a href="#@page.parent - La page parent d'une page spécifique" class="toc-anchor after"></a></h5>

Il s'agit d'une collection de cas particuliers car elle renverra toujours uniquement le **parent** de la page actuelle :

```yaml
1 | content:
2 |     items:
3 |         '@page.parent': '/blog'
```
@

<div class="notice note">
Une collection vide sera retournée si la page est au niveau supérieur.
</div>

<h5 id="@page.siblings - Frères et sœurs d'une page spécifique">@page.siblings - Frères et sœurs d'une page spécifique
<a href="#@page.siblings - Frères et sœurs d'une page spécifique" class="toc-anchor after"></a></h5>

Cette collection collectera toutes les **pages publiées** au même niveau de la page, à l'exclusion de la page elle-même :

```yaml
1 | content:
2 |     items:
3 |         '@page.siblings': '/blog'
```

<h5 id="@page.children - Enfants d'une page spécifique">@page.children - Enfants d'une page spécifique
<a href="#@page.children - Enfants d'une page spécifique" class="toc-anchor after"></a></h5>

Cette collection prend une route slug d'une page comme argument et renverra tous les **enfants publiés** de cette page :

```yaml
1 | content:
2 |     items:
3 |         '@page.children': '/blog'
```

Un alias de `'@page.pages' : '/blog'` est également valide. Utilisation de `'@page' : '/blog'` est obsolète car sa signification peut changer à l'avenir.

<h5 id="@page.modules - Modules d'une page spécifique">@page.modules - Modules d'une page spécifique
<a href="#@page.modules - Modules d'une page spécifique" class="toc-anchor after"></a></h5>

Cette collection prend une route slug d'une page comme argument et renverra tous les **modules publiés** de cette page :

```yaml
1 | content:
2 |     items:
3 |         '@page.modules': '/blog'
```

Utiliser l'alias de `'@page.modular' : '/blog'` est obsolète.

<h5 id="@self.all - Enfants et modules d'une page spécifique">@self.all - Enfants et modules d'une page spécifique
<a href="#@self.all - Enfants et modules d'une page spécifique" class="toc-anchor after"></a></h5>

Cette méthode récupère uniquement les **enfants et modules publiés** de la page spécifique :

```yaml
1 | content:
2 |     items:
3 |         '@page.all': '/blog'
```

<h5 id="@page.descendants - Collection d'enfants + tous les descendants d'une page spécifique">@page.descendants - Collection d'enfants + tous les descendants d'une page spécifique
<a href="#@page.descendants - Collection d'enfants + tous les descendants d'une page spécifique" class="toc-anchor after"></a></h5>

Cette collection prend une route slug d'une page comme argument et renverra tous les **enfants publiés** et tous leurs descendants de cette page :

```yaml
1 | content:
2 |     items:
3 |         '@page.descendants': '/blog'
```

<h2 id="Collections de taxonomie">Collections de taxonomie
<a href="#Collections de taxonomie" class="toc-anchor after"></a></h2>

```yaml
1 | content:
2 |     items:
3 |         '@taxonomy.tag': foo
```

En utilisant l'option `@taxonomy`, vous pouvez utiliser la puissante fonctionnalité de taxonomie de Grav. C'est là que la variable `taxonomy` dans le fichier de [configuration du site](./configuration.md#Configuration du site) entre en jeu. Il **doit** y avoir une définition pour la taxonomie définie dans ce fichier de configuration pour que Grav interprète une référence de page comme valide.

En définissant `@taxonomy.tag:foo`, Grav trouvera toutes les **pages publiées** dans le dossier `/user/pages` qui ont elles-mêmes défini `tag:foo` dans leur variable de taxonomie :

```yaml
1 | content:
2 |     items:
3 |         '@taxonomy.tag': [foo, bar]
```

La variable `content.items` peut prendre un tableau de taxonomies et rassemblera toutes les pages qui satisfont à ces règles. Les pages publiées contenant **à la fois** des balises `foo` et `bar` seront collectées. Le chapitre [Taxonomie](./taxonomy) couvrira ce concept plus en détail.

<div class="notice info">
Si vous souhaitez placer plusieurs variables en ligne, vous devrez séparer les sous-variables de leurs parents avec des crochets <code>{}</code>. Vous pouvez ensuite séparer les variables individuelles à ce niveau par une virgule. Par exemple : <code>'@taxonomy' : {category : [blog, feature], tag : [foo, bar]}</code>. Dans cet exemple, les sous-variables <code>category</code> et <code>tag</code> sont placées sous <code>@taxonomy</code> dans la hiérarchie, chacune avec des valeurs répertoriées placées entre crochets <code>[]</code>. Les pages doivent répondre à toutes ces exigences pour être trouvées.
</div>

Si vous avez plusieurs variables à définir dans un seul parent, vous pouvez le faire en utilisant la méthode en ligne, mais pour plus de simplicité, nous vous recommandons d'utiliser la méthode standard. Voici un exemple:

```yaml
1 | content:
2 |     items:
3 |        '@taxonomy':
4 |           category: [blog, featured]
5 |           tag: [foo, bar]
```

Chaque niveau de la hiérarchie ajoute deux espaces avant la variable. YAML vous permettra d'utiliser autant d'espaces que vous le souhaitez ici, mais deux sont une pratique courante. Dans l'exemple ci-dessus, les variables `category` et `tag` sont définies sous `@taxonomy`.

Les collections de pages seront automatiquement filtrées par taxonomie lorsqu'une a été définie dans l'URL (par exemple, /archive/category:news). Cela vous permet de créer un seul modèle d'archive de blog mais de filtrer la collection de manière dynamique à l'aide de l'URL. Si votre collection de pages doit ignorer la taxonomie définie dans l'URL, utilisez l'indicateur `url_taxonomy_filters:false` pour désactiver cette fonctionnalité.

<h2 id="Collections complexes">Collections complexes
<a href="#Collections complexes" class="toc-anchor after"></a></h2>

Vous pouvez également fournir plusieurs définitions de collection complexes et la collection résultante sera la somme de toutes les pages trouvées à partir de chacune des définitions de collection. Par exemple:

```yaml
1 | content:
2 |     items:
3 |       - '@self.children'
4 |       - '@taxonomy':
5 |            category: [blog, featured]
```

En outre, vous pouvez filtrer la collection à l'aide de `filter : type : value`. Le type peut être l'un des suivants : `published`, `visible`, `page`, `module`, `routable`. Celles-ci correspondent aux [méthodes spécifiques à la collection](#Méthodes de collection d'objets), et vous pouvez en utiliser plusieurs pour filtrer votre collection. Ils sont tous `true` ou `false`. De plus, il existe un `type` qui prend un seul nom de modèle, un `types` qui prent un tableau de noms de modèles et `access` qui prend un tableau de niveaux d'accès. Par exemple:

```yaml
1 | content:
2 |   items: '@self.siblings'
3 |   filter:
4 |     visible: true
5 |     type: 'blog'
6 |     access: ['site.login']
```

Le type peut aussi être négatif : `non-published`, `non-visible`, `non-page` (=module), `non-module` (=page) et `non-routable`, mais il est préférable d'utiliser la version positive avec la valeur `false`.

```yaml
1 | content:
2 |   items: '@self.children'
3 |   filter:
4 |     published: false
```

<div class="notice info">
Les filtres de collecte ont été simplifiés depuis <strong>Grav 1.6.</strong> Les anciennes variantes <code>modular</code> et <code>non-modular</code> fonctionneront toujours, mais leur utilisation n'est pas recommandée. Utilisez <code>module</code> et <code>page</code> à la place.
</div>

<h2 id="Options de commande">Options de commande
<a href="#Options de commande" class="toc-anchor after"></a></h2>

```yaml
1 | content:
2 |   order:
3 |      by: date
4 |      dir: desc
5 |   limit: 5
6 |   pagination: true
```

L'ordre des sous-pages suit les mêmes règles que l'ordre des dossiers, les options disponibles sont :

| **COMMANDE**                | **DÉTAILS**
| :------------               | :---------------
| `default`                   | L'ordre est basé sur le système de fichiers, c'est-à-dire <code>01.home</code> avant <code>02.advark</code>.
| `title`                     | L'ordre est basé sur le titre tel que défini dans chaque page.
| `basename`                   | L'ordre est basé sur le nom alphabétique du dossier après qu'il a été traité par la fonction PHP <code>basename()</code>.
| `date`                      | L'ordre est basé sur la date telle que définie dans chaque page.
| `modified`                  | L'ordre est basé sur l'horodatage modifié de la page.
| `folder`                    | L'ordre est basé sur le nom du dossier avec tout préfixe numérique, c'est-à-dire `01`., supprimé.
| `header.x`                  | L'ordre est basé sur n'importe quel champ d'en-tête de page. c'est-à-dire `header.taxonomy.year`. <br>Une valeur par défaut peut également être ajoutée via un tube. c'est-à-dire `header.taxonomy.year|2015`.
| `random`                    | La commande est aléatoire.
| `custom`                    | La commande est basée sur la variable `content.order.custom`.
| `manual`                    | La commande est basée sur la variable `order_manual`. **DÉPRECIÉ**
| `sort_flag`                 | Permet de remplacer les drapeaux de tri pour le classement par en-tête de page ou par défaut.<br> Si l'extension PHP `intl` est chargée, seuls [ces drapeaux](https://secure.php.net/manual/en/collator.asort.php) sont disponibles. Sinon, vous<br> pouvez utiliser les [indicateurs de tri standard](https://secure.php.net/manual/en/array.constants.php) PHP.

La variable `content.order.dir` contrôle la direction dans laquelle le classement doit être dans des valeurs admises. Les valeurs valides sont `desc` ou `asc`.

```yaml
1  | content:
2  |    order:
3  |       by: default
4  |       custom:
5  |          - _showcase
6  |          - _highlights
7  |          - _callout
8  |          - _features
9  |    limit: 5
10 |    pagination: true
```

Dans la configuration ci-dessus, vous pouvez voir que `content.order.custom` définit une **commande manuelle personnalisée** pour s'assurer que la page est construite avec **showcase** en premier, la section **highlights** en second, etc. Veuillez noter que si une page n'est pas spécifiée dans la liste de commande personnalisée , puis Grav se rabat sur le `content.order.by` pour les pages non spécifiées.

Si une page a un slug personnalisé, vous devez utiliser ce slug dans la liste `content.order.custom`.

Le `content.pagination` est un simple indicateur booléen à utiliser par les plugins, etc. pour savoir si la **pagination** doit être initialisée pour cette collection,`content.limit` est le nombre d'éléments affichés par page lorsque la pagination est activée.

<h2 id="Plage de dates">Plage de dates
<a href="#Plage de dates" class="toc-anchor after"></a></h2>

Il est également possible de filtrer les pages par plage de dates :

```yaml
1 | content:
2 |    items: '@self.children'
3 |    dateRange:
4 |       start: 1/1/2014
5 |       end: 1/1/2015
```

Vous pouvez utiliser n'importe quel format de date de chaîne pris en charge par [strtotime()](https://php.net/manual/en/function.strtotime.php) tel que `-6 semaines` ou `lundi dernier` ainsi que des dates plus traditionnelles telles que `23/01/2014` ou `23 janvier 2014`. Le dateRange filtrera toutes les pages qui ont une date en dehors la plage de dates fournie. Les dates de **début** et de **fin** sont facultatives, mais au moins une doit être fournie.

<h2 id="Collections multiples">Collections multiples
<a href="#Collections multiples" class="toc-anchor after"></a></h2>

Lorsque vous créez une collection avec du `content : items:` dans votre YAML, vous définissez une seule collection basée sur plusieurs conditions. Cependant, Grav vous permet de créer un ensemble arbitraire de collections par page, il vous suffit d'en créer une autre :

```yaml
1  | content:
2  |    items: '@self.children'
3  |    order:
4  |       by: date
5  |       dir: desc
6  |    limit: 10
7  |    pagination: true
8  |
9  | fruit:
10 |    items:
11 |       '@taxonomy.tag': [fruit]
```

Cela configure **2 collections** pour cette page, la première utilise `content` de la collection par défaut, mais la seconde définit une collection basée sur la taxonomie appelée `fruit`. Pour accéder à ces deux collections via Twig vous pouvez utiliser la syntaxe suivante :

```yaml
1 | {% set default_collection = page.collection %}
2 | {% set fruit_collection = page.collection('fruit') %}
```

<h2 id="Méthodes de collection d'objets">Méthodes de collection d'objets
<a href="#Méthodes de collection d'objets" class="toc-anchor after"></a></h2>  
  
Les méthodes itérables incluent :

| **PROPRIÉTÉS**                        | **DESCRIPTION**
| :---------                            | :---------
| `Collection::append($items)`          | Ajouter une autre collection ou tableau.
| `Collection::first()`                 | Récupère le premier élément de la collection.
| `Collection::last()`                  | Récupère le dernier élément de la collection.
| `Collection::random($num)`            | Choisissez `$num` éléments aléatoires de la collection.
| `Collection::reverse()`               | Inverse l'ordre de la collection.
| `Collection::shuffle()`               | Randomise toute la collection.
| `Collection::slice($offset, $length)` | Coupe la liste.

Dispose également de plusieurs méthodes utiles spécifiques à la collection :

| **PROPRIÉTÉS**                          | **DESCRIPTION**
| :--------------                         | :-------------
| `Collection::addPage($page)`            | Vous pouvez ajouter une autre page à cette collection.
| `Collection::copy()`                    | Crée une copie de la collection courante.
| `Collection::current()`                 | Obtient l'élément actuel de la collection.
| `Collection::key()`                      | Renvoie le slug de l'élément courant.
| `Collection::remove($path)`             | Supprime une page spécifique de la collection, ou actuelle si `$path = null`.
| `Collection::order($by, $dir, $manual)` | Ordonne la collection actuelle
| `Collection::intersect($collection2)`   | Fusionne deux collections, en conservant les éléments qui se produisent dans <br>les deux collections (comme une condition "ET").
| `Collection::merge($collection2)`       | Fusionne deux collections, en conservant les éléments qui se produisent dans <br>l'une ou l'autre des collections (comme une condition "OU").
| `Collection::isFirst($path)`            | Détermine si la page identifiée par path est la première.
| `Collection::isLast($path)`             | Détermine si la page identifiée par path est la dernière.
| `Collection::prevSibling($path)`        | Renvoie la page sœur précédente si possible.
| `Collection::nextSibling($path)`        | Renvoie la page sœur suivante si possible.
| `Collection::currentPosition($path)`    | Renvoie l'index courant.
| `Collection::dateRange($startDate, $endDate, $field)`  | Filtre la collection actuelle avec des dates.
| `Collection::visible()`                 | Filtre la collection actuelle pour n'inclure que les pages visibles.
| `Collection::nonVisible()`              | Filtre la collection actuelle pour n'inclure que les pages non visibles.
| `Collection::pages()`                   | Filtre la collection actuelle pour n'inclure que les pages (et non les modules).
| `Collection::modules()`                 | Filtre la collection actuelle pour n'inclure que les modules (et non les pages).
| `Collection::published()`               | Filtre la collection actuelle pour n'inclure que les pages publiées.
| `Collection::nonPublished()`            | Filtre la collection actuelle pour n'inclure que les pages non | publiées.
| `Collection::routable()`                | Filtre la collection actuelle pour n'inclure que les pages routables.
| `Collection::nonRoutable()`             | Filtre la collection actuelle pour n'inclure que les pages non routables.
| `Collection::ofType($type)`             | Filtre la collection actuelle pour n'inclure que les pages où template = `$type`.
| `Collection::ofOneOfTheseTypes($types)` | Filtre la collection actuelle pour n'inclure que les pages où le modèle est dans le tableau `$types`.
| `Collection::ofOneOfTheseAccessLevels($levels)` | Filtre la collection actuelle pour n'inclure que les pages où l'accès à la <br>page se trouve dans le tableau de `$levels`.

<div class="notice info">
Les méthodes suivantes ont été dépréciées dans <strong>Grav 1.7</strong> : <code>Collection::modular()</code> et <code>Collection::nonModular()</code>. Utilisez <code>Collection::modules()</code> et <code>Collection::pages()</code> respectivement.
</div>

Voici un exemple tiré du thème **Learn2 docs.html.twig** qui définit une collection basée sur la taxonomie (et éventuellement des balises si elles existent) et utilise les méthodes `Collection::isFirst` et `Collection::isLast` pour ajouter conditionnellement la navigation de page :

```html
1  | {% set tags = page.taxonomy.tag %}
2  | {% if tags %}
3  |    {% set progress = page.collection({'items':{'@taxonomy':{'category': 'docs', 'tag': tags}},'order': {'by': 'default', 'dir': 'asc'}}) %}
4  | {% else %}
5  |    {% set progress = page.collection({'items':{'@taxonomy':{'category': 'docs'}},'order': {'by': 'default', 'dir': 'asc'}}) %}
6  | {% endif %}
7  | 
8  | {% block navigation %}
9  |     <div id="navigation">
10 |     {% if not progress.isFirst(page.path) %}
11 |        <a class="nav nav-prev" href="{{ progress.nextSibling(page.path).url|e }}"> <i class="fa fa-chevron-left"></i></a>
12 |     {% endif %}
13 |
14 |     {% if not progress.isLast(page.path) %}
15 |        <a class="nav nav-next" href="{{ progress.prevSibling(page.path).url|e }}"><i class="fa fa-chevron-right"></i></a>
16 |     {% endif %}
17 |     </div>
18 | {% endblock %}
```

`nextSibling()` est en haut de la liste et `prevSibling(`) en bas de la liste, voici comment cela fonctionne :

En supposant que vous ayez les pages :

```console
1 | Projet A
2 | Projet B
3 | Projet C
```

Vous êtes sur le projet A, la page précédente est le projet B. Si vous êtes sur le projet B, la page précédente est le projet C et la suivante est le projet A.

<h2 id="Collections programmatiques">Collections programmatiques
<a href="#Collections programmatiques" class="toc-anchor after"></a></h2>

Vous pouvez prendre le contrôle total des collections directement depuis PHP dans les plugins Grav, les thèmes ou même depuis Twig. Il s'agit d'une approche plus codée en dur par rapport à leur définition dans le frontmatter de votre page, mais elle permet également une logique de collecte plus complexe et flexible.

<h3 id="Collections PHP">Collections PHP
<a href="#Collections PHP" class="toc-anchor after"></a></h3>

Vous pouvez effectuer une collection logique avancée avec PHP, par exemple :

```php
1 | $collection = new Collection($pages);
2 | $collection->setParams(['taxonomies' => ['tag' => ['dog', 'cat']]])->dateRange('01/01/2016', '12/31/2016')->published()->ofType('blog-item')->order('date', 'desc');
3 | 
4 | $titles = [];
5 | 
6 | foreach ($collection as $page) {
7 |    $titles[] = $page->title();
8 | }
```
La fonction `order()` peut également, en plus des paramètres `by` et `dir`, prendre un paramètre manual et `sort_flags`. Ceux-ci sont [documentés ci-dessus](#Options de commande). Vous pouvez également utiliser la même méthode `évalue()` que celle utilisée par les collections de pages basées sur le frontmatter :

```php
1 | $page = Grav::instance()['page'];
2 | $collection = $page->evaluate(['@page.children' => '/blog', '@taxonomy.tag' => 'photography']);
3 | $ordered_collection = $collection->order('date', 'desc');
```

Et un autre exemple de commande personnalisée serait :

    1 | $collection_commandée = $collection->commande('header.price','asc',null,SORT_NUMERIC);

Vous pouvez également faire la même chose directement dans **Twig Templates** :

```html
1 | {% set collection = page.evaluate([{'@page.children':'/blog', '@taxonomy.tag':'photography'}]) %}
2 | {% set order_collection = collection.order('date','desc') %}
```

<h3 id="Collections avancées">Collections avancées
<a href="#Collections avancées" class="toc-anchor after"></a></h3>

Par défaut, lorsque vous appelez `page.collection()` dans le Twig d'une page qui a une collection définie dans l'en-tête, Grav recherche une collection appelée `content`. Cela permet de définir [plusieurs collections](#Collections multiples), mais vous pouvez même aller plus loin.

Si vous avez besoin de générer par programmation une collection, vous pouvez le faire en appelant `page.collection()` et en transmettant un tableau au même format que la définition de collection d'en-tête de page. Par exemple:

```html
1 | {% set options = { items: {'@page.children': '/my/pages'}, 'limit': 5, 'order': {'by': 'date', 'dir': 'desc'}, 'pagination': true } %}
2 | {% set my_collection = page.collection(options) %}
3 |    
4 | <ul>
5 | {% for p in my_collection %}
6 | <li>{{ p.title|e }}</li>
7 | {% endfor %}
8 | </ul>
```

Génération du menu pour l'ensemble du site (vous devez définir la propriété du *menu* dans le frontmatter de la page) :

```html
1  | ---
2  | titre : Home
3  | Menu : Home
4  | ---


1  | {% set options = { items: {'@root.descendants':''}, 'order': {'by': 'folder', 'dir': 'asc'}} %}
2  | {% set my_collection = page.collection(options) %}
3  |
4  | {% for p in my_collection %}
5  | {% if p.header.menu %}
6  |   <ul>
7  |   {% if page.slug == p.slug %}
8  |     <li class="{{ p.slug|e }} active"><span>{{ p.menu|e }}</span></li>
9  |   {% else %}
10 |     <li class="{{ p.slug|e }}"><a href="{{ p.url|e }}">{{ p.menu|e }}</a></li>
11 |   {% endif %}
12 |   </ul>
13 | {% endif %}
14 | {% endfor %}
```

<h3 id="Pagination avec les collections avancées">Pagination avec les collections avancées
<a href="#Pagination avec les collections avancées" class="toc-anchor after"></a></h3>

Une question courante que nous entendons est de savoir comment activer la pagination pour les collections personnalisées. Pagination est un plugin qui peut être installé via GPM avec le nom `pagination`. Une fois installé, il fonctionne "prêt à l'emploi" avec les collections configurées par page, mais ne sait rien des collections personnalisées créées dans Twig. Pour faciliter ce processus, la pagination est livrée avec sa propre fonction Twig appelée `paginate()` qui fournira la fonctionnalité de pagination dont nous avons besoin.

Après avoir transmis la collection et la limite à la fonction `paginate()`, nous devons également transmettre les informations de pagination directement au modèle `partials/pagination.html.twig` pour un rendu correct.

```html
1  | {% set options = { items: {'@root.descendants':''}, 'order': {'by': 'folder', 'dir': 'asc'}} %}
2  | {% set my_collection = page.collection(options) %}
3  | {% do paginate( my_collection, 5 ) %}
4  |
5  | {% for p in my_collection %}
6  |     <ul>
7  |          {% if page.slug == p.slug %}
8  |              <li class="{{ p.slug|e }} active"><span>{{ p.menu|e }}</span></li>
9  |          {% else %}
10 |              <li class="{{ p.slug|e }}"><a href="{{ p.url|e }}">{{ p.menu|e }}</a></li>
11 |          {% endif %}
12 |    </ul>
13 | {% endfor %}
14 |
15 | {% include 'partials/pagination.html.twig' with {'base_url':page.url, 'pagination':my_collection.params.pagination} %}
```

<h3 id="Gestion personnalisée des collections avec l'événement onCollectionProcessed()">Gestion personnalisée des collections avec l'événement <code>onCollectionProcessed()</code>
<a href="#Gestion personnalisée des collections avec l'événement onCollectionProcessed()" class="toc-anchor after"></a></h3>

Il y a des moments où les options d'événement ne suffisent pas. Parfois, lorsque vous souhaitez obtenir une collection, mais que vous manipulez ensuite la collection en fonction de quelque chose de très personnalisé. Imaginez si vous voulez, un cas d'utilisation où vous avez ce qui semble être une liste de blogs plutôt standard, mais votre client veut avoir un contrôle précis sur ce qui s'affiche dans la liste. Ils veulent avoir une bascule personnalisée sur chaque élément de blog qui leur permet de le supprimer de la liste, tout en le faisant publier et disponible via un lien direct.

Pour ce faire, nous pouvons simplement ajouter une option personnalisée `display_in_listing : false` dans l'en-tête de page de l'élément :

```yaml
1 | ---
2 | title: 'My Story'
3 | date: '13:34 04/14/2020'
4 | taxonomy:
5 |    tag:
6 |       - journal
7 |  display_in_listing: false
8 |  ---
9 |  ...
```  
   
Le problème est qu'il n'y a aucun moyen de définir ou d'inclure ce filtre lors de la définition d'une collection dans la page de liste. Il est probablement défini quelque chose comme ceci :

```yaml
1  | ---
2  | menu: News
3  | title: 'My Blog'
4  | content:
5  |     items:
6  |        - self@.children
7  |     order:
8  |        by: date
9  |        dir: desc
10 |     limit: 8
11 |     pagination: true
12 |     url_taxonomy_filters: true
13 | ---
14 | ...
```

Ainsi, la collection est simplement définie par la directive `self@.children` pour obtenir tous les enfants publiés de la page en cours. Alors qu'en est-il des pages qui ont le `display_in_listing: false` défini ? Nous devons faire un travail supplémentaire sur cette collection avant qu'elle ne soit retournée pour nous assurer que nous supprimons tous les éléments que nous ne voulons pas voir. Pour ce faire, nous pouvons utiliser l'événement `onCollectionProcessed()` dans un plugin personnalisé. Nous devons ajouter l'écouteur :

```php
1 | public static function getSubscribedEvents(): array
2 |   {
3 |     return [
4 |        ['autoload', 100000],
5 |         'onPluginsInitialized' => ['onPluginsInitialized', 0],
6 |         'onCollectionProcessed' => ['onCollectionProcessed', 10]
7 |      ];
8 |   }
```

Ensuite, nous devons définir la méthode et parcourir les éléments de la collection, en recherchant toutes les pages avec ce champ `display_in_listing :` défini, puis le supprimer s'il est `false` :

```php
1  | /**
2  |  * Supprimez n'importe quelle page de la collection avec display_in_listing : false|0
3  |  *
4  |  * @param Event $event
5  |  */
6  |  public function onCollectionProcessed(Event $event): void
7  |  {
8  |     /** @var Collection $collection */
9  |     $collection = $event['collection'];
10 |        
11 |     foreach ($collection as $item) {
12 |        $display_in_listing = $item->header()->display_in_listing ?? true;
13 |        if ((bool) $display_in_listing === false) {
14 |           $collection->remove($item->path());
15 |        }
16 |     }
17 |
18 |  }
```

Maintenant, votre collection contient les bons éléments, et tous les autres plugins ou modèles Twig qui s'appuient sur cette collection verront cette collection modifiée afin que des choses comme la pagination fonctionnent comme prévu.

