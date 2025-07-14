<h1 class="rem">Recettes générales</h1>

Cette page contient un assortiment de problèmes et leurs solutions respectives liées à Grav en général.

<h2 id="Modifier la version de l'interface de ligne de commande PHP">Modifier la version de l'interface de ligne de commande PHP.
<a href="#Modifier la version de l'interface de ligne de commande PHP" class="toc-anchor after"></a></h2>

Parfois, sur le terminal, la version de PHP est différente de la version de PHP utilisée par le serveur Web.

Vous pouvez vérifier la version PHP en cours d'exécution dans la CLI en exécutant la commande `php -v`. Si la version de PHP est inférieure à 5.5.9, Grav ne fonctionnera pas car il nécessite au moins PHP 5.5.9.

Comment réparer?

Vous devez entrer une configuration dans `.bashrc` ou dans `.bash_profile` dans votre dossier d'accueil utilisateur. Créez ces fichiers si vous ne les avez pas déjà dans le dossier utilisateur. Ce sont des fichiers cachés, vous devrez donc peut-être faire `ls -al` pour les afficher. Une fois la configuration ajoutée, vous devrez démarrer une nouvelle session de terminal pour que ces paramètres s'appliquent.

Un exemple de configuration pourrait être :

    $ | alias php="/usr/local/bin/php53"
    $ | export PHP_PATH = /usr/local/bin/php53

Une autre façon consiste à ajouter :

```configuration
1  | # .bash_profile
2  | 
3  | # Get the aliases and functions
4  | if [ -f ~/.bashrc ]; then
5  |         . ~/.bashrc
6  | fi
7  | 
8  | # User specific environment and startup programs
9  | 
10 |PATH=/usr/local/lib/php-5.5/bin:$PATH:$HOME/bin
11 |
12 |export PATH
```

Le chemin exact dépend bien sûr de la configuration de votre système, où il stocke les binaires de la version PHP la plus récente. Cela peut être quelque chose que vous trouverez dans la documentation de l'hébergement, ou vous pouvez demander à votre configuration d'hébergement si vous ne le trouvez nulle part.

Vous pouvez également essayer de regarder dans les fichiers ou dossiers `php-something` sous les dossiers `/usr/local/bin` ou `/usr/local/lib`, avec `ls -la /usr/local/lib/ |grep -i php`.

<h2 id="Créer une galerie simple">Créer une galerie simple.
<a href="#Créer une galerie simple" class="toc-anchor after"></a></h2>

<h3 id="Problème-1 :">Problème :
<a href="#Problème-1 :" class="toc-anchor after"></a></h3>

Une exigence courante en matière de conception de sites Web est d'avoir une galerie quelconque rendue sur une page. Il peut s'agir d'afficher des photos de votre nouvel animal de compagnie, un portefeuille de travaux de conception antérieurs ou même un catalogue de base de certains produits que vous souhaitez afficher et vendre à vos utilisateurs. Dans cet exemple, nous supposerons que vous souhaitez simplement afficher un groupe de photos avec une légende ci-dessous. Cela peut bien sûr être adapté à d'autres utilisations également.

<h3 id="La solution-1 :">La solution :
<a href="#La solution-1 :" class="toc-anchor after"></a></h3>

Le moyen le plus simple de fournir une solution à ce problème est d'utiliser la [fonctionnalité média](/media) de Grav qui permet à une page de connaître les images disponibles dans son dossier.

Supposons que vous ayez une page que vous avez appelée `gallery.md` et que vous ayez également une variété d'images dans le même répertoire. Les noms de fichiers eux-mêmes ne sont pas importants car nous allons simplement parcourir chacune des images. Parce que nous voulons avoir des données supplémentaires associées à chaque image, nous inclurons un fichier `meta.yaml` pour chaque image. Par exemple, nous avons quelques images :

```configuration
1 | - fido-playing.jpg
2 | - fido-playing.jpg.meta.yaml
3 | - fido-dormir.jpg
4 | - fido-dormir.jpg.meta.yaml
5 | - fido-eating.jpg
6 | - fido-eating.jpg.meta.yaml
7 | - fido-grognement.jpg
8 | - fido-growling.jpg.meta.yaml
```

Chacun des fichiers `.jpg` a une taille relativement bonne qui convient à une version pleine grandeur, 1280px x 720px. Chacun des fichiers `meta.yaml` contient quelques entrées clés, regardons `fido-playing.jpg.meta.yaml` :

    1 | title : Fido jouant avec son os
    2 | description : L'autre jour, Fido a eu un nouvel os, et il en est devenu vraiment captivé.

Vous avez un **contrôle total** sur ce que vous mettez dans ces méta-fichiers, ils peuvent être absolument tout ce dont vous avez besoin.

Nous devons maintenant afficher ces images dans l'ordre chronologique inverse afin que les images les plus récentes soient affichées en premier et les affichent. Parce que notre page s'appelle `gallery.md`, nous devons créer un `templates/gallery.html.twig` approprié pour contenir la logique de rendu dont nous avons besoin :

```html
1  | {% extends 'partials/base.html.twig' %}
2  | 
3  | {% block content %}
4  |     {{ page.content|raw }}
5  | 
6  |     <ul>
7  |     {% for image in page.media.images %}
8  |     <li>
9  |         <div class="image-surround">
10 |             {{ image.cropResize(300,200).html|raw }}
11 |         </div>
12 |         <div class="image-info">
13 |             <h2>{{ image.meta.title }}</h2>
14 |             <p>{{ image.meta.description }}</p>
15 |         </div>
16 |     </li>
17 |     {% endfor %}
18 |     </ul>
19 | 
20 | {% endblock %}
```

Pour qu'une galerie modulaire s'affiche dans une autre page, supprimez le code suivant du fichier Twig afin de le faire fonctionner :

```twig
1 | {% extends 'partials/base.html.twig' %}
2 | 
3 | {% block content %}
4 |     {{ page.content|raw }}
```
 
et

```twig
{% endblock %}
```

Fondamentalement, cela étend le standard `partials/base.html.twig` (en supposant que votre thème a ce fichier), il définit ensuite le bloc de contenu `content` et fournit le contenu pour celui-ci. La première chose que nous faisons est de faire écho à n'importe quelle `page.content`. Ce serait le contenu du fichier `gallery.md`, il pourrait donc contenir un titre et une description de cette page.

La section suivante boucle simplement sur tous les médias de la page qui sont des **images**. Nous les publions dans une liste non ordonnée pour rendre la sortie sémantique et facile à styliser avec CSS. nous attribuons à chaque image le nom de la variable `image`, puis nous pouvons exécuter une simple méthode `cropResize()` pour redimensionner l'image à quelque chose de convenable, puis en dessous, nous fournissons une section d'informations avec le `title` et la `description`.

Vous pouvez faire une implémentation de galerie plus avancée en utilisant la création de filtres pour les données de l'appareil photo, avec la fonction [EXIF](/fonction-twig).

<h2 id="Rendre le contenu en colonnes">Rendre le contenu en colonnes.
<a href="#Rendre le contenu en colonnes" class="toc-anchor after"></a></h2>

<h3 id="Problème-2 :">Problème :
<a href="#Problème-2 :" class="toc-anchor after"></a></h3>

Une question qui a été soulevée à plusieurs reprises est de savoir comment afficher rapidement une seule page dans plusieurs colonnes.

<h3 id="La solution-2 :">La solution :
<a href="#La solution-2 :" class="toc-anchor after"></a></h3>

Il existe de nombreuses solutions potentielles, mais une solution simple consiste à diviser votre contenu en sections logiques à l'aide d'un délimiteur tel que la balise HTML `<hr />` ou la balise de *rupture thématique*. Dans le démarquage, cela est représenté par 3 tirets ou plus ou `---`. Nous créons simplement notre contenu et séparons nos sections de contenu avec ces tirets :

<h4 id="colonnes.md">colonnes.md.
<a href="#colonnes.md" class="toc-anchor after"></a></h4>

```configuration
1  | ---
2  | title: 'Columns Page Test'
3  | ---
4  | 
5  | Lorem ipsum dolor sit amet, consectetur adipiscing elit. Maecenas arcu leo, hendrerit ut rhoncus eu, dictum vitae ligula. Suspendisse interdum at purus eget congue. Aliquam erat volutpat. Proin ultrices ligula vitae nisi congue sagittis. Nulla mollis, libero id maximus elementum, ante dolor auctor sem, sed volutpat mauris nisl non quam.
6  | 
7  | ---
8  | Phasellus id eleifend risus. In dui tellus, dignissim id viverra non, convallis sed ante. Suspendisse dignissim, felis vitae faucibus dictum, dui mi tempor lectus, non porta elit libero quis orci. Morbi porta neque quis magna imperdiet hendrerit.
9  | 
10 | ---
11 | Praesent eleifend commodo purus, sit amet viverra nunc dictum nec. Mauris vehicula, purus sed convallis blandit, massa sem egestas ex, a congue odio lacus non quam. Donec vitae metus vitae enim imperdiet tempus vitae sit amet quam. Nam sed aliquam justo, in semper eros. Suspendisse magna turpis, mollis quis dictum sit amet, luctus id tellus. Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia Curae; Aenean eu rutrum mi.
```

<div class = "notice info">
Remarque : la ligne supplémentaire après la colonne et avant le <code>---</code>. En effet, si vous mettez un triple tiret juste en dessous du texte, il est en fait interprété comme un en-tête.
</div>

Ensuite, nous devons simplement restituer ce contenu avec un modèle `columns.html.twig` (car le fichier de page s'appelait `columns.md`) :

```twig
1  | {% étend 'partials/base.html.twig' %}
2  | 
3  | {% block content %}
4  |   <table>
5  |        <tr>
6  |             {% for column in page.content|split('<hr />') %}
7  |             <td>{{ column|raw }}</td>
8  |            {% endfor %}
9  |        </tr>
10 |    </table>
11 | {% endblock %}
```

Vous pouvez voir comment le contenu est **divisé** par la balise `<hr />` et converti en un tableau de 3 colonnes que nous parcourons et restituons. Dans cet exemple, nous utilisons une simple balise de table HTML, mais vous pouvez utiliser tout ce que vous souhaitez.

<h2 id="Curseur d'image CSS vraiment simple">Curseur d'image CSS vraiment simple.
<a href="#Curseur d'image CSS vraiment simple" class="toc-anchor after"></a></h2>

<h3 id="Problème-3 :">Problème :
<a href="#Problème-3 :" class="toc-anchor after"></a></h3>

Vous avez besoin d'un curseur d'image sans frais généraux.

<h3 id="La solution-3 :">La solution :
<a href="#La solution-3 :" class="toc-anchor after"></a></h3>

Cette recette est pour 4 images et une page appelée `slider.md` ! Placez simplement les images là où se trouve le fichier .md. Ensuite, créez un nouveau modèle Twig et étendez `base.html.twig`.

```twig
1  | {% extends 'partials/base.html.twig' %}
2  |
3  | {% block content %}
4  | 
5  |    <div id="slider">
6  |        <figure>
7  |        {% for image in page.media.images %}
8  |             {{ image.html|raw }}
9  |         {% endfor %}
10 |         </figure>
11 |     </div>
12 | 
13 |     {{ page.content|raw }}
14 | {% endblock %}
```

Pour le curseur modulaire, veuillez retirer le

```twig
1 | {% extends 'partials/base.html.twig' %}
2 | 
3 | {% block content %}
```

et

    {% endblock %}

du fichier Twig précédent.

C'est l'heure des trucs CSS. Ajoutez ceci à votre _custom.scss

```css
1  | @keyframes slidy {
2  |     0% { left: 0%; }
3  |     20% { left: 0%; }
4  |     25% { left: -100%; }
5  |     45% { left: -100%; }
6  |     50% { left: -200%; }
7  |     70% { left: -200%; }
8  |     75% { left: -300%; }
9  |     95% { left: -300%; }
10 |    100% { left: -400%; }
11 | }
12 | body { margin: 0; }
13 | div#slider {
14 |     overflow: hidden;
15 |     margin-top: -3rem;
16 |     max-height: 30rem;
17 | }
18 | div#slider figure img { width: 20%; float: left; }
19 | div#slider figure {
20 |     position: relative;
21 |     width: 500%;
22 |     margin: 0;
23 |     left: 0;
24 |     animation: 30s slidy infinite;
25 | }
```

C'est tout.

<h2 id="Envelopper markdown en html">Envelopper markdown en html.
<a href="#Envelopper markdown en html" class="toc-anchor after"></a></h2>

Sur certaines pages, vous souhaiterez peut-être encapsuler des parties du contenu de démarquage dans un code html personnalisé au lieu de créer un nouveau modèle Twig.

Pour y parvenir, suivez ces étapes :

dans votre fichier de configuration système `user/config/system.yaml` assurez-vous d'activer l'option supplémentaire markdown :

```yaml
1 | pages:
2 |   markdown:
3 |     extra: true
```

dans votre balise wrapper assurez-vous d'ajouter le paramètre `markdown="1"` pour activer le traitement du contenu markdown :

```html
1 | <div class="monWrapper" markdown="1">
2 | # mon contenu markdown
3 |
4 | ce contenu est enveloppé dans un div avec la classe "myWrapper"
5 | </div>
```

Fini.

<h2 id="Ajouter un widget de publication récente à votre barre latérale">Ajouter un widget de publication récente à votre barre latérale.
<a href="#Ajouter un widget de publication récente à votre barre latérale" class="toc-anchor after"></a></h2>

<h3 id="Problème-4 :">Problème :
<a href="#Problème-4 :" class="toc-anchor after"></a></h3>

Vous souhaitez créer un widget de publication récente sur la barre latérale

<h3 id="La solution-4 :">La solution :
<a href="#La solution-4 :" class="toc-anchor after"></a></h3>

Il est toujours possible de créer un modèle partiel étendant `partials/base.html.twig` (voir d'autres solutions sur cette page), mais ici vous allez plutôt créer un modèle complet. Le code final de votre modèle Twig est présenté ci-dessous :

```html
1  | <div class="sidebar-content recent-posts">
2  |     <h3>Recent Posts</h3>
3  |     {% for p in page.find('/blog').children.order('date', 'desc').slice(0, 5) %}
4  |         {% set bannerimage = p.media['banner.jpg'] %}
5  |         <div class="recent-post">
6  |             {% if bannerimage %}
7  |                 <div class="recent-post-image">{{ bannerimage.cropZoom(60,60).quality(60) }}</div>
8  |             {% else %}
9  |                 <div class="recent-post-image"><img src="{{ url('theme://images/logo.png') }}" width="60" height="60"></div>
10 |             {% endif %}
11 |            <div class="recent-post-text">
12 |                <h4><a href="{{p.url}}">{{ p.title }}</a></h4>
13 |                 <p>{{ p.date|date("M j, Y")}}</p>
14 |             </div>
15 |         </div>
16 |     {% endfor %}
17 | </div>
```

Tout ce que fait ce code est de trier les enfants (articles de blog) de la page `/blog` par ordre décroissant de date. Il prend ensuite les cinq premiers articles de blog en utilisant le filtre `slice` Twig. Au fait, `slice(n,m)` prend des éléments de `n` à `m-1`. Dans cet exemple, tous les articles de blog comportant une image de bannière ont été nommés `banner.jpg`. Ceci est défini dans une variable `bannerimage`. Si `bannerimage` existe, elle est réduite à une boîte de `60px x 60px` et apparaîtra à gauche du texte et de la date du titre du message. S'il n'existe pas, le logo du site Web est redimensionné à `60 px x 60 px` et placé à gauche du texte du titre et de la date à la place.

Le CSS de ce widget est répertorié ci-dessous :

```css
1  | .sidebar-content .recent-post {
2  |     margin-bottom: 25px;
3  |     padding-bottom: 25px;
4  |     border-bottom: 1px solid #F0F0F0;
5  |     float: left;
6  |     clear: both;
7  |     width: 100%;
8  | }
9  | 
10 | .sidebar-content [class~='recent-post']:last-of-type {
11 |     border-bottom: none;
12 | }
13 | 
14 | .sidebar-content .recent-post .recent-post-image,
15 | .sidebar-content .recent-post .recent-post-text {
16 |     float: left;
17 | }
18 | 
19 | .sidebar-content .recent-post .recent-post-image {
20 |     margin-right: 10px;
21 | }
22 | 
23 | .sidebar-content .recent-post .recent-post-text h4 {
24 |     font-family: serif;
25 |     margin-bottom: 10px;
26 | }
27 | 
28 | .sidebar-content .recent-post .recent-post-text h4 a {
29 |     color: #193441;
30 | }
31 | 
32 | .sidebar-content .recent-post .recent-post-text p {
33 |     font-family: Arial, sans-serif;
34 |     font-size: 1.5rem;
35 |     color: #737373;
36 |     margin: 0;
37 | }
```

Ajustez l'espacement entre les éléments de publication récents, la famille de polices, la taille de police et l'épaisseur de police à votre goût.

<h2 id="Créer un espace privé">Créer un espace privé.
<a href="#Créer un espace privé" class="toc-anchor after"></a></h2>

Grav permet de créer très facilement un espace privé sur un site web. Tout fonctionne grâce au plugin de connexion.

<h2 id="Exiger que les utilisateurs se connectent avant d'accéder à une partie du site">Exiger que les utilisateurs se connectent avant d'accéder à une partie du site.
<a href="#Exiger que les utilisateurs se connectent avant d'accéder à une partie du site" class="toc-anchor after"></a></h2>

Si vous ne l'avez pas déjà, installez-le via le panneau d'administration ou à l'aide de l'utilitaire de ligne de commande GPM.

Ensuite, ouvrez une page dans Admin, passez en mode expert et dans la section FrontMatter ajoutez

    1 | access:
    2 |     site.login: true

Les utilisateurs accédant à la page devront se connecter avant de voir le contenu de la page.

Notez que l'autorisation ne s'étend pas par défaut aux sous-pages. Pour cela, depuis la configuration du plugin Login, activez "Utiliser les règles d'accès parent".

Cette option vous permet de créer des zones privées étendues sans vous soucier davantage du niveau d'accès. Il suffit de tout mettre sous une page dont l'accès est restreint.

<h2 id="Exiger des autorisations spéciales pour afficher une ou plusieurs pages">Exiger des autorisations spéciales pour afficher une ou plusieurs pages.
<a href="#Exiger des autorisations spéciales pour afficher une ou plusieurs pages" class="toc-anchor after"></a></h2>

De la même manière que pour le processus ci-dessus, vous pouvez attribuer n'importe quelle autorisation à une page. Vous pouvez même créer vos propres noms d'autorisation.

Par exemple:

    1 | access:
    2 |    site.onlybob: true

Ensuite, ajoutez la permission `site.onlybob` à Bob, dans son fichier utilisateur `bob.yaml` sous le dossier `user/accounts` :

    1 | access:
    2 |    site.onlybob: true

<h2 id="Utiliser les autorisations basées sur le groupe">Utiliser les autorisations basées sur le groupe.
<a href="#Utiliser les autorisations basées sur le groupe" class="toc-anchor after"></a></h2>

Vous pouvez également affecter des utilisateurs à un groupe et affecter des autorisations au groupe plutôt qu'à des utilisateurs individuels. Les utilisateurs hériteront des autorisations de groupe.

Ajoutez un fichier `user/config/groups.yaml`, par exemple avec ce contenu :

```yaml
1  | registered:
2  |   readableName: 'Registered Users'
3  |   description: 'The group of registered users'
4  |   access:
5  |     site:
6  |       login: true
7  | premium:
8  |   readableName: 'Premium Members'
9  |   description: 'The group of premium members'
10 |   access:
11 |     site:
12 |       login: true
13 |       paid: true
```

Attribuez maintenant des utilisateurs à un groupe en ajoutant

    1 | groups:
    2 |       - premium

à leur fichier utilisateur yaml, sous `user/accounts`

Désormais, les utilisateurs appartenant au groupe `premium` seront autorisés à accéder aux pages avec une autorisation `site.paid`.

<h2 id="Ajouter JavaScript au pied de page">Ajouter JavaScript au pied de page.
<a href="#Ajouter JavaScript au pied de page" class="toc-anchor after"></a></h2>

Dans de nombreux cas, vous voudriez que "certains" javascripts soient ajoutés au pied de page au lieu de l'en-tête de la page, à charger après le rendu du contenu.

Un bon exemple de cela est de vérifier le thème Antimatière.

Les `templates/partials/base.html.twig` d'Antimatter définissent un bloc inférieur pour js en appelant `{{ assets.js('bottom') }}`

```twig
1 | {% block bottom %}
2 |     {{ assets.js('bottom') }}
3 | {% endblock %}
```

Vous pouvez ajouter des actifs dans ce bloc dans Twig par exemple en appelant

`{% do assets.addJs('theme://js/slidebars.min.js', {group: 'bottom'}) %}`

ou en PHP en appelant

`$this->grav['assets']->addJs($this->grav['base_url'] . '/user/plugins/yourplugin/js/somefile.js', ['group' => 'bottom'] );`

<h2 id="Remplacer l'emplacement du dossier des journaux par défaut">Remplacer l'emplacement du dossier des journaux par défaut.
<a href="#Remplacer l'emplacement du dossier des journaux par défaut" class="toc-anchor after"></a></h2>

L'emplacement par défaut pour la sortie des journaux de Grav est simplement appelé `logs/`. Malheureusement, il existe des cas où ce dossier `logs/` est déjà utilisé ou est interdit. Le système de flux flexible de Grav permet de personnaliser les emplacements de ces dossiers.

Tout d'abord, vous devez créer votre nouveau dossier. Dans cet exemple, nous allons créer un nouveau dossier à la racine de votre installation Grav appelé `grav-logs/`. Créez ensuite un nouveau fichier de niveau racine appelé `setup.php` et collez le code suivant :

```php
1  | <?php
2  | use Grav\Common\Utils;
3  | 
4  | return [
5  |     'streams' => [
6  |         'schemes' => [
7  |             'log' => [
8  |                'type' => 'ReadOnlyStream',
9  |                'prefixes' => [
10 |                    '' => ["grav-logs"],
11 |                ]
12 |             ]
13 |         ]
14 |     ]
15 | ];
```

Cela remplace essentiellement le flux de journaux avec le dossier `grav-logs/` plutôt que le dossier `logs/ `par défaut tel que défini dans `system/src/Grav/Common/Config/Setup.php`.

<h2 id="Système de menu vertical divisé">Système de menu vertical divisé.
<a href="#Système de menu vertical divisé" class="toc-anchor after"></a></h2>

Pour créer un menu de pages vertical, pliable et hiérarchique, vous avez besoin d'une boucle Twig, d'un peu de CSS et d'un peu de JavaScript. Le résultat final, lors de l'utilisation du thème Antimatière, ressemblera à ceci :

![Menu vertical](https://learn.getgrav.org/user/pages/10.cookbook/01.general-recipes/vertical_menu.png)

Commençons par Twig :

```html
1  | <ol class="tree">
2  |     {% for page in pages.children.visible %}
3  |         {% if page.children.visible is empty %}
4  |             <li class="item">
5  |             <a href="{{ page.url }}">{{ page.title }}</a>
6  |         {% else %}
7  |             <li class="parent">
8  |             <a href="javascript:void(0);">{{ page.title }}</a>
9  |             <ol>
10 |                 {% for child in page.children.visible %}
11 |                     {% if child.children.visible is empty %}
12 |                         <li class="item">
13 |                         <a href="{{ child.url }}">{{ child.title }}</a>
14 |                     {% else %}
15 |                         <li class="parent">
16 |                         <a href="javascript:void(0);">{{ child.title }}</a>
17 |                         <ol>
18 |                             {% for subchild in child.children.visible %}
19 |                                 <li><a href="{{ subchild.url }}">{{ subchild.title }}</a></li>
20 |                            {% endfor %}
21 |                         </ol>
22 |                     {% endif %}
23 |                     </li>
24 |                 {% endfor %}
25 |             </ol>
26 |         {% endif %}
27 |         </li>
28 |     {% endfor %}
29 | </ol>
```

Cela crée une liste ordonnée qui itère sur toutes les pages visibles dans Grav, en allant sur trois niveaux pour créer une structure pour chaque niveau. La liste enroulée autour de la structure entière a *l'arborescence* des classes, et chaque élément de la liste a le *parent* de la classe s'il contient des enfants ou un *élément* s'il n'en a pas.

Cliquer sur un parent ouvre la liste, tandis que les éléments réguliers renvoient à la page elle-même. Vous pouvez l'ajouter à pratiquement n'importe quel modèle Twig dans un thème Grav, à condition que Grav puisse accéder aux pages visibles.

Pour ajouter du style, nous ajoutons du CSS :

```css
1  | <style>
2  | ol.tree li {
3  |      position : relative ;
4  | }
5  | ol.tree li ol {
6  |      affichage : aucun ;
7  | }
8  | ol.tree li.open > ol {
9  |      bloc de visualisation;
10 | }
11 | ol.tree li.parent:après {
12 |      contenu : '[+]' ;
13 | }
14 | ol.tree li.parent.open:après {
15 |      contenu: '';
16 | }
17 | </style>
```

Cela devrait généralement être placé avant la structure Twig, ou idéalement être diffusé dans le [gestionnaire d'actifs](/asset-manager) de votre thème. L'effet est d'ajouter **[+]** après chaque élément parent, indiquant qu'il peut être ouvert, qui disparaît lorsqu'il est ouvert.

Enfin, ajoutons un peu de JavaScript pour [gérer le basculement](https://stackoverflow.com/a/36297446/603387) de la classe *ouverte* :

```html
1  | <script type="text/javascript">
2  | var tree = document.querySelectorAll('ol.tree a:not(:last-child)');
3  | for(var i = 0; i < tree.length; i++){
4  |     tree[i].addEventListener('click', function(e) {
5  |         var parent = e.target.parentElement;
6  |         var classList = parent.classList;
7  |         if(classList.contains("open")) {
8  |             classList.remove('open');
9  |             var opensubs = parent.querySelectorAll(':scope .open');
10 |             for(var i = 0; i < opensubs.length; i++){
11 |                 opensubs[i].classList.remove('open');
12 |             }
13 |         } else {
14 |             classList.add('open');
15 |         }
16 |     });
17 | }
18 | </script>
```

Celui-ci doit toujours être placé après la structure Twig, idéalement également dans [l'Asset Manager](/asset-manager)</span>.

<h2 id="Styliser dynamiquement une ou plusieurs pages">Styliser dynamiquement une ou plusieurs pages.
<a href="#Styliser dynamiquement une ou plusieurs pages" class="toc-anchor after"></a></h2>

Vous pouvez styliser dynamiquement différentes pages/publications de votre site Grav (indépendamment de l'affectation du fichier de modèle) en personnalisant le fichier Twig d'un thème pour appliquer une classe CSS transmise en tant que variable dans le FrontMatter d'une page.

Vous pouvez styliser différents articles/pages de votre site Grav de deux manières :

1. Si vous utilisez le thème Antimatière, vous pouvez utiliser la propriété d'en-tête `body_classes` existante pour définir votre classe CSS personnalisée pour cette page.
2. Si vous utilisez un thème non basé sur Antimatière (ou n'implémentant pas `body_classes` comme il le fait), vous pouvez personnaliser un fichier Twig du thème pour appliquer une classe CSS passée en tant que variable dans la propriété d'en-tête d'une page.

Par exemple, dans le fichier `base.html.twig` de votre thème ou un modèle plus spécifique tel que le fichier `page.html.twig`, vous pouvez ajouter une classe à l'affichage du contenu de la page, comme :

```html
1 | <div class="{{ page.header.body_classes }}">
2 | ...
3 | </div>
```

Ensuite, pour chaque page que vous souhaitez avoir un style unique, vous ajouterez la propriété d'en-tête suivante (en supposant que vous avez défini une classe CSS pour `featurepost`) :

    1 | body_classes : featurepost

Remarque : c'est ainsi que le thème Antimatière applique des classes spécifiques à la page, et c'est donc une bonne norme à suivre.

<h2 id="Migrer un thème HTML vers Grav">Migrer un thème HTML vers Grav.
<a href="#Migrer un thème HTML vers Grav" class="toc-anchor after"></a></h2>

Migrer un thème HTML vers Grav est une tâche courante. Voici un processus pratique étape par étape qui peut être utilisé pour atteindre cet objectif.

Vous avez probablement téléchargé le thème, et il est composé de plusieurs fichiers HTML. Commençons simplement par faire en sorte que Grav charge la page d'accueil. Pas de contenu personnalisé, répliquez simplement le thème HTML, mais dans une structure Grav.

Tout d'abord, [utilisez le plugin Grav Devtools](/tutoriel-theme) pour créer un thème vierge et configurez Grav pour l'utiliser dans les paramètres système.

Créez un template `templates/home.html.twig` Twig dans le dossier templates du thème. Cela représentera un modèle spécifique pour la page d'accueil. Habituellement, la page d'accueil est une page unique sur le site, elle mérite donc probablement un fichier Twig dédié.

Copiez le code HTML de la page d'accueil du modèle, commençant à `<html>` et se terminant à `</html>` dans votre nouveau fichier `home.html.twig`.

Maintenant, déplacez tous les éléments du thème HTML (images, CSS, JS) dans votre dossier de thème. Vous pouvez conserver la structure de dossiers de thèmes existante ou la modifier.

Créez un fichier `pages/01.home/home.md` vide. Pointez maintenant votre navigateur vers yoursite.com/home : il devrait afficher le contenu, mais le CSS, le JS et les images ne seront pas chargés, probablement parce que le thème les a codés en dur sous forme de liens `/img/*` ou /`css/*`.

<h3 id="Ajouter les bons liens d'actifs">Ajouter les bons liens d'actifs.
<a href="#Ajouter les bons liens d'actifs" class="toc-anchor after"></a></h3>

Dans Grav, les liens sont rompus car ils pointent vers la route d'accueil, donc au lieu de pointer vers `/user/themes/mytheme/img`, ils pointent vers `/img` dans la racine Grav. Puisqu'il est préférable de conserver tous les éléments liés au thème à l'intérieur du thème, nous devons pointer Grav vers l'emplacement correct.

Recherchez des ressources dans la page et modifiez les références des images de `img/*.*` à `<img src="{{ url('theme://img/*.*', true) }}" />`.

Les feuilles de style nécessitent un peu plus de réflexion car il y a un pipeline d'actifs que nous voudrons activer à un moment donné, nous les déplaçons donc vers un bloc de feuilles de style dans la balise `<head>`.

Exemple:

```twig
1 | {% block stylesheets %}
2 |     {% do assets.addCss('theme://css/styles.min.css', 100) %}
3 | {% endblock %}
4 | {{ assets.css()|raw }}
```

Il en va de même pour les fichiers JavaScript, avec l'exigence supplémentaire que du JS soit chargé dans le pied de page.

Exemple:

```twig
1 | {% block javascripts %}
2 |     {% do assets.addJs('theme://js/custom.js') %}
3 |     {% do assets.addJs('jquery', 101) %}
4 | {% endblock %}
5 | {{ assets.js()|raw }}
```

Les changements de page devraient maintenant être affichés dans votre navigateur. Si ce n'est pas le cas, assurez-vous que le cache des pages et le cache des brindilles sont désactivés dans les paramètres de configuration du système Grav.

Ce n'est que le début. Maintenant, vous devrez peut-être ajouter plus de pages et trouver de meilleures façons de présenter le contenu de vos pages à l'aide de l'en-tête FrontMatter et de Twig personnalisé qui traite les éléments de base habituels : les témoignages de la page d'accueil, les critiques, les fonctionnalités du produit, etc. .

<h3 id="Ajouter une autre page">Ajouter une autre page.
<a href="#Ajouter une autre page" class="toc-anchor after"></a></h3>

Pour ajouter une autre page, le processus est similaire. Par exemple, supposons que vous souhaitiez ensuite créer la page de blog. Répétez le processus pour ajouter un fichier `templates/blog.html.twig`, collez la source HTML et créez une page `pages/02.blog/blog.md.`

Maintenant, alors que les liens d'images à l'intérieur des pages doivent encore être migrés vers la syntaxe des actifs de Grav (ou simplement changer le chemin), vous ne voulez pas répéter le même travail que vous avez fait ci-dessus pour les actifs CSS et JS. Cela devrait être réutilisé sur l'ensemble du site.

<h3 id="Éléments partagés">Éléments partagés.
<a href="#Éléments partagés" class="toc-anchor after"></a></h3>

Identifiez les parties communes des pages (en-tête et pied de page) et déplacez-les vers le fichier `templates/partials/base.html.twig`.

Chaque modèle de page doit ensuite étendre `partials/base.html.twig` (https://github.com/getgrav/grav-theme-antimatter/blob/develop/templates/default.html.twig#L1) et ajouter simplement leur unique contenu.

<h2 id="Ajouter un élément à une page spécifique">Ajouter un élément à une page spécifique.
<a href="#Ajouter un élément à une page spécifique" class="toc-anchor after"></a></h2>

<h3 id="Problème-5 :">Problème :
<a href="#Problème-5 :" class="toc-anchor after"></a></h3>

Vous devez ajouter un atout à un modèle spécifique sur votre thème.

<h3 id="La solution-5 :">La solution :
<a href="#La solution-5 :" class="toc-anchor after"></a></h3>

La plupart du temps, vos actifs seront ajoutés à l'intérieur d'un bloc de brindilles dans votre modèle de base comme ci-dessous.

```twig
1 | {% block javascripts %}
2 |     {% do assets.addJs('theme://js/jquery.js', 91) %}
3 | {% endblock %}
4 | {{ assets.js()|raw }}
```

Afin d'ajouter votre actif, vous devez étendre ce bloc dans votre modèle et appeler `{{ parent() }}` qui obtiendra les actifs déjà ajoutés dans votre modèle de base. Supposons que vous souhaitiez ajouter un fichier "gallery.js" sur votre page "Portfolio Gallery". Modifiez votre modèle et ajoutez votre élément avec le `{{ parent() }}`.

```twig
1 | {% block javascripts %}
2 |   {% do assets.addJs('theme://js/gallery.js', 100) %}
3 |   {{ parent() }}
4 | {% endblock %}
```

<h2 id="Réutiliser la page ou le contenu modulaire sur une autre page">Réutiliser la page ou le contenu modulaire sur une autre page.
<a href="#Réutiliser la page ou le contenu modulaire sur une autre page" class="toc-anchor after"></a></h2>

<h3 id="Problème-6 : ">Problème : 
<a href="#Problème-6 : " class="toc-anchor after"></a></h3>

Vous avez de nombreuses pages ou modules et souhaitez partager le même bloc de contenu sur plusieurs pages sans avoir à gérer plusieurs instances distinctes du même texte.

<h3 id="La solution-6 :">La solution :
<a href="#La solution-6 :" class="toc-anchor after"></a></h3>

Il s'agit d'une méthode directe très simple qui ne nécessite pas de plugin et peut être utilisée dans le panneau d'administration.

**Remarque :** Il existe également un plugin [Grav Page Inject Plugin](https://github.com/getgrav/grav-plugin-page-inject) pour cette fonctionnalité qui peut convenir à des scénarios plus avancés.

Tout d'abord, créez un nouveau fichier de modèle pour agir comme un espace réservé pour le contenu - il peut avoir n'importe quel nom, celui-ci est nommé "modular*reuse" et sera stocké dans le dossier templates/modular de votre thème* pour cet exemple mais peut être stocké n'importe où dans le dossier des modèles.

`modular_reuse.html.twig` ne contient qu'une seule ligne :

    1 | {{ page.content|brut }}

Ensuite, créez une nouvelle page modulaire dans le panneau d'adm*inistration où ce contenu doit être affiché à l'aide de ce nouveau modèle de "réutilisation modulaire". Le nouveau nom de page peut être ce que vous voulez car il ne sera pas affiché - le titre de la page d'origine sera affiché.

Le contenu de la page est d'une seule ligne : Page :

    1 | {% include 'modular_reuse.html.twig' with {'page': page.find('/test-page/amazing-offers')} %}

Modulaire :

    1 | {% include 'modular/modular_reuse.html.twig' with {'page': page.find('/test-page/_amazing-offers')} %}

Ce qui vient après "include" est l'endroit où le modèle de la première étape est stocké, probablement dans le dossier `templates` pour les pages du dossier `templates/modular` pour les modulaires.

Après page.find devrait venir le lien réel vers le contenu original que vous souhaitez réutiliser. Le contenu modulaire commence par un _ mais pas les pages. Le moyen le plus simple de trouver le bon lien est d'ouvrir la page dans le panneau d'administration et de copier l'URL après le mot admin.

La page finale devrait ressembler à ceci :

```html
1 | ---
2 | title: 'Modular Reuse Example'
3 | ---
4 | 
5 | {% include 'modular/modular_reuse.html.twig' with {'page': page.find('/test-page/_amazing-offers')} %}
```

Désormais, les "offres incroyables" peuvent être affichées à plusieurs endroits, mais ne doivent être mises à jour qu'une seule fois.

<h2 id="Créez un champ anti-spam personnalisé pour votre formulaire de contact">Créez un champ anti-spam personnalisé pour votre formulaire de contact.
<a href="#Créez un champ anti-spam personnalisé pour votre formulaire de contact" class="toc-anchor after"></a></h2>

<h3 id="Problème-7 :">Problème :
<a href="#Problème-7 :" class="toc-anchor after"></a></h3>

Les méthodes normales de prévention du spam, comme le champ de pot de miel, sont contournées par certains spam-bots.

<h3 id="La solution-7 :">La solution :
<a href="#La solution-7 :" class="toc-anchor after"></a></h3>

Faites en sorte qu'il soit plus difficile pour le bot de deviner ce qu'il peut et ne peut pas remplir lorsqu'il remplit le formulaire de contact. En termes simples, posez une question à laquelle l'utilisateur ne manquera pas de répondre, mais dont les réponses sont difficiles à comprendre pour un bot. Dans votre fichier Markdown avec [Form-data](/formulaires-ex-formulaire-contact), ajoutez ce champ :

```configuration
1  | - name: personality
2  |       type: radio
3  |       label: What is five times eight?
4  |       options:
5  |         alaska: 32
6  |         oklahoma: 40
7  |         california: 48
8  |       validate:
9  |         required: true
10 |         pattern: "^oklahoma$"
11 |         message: Not quite, try that math again.
```

La question devrait être quelque chose de simple, mais accompagnée de plusieurs mauvaises réponses simples. Ce qui compte, c'est l'ordre des réponses. La bonne réponse ne devrait jamais être la première ; viser quelque part au milieu. Il est important de randomiser vous-même les valeurs derrière les réponses (étiquettes), de sorte qu'une base de données de valeurs et de réponses associées n'aidera pas à répondre.

Les robots deviennent de plus en plus intelligents, mais ils ont tendance à renoncer à répondre plusieurs fois à la même question si la première tentative échoue. De plus, même les plus intelligents d'entre eux s'appuient sur des dictionnaires de données connues pour deviner une réponse. Nous posons une question simple, "Qu'est-ce que cinq fois huit?", Et donnons trois options, "32", "40" et "48". La bonne réponse est évidemment "40", mais au lieu de vérifier les compétences en mathématiques du bot, nous attribuons respectivement les valeurs "alaska", "oklahoma" et "californie" à ces nombres. Parce que les bots regardent les valeurs possibles, plutôt que leur étiquette, les réponses n'ont aucun rapport avec la question. Vous pouvez même ajouter une réponse "Ananas" avec la valeur "mississippi" et valider par rapport à cela, et simplement dire à vos utilisateurs de choisir cela comme réponse. Il s'agit de personnaliser la randomisation des données.

<h2 id="Afficher différents contenus robots.txt pour différents environnements">Afficher différents contenus robots.txt pour différents environnements.
<a href="#Afficher différents contenus robots.txt pour différents environnements" class="toc-anchor after"></a></h2>

<h3 id="Problème-8 :">Problème :
<a href="#Problème-8 :" class="toc-anchor after"></a></h3>

Vous avez configuré un sous-domaine, `dev.votredomaine.com`, en tant que site de développement pour prévisualiser ce sur quoi vous travaillez avant de publier des modifications sur `votredomaine.com`, et souhaitez interdire aux indexeurs de recherche de l'explorer tout en gardant le site de production visible dans la recherche résultats.

<h3 id="La solution-8 :">La solution :
<a href="#La solution-8 :" class="toc-anchor after"></a></h3>

Bien que vous deviez protéger votre site de développement par un mot de passe pour le garder vraiment privé, il est parfois suffisant, et simplement plus pratique, d'interdire simplement aux indexeurs des moteurs de recherche d'explorer votre site. Heureusement, Grav peut gérer les pages au format txt comme il le fait en html, nous pouvons donc utiliser des [configurations d'environnement](avance-config-environnement) et des modèles Twig pour terminer le travail.

Commençons par créer un fichier de configuration `site.yaml` qui indiquera à notre modèle que `dev.votredomaine.com` est un environnement de développement.

`/user/[dev.votredomaine.com]/config/site.yaml :`

`environment : dev`

Ensuite, créez un modèle de page `robots.txt.twig` qui vérifie si Grav est actuellement en cours d'exécution sur notre site de développement et affiche un contenu différent si c'est le cas.

`/user/themes/[votre thème]/templates/robots.txt.twig :`

```twig
1 | {% if config.site.environment == 'dev' %}
2 | {% for rule in page.header.dev %}
3 | {{ rule }}
4 | {% endfor %}
5 | 
6 | {% else %}
7 | {{ page.content|raw }}
8 | 
9 | {% endif %}
```

Enfin, créez une page routée vers `/robots.txt` avec les règles `robots.txt` par défaut dans le contenu de la page, et nos règles de version de développement alternative dans le frontmatter de la page. Pour rendre le contenu de la page sous forme de texte brut au lieu de HTML, nous désactiverons également le rendu Markdown.

`/user/pages/robots/robots.md :`

```configuration
1  | ---
2  | routes:
3  |   default: /robots.txt
4  | process:
5  |   markdown: false
6  | 
7  | dev:
8  |   - 'User-agent: *'
9  |   - 'Disallow: /'
10 | ---
11 |
12 | User-agent: *
13 | Disallow: /backup/
14 | Disallow: /bin/
15 | Disallow: /cache/
16 | Disallow: /grav/
17 | Disallow: /logs/
18 | Disallow: /system/
19 | Disallow: /vendor/
20 | Disallow: /user/
21 | Allow: /user/pages/
22 | Allow: /user/themes/
23 | Allow: /user/images/
24 | Allow: /user/plugins/*.css$
25 | Allow: /user/plugins/*.js$
```

Vous devriez maintenant avoir un fichier `robots.txt` placé à la racine de votre site avec un contenu dynamique, également modifiable avec le plugin Admin.

Remarque : assurez-vous que votre site de production n'affichera pas `Disallow: /`, car cela oblitérera complètement la visibilité de votre moteur de recherche.

