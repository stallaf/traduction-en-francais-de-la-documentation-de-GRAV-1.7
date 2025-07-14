<h1 class = "rem">Syntaxe Markdown</h1>

Avouons-le : écrire du contenu pour le Web est fastidieux. Les éditeurs WYSIWYG aident à alléger cette tâche, mais ils se traduisent généralement par un code horrible, ou pire encore, des pages Web laides.

**Markdown** est une meilleure façon d'écrire du **HTML**, sans toutes les complexités et la laideur qui l'accompagnent habituellement.

Certains des principaux avantages sont :

   1. Markdown est simple à apprendre, avec un minimum de caractères supplémentaires, il est donc également plus rapide d'écrire du contenu.
   2. Moins de risques d'erreurs lors de l'écriture en markdown.
   3. Produit une sortie XHTML valide.
   4. Maintient le contenu et l'affichage visuel séparés, de sorte que vous ne pouvez pas gâcher l'apparence de votre site.
   5. Écrivez dans n'importe quel éditeur de texte ou application Markdown que vous aimez.
   6. Markdown est un plaisir à utiliser !

John Gruber, l'auteur de Markdown, le dit ainsi :

> L'objectif primordial de la conception de la syntaxe de formatage de Markdown est de la rendre aussi lisible que possible. L'idée est qu'un document au format Markdown doit être publiable tel quel, en texte brut, sans donner l'impression qu'il a été conçu avec des balises ou des instructions de formatage. Alors que la syntaxe de Markdown a été influencée par plusieurs filtres texte-HTML existants, la principale source d'inspiration pour la syntaxe de Markdown est le format du courrier électronique en texte brut.
*--John Gruber*

Grav est livré avec un support intégré pour [Markdown](https://daringfireball.net/projects/markdown/) et [Markdown Extra](https://michelf.ca/projects/php-markdown/extra/). Vous devez activer **Markdown Extra** dans votre fichier de configuration `system.yaml`.

Sans plus tarder, passons en revue les principaux éléments de Markdown et à quoi ressemble le HTML résultant :

<div class="notice info">
<span class="fa-bookmark"> Ajoutez</span> cette page à vos favoris pour une référence ultérieure facile !
</div>

<h2 id="Titres">Titres
<a href="#Titres" class="toc-anchor after"></a></h2>

Les titres de `h1` à `h6` sont construits avec un `#` pour chaque niveau :

```html
# h1 Titre
## h2 Titre
### h3 Titre
#### h4 Titre
##### h5 Titre
###### h6 Titre
```

donne :

<h1>h1 Titre</h1>
<h2>h2 Titre</h2>
<h3>h3 Titre</h3>
<h4>h4 Titre</h4>
<h5>h5 Titre</h5>
<h6>h6 Titre</h6>


HTML:

```html
<h1>h1 Titre</h1>
<h2>h2 Titre</h2>
<h3>h3 Titre</h3>
<h4>h4 Titre</h4>
<h5>h5 Titre</h5>
<h6>h6 Titre</h6>
```

<h2 id="Commentaires">Commentaires
<a href="#Commentaires" class="toc-anchor after"></a></h2>

Les commentaires doivent être compatibles HTML

```html
<!--
Ceci est un commentaire
-->
```

Le commentaire ne doit **PAS** être vu :

<h2 id="Lignes horizontales">Lignes horizontales
<a href="#Lignes horizontales" class="toc-anchor after"></a></h2>

L'élément HTML `<hr>` sert à créer une "rupture thématique" entre les éléments de niveau paragraphe. Dans Markdown, vous pouvez créer un `<hr> `avec l'un des éléments suivants :

* `___` : trois traits de soulignement consécutifs
* `---` : trois tirets consécutifs
* `***` : trois astérisques consécutifs

correspondant à :
___
___
___

<h2 id="Corps du texte">Corps du texte
<a href="#Corps du texte" class="toc-anchor after"></a></h2>

Le corps du texte écrit normalement, le texte brut sera entouré de balises `<p></p>` dans le rendu HTML.

Donc, cette copie du corps :

    Lorem ipsum dolor sit amet, graecis denique ei vel, at duo primis mandamus. Et legere ocurreret pri, animal
    tacimates complectitur ad cum. Cu eum inermis inimicus efficiendi. Labore officiis son ex, soluta officiis 
    concludaturque ei qui, vide sensibus vim ad.

donne cela en HTML :

```html
<p>Lorem ipsum dolor sit amet, graecis denique ei vel, at duo primis mandamus. Et legere ocurreret pri, 
animal tacimates complectitur ad cum. Cu eum inermis inimicus efficiendi. Labore officis his ex, soluta 
officiis concludaturque ei qui, vide sensibus vim ad.</p>
```

Un **saut de ligne** peut se faire avec 2 espaces suivis d'1 retour.

<h2 id="HTML en ligne">HTML en ligne
<a href="#HTML en ligne" class="toc-anchor after"></a></h2>

Si vous avez besoin d'une certaine balise HTML (avec une classe), vous pouvez simplement utiliser HTML :

```html
Paragraphe dans Markdown.

<div class="classe">
   Ceci est <b>HTML</b>
</div>

Paragraphe dans Markdown.
```

<h2 id="Rendus">Rendus
<a href="#Rendus" class="toc-anchor after"></a></h2>

<h3 id="Gras">Gras
<a href="#Gras" class="toc-anchor after"></a></h3>

Pour mettre en valeur un extrait de texte avec un poids de police plus lourd.

L'extrait de texte suivant est **affiché en gras**.

```console
**rendra le texte en gras**
```

correspondant à :

**rendra le texte en gras**

et en HTML :

    <strong>rendu en texte gras</strong>

<h3 id="Italique">Italique
<a href="#Italique" class="toc-anchor after"></a></h3>

Pour mettre en évidence un extrait de texte en italique.

L'extrait de texte suivant *est rendu en italique*.

```console
_rendra le texte en italique_
```

correspondant à :

_rendra le texte en italique_

et en HTML :

    <em>rendu sous forme de texte en italique</em>

<h3 id="Barré">Barré
<a href="#Barré" class="toc-anchor after"></a></h3>

Dans GFM (GitHub aromatisé Markdown), vous pouvez faire des barrés.

```console
~~Barrez ce texte.~~
```

correspondant à :

<del>Barrez ce texte.</del>

et en HTML :

    <del>Barrez ce texte.</del>

<h2 id="Blockquotes">Blockquotes
<a href="#Blockquotes" class="toc-anchor after"></a></h2>

Pour citer des blocs de contenu provenant d'une autre source dans votre document.

Ajoutez > avant tout texte que vous souhaitez citer.

```markdown
> **Fusion Drive** combine un disque dur avec un stockage flash (disque SSD) et le présente comme un seul volume logique avec l'espace des deux disques combinés.
```

Correspond à :

<div class = "notice defaut">
<strong>Fusion Drive</strong> combine un disque dur avec un stockage flash (disque SSD) et le présente comme un volume logique unique avec l'espace des deux disques combinés.
</div>

et en HTML :

```html
<blockquote>
   <p><strong>Fusion Drive</strong> combine un disque dur avec un stockage flash 
   (disque SSD) et le présente comme un volume logique unique avec l'espace des 
   deux disques combinés.</p>
</blockquote>
```

Les blocs de citations peuvent également être imbriqués :

```console
> Donec massa lacus, ultricies a ullamcorper in, fermentum sed augue. 
Nunc augue augue, aliquam non hendrerit ac, commodo vel nisi.
  >> Sed adipiscing elit vitae augue consectetur a gravida nunc vehicula. Donec auctor 
  odio non est accumsan facilisis. Aliquam id turpis in dolor tincidunt mollis ac eu diam.
```

Correspond à :

<div class = "notice defaut">
Donec massa lacus, ultricies a ullamcorper in, fermentum sed augue. 
Nunc augue augue, aliquam non hendrerit ac, commodo vel nisi.
<div class = "notice defaut">
Sed adipiscing elit vitae augue consectetur a gravida nunc vehicula. Donec auctor 
odio non est accumsan facilisis. Aliquam id turpis in dolor tincidunt mollis ac eu diam.
</div>
</div>

<h2 id="Notices">Notices
<a href="#Notices" class="toc-anchor after"></a></h2>

<div class="notice note">
L'ancien mécanisme pour les notices remplaçant la syntaxe des apostrophes (<code>>>></code>) est obsolète. Les avis sont désormais gérés via un plugin dédié appelé <a href="https://github.com/getgrav/grav-plugin-markdown-notices">Markdown Notices</a>.
</div>

<h3 id="Listes">Listes
<a href="#Listes" class="toc-anchor after"></a></h3>

<h4 id="Non ordonné">Non ordonnées
<a href="#Non ordonné" class="toc-anchor after"></a></h4>

Une liste d'éléments dans laquelle l'ordre des éléments n'a pas d'importance explicite.

Vous pouvez utiliser l'un des symboles suivants pour désigner les puces de chaque élément de la liste :

    * puce valide
    - puce valide
    + puce valide

Par exemple

    + Lorem ipsum dolor sit amet
    + Consectetur adipiscing élite
    + Integer molestie lorem at massa
    + Facilisis in pretium nisl aliquet
    + Nulla volutpat aliquam velit
      - Phasellus iaculis nèque
      - Ultricies de Purus sodales
      - Vestibulum laoreet porttitor sem
      - Ac tristique libero volutpat à
    + Faucibus porta lacus fringilla vel
    + Aenean sit amet erat nunc
    + Eget porttitor lorem

Correspond à :

+ Lorem ipsum dolor sit amet
+ Consectetur adipiscing élite
+ Integer molestie lorem at massa
+ Facilisis in pretium nisl aliquet
+ Nulla volutpat aliquam velit
    - Phasellus iaculis neque
    - Purus sodales ultricies
    - Vestibulum laoreet porttitor sem
    - Ac tristique libero volutpat at
+ Faucibus porta lacus fringilla vel
+ Aenean sit amet erat nunc
+ Eget porttitor lorem

Et en HTML

```html
<ul>
  <li>Lorem ipsum dolor sit amet</li>
  <li>Consectetur adipiscing elit</li>
  <li>Entier molestie lorem at massa</li>
  <li>Facilisis in pretium nisl aliquet</li>
  <li>Nulla volutpat aliquam velit
    <ul>
      <li>Phasellus iaculis nèque</li>
      <li>Ultricies de Purus sodales</li>
      <li>Vestibulum laoreet porttitor sem</li>
      <li>Ac tristique libero volutpat at</li>
    </ul>
  </li>
  <li>Faucibus porta lacus fringilla vel</li>
  <li>Énée sit amet erat nunc</li>
  <li>Eget porttitor lorem</li>
</ul>
```

<h4 id="Ordonnée">Ordonnées
<a href="#Ordonnée" class="toc-anchor after"></a></h4>

Une liste d'éléments dans lesquels l'ordre des éléments importe explicitement.

    1. Lorem ipsum dolor sit amet
    2. Consectetur adipiscing elit
    3. Integer molestie lorem at massa
    4. Facilisis in pretium nisl aliquet
    5. Nulla volutpat aliquam velit
    6. Faucibus porta lacus fringilla vel
    7. Aenean sit amet erat nunc
    8. Eget porttitor lorem

Correspond à :

1. Lorem ipsum dolor sit amet
2. Consectetur adipiscing elit
3. Integer molestie lorem at massa
4. Facilisis in pretium nisl aliquet
5. Nulla volutpat aliquam velit
6. Faucibus porta lacus fringilla vel
7. Aenean sit amet erat nunc
8. Eget porttitor lorem

Et en HTML :

```html
<ol>
  <li>Lorem ipsum dolor sit amet</li>
  <li>Consectetur adipiscing elit</li>
  <li>Entier molestie lorem at massa</li>
  <li>Facilisis in pretium nisl aliquet</li>
  <li>Nulla volutpat aliquam velit</li>
  <li>Faucibus porta lacus fringilla vel</li>
  <li>Énée sit amet erat nunc</li>
  <li>Eget porttitor lorem</li>
</ol>
```

**ASTUCE** : Si vous n'utilisez que `1`. pour chaque numéro, Markdown numérotera automatiquement chaque élément. Par exemple:

    1. Lorem ipsum dolor sit amet
    1. Consectetur adipiscing elit
    1. Integer molestie lorem at massa
    1. Facilisis in pretium nisl aliquet
    1. Nulla volutpat aliquam velit
    1. Faucibus porta lacus fringilla vel
    1. Aenean sit amet erat nunc
    1. Eget porttitor lorem

Correspond à :

1. Lorem ipsum dolor sit amet
1. Consectetur adipiscing elit
1. Integer molestie lorem at massa
1. Facilisis in pretium nisl aliquet
1. Nulla volutpat aliquam velit
1. Faucibus porta lacus fringilla vel
1. Aenean sit amet erat nunc
1. Eget porttitor lorem

<h3 id="Code">Code
<a href="#Code" class="toc-anchor after"></a></h3>

<h4 id="Code en ligne">Code en ligne
<a href="#Code en ligne" class="toc-anchor after"></a></h4>

Enveloppez les extraits de code en ligne avec <code>`</code>.

    Dans cet exemple, `<section></section>` doit être enveloppé comme **code**.

Correspond à :

Dans cet exemple, `<section></section>` doit être enveloppé de **code**.

et en HTML :

    <p>Dans cet exemple, <code>&lt;section&gt;&lt;/section&gt;</code> doit être entouré de <strong>code</strong>.</p>

<h4 id="Code indenté">Code indenté
<a href="#Code indenté" class="toc-anchor after"></a></h4>

Ou indentez plusieurs lignes de code d'au moins quatre espaces, comme dans :

<pre>
    // Du commentaire
    ligne 1 du code
    ligne 2 du code
    ligne 3 du code
</pre>

Ce qui rendra :

    // Certains commentaires
    ligne 1 du code
    ligne 2 du code
    ligne 3 du code

Et en HTML :

```html
<pre>
  <code>
    // Certains commentaires
    ligne 1 du code
    ligne 2 du code
    ligne 3 du code
  </code>
</pre>
```

<h4 id="Code de bloc "fences"">Code de bloc "fences"
<a href="#Code de bloc "fences"" class="toc-anchor after"></a></h4>

Utilisez "fences" <code>```</code> pour bloquer plusieurs lignes de code avec un attribut de langue.

    ```
    Exemple de texte ici...
    ```

Et en HTML :

    <pre language-html>
      <code>Exemple de texte ici...</code>
    </pre>

<h4 id="Mise en évidence de la syntaxe">Mise en évidence de la syntaxe
<a href="#Mise en évidence de la syntaxe" class="toc-anchor after"></a></h4>

GFM, ou "GitHub Flavored Markdown" prend également en charge la coloration syntaxique. Pour l'activer, ajoutez simplement l'extension de fichier du langage que vous souhaitez utiliser directement après le premier code "fence", ````js`, et la coloration syntaxique sera automatiquement appliquée dans le rendu HTML. Par exemple, pour appliquer la coloration syntaxique au code JavaScript :

<pre>
    ```js
    grunt.initConfig({
      assemble: {
        options: {
          assets: 'docs/assets',
          data: 'src/data/*.{json,yml}',
          helpers: 'src/custom-helpers.js',
          partials: ['src/partials/**/*.{hbs,md}']
        },
        pages: {
          options: {
            layout: 'default.hbs'
          },
          files: {
            './': ['src/templates/pages/index.hbs']
          }
        }
      }
    };
    ```
</pre>

Correspond  à :

```js
    grunt.initConfig({
      assemble: {
        options: {
          assets: 'docs/assets',
          data: 'src/data/*.{json,yml}',
          helpers: 'src/custom-helpers.js',
          partials: ['src/partials/**/*.{hbs,md}']
        },
        pages: {
          options: {
            layout: 'default.hbs'
          },
          files: {
            './': ['src/templates/pages/index.hbs']
          }
        }
      }
    };
```

<div class="notice tip">
Pour que la coloration syntaxique fonctionne, le <a href="https://github.com/getgrav/grav-plugin-highlight">plugin Highlight</a> doit être installé et activé. Il utilise à son tour un plugin jquery, donc jquery doit également être chargé dans votre thème.
</div>

<h3 id="Les tableaux">Les tableaux
<a href="#Les tableaux" class="toc-anchor after"></a></h3>

Les tableaux sont créés en ajoutant des barres comme séparateurs entre chaque cellule et en ajoutant une ligne de tirets (également séparés par des barres) sous l'en-tête. Notez que les pipes n'ont pas besoin d'être alignés verticalement.

    | Option | Description |
    | ------ | ----------- |
    | data   | path to data files to supply the data that will be passed into templates. |
    | engine | engine to be used for processing templates. Handlebars is the default. |
    | ext    | extension to be used for dest files. |

Correspond à :

| Option | Description |
| ------ | ----------- |
| data   | path to data files to supply the data that will be passed into templates. |
| engine | engine to be used for processing templates. Handlebars is the default. |
| ext    | extension to be used for dest files. |

Et en HTML :

```html
<tableau>
  <tête>
    <tr>
      <th>Option</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>données</td>
      <td>chemin d'accès aux fichiers de données pour fournir les données qui seront transmises aux modèles.</td>
    </tr>
    <tr>
      <td>moteur</td>
      <td>moteur à utiliser pour le traitement des modèles. Le guidon est la valeur par défaut.</td>
    </tr>
    <tr>
      <td>ext</td>
      <td>extension à utiliser pour les fichiers dest.</td>
    </tr>
  </tbody>
</table>
```

<h4 id="Texte aligné à droite">Texte aligné à droite
<a href="#Texte aligné à droite" class="toc-anchor after"></a></h4>

L'ajout de deux-points sur le côté droit des tirets sous n'importe quel titre alignera à droite le texte de cette colonne.

    | Option | Description |
    | ------:| -----------:|
    | data   | path to data files to supply the data that will be passed into templates. |
    | engine | engine to be used for processing templates. Handlebars is the default. |
    | ext    | extension to be used for dest files. |

| Option | Description |
| ------:| -----------:|
| data   | path to data files to supply the data that will be passed into templates. |
| engine | engine to be used for processing templates. Handlebars is the default. |
| ext    | extension to be used for dest files. |

<h3 id="Liens">Liens
<a href="#Liens" class="toc-anchor after"></a></h3>

<h4 id="Lien de base">Lien de base
<a href="#Lien de base" class="toc-anchor after"></a></h4>

    [Assemble] (https://assemble.io)

Rend (survolez le lien, il n'y a pas d'info-bulle):

[Assemble](https://assemble.io/)

Et en HTML :

    <a href="https://assemble.io">Assembler</a>

<h4 id="Ajouter un titre">Ajouter un titre
<a href="#Ajouter un titre" class="toc-anchor after"></a></h4>

    [Upstage](https://github.com/upstage/ "Visitez Upstage!")

Rend (passez la souris sur le lien, il devrait y avoir une info-bulle):

[Upstage](https://github.com/upstage/ "Visitez Upstage!")

En HTML :

    <a href="https://github.com/upstage/" title="Visitez Upstage!">Upstage</a>

<h4 id="Ancres nommées">Ancres nommées
<a href="#Ancres nommées" class="toc-anchor after"></a></h4>

Les ancres nommées vous permettent d'accéder au point d'ancrage spécifié sur la même page. Par exemple, chacun de ces chapitres :

```console
# Table des matières
 * [Chapitre 1](#chapitre-1)
 * [Chapitre 2](#chapitre-2)
 * [Chapitre 3](#chapitre-3)
```

passera à ces sections :

    ## Chapitre 1 <a id="chapter-1"></a>
    Contenu du premier chapitre.

    ## Chapitre 2 <a id="chapter-2"></a>
    Contenu du premier chapitre.

    ## Chapitre 3 <a id="chapter-3"></a>
    Contenu du premier chapitre.

**REMARQUE** que le placement spécifique de la balise d'ancrage semble être arbitraire. Ils sont placés en ligne ici car cela semble être discret et cela fonctionne.

<h3 id="Images">Images
<a href="#Images" class="toc-anchor after"></a></h3>

Les images ont une syntaxe similaire aux liens mais incluent un point d'exclamation précédent.

<div class="notice cadre">
<code>![Minion](https://octodex.github.com/images/minion.png)</code>
</div>

![Minion](https://octodex.github.com/images/minion.png)

ou alors:

<div class="notice cadre">
<code>![ALt text](https://octodex.github.com/images/stormtroopocat.jpg "Le Stormtroopocat")</code>
</div>

![Alt text](https://octodex.github.com/images/stormtroopocat.jpg "Le Stormtroopocat")
    
Comme les liens, les images ont également une syntaxe de style note de bas de page :

<div class="notice cadre">
<code>![Alt text][id]</code>
</div>

![Alt text](https://octodex.github.com/images/dojocat.jpg)

Avec une référence plus loin dans le document définissant l'emplacement de l'URL :

<div class="notice cadre">
<code>[id] : https://octodex.github.com/images/dojocat.jpg "Le Dojocat"</code>
</div>


