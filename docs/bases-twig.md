<h1 class="rem">Approche de Twig</h1>

Twig est un moteur de template rapide et optimisé pour PHP. Il est conçu dès le départ pour faciliter la création de modèles à la fois pour le développeur et le concepteur.

Sa syntaxe facile à suivre et ses processus simples en font un choix naturel pour toute personne familiarisée avec Smarty, Django, Jinja, Liquid ou Stencil.

Nous l'utilisons pour nos modèles Grav en partie à cause de sa flexibilité et de sa sécurité inhérente. Le fait qu'il s'agisse également de l'un des moteurs de templates les plus rapides pour PHP a fait de son choix pour une utilisation dans Grav une évidence.

Twig compile les modèles en PHP simple. Cela réduit au minimum la surcharge PHP, ce qui se traduit par une expérience utilisateur plus rapide et plus simple.

C'est aussi un moteur très flexible grâce à son lexer et à son analyseur. Cela permet au développeur de créer ses propres balises et filtres personnalisés. Il lui permet également de créer son propre [langage spécifique au domaine](https://en.wikipedia.org/wiki/Domain-specific_language) (DSL).

En matière de sécurité, Twig ne fait aucun compromis. Il donne au développeur un mode bac à sable qui lui permet d'examiner tout code non approuvé. Cela vous donne la possibilité d'utiliser Twig comme langage de modèle pour les applications tout en donnant aux utilisateurs la possibilité de modifier la conception du modèle.

Fondamentalement, il s'agit d'un moteur puissant qui vous permet de contrôler l'interface utilisateur. Lorsqu'il est combiné avec YAML pour la configuration, il constitue un système puissant et simple avec lequel tout développeur ou gestionnaire de site peut travailler.

<h2 id="Comment fonctionne Twig ?">Comment fonctionne Twig ?
<a href="#Comment fonctionne Twig ?" class="toc-anchor after"></a></h2>

Twig fonctionne en éliminant tous les tour de passe-passe de la conception de modèles. Les modèles sont essentiellement des fichiers texte contenant des variables ou des expressions qui sont remplacées par des valeurs lors de l'évaluation du modèle.

Les balises sont également une partie importante d'un fichier de modèle, car elles contrôlent la logique du modèle lui-même.

Twig a deux contraintes linguistiques principales.

* `{{ }}` affiche le résultat d'une évaluation d'expression ;
* `{% %}` exécute les instructions.

Voici un modèle de base créé à l'aide de Twig :

```twig
<!DOCTYPE html>
<html>
   <head>
      <title>All About Cookies</title>
   </head>
   <body>
      My name is {{ name|e }} and I love cookies.
      My favorite flavors of cookies are:
      <ul>
         {% for cookie in cookies %}
            <li>{{ cookie.flavor|e }}</li>
         {% endfor %}
      </ul>
      <h1>Cookies are the best!</h1>
   </body>
</html>
```

Dans cet exemple, nous définissons le titre du site comme vous le feriez avec n'importe quelle page Web standard. La différence est que nous avons pu utiliser la syntaxe Twig simple pour présenter le nom de l'auteur et créer une liste dynamique de types d'éléments.

Un modèle est d'abord chargé, puis passé par le **lexer** où son code source est tokenisé et divisé en petits morceaux. À ce stade, **l'analyseu**r prend les jetons et les transforme en arbre de syntaxe abstraite.

Une fois cela fait, le compilateur le transforme en code PHP qui peut ensuite être évalué et affiché à l'utilisateur.

Twig peut également être étendu pour ajouter des balises, des filtres, des tests, des opérateurs, des variables globales et des fonctions supplémentaires. Vous trouverez plus d'informations sur l'extension de Twig dans sa [documentation officielle](https://twig.symfony.com/doc/1.x/advanced.html).

<h2 id="Syntaxe de Twig">Syntaxe de Twig
<a href="#Syntaxe de Twig" class="toc-anchor after"></a></h2>

Un modèle Twig comporte plusieurs éléments clés qui l'aident à comprendre ce que vous aimeriez faire. Ceux-ci incluent des balises, des filtres, des fonctions et des variables.

Examinons de plus près ces outils importants et comment ils peuvent vous aider à créer un modèle incroyable.

<h3 id="Balises">Balises
<a href="#Balises" class="toc-anchor after"></a></h3>

Les balises indiquent à Twig ce qu'il doit faire. Il vous permet de définir quel code Twig doit gérer et quel code il doit ignorer lors de l'évaluation.

Il existe plusieurs types de balises, et chacune a sa propre syntaxe spécifique qui les distingue.

<h4 id="Balises de commentaires">Balises de commentaires
<a href="#Balises de commentaires" class="toc-anchor after"></a></h4>

Les balises de commentaire (`{# Insérer un commentaire ici #}`) sont utilisées pour définir des commentaires qui existent dans le fichier de modèle Twig, mais qui ne sont pas réellement vus par l'utilisateur final. Ils sont supprimés lors de l'évaluation et ne sont ni analysés ni générés.

Une bonne utilisation de ces balises consiste à expliquer ce que fait une ligne de code ou une commande spécifique afin qu'un autre développeur ou concepteur de votre équipe puisse lire et comprendre rapidement.

Voici un exemple de balise de commentaire telle que vous la trouveriez dans un fichier de modèle Twig :

```twig
{# Commentaire lu quelque part : Merci stallaf pour cette traduction #}
```

<h4 id="Balises de sortie">Balises de sortie
<a href="#Balises de sortie" class="toc-anchor after"></a></h4>

Les balises de sortie (`{{ Insérer la sortie ici }}`) seront évaluées et ajoutées à la sortie générée. C'est là que vous placeriez tout ce que vous voulez voir apparaître sur le front-end ou dans un autre contenu généré.

Voici un exemple de balises de sortie utilisées dans un modèle Twig :

```twig
Je m'appelle {{ name|e }} et j'adore les cookies.
```

La variable `name`a été insérée dans cette ligne et apparaîtra à l'utilisateur final sous la forme `Mon nom est Jake et j'adore les cookies`. car `Jake` était la valeur de la variable name.

<div class="notice info">
Il est très important d'activer le paramètre d'échappement automatique <code>autoescape</code> à partir de votre configuration système ou de ne pas oublier d'échapper chaque variable dans les fichiers de modèle en utilisant le filtre <code>|e</code> pour protéger votre site contre les attaques XSS. Pour un contenu HTML sûr, utilisez le filtre <code>|raw</code>.
</div>

<h4 id="Balises d'action">Balises d'action
<a href="#Balises d'action" class="toc-anchor after"></a></h4>

Les balises d'action sont les fonceuses du monde Twig. Ces balises font réellement quelque chose, contrairement aux autres qui transmettent quelque chose ou restent les bras croisés dans le code source en attendant qu'un concepteur le lise.

Les balises d'action définissent des variables, parcourent des tableaux et testent des conditions. Vos déclarations `for` et `if` sont faites à l'aide de ces balises.

Voici à quoi pourrait ressembler une balise d'action dans un modèle Twig :

```twig
1 | {% set hour = now | date("G") %}
2 | {% if hour >= 9 and hour < 17 %}
3 |   <p>Time for cookies!</p>
4 | {% else %}
5 |   <p>Time to make more cookies!</p>
6 | {% endif %}
```

La balise d'action initiale définit l'heure comme l'heure actuelle dans une horloge de 24 heures. Cette valeur est ensuite utilisée pour évaluer s'il est entre 9 h et 17 h. Si c'est le cas, c'est `Time for cookies` ! est affiché. Si ce n'est pas le cas, `Time to make more cookies !` s'affiche à la place.

Il est très important que les balises ne se chevauchent pas. Vous ne pouvez pas mettre une balise de sortie à l'intérieur d'une balise d'action, ou vice versa.

<h4 id="Filtres">Filtres
<a href="#Filtres" class="toc-anchor after"></a></h4>

Les filtres sont utiles, en particulier lorsque vous utilisez les balises de sortie pour afficher des données qui peuvent ne pas être formatées comme vous le souhaitez.

Supposons que la valeur de la variable `name` puisse inclure des balises SGML/XML indésirables. Vous pouvez les filtrer en utilisant le code ci-dessous :

```twig
{{ nom|striptags|e }}
```

<h4 id="Les fonctions">Les fonctions
<a href="#Les fonctions" class="toc-anchor after"></a></h4>

Les fonctions peuvent générer du contenu. Ils sont généralement suivis d'arguments, qui apparaissent entre parenthèses placées directement après l'appel de la fonction. Même si aucun argument n'est présent, la fonction aura toujours une parenthèse () placée directement après elle.

```twig
{% if date(cookie.created_at) < date('-2days') %}
   {# Eat it! #}
{% endif %}
```

<h4 id="Ressources">Ressources
<a href="#Ressources" class="toc-anchor after"></a></h4>

* [Documentation officielZe Twig](https://twig.symfony.com/doc/1.x/)
* [Twig pour les concepteurs de modèles](https://twig.symfony.com/doc/1.x/templates.html)
* [Twig pour les développeurs](https://twig.symfony.com/doc/1.x/api.html)
* [Introduction vidéo de 6 minutes sur Twig](http://www.dev-metal.com/6min-video-introduction-twig-php-templating-engine/)
* [Introduction à Twig](http://www.slideshare.net/markstory/introduction-to-twig)
* [Twig: Les bases](https://knpuniversity.com/screencast/twig/basics) (introduction gratuite au cours payant)

