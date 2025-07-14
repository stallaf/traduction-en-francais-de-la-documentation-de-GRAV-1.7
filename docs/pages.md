<h1 class = "rem">Pages</h1>

Dans le vocabulaire de Grav, les **Pages** sont les éléments fondamentaux de votre site. Ils vous permettent d'écrire du contenu et de naviguer dans le système Grav.

La combinaison du contenu et de la navigation garantit que le système est intuitif à utiliser, même pour les auteurs de contenu les plus inexpérimentés. Cependant, ce système, associé à de puissantes capacités de taxonomie, reste suffisamment puissant pour gérer des exigences de contenu complexes.

Grav prend en charge nativement **3 types de page**s qui vous permettent de créer une riche sélection de contenu Web. Ces types sont :

![3 type de pages](https://learn.getgrav.org/user/pages/02.content/01.content-pages/page-types.png)

<h3 id="Page standard">Page standard
<a href="#Page standard" class="toc-anchor after"></a></h3>

![installation réussie](https://learn.getgrav.org/user/pages/01.basics/03.installation/install.png)

Une page standard est généralement une page unique telle qu'un **article de blog, un formulaire de contact, une page d'erreur**, etc. Il s'agit du type de page le plus courant que vous allez créer. Par défaut, une page est considérée comme une page normale, sauf si vous indiquez le contraire à Grav.

Lorsque vous téléchargez et installez le package **Core Grav**, vous êtes accueilli par une page standard. Nous avons couvert la création d'une simple page régulière dans le [tutoriel de base](./tutoriel.md).

<h3 id="Page de liste">Page de liste
<a href="#Page de liste" class="toc-anchor after"></a></h3>

![Page de liste](https://learn.getgrav.org/user/pages/02.content/01.content-pages/content-listing.png)

Il s'agit d'une extension d'une page normale. Il s'agit d'une page qui fait référence à une collection de pages.

L'approche la plus simple pour configurer cela consiste à créer des **pages enfants** sous la page de liste. Un exemple de ceci serait une **page de liste de blogs**, où vous afficheriez une liste récapitulative des articles de blog qui existent en tant que pages enfants.

Il existe également des paramètres de configuration pour **contrôler l'ordre** de la liste ainsi qu'une **limite sur le nombre d'éléments**, et si la **pagination** doit être activée ou non.

 <div class = "notice info">
Un exemple de <strong>squelette de blog</strong> utilisant une <strong>page de liste</strong> peut être trouvé dans les <a href ="https://getgrav.org/downloads/skeletons)">téléchargements Grav</a>.
</div>

<h3 id="Page modulaire">Page modulaire
<a href="#Page modulaire" class="toc-anchor after"></a></h3>

![Page modulaire](https://learn.getgrav.org/user/pages/02.content/01.content-pages/content-modular.png)

Une **Page Modulaire** est un type spécial de page de liste car elle construit une **seule page** à partir de ses **pages** ou **modules enfants**. Cela permet de créer des mises en page **d'une page très complexes** à partir de modules. Ceci est accompli en construisant la **Page Modulaire** à partir de plusieurs **Modules** trouvés dans le dossier principal de la page modulaire.

 <div class = "notice info">
Un exemple de <strong>squelette de blog</strong> utilisant une <strong>page de liste</strong> peut être trouvé dans les <a href ="https://getgrav.org/downloads/skeletons)">téléchargements Grav</a>.
</div>

Chacun de ces types de pages suit la même structure de base, donc avant de pouvoir entrer dans les détails de chaque type, nous devons expliquer comment les pages de Grav sont construites.

 <div class = "notice info">
Un module, parce qu'il est destiné à faire partie d'une autre page, n'est par nature pas une page à laquelle vous pouvez accéder directement via une URL. Pour cette raison, toutes les pages de module sont définies par défaut comme <strong>non routables</strong>.
</div>

<h2 id="Dossiers">Dossiers
<a href="#Dossiers" class="toc-anchor after"></a></h2>

Toutes les pages de contenu se trouvent dans le dossier `/user/pages`. Chaque **page** doit être placée dans son propre dossier.

 <div class = "notice info">
Les noms de dossier doivent également être des <strong>slugs</strong> valides. Les slugs sont entièrement en minuscules, avec des caractères accentués remplacés par des lettres de l'alphabet latin et des caractères d'espacement remplacés par un tiret ou un trait de soulignement, pour éviter d'être encodés.
</div>

Grav comprend que toute valeur entière suivie d'un point sera uniquement destinée à la commande et est supprimée en interne dans le système. Par exemple, si vous avez un dossier nommé `01.home`, Grav traitera ce dossier comme `home`, mais s'assurera qu'avec l'ordre par défaut, il vient avant `02.blog`.

<div class = "notice cadre">
<pre>
1  |    /user
2  |    └── /pages
3  |        ├── /01.home
4  |        │   ├── /_header
5  |        │   ├── /_features
6  |        │   ├── /_body
7  |        ├── /02.blog
8  |        │   ├── /blog-item-1
9  |        │   ├── /blog-item-2
10 |        │   ├── /blog-item-3
11 |        │   ├── /blog-item-4
12 |        │   └── /blog-item-5
13 |        ├── /03.about-us
14 |        └── /error
</pre>
</div>

Votre site doit avoir un point d'entrée afin qu'il sache où aller lorsque vous pointez votre navigateur vers la racine de votre site. Par exemple, si vous deviez saisir `http://votresite.com` dans votre navigateur, Grav attend par défaut un alias `home/`, mais vous pouvez remplacer l'emplacement de la page d'accueil en modifiant l'option `home.alias` dans le [fichier de configuration de Grav](./configuration.md).

Les **Modules** sont identifiés par un trait de soulignement `(_)` avant le nom du dossier. Il s'agit d'un type de dossier spécial destiné à être utilisé uniquement avec du **contenu modulaire**. Ceux-ci ne sont **pas routables** et ne sont **pas visibles** dans la navigation. Un exemple de configuration de page modulaire serait un dossier tel que `user/pages/01.home`. Home est configuré comme une **page modulaire** qui contiendrait une collection de **modules** et serait construit à partir des dossiers `_header`, `_features` et `_body` dans le dossier home.

Le nom textuel de chaque dossier est par défaut le slug que le système utilise dans le cadre de l'URL. Par exemple, si vous avez un dossier tel que `/user/pages/02.blog`, le slug de cette page sera par défaut `blog` et l'URL complète sera `http://yoursite.com/blog`. Une page d'élément de blog, située dans `/user/pages/02.blog/blog-item-5` serait accessible via `http://yoursite.com/blog/blog-item-5`.

Si aucun numéro n'est fourni comme préfixe du nom du dossier, la page est considérée comme **invisible** et n'apparaîtra pas dans la navigation. Un exemple de ceci serait la page `error` dans la structure de dossiers ci-dessus.

<div class = "notice info">
Cela peut en fait être remplacé dans la page elle-même en définissant le <a href="/en-tete-frontmatter#visible">paramètre visible</a> dans les en-têtes.
</div>

<h2 id="Commande">Commande
<a href="#Commande" class="toc-anchor after"></a></h2>

Lorsqu'il s'agit de collections, plusieurs options sont disponibles pour contrôler l'ordre des dossiers. L'option la plus importante est définie dans le `content.order.by` des paramètres de configuration de la page. Les options sont :

| PROPRIÉTÉS                | DESCRIPTION
| :----------------------   | :-----------------------
| **default**               | L'ordre est basé sur le système de fichiers, c'est-à-dire `01.home` avant `02.advark`.
| **title**                 | L'ordre est basé sur le titre tel que défini dans chaque page.
| **basename**              | L'ordre est basé sur le dossier alphabétique sans ordre numérique.
| **date**                  | L'ordre est basé sur la date telle que définie dans chaque page.
| **modified**              | L'ordre est basé sur l'horodatage modifié de la page.
| **folder**                | L'ordre est basé sur le nom du dossier avec tout préfixe numérique, c'est-à-dire `01.`, supprimé.
| **header.x**              | L'ordre est basé sur n'importe quel champ d'en-tête de page. c'est-à-dire `header.taxonomy.year`.<br>Une valeur par défaut peut également être ajoutée via un tube. c'est-à-dire `header.taxonomy.year|2015`.
| **manual**                | La commande est basée sur la variable `order_manual`.
| **random**                | La commande est aléatoire.

Vous pouvez définir spécifiquement une commande manuelle en fournissant une liste d'options au paramètre de configuration `content.order.custom`. Cela fonctionnera en conjonction avec `content.order.by` car il essaie d'abord de trier les pages manuellement, mais toutes les pages non spécifiées dans la commande manuelle échoueront et seront triées selon la commande fournie.

<div class = "notice info">
Vous pouvez remplacer le <strong>comportement par défaut</strong> pour l'ordre des dossiers et la direction dans laquelle l'ordre se produit en définissant les options <code>pages.order.dir</code> et <code>pages.order.by</code> dans le <a href="/configuration">fichier de configuration du système Grav</a>.
</div>

<h2 id="Fichier de page">Fichier de page
<a href="#Fichier de page" class="toc-anchor after"></a></h2>

Dans le dossier de page, nous créons le fichier de page proprement dit. Le nom du fichier doit se terminer par `.md` pour indiquer qu'il s'agit d'un fichier au format Markdown. Techniquement, c'est Markdown avec YAML FrontMatter, ce qui semble impressionnant mais ce n'est vraiment pas grave du tout. Nous couvrirons les détails de la structure du fichier bientôt.

La chose importante à comprendre est que le nom du fichier fait directement référence au nom du fichier modèle du thème qui sera utilisé pour le rendu. Le nom standard du fichier de modèle principal est **default**, le fichier s'appellera donc `default.md`.

Vous pouvez, bien sûr, nommer votre fichier comme vous le souhaitez, par exemple : `document.md`, ce qui obligerait Grav à rechercher un fichier de modèle dans le thème correspondant, tel que **document.html.twig** Twig-template.

<div class = "notice info">
Ce comportement peut être remplacé dans la page en définissant le <a href="/en-tete-frontmatter#template">paramètre de modèle</a> dans les en-têtes.
</div>

Un exemple de fichier de page pourrait ressembler à ceci :

```yaml
1  | ---
2  | titre : Titre de la page
3  | taxonomie :
4  |    catégorie : blog
5  | ---
6  | # Titre de la page
7  |
8  | Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque porttitor eu
9  | felis sed ornaré. Sed a mauris venenatis, pulvinar velit vel, dictum enim. Phasellus
10 | ac rutrum velit. **Nunc lorem** purus, hendrerit sit amet augue aliquet, iaculis
11 | ultricies nisl. Suspendisse tincidunt euismod risus, _quis feugiat_ arcu tincidunt
12 | égète. Nulla eros mi, commodo vel ipsum vel, aliquet congue odio. Classe aptent taciti
13 | sociosqu ad litora torquent per conubia nostra, per inceptos himenaeos. Pellentesque
14 | velit orci, laoreet et adipiscing eu, interdum quis nibh. Nunc a accumsan purus.
```

Les paramètres entre la paire de marqueurs `---` sont connus sous le nom de FrontMatter YAML et comprennent les paramètres YAML de base de la page.

Dans cet exemple, nous définissons explicitement le titre, ainsi que la taxonomie sur **blog** afin que nous puissions le filtrer plus tard. Le contenu après le deuxième `---` est le contenu réel qui sera compilé et rendu au format HTML sur votre site. Ceci est écrit en [Markdown](./syntaxe-markdown.md), qui sera traité en détail dans un prochain chapitre. Sachez simplement que les marqueurs `#`, `**` et `_` se traduisent respectivement en **titre 1**, en **gras** et en **italique**.

<div class = "notice info">
Assurez-vous d'enregistrer vos fichiers <code>.md</code> en tant que fichiers encodés en <code>UTF-8</code>. Cela garantira qu'ils fonctionnent avec des caractères spéciaux spécifiques à la langue.
</div>

<h2 id="Taille du résumé et séparateur">Taille du résumé et séparateur
<a href="#Taille du résumé et séparateur" class="toc-anchor after"></a></h2>

Il existe un paramètre dans le fichier `site.yaml` qui vous permet de définir une taille par défaut (en caractères) du résumé qui peut être utilisé via `page.summary(`) pour afficher un résumé ou un synopsis de la page. Ceci est particulièrement utile pour les blogs où vous souhaitez avoir une liste contenant uniquement des informations récapitulatives, et non le contenu de la page entière.

Par défaut, cette valeur est de `300` caractères. Vous pouvez remplacer cela dans votre fichier `user/config/site.yaml`, mais une approche encore plus utile consiste à utiliser le **séparateur de résumé** manuel également connu sous le nom de **délimiteur de résumé** : `===`.

Vous devez vous assurer de le mettre dans votre contenu avec des lignes vides **au-dessus** et en **dessous**. Par exemple:

```console
1  | Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
2  | tempor inciddunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, 
3  | quis nostrud exercice ullamco laboris nisi ut aliquip ex ea commodo
4  | consequat.
5  |
6  | ===
7  |
8  | Duis aute irure dolor in reprehenderit in voluptate velit esse
9  | cillum dolore eu fugiat nulla pariatur. Sauf sint occaecat cupidatat non
10 | proident, sunt in culpa qui officia deserunt mollit anim id est laborum. 
     Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
11 | tempor inciddunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
12 | quis nostrud exercice ullamco laboris nisi ut aliquip ex ea commodo
13 | conséquent. Duis aute irure dolor in reprehenderit in voluptate velit esse
14 | cillum dolore eu fugiat nulla pariatur. Sauf sint occaecat cupidatat non
15 | proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
```

Cela utilisera le texte au-dessus du séparateur lorsqu'il est référencé par `page.summary()` et le contenu complet de la page lorsqu'il est référencé par `page.content()`.

<div class = "notice info">
Lors de l'utilisation de <code>page.summary()</code>, le paramètre de taille du résumé sera utilisé si le séparateur n'est pas trouvé dans le contenu de la page.
</div>

<h2 id="Trouver d'autres pages">Trouver d'autres pages
<a href="#Trouver d'autres pages" class="toc-anchor after"></a></h2>

Grav a une fonctionnalité utile qui vous permet de trouver une autre page et d'effectuer des actions sur cette page. Cela peut être accompli avec la méthode `find()` qui prend simplement la **route** et renvoie un nouvel objet Page.

Cela vous permet d'exécuter une grande variété de fonctionnalités à partir de n'importe quelle page de votre site Grav. Par exemple, vous pouvez fournir une liste de tous les projets en cours sur une page de détails de projet particulière :

```html
1 | # Tous les projets
2 | <ul>
3 | {% for p in pages.find('/projects').children if p != page %}
4 | <li><a href="{{ p.url|e }}">{{ p.title|e }}</a></li>
5 | {% endfor %}
6 | </ul>
```

Dans les sections suivantes, nous continuerons à approfondir les spécificités d'une page et des collections de pages en détail.

<h2 id="contentMeta">contentMeta
<a href="#contentMeta" class="toc-anchor after"></a></h2>

Le référencement des pages et du contenu est simple, mais qu'en est-il du contenu qui n'est pas rendu sur le front-end avec le reste de la page ?

Lorsque Grav lit le contenu de la page, il stocke ce contenu dans le cache. Ainsi, la prochaine fois que la page sera rendue, elle n'aura pas à lire tout le contenu du fichier `.md`. Généralement, ce contenu est entièrement rendu au front-end. Cependant, il existe des cas où il est utile d'avoir des données supplémentaires stockées à côté de la page dans le cache.

C'est là qu'intervient `contentMeta()`. Nous utilisons ContentMeta dans notre plugin [Shortcode](https://github.com/getgrav/grav-plugin-shortcode-core) pour [récupérer des sections d'autres pages](https://github.com/getgrav/grav-plugin-shortcode-core#sections-from-other-pages) à l'aide de shortcodes. Par example:

```html
1 | <div id="author">{{ page.find('/my/custompage').contentMeta.
  | shortcodeMeta.shortcode.section.author|e }}</div>
```

Nous l'avons utilisé dans Shortcode Core pour stocker les actifs CSS et JS requis par le shortcode sur la page, mais cette fonctionnalité peut être utilisée pour stocker à peu près n'importe quelle structure de données dont vous avez besoin.

