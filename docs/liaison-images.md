<h1 class="rem">Liaison d'images</h1>

Grav propose une variété d'options de liaison flexibles qui vous permettent de lier des images d'une page à une autre, et même à partir de sources distantes. Si vous avez déjà lié des fichiers à l'aide de HTML ou travaillé avec un système de fichiers à l'aide d'une ligne de commande, une grande partie de cela devrait être élémentaire à comprendre.

Nous allons passer à quelques exemples simples en utilisant ce modèle très basique et réduit de ce à quoi pourrait ressembler le répertoire **Pages** d'un site Grav.

![Liaison d'images](https://learn.getgrav.org/user/pages/02.content/06.image-linking/pages.png)

En utilisant cette structure de répertoires comme exemple, nous examinerons les différents types de liens que vous pouvez utiliser pour afficher des fichiers multimédias dans votre contenu. Nous avons des fichiers image dans chaque dossier, y compris une image pour chaque article de blog, et trois images dans un répertoire spécial `/images` qui agit comme une page mais ne contient que des fichiers multimédias.

L'utilisation du dossier `/images` sert d'exemple de la manière dont vous pouvez gérer un répertoire d'images simple et centralisé pour stocker les fichiers fréquemment utilisés par plusieurs pages. Cela simplifie le processus de liaison dans ces cas.

<div class="notice warning">
Si vous décidez d'utiliser un répertoire d'images centralisé, sachez que ce répertoire doit exister dans le dossier <code>pages</code> car ce dossier est destiné au contenu frontal.
</div>

Pour commencer, voici un bref aperçu de certains des composants standard d'une balise d'image basée sur Grav Markdown.

<div class="cadre">
<code>![Alt Text](../path/image.ext)</code>
</div>
<br/>

| **CHAINE**          | **DESCRIPTION**
| :---------          | :----------
| `!`                 | Lorsqu'il est placé au début d'une balise de lien Markdown traditionnelle,<br>  
il indique qu'il s'agit d'une balise d'image.
| `[]`                | Le crochet est utilisé pour envelopper le texte alternatif facultatif de l'image.
| `()`                | La parenthèse est utilisée pour entourer la référence à l'image elle-même. <br>
Ceci est placé directement après le crochet.
| `../`               | Lorsqu'il est utilisé dans le lien, il indique un déplacement vers le haut d'un <br>répertoire.

<div class="notice tip">
Vous pouvez combiner un lien de page normal et un lien d'image comme pour envelopper une image dans un lien : <code>[![Alt ​​text](/path/to/img.jpg)](http://example.net/)</code>.
</div>

<h2 id="Slug Relatif">Slug Relatif
<a href="#Slug Relatif" class="toc-anchor after"></a></h2>

Les liens d'image **relatifs** utilisent des destinations définies par rapport à la page actuelle. Cela peut être aussi simple que de créer un lien vers un autre fichier dans le répertoire en cours, tel qu'un fichier image associé à la page en cours, ou aussi complexe que de remonter plusieurs niveaux de répertoire, puis de revenir au dossier/fichier spécifique où une image peut résider.

Avec des liens relatifs, l'emplacement du fichier source est tout aussi important que celui de la destination. Si l'un ou l'autre des fichiers de l'ensemble est déplacé, en modifiant le chemin entre eux, le lien peut être rompu.

L'avantage de ce type de structure de liaison est que vous pouvez basculer rapidement entre un serveur de développement local et un serveur live avec un nom de domaine différent et tant que la structure de fichiers reste cohérente, les liens devraient fonctionner sans problème.

Un lien de fichier pointe vers un fichier particulier par son nom, plutôt que par son répertoire ou son slug. Si vous créez un lien d'image dans `pages/01.blog/test-post-1/item.md` vers `/pages/01.blog/test-post-3/test-image-3.jpg` vous utiliserez la commande suivante :

<div class="cadre">
<code>![Test Image 3](../test-post-3/test-image-3.jpg)</code>
</div>

Ce lien monte d'un dossier, comme indiqué par `../`, puis descend d'un dossier, pointant directement vers `test-image-3.jpg` comme destination.

Si nous voulons charger `blog-header.jpg` depuis le répertoire `.blog`, nous procéderions comme suit :

<div class="cadre">
<code>![Blog Header](../../blog/blog-header.jpg)</code>
</div>

<div class="notice note">
Vous n'avez pas besoin d'inclure les numéros de commande (<code>01</code>.) pour les liens relatifs slug.
</div>

Grav a intégré la prise en charge des slugs dans l'en-tête du fichier de démarquage principal de la page. Ce slug remplace le nom du dossier de la page et tous les fichiers multimédias qu'il contient.

Par exemple, **Test Post 2** a un slug défini via son fichier Markdown (`/pages/01.blog/test-post-2/item.md`). L'en-tête de ce fichier contient les éléments suivants :

```yaml
1 | ---
2 | title: Test Post 2
3 | slug: test-slug
4 | taxonomy:
5 |    category: blog
6 |---
```

Vous remarquerez que le slug `test-slug` a été défini. Les slugs définis de cette manière sont complètement facultatifs et n'ont pas besoin d'être présents. Comme mentionné dans le dernier chapitre, ils fournissent un moyen facile de créer des liens. Si un slug est défini, tout lien que vous créez vers un fichier multimédia dans ce dossier devra être **Slug Relatif** ou **Absolu** avec une URL complète définie pour le lien.

Si nous voulons lier `test-image-2.jpg` de **Test Post 2**, nous entrerons ce qui suit :

<div class="cadre">
<code>![Test Image 2](../test-slug/test-image-2.jpg)</code>
</div>

Vous remarquerez que nous avons navigué vers le haut d'un répertoire en utilisant (`../`) puis vers le bas dans le dossier de page `test-slug` en utilisant le slug qui a été défini dans le fichier`/pages/01.blog/test-post-2/item.md`.

<h2 id="Répertoire Relatif">Répertoire Relatif
<a href="#Répertoire Relatif" class="toc-anchor after"></a></h2>

Les liens d'image **relatifs au répertoire** utilisent des destinations définies par rapport à la page actuelle. La principale différence entre un lien relatif au slug et un lien relatif au répertoire est qu'au lieu d'utiliser les slugs d'URL, vous faites référence via le chemin complet avec les noms de dossier.

Un exemple de ceci serait quelque chose comme :

<div class="cadre">
<code>![Test Image 3](../../01.blog/02.my_folder/test-image-3.jpg)</code>
</div>

<div class="notice info">
Le principal avantage de cela est que vous pouvez maintenir l'intégrité des liens dans d'autres systèmes en dehors de Grav, tels que GitHub.
</div>

<h2 id="Absolu">Absolu
<a href="#Absolu" class="toc-anchor after"></a></h2>

Les liens absolus sont similaires aux liens relatifs, mais sont relatifs à la racine du site. Dans **Grav**, ceci est généralement basé dans votre répertoire **/user/pages/**. Ce type de lien peut se faire de deux manières différentes.

Vous pouvez le faire de la même manière que le style **Slug Relatif** qui utilise le slug ou le nom du répertoire dans le chemin pour plus de simplicité. Cette méthode supprime les problèmes potentiels de changements de commande ultérieurs (changement du numéro au début du nom du dossier) rompant le lien. Ce serait la méthode de liaison absolue la plus couramment utilisée.

Dans un lien absolu, le lien s'ouvre avec un `/`. Voici un exemple de lien absolu fait vers `pages/01.blog/test-post-2/test-image-2.jpg` dans le style **Slug** depuis `pages/01.blog/blog.md`.

<div class="cadre">
<code>![Test Image 2](/blog/test-slug/test-image-2.jpg)</code>
</div>

<div class="notice tip">
Une technique puissante consiste à créer un dossier <code>user/pages/images/</code> dans votre site Grav et à y placer vos images. Ensuite, vous pouvez facilement les référencer avec une URL absolue à partir de n'importe quelle page Grav : <code>/images/test-image-4.jpg</code> et être toujours en mesure d'effectuer des <a href="/media">actions multimédias</a> sur celles-ci.
</div>

<h2 id="PHP Streams">PHP Streams
<a href="#PHP Streams" class="toc-anchor after"></a></h2>

Grav a également la capacité de référencer et de lier des images via des flux PHP. Il existe plusieurs flux PHP intégrés disponibles qui sont utiles, notamment :

* `user://` - dossier utilisateur, par exemple: `user/`
* `page://` - dossier de pages, par exemple : `user/pages/`
* `image://` - dossier d'images, par exemple : `user/images/`
* `plugins://` - dossier des plugins, par exemple : `user/plugins/`
* `theme://` - thème actuel, par exemple : `user/thèmes/antimatter/`

Ceux-ci vous permettent d'accéder facilement aux images qui sont traditionnellement en dehors de la hiérarchie des pages (`user/pages/`).

<div class="cadre">
<code>![Stream Image](user://media/images/my-image.jpg)</code>
</div>

ou alors:

<div class="cadre">
<code>![Stream Image](theme://images/my-image.jpg)</code>
</div>

Pour obtenir la liste complète des emplacements de flux par défaut, consultez [Configuration multisite - Flux](./avance-config-multisite.md).

<h2 id="Remote">Remote
<a href="#Remote" class="toc-anchor after"></a></h2>

Les liens d'image remote vous permettent d'afficher directement à peu près n'importe quel fichier multimédia via son URL. Cela ne doit pas nécessairement inclure le contenu de votre propre site, mais c'est possible. Voici un exemple d'affichage d'un fichier image distant.

<div class="cadre">
<code>![Remote Image 1](https://getgrav.org/images/testimage.png)</code>
</div>

Vous pouvez créer un lien vers pratiquement n'importe quelle URL directe, y compris les liens HTTPS sécurisés.

<h2 id="Actions media sur les images">Actions media sur les images
<a href="#Actions media sur les images" class="toc-anchor after"></a></h2>

L'un des principaux avantages de l'utilisation d'images associées à des pages est qu'elle vous permet de profiter des puissantes actions média de Grav. Par exemple, voici une ligne que vous utiliseriez pour charger une image depuis une autre page :

<div class="cadre">
<code>![Styling Example](../test-post-3/test-image-3.jpg?cropResize=400,200)</code>
</div>

ou profiter des streams pour accéder à une image dans votre thème actuel :

<div class="cadre">
<code>![Stream Image](theme://images/default-avatar.jpg?cropZoom=200,200&brightness=-75)</code>
</div>

Vous trouverez plus d'informations sur les actions et les autres [fonctionnalités des fichiers multimédias dans le chapitre suivant](./media.md).

<h2 id="Attributs d'image">Attributs d'image
<a href="#Attributs d'image" class="toc-anchor after"></a></h2>

Une nouvelle fonctionnalité intéressante dont vous pouvez profiter consiste à fournir des attributs d'image directement via la syntaxe markdown. Cela vous permet d'ajouter facilement des **classes** et des **id** HTML sans avoir besoin de [Markdown Extra](https://michelf.ca/projects/php-markdown/extra/).

Voici quelques exemples :

<h3 id="Attribut de classe unique">Attribut de classe unique
<a href="#Attribut de classe unique" class="toc-anchor after"></a></h3>

<div class="cadre">
<code>![My Image](my-image.jpg?classes=float-left)</code>
</div>

qui se traduira par un code HTML similaire à :

    <img src="/your/pages/some-page/my-image.jpg" class="float-left" />

<h3 id="Attribut de plusieurs classes">Attribut de plusieurs classes
<a href="#Attribut de plusieurs classes" class="toc-anchor after"></a></h3>

<div class="cadre">
<code>![My Image](my-image.jpg?classes=float-left,shadow)</code>
</div>

qui se traduira par un code HTML similaire à :

    <img src="/your/pages/some-page/my-image.jpg" class="float-left shadow" />

<h3 id="Attribut d'id">Attribut d'id
<a href="#Attribut d'id" class="toc-anchor after"></a></h3>

<div class="cadre">
<code>![My Image](my-image.jpg?id=special-id)</code>
</div>

qui se traduira par un code HTML similaire à :

    <img src="/your/pages/some-page/my-image.jpg" id="special-id" />

