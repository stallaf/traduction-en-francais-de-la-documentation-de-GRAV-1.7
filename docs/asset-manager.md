<h1 class="rem">Gestionnaire d'actifs</h1>

Dans Grav 1.6, le **Gestionnaire d'Actifs** a été complètement réécrit pour fournir un mécanisme plus flexible de gestion des actifs **CSS** et **JavaScript** dans les thèmes. L'objectif principal du gestionnaire d'actifs est de simplifier le processus d'ajout d'actifs à partir de thèmes et de plug-ins tout en offrant des fonctionnalités améliorées telles que la priorité et en fournissant un **Pipeline d'Actifs** pouvant être utilisé pour **réduire**, **compresser** et intégrer des actifs afin de réduire le nombre de requêtes de navigateur. , ainsi que la taille globale des actifs.

C'est beaucoup plus flexible et plus fiable qu'auparavant. De plus, c'est considérablement plus "propre" et plus facile à suivre si vous commencez à parcourir le code. Le gestionnaire d'actifs est disponible dans Grav et est accessible dans les crochets d'événement du plugin, mais aussi directement dans les thèmes via des appels Twig.

<div class="notice note">
<strong>Détails techniques</strong> : la classe d'actifs principale a été considérablement simplifiée et réduite. Une grande partie de la logique a été divisée en 3 traits. Un <em>trait de test</em> qui contient des fonctions principalement utilisées dans notre suite de tests, un <em>trait utils</em> qui contient des méthodes partagées entre les types d'actifs réguliers (js, inline_js, css, inline_css) et le pipeline d'actifs qui peut minifier et compresser, et enfin un <em>trait hérité</em> qui contient des méthodes qui sont des raccourcis ou des solutions de contournement, et ne doivent généralement pas être utilisées à l'avenir.
</div>

<div class="notice tip">
Le gestionnaire d'actifs est entièrement rétrocompatible avec la syntaxe utilisée dans les versions antérieures à Grav 1.6, cependant, la documentation ci-dessous couvrira la nouvelle <strong>syntaxe préférée</strong>.
</div>

<h2 id="Configuration">Configuration
<a href="#Configuration" class="toc-anchor after"></a></h2> 

Le Grav Asset Manager dispose d'un ensemble simple d'options de configuration. Les valeurs par défaut se trouvent dans le fichier système `system.yaml`, mais vous pouvez remplacer tout ou partie d'entre elles dans votre fichier `user/config/system.yaml` :

```yaml
1  | assets:                                         # Configuration for Assets Manager (JS, CSS)
2  |     css_pipeline: false                         # The CSS pipeline is the unification of multiple CSS resources into one file
3  |     css_pipeline_include_externals: true        # Include external URLs in the pipeline by default
4  |     css_pipeline_before_excludes: true          # Render the pipeline before any excluded files
5  |     css_minify: true                            # Minify the CSS during pipelining
6  |     css_minify_windows: false                   # Minify Override for Windows platforms, also applies to js. False by default due to ThreadStackSize
7  |     css_rewrite: true                           # Rewrite any CSS relative URLs during pipelining
8  |     js_pipeline: false                          # The JS pipeline is the unification of multiple JS resources into one file
9  |     js_pipeline_include_externals: true         # Include external URLs in the pipeline by default
10 |     js_pipeline_before_excludes: true           # Render the pipeline before any excluded files
11 |     js_module_pipeline: false                   # The JS Module pipeline is the unification of multiple JS Module resources into one file
12 |     js_module_pipeline_include_externals: true  # Include external URLs in the pipeline by default
13 |     js_module_pipeline_before_excludes: true    # Render the pipeline before any excluded files
14 |     js_minify: true                             # Minify the JS during pipelining
15 |     enable_asset_timestamp: false               # Enable asset timestamps
16 |     collections:
17 |        jquery: system://assets/jquery/jquery-2.x.min.js
```

<h2 id="Structure">Structure
<a href="#Structure" class="toc-anchor after"></a></h2>

Il existe plusieurs niveaux de contrôle de positionnement, comme indiqué dans le schéma ci-dessous. Dans l'ordre de portée, ils sont:

* **Group** - permet le regroupement d'actifs tels que la tête (par défaut) et le bas
* **Position** - `before`, `pipeline` (par défaut) et `after`. Fondamentalement, cela vous permet de spécifier où dans le groupe l'actif doit être chargé.
* **Priority** - Ceci contrôle **l'ordre**, où les entiers plus grands (par exemple `100`) seront sortis avant les entiers inférieurs. `10` est la valeur par défaut.

<div class="content3 pre">
<pre>
CSS
┌───────────────────────┐
│ Group (head)          │
│┌─────────────────────┐│       ┌──────────────────┐
││ Position            ││       │   priority 100   │───┐    ┌───────────┐
││┌───────────────────┐││       ├──────────────────┤   ├───>│   CSS     │
│││                   │││       │   priority 99    │───┤    └───────────┘
│││      before       │├┼──┬───>├──────────────────┤   │
│││                   │││  │    │    priority 1    │───┤    ┌───────────┐
││├───────────────────┤││  │    ├──────────────────┤   ├───>│inline CSS │
│││                   │││  │    │    priority 0    │───┘    └───────────┘
│││     pipeline      │├┼──┤    └──────────────────┘
│││                   │││  │
││├───────────────────┤││  │
│││                   │││  │
│││       after       │├┼──┘
│││                   │││
││└───────────────────┘││
│└─────────────────────┘│
└───────────────────────┘

JS
┌───────────────────────┐
│ Group (head)          │
│┌─────────────────────┐│       ┌──────────────────┐
││ Position            ││       │   priority 100   │───┐     ┌──────────┐
││┌───────────────────┐││       ├──────────────────┤   ├───> │   JS     │
│││                   │││       │   priority 99    │───┤     └──────────┘
│││      before       │├┼──┬───>├──────────────────┤   │
│││                   │││  │    │    priority 1    │───┤     ┌──────────┐
││├───────────────────┤││  │    ├──────────────────┤   ├───> │inline JS │
│││                   │││  │    │    priority 0    │───┘     └──────────┘
│││     pipeline      │├┼──┤    └──────────────────┘
│││                   │││  │
││├───────────────────┤││  │
│││                   │││  │
│││       after       │├┼──┘
│││                   │││
││└───────────────────┘││
│└─────────────────────┘│
└───────────────────────┘

JS Module
┌───────────────────────┐
│ Group (head)          │
│┌─────────────────────┐│       ┌──────────────────┐
││ Position            ││       │   priority 100   │───┐    ┌───────────┐
││┌───────────────────┐││       ├──────────────────┤   ├───>│ JS Module │
│││                   │││       │   priority 99    │───┤    └───────────┘
│││      before       │├┼──┬───>├──────────────────┤   │
│││                   │││  │    │    priority 1    │───┤    ┌────────────────┐
││├───────────────────┤││  │    ├──────────────────┤   ├───>│inline JS Module│
│││                   │││  │    │    priority 0    │───┘    └────────────────┘
│││     pipeline      │├┼──┤    └──────────────────┘
│││                   │││  │
││├───────────────────┤││  │
│││                   │││  │
│││       after       │├┼──┘
│││                   │││
││└───────────────────┘││
│└─────────────────────┘│
└───────────────────────┘
</pre>
</div>
<br>

Par défaut, `CSS`, `JS` et `JS Module` s'affichent par défaut dans la position du `pipeline` lors de leur sortie. Alors que `InlineCSS`, `InlineJS` et `Inline JS Module` sont par défaut dans la position `after`. Ceci est cependant configurable et vous pouvez définir n'importe quel actif dans n'importe quelle position.

<h2 id="Assets dans les thèmes">Assets dans les thèmes
<a href="#Assets dans les thèmes" class="toc-anchor after"></a></h2>

<h3 id="Aperçu">Aperçu
<a href="#Aperçu" class="toc-anchor after"></a></h3>

En général, vous ajoutez des actifs CSS un par un à l'aide des appels `assets.addCss()` ou `assets.addInlineCss()`, puis restituez ces actifs via `assets.css()`. Les options contrôlant la priorité, le pipelining ou l'inlining peuvent être spécifiées par ressource lors de son ajout, ou au moment du rendu pour un groupe de ressources.

Les ressources JS sont gérées de la même manière avec les appels `assets.addJs()` et `assets.addInlineJs()`. Il existe également une méthode générique `assets.add()` qui essaie de deviner le type d'actif que vous ajoutez, mais il est recommandé d'utiliser les appels de méthode plus spécifiques.

Depuis la version 1.7.27, Grav's Assets Manager prend également en charge les modules JS. Ces actifs fonctionnent exactement comme les actifs JS mais leur type est `type="module"` et ils sont gérés avec des appels `assets.addJsModule()` et `assets.addInlineJsModule()`. La méthode générique `assets.add()` ne reviendra au module JS que si l'extension détectée est `.mjs`. Sinon, tout fichier `.js` sera traité comme JS normal.

<div class="notice note">
Pour en savoir plus sur les modules JS
<p>
<ul>
  <li><a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules]" title="Lien  mort">https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules</a></li>
  <li><a href="https://v8.dev/features/modules">https://v8.dev/features/modules</a></li>
  <li><a href="https://javascript.info/modules-intro">https://javascript.info/modules-intro</a></li>
</p>
</div>

L'Asset Manager prend également en charge :

* ajouter des actifs à des groupes nommés afin de rendre ces groupes à différents endroits et/ou avec différents ensembles d'options,
* configurer des collections de ressources nommées, qui peuvent être ajoutées en un seul appel `assets.add*()`.

<h3 id="Exemple">Exemple
<a href="#Exemple" class="toc-anchor after"></a></h3>

Un exemple de la façon dont vous pouvez ajouter des fichiers CSS dans votre thème peut être trouvé dans le thème **quark** par défaut fourni avec Grav. Si vous regardez le partiel `templates/partials/base.html.twig`, vous verrez quelque chose de similaire à ce qui suit :

```
1  | <!DOCTYPE html>
2  | <html>
3  |     <head>
4  |     ...
5  | 
6  |     {% block stylesheets %}
7  |         {% do assets.addCss('theme://css-compiled/spectre.css') %}
8  |         {% do assets.addCss('theme://css-compiled/theme.css') %}
9  |         {% do assets.addCss('theme://css/custom.css') %}
10 |         {% do assets.addCss('theme://css/line-awesome.min.css') %}
11 |     {% endblock %}
12 | 
13 |     {% block javascripts %}
14 |         {% do assets.addJs('jquery', 101) %}
15 |         {% do assets.addJs('theme://js/jquery.treemenu.js', { group: 'bottom' }) %}
16 |         {% do assets.addJs('theme://js/site.js', { group: 'bottom' }) %}
17 |         {% do assets.addJsModule('plugin://my_plugin/app/main.js', { group: 'bottom' }) %}
18 |     {% endblock %}
19 | 
20 |     {% block assets deferred %}
21 |         {{ assets.css()|raw }}
22 |         {{ assets.js()|raw }}
23 |     {% endblock %}
24 |     </head>
25 | 
26 |     <body>
27 |     ...
28 | 
29 |     {% block bottom %}
30 |         {{ assets.js('bottom')|raw }}
31 |     {% endblock %}
32 |     </body>
33 | </html>
```

La balise twig `block stylesheets` définit simplement une région qui peut être remplacée ou ajoutée dans les modèles qui étendent celle-ci. Dans le bloc, vous verrez un certain nombre d'appels à `do assets.addCss()`.

La balise `{% do %} `est en fait [intégrée à Twig](https://twig.symfony.com/doc/1.x/tags/do.html) lui-même, et elle vous permet de manipuler des variables sans générer de sortie.

La méthode `addCss()` ajoute des assets CSS au Asset Manager. Si vous spécifiez un deuxième paramètre numérique, cela définit la priorité de la feuille de style. Si vous ne spécifiez pas de priorité, la priorité à laquelle les ressources sont ajoutées dictera l'ordre dans lequel elles sont rendues. Vous remarquerez l'utilisation d'un **wrapper de flux PHP** `theme://` pour fournir à Grav un moyen simple de déterminer le chemin relatif du thème actuel.

<div class="notice info">
Le fichier <code>assets.addJs('jquery', 101)</code> inclura la collection <code>jquery</code> définie dans la configuration globale des actifs. Le paramètre facultatif ici de <code>101</code> définit la priorité sur une valeur assez élevée pour s'assurer qu'il est rendu en premier. La priorité par défaut lorsqu'elle n'est pas fournie est une valeur de <code>10</code>. Une manière plus flexible d'écrire ceci serait <code>assets.addJs('jquery', {priority: 101})</code>. Cela vous permet d'ajouter d'autres paramètres à côté de la priorité.
</div>

L'appel `assets.css()|raw` restitue les ressources CSS sous forme de balises HTML. Comme il n'y a pas de paramètre fourni à cette méthode, le groupe est défini par défaut sur `head`. Notez comment cela est enveloppé dans un bloc `assets defered`. Il s'agit d'une nouvelle fonctionnalité de Grav 1.6 qui vous permet d'ajouter des ressources à partir d'autres modèles Twig qui sont inclus plus bas dans la page (ou n'importe où en réalité), tout en garantissant qu'ils peuvent s'afficher dans ce bloc `head` si nécessaire.

Le bloc `bottom` à la toute fin de la sortie de votre thème rend le JavaScript qui a été attribué au groupe `bottom`.

<h2 id="Ajout d'actifs">Ajout d'actifs
<a href="#Ajout d'actifs" class="toc-anchor after"></a></h2>

<h4 id="add(asset, [options])">add(asset, [options])
<a href="#add(asset, [options])" class="toc-anchor after"></a></h4>

La méthode d'ajout fait de son mieux pour faire correspondre un actif en fonction de l'extension de fichier. C'est une méthode de commodité, il est préférable d'appeler l'une des méthodes directes pour CSS, Link, JS et JS Module. Voir les méthodes directes pour plus de détails.

<div class="notice info">
Le tableau d'options est l'approche préférée pour passer plusieurs options. Cependant, comme dans l'exemple précédent avec <code>jquery</code>, vous pouvez utiliser un raccourci et passer un entier pour le <strong>deuxième argument</strong> de la méthode si tout ce que vous souhaitez définir est la <strong>priorité</strong>.
</div>

<h4 id="addCss(asset, [options])">addCss(asset, [options])
<a href="#addCss(asset, [options])" class="toc-anchor after"></a></h4>

Cette méthode ajoutera des actifs à la liste des actifs CSS. La priorité par défaut est 10 si elle n'est pas fournie. Un nombre plus élevé signifie qu'il s'affichera avant les éléments de priorité inférieure. L'option de `pipeline` contrôle si cet actif doit être inclus dans le pipeline de combinaison/réduction. S'il n'est pas en pipeline, l'option `loading` contrôle si la ressource doit être rendue sous forme de lien vers une feuille de style externe ou si son contenu doit être intégré dans une balise de style en ligne.

<h4 id="addLink($asset, [options])">addLink($asset, [options])
<a href="#addLink($asset, [options])" class="toc-anchor after"></a></h4>

Cette méthode ajoutera des actifs aux actifs de Link, sous la forme d'une balise `<link>`. Il est utile pour ajouter des balises de lien à la tête de n'importe où sur votre site qui ne sont pas des fichiers CSS. La priorité par défaut est 10 si elle n'est pas fournie. Un nombre plus élevé signifie qu'il s'affichera avant les éléments de priorité inférieure.

Contrairement aux autres méthodes d'ajout d'actifs, `link()` ne prend pas en charge le pipelining, ni ne prend en charge `inline`.

<h4 id="addInlineCss(css, [options])">addInlineCss(css, [options])
<a href="#addInlineCss(css, [options])" class="toc-anchor after"></a></h4>

Vous permet d'ajouter une chaîne de CSS à l'intérieur d'une balise de style en ligne. Utile pour l'initialisation ou tout ce qui est dynamique. Pour incorporer le contenu d'un fichier de ressources standard, consultez l'option `{ 'loading': 'inline' }` des méthodes `addCss()` et `css()`.

<h4 id="addJs(asset, [options])">addJs(asset, [options])
<a href="#addJs(asset, [options]))" class="toc-anchor after"></a></h4>

Cette méthode ajoutera des actifs à la liste des actifs JavaScript. La priorité par défaut est 10 si elle n'est pas fournie. Un nombre plus élevé signifie qu'il s'affichera avant les éléments de priorité inférieure. L'option de `pipeline` contrôle si cet actif doit être inclus dans le pipeline de combinaison/réduction. S'il n'est pas en pipeline, l'option `loading` contrôle si la ressource doit être rendue sous forme de lien vers un fichier de script externe ou si son contenu doit être intégré dans une balise de script en ligne.

<h4 id="addInlineJs(javascript, [options])">addInlineJs(javascript, [options])
<a href="#addInlineJs(javascript, [options])" class="toc-anchor after"></a></h4>

Vous permet d'ajouter une chaîne de caractères JavaScript dans une balise de script en ligne. Utile pour l'initialisation ou tout ce qui est dynamique. Pour incorporer le contenu d'un fichier de ressources standard, consultez l'option `{ 'loading': 'inline' }` des méthodes `addJs()` et `js()`.

<h4 id="addJsModule(asset, [options])">addJsModule(asset, [options])
<a href="#addJsModule(asset, [options])" class="toc-anchor after"></a></h4>

Cette méthode ajoutera des actifs à la liste des actifs des modules JavaScript. La priorité par défaut est 10 si elle n'est pas fournie. Un nombre plus élevé signifie qu'il s'affichera avant les éléments de priorité inférieure. L'option `pipeline` contrôle si cet actif doit être inclus dans le pipeline de combinaison/réduction. S'il n'est pas en pipeline, l'option `loading` contrôle si la ressource doit être rendue sous forme de lien vers un fichier de script externe ou si son contenu doit être intégré dans une balise de script en ligne.

<h4 id="addInlineJsModule(javascript, [options])">addInlineJsModule(javascript, [options])
<a href="addInlineJsModule(javascript, [options])" class="toc-anchor after"></a></h4>

Vous permet d'ajouter une chaîne de caractères JavaScript à l'intérieur d'une balise de script de module en ligne. Pour incorporer le contenu d'un fichier de ressources standard, consultez l'option `{ 'loading': 'inline' }` des méthodes `addJsModule()` et `js()`.

<h4 id="registerCollection(name, array)">registerCollection(name, array)
<a href="#registerCollection(name, array)" class="toc-anchor after"></a></h4>

Vous permet d'enregistrer un tableau d'actifs CSS et JavaScript avec un nom pour une utilisation ultérieure par la méthode `add()`. Particulièrement utile si vous souhaitez enregistrer une collection pouvant être utilisée par plusieurs thèmes ou plugins, tels que jQuery ou Bootstrap.

<h2 id="Options">Options
<a href="#Options" class="toc-anchor after"></a></h2>

Le cas échéant, vous pouvez transmettre un tableau d'options d'actifs. Les options de base sont :

<h4 id="Pour CSS">Pour CSS
<a href="#Pour CSS" class="toc-anchor after"></a></h4>

* **priority** : valeur entière (la valeur par défaut est `10`)
* **position** : le `pipeline` est par défaut mais peut également être `before` ou `after` les actifs en position `pipeline`.
* **loading** : `inline` si cet élément doit être sorti plutôt en ligne (par défaut : référencé via un lien vers la feuille de style). Doit être utilisé en conjonction avec `position : before` ou `position : after` car il n'aura aucun effet avec `position : pipeline` (par défaut).
* **group** : chaîne permettant de spécifier un nom de groupe unique pour l'actif (la valeur par défaut est `head`)

<h4 id="Pour JS et les modules JS">Pour JS et les modules JS
<a href="#Pour JS et les modules JS" class="toc-anchor after"></a></h4>

* **priority** : valeur entière (la valeur par défaut est `10`)
* **position** : le `pipeline` est par défaut mais peut également être avant ou après les actifs en position de pipeline.
* **loading** : prend en charge tout type de chargement, tel que `async`, `defer`, `asyncdefer` ou `inline`. Doit être utilisé en conjonction avec `position : before` ou `position : after` car il n'aura aucun effet avec `position : pipeline` (par défaut).
* **group** : chaîne permettant de spécifier un nom de groupe unique pour l'actif (la valeur par défaut est head)

<h4 id="Autres attributs">Autres attributs
<a href="#Autres attributs" class="toc-anchor after"></a></h4>

Vous pouvez également passer tout ce que vous voulez dans le tableau d'options, et s'il ne s'agit pas de ces types standard, ils seront simplement rendus sous forme d'attributs tels que `{id : 'custom-id'}` sera rendu comme `id="custom-id"` dans la balise HTML. Cela peut également être utilisé pour inclure des données structurées telles que json-ld via `addInlineJs()` en utilisant `{type : 'application/ld+json'}`.

<h4 id="Exemples">Exemples
<a href="#Exemples" class="toc-anchor after"></a></h4>

Par exemple:

```twig
{% do assets.addCss('page://01.blog/assets-test/example.css?foo=bar',
   { priority: 20, loading: 'inline', position: 'before'}) %}
```

Rendu comme suit :

```css
1 | <style>
2 | h1.clignotant {
3 |     décoration de texte : souligné ;
4 | }
5 | </style>
6 | <lien.....
```

Un autre exemple:

```twig
{% do assets.addJs('page://01.blog/assets-test/example.js',
   {loading: 'async', id: 'custom-css'}) %}
```
    
Rendu comme suit :

```html
<script src="/grav/user/pages/01.blog/assets-test/example.js" async id="custom-css"></script>`
```

Un exemple de lien :

```twig
{% do assets.addLink('theme://images/favicon.png', { rel: 'icon', type: 'image/png' }) %}
{% do assets.addLink('plugin://grav-plugin/build/js/vendor.js', { rel: 'modulepreload' }) %}
```
    
Rendu comme suit :

```html
<link rel="icon" type="image/png" href="/user/themes/quark/images/favicon.png">
<link href="/user/plugins/grav-plugin/build/js/vendor.js" rel="modulepreload">
```

<h4 id="Actifs de rendu">Actifs de rendu
<a href="#Actifs de rendu" class="toc-anchor after"></a></h4>

Les éléments suivants vous permettent de rendre l'état actuel des actifs CSS et JavaScript.

<h4 id="css(group, [options], include_link = true)">css(group, [options], include_link = true)
<a href="#css(group, [options], include_link = true)" class="toc-anchor after"></a></h4>

Rend les actifs CSS qui ont été ajoutés au groupe d'un gestionnaire d'actifs (la valeur par défaut est `head`). Les options sont :

* **loading** : `inline` si **tous** les éléments de ce groupe doivent être alignés (par défaut : afficher chaque élément en fonction de son option de `position`).
* **_link attribute_** : attributs de lien, voir ci-dessous (par défaut : `{'type' : 'text/css', 'rel' : 'stylesheet'}`). Efficace uniquement si `inline` n'est pas utilisé comme option de rendu de ce groupe.

Lorsque `include_link` est activé, ce qui est le cas par défaut, l'appel de `css()` se propagera également à l'appel de `link()`.

Si le pipelining est **désactivé** dans la configuration, les actifs du groupe sont rendus individuellement, classés par priorité d'actif (de haut en bas), suivis de l'ordre dans lequel les actifs ont été ajoutés.

Si le pipelining est **activé** dans la configuration, les actifs dans la position du pipeline sont combinés dans l'ordre dans lequel les actifs ont été ajoutés, puis traités en fonction de la configuration du pipeline.

Chaque élément est rendu soit sous forme de lien de feuille de style, soit en ligne, selon l'option `loading` de l'élément et si `loading' : 'inline'}` est utilisé pour le rendu de ce groupe. Le CSS ajouté par `addInlineCss()` sera rendu dans la position `after` par défaut, mais vous pouvez le configurer pour qu'il soit rendu avant la sortie en pipeline avec la `position : before`.

<h4 id="link(group, [options]">link(group, [options]
<a href="#link(group, [options]" class="toc-anchor after"></a></h4>

Renders Link assets qui ont été ajoutés au groupe d'un Asset Manager (la valeur par défaut est `head`). Il n'est pas recommandé d'utiliser un groupe différent de head, c'est là que le navigateur s'attend à ce que la balise soit trouvée et traitée.

Contrairement aux autres méthodes d'ajout d'actifs, `link()` ne prend pas en charge le pipelining, ni ne prend en charge `inline`.

<h4 id="js(group, [options], include_js_module = true)">js(group, [options], include_js_module = true)
<a href="#js(group, [options], include_js_module = true)" class="toc-anchor after"></a></h4>

Rend les actifs JavaScript qui ont été ajoutés au groupe d'un gestionnaire d'actifs (la valeur par défaut est `head`). Les options sont :

* **loading** : `inline` si **tous** les éléments de ce groupe doivent être alignés (par défaut : afficher chaque élément en fonction de son option de `position`).
* **_script attributes_**, voir ci-dessous (par défaut : `{'type' : 'text/javascript'}`). Efficace uniquement si `inline` **n'est pas** utilisé comme option de rendu de ce groupe.

Lorsque `include_js_module` est activé, ce qui est le cas par défaut, l'appel de `js()` se propagera également à l'appel de `jsModule()`.

Si le pipelining est **désactivé** dans la configuration, les actifs du groupe sont rendus individuellement, classés par priorité d'actif (de haut en bas), suivis de l'ordre dans lequel les actifs ont été ajoutés.

Si le pipelining est **activé** dans la configuration, les actifs dans la position du pipeline sont combinés dans l'ordre dans lequel les actifs ont été ajoutés, puis traités en fonction de la configuration du pipeline. Le résultat du pipeline combiné est ensuite rendu avant ou après les actifs non pipelinés en fonction du paramètre de `js_pipeline_before_excludes`.

Chaque élément est rendu sous forme de lien de script ou en ligne, selon l'option `loading` de l'élément et si `{'loading' : 'inline'}` est utilisé pour le rendu de ce groupe. Notez que la seule façon d'intégrer un pipeline JS est d'utiliser le chargement en ligne comme option de la méthode `js()`. JS ajouté par `addInlineJs()` sera rendu dans la position après par défaut, mais vous pouvez le configurer pour qu'il soit rendu avant la sortie en pipeline avec la `position : before`.

<h4 id="jsModule(group, [options])">jsModule(group, [options])
<a href="#jsModule(group, [options])" class="toc-anchor after"></a></h4>

Fonctionne exactement comme le moteur de rendu `js()`, mais pour les modules JavaScript. L'attribut de type de script par défaut est `type="module"`, même lors du rendu `inline`.

<h4 id="all(group, [options])">all(group, [options])
<a href="#all(group, [options])" class="toc-anchor after"></a></h4>

Rend chaque élément ci-dessus dans l'ordre : `css()`, `link()`, `js()`, `jsModule()`.

C'est la méthode recommandée pour inclure les actifs différés dans votre fichier twig principal (généralement `base.html.twig`).

```twig
1 | {% block assets deferred %}
2 |   {{ assets.all()|raw }}
3 | {% endblock %}
```

<h2 id="Actifs et collections nommés">Actifs et collections nommés
<a href="#Actifs et collections nommés" class="toc-anchor after"></a></h2>

Grav dispose désormais d'une fonctionnalité puissante appelée **named assts** qui vous permet d'enregistrer une collection d'actifs CSS et JavaScript avec un nom. Ensuite, vous pouvez simplement **ajouter** ces actifs au gestionnaire d'actifs via le nom avec lequel vous avez enregistré la collection. Grav est préconfiguré avec **jQuery** mais a la capacité de définir des collections personnalisées dans le `system.yaml` à utiliser par n'importe quel thème ou plugin :

```yaml
1 | assets:
2 |   collections:
3 |     jquery: system://assets/jquery/jquery-2.1.3.min.js
4 |     bootstrap:
5 |         - https://maxcdn.bootstrapcdn.com/bootstrap/3.3.4/css/ bootstrap.min.css
6 |         - https://maxcdn.bootstrapcdn.com/bootstrap/3.3.4/css/bootstrap-theme.min.css
7 |         - https://maxcdn.bootstrapcdn.com/bootstrap/3.3.4/js/bootstrap.min.js
```

Vous pouvez également utiliser la méthode `registerCollection()` par programmation.

```php
1 | $assets = $this->grav['assets'];
2 | $bootstrapper_bits = [https://maxcdn.bootstrapcdn.com/bootstrap/3.3.4/css/bootstrap.min.css,
3 |                      https://maxcdn.bootstrapcdn.com/bootstrap/3.3.4/css/bootstrap-theme.min.css,
4 |                      https://maxcdn.bootstrapcdn.com/bootstrap/3.3.4/js/bootstrap.min.js];
5 | $assets->registerCollection('bootstrap', $bootstrap_bits);
6 | $assets->add('bootstrap', 100);
```

Un exemple de cette action peut être trouvé dans le [**plugin** bootstrapper](https://github.com/getgrav/grav-plugin-bootstrapper/blob/develop/bootstrapper.php#L51-L71).

<h4 id="Collections avec attributs">Collections avec attributs
<a href="#Collections avec attributs" class="toc-anchor after"></a></h4>

Parfois, vous souhaiterez peut-être spécifier des attributs personnalisés et/ou différents pour des éléments spécifiques d'une collection, par exemple si vous chargez des actifs à partir d'un CDN distant et que vous souhaitez inclure le contrôle d'intégrité (SRI). Ceci est possible en traitant la valeur de l'actif nommé comme un tableau où la clé est l'emplacement de l'actif et la valeur est la liste des attributs supplémentaires. Par example:

```yaml
1 | assets:
2 |   collections:
3 |     jquery_and_ui:
4 |        https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js:
5 |            integrity: 'sha512-894YE6QWD5I59HgZOGReFYm4dnWc1Qt5NtvYSaNcOP+u1T9qYdvdihz0PPSiiqn/+/3e7Jo4EaG7TubfWGUrMQ=='
6 |            group: 'bottom'
7 |        https://cdnjs.cloudflare.com/ajax/libs/jqueryui/1.12.1/jquery-ui.min.js:
8 |            integrity: 'sha512-uto9mlQzrs59VwILcLiRYeLKPPbS/bT71da/OEBYEwcdNUk8jYIy+D176RYoop1Da+f9mvkYrmj5MCLZWEtQuA=='
9 |            group: 'bottom'
```

Ensuite, après avoir ajouté le JS dans votre twig via `{% do assets.addJs('jquery_and_ui', { defer: true }) %}`, les actifs se chargeront comme :

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js" defer="1"
integrity="sha512-894YE6QWD5I59HgZOGReFYm4dnWc1Qt5NtvYSaNcOP+u1T9qYdvdihz0PPSiiqn/+/3e7Jo4EaM7TubfWG7Tubf= ">
</script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jqueryui/1.12.1/jquery-ui.min.js" defer="1"
integrity="sha512-uto9mlQzrs59VwILcLiRYeLKPPbS/bT71da/OEBYEwcdNUk8jYIy+D176RYoop1Da +f9mvkYrmj5MCLZWEtQuA==">
</script>
```

Notez que `defer` a été défini au niveau de la brindille et qu'il a été appliqué à tous les actifs de la collection. En effet, Grav fusionnera les attributs de la définition twig et yaml, en donnant la priorité à ceux de la définition yaml.

Si l'actif `jquery-ui.min.js` incluait également un attribut `defer : null`, il aurait alors pris le pas sur le twig `defer : 1` et il n'aurait pas été rendu.

<h2 id="Actifs groupés">Actifs groupés
<a href="#Actifs groupés" class="toc-anchor after"></a></h2>

Le gestionnaire d'actifs vous permet de transmettre un `group` facultatif dans le cadre d'un tableau d'options lors de l'ajout d'actifs. Bien que cela soit d'une utilité marginale pour CSS, il est particulièrement utile pour JavaScript où vous devrez peut-être avoir des fichiers JS ou Inline JS référencés dans l'en-tête, et d'autres au bas de la page.

Pour tirer parti de cette fonctionnalité, vous devez spécifier le groupe lors de l'ajout de l'actif et utiliser la syntaxe des options :

    {% do assets.addJs('theme://js/example.js', {'priority':102, 'group':'bottom'}) %}

Ensuite, pour que ces actifs du groupe inférieur s'affichent, vous devez ajouter les éléments suivants à votre thème :

    {{ assets.js('bottom')|raw }}

Si aucun groupe n'est défini pour un actif, `head` est le groupe par défaut. Si aucun groupe n'est défini pour le rendu, le groupe `head` sera rendu. Cela garantit que la nouvelle fonctionnalité est 100 % rétrocompatible avec les thèmes existants.

Il en va de même pour les fichiers CSS :

    {% do assets.addCss('theme://css/ie8.css', {'group':'ie'}) %}

rendu comme suit :

    {{ assets.css('ie')|raw }}

<h2 id="Modifier l'attribut des éléments CSS/JS rendus">Modifier l'attribut des éléments CSS/JS rendus
<a href="#Modifier l'attribut des éléments CSS/JS rendus" class="toc-anchor after"></a></h2>

CSS est ajouté par défaut en utilisant l'attribut `rel="stylesheet"` et `type="text/css"` , tandis que JS a `type="text/javascript"`.

Pour modifier les valeurs par défaut ou pour ajouter de nouveaux attributs, vous devez créer un nouveau groupe d'actifs et dire à Grav de le rendre avec cet attribut.

Exemple de modification de l'attribut `rel` sur un groupe d'assets :

```twig
1 | {% do assets.addCSS('theme://whatever.css', {'group':'my-alternate-group'}) %}
2 | ...
3 | {{ assets.css('my-alternate-group', {'rel': 'alternate'})|raw }}
```

<h2 id="Inlining Assets">Inlining Assets
<a href="#Inlining Assets" class="toc-anchor after"></a></h2>

L'inlining permet de placer le code CSS (et JS) critique directement dans le document HTML et permet au navigateur d'afficher une page immédiatement sans attendre les téléchargements de feuilles de style ou de scripts externes. Cela peut améliorer sensiblement les performances du site pour les utilisateurs, en particulier sur les réseaux mobiles. Vous trouverez des détails dans [cet article sur l'optimisation de la livraison CSS](https://developers.google.com/speed/docs/insights/OptimizeCSSDelivery).

Cependant, l'insertion directe de code CSS ou JavaScript dans un modèle de page n'est pas toujours possible, par exemple, lorsque le CSS conforme à Sass est utilisé. La conservation des actifs CSS et JS dans des fichiers séparés simplifie également la maintenance. L'utilisation de la capacité en ligne d'Asset Manager vous permet d'optimiser la vitesse sans modifier la façon dont vos actifs sont stockés. Même des pipelines entiers peuvent être alignés.

Pour incorporer le contenu d'un fichier de ressources, utilisez l'option `{'loading' : 'inline'}` avec `addCss()` ou `addJs()`. Vous pouvez également intégrer tous les actifs lors du rendu d'un groupe avec `js()` ou `css()`, qui fournissent la même option.

Exemple d'utilisation de `system.yaml` pour définir des collections de ressources nommées en fonction des exigences de chargement des ressources, avec `app.css` étant un fichier CSS généré par [Sass](http://sass-lang.com/) :

```yaml
1  | assets:
2  |    collections:
3  |       css-inline:
4  |          - 'http://fonts.googleapis.com/css?family=Ubuntu:400|Open+Sans: 400,400i,700'
5  |          - 'theme://css-compiled/app.css'
6  |       js-inline:
7  |          - 'https://use.fontawesome.com/<embedcode>.js'
8  |       js-async:
9  |          - 'theme://foundation/dist/assets/js/app.js'
10 |          - 'theme://js/header-display.js'
```

Le modèle insère chaque collection dans son groupe correspondant, à savoir `head` et `head-link` pour CSS, `head` et `head-async` pour JS. L'en-tête de groupe par défaut est utilisé pour le chargement en ligne dans chaque cas :

```twig
1  | {% block stylesheets %}
2  |     {% do assets.addCss('css-inline') %}
3  |     {% do assets.addCss('css-link', {'group': 'head-link'}) %}
4  | {% endblock %}
5  | {{ assets.css('head-link')|raw }}
6  | {{ assets.css('head', {'loading': 'inline'})|raw }}
7  | {% block javascripts %}
8  |     {% do assets.addJs('js-inline') %}
9  |     {% do assets.addJs('js-async', {'group': 'head-async'}) %}
10 | {% endblock %}
11 | {{ assets.js('head-async', {'loading': 'async'})|raw }}
12 | {{ assets.js('head', {'loading': 'inline'})|raw }}
```
<h2 id="Actifs statiques">Actifs statiques
<a href="#Actifs statiques" class="toc-anchor after"></a></h2>

Parfois, il est nécessaire de référencer des actifs sans utiliser Asset Manager. Il existe une méthode d'assistance `url()` disponible pour y parvenir. Un exemple de cela pourrait être si vous vouliez référencer une image du thème. La syntaxe pour cela est :

    <img src="{{ url("theme://" ~ widget.image)|e }}" alt="{{ widget.text|e }}" />

La méthode `url()` prend un deuxième paramètre facultatif `true` ou `false` pour permettre à l'URL d'inclure le schéma et le domaine. Par défaut, cette valeur est supposée `false`, ce qui donne uniquement l'URL relative. Par example:

    <script src="{{ url('theme://some/extra.css', true)|e }}"></script>

