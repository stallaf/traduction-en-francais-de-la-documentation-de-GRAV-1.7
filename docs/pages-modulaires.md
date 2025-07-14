<h1 class="rem">Pages modulaires</h1>

Le concept des **pages Modulaires** est un peu difficile à comprendre au début, mais quand vous le saurez, vous verrez à quel point elles sont pratiques à utiliser. Une **Page Modulaire** est une collection de pages empilées les unes sur les autres pour créer une page unique unifiée. Cela vous permet de créer une structure de page complexe en utilisant l'approche des **briques de construction LEGO**, et qui n'aime pas LEGO ? !

<h2 id="Que sont les pages modulaires et qu'est-ce qu'elles ne sont pas ?">Que sont les pages modulaires et qu'est-ce qu'elles ne sont pas ?
<a href="#Que sont les pages modulaires et qu'est-ce qu'elles ne sont pas ?" class="toc-anchor after"></a></h2>

Dans Grav, les [Pages](./pages.md) sont un concept large qui capture presque tous les types de combinaisons d'éléments que vous pouvez imaginer intégrer dans un site Web. Il est important de noter que les pages modulaires sont un sous-ensemble de ce concept, mais pas la même chose qu'une page ordinaire. Une page normale est assez autonome, dans le sens où Grav la rendra et l'affichera sans dépendre d'autres contenus tels que d'autres pages ou pages enfants. Une page modulaire, cependant, n'a pas de pages enfants. Ceci est illustré en imaginant une structure de page simple :

Une page régulière trouvée sur *domain.com/books* contient des détails sur les livres à vendre. Plusieurs pages enfants existent pour cette page, telles que *domain.com/books/gullivers-travels* et *domain.com/books/the-hobbit*. Leurs dossiers portent le même nom que l'adresse affichée par Grav : `/pages/books`, `/pages/books/gullivers-travels` et `/pages/books/the-hobbit`. Cette structure ne fonctionnerait pas dans une page modulaire.

Une page modulaire n'a pas de pages enfants dans le même sens, mais plutôt des **modules** qui constituent les parties de la page. Ainsi, plutôt que divers livres situés sous la page de niveau supérieur, la page modulaire affiche ses modules sur **la même page**. Les Voyages de Gulliver et Le Hobbit apparaissent tous les deux dans *domain.com/books*, avec les chemins `/pages/books/_gullivers-travels` et `/pages/books/_the-hobbit`. Ainsi, les Pages Modulaires ne sont pas directement compatibles avec les Pages régulières et ont leur propre structure.

<h2 id="Exemple de structure de dossier">Exemple de structure de dossier
<a href="#Exemple de structure de dossier" class="toc-anchor after"></a></h2>

En utilisant notre **squelette One-Page** comme exemple, nous expliquerons plus en détail le fonctionnement des pages modulaires.

La **Page Modulaire** elle-même est assemblée à partir de pages qui existent dans des sous-dossiers trouvés sous le dossier principal de la page. Dans le cas de notre squelette One-Page, cette page se trouve dans le dossier `01.home`. Dans ce dossier se trouve un seul fichier `modular.md` qui indique à Grav quelles sous-pages extraire pour assembler la page modulaire et dans quel ordre les afficher. Le nom de ce fichier est important car il indique à Grav d'utiliser le `modular.html. twig`-template du thème actuel pour rendre la page.

Ces sous-pages se trouvent dans des dossiers dont les noms commencent par un trait de soulignement (`_`). En utilisant un trait de soulignement, vous dites à Grav qu'il s'agit de **Modules** et non de pages autonomes. Par exemple, les dossiers de sous-page peuvent être nommés `_features` ou `_showcase`. Ces pages ne sont **pas routables** - elles ne peuvent pas être pointées directement dans un navigateur, et elles ne sont **pas visibles** - elles n'apparaissent pas dans un menu.

Dans le cas de notre squelette One-Page, nous avons créé la structure de dossiers illustrée ci-dessous.

![Page de liste](https://learn.getgrav.org/user/pages/02.content/09.modular/modular-explainer-2.jpg)

Chaque sous-dossier contient un fichier Markdown qui agit comme une page.

Les données contenues dans ces dossiers de Modules - y compris les fichiers Markdown, les images, etc. - sont ensuite extraites et affichées sur la page modulaire. Ceci est accompli en créant une page principale, en définissant une [Collection de Pages](./pages-collection.md) dans le FrontMatter YAML de la page principale, puis en itérant sur cette collection dans un modèle Twig pour générer la page HTML combinée. Un thème doit déjà avoir un modèle `modular.html.twig` qui fera cela et est utilisé lorsque vous créez un type de page modulaire. Voici un exemple simple d'un `modular.html.twig` :

```twig
{% for module in page.collection() %}
   {{ module.content|raw }}
{% endfor %}
```

Voici un exemple de la page modulaire résultante, mettant en évidence les différents dossiers modulaires utilisés.

![Page de liste](https://learn.getgrav.org/user/pages/02.content/09.modular/modular-explainer-1.jpg)

<h2 id="Configuration  de la page principale">Configuration  de la page principale
<a href="#Configuration  de la page principale" class="toc-anchor after"></a></h2>

Comme vous pouvez le voir, chaque section extrait le contenu d'un dossier de module différent. Déterminer quels dossiers de modules sont utilisés et dans quel ordre se produit dans le fichier Markdown principal du dossier parent du module. Voici le contenu du fichier `modular.md` dans le `dossier 01.home`.

```yaml
---
title: One Page Demo Site
menu: Home
onpage_menu: true
body_classes: "modular header-image fullwidth"

content:
   items: '@self.modular'
   order:
      by: default
      dir: asc
      custom:
         - _showcase
         - _highlights
         - _callout
         - _features
---
```

Comme vous pouvez le voir, il n'y a pas de contenu réel dans ce fichier. Tout est géré dans le YAML FrontMatter dans l'en-tête. Le **titre** de la page, l'affectation du **menu*** et d'autres paramètres que vous trouveriez dans une page typique se trouvent ici. Le [contenu](./en-tete-frontmatter.md) demande à Grav de créer le contenu basé sur une collection de pages modulaires, et fournit même un ordre manuel personnalisé pour leur rendu.

<h2 id="Modules">Modules
<a href="#Modules" class="toc-anchor after"></a></h2>

![Page de liste](https://learn.getgrav.org/user/pages/02.content/09.modular/modular-explainer-3.jpg)

Le fichier Markdown de chaque Module peut avoir son propre modèle, ses propres paramètres, etc. À toutes fins utiles, il possède la plupart des fonctionnalités et des paramètres d'une page normale, il n'est tout simplement pas rendu comme tel. Nous recommandons que les paramètres à l'échelle de la page, tels que la **taxonomie**, soient placés dans le fichier Markdown principal qui contrôle l'ensemble de la page.

Les pages modulaires elles-mêmes sont gérées comme des pages normales. Voici un exemple utilisant le fichier `text.md` dans la page `_callout` qui apparaît au milieu de la page modulaire.

```yaml
---
title: Homepage Callout
image_align: right
---

## Content Unchained

Vous n'êtes plus <em>_esclave de votre CMS_</em>. Grav ** vous permet ** de créer n'importe 
quoi à partir d'un <code>[simple site d'une page](#)</code>, d'un [`beau blog](#)`, d'un <code>
[site produit](#)</code> puissant et riche en fonctionnalités, ou vous pouvez réver d'à peu 
près n'importe quoi !
```

Comme vous pouvez le voir, l'en-tête de la page contient des informations de base que vous pourriez trouver sur une page normale. Il a son propre titre qui peut être référencé et des [options de page personnalisées](./en-tete-frontmatter.md#En-têtes de page personnalisés), telles que l'alignement de l'image, peuvent être définies ici, comme sur n'importe quelle autre page.

Le fichier de modèle pour le fichier `text.md` doit être situé dans le dossier `/templates/modular`- de votre thème et doit être nommé `text.html.twig`. Ce fichier, comme tout fichier de modèle Twig pour toute autre page, définit les paramètres, ainsi que les différences de style entre celui-ci et la page de base.

```twig
<div class="modular-row callout">
   {% set image = page.media.images|first %}
   {% if image %}
      {{ image.cropResize(400,400).html('','','
      align-'~page.header.image_align)|raw }}
   {% endif %}
{{ content|raw }}
</div>
```

Généralement, les Pages Modulaires sont très simples. Vous devez juste vous habituer à l'idée que chaque section de votre page est définie dans un module qui a son propre dossier sous la page réelle. Ils sont affichés tous en même temps à vos visiteurs, mais organisés légèrement différemment des pages ordinaires. N'hésitez pas à expérimenter et à découvrir tout ce que vous pouvez accomplir avec une page modulaire dans Grav.

