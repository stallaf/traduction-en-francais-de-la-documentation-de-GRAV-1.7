<h1 class="rem">Tutoriel sur les thèmes</h1>

Souvent, la meilleure façon d'apprendre une nouvelle chose est d'utiliser un exemple, puis d'essayer de construire votre propre création à partir de celui-ci. Nous allons utiliser cette même méthodologie pour créer un nouveau thème Grav.

<h2 id="Quark">Quark
<a href="#Quark" class="toc-anchor after"></a></h2>

Grav est livré avec un thème propre et moderne appelé Quark qui utilise le [framework Spectre.css](https://picturepan2.github.io/spectre/).

Spectre.css est un framework CSS léger, réactif et moderne pour un développement plus rapide et extensible.

Spectre fournit des styles de base pour la typographie et les éléments, un système de mise en page réactif basé sur une boîte flexible, des composants et des utilitaires CSS purs avec un codage des meilleures pratiques et un langage de conception cohérent.

Cependant, il est souvent préférable de commencer par quelque chose d'encore plus simple.

<h2 id="Pure.css">Pure.css
<a href="#Pure.css" class="toc-anchor after"></a></h2>

Pour les besoins de ce didacticiel, nous allons créer un thème qui utilise lepolulaire [framework  Pure.css](http://purecss.io/) développé par Yahoo!

Pure est un framework CSS petit, rapide et réactif qui contient les bases pour vous permettre de développer votre site sans la surcharge de frameworks plus grands tels que [Bootstrap](https://getbootstrap.com/css/) ou [Foundation](http://foundation.zurb.com/). Il contient plusieurs modules qui peuvent être utilisés indépendamment, mais tous ensemble, le package résultant ne fait que **4,0 Ko minifié et gzippé** !

Vous pouvez lire toutes les fonctionnalités que Pure apporte à la table sur le [site du projet Pure.css](http://purecss.io/).

En outre, vous devriez lire l'article du blog sur [les mises à jour importantes du thème](https://getgrav.org/blog/important-theme-updates) qui décrit certains changements clés dans les thèmes Grav pour fournir le meilleur support de plugin à l'avenir.

<h2 id="Étape 1 - Installer le plugin DevTools">Étape 1 - Installer le plugin DevTools
<a href="#Étape 1 - Installer le plugin DevTools" class="toc-anchor after"></a></h2>

<div class"notice note">
Les versions précédentes de ce didacticiel nécessitaient la création d'un thème de base par défaut. Tout ce processus peut être ignoré grâce à notre nouveau <strong>plugin DevTools</strong>.
</div>

La première étape de la création d'un nouveau thème consiste à **installer le plugin DevTools**. Ceci peut être fait de deux façons.

<h3 id="Installer via CLI GPM">Installer via CLI GPM
<a href="#Installer via CLI GPM" class="toc-anchor after"></a></h3>

* Naviguez dans la ligne de commande jusqu'à la racine de votre installation Grav.

```console
$ bin/gpm install devtools
```

<h3 id="Installer via le plugin d'administration">Installer via le plugin d'administration
<a href="#Installer via le plugin d'administration" class="toc-anchor after"></a></h3>

* Une fois connecté, accédez simplement à la section **Plugin**s depuis la barre latérale.
* Cliquez sur le bouton <span class="fa fa-plus"></span> **Add** en haut à droite.
* Recherchez **DevTools** dans la liste et cliquez sur le bouton <span class="fa fa-plus"></span> **Install**.

<h2 id="Étape 2 - Créer un thème de base">Étape 2 - Créer un thème de base
<a href="#Étape 2 - Créer un thème de base" class="toc-anchor after"></a></h2>

Pour cette prochaine étape, vous devez vraiment être dans la [ligne de commande](./cli-intro.md) car les DevTools fournissent quelques commandes CLI pour faciliter le processus de création d'un nouveau thème !

Depuis la racine de votre installation Grav saisissez la commande suivante :

```console
$ bin/plugin devtools new-theme
```

Ce processus vous posera quelques questions nécessaires à la création du nouveau thème :

<div class="notice note">
Nous allons utiliser du <strong>blanc pur</strong> pour créer un nouveau thème, mais vous pouvez créer un modèle de style <strong>d'héritage</strong> simple qui hérite d'un autre thème de base
</div>

```console
$ | bin/plugin devtools new-theme
  |
  | Enter Theme Name: MyTheme
  | Enter Theme Description: My New Theme
  | Enter Developer Name: Acme Corp
  | Enter Developer Email: contact@acme.co
  | Please choose a template type
  |   [pure-blank ] Basic Theme using Pure.css
  |   [inheritance] Inherit from another theme
  |   [copy       ] Copy another theme
  |  > pure-blank
  |
  | SUCCESS theme mytheme -> Created Successfully
  | 
  | Path: /www/user/themes/my-theme
```

La commande DevTools vous indique où ce nouveau modèle a été créé. Ce modèle créé est entièrement fonctionnel mais aussi très simple. Vous voudrez le modifier en fonction de vos besoins.

Afin de voir votre nouveau thème en action, vous devrez changer le thème par défaut de `quar`k à `my-theme`, et donc, éditez votre `user/config/system.yaml` et changez-le :

```yaml
1 | ...
2 | pages:
3 |   theme: my-theme
4 | ...
```

Rechargez votre site dans votre navigateur et vous devriez voir que le thème a maintenant changé.

<h2 id="Étape 3 - Bases du thème">Étape 3 - Bases du thème
<a href="#Étape 3 - Bases du thème" class="toc-anchor after"></a></h2>

Maintenant que nous avons créé un nouveau thème de base qui peut être modifié et développé, décomposons-le et regardons ce qui constitue un thème. Si vous regardez dans le dossier `user/themes/my-theme` vous verrez :

    .
    ├── CHANGELOG.md
    ├── LICENSE
    ├── README.md
    ├── blueprints.yaml
    ├── css
    │   └── custom.css
    ├── fonts
    ├── images
    │   └── logo.png
    ├── js
    ├── my-theme.php
    ├── my-theme.yaml
    ├── screenshot.jpg
    ├── templates
    │   ├── default.html.twig
    │   ├── error.html.twig
    │   └── partials
    │       ├── base.html.twig
    │       └── navigation.html.twig
    └── thumbnail.jpg

Ceci est un exemple de structure mais certaines choses sont requises :

<h3 id="Éléments requis pour fonctionner">Éléments requis pour fonctionner
<a href="#Éléments requis pour fonctionner" class="toc-anchor after"></a></h3>

Ces éléments sont essentiels et votre thème ne fonctionnera pas de manière fiable à moins que vous ne les incluiez dans votre thème.

* `blueprints.yaml` - Le fichier de configuration utilisé par Grav pour obtenir des informations sur votre thème. Il peut également définir un formulaire que l'administrateur peut afficher lors de l'affichage des détails du thème. Ce formulaire vous permettra d'enregistrer les paramètres du thème. [Ce fichier est documenté dans le chapitre Formulaires.](./formulaires-blueprints.md)
* `my-theme.php` - Ce fichier sera nommé en fonction de votre thème, mais peut être utilisé pour héberger toute logique dont votre thème a besoin. Vous pouvez utiliser n'importe quel [hook d'événement de plugin](./crochets-evenements.md) sauf `onPluginsInitialized(`), cependant il existe un hook `onThemeInitialized()` spécifique au thème pour les thèmes que vous pouvez utiliser à la place.
* `my-theme.yaml` - Il s'agit de la configuration utilisée par le plugin pour définir les options que le thème pourrait utiliser.
* `templates/` - Il s'agit d'un dossier qui contient les modèles Twig pour rendre vos pages.

<h3 id="Éléments requis pour Release">Éléments requis pour Release
<a href="#Éléments requis pour Release" class="toc-anchor after"></a></h3>

Ces éléments sont nécessaires si vous souhaitez publier votre thème via GPM.

* `CHANGELOG.md` - Un fichier qui suit le [Format Grav Changelog](./avance-grav-developpement.md#Format du journal des modification) pour montrer les changements dans les versions.
* `LICENCE` - un fichier de licence, devrait probablement être MIT sauf si vous avez un besoin spécifique pour autre chose.
* `README.md` - Un 'Readme' qui devrait contenir toute documentation pour le thème. Comment l'installer, le configurer et l'utiliser.
* `screenshot.jpg` - Capture d'écran 1009px x 1009px du thème.
* `thumbnail.jpg` - Capture d'écran 300px x 300px du thème.

<h2 id="Étape 4 - Modèle de base">Étape 4 - Modèle de base
<a href="#Étape 4 - Modèle de base" class="toc-anchor after"></a></h2>

Comme vous le savez depuis le [chapitre précédent](./themes-base.md), chaque élément de contenu dans Grav a un nom de fichier particulier, par ex. `default.md`, qui demande à Grav de rechercher un modèle de rendu Twig appelé `default.html.twig`. Il est possible de mettre tout ce dont vous avez besoin pour afficher une page dans ce seul fichier, et cela fonctionnerait bien. Cependant, il existe une meilleure solution.

En utilisant la balise Twig [Extends](https://twig.symfony.com/doc/1.x/tags/extends.html), vous pouvez définir une disposition de base avec des [blocs](https://twig.symfony.com/doc/1.x/tags/block.html) que vous définissez. Cela permet à n'importe quel modèle twig **d'étendre** le modèle de base et fournit des définitions pour tout **bloc** défini dans la base. Regardez donc le fichier `templates/default.html.twig` et examinez son contenu :

```twig
1 | {% extends 'partials/base.html.twig' %}
2 |
3 | {% block content %}
4 |    {{ page.content|raw }}
5 | {% endblock %}
```

Il y a vraiment deux choses qui se passent ici.

Tout d'abord, le modèle étend un modèle situé dans `partials/base.html.twig`.

<div class="notice note">
Vous n'avez pas besoin d'inclure <code>templates/</code> dans les templates Twig car Twig recherche déjà dans <code>templates/</code> comme niveau racine pour n'importe quel template.
</div>

Deuxièmement, le `content` du bloc est remplacé par le modèle de base et le contenu de la page est généré à sa place.

<div class="notice info">
Pour plus de cohérence, c'est une bonne idée d'utiliser le dossier <code>templates/partials</code> pour contenir les modèles Twig qui représentent soit de petits morceaux de HTML, soit qui sont partagés. Nous utilisons également des <code>templates/modular</code> pour les modèles modulaires et des <code>templates/forms</code> pour tous les formulaires. Vous pouvez créer tous les sous-dossiers que vous souhaitez si vous préférez organiser vos modèles différemment.
</div>

Si vous regardez les `templates/partials/base.html.twig`, vous verrez le corps de la mise en page HTML :

```html
1  | {% set theme_config = attribute(config.themes, config.system.pages.theme) %}
2  | <!DOCTYPE html>
3  | <html lang="{{ (grav.language.getActive ?: theme_config.default_lang)|e }}">
4  | <head>
5  | {% block head %}
6  |    <meta charset="utf-8" />
7  |    <title>{% if header.title %}{{ header.title|e }} | {% endif %}{{ site.title|e }}</title>
8  |
9  |   <meta http-equiv="X-UA-Compatible" content="IE=edge">
10 |   <meta name="viewport" content="width=device-width, initial-scale=1">
11 |   {% include 'partials/metadata.html.twig' %}
12 |
13 |   <link rel="icon" type="image/png" href="{{ url('theme://images/logo.png')|e }}" />
14 |   <link rel="canonical" href="{{ page.url(true, true)|e }}" />
15 | {% endblock head %}
16 |
17 | {% block stylesheets %}
18 |   {% do assets.addCss('http://yui.yahooapis.com/pure/0.6.0/pure-min.css', 100) %}
19 |   {% do assets.addCss('https://maxcdn.bootstrapcdn.com/font-awesome/4.5.0/css/font-awesome.min.css', 99) %}
20 |   {% do assets.addCss('theme://css/custom.css', 98) %}
21 | {% endblock %}
22 |
23 | {% block javascripts %}
24 |    {% do assets.addJs('jquery', 100) %}
25 | {% endblock %}
26 |
27 | {% block assets deferred %}
28 |    {{ assets.css()|raw }}
29 |    {{ assets.js()|raw }}
30 | {% endblock %}
31 |
32 | </head>
33 | <body id="top" class="{{ page.header.body_classes|e }}">
34 | 
35 | {% block header %}
36 |    <div class="header">
37 |       <div class="wrapper padding">
38 |          <a class="logo left" href="{{ (base_url == '' ? '/' : base_url)|e }}">
39 |             <i class="fa fa-rebel"></i>
40 |             {{ config.site.title|e }}
41 |          </a>
42 |          {% block header_navigation %}
43 |          <nav class="main-nav">
44 |             {% include 'partials/navigation.html.twig' %}
45 |          </nav>
46 |          {% endblock %}
47 |       </div>
48 |    </div>
49 | {% endblock %}
50 | 
51 | {% block body %}
52 |    <section id="body">
53 |       <div class="wrapper padding">
54 |       {% block content %}{% endblock %}
55 |       </div>
56 | </section>
57 | {% endblock %}
58 | 
59 | {% block footer %}
60 |    <div class="footer text-center">
61 |       <div class="wrapper padding">
62 |          <p><a href="https://getgrav.org">Grav</a> was <i class="fa fa-code"></i> with <i class="fa fa-heart"></i> by <a href="http://www.rockettheme.com">RocketTheme</a>.</p>
63 |       </div>
64 |    </div>
65 | {% endblock %}
66 | 
67 | {% block bottom %}
68 |    {{ assets.js('bottom')|raw }}
69 | {% endblock %}
70 | 
71 |</body>
```

<div class="notice note">
ASTUCE : si la variable peut être rendue en toute sécurité et contient du code HTML, utilisez toujours le filtre <code>|raw</code> pour que le modèle fonctionne avec la fonction <code>autoescape</code> d'échapement automatique activée.
</div>

<div class="notice info">
Il est très important d'activer le paramètre d'échappement automatique <code>autoescape</code> à partir de votre <a href ="../configuration#Twig">Configuration Système</a> ou de ne pas oublier d'échapper chaque variable dans les fichiers de modèle pour protéger votre site contre les <a href ="https://developer.mozilla.org/en-US/docs/Glossary/Cross-site_scripting">attaques XSS</a>.
</div>

<h2 id="Étape 5 - Décomposition">Étape 5 - Décomposition
<a href="#Étape 5 - Décomposition" class="toc-anchor after"></a></h2>

Veuillez relire le code dans le fichier `base.html.twig` pour essayer de comprendre ce qui se passe. Il y a plusieurs éléments clés à noter :

1. Une variable `theme_config` est définie avec la configuration du thème. Parce que Twig ne fonctionne pas bien avec les tirets, pour récupérer les variables avec des tirets (par exemple `config.themes.my-theme`), nous utilisons la fonction `attribute()` Twig pour récupérer dynamiquement les données `my-theme` de `config.themes`.

2. L'élément `<html lang=...` est défini en fonction de la langue active de Grav s'il est activé, sinon il utilise le `default_lang` tel que défini dans le `theme_config`.

3. La syntaxe `{% block head %}{% endblock head %}` définit une zone dans le modèle Twig de base. Notez que l'utilisation de `head` dans la balise `{% endblock head %}` n'est pas obligatoire, mais est utilisée ici pour des raisons de lisibilité. Dans ce bloc, nous plaçons des éléments qui se trouvent généralement dans la balise HTML `<head>`.

4. La balise `<title>` est définie dynamiquement en fonction de la variable `title` de la page telle qu'elle est définie dans l'en-tête de la page. `header.title` est une méthode de raccourci mais équivaut à `page.header.title`.

5. Une fois que quelques balises méta standard sont définies, il existe une référence pour inclure `partials/metadata.html.twig`. Ce fichier se trouve dans le dossier `systems/templates/partials` et contient une boucle qui parcourt les métadonnées de la page. Il s'agit en fait d'une fusion des métadonnées de `site.yaml` et de tous les remplacements spécifiques à la page.

6. L'entrée `<link rel="icon"...` est définie en pointant vers une image spécifique au thème. Dans ce cas, il se trouve dans le répertoire du thème sous `images/logo.png`. La syntaxe pour cela est `{{ url('theme://images/logo.png') }}`.

7. L'entrée `<link rel="canonical"...` définit une URL canonique pour la page qui est toujours définie sur l'URL complète de la page via `{{ page.url(true, true) }}`.

8. Nous définissons maintenant un bloc appelé feuilles de style `stylesheets`, et ici nous utilisons le [Gestionnaire d'Actifs](./asset-manager.md) pour ajouter plusieurs actifs. Le premier charge le framework Pure.css. Le second charge [FontAwesome](http://fontawesome.io/) pour fournir des icônes utiles. La dernière entrée pointe vers un fichier `custom.css` dans le dossier `css/` du thème. Voici quelques styles utiles pour vous aider à démarrer, mais vous pouvez en ajouter d'autres ici. Vous pouvez également ajouter d'autres entrées de fichier CSS si nécessaire.

9. L'appel `{{ assets.css()|raw }}` est ce qui déclenche le modèle pour afficher toutes les balises de lien CSS.

10. Le bloc `javascripts`, comme le bloc des feuilles de style 'stylesheets`est un bon endroit pour mettre vos fichiers JavaScript. Dans cet exemple, nous ajoutons uniquement la bibliothèque 'jquery' qui est déjà fournie avec Grav, vous n'avez donc pas besoin de lui fournir un chemin.

11. Le `{{ assets.js()|raw }}` affichera toutes les balises JavaScript.

12. La balise `<body>` a un attribut de classe qui affichera tout ce que vous définissez dans la variable `body_classes` du frontmatter de la page.

13. Le bloc d'en-tête `header`contient quelques éléments qui génèrent l'en-tête HTML de la page. Une chose importante à noter est que le logo est lié à la base_url avec la logique : {{ base_url == '' ? '/' : base_url }}. Cela permet de s'assurer que s'il n'y a pas de sous-répertoire, le lien est juste /.

14. Le titre du site est généré en tant que logo dans cet exemple de thème avec `{{ config.site.title }}` mais vous pouvez simplement le remplacer par une balise `<img>` vers un logo si vous le souhaitez.

15. La balise `<nav>` contient en fait un lien vers `partials/navigation.html.twig` qui contient la logique pour boucler sur toutes les pages **visibles** et les afficher sous forme de menu. Par défaut, il prend en charge les menus déroulants pour les pages imbriquées, mais cela peut être désactivé via la configuration du thème. Jetez un œil à ce fichier de navigation pour avoir une idée de la façon dont le menu est généré.

16. L'utilisation de `{% block content %}{% endblock %}` fournit un espace réservé qui nous permet de fournir du contenu à partir d'un modèle qui étend celui-ci. N'oubliez pas que nous avons remplacé cela dans `default.html.twig` pour afficher le contenu de la page.

17. Le bloc de pied de page contient un pied de page simple, vous pouvez facilement le modifier selon vos besoins.

18. Semblable au bloc de contenu, le `{% block bottom %}{% endblock %}` est conçu comme un espace réservé pour les modèles afin d'ajouter une initialisation JavaScript personnalisée ou des codes analytiques. Dans cet exemple, nous produisons tout code JavaScript ajouté au groupe d'actifs `bottom`. En savoir plus à ce sujet dans la documentation d'[Asset Manager](./asset-manager.md).

<h2 id="Étape 6 - Thème CSS">Étape 6 - Thème CSS
<a href="#Étape 6 - Thème CSS" class="toc-anchor after"></a></h2>

Vous avez peut-être remarqué que dans le fichier `partials/base.html.twig` nous avons fait référence à un CSS de thème personnalisé via Asset Manager : `do assets.add('theme://css/custom.css', 98)`. Ce fichier hébergera tout CSS personnalisé dont nous avons besoin pour combler les lacunes non fournies par le framework Pure.css. Comme Pure est un cadre très minimal, il fournit l'essentiel mais presque pas de style.

1. Dans votre dossier `user/themes/my-theme/css`, visualisez le `custom.css` :

```css
1   | /* Core Styles */
2   |* {
3   |    -webkit-box-sizing: border-box;
4   |    -moz-box-sizing: border-box;
5   |    box-sizing: border-box;
6   | }
7   |
8   | body {
9   |    font-size: 1rem;
10  |    line-height: 1.7;
11  |    color: #606d6e;
12  | }
13  |
14  | h1,
15  | h2,
16  | h3,
17  | h4,
18  | h5,
19  | h6 {
20  |    color: #454B4D;
21  | }
22  | 
23  | a {
24  |    color: #1F8CD6;
25  |    text-decoration: none;
26  | }
27  | 
28  | a:hover {
29  |    color: #175E91;
30  | }
31  | 
32  | pre {
33  |    background: #F0F0F0;
34  |    margin: 1rem 0;
35  |    border-radius: 2px;
36  | }
37  | 
38  | blockquote {
39  |    border-left: 10px solid #eee;
40  |    margin: 0;
41  |    padding: 0 2rem;
42  | }
43  | 
44  | /* Utility Classes */
45  | .wrapper {
46  |    margin: 0 3rem;
47  | }
48  | 
49  | .padding {
50  |    padding: 3rem 1rem;
51  | }
52  | 
53  | .left {
54  |    float: left;
55  | }
56  | 
57  | .right {
58  |    float: right
59  | }
60  | 
61  | .text-center {
62  |    text-align: center;
63  | }
54  | 
65  | .text-right {
66  |    text-align: right;
67  | }
68  | 
69  | .text-left {
70  |    text-align: left;
71  | }
72  | 
73  | /* Content Styling */
74  | .header .padding {
75  |    padding: 1rem 0;
76  | }
77  | 
78  | .header {
79  |    background-color: #1F8DD6;
80  |    color: #eee;
81  | }
82  | 
83  | .header a {
84  |    color: #fff;
85  | }
86  | 
87  | .header .logo {
88  |    font-size: 1.7rem;
89  |    text-transform: uppercase;
90  | }
91  | 
92  | .footer {
93  |    background-color: #eee;
94  | }
95  | 
96  | /* Menu Settings */
97  | .main-nav ul {
98  |    text-align: center;
99  |    letter-spacing: -1em;
100 |    margin: 0;
101 |    padding: 0;
102 | }
103 | 
104 | .main-nav ul li {
105 |    display: inline-block;
106 |    letter-spacing: normal;
107 | }
108 | 
109 | .main-nav ul li a {
110 |    position: relative;
111 |    display: block;
112 |    line-height: 45px;
113 |    color: #fff;
114 |    padding: 0 20px;
115 |    white-space: nowrap;
116 | }
117 | 
118 | .main-nav > ul > li > a {
119 |    border-radius: 2px;
120 | }
121 | 
122 | /*Active dropdown nav item */
123 | main-nav ul li:hover > a {
124 |    background-color: #175E91;
125 | }
126 | 
127 | /* Selected Dropdown nav item */
128 | .main-nav ul li.selected > a {
129 |    background-color: #fff;
130 |    color: #175E91;
131 | }
132 | 
133 | /* Dropdown CSS */
134 | .main-nav ul li {position: relative;}
135 | 
136 | .main-nav ul li ul {
137 |    position: absolute;
138 |    background-color: #1F8DD6;
139 |    min-width: 100%;
140 |    text-align: left;
141 |    z-index: 999;
142 |    display: none;
143 | }
144 | 
145 | .main-nav ul li ul li {
146 |    display: block;
147 | }
148 | 
149 | /* Dropdown CSS */
150 | .main-nav ul li ul ul {
151 |    left: 100%;
152 |    top: 0;
153 | }
154 | 
155 | /* Active on Hover */
156 | .main-nav li:hover > ul {
157 |    display: block;
158 | }
159 | 
160 | /* Child Indicator */
161 | .main-nav .has-children > a {
162 |    padding-right: 30px;
163 | }
164 |
165 | .main-nav .has-children > a:after {
166 |    font-family: FontAwesome;
167 |    content: '\f107';
168 |    position: absolute;
169 |    display: inline-block;
170 |    right: 8px;
171 |    top: 0;
172 | }
173 | 
174 | .main-nav .has-children .has-children > a:after {
175 |    content: '\f105';
176 | }
```
C'est un truc CSS assez standard et définit des marges, des polices, des couleurs et des classes utilitaires de base. Il existe un style de contenu de base et un style plus étendu requis pour afficher le menu déroulant. N'hésitez pas à modifier ce fichier selon vos besoins, ou même à ajouter de nouveaux fichiers CSS (assurez-vous simplement d'ajouter une référence dans le bloc `head` en suivant l'exemple pour `custom.css`).

<h2 id="Étape 7 - Test">Étape 7 - Test
<a href="#Étape 7 - Test" class="toc-anchor after"></a></h2>

Pour voir votre thème en action, ouvrez votre navigateur et pointez-le vers votre site Grav. Vous devriez voir quelque chose comme ceci :

![Test](https://learn.getgrav.org/images/1/0/e/f/d/10efdce6c8720a9f16d2406c14c23142b776bff9-pure-theme.png)

Félicitations, vous avez créé votre premier thème !

