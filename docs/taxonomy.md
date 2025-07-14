<h1 class="rem">Taxonomie</h1>

Avec **Grav**, la possibilité de regrouper ou de baliser des pages est intégrée directement dans le système avec **Taxonomy**.

> **Taxonomie (général)**, la pratique et la science (étude) de la classification des choses
> ou des concepts, y compris les principes qui sous-tendent une telle classification.

> <cite>Wikipedia</cite>

L'utilisation de la taxonomie sur votre site comporte deux éléments clés :

1. Définissez une liste de types de taxonomie dans votre `site.yaml`.
2. Affectez vos pages aux types de `taxonomy` appropriés avec des valeurs.

<h2 id="Exemple de taxonomie">Exemple de taxonomie
<a href="#Exemple de taxonomie" class="toc-anchor after"></a></h2>

Ce concept est mieux expliqué avec un exemple. Disons que vous voulez créer un blog simple. Dans ce blog, vous créerez des articles que vous voudrez peut-être attribuer à certains tags pour fournir un affichage en **nuage de tags**. En outre, vous pouvez avoir plusieurs auteurs et vous pouvez attribuer chaque article à cet auteur.

Accomplir cela dans Grav est une procédure simple. Grav fournit un fichier `site.yaml` par défaut qui se trouve dans le dossier `system/config`. Par défaut, cette configuration définit deux types de taxonomie `category` et `tag` :

    taxonomies: [category,tag,author]

Comme le `tag` est déjà défini, il vous suffit d'ajouter des `authors`. Pour ce faire, créez simplement un nouveau fichier `site.yaml` dans votre dossier `user/config` et ajoutez la ligne suivante :

    taxonomies: [category,tag,author]

Cela remplacera les taxonomies connues de Grav afin que les pages puissent être attribuées à l'une de ces trois taxonomies.

L'étape suivante consiste à créer des pages qui utilisent ces types de taxonomie. Par exemple, vous pourriez avoir une page qui ressemble à ceci :

```yaml
---
titre : Message 1
taxonomy :
   tag: [animal, dog]
   author : ksmith
---

Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse
cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non
proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
```

...et une autre page qui ressemble à :

```yaml
---
titre : Message 2
taxonomy :
   tag: [animal, cat]
   author: jdoe
---

Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse
cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non
proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
```

Comme vous pouvez le voir dans la configuration YAML, chaque page attribue des **valeurs** aux **types de taxonomie** que nous avons définis dans notre configuration utilisateur `site.yaml`. Ces informations sont utilisées par Grav lors du traitement des pages et créent une **carte de taxonomie** interne qui peut être utilisée pour trouver des pages en fonction de la taxonomie que vous avez définie.

<div class="notice warning">
Vos pages ne doivent pas nécessairement utiliser toutes les taxonomies que vous définissez dans votre <code>site.yaml</code>, mais vous devez définir toutes les taxonomies que vous utilisez.
</div>

Dans votre thème, vous pouvez facilement afficher une liste de pages écrites par `ksmith` en utilisant `taxonomy.findTaxonomy()` pour les trouver et les parcourir :

```html
<h2>Messages de Kevin Smith</h2>
<ul>
{% pour le message dans taxonomy.findTaxonomy({'author':'ksmith'}) %}
   <li>{{ post.title|e }}</li>
{% endfor %}
</ul>
```

Vous pouvez également effectuer des recherches sophistiquées basées sur plusieurs taxonomies en utilisant des tableaux/hachages, par exemple :

    {% for post in taxonomy.findTaxonomy({'tag':['animal','cat'],'author':'jdoe'}) %}

Cela trouvera tous les messages avec un `tag` défini sur `animal` **et**  `cat` **et** `author` défini sur `jdoe`. Fondamentalement, cela trouvera spécifiquement le **Post 2**.

Si vous avez besoin d'une collection qui inclut un terme **ou** l'autre, ajoutez simplement le paramètre `'or'` après le tableau, exemple :

    {% for post in taxonomy.findTaxonomy({'tag':['dog','cat']},'or') %}

Cela trouvera tous les messages avec un `tag` défini sur `dog` **ou** `cat`.

<h2 id="Collections basées sur la taxonomie">Collections basées sur la taxonomie
<a href="#Collections basées sur la taxonomie" class="toc-anchor after"></a></h2>

Nous avons couvert cela dans un chapitre précédent, mais il est important de se rappeler que vous pouvez également utiliser des taxonomies dans les en-têtes de page pour filtrer une collection de pages associées à une page parent. Si vous avez besoin d'un rappel sur ce sujet, veuillez vous reporter au chapitre sur les en-têtes de collection de taxonomie.

<h2 id="Ajout de valeurs de taxonomie personnalisées par défaut et dans les options">Ajout de valeurs de taxonomie personnalisées par défaut et dans les options
<a href="#Ajout de valeurs de taxonomie personnalisées par défaut et dans les options" class="toc-anchor after"></a></h2>

Vous pouvez utiliser le format ci-dessous dans les blueprints pour remplacer les taxonomies par `Default` et/ou `Options`. Une remarque importante ici est que si vous utilisez cette méthode pour remplacer ces deux attributs, vous devez ajouter `validate: type: commalist`, sinon cela risque de ne pas fonctionner comme vous le souhaitez.

```yaml
taxonomies:
   fields:
      header.taxonomy:
         default:
            category: ['blog','page']
            tag: ['test']
         options:
            category: ['grav']
         validate:
            type: commalist
```

