#Liaison de pages

Grav propose une variété d'options de liaison flexibles qui vous permettent de créer un lien d'une page à une autre, et même à des pages distantes. Si vous avez déjà lié des fichiers à l'aide de HTML ou travaillé avec un système de fichiers à l'aide d'une ligne de commande, cela devrait être très facile à comprendre.

Nous allons passer à quelques exemples simples en utilisant ce modèle très basique et réduit de ce à quoi pourrait ressembler le répertoire **Pages** d'un site Grav.

![Annuaire des pages](https://learn.getgrav.org/user/pages/02.content/05.linking/pages.jpg)

En utilisant cette structure de répertoires comme exemple, nous examinerons les différents types de liens que vous pouvez utiliser dans votre contenu.

Pour commencer, voici un bref aperçu de certains des composants communs d'un lien Grav et de ce qu'ils signifient.

<div class="cadre">
<code>[Contenu lié](../chemin/slug/page)</code>
</div>
<br>

| **CHAINE**             | **DESCRIPTION**
| :---------             | :---------
| `[]`                   | Le crochet est utilisé pour envelopper le texte ou le contenu alternatif <br>qui devient lié. En HTML, ce serait le contenu placé entre<br> `<a href="">` et `</a>`.
| `()`                   | La parenthèse est utilisée pour entourer le lien lui-même.<br>Ceci est placé directement après le crochet.
| `../`                  | Lorsqu'il est utilisé dans le lien, il indique un<br> déplacement vers le haut d'un répertoire.

<h2 id="Slug Relatif">Slug Relatif
<a href="#Slug Relatif" class="toc-anchor after"></a></h2>

Grav ne se contente pas de limiter vos liens internes à des noms spécifiques dans votre structure de fichiers/répertoires. Il peut également extraire les slugs attribués à la fois dans l'en-tête du fichier, ainsi que dans le nom du répertoire de secours. Cela facilite la création de liens rapides car vous n'avez pas à vous souvenir du nom de fichier spécifique, mais d'un slug facilement mémorisable (et pertinent).

Le moteur de template de Grav utilise les noms de fichiers pour déterminer quel template leur appliquer. Par exemple, un blog peut utiliser le nom de fichier standard `item.md` pour chaque article de blog. Le billet de blog lui-même peut se voir attribuer un slug qui a plus de sens, comme `grass` ou `grass-is-green`.

Les noms de répertoire ont également des numéros attribués, ce qui facilite la commande. Vous n'êtes pas obligé d'inclure ces chiffres avec des liens relatifs aux slugs. Grav les ignore lors de la création du slug, la structure d'URL de votre site est donc plus propre.

Voici quelques exemples de liens slug-relatif.

Dans cet exemple, nous montons d'un répertoire et chargeons la page par défaut située dans le répertoire `pages/01.blue/02.water/item.md` depuis `pages/01.blue/01.sky/item.md`. Le fichier, `item.md`, n'a pas de slug assigné, donc Grav utilise le nom du répertoire.

<div class="cadre">
<code>[link](../water)</code>
</div>
<br>
Cet exemple suivant fait une chose très similaire, reliant les pages/`01.blue/01.sky/item.md` aux `pages/02.green/02.tree/item.md`, mais lors du chargement du fichier `item.md`, un slug a été affecté au dossier de `tree-is-green`.

<div class="cadre">
<code>[link](../../green/tree-is-green)</code>
</div>
<br>
Le slug placé dans l'en-tête de `item.md` remplace le slug `green` du nom de dossier par défaut.

<h2 id="Répertoire relatif">Répertoire relatif
<a href="#Répertoire relatif" class="toc-anchor after"></a></h2>

Les liens **relatifs au répertoire** utilisent des destinations définies par rapport à la page actuelle. Cela peut être aussi simple que de créer un lien vers un autre fichier dans le répertoire actuel, tel qu'un fichier image, ou aussi complexe que de remonter plusieurs niveaux de répertoire, puis de revenir au dossier/fichier spécifique que vous devez afficher.

Avec des liens relatifs, l'emplacement du fichier source est tout aussi important que celui de la destination. Si l'un ou l'autre des fichiers du mixage est déplacé, en modifiant le chemin entre eux, le lien peut être rompu.

L'avantage de ce type de structure de liaison est que vous pouvez facilement basculer entre un serveur de développement local et un serveur de production avec un nom de domaine différent et tant que la structure de fichiers reste cohérente, les liens devraient fonctionner sans problème.

Un lien de fichier pointe vers un fichier particulier par son nom, plutôt que par son répertoire ou son slug. Si vous créiez un lien de `pages/01.blue/01.sky/item.md` vers `/pages/02.green/01.grass/item.md`, vous utiliseriez la commande suivante.

<div class="cadre">
<code>[link](../../02.green/01.grass/item.md)</code>
</div>
<br>
Ce lien monte de deux dossiers, comme indiqué par `../../`, puis descend de deux dossiers, pointant directement vers `item.md` comme destination.

Parfois, vous souhaitez simplement diriger l'utilisateur vers un seul répertoire, en chargeant la page par défaut. Puisqu'un fichier exact n'est pas indiqué, Grav est chargé de choisir le bon à charger. Pour un site Grav bien organisé, cela ne devrait pas poser de problème.

Dans cet exemple, nous allons lier les `pages/01.blue/01.sky/item.md` à /`pages/02.green/` qui chargerait le fichier `color.md` par défaut.

<div class="cadre">
<code>[link](../../02.green)</code>
</div>
<br>
Si vous souhaitez créer un lien vers un répertoire deux étapes plus haut, vous pouvez le faire en utilisant ce processus.

L'exemple suivant ressemble beaucoup au lien de fichier que nous avons démontré précédemment. Au lieu de créer un lien direct vers le fichier, nous lions vers son répertoire, qui devrait de toute façon charger le fichier que nous voulons car c'est le fichier par défaut. Si vous créez un lien de `pages/01.blue/01.sky/item.md` vers `/pages/02.green/01.grass/` vous utiliserez la commande suivante.

<div class="cadre">
<code>[link](../../02.green/01.grass)</code>
</div>
<br>
<h2 id="Absolu">Absolu
<a href="#Absolu" class="toc-anchor after"></a></h2>

Les liens absolus sont similaires aux liens relatifs, mais sont relatifs à la racine du site. Dans **Grav**, ceci est généralement basé dans votre répertoire **/user/pages/**. Ce type de lien peut se faire de deux manières différentes.

Vous pouvez le faire de la même manière que le style **Slug Relatif** qui utilise le slug ou le nom du répertoire dans le chemin pour plus de simplicité. Cette méthode supprime les problèmes potentiels de changements de commande ultérieurs (changement du numéro au début du nom du dossier) rompant le lien. Ce serait la méthode de liaison absolue la plus couramment utilisée.

Dans un lien absolu, le lien s'ouvre avec un `/`. Voici un exemple de lien absolu fait vers `pages/01.blue/01.sky/item.md` dans le style **Slug**.

<div class="cadre">
<code>[link](/blue/sky)</code>
</div>
<br>
La deuxième méthode est façonnée d'après le style **Directory Relative** décrit précédemment. Cette méthode laisse des éléments tels que les numéros de classement au début des noms de répertoire. Bien que cela ajoute le potentiel d'un lien rompu lorsque le contenu est réorganisé, il est plus fiable lorsqu'il est utilisé avec des services comme [Github](https://github.com/) où les liens de contenu ne bénéficient pas de la flexibilité de Grav. Voici un exemple de lien absolu fait vers `pages/01.blue/01.sky/item.md` en utilisant ce style.

<div class="cadre">
<code>[link](/01.blue/01.sky)</code>
</div>
<br>
<h2 id="Distants">Distants
<a href="#Distants" class="toc-anchor after"></a></h2>

Les liens distants vous permettent de vous connecter directement à pratiquement n'importe quel fichier ou document via son URL. Cela ne doit pas nécessairement inclure le contenu de votre propre site, mais c'est possible. Voici un exemple de lien vers la page d'accueil de Google.

<div class="cadre">
<code>[link](http://www.google.com)</code>
</div>

Vous pouvez créer un lien vers pratiquement n'importe quelle URL directe, y compris les liens HTTPS sécurisés. Par exemple:

<div class="cadre">
<code>[link](https://github.com)</code>
</div>
<br>
<h2 id="Attributs de liens">Attributs de liens
<a href="#Attributs de liens" class="toc-anchor after"></a></h2>

Une nouvelle fonctionnalité intéressante dont vous pouvez profiter consiste à fournir des attributs de lien directement via la syntaxe de démarquage. Cela vous permet d'ajouter facilement des attributs HTML **class**, **id**, **rel** et **target** sans avoir besoin de [Markdown Extra](https://michelf.ca/projects/php-markdown/extra/).

Voici quelques exemples :

<h3 id="Classe/Classes d'Attributs">Classe/Classes d'Attributs
<a href="#Classe/Classes d'Attributs" class="toc-anchor after"></a></h3>

<div class="cadre">
<code>[Big Button](../some-page?classes=button,big)</code>
</div>

qui se traduira par un code HTML similaire à :

```html
<a href="/your/pages/some-page" class="button,big">Big Button</a>
```

<h3 id="ID Attribut">ID Attribut
<a href="#ID Attribut" class="toc-anchor after"></a></h3>

<div class="cadre">
<code>[Unique Button](../some-page?id=important-button)</code>
</div>

qui se traduira par un code HTML similaire à :

```html
<a href="/your/pages/some-page" id="important-button">Unique Button</a>
```

<h3 id="Rel Attribut">Rel Attribut
<a href="#Rel Attribut" class="toc-anchor after"></a></h3>

<div class="cadre">
</code>[NoFollow Link](../some-page?rel=nofollow)</code>
</div></div>

qui se traduira par un code HTML similaire à :

    <a href="/your/pages/some-page" rel="nofollow">NoFollow Link</a>

<h3 id="Target Attribut">Target Attribut
<a href="#Target Attribut" class="toc-anchor after"></a></h3>

<div class="cadre">
<code>[Link in new Tab](../some-page?target=_blank)</code>
</div>
<br>
qui se traduira par un code HTML similaire à :

    <a href="/your/pages/some-page" target="_blank">Link in new Tab</a>

<h3 id="Combinaisons d'Attributs">Combinaisons d'Attributs
<a href="#Combinaisons d'Attributs" class="toc-anchor after"></a></h3>

<div class="cadre">
<code>[Combinaisons d'Attributs](../some-page?target=_blank&classes=button)</code>
</div>
<br>
qui se traduira par un code HTML similaire à :

    <a href="/your/pages/some-page" target="_blank" class="button">Combinations of Attributes</a>

<h3 id="Combinaisons d'Attributs avec ancres">Combinaisons d'Attributs avec ancres
<a href="#Combinaisons d'Attributs avec ancres" class="toc-anchor after"></a></h3>

<div class="cadre">
<code>[Element Ancre](../some-page?target=_blank&classes=button#element-id)</code>
</div>
<br>
qui se traduira par un code HTML similaire à :

    <a href="/your/pages/some-page#element-id" target="_blank" class="button">Element Anchor</a>

<h3 id="Liens d'ancrage dans la même page">Liens d'ancrage dans la même page
<a href="#Liens d'ancrage dans la même page" class="toc-anchor after"></a></h3>

<div class="cadre">
<code>[Element Ancre](?classes=button#element-id)</code>
</div>
<br>
qui se traduira par un code HTML similaire à :

    <a href="#element-id" class="button">

Notez que l'ancre doit venir *après* la requête, comme indiqué dans le [problème 1324](https://github.com/getgrav/grav/issues/1324#issuecomment-282587549)

<h3 id="Transmission d'un attribut non pris en charge">Transmission d'un attribut non pris en charge
<a href="#Transmission d'un attribut non pris en charge" class="toc-anchor after"></a></h3>

<div class="cadre">
<code>[Pass-through of 'cat' attribute](../some-page?classes=underline&cat=black)</code>
</div>
<br>
qui se traduira par un code HTML similaire à :

    <a href="/your/pages/some-page?cat=black" class="underline">Pass-through of 'cat' attribute</a>

<h3 id="Ignorer tous les Attribus">Ignorer tous les Attribus
<a href="#Ignorer tous les Attribus" class="toc-anchor after"></a></h3>

<div class="cadre">
<code>[Ignorer tous les Attribus](../some-page?classes=underline&rel=nofollow&noprocess)</code>
</div>
<br>
qui se traduira par un code HTML similaire à :

    <a href="/your/pages/some-page?rel=nofollow&classes=underline">Skip All attributes</a>

<h3 id="Ignorer Certains Attribus">Ignorer Certains Attribus
<a href="#Ignorer Certains Attribus" class="toc-anchor after"></a></h3>

<div class="cadre">
<code>[Ignorer Certains Attribus](../some-page?id=myvariable&classes=underline&target=_blank&noprocess=id,classes)</code>
</div>
<br>
qui se traduira par un code HTML similaire à :

    <a href="/your/pages/some-page?id=myvariable&classes=underline" target="_blank">Skip Certain attributes</a>

Ceci est utile lorsque vous essayez d'ignorer un ou plusieurs attributs spécifiques sans affecter les autres.

