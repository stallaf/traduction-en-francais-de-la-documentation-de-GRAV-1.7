<h1 class="rem">Balises, filtres & fonctions Twig</h1>

Bien que Twig fournisse déjà une longue liste de [filtres, de fonctions et de balises](https://twig.symfony.com/doc/1.x/#reference), Grav propose également une sélection d'ajouts utiles pour faciliter le processus de thématisation.

<div class="notice info">
Pour plus d'informations sur le développement de vos propres filtres Twig personnalisés, consultez l'exemple de <a href ="../cookbook-recettes-twig#Filtre/fonction Twig personnalisé">filtre/fonction Twig personnalisé</a> dans la section <strong>Twig Recipes</strong> du chapitre <strong>Cookbook</strong>.
</div>

<h2 id="Balises">Balises
<a href="#Balises" class="toc-anchor after"></a></h2>

Une balise fournit des fonctionnalités Twig de haut niveau. Des exemples de balises intégrées incluent des constructions telles que `include`, `block`, `for`, `if` et bien d'autres. Les tags sont identifiés dans Twig par l'utilisation de la syntaxe `{% tagname %}`. De plus, la plupart des balises sont fermées par un `{% endtagname %}`.

Grav inclut plusieurs balises personnalisées utiles qui fournissent des fonctionnalités telles que `cache`, `markdown`, `script`, `style`, `switch`, etc.

<br>
<a href="../balises-twig" class="button button-primary">Grav Twig Tags<span class="fa fa-arrow-right"></a>

<br>
<h2 id="Filtres">Filtres
<a href="#Filtres" class="toc-anchor after"></a></h2>  

Les filtres Twig vous permettent d'appliquer des fonctionnalités à la variable qui apparaît sur le côté gauche du symbole du pipe `(|)`. Ils sont particulièrement utiles lorsqu'il s'agit de manipuler du texte ou des variables. Le premier argument du filtre est toujours l'élément de gauche, mais les arguments suivants peuvent être passés entre parenthèses. Les filtres ont des capacités spéciales, notamment la capacité d'être sensibles au contexte et à l'environnement.

Des exemples de filtres Twig intégrés incluent `date`, `escape`, `join`, `lower`, `slice` et bien d'autres. Un exemple serait :

    {% set foo = "one,two,three,four,five"|split(',', 3) %}

Grav inclut plusieurs filtres personnalisés utiles qui fournissent des fonctionnalités telles que `hypernize`, `nicetime`, `starts_with`, `contains`, `base64_decode` et bien d'autres.

<br>
<a href="../filtres-twig" class="button button-primary">Grav Twig Filters <span class="fa fa-arrow-right"></a>

<br>
<h2 id="Fonctions">Fonctions
<a href="#Fonctions" class="toc-anchor after"></a></h2>

Les fonctions Twig sont un autre moyen d'implémenter des fonctionnalités dans Twig. Ils sont similaires aux filtres, mais plutôt que d'agir sur une variable via un `|` vous appelleriez ces fonctions directement et passeriez tous les attributs qu'elles prennent en charge entre parenthèses après le nom de la fonction. Souvent, Grav fournit à la fois un filtre et une fonction pour la même logique et laisse à l'utilisateur le soin de choisir la méthode qu'il préfère.

Des exemples de filtres Twig intégrés incluent `block`, `dump`, `parent` `random`, `range`, etc. Un exemple serait :

    {{ random(['apple', 'orange', 'citrus']) }}

Grav inclut plusieurs fonctions personnalisées utiles qui fournissent des fonctionnalités telles que `authorize`, `debug`, `evaluate`, `regex_filter`, `medias` et bien d'autres.

<br>
<a href="../fonctions-twig" class="button button-primary">Grav Twig Functions <span class="fa fa-arrow-right"></a>

<br>

