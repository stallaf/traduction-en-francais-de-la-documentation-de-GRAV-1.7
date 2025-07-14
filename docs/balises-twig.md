<h1 class="rem">Balises</h1>

Grav fournit également une variété de balises Twig personnalisées qui étendent les capacités de création de modèles Twig déjà très performantes avec de nouvelles balises que nous avons trouvées utiles.

<div class="typo">
markdown
</div>

La balise Markdown fournit un nouveau moyen puissant d'intégrer le démarquage dans le modèle Twig. Vous pouvez utiliser une variable et restituer cette variable avec le filtre `|markdown`, mais la syntaxe `{% markdown %}` rend la création de blocs de texte markdown encore plus simple.

```twig
1 | {% markdown %}
2 | This is **bold** and this _underlined_
3 |    
4 | 1. This is a bullet list
5 | 2. This is another item in that same list
6 | {% endmarkdown %}
```

<div class="typo">
script
</div>

La balise Script est vraiment une balise pratique qui rend votre Twig plus lisible par rapport à l'approche habituelle `{% do assets...%}`. C'est purement une manière alternative d'écrire les choses.

<h2 id="Fichier de script">Fichier de script
<a href="#Fichier de script" class="toc-anchor after"></a></h2>

```twig
1 | {% script 'theme://js/something.js' at 'bottom' priority: 20 with { defer: true, async: true } %}
```

Grav 1.7.28 ajoute également le support des modules :

```twig
1 | {% script module 'theme://js/module.mjs' %}
```

<h2 id="Script en ligne">Script en ligne
<a href="#Script en ligne" class="toc-anchor after"></a></h2>

```twig
1 | {% script at 'bottom' priority: 20 %}
2 |    alert('Warning!');
3 | {% endscript %}
```

<div class="typo">
style
</div>

<h2 id="Fichier CSS">Fichier CSS
<a href="#Fichier CSS" class="toc-anchor after"></a></h2>

```twig
1 | {% style 'theme://css/foo.css' priority: 20 %}
```

<h2 id="CSS en ligne">CSS en ligne
<a href="#CSS en ligne" class="toc-anchor after"></a></h2>

```twig
1 | {% style priority: 20 with { media: 'screen' } %}
2 |    a { color: red; }
3 | {% endstyle %}
```

<div class="typo">
lien
</div>

```twig
1 | {% link icon 'theme://images/favicon.png' priority: 20 with { type: 'image/png' } %}
2 | {% link modulepreload 'plugin://grav-plugin/build/js/vendor.js' %}
```

<div class="typo">
switch
</div>

Dans la plupart des langages de programmation, l'utilisation d'une instruction `switch` est un moyen courant de rendre un tas d'instructions `if else` plus propres et plus lisibles. Ils peuvent également s'avérer légèrement plus rapides. Nous fournissons simplement un moyen simple de les créer car ils manquaient dans la fonctionnalité de base de Twig.

```twig
1 | {% switch type %}
2 |   {% case 'foo' %}
3 |      {{ my_data.foo }}
4 |   {% case 'bar' %}
5 |      {{ my_data.bar }}
6 |   {% default %}
7 |      {{ my_data.default }}
8 | {% endswitch %}
```

<div class="typo">
deferred
</div>

Avec les blocs traditionnels, une fois le bloc rendu, il ne peut plus être manipulé. Prenons l'exemple d'un `{% block scripts %}` qui pourrait contenir des entrées pour les inclusions JavaScript. Si vous avez un modèle Twig enfant et que vous étendez un modèle de base où ce bloc est défini, vous pouvez étendre le bloc et ajouter vos propres entrées JavaScript personnalisées. Cependant, les modèles de brindilles partiels inclus à partir de cette page ne peuvent pas atteindre ou interagir avec le bloc.

L'attribut différé sur le bloc qui est alimenté par [l'extension différée](https://github.com/rybakit/twig-deferred-extension) signifie que vous pouvez définir ce bloc dans n'importe quel modèle Twig, mais son rendu est différé, de sorte qu'il s'affiche après tout le reste. Cela signifie que vous pouvez ajouter des références JavaScript via l'appel `{% do assets.addJs() %}` depuis n'importe où dans votre page, et comme le rendu est différé, la sortie contiendra tous les actifs que Grav connaît, peu importe quand vous les a ajoutés.

```twig
1 | {% block myblock deferred %}
2 |   This will be rendered after everything else.
3 | {% endblock %}
```   

Il est également possible de fusionner le contenu du bloc parent avec le bloc différé en utilisant `{{ parent() }}`. Cela peut être particulièrement utile pour les thèmes si des fichiers css ou javascript supplémentaires sont ajoutés.

```twig
1 | {% block stylesheets %}
2 |   <!-- Additional css library -->
3 |   {% do assets.addCss('theme://libraries/leaflet/dist/leaflet.css')%}
4 |   {{ parent() }}
5 | {% endblock %}
```

<div class="typo">
throw
</div>

Il existe certaines situations où vous devez lever manuellement une exception, nous avons donc également une balise pour cela.

```twig
1 | {% throw 404 'Not Found' %}
```

<div class="typo">
try & catch
</div>

Il est également utile d'avoir une gestion des erreurs de style PHP plus puissante dans vos modèles Twig, nous avons donc une nouvelle balise `try/catch`.

```twig
1 | {% try %}
2 |   <li>{{ user.get('name') }}</li>
3 | {% catch %}
4 |   User Error: {{ e.message }}
5 | {% endcatch %}
```

<div class="typo">
render
</div>

Les objets flexibles font lentement leur chemin dans de plus en plus d'éléments de Grav. Ce sont des objets auto-conscients qui ont une structure de modèle Twig associée, ils savent donc comment s'afficher eux-mêmes. Afin de les utiliser, nous avons implémenté une nouvelle balise de rendu `render` qui prend une mise en page facultative qui à son tour contrôle avec quelle mise en page de modèle l'objet doit être rendu.

```twig
1 | {% render collection layout: 'list' %}
2 | {% render object layout: 'default' with { variable: 'value' } %}
```

<div class="typo">
cache
</div>

Parfois, vous devrez peut-être mettre en cache des parties de la page, dont le rendu prend beaucoup de temps. Vous pouvez le faire avec la balise `cache`.

```twig
1 | {% cache 600 %}
2 |   {{ some_complex_work() }}
3 | {% endcache %}
```

Dans l'exemple, `600` est une durée de vie facultative en secondes. Si le paramètre n'est pas passé, la durée de vie du cache par défaut sera utilisée.

