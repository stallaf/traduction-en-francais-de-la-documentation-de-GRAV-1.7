<h1 class="rem">Créer un blog</h1>

<div class = "notice info">
Téléchargez et installez localement le squelette du site de blog à partir de <a href = "https://getgrav.org/downloads/skeletons">https://getgrav.org/downloads/skeletons</a>, ou au moins préparez le référentiel <a href = "https://github.com/getgrav/grav-skeleton-blog-site">https://github.com/getgrav/grav-skeleton-blog-site</a>à vérifier. Ceci est un exemple de site qui utilise le thème Antimatière. Avoir un site Grav opérationnel qui fonctionne déjà avec une structure de blog vous donnera sûrement un coup de main si vous êtes bloqué ou si vous ne comprenez pas quoi faire ensuite.
</div>

<h2 id="Vérifiez que votre thème fournit les modèles de page Blog et Article">Vérifiez que votre thème fournit les modèles de page Blog et Article.
<a href="#Vérifiez que votre thème fournit les modèles de page Blog et Article" class="toc-anchor after"></a></h2>

Commençons simplement : choisissez un thème qui fournit déjà un modèle de page de blog. Par exemple Antimatter, TwentyFifteen, Deliver, Lingonberry, Afterburner2 et bien d'autres. Comment vérifier si votre thème fournit déjà un modèle de page de blog ? Allez dans le dossier `/user/themes/[votretheme]/templates`, et vérifiez l'existence des fichiers `blog.html.twig` et `item.html.twig`.

Si vous avez déjà choisi un thème et que votre thème n'est pas fourni avec ces fichiers, copiez-les depuis Antimatter : [https://github.com/getgrav/grav-theme-antimatter/tree/develop/templates](https://github.com/getgrav/grav-theme-antimatter/tree/develop/templates)

Vous devrez peut-être modifier le balisage en fonction de votre thème. La meilleure option si vous débutez est d'utiliser un thème qui les accompagne déjà.

<h2 id="Créer la structure des pages du blog">Créer la structure des pages du blog.
<a href="#Créer la structure des pages du blog" class="toc-anchor after"></a></h2>

Il existe différentes manières de structurer les pages. La valeur par défaut et la plus simple consiste à avoir une page parent, de type Blog, et des pages enfants pour les articles de blog.

<h3 id="Avec le plugin d'administration">Avec le plugin d'administration.
<a href="#Avec le plugin d'administration" class="toc-anchor after"></a></h3>

Créez une page de type Blog. Cette page est la "page d'accueil" du blog, avec la liste des articles du blog.

Créez une ou plusieurs pages enfants de type `Item`. Ce sont les articles du blog.

<h3 id="Manuellement">Manuellement.
<a href="#Manuellement" class="toc-anchor after"></a></h3>

Allez dans votre dossier pages/, créez une page `01.blog` (modifiez le numéro pour refléter la structure de votre menu), ajoutez-y un fichier `blog.md`. Dans ce fichier, ajoutez ce contenu :

```yaml
1 | ---
2 | teneur:
3 |      éléments : '@self.children'
4 | ---
```

Cela indique à Grav de parcourir les sous-pages (les articles de blog).

Créez un sous-dossier pour chaque article que vous souhaitez ajouter, et ajoutez dans chaque dossier un fichier `item.md`, avec le contenu de l'article de blog.

<h2 id="URLs">URLs.
<a href="#URLs" class="toc-anchor after"></a></h2>

La structure expliquée ci-dessus créera des articles de blog avec `/blog/` dans l'URL. Ce n'est peut-être pas ce dont vous avez besoin. Par exemple : si un blog est tout ce que vous avez sur votre site et que la liste des articles de blog est la page d'accueil. Dans ces cas, vous voudriez simplement que votre domaine racine accède à ce contenu plutôt que de renvoyer les visiteurs vers un répertoire enfant.

Dans ce cas, dans system.yaml (Configuration système dans Admin), définissez l'option `home.hide_in_urls` (Masquer l'accueil dans les URL dans Admin) sur true.

<h2 id="Le fonctionnement interne">Le fonctionnement interne.
<a href="#Le fonctionnement interne" class="toc-anchor after"></a></h2>

Vous voudrez peut-être savoir comment cela fonctionne. Le modèle Blog, le contenu du fichier `blog.html.twig` fourni dans le dossier `templates/` du thème, itère simplement sur ses pages enfants.

Dans sa manière la plus simple :

```twig
1 | {% set collection = page.collection() %}
2 | 
3 | {% for child in collection %}
4 |         {% include 'partials/blog_item.html.twig' with {'blog':page, 'page':child, 'truncate':true} %}
5 | {% endfor %}
```

`page.collection()` sélectionne par défaut la propriété `content.items` de la page frontmatter YAML et renvoie un tableau contenant les éléments qui correspondent à cette définition.

Si la page contient :

```yaml
1 | ---
2 | content:
3 |     items: '@self.children'
4 | ---
```

alors `collection` sera le tableau des sous-pages de la page courante.

Dans ce cas, le thème inclut le partiel `partials/blog_item.html.twig`, responsable du rendu du billet de blog unique, et lui transmet l'objet enfant contenant le billet de blog réel à rendre.

Pour en savoir plus

* Collections : [https://learn.getgrav.org/content/collections](/pages-collection)
* Page de liste : [https://learn.getgrav.org/content/content-pages#listing-page](/pages/#Page de liste)
* Dossiers : [https://learn.getgrav.org/content/content-pages#folders](/pages/#Dossiers)
* Taxonomie : [https://learn.getgrav.org/content/taxonomy#taxonomy-example](/taxonomy/#Exemple de taxonomie)

