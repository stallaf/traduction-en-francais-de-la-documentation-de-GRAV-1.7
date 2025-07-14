<h1 class = "rem">En-têtes / Frontmatter</h1>

Les en-têtes de page (également connus sous le nom de frontmatter) en haut d'une page sont complètement facultatifs, vous n'en avez pas du tout besoin pour qu'une page s'affiche dans Grav. Il existe 3 principaux types de pages (**Standard**, **Listing** et **Modular**) dans Grav, et chacun a des en-têtes pertinents.

<div class = "notice note">
Les en-têtes sont également connus sous le nom de <strong>Page Frontmatter</strong> et sont communément appelés ainsi afin de ne pas être confondus avec les en-têtes HTTP.
</div>

<h2 id="En-têtes de page de base">En-têtes de page de base
<a href="#En-têtes de page de base" class="toc-anchor after"></a></h2>

Il existe un certain nombre d'options d'en-tête de base disponibles.

<h3 id="Activer le cache">Activer le cache
<a href="#Activer le cachee" class="toc-anchor after"></a></h3>

    cache_enable : false

Par défaut, Grav mettra en cache le contenu du fichier de page pour s'assurer que tout fonctionne aussi vite que possible. Il existe des scénarios avancés dans lesquels vous ne souhaitez pas que la page soit mise en cache.

Un exemple de ceci est lorsque vous utilisez des variables Twig dynamiques dans votre contenu. La variable `cache_enable` permet de remplacer ce comportement. Nous aborderons les variables de contenu Twig dans un chapitre ultérieur. Les valeurs valides sont `true` ou `false`.

<h3 id="Date">Date
<a href="#Date" class="toc-anchor after"></a></h3>

    date: 01/01/2020 15h14

La variable `date` permet de définir spécifiquement une date associée à cette page. Ceci est souvent utilisé pour indiquer quand un message a été créé et peut être utilisé à des fins d'affichage ou de tri. S'il n'est pas défini, la valeur par défaut est **l'heure de la dernière modification** de la page.

<div class = "notice note">
Les dates aux formats <code>m/d/y</code> ou <code>d-m-y</code> sont désambiguïsées en regardant le séparateur entre les différents composants : si le séparateur est une barre oblique (<code>/</code>), alors le<code> m/d/y</code> <strong>américain</strong> est supposé ; tandis que si le séparateur est un tiret (<code>-</code>) ou un point (<code>.</code>), le format <code>d.m.y</code> <strong>européen</strong> est utilisé.
</div>

<h3 id="Menu">Menu
<a href="#Menu" class="toc-anchor after"></a></h3>

    menu : Ma page

La variable `menu` vous permet de définir le texte à utiliser dans la navigation. Il existe plusieurs couches de solutions de repli pour le menu, donc si aucune variable `menu` n'est définie, Grav essaiera d'utiliser la variable `title`.

<h3 id="Published">Published
<a href="#Published" class="toc-anchor after"></a></h3>

    published : true

Par défaut, une page est **publiée** à moins que vous ne définissiez explicitement `published : false` ou via un `publish_date` défini ultérieurement, ou `unpublish_date` précédemment. Les valeurs valides sont `true` ou `false`.

<h3 id="Slug">Slug
<a href="#Slug" class="toc-anchor after"></a></h3>

    slug : ma-page-slug

La variable `slug` vous permet de définir spécifiquement la partie de l'URL de la page. Par exemple : `http://yoursite.com/my-page-slug` serait l'URL si vous définissez le `slug` ci-dessus. Si le `slug` n'est pas défini dans la page, Grav revient à utiliser le nom du dossier (sans aucun préfixe numérique).

Les [Slugs](https://en.wikipedia.org/wiki/Semantic_URL#Slug) sont généralement entièrement en minuscules, avec des caractères accentués remplacés par des lettres de l'alphabet anglais et des caractères d'espacement remplacés par un tiret ou un trait de soulignement. Alors que les futures versions de Grav prendront en charge les espaces dans les slugs, il n'est pas recommandé d'avoir des espaces vides ou des lettres majuscules.

Par exemple : si le titre d'un article de blog est `Blog Post Example`, le slug recommandé serait `blog-post-example`.

<h3 id="Taxonomy">Taxonomy
<a href="#Taxonomy" class="toc-anchor after"></a></h3>

    taxonomy :
        category : blog
        tag : [sample, démo, grav]

Variable d'en-tête très utile, la `taxinomy` vous permet d'attribuer des valeurs à la **taxonomie** que vous avez définie comme types valides dans le fichier de [configuration du site](/configuration#Configuration du site).

Si la taxonomie n'est pas définie dans ce fichier, elle sera ignorée. Dans cet exemple, la page est définie comme appartenant à la catégorie `blog` et contient les balises : `sample`, `demo` et` grav`. Ces taxonomies peuvent être utilisées pour trouver ces pages à partir d'autres pages, plugins et même thèmes. Le chapitre [Taxonomie](./taxonomy.md) couvrira ce concept plus en détail.

<h3 id="Title">Title
<a href="#Title" class="toc-anchor after"></a></h3>

Si vous n'avez aucun en-tête, vous n'aurez aucun contrôle sur le titre de la page tel qu'il apparaît dans le navigateur et les moteurs de recherche. Pour cette raison, il est recommandé de mettre *au moins* la variable `title` dans l'entête de la page :

    title : Titre de ma page

Si la variable `title` n'est pas définie, Grav a une solution de repli et essaiera d'utiliser la variable `slug` en majuscule.

<h2 id="En-têtes avancés">En-têtes avancés
<a href="#En-têtes avancés" class="toc-anchor after"></a></h2>

Ceux-ci sont toujours importants mais moins couramment utilisés. Ils peuvent être utilisés pour fournir des fonctionnalités avancées au sein de votre page.

<h3 id="Ajouter l'extension d'URL">Ajouter l'extension d'URL
<a href="#Ajouter l'extension d'URL" class="toc-anchor after"></a></h3>

    append_url_extension : '.json'

Permet à la page de remplacer l'extension par défaut et d'en définir une par programme. Il définira également les attributs d'en-tête appropriés pour la réponse.

<h3 id="Cache-Control">Cache-Control
<a href="#Cache-Control" class="toc-anchor after"></a></h3>

    cache_control : max-age=604800

Peut être vide pour aucun paramètre ou une valeur de texte de `cache-control` [valide](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control).

<div class = "notice note">
Assurez-vous que vous utilisez <code>no-cache</code> si la page contient des informations susceptibles de changer en fonction de l'utilisateur. Sinon, le contenu peut fuir vers d'autres utilisateurs. Expire le paramètre s'il est défini sur <code>expire : 0</code> a le même effet.
</div>

<h3 id="Date Format">Date Format
<a href="#Date Format" class="toc-anchor after"></a></h3>

    dateformat : 'Y-m-d H:i:s'

Remplace la configuration par défaut de Grav pour les formats de date et permet de la définir au niveau de la page. Vous pouvez utiliser n'importe lequel des [formats de date PHP](https://www.php.net/manual/en/function.date.php) disponibles.

<h3 id="Debugger">Debugger
<a href="#Debugger" class="toc-anchor after"></a></h3>

Lorsque vous activez le débogueur via le fichier de configuration `system.yaml`, le débogueur s'affichera sur chaque page. Il y a des cas où cela peut ne pas être souhaitable ou peut provoquer des conflits avec la sortie. Cela arrive par exemple lorsque vous demandez une page destinée à renvoyer du HTML rendu à un appel Ajax. Cela ne devrait pas injecter le débogueur dans les données résultantes. Pour désactiver le débogueur sur cette page, vous pouvez utiliser l'en-tête de la page `debugger` :

    debugger : false

<h3 id="ETag">ETag
<a href="#ETag" class="toc-anchor after"></a></h3>

    etag : true

Activez ou désactivez au niveau de la page l'affichage ou non d'une variable d'en-tête ETag avec une valeur unique. `false` par défaut sauf si remplacé dans votre `system.yaml`.

<h3 id="Expires">Expires
<a href="#Expires" class="toc-anchor after"></a></h3>

    expires: 604800

La page expire dans une durée fixée en secondes (604800 secondes = 7 jours).

<div class = "notice note">
Assurez-vous que vous utilisez <code>expire : 0</code> si la page contient des informations qui peuvent changer en fonction de l'utilisateur. Sinon, le contenu peut fuir vers d'autres utilisateurs. Voir également le paramètre<a href="/en-tete-frontmatter">Cache-Control</a>.
</div>

<h3 id="External Url">External Url
<a href="#External Url" class="toc-anchor after"></a></h3>

    external_url : https://www.monsite.com/foo/bar

Vous permet de remplacer l'URL générée dynamiquement par celle que vous fournissez explicitement.

<h3 id="Code de réponse HTTP">Code de réponse HTTP
<a href="#Code de réponse HTTP" class="toc-anchor after"></a></h3>

    http_response_code : 404

Permet la configuration dynamique d'un code de réponse HTTP.

<h3 id="Language">Language
<a href="#Language" class="toc-anchor after"></a></h3>

    language: fr

Cela vous permet de remplacer la langue d'une page particulière.

<h3 id="LastModified">LastModified
<a href="#LastModified" class="toc-anchor after"></a></h3>

    last_modified : true

Activez ou désactivez au niveau de la page l'affichage ou non d'une variable d'en-tête Dernière modification avec la date de modification. `false` par défaut sauf si remplacé dans votre `system.yaml`.

<h3 id="Lightbox">Lightbox
<a href="#Lightbox" class="toc-anchor after"></a></h3>

    lightbox : true

Bien qu'il ne s'agisse pas à proprement parler d'un en-tête de page standard, il s'agit d'un moyen courant d'activer le chargement d'un JavaScript et d'un CSS lightbox standard pour une page. Par défaut, le thème principal `antimatter` ne charge pas les prérequis pour activer les fonctionnalités lightbox des images, assurez-vous d'installer un plugin lightbox tel que **Featherlight**, qui est disponible via GPM.

<h3 id="Login Redirect Here">Login Redirect Here
<a href="#Login Redirect Here" class="toc-anchor after"></a></h3>

    login_redirect_here : false

L'en-tête `login_redirect_here` vous permet de déterminer si quelqu'un est conservé ou non sur cette page après s'être connecté via le [plugin de connexion Grav](https://github.com/getgrav/grav-plugin-login). Définir cet en-tête sur `false` redirigera quelqu'un vers la page précédente après une connexion réussie.

Le réglage à `true` ici permettra à la personne de rester sur la page actuelle après une connexion réussie. C'est également le paramètre par défaut, qui s'applique s'il n'y a pas d'en-tête `login_redirect_here` dans le frontmatter.

Vous pouvez remplacer ce comportement par défaut en forçant un emplacement standard en spécifiant une option explicite dans votre configuration de connexion YAML :

    redirect_after_login : '/profile'

Cela vous mènera toujours à la route `/profile` après une connexion réussie.

<h3 id="Markdown">Markdown
<a href="#Markdown" class="toc-anchor after"></a></h3>

    markdown:
      extra: false
      auto_line_breaks: false
      auto_url_links: false
      escape_markup: false
      special_chars:
        '>': 'gt'
        '<': 'lt'

| Propriété                | Description
| :-------------------     | :-----------------
| **extra** :              | Activer la prise en charge de Markdown Extra (GFM par défaut).
| **auto_line_breaks** :   | Active les sauts de ligne automatiques.
| **auto_url_links** :     | Activer les liens HTML automatiques.
| **escape_markup** :      | Échappez les balises de balisage dans les entités.
| **special_chars** :      | Liste des caractères spéciaux à convertir automatiquement.

Vous pouvez les activer globalement via votre fichier de configuration `user/config/system.yaml`, ou vous pouvez remplacer ce paramètre global par page avec cette option d'en-tête `markdown`.

<h3 id="Never Cache Twig">Never Cache Twig
<a href="#Never Cache Twig" class="toc-anchor after"></a></h3>

    never_cache_twig : true

L'activer vous permettra d'ajouter une logique de traitement qui peut changer dynamiquement à chaque chargement de page, plutôt que de mettre en cache les résultats et de les stocker pour chaque chargement de page. Cela peut être activé/désactivé à l'échelle du site dans le fichier **system.yaml**, ou sur une page spécifique. Peut être défini sur `true` ou `false`.

C'est un changement subtil, mais qui est particulièrement utile dans les pages modulaires car il vous évite d'avoir à désactiver constamment la mise en cache lorsque vous travaillez avec. La page est toujours en cache, mais pas le Twig. Le Twig est traité après la récupération du contenu mis en cache. Pour les formulaires modulaires, cela fonctionne désormais uniquement avec ce paramètre plutôt que d'avoir à désactiver le cache de page modulaire.

<div class = "notice info">
Ceci n'est pas compatible avec <code>twig_first: true</code> actuellement car tout le traitement se produit dans le seul appel Twig.
</div>

<h3 id="Process">Process
<a href="#Process" class="toc-anchor after"></a></h3>

    process:
        markdown: false
        twig: true

Le traitement de la page est une autre fonctionnalité avancée. Par défaut, Grav traitera le `markdown` mais ne traitera pas `twig` dans une page. Ce choix de ne pas traiter Twig par défaut existe purement pour des raisons de performances car ce n'est pas une fonctionnalité couramment nécessaire. La variable `process` vous permet de remplacer ce comportement.

Vous pouvez désactiver `markdown` sur une page particulière si vous souhaitez utiliser 100 % HTML dans votre page et ne pas exécuter du tout le processus de démarquage. Cela permet également à un plugin de traiter complètement le contenu d'une autre manière. Les valeurs valides sont `true` ou `false`.

Il existe des situations où vous souhaitez utiliser la fonctionnalité de création de modèles Twig dans votre contenu, et cela est accompli en définissant la variable `twig` sur `true`.

<h3 id="Process Twig First">Process Twig First
<a href="#Process Twig First" class="toc-anchor after"></a></h3>

    twig_first: false

Si défini sur `true`, le traitement Twig se produira avant tout traitement Markdown. Cela peut être particulièrement utile si votre Twig génère des démarques qui doivent être disponibles pour être traitées par le compilateur Markdown. Une chose à noter, si vous avez `cache_enable : false` **et** `twig_first : true` la vraie mise en cache de la page est effectivement désactivée.

<h3 id="Publish Date">Publish Date
<a href="#Publish Date" class="toc-anchor after"></a></h3>

    publish_date: 01/23/2020 13:00

Champ facultatif, mais peut fournir une date pour déclencher automatiquement la publication. Les valeurs valides sont toutes les valeurs de date de chaîne prises en charge par [strtotime()](https://php.net/manual/en/function.strtotime.php).

<h3 id="Redirection">Redirection
<a href="#Redirection" class="toc-anchor after"></a></h3>

    redirect: '/some/custom/route'

ou alors

    redirect: 'http://someexternalsite.com'

Vous pouvez rediriger vers une autre page interne ou externe directement à partir d'un en-tête de page. Bien sûr, cela signifie que cette page ne sera pas affichée, mais la page peut toujours être dans une collection, un menu, etc. car elle existera en tant que page dans Grav.

Vous pouvez également ajouter un code de redirection à une URL en utilisant des crochets :

    redirect: '/some/custom/route[303]'
  
<h3 id="Routes">Routes
<a href="#Routes" class="toc-anchor after"></a></h3>  

    routes:
      default: '/my/example/page'
      canonical: '/canonical/url/alias'
      aliases:
        - '/some/other/route'
        - '/can-be-any-valid-slug'

Vous pouvez désormais fournir une **route par défaut** qui remplace la structure de route standard telle que définie par la structure de dossiers.

Vous pouvez également spécifier une **route canonique** spécifique qui peut être utilisée dans les thèmes pour générer un lien canonique :

``` html
<link rel="canonical" href="https://votresite/les-robes/les-robes-vertes-sont-impressionnantes" />
```

Enfin, vous pouvez spécifier un tableau **d'alias de route** qui peuvent être utilisés comme routes alternatives pour une page particulière.

<h3 id="Routable">Routable
<a href="#Routable" class="toc-anchor after"></a></h3>

    routable : faux

Par défaut, toutes les pages sont **routables**. Cela signifie qu'elles peuvent être atteintes en pointant votre navigateur vers l'URL de la page. Cependant, vous devrez peut-être créer une page pour contenir un contenu spécifique, mais elle est destinée à être appelée directement par un plugin, un autre contenu ou même un thème directement. Un bon exemple de cela est une page d'`erreur 404`.

Grav recherche automatiquement une page avec la route `/error` si une autre page est introuvable. Comme il s'agit d'une page réelle dans Grav, vous auriez un contrôle total sur l'apparence de cette page. Cependant, vous ne voulez probablement pas que les gens accèdent à cette page directement dans leur navigateur, donc cette page a généralement sa variable `routable` définie sur false. Les valeurs valides sont `true` ou `false`.

<h3 id="SSL">SSL
<a href="#SSL" class="toc-anchor after"></a></h3>

    ssl: true

Vous pouvez désormais autoriser une page spécifique à être forcée avec SSL **activé** ou **désactivé**. Cela ne fonctionne qu'avec l'option `absolute_urls: true` définie dans la configuration `system.yaml`. En effet, pour pouvoir basculer entre les pages SSL et non SSL, vous devez utiliser des URL complètes avec le protocole et l'hôte inclus.

<h3 id="Summary">Summary
<a href="#Summary" class="toc-anchor after"></a></h3>

    summary:
      enabled: true
      format: short | long
      size: int

L'option **summary** configure ce que la méthode `page.summary()` renvoie. Ceci est le plus souvent utilisé dans un scénario de type liste de blogs, mais peut être utilisé chaque fois que vous avez besoin d'un synopsis ou d'un résumé du contenu de la page. Les scénarios sont les suivants :

| Propriété                | Description
| :-----------------       | :-----------------
| **enabled** :            | Désactiver le résumé de la page (le résumé renvoie le même que le contenu de la page).
| **format** :             | <ul><li><code>long</code> = Tout délimiteur récapitulatif du contenu sera ignoré.</li><li><code>short</code> = Détecter et tronquer le contenu jusqu'à la position du délimiteur récapitulatif.</li></ul>

L'attribut `size` a des significations différentes lorsque le format est défini sur `short` et `long` :
 
| Taille Short                | Description
| :----------------           | :-----------------
| **size: 0**                 | Si aucun délimiteur de résumé n'est trouvé, le résumé est égal au contenu de la page,<br> sinon le contenu sera tronqué jusqu'à la position du délimiteur de résumé.
| **size:** <code>int</code>  | Tronque toujours le contenu après les caractères **int**. Si un délimiteur récapitulatif<br> a été trouvé, tronquer le contenu jusqu'à la position du délimiteur récapitulatif.

| Taille Long                 | Description
| :----------------           | :-----------------
| **size : 0**                | Le résumé équivaut à tout le contenu de la page.
| **size:** <code>int</code>  | Le contenu sera tronqué après **int** chars, indépendamment de la position du délimiteur récapitulatif

<h3 id="Template">Template
<a href="#Template" class="toc-anchor after"></a></h3>

    template: custom

Comme expliqué dans le [chapitre précédent](./pages.md), le modèle du thème utilisé pour afficher une page est basé sur le nom de fichier du fichier `.md`.

Ainsi, un fichier appelé `default.md` utilisera le modèle `default` dans le thème actif. Vous pouvez, bien sûr, remplacer ce comportement en définissant simplement la variable `template` dans l'en-tête et en choisissant un modèle différent.

Dans l'exemple ci-dessus, la page utilisera le modèle `custom` du thème. Cette variable existe car vous devrez peut-être modifier le modèle d'une page par programmation à partir d'un plugin.

<h3 id="Template Format">Template Format
<a href="#Template Format" class="toc-anchor after"></a></h3>

    template_format: xml

Traditionnellement, si vous voulez qu'une page affiche un format spécifique (c'est-à-dire : xml, json, etc.), vous deviez ajouter le format à l'url. Par exemple, la saisie de `http://example.com/sitemap.xml` indiquerait au navigateur d'afficher le contenu à l'aide du modèle `xml` twig se terminant par `.xml.twig`. C'est bien beau, car on aime faire les choses simplement en Grav.

En utilisant l'en-tête de page `template_format`, nous pouvons dire au navigateur comment rendre la page sans avoir besoin d'extensions dans l'URL. En saisissant `template_format: xml` dans notre page de plan de site, nous pouvons faire en sorte que `http://example.com/sitemap` fonctionne pour nous sans avoir à ajouter `.xml` à la fin.

Nous avons [utilisé cette méthode](https://github.com/getgrav/grav-plugin-sitemap/commit/00c23738bdbfe9683627bf0f99bda12eab9505d5#diff-190081f40350c0272970d9171f3437a2) avec le [plugin Grav Sitemap](https://github.com/getgrav/grav-plugin-sitemap).

<h3 id="Unpublish Date">Unpublish Date
<a href="#Unpublish Date" class="toc-anchor after"></a></h3>

    unpublish_date: 05/17/2020 00:32

Champ facultatif, mais peut fournir une date pour déclencher automatiquement la dépublication. Les valeurs valides sont toutes les valeurs de date de chaîne prises en charge par [strtotime()](https://php.net/manual/en/function.strtotime.php).

<h3 id="Visible">Visible
<a href="#Visible" class="toc-anchor after"></a></h3>

    visible: false

Par défaut, une page est **visible** dans la **navigation** si le dossier qui l'entoure a un préfixe numérique, c'est-à-dire que `/01.home` est visible, tandis que `/error` n'est pas **visible**. Ce comportement peut être remplacé en définissant la variable `visible` dans l'en-tête. Les valeurs valides sont `true` ou `false`.

<h3 id="#En-têtes de page personnalisés">En-têtes de page personnalisés
<a href="##En-têtes de page personnalisés" class="toc-anchor after"></a></h3>

Bien sûr, vous pouvez créer vos propres en-têtes de page personnalisés en utilisant n'importe quelle syntaxe YAML valide. Ceux-ci seraient spécifiques à la page et seraient disponibles pour n'importe quel plugin ou thème à utiliser. Un bon exemple serait de définir une variable spécifique à un plugin de sitemap, telle que :

    1 | sitemap:
    2 |     changefreq: monthly
    3 |     priority: 1.03

L'importance de ces en-têtes est que Grav ne les utilise pas par défaut. Ils ne sont lus que par le **plugin sitemap** pour déterminer à quelle fréquence cette page particulière est modifiée et quelle devrait être sa priorité.

Tout en-tête de page comme celui-ci doit être documenté, et généralement, il y aura une valeur par défaut qui sera utilisée si la page ne la fournit pas.

Un autre exemple serait de stocker des données spécifiques à la page qui pourraient ensuite être utilisées par Twig dans le contenu de la page.

Par exemple, vous avez peut-être souhaité associer une référence d'auteur à la page. Si vous avez ajouté ces paramètres YAML à l'en-tête de la page :

    1 | author:
      |     name: Sandy Johnson
      |     twitter: @sandyjohnson
      |     bio: Sandy is a freelance journalist and author of several publications on open source CMS platforms.

Vous pourrez ensuite y accéder depuis Twig :

```html
1 | <section id="détails-auteur">
2 |     <h2>{{ page.header.author.name|e }}</h2>
3 |     <p>{{ page.header.author.bio|e }}</p>
4 |     <span>Contact : <a href="https://twitter.com/{{ page.header.author.twitter|e }}"><i class="fa fa-twitter"></i></ a></span>
5 | </section>
```

Si le nom de la variable [contient un caractère spécial comme un tiret](https://github.com/getgrav/grav/issues/1957#issuecomment-723236844), vous devez utiliser la [fonction d'attribut twigs](https://twig.symfony.com/doc/2.x/functions/attribute.html) :

    attribute(page.header, 'plugin-name').active

<h3 id="En-têtes de méta-page">En-têtes de méta-page
<a href="#En-têtes de méta-page" class="toc-anchor after"></a></h3>

Les méta-en-têtes vous permettent de définir [l'ensemble standard de balises HTML](https://www.w3schools.com/tags/tag_meta.asp) pour chaque page ainsi que [OpenGraph](http://ogp.me/), [Facebook](https://developers.facebook.com/docs/sharing/best-practices) et [Twitter](https://dev.twitter.com/cards/overview).

<h4 id="Exemples de balises Meta standards">Exemples de balises Meta standards
<a href="#Exemples de balises Meta standards" class="toc-anchor after"></a></h4>

    1 | metadata:
    2 |    refresh: 30
    3 |    generator: 'Grav'
    4 |    description: 'Your page description goes here'
    5 |    keywords: 'HTML, CSS, XML, JavaScript'
    6 |    author: 'John Smith'
    7 |    robots: 'noindex, nofollow'
    8 |    my_key: 'my_value'

Cela produira le HTML :

```html
1 | <meta name="generator" content="Grav" />
2 | <meta name="description" content="Votre description de page va ici" />
3 | <meta http-equiv="refresh" content="30" />
4 | <meta name="keywords" content="HTML, CSS, XML, JavaScript" />
5 | <meta name="auteur" content="John Smith" />
6 | <meta name="robots" content="noindex, nofollow" />
7 | <meta name="ma_clé" content="ma_valeur" />
```

Toutes les balises méta HTML5 sont prises en charge.

<h4 id="Exemples de métabalises OpenGraph">Exemples de métabalises OpenGraph
<a href="#Exemples de métabalises OpenGraph" class="toc-anchor after"></a></h4>

    1 | metadata:
    2 |     'og:title': The Rock
    3 |     'og:type': video.movie
    4 |     'og:url': http://www.imdb.com/title/tt0117500/
    5 |     'og:image': http://ia.media-imdb.com/images/rock.jpg

Cela produira le HTML :

```html
1 | <meta name="og:title" property="og:title" content="Le Rocher" />
2 | <meta name="og:type" property="og:type" content="video.movie" />
3 | <meta name="og:url" property="og:url" content="http://www.imdb.com/title/ tt0117500/" />
4 | <meta name="og:image" property="og:image" content="http://ia.media-imdb.com/images/rock.jpg" />
```
Pour un aperçu complet de toutes les balises méta OpenGraph pouvant être utilisées, veuillez consulter la [documentation officielle](http://ogp.me/).

<h4 id="Exemples de métabalise Facebook">Exemples de métabalise Facebook
<a href="#Exemples de métabalise Facebook" class="toc-anchor after"></a></h4>

    1 | metadata:
    2 |    'fb:app_id': your_facebook_app_id

Cela produira le HTML :

```html
1 | <meta name="fb:app_id" property="fb:app_id" content="your_facebook_app_id" />
```

Facebook utilise principalement les métabalises OpenGraph, mais il existe des balises spécifiques à Facebook et celles-ci sont prises en charge automatiquement par Grav.

<h4 id="Exemples de métabalise Twitter">Exemples de métabalise Twitter
<a href="#Exemples de métabalise Twitter" class="toc-anchor after"></a></h4>

    1 | metadata:
    2 |    'twitter:card' : summary
    3 |    'twitter:site' : @flickr
    4 |    'twitter:title' : Your Page Title
    5 |    'twitter:description' : Your page description can contain summary information
    6 |    'twitter:image' : https://farm6.staticflickr.com/5510/14338202952_93595258ff_z.jpg

Cela produira le HTML :

```html
1 | <meta name="twitter:card" property="twitter:card" content="summary" />
2 | <meta name="twitter:site" property="twitter:site" content="@flickr" />
3 | <meta name="twitter:title" property="twitter:title" content="Titre de votre page" />
4 | <meta name="twitter:description" property="twitter:description" content="La description de votre page peut contenir des informations récapitulatives" />
5 | <meta name="twitter:image" property="twitter:image" content="https://farm6.staticflickr.com/5510/14338202952_93595258ff_z.jpg" />
```

Pour un aperçu complet de toutes les balises méta Twitter pouvant être utilisées, veuillez consulter la [documentation officielle](https://dev.twitter.com/cards/overview).

Cela offre vraiment beaucoup de flexibilité et de puissance.

<h2 id="Frontmatter.yaml">Frontmatter.yaml
<a href="#Frontmatter.yaml" class="toc-anchor after"></a></h2>

Une fonctionnalité avancée qui peut s'avérer utile pour certains utilisateurs expérimentés est la possibilité d'utiliser des valeurs frontmatter communes via un fichier `frontmatter.yaml` situé dans le dossier de la page. Ceci est particulièrement utile lorsque vous travaillez avec des sites multilingues où vous souhaiterez peut-être partager une partie du frontmatter entre toutes les versions linguistiques d'une page donnée.

Pour en profiter, créez simplement un fichier `frontmatter.yaml` à côté du fichier `.md` de votre page et ajoutez toutes les valeurs frontmatter valides. Par exemple:

    1 | metadata:
    2 |     generator: 'Super Grav'
    3 |     description: Give your page a powerup with Grav!

Si un en-tête est défini à la fois dans frontmatter.yaml et dans la page frontmatter, les valeurs de la page sont utilisées, les valeurs frontmatter.yaml sont remplacées.

<div class="notice note">
L'utilisation de frontmatter.yaml est une fonctionnalité côté fichier et n'est pas prise en charge par le plug-in d'administration.
</div>

<div class="notice warning">
L'utilisation de frontmatter.yaml est une fonctionnalité côté fichier et <strong>n'est pas prise en charge</strong> par le plug-in d'administration.
</div>

