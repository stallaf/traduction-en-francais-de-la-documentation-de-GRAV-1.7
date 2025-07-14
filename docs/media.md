<h1 class="rem">Médias</h1>

Lors de la création de contenu dans **Grav**, vous devez souvent afficher différents types de médias tels que **des images**, **des vidéos** et divers autres **fichiers**. Ces fichiers sont automatiquement trouvés et traités par Grav et sont mis à disposition pour être utilisés par n'importe quelle page. Ceci est particulièrement pratique car vous pouvez ensuite utiliser la fonctionnalité intégrée de la page pour exploiter les vignettes, accéder aux métadonnées et modifier les médias de manière dynamique (par exemple, redimensionner les images, définir la taille d'affichage des vidéos, etc.) selon vos besoins.

Grav utilise un système de **mise en cache intelligent** qui crée automatiquement des copies en cache des médias générés dynamiquement si nécessaire. De cette façon, toutes les requêtes ultérieures peuvent utiliser la version mise en cache au lieu d'avoir à générer à nouveau le média.

<h2 id="Fichiers multimédia pris en charge">Fichiers multimédia pris en charge
<a href="#Fichiers multimédia pris en charge" class="toc-anchor after"></a></h2>

Les types de fichiers multimédias suivants sont pris en charge nativement par Grav. Une prise en charge supplémentaire des fichiers multimédias et des intégrations de streaming peut être ajoutée via des plugins.

|Type de média          | Type de fichier
| :---------            | :---------
| Image                 | jpg, jpeg, png
| Audio                 | mp3, wav, wma, ogg, m4a, aiff, aif
| Image animée          | gif
| Image vectorisée      | svg
| Vidéo                 | mp4, mov, m4v, swf, flv, webm, ogv
| Données / Informations   | txt, doc, docx, html, htm, pdf, zip, gz, 7z, tar, css, js,<br> json, xml, xls, xlt, xlm, xlsm, xld, xla, xlc, xlw, xll, ppt ,<br> pps, rtf, bmp, tiff, mpeg, mpg, mpe, avi, wmv

Une liste complète des types mime pris en charge se trouve dans le fichier `system/config/media.yaml`. S'il existe un type mime qui n'est pas actuellement pris en charge, vous pouvez simplement créer votre propre `user/config/media.yaml` et l'y ajouter. Assurez-vous simplement de suivre le même format que le fichier `system` d'origine. L'approche la plus simple consiste à copier l'intégralité du fichier d'origine et à apporter vos modifications.

<h2 id="Où placer vos fichiers multimédias">Où placer vos fichiers multimédias
<a href="#Où placer vos fichiers multimédias" class="toc-anchor after"></a></h2>

Habituellement, vous utiliserez un fichier multimédia dans une page, il vous suffit donc de placer le fichier dans le dossier de la page, et vous pouvez le référencer dans le Markdown de la page, par exemple :

<div class="cadre">
<code>![my image](image.jpg)</code>
</div>

Si vous souhaitez mettre toutes vos images dans un seul dossier, vous pouvez les mettre dans un dossier `/user/pages/images`. De cette façon, dans Twig, vous pouvez les joindre via :

    {% set my_image = page.find('/images').media['my-image.jpg'] %}

et vous pouvez également les trouver facilement via markdown et effectuer des opérations dessus :

<div class="cadre">
<code>![my image](/images/my-image.jpg?cropResize=300,300)</code>
</div>

Alternativement, vous pouvez les mettre dans votre thème, car cela est facilement accessible via des références CSS ou à partir d'un fichier Markdown en utilisant le flux `theme://` :

<div class="cadre">
<code>![my image](theme://images/theme-image.jpg)</code>
</div>

Une autre option est `user/images`, où vous pouvez utiliser le flux `image://`  pour y accéder :

<div class="cadre">
<code>![my image](image://my-image.jpg)</code>
</div>

Vous pouvez en fait utiliser n'importe quel flux, y compris n'importe quel dossier à l'intérieur de `user/` via le flux `user://` :

<div class="cadre">
<code>![my image](user://themes/mytheme/images/my-image.jpg)</code>
</div>

Vous pouvez également faire ce même genre de choses en utilisant l'objet Twig `Media` :

```twig
{{ media['user://themes/mytheme/images/my-image.jpg'].html()|raw }}
```

<div class="notice warning">
Grav a un dossier <code>/images</code>. Ne placez pas vos propres images dans ce dossier, car il héberge des images en cache générées automatiquement par Grav.
</div>

Vous pouvez également placer tous les fichiers multimédias dans leur propre dossier, afin qu'ils soient tous accessibles en une seule fois. Par exemple, vous pouvez conserver tous vos fichiers MP3 dans un dossier `user/pages/mp3s` (non visible) et mettre le nom du fichier MP3 associé à une page particulière dans un champ d'en-tête appelé `thistrack`. Si vous souhaitez ensuite accéder au fichier d'une page particulière et le lire à l'aide de l'élément audio HTML5, vous aurez besoin d'un code comme celui-ci :

```html
1 | <audio controls>
2 |    <source src="{{ page.find('mp3s').media[page.header.thistrack~'.mp3']|e }}">
3 | </audio>
```
  
<h2 id="Modes d'affichage">Modes d'affichage
<a href="#Modes d'affichage" class="toc-anchor after"></a></h2>

Grav fournit quelques modes d'affichage différents pour chaque type d'objet multimédia.

| MODE         | EXPLICATION
| :---------   | :---------
| source       | Représentation visuelle du média lui-même, c'est-à-dire<br> l'image, la vidéo ou le fichier réel
| text         | Représentation textuelle du média
| vignette     | La vignette de cet objet multimédia

<div class="notice warning">
Les médias de type <strong>Données/Informations</strong> ne prennent pas en charge le mode <code>source</code>, ils passeront par défaut en mode <code>text</code> si un autre mode n'est pas explicitement choisi.
</div>

<h2 id="Emplacement de la vignette">Emplacement de la vignette
<a href="#Emplacement de la vignette" class="toc-anchor after"></a></h2>

Il y a trois endroits où Grav cherchera votre vignette.

1. Dans le même dossier que votre fichier multimédia : `[media-name].[media-extension].thumb.[thumb-extension]` où `media-name` et `media-extension` sont respectivement le nom et l'extension du fichier multimédia d'origine et la `thumb-extension` est toute extension prise en charge par le type de média `image`. Les exemples sont `my_video.mp4.thumb.jpg` et `my-image.jpg.thumb.png` **Pour les images uniquement !** L'image elle-même sera utilisée comme vignette.
2. Votre dossier utilisateur : `user/images/media/thumb-[media-extension].png` où `media-extension` est l'extension du fichier multimédia d'origine. Les exemples sont `thumb-mp4.png` et `thumb-jpg.jpg`.
3. Le dossier système : `system/images/media/thumb-[media-extension].png` où `media-extension` est l'extension du fichier multimédia d'origine. **Les vignettes dans les dossiers système sont pré-fournies par Grav**.

<div class="notice info">
Vous pouvez également sélectionner manuellement la vignette souhaitée avec les actions expliquées ci-dessous.
</div>

<h2 id="Liens et Lightbox">Liens et Lightbox
<a href="#Liens et Lightbox" class="toc-anchor after"></a></h2>

Les modes d'affichage ci-dessus peuvent également être utilisés en combinaison avec des liens et des lightboxes, qui sont expliqués plus en détail plus tard. Il est toutefois important de noter :

<div class="notice warning">
Grav ne fournit pas de fonctionnalité lightbox prête à l'emploi, vous avez besoin d'un plugin pour cela. Vous pouvez utiliser le plugin <a href="https://github.com/getgrav/grav-plugin-featherlight">Feather Light Grav plugin</a> pour faire çà.
</div>

Lorsque vous utilisez la fonctionnalité multimédia de Grav pour rendre une lightbox, tout ce que fait Grav est de générer une balise **d'ancrage** qui a certains attributs à lire par le plugin lightbox. Si vous souhaitez utiliser une bibliothèque lightbox qui ne se trouve pas dans notre référentiel de plugins ou si vous souhaitez créer votre propre plugin, vous pouvez utiliser le tableau ci-dessous comme référence.

| ATTRIBUT           | EXPLICATION
| :---------         | :---------
| rel                | Un simple indicateur qu'il ne s'agit pas d'un lien régulier,<br>mais d'un lien lightbox. La valeur sera toujours `lightbox`.
| href               | Une URL vers l'objet multimédia lui-même.
| data-width         | La largeur demandée par l'utilisateur pour cette lightbox.
| data-height        | La hauteur demandée par l'utilisateur pour cette lightbox.
| data-srcset        | Dans le cas d'un média image, ceci<br> contient la chaîne `srcset`. [Plus d'informations](/media#Images reactives)

<h2 id="Actions">Actions
<a href="#Actions" class="toc-anchor after"></a></h2>

Grav utilise un **modèle de construction** lors de la gestion des médias, vous pouvez donc effectuer **plusieurs actions** sur un support particulier. Certaines actions sont disponibles pour chaque type de support tandis que d'autres sont spécifiques au support.

<h3 id="Général">Général
<a href="#Général" class="toc-anchor after"></a></h3>

Ces actions sont disponibles pour tous les types de médias.

<h4 id="url">url
<a href="#url" class="toc-anchor after"></a></h4>

<div class="notice info">
Cette méthode est uniquement destinée à être utilisée dans les templates <strong>Twig</strong>, d'où l'absence de syntaxe Markdown.
</div>

Cela renvoie le **chemin d'URL brut** vers le média.

<div class="tabs">
   <div id="Twig"><a href="#Twig">Twig</a>
      <div>
<pre class="pre">{{ page.media['sample-image.jpg'].url|e }}</pre>
      </div>
   </div>
   <div id="tab2"><a href="#tab2">HTML Code</a>
      <div>  
<pre class="pre">/images/a/f/2/8/f/af28f2ad724f1e05248ac8dd518b2a5789c6cd41-sample-image.jpg</pre>
      </div>
   </div>  
</div>

<h4 id="html">html
<a href="#html" class="toc-anchor after"></a></h4>

<div class="notice info">
Dans Markdown, cette méthode est implicitement appelée lors de l'utilisation de la syntaxe <code>![]</code>.
</div>

L'action `html` générera une balise HTML valide pour le média en fonction du mode d'affichage actuel.

<div class="tabs">
   <div id="Markdown"><a href="#Markdown">Markdown</a>
      <div class="content">
<pre class="pre">![Some ALT text](sample-image.jpg?classes=myclass "My title")</pre>
      </div>
   </div>
   <div id="tab3"><a href="#tab3">Twig</a>
      <div>  
<pre class="pre">{{ page.media['sample-image.jpg'].html('My title', 'Some ALT text',
 'myclass')|raw }}</pre>
      </div>
   </div>  
   <div id="tab4"><a href="#tab4">HTML Code</a>
      <div>  
<pre class="pre">&lt;img title="My title" alt="Some ALT text" class="myclass"<br>src="/images/a/f/2/8/f/af28f2ad724f1e05248ac8dd518b2a5789c6cd41<br>-sample-image.jpg" /&gt;</pre>
      </div>
   </div>    
</div>

<h4 id="affichage">affichage
<a href="#affichage" class="toc-anchor after"></a></h4>

Utilisez cette action pour basculer entre les différents modes d'affichage fournis par Grav. Une fois que vous avez changé de mode d'affichage, toutes les actions précédentes seront réinitialisées. Les exceptions à cette règle sont les actions `lightbox` et `link` et toutes les actions qui ont été utilisées avant ces deux actions.

Par exemple, la vignette qui résulte de l'appel de `page.media['sample-image.jpg'].sepia().display('thumbnail').html()` n'aura pas l'action `sepia()` appliquée, mais `page.media ['sample-image.jpg'].display('thumbnail').sepia().html()` le fera.

<div class="notice note">
Une fois que vous passez en mode vignette, vous manipulez une image. Cela signifie que même si votre média actuel est une vidéo, vous pouvez utiliser toutes les actions de type image sur la vignette.
</div>

<h4 id="lien">lien
<a href="#lien" class="toc-anchor after"></a></h4>

Transformez votre objet média en lien. Toutes les actions que vous appelez avant `link()` seront appliquées à la cible du lien, tandis que toutes les actions appelées après s'appliqueront à ce qui est affiché sur votre page.

<div class="notice info">
Après avoir appelé <code>link()</code>, Grav basculera automatiquement le mode d'affichage sur <strong>vignette</strong>.
</div>

L'exemple suivant affiche un lien textuel `(display('text'))` vers une version sépia du fichier `sample-image.jpg` :

<div class="tabs">
   <div id="tab6"><a href="#tab6">Markdown</a>
      <div class="content">
<pre class="pre">![Image link](sample-image.jpg?sepia&link&display=text)</pre>
      </div>
   </div>
   <div id="tab7"><a href="#tab7">Twig</a>
      <div class="content">
<pre class="pre">{{ page.media['sample-image.jpg'].sepia().link().<br>display('text').html('Image link')|raw }}</pre>
      </div>
   </div>
   <div id="tab8"><a href="#tab8">HTML Code</a>
      <div class="content">
<pre class="pre">&lt;a href="/images/3/7/7/1/c/3771c3fabb6d7bc0035dd119281718f9143c4653-sample<br>-image.jpg"&gt;>&lt;img title="Image link" alt="" src="/images/a/f/2/8/<br>faf28f2ad724f1e05248ac8dd518b2a5789c6cd41-sample-image.jpg" /&gt;</a></pre>
      </div>
   </div>
</div>

<h4 id="Cache uniquement">Cache uniquement
<a href="#Cache uniquement" class="toc-anchor after"></a></h4>

Grav peut être configuré pour mettre en cache tous les fichiers image, cela peut augmenter la vitesse à laquelle les fichiers sont servis. Cependant, les images passeront par le système de manipulation d'images Grav, ce qui peut entraîner une taille de fichier considérablement plus grande pour les images qui ont déjà été optimisées avant Grav. La manipulation d'image peut être contournée.

Activer `cache_all` dans `system/config/system.yaml`.

    images:
      default_image_quality: 85
      cache_all: false

Désactivez la manipulation d'image avec l'option `cache`.

<div class="tabs">
   <div id="tab9"><a href="#tab9">Markdown</a>
      <div class="content">
<pre class="pre">![]\(sample-image.jpg?cache)</pre>
      </div>
   </div>
   <div id="tab10"><a href="#tab10">Twig</a>
      <div class="content">
<pre class="pre">{{ page.media['sample-image.jpg'].cache.html()|raw }}</pre>
      </div>
   </div>
   <div id="tab11"><a href="#tab11">HTML Code</a>
      <div class="content">
<pre class="pre">&lt;img alt="" src="/images/a/f/2/8/f/af28f2ad724f1e05248ac8dd518b2a5789c6cd41<br>-sample-image.jpg" /&gt;</pre>
      </div>
   </div>
</div>

<h4 id="lightbox">lightbox
<a href="#lightbox" class="toc-anchor after"></a></h4>

L'action lightbox est essentiellement la même que l'action lien mais avec quelques extras. Comme expliqué ci-dessus (Liens et lightboxes), l'action lightbox ne fera rien de plus que de créer un lien avec des attributs supplémentaires. Elle diffère de l'action de lien en ce qu'elle ajoute un attribut `rel="lightbox"` et accepte un attribut `width` et `height`.

Si possible (actuellement uniquement dans le cas des images), Grav redimensionnera votre média à la largeur et à la hauteur demandées. Sinon, il ajoutera simplement un attribut `data-width` et `data-height` au lien.

<div class="tabs">
   <div id="tab12"><a href="#tab12">Markdown</a>
      <div class="content">
<pre class="pre">![Sample Image](sample-image.jpg?lightbox=600,400&resize=200,200)</pre>
      </div>
   </div>
   <div id="tab13"><a href="#tab13">Twig</a>
      <div class="content">
<pre class="pre">{{ page.media['sample-image.jpg'].lightbox(600,400).resize(200,200).html
('Sample Image')|raw }}</pre>
      </div>
   </div>
   <div id="tab14"><a href="#tab14">HTML Code</a>
      <div class="content">
<pre class="pre">&lt;a rel="lightbox" data-width="600" data-height="400" href="/images/b/5/e/b/3/b5eb31744b96349b1a711697692b897624202cb1-sample-image.jpg"&gt;</br>&lt;img title="Sample Image" alt="" src="/images/4/5/5/e/4/455e41587c2cd25f34cfdccd8ab5078707aabe6b-sample-image.jpg" /&gt;&lt;/a&gt;</pre>
      </div>
   </div>
</div>

#####Résultat :

![Resultat](https://learn.getgrav.org/images/6/e/3/8/8/6e388bab69f2d1bc5c0d3cd80ef317e9d1a343ed-sample-image.jpg)

<h4 id="miniature">miniature
<a href="#miniature" class="toc-anchor after"></a></h4>

Choisissez manuellement la vignette que Grav doit utiliser. Vous pouvez choisir entre `page` et `défault` pour tout type de média ainsi que `média` pour le média image si vous souhaitez utiliser l'objet média lui-même comme vignette.

<div class="tabs">
   <div id="tab15"><a href="#tab15">Markdown</a>
      <div class="content">
<pre class="pre">![Sample Image](sample-image.jpg?thumbnail=default&display=thumbnail)</pre>
      </div>
   </div>
   <div id="tab16"><a href="#tab16">Twig</a>
      <div class="content">
<pre class="pre">{{ page.media['sample-image.jpg'].thumbnail('default').display('thumbnail')
.html('Sample Image')|raw }}</pre>
      </div>
   </div>
   <div id="tab17"><a href="#tab17">HTML Code</a>
      <div class="content">
<pre class="pre">&lt;img title="Sample Image" alt="" src="/system/images/media/thumb-jpg.png" /&gt;</pre>
      </div>
   </div>
</div>

#####Résultat :

![Résultat](https://learn.getgrav.org/system/images/media/thumb-jpg.png)

<h4 id="attribut">attribut
<a href="#attribut" class="toc-anchor after"></a></h4>

Cela ajoute un attribut HTML supplémentaire à la sortie.

<div class="tabs">
   <div id="tab18"><a href="#tab18">Markdown</a>
      <div class="content">
<pre class="pre">![Sample Image](sample-image.jpg?thumbnail=default&display=thumbnail)</pre>
      </div>
   </div>
   <div id="tab19"><a href="#tab19">Twig</a>
      <div class="content">
<pre class="pre">{{ page.media['sample-image.jpg'].thumbnail('default').display('thumbnail')
.html('Sample Image')|raw }}</pre>
      </div>
   </div>
   <div id="tab20"><a href="#tab20">HTML Code</a>
      <div class="content">
<pre class="pre">&lt;img title="Sample Image" alt="" src="/system/images/media/thumb-jpg.png" /&gt;</pre>
      </div>
   </div>
</div>

<h2 id="Actions sur les images">Actions sur les images
<a href="#Actions sur les images" class="toc-anchor after"></a></h2>

<h4 id="redimensionner">redimensionner
<a href="#redimensionner" class="toc-anchor after"></a></h4>

Le redimensionnement fait exactement ce que vous attendez de lui. `resize` vous permet de créer une nouvelle image basée sur `width` (la largeur) et `height` (la hauteur). Le rapport d'aspect est conservé et la nouvelle image contiendra des zones vides dans la couleur de la couleur d'arrière-plan **facultative**fournie sous forme de `hex value` (valeur hexadécimale), par ex. `0xffffff`. Le paramètre d'arrière-plan est facultatif et, s'il n'est pas fourni, il sera par défaut **transparent** si l'image est au format PNG ou **blanc** s'il s'agit d'un JPEG.

<div class="tabs">
   <div id="tab21"><a href="#tab21">Markdown</a>
      <div class="content">
<pre class="pre">![Sample Image](sample-image.jpg?resize=400,200)</pre>
      </div>
   </div>
   <div id="tab22"><a href="#tab22">Twig</a>
      <div class="content">
<pre class="pre">{{ page.media['sample-image.jpg'].resize(400, 200).html()|raw }}</pre>
      </div>
   </div>
</div>

#####Résultat :

![Résultat](https://learn.getgrav.org/images/f/8/2/e/8/f82e84dcb05f4e4abc0337d77f766863a4864658-sample-image.jpg)

<h4 id="forcer le redimensionnement">forcer le redimensionnement
<a href="#forcer le redimensionnement" class="toc-anchor after"></a></h4>

Redimensionne l'image à la largeur `widht` et à la hauteur `height` fournies. `forceResize` ne respectera pas le format d'image d'origine et étirera l'image selon les besoins pour s'adapter à la nouvelle taille de l'image.

<div class="tabs">
   <div id="tab23"><a href="#tab23">Markdown</a>
      <div class="content">
<pre class="pre">![Sample Image](sample-image.jpg?forceResize=200,300)</pre>
      </div>
   </div>
   <div id="tab24"><a href="#tab24">Twig</a>
      <div class="content">
<pre class="pre">{{ page.media['sample-image.jpg'].forceResize(200, 300).html()|raw }}</pre>
      </div>
   </div>
</div>

#####Résultat :

![Résultat](https://learn.getgrav.org/images/4/5/a/6/d/45a6d20510669bf09fc1f89b470ec8508851f1e3-sample-image.jpg)

<h4 id="cropResize">cropResize
<a href="#cropResize" class="toc-anchor after"></a></h4>

`cropResize` redimensionne une image à une taille plus petite ou plus grande en fonction de la largeur `widht` et de la hauteur `height`. Le rapport d'aspect est conservé et la nouvelle image sera redimensionnée pour s'adapter à la boîte englobante comme décrit par la largeur `widht` et la hauteur `height` fournies. En d'autres termes, toute zone d'arrière-plan que vous verriez dans un `resize` normal est recadrée.

Par exemple, si vous avez une image de `640` x `480` et que vous effectuez une action `cropResize(100, 100)` dessus, vous obtiendrez une image de `100` x `75`.

<div class="tabs">
   <div id="tab25"><a href="#tab25">Markdown</a>
      <div class="content">
<pre class="pre">![Sample Image](sample-image.jpg?cropResize=300,300)</pre>
      </div>
   </div>
   <div id="tab26"><a href="#tab26">Twig</a>
      <div class="content">
<pre class="pre">{{ page.media['sample-image.jpg'].cropResize(300, 300).html()|raw }}</pre>
      </div>
   </div>
</div>

#####Résultat :

![Résultat](https://learn.getgrav.org/images/c/6/f/2/a/c6f2a4797dd65154696266da9292f3280240de79-sample-image.jpg)

<h4 id="crop">crop
<a href="#crop" class="toc-anchor after"></a></h4>

`crop` ne redimensionnera pas du tout l'image, il recadrera simplement l'image d'origine de sorte que seule la partie de la boîte englobante telle que décrite par la largeur `widht` et la hauteur  `height` provenant de l'emplacement `x` et `y` soit utilisée pour créer la nouvelle image.

Par exemple, une image de `640` x `480` sur laquelle l'action de recadrage `cro (0, 0, 400, 100)` est appliquée obtiendra simplement la largeur et la hauteur recadrées de sorte que l'image résultante soit une image d'une largeur de `400` et une hauteur de `100` provenait du coin supérieur gauche comme décrit par `0, 0`.

<div class="tabs">
   <div id="tab27"><a href="#tab27">Markdown</a>
      <div class="content">
<pre class="pre">![Sample Image](sample-image.jpg?crop=100,100,300,200)</pre>
      </div>
   </div>
   <div id="tab28"><a href="#tab28">Twig</a>
      <div class="content">
<pre class="pre">{{ page.media['sample-image.jpg'].crop(100,100,300,200).html()|raw }}</pre>
      </div>
   </div>
</div>

#####Résultat :

![Résultat](https://learn.getgrav.org/images/f/1/8/9/a/f189a48ef158968a6e707cb65f6fa4167e3513ab-sample-image.jpg)

<h4 id="cropZoom">cropZoom
<a href="#cropZoom" class="toc-anchor after"></a></h4>

Semblable à `cropResize`, `cropZoom` prend également une largeur `width` et une hauteur `hright` mais **redimensionne et recadre** l'image pour s'assurer que l'image résultante a la taille exacte que vous avez demandée. Le rapport d'aspect est conservé mais certaines parties de l'image peuvent être recadrées, mais l'image résultante est centrée.

<div class="notice info">
La principale différence entre <strong>cropResize</strong> et <strong>cropZoom</strong> est que dans cropResize, l'image est redimensionnée en conservant les proportions de sorte que l'image entière soit affichée, et tout espace supplémentaire est considéré comme un arrière-plan.
</div>

La principale différence entre cropResize et cropZoom est que dans cropResize, l'image est redimensionnée en conservant les proportions de sorte que l'image entière soit affichée, et tout espace supplémentaire est considéré comme un arrière-plan.

Avec **cropZoom**, l'image est redimensionnée de sorte qu'il n'y ait pas d'arrière-plan visible, et la zone d'image supplémentaire de l'image en dehors de la nouvelle taille d'image est recadrée.

Par exemple, si vous avez une image de `640` x `480` et que vous effectuez une action `cropZoom(400, 100)`, l'image résultante sera redimensionnée à `400` x `300`, puis la hauteur est recadrée, ce qui donne une image de `400` x `100`.

<div class="tabs">
   <div id="tab29"><a href="#tab29">Markdown</a>
      <div class="content">
<pre class="pre">![Sample Image]\(sample-image.jpg?cropZoom=600,200)</pre>
      </div>
   </div>
   <div id="tab30"><a href="#tab30">Twig</a>
      <div class="content">
<pre class="pre">{{ page.media['sample-image.jpg'].cropZoom(600,200).html()\|raw }}</pre>
      </div>
   </div>
</div>

<div class="notice info">
Les personnes familiarisées avec l'utilisation de <code>zoomCrop</code> à cette fin constateront que cela fonctionne également dans Grav.
</div>

#####Résultat :

![Résultat](https://learn.getgrav.org/images/d/9/2/b/f/d92bfdaee70366a7390266cc163e6c1cbf085b6b-sample-image.jpg)

<h4 id="qualité">qualité
<a href="#qualité" class="toc-anchor after"></a></h4>

Permet dynamiquement de définir une valeur `value` de **pourcentage de compression** pour l'image entre `0` et `100`. Un nombre inférieur signifie une qualité moindre, où `100` signifie une qualité maximale.

<div class="tabs">
   <div id="tab31"><a href="#tab31">Markdown</a>
      <div class="content">
<pre class="pre">![Sample Image]\(sample-image.jpg?cropZoom=300,200&quality=25)</pre>
      </div>
   </div>
   <div id="tab32"><a href="#tab32">Twig</a>
      <div class="content">
<pre class="pre">{{ page.media['sample-image.jpg'].cropZoom(300,200).quality(25).html()|raw }}</pre>
      </div>
   </div>
</div>

#####Résultat :

![Résultat](https://learn.getgrav.org/images/4/d/3/7/a/4d37ada8680de55cf46d1cb9b78217463755f765-sample-image.jpg)

<h4 id="négatif">négatif
<a href="#négatif" class="toc-anchor after"></a></h4>

Applique un **filtre négatif** à l'image où les couleurs sont inversées.

<div class="tabs">
   <div id="tab33"><a href="#tab33">Markdown</a>
      <div class="content">
<pre class="pre">![Sample Image](sample-image.jpg?cropZoom=300,200&negate)</pre>
      </div>
   </div>
   <div id="tab34"><a href="#tab34">Twig</a>
      <div class="content">
<pre class="pre">{{ page.media['sample-image.jpg'].cropZoom(300,200).negate.html()|raw }}</pre>
      </div>
   </div>
</div>

#####Résultat :

![Résultat](https://learn.getgrav.org/images/3/4/f/8/0/34f801552d1959142ff0a8af90d1ef0b7ffca31b-sample-image.jpg)

<h4 id="luminosité">luminosité
<a href="#luminosité" class="toc-anchor after"></a></h4>

Applique un filtre de luminosité à l'image avec une valeur `value` comprise entre `-255` et `+255`. Des nombres négatifs plus grands rendront l'image plus sombre, tandis que des nombres positifs plus grands rendront l'image plus lumineuse.

<div class="tabs">
   <div id="tab35"><a href="#tab35">Markdown</a>
      <div class="content">
<pre class="pre">![Sample Image](sample-image.jpg?cropZoom=300,200&brightness=-100)</pre>
      </div>
   </div>
   <div id="tab36"><a href="#tab36">Twig</a>
      <div class="content">
<pre class="pre">{{ page.media['sample-image.jpg'].cropZoom(300,200).brightness(-100)
.html()|raw }}</pre>
      </div>
   </div>
</div>

#####Résultat :

![Résultat](https://learn.getgrav.org/images/7/4/2/3/b/7423b7e6ff834ef259c41a05134bf6dd954a943c-sample-image.jpg)

<h4 id="contraste">contraste
<a href="#contraste" class="toc-anchor after"></a></h4>

Ceci applique un **filtre de contraste** à l'image avec une valeur `value` de `-100` à `+100`. Des nombres négatifs plus grands augmenteront le contraste, tandis que des nombres positifs plus grands réduiront le contraste.

<div class="tabs">
   <div id="tab37"><a href="#tab37">Markdown</a>
      <div class="content">
<pre class="pre">![Sample Image](sample-image.jpg?cropZoom=300,200&contrast=-50)</pre>
      </div>
   </div>
   <div id="tab38"><a href="#tab38">Twig</a>
      <div class="content">
<pre class="pre">{{ page.media['sample-image.jpg'].cropZoom(300,200).contrast(-50).html()|raw }}</pre>
      </div>
   </div>
</div>

#####Résultat :

![Résultat](https://learn.getgrav.org/images/3/1/4/1/e/3141e4a436373d0d14b30c81f5fd67e420de4546-sample-image.jpg)

<h4 id="niveau de gris">niveau de gris
<a href="#niveau de gris" class="toc-anchor after"></a></h4>

Cela traite l'image avec un **filtre en niveaux de gris**.

<div class="tabs">
   <div id="tab39"><a href="#tab39">Markdown</a>
      <div class="content">
<pre class="pre">![Sample Image](sample-image.jpg?cropZoom=300,200&grayscale)</pre>
      </div>
   </div>
   <div id="tab40"><a href="#tab40">Twig</a>
      <div class="content">
<pre class="pre">{{ page.media['sample-image.jpg'].cropZoom(300,200).grayscale.html()|raw }}</pre>
      </div>
   </div>
</div>

#####Résultat :

![Résultat](https://learn.getgrav.org/images/9/b/8/d/f/9b8dfb148124c5623c59d1a63c006e2f7eb066ce-sample-image.jpg)

<h4 id="gaufrer">gaufrer
<a href="#gaufrer" class="toc-anchor after"></a></h4>

Cela traite l'image avec un **filtre de gaufrage**.

<div class="tabs">
   <div id="tab41"><a href="#tab41">Markdown</a>
      <div class="content">
<pre class="pre">![Sample Image](sample-image.jpg?cropZoom=300,200&emboss)</pre>
      </div>
   </div>
   <div id="tab42"><a href="#tab42">Twig</a>
      <div class="content">
<pre class="pre">{{ page.media['sample-image.jpg'].cropZoom(300,200).emboss.html()|raw }}</pre>
      </div>
   </div>
</div>

#####Résultat :

![Résultat](https://learn.getgrav.org/images/4/c/e/2/1/4ce21377a2ee4a7749535dc82bd3521f48e4e117-sample-image.jpg)

<h4 id="lisser">lisser
<a href="#lisser" class="toc-anchor after"></a></h4>

Cela applique un **filtre de lissage** à l'image en fonction du réglage de la valeur `value` de lissage de `-10` à `10`.

<div class="tabs">
   <div id="tab43"><a href="#tab43">Markdown</a>
      <div class="content">
<pre class="pre">![Sample Image](sample-image.jpg?cropZoom=300,200&smooth=5)</pre>
      </div>
   </div>
   <div id="tab44"><a href="#tab44">Twig</a>
      <div class="content">
<pre class="pre">{{ page.media['sample-image.jpg'].cropZoom(300,200).smooth(5).html()|raw }}</pre>
      </div>
   </div>
</div>

#####Résultat :

![Résultat](https://learn.getgrav.org/images/5/5/8/0/3/55803bc6242fe0f0eb7602c4376c3643880beca9-sample-image.jpg)

<h4 id="bordure">bordure
<a href="#bordure" class="toc-anchor after"></a></h4>

Cela applique un **filtre de bordure** sur l'image.

<div class="tabs">
   <div id="tab45"><a href="#tab45">Markdown</a>
      <div class="content">
<pre class="pre">![Sample Image](sample-image.jpg?cropZoom=300,200&sharp)</pre>
      </div>
   </div>
   <div id="tab46"><a href="#tab46">Twig</a>
      <div class="content">
<pre class="pre">{{ page.media['sample-image.jpg'].cropZoom(300,200).sharp.html()|raw }}</pre>
      </div>
   </div>
</div>

#####Résultat :

![Résultat](https://learn.getgrav.org/images/d/1/c/b/8/d1cb87797f40ca52e4c8010d4e95e2ed63e2f664-sample-image.jpg)

<h4 id="contours">contours
<a href="#contours" class="toc-anchor after"></a></h4>

Cela applique un **filtre de contours** sur l'image.

<div class="tabs">
   <div id="tab47"><a href="#tab47">Markdown</a>
      <div class="content">
<pre class="pre">![Sample Image](sample-image.jpg?cropZoom=300,200&edge)</pre>
      </div>
   </div>
   <div id="tab48"><a href="#tab48">Twig</a>
      <div class="content">
<pre class="pre">{{ page.media['sample-image.jpg'].cropZoom(300,200).edge.html()|raw }}</pre>
      </div>
   </div>
</div>

#####Résultat :

![Résultat](https://learn.getgrav.org/images/2/c/3/4/7/2c3473fc46d715d58d6d54978c896f63a6c737e6-sample-image.jpg)

<h4 id="coloriser">coloriser
<a href="#coloriser" class="toc-anchor after"></a></h4>

Vous pouvez coloriser l'image en ajustant les valeurs de rouge `red`, de vert `green` et de bleu `blue` de l'image de `-255` à `+255` pour chaque couleur.

<div class="tabs">
   <div id="tab49"><a href="#tab49">Markdown</a>
      <div class="content">
<pre class="pre">![Sample Image](sample-image.jpg?cropZoom=300,200&colorize=100,-100,40)</pre>
      </div>
   </div>
   <div id="tab50"><a href="#tab50">Twig</a>
      <div class="content">
<pre class="pre">{{ page.media['sample-image.jpg'].cropZoom(300,200).colorize(100,-100,40
html()|raw }}</pre>
      </div>
   </div>
</div>

#####Résultat :

![Résultat](https://learn.getgrav.org/images/8/6/5/b/9/865b9c011421966a2e11320c3b84b7c98139c87a-sample-image.jpg)

<h4 id="sépia">sépia
<a href="#sépia" class="toc-anchor after"></a></h4>

Cela applique un **filtre sépia** sur l'image pour produire un look vintage.

<div class="tabs">
   <div id="tab51"><a href="#tab51">Markdown</a>
      <div class="content">
<pre class="pre">![Sample Image](sample-image.jpg?cropZoom=300,200&sepia)</pre>
      </div>
   </div>
   <div id="tab52"><a href="#tab52">Twig</a>
      <div class="content">
<pre class="pre">{{ page.media['sample-image.jpg'].cropZoom(300,200).sepia.html()|raw }}</pre>
      </div>
   </div>
</div>

#####Résultat :

![Résultat](https://learn.getgrav.org/images/b/2/1/5/3/b2153a55ad179282856a085973af657bde607e95-sample-image.jpg)

<h4 id="flou gaussien">flou gaussien
<a href="#flou gaussien" class="toc-anchor after"></a></h4>

**Brouille** l'image en définissant la fréquence à laquelle le filtre de flou est appliqué à l'image. La valeur par défaut est 1 fois.

<div class="tabs">
   <div id="tab53"><a href="#tab53">Markdown</a>
      <div class="content">
<pre class="pre">![Sample Image](sample-image.jpg?gaussianBlur=3)</pre>
      </div>
   </div>
   <div id="tab54"><a href="#tab54">Twig</a>
      <div class="content">
<pre class="pre">{{ page.media['sample-image.jpg'].gaussianBlur(3).html()|raw }}</pre>
      </div>
   </div>
</div>

#####Résultat :

![Résultat](https://learn.getgrav.org/images/8/f/d/7/c/8fd7ce172594ef49d2307c1b405c68360d714380-sample-image.jpg)

<h4 id="tourner">tourner
<a href="#tourner" class="toc-anchor after"></a></h4>

**Fait pivoter** l'image d'un angle `angle` en degrés dans le sens inverse des aiguilles d'une montre, les valeurs négatives tournent dans le sens des aiguilles d'une montre.

<div class="tabs">
   <div id="tab55"><a href="#tab55">Markdown</a>
      <div class="content">
<pre class="pre">![Sample Image](sample-image.jpg?cropZoom=300,200&rotate=-90)</pre>
      </div>
   </div>
   <div id="tab56"><a href="#tab56">Twig</a>
      <div class="content">
<pre class="pre">{{ page.media['sample-image.jpg'].cropZoom(300,200).rotate(-90).html()|raw }}</pre>
      </div>
   </div>
</div>

#####Résultat :

![Résultat](https://learn.getgrav.org/images/5/0/f/7/f/50f7f806aedb26a4bdba9c9b4b8615b900cd0098-sample-image.jpg)

<h4 id="retourner">retourner
<a href="#retourner" class="toc-anchor after"></a></h4>

**Retourne** l'image dans les directions données. Les deux paramètres peuvent être `0|1`. Les deux `0` équivaut à aucun retournement dans les deux sens.

<div class="tabs">
   <div id="tab57"><a href="#tab57">Markdown</a>
      <div class="content">
<pre class="pre">![Sample Image](sample-image.jpg?cropZoom=300,200&flip=0,1)</pre>
      </div>
   </div>
   <div id="tab58"><a href="#tab58">Twig</a>
      <div class="content">
<pre class="pre">{{ page.media['sample-image.jpg'].cropZoom(300,200).flip(0,1).html()|raw }}</pre>
      </div>
   </div>
</div>

#####Résultat :

![Résultat](https://learn.getgrav.org/images/8/b/2/d/7/8b2d7799726efac9caceeb2e867a5b49c327eb63-sample-image.jpg)

<h4 id="fixOrientation">fixOrientation
<a href="#fixOrientation" class="toc-anchor after"></a></h4>

Corrige l'orientation de l'image lors de la rotation via les données EXIF ​​(s'applique aux images jpeg prises avec des téléphones et des appareils photo).

<div class="tabs">
   <div id="tab59"><a href="#tab59">Markdown</a>
      <div class="content">
<pre class="pre">![Sample Image](sample-image.jpg?fixOrientation)</pre>
      </div>
   </div>
   <div id="tab60"><a href="#tab60">Twig</a>
      <div class="content">
<pre class="pre">{{ page.media['sample-image.jpg'].fixOrientation().html()|raw }}</pre>
      </div>
   </div>
</div>

<h4 id="Chargement">Chargement
<a href="#Chargement" class="toc-anchor after"></a></h4>

L'attribution du chargement aux images permet aux auteurs de contrôler le moment où le navigateur doit commencer à charger la ressource. La valeur de l'attribut de chargement peut être `auto` (par défaut), `lazy`, `eager`. La valeur peut être définie dans `system.images.defaults.loading` comme valeur par défaut, ou par image md avec `?loading=lazy` Lorsque la valeur `auto` est choisie, aucun `loading` attribut de chargement n'est ajouté et le navigateur déterminera la stratégie à utiliser.

<div class="tabs">
   <div id="tab61"><a href="#tab61">Markdown</a>
      <div class="content">
<pre class="pre">![Sample Image](sample-image.jpg?loading=lazy)</pre>
      </div>
   </div>
   <div id="tab62"><a href="#tab62">Twig</a>
      <div class="content">
<pre class="pre">{# Using default value as defined in 'config.system.images.defaults.loading' #}
{{ page.media['sample-image.jpg'].loading.html('Sample Image')|raw }}

{# Using explicit value #}
{{ page.media['sample-image.jpg'].loading('lazy').html('Sample Image')|raw }}</pre>
      </div>
   </div>
   <div id="tab63"><a href="#tab63">HTML Code</a>
      <div class="content">
<pre class="pre">&lt;img loading="lazy" title="Sample Image"  src="/images/e/f/1/0/5/<br>ef10554cd3a99f2e65136e79dce170d4f8a7a1b9-sample-image.jpg" /&gt;</pre>
      </div>
   </div>
</div>

<h2 id="Actions animées">Actions animées / vectorisées
<a href="#Actions animées" class="toc-anchor after"></a></h2>

<h4 id="redimensionner2">redimensionner
<a href="#redimensionner2" class="toc-anchor after"></a></h4>

Étant donné que PHP ne peut pas gérer le redimensionnement dynamique de ces types de médias, l'action de redimensionnement s'assurera uniquement qu'un attribut `width` et `height` ou `data-width` et `data-height` est défini sur votre balise `<img>`/`<video>` ou `<a>` respectivement. Cela signifie que votre image ou vidéo sera affichée dans la taille demandée, mais l'image ou le fichier vidéo réel ne sera en aucun cas converti.

<div class="tabs">
   <div id="tab64"><a href="#tab64">Markdown</a>
      <div class="content">
<pre class="pre">![Sample Trailer](sample-trailer.mov?resize=400,200)</pre>
      </div>
   </div>
   <div id="tab65"><a href="#tab65">Twig</a>
      <div class="content">
<pre class="pre">{{ page.media['sample-trailer.mov'].resize(400, 200).html('Sample Trailer')|raw }}}</pre>
      </div>
   </div>
   <div id="tab66"><a href="#tab66">HTML Code</a>
      <div class="content">
<pre class="pre">&lt;video controls="1" style="width: 400px;height: 200px;" title="Sample Trailer"<br> alt=""&gt;&lt;source src="/user/pages/02.content/07.media/sample-trailer.mov?<br>loading=auto"&gt;Your browser does not support the video tag.&lt;video&gt;<pre>
      </div>
   </div>
</div>

<h4 id="exemples">exemples
<a href="#exemples" class="toc-anchor after"></a></h4>

Quelques exemples de ceci :

<div class="tabs">
   <div id="tab67"><a href="#tab67">Image Vectorielle</a>
      <div class="content2">
<pre class="pre">![Sample Vector](img/sample-vector.svg?resize=300,300)</pre>
<div class="img">
   <img = src="/img/sample-vector.svg" style="width: 300px; height: 300px;""/>
</div>
      </div>
   </div>
   <div id="tab68"><a href="#tab68">Image Animée</a>
      <div class="content2">
<pre class="pre">![Animated Gif](img/sample-animated.gif?resize=300,300)</pre>
<div class="img">
   <img = src="/img/sample-animated.gif" style="width: 300px; height: 300px;""/>
</div>
      </div>
   </div>
   <div id="tab69"><a href="#tab69">Vidéo</a>
      <div class="content2">
<pre class="pre">![Sample Trailer](sample-trailer.mov?resize=400,200)<pre>
<br>
<video controls="1" style="width: 400px;height: 200px;" title="Sample Trailer" alt=""><source src="/../video/sample-trailer.mov?loading=auto">Your browser does not support the video tag.</video>
      </div>
   </div>
</div>
<br><br><br><br><br><br><br><br><br><br><br><br>

<h2 id="Actions audio">Actions audio
<a href="#Actions audio" class="toc-anchor after"></a></h2>

Le média audio affichera un lien audio HTML5 :

<div class="tabs">
   <div id="tab70"><a href="#tab70">Markdown</a>
      <div class="content">
<pre class="pre">![Hal 9000: I'm Sorry Dave](hal9000.mp3)</pre>
      </div>
   </div>
   <div id="tab71"><a href="#tab71">Twig</a>
      <div class="content">
<pre class="pre">{{ page.media['hal9000.mp3'].html()|raw }}</pre>
      </div>
   </div>
</div>

#####Résultat :
<audio controls="1" alt="Hal 9000: I'm Sorry Dave"><source src="/../video//hal9000.mp3?loading=auto">Your browser does not support the audio tag.</audio>

<h4 id="controls">controls
<a href="#controls" class="toc-anchor after"></a></h4>

Permet de définir ou de supprimer explicitement les contrôles HTML5 par défaut. Passer `0` masque les commandes du navigateur pour la lecture, le volume, etc.

<div class="tabs">
   <div id="tab72"><a href="#tab72">Markdown</a>
      <div class="content">
<pre class="pre">![Hal 9000: I'm Sorry Dave](hal9000.mp3?controls=0)</pre>
      </div>
   </div>
   <div id="tab73"><a href="#tab73">Twig</a>
      <div class="content">
<pre class="pre">{{ page.media['hal9000.mp3'].controls(0)|raw }}</pre>
      </div>
   </div>
   <div id="tab74"><a href="#tab74">HTML Code</a>
      <div class="content">
<pre class="pre">&lt;audio alt=""&gt;&lt;source src="/user/pages/02.content/07.media/hal9000.mp3?<br>loading=auto"&gt;Your browser does not support the audio tag.&lt;/audio&gt;<pre>
      </div>
   </div>
</div>

<h4 id="préchargement">préchargement
<a href="#préchargement" class="toc-anchor after"></a></h4>

Permet de définir la propriété de préchargement `preload`, qui est par défaut sur `auto`. Les paramètres autorisés sont `auto`, `metadata` et `none`.

<div class="notice info">
Si elle n'est pas définie, sa valeur par défaut est fixée par le navigateur (c'est-à-dire que chaque navigateur peut avoir sa propre valeur par défaut). La spécification conseille de la définir sur les métadonnées <code>metadata</code>.
</div>

<div class="notice info">
L'attribut <code>preload</code> est ignoré si la lecture <code>autoplay</code> est présente.
</div>

<div class="tabs">
   <div id="tab75"><a href="#tab75">Markdown</a>
      <div class="content">
<pre class="pre">![Hal 9000: I'm Sorry Dave](hal9000.mp3?preload=metadata)</pre>
      </div>
   </div>
   <div id="tab76"><a href="#tab76">Twig</a>
      <div class="content">
<pre class="pre">{{ page.media['hal9000.mp3'].preload('metadata')|raw }}</pre>
      </div>
   </div>
</div>

<h4 id="lecture automatique">lecture automatique
<a href="#lecture automatique" class="toc-anchor after"></a></h4>

Permet de définir si l'audio sera lu `autoplay` lors du chargement de la page. La valeur par défaut est `false` par omission si elle n'est pas définie.

<div class="notice info">
Si <code>autoplay</code>et <code>preload</code> sont tous les deux présents sur un élément <code>audio</code> donné, <code>preload</code> sera ignoré.
</div>

<div class="tabs">
   <div id="tab77"><a href="#tab77">Markdown</a>
      <div class="content">
<pre class="pre">![Hal 9000: I'm Sorry Dave](hal9000.mp3?autoplay=1)</pre>
      </div>
   </div>
   <div id="tab78"><a href="#tab78">Twig</a>
      <div class="content">
<pre class="pre">{{ page.media['hal9000.mp3'].autoplay(1)|raw }}</pre>
      </div>
   </div>
</div>

<h4 id="controle de liste">contrôle de liste
<a href="#controle de liste" class="toc-anchor after"></a></h4>

Permet de définir la propriété `controlsList`, qui prend une ou plusieurs des trois valeurs possibles : `nodownload`, `nofullscreen` et `noremoteplayback`.

<div class="notice info">
Si vous définissez plus d'un paramètre dans Markdown, séparez-les par un tiret (<code>-</code>). Ceux-ci seront remplacés par des espaces dans le HTML de sortie.
</div>

<div class="tabs">
   <div id="tab79"><a href="#tab79">Markdown</a>
      <div class="content">
<pre class="pre">![Hal 9000: I'm Sorry Dave](hal9000.mp3?controlsList=<br>nodownload-nofullscreen-noremoteplayback)</pre>
      </div>
   </div>
   <div id="tab80"><a href="#tab80">Twig</a>
      <div class="content">
<pre class="pre">{{ page.media['hal9000.mp3'].controlsList('nodownload nofullscreen 
noremoteplayback')|raw }}</pre>
      </div>
   </div>
</div>

<h4 id="sourdine">sourdine
<a href="#sourdine" class="toc-anchor after"></a></h4>

Permet de définir si le son est coupé `muted` lors du chargement. La valeur par défaut est `false` par omission si elle n'est pas définie.

<div class="tabs">
   <div id="tab81"><a href="#tab81">Markdown</a>
      <div class="content">
<pre class="pre">![Hal 9000: I'm Sorry Dave](hal9000.mp3?muted=1)</pre>
      </div>
   </div>
   <div id="tab82"><a href="#tab82">Twig</a>
      <div class="content">
<pre class="pre">{{ page.media['hal9000.mp3'].muted(1)|raw }}</pre>
      </div>
   </div>
</div>

<h4 id="boucle">boucle
<a href="#boucle" class="toc-anchor after"></a></h4>

Permet de définir si l'audio sera en boucle `loop` lors de la lecture jusqu'à la fin. La valeur par défaut est `false` par omission si elle n'est pas définie.

<div class="tabs">
   <div id="tab83"><a href="#tab83">Markdown</a>
      <div class="content">
<pre class="pre">![Hal 9000: I'm Sorry Dave](hal9000.mp3?loop=1)</pre>
      </div>
   </div>
   <div id="tab84"><a href="#tab84">Twig</a>
      <div class="content">
<pre class="pre">{{ page.media['hal9000.mp3'].loop(1)|raw }}</pre>
      </div>
   </div>
</div>

<h2 id="Actions sur fichiers">Actions sur fichiers
<a href="#Actions sur fichiers" class="toc-anchor after"></a></h2>

Grav ne fournit aucune action personnalisée sur les fichiers pour le moment et il n'est pas prévu d'en ajouter. Si vous pensez à quelque chose, veuillez nous contacter.

<div class="tabs">
   <div id="tab85"><a href="#tab85">Markdown</a>
      <div class="content">
<pre class="pre">[View Text File](acronyms.txt)</pre>
      </div>
   </div>
   <div id="tab86"><a href="#tab86">Twig</a>
      <div class="content">
<pre class="pre">&lt;a href="{{ page.media['acronyms.txt'].url()|raw }}"&gt;<br>View Text File&lt;/a&gt;</pre>
      </div>
   </div>
</div>

######Résultat:

[Afficher le fichier texte](https://learn.getgrav.org/17/content/media/acronyms.txt)

<h4 id="Combinaisons">Combinaisons
<a href="#Combinaisons" class="toc-anchor after"></a></h4>

Comme vous pouvez le voir : Grav fournit de puissantes fonctionnalités de manipulation d'images qui facilitent vraiment le travail avec elles ! Cette véritable puissance est visible lorsque vous combinez plusieurs effets et produisez des manipulations d'images dynamiques très sophistiquées. Par exemple, ceci est tout à fait valable :

<div class="tabs">
   <div id="tab87"><a href="#tab87">Markdown</a>
      <div class="content">
<pre class="pre">![Sample Image](sampleimage.jpgnegate&lightbox&cropZoom=200,200)</pre>
      </div>
   </div>
   <div id="tab88"><a href="#tab88">Twig</a>
      <div class="content">
<pre class="pre">{{ page.media['sample-image.jpg']<br>.negate.lightbox.cropZoom(200,200)|raw }}</pre>
      </div>
   </div>
</div>

######Résultat:

![Résultat](https://learn.getgrav.org/images/e/2/d/6/c/e2d6c3b7a09ac442d942a3628dfef6b48977093a-sample-image.jpg)

<h4 id="Réinitialiser plusieurs appels à la même image">Réinitialiser plusieurs appels à la même image
<a href="#Réinitialiser plusieurs appels à la même image" class="toc-anchor after"></a></h4>

Lorsque vous accédez plusieurs fois à la même image dans une même page, les actions que vous avez fournies à l'image ne sont pas réinitialisées par défaut. Ainsi, si vous redimensionnez une image et affichez le code HTML, puis plus tard dans la même page, affichez simplement l'URL de l'image, vous obtiendrez également l'URL de l'image redimensionnée. Vous vous attendiez probablement à l'URL de l'image d'origine.

Pour lutter contre cela, vous pouvez réinitialiser les actions sur les images en passant `false` à la méthode `url()` :

    {% for item in page.header.gallery %}
        {% set image = page.media[item.src].cropZoom(800, 600).quality(70) %}
        <a href="{{ image.url(false)|e }}">
          <img src="{{ image.url|e }}" alt="{{ item.alt|e }}" title="{{ item.title|e }}" />
        </a>
    {% endfor %}

<h3 id="Images réactives">Images réactives
<a href="#Images réactives" class="toc-anchor after"></a></h3>

<h4 id="Écrans à haute densité">Écrans à haute densité
<a href="#Écrans à haute densité" class="toc-anchor after"></a></h4>

Grav a un support intégré pour les images réactives pour les écrans à haute densité (par exemple, les écrans **Retina**). Grav accomplit cela en implémentant `srcset` à partir de la [proposition HTML de l'élément Picture](https://html.spec.whatwg.org/multipage/embedded-content.html#the-picture-element). Un bon article à lire si vous voulez mieux comprendre cela est [ce billet de blog d'Eric Portis](http://ericportis.com/posts/2014/srcset-sizes/).

<div class="notice info">
Grav définit l'argument <code>sizes</code> tailles mentionné dans les messages ci-dessus sur la largeur totale de la fenêtre par défaut. Utilisez l'action <code>sizes</code> tailles présentée ci-dessous pour choisir vous-même.
</div>

Pour commencer à utiliser des images réactives, il vous suffit d'ajouter des images de densité supérieure à vos pages en ajoutant un suffixe au nom du fichier. Si vous ne fournissez que des images de densité plus élevée, Grav générera automatiquement des versions de qualité inférieure pour vous. Le nommage fonctionne comme suit : `[image-name]@[density-ratio]x.[image-extension]`, donc par exemple en ajoutant `sample-image@3x.jpg` à votre page, Grav créera une version `2x `et un `1x` (taille normale) par défaut.

<div class="notice note">
Ces fichiers générés par Grav seront stockés dans <code>images/</code> du dossier cache, pas dans votre dossier page.
</div>

Supposons que vous ayez un fichier appelé `retina@2x.jpg`, vous feriez en fait référence à cela dans vos liens comme `retina.jpg`, puis Grav ne trouvera pas cette image et commencera à rechercher les tailles d'image retrina. Il trouvera `retina@2x.jpg` puis réalisera qu'il doit créer une variante `@1x` et afficher la sortie `srcset` appropriée :

<div class="tabs">
   <div id="tab89"><a href="#tab89">Markdown</a>
      <div class="content">
<pre class="pre">![Retina Image](retina.jpg?sizes=80vw)</pre>
      </div>
   </div>
   <div id="tab90"><a href="#tab90">Twig</a>
      <div class="content">
<pre class="pre">{{ page.media['retina.jpg'].sizes('80vw').html()|raw }}</pre>
      </div>
   </div>
   <div id="tab91"><a href="#tab91">HTML Code</a>
      <div class="content">
<pre class="pre">&lt;img alt="Retina Image" src="/images/3/7/f/0/c/37f0ca845b3eb054374d6a1ac2e36e13c59e14f8<br>-retina.jpg" srcset="/images/b/a/c/1/9/bac199ed46f9188dafad759760afd27da935e564<br>-retina2x.jpg 2880w, /images/3/7/f/0/c/37f0ca845b3eb054374d6a1ac2e36e13c59e14f8<br>-retina.jpg 1440w" sizes="80vw"&gt;
</pre>
      </div>
   </div>
</div>

######Résultat :

![Image rétina](https://learn.getgrav.org/images/3/7/f/0/c/37f0ca845b3eb054374d6a1ac2e36e13c59e14f8-retina.jpg)

<div class="notice warning">
En fonction de votre affichage et de l'implémentation de votre navigateur et de la prise en charge de <code>srcset</code>, vous ne verrez peut-être jamais de différence. Nous avons inclus le balisage HTML dans le troisième onglet afin que vous puissiez voir ce qui se passe derrière les écrans.
</div>

<h4 id="Tailles avec media queries">Tailles avec media queries
<a href="#Tailles avec media queries" class="toc-anchor after"></a></h4>

Grav prend également en charge les requêtes multimédias dans l'attribut `sizes` tailles, vous permettant d'utiliser différentes largeurs en fonction de la taille de l'écran de l'appareil. Contrairement à la première méthode, vous n'avez pas besoin de créer plusieurs images ; elles seront créées automatiquement. L'image de secours est l'image actuelle, donc un navigateur sans prise en charge de `srcset` affichera l'image d'origine.

<div class="tabs">
   <div id="tab92"><a href="#tab92">Markdown</a>
      <div class="content">
<pre class="pre">![Retina Image](retina.jpg?sizes=%28max-width%3A26em%29+100vw%2C+50vw)</pre>
      </div>
   </div>
   <div id="tab93"><a href="#tab93">Twig</a>
      <div class="content">
<pre class="pre">{{ page.media['retina.jpg'].sizes('(max-width:26em) 100vw, 50vw').html('Retina Image')|raw }}</pre>
      </div>
   </div>
   <div id="tab94"><a href="#tab94">HTML Code</a>
      <div class="content">
<pre class="pre">&lt;img alt="Retina Image" src="/images/3/7/f/0/c/37f0ca845b3eb054374d6a1ac2e36e13c59e14f8<br>-retina.jpg" srcset="/images/b/a/c/1/9/bac199ed46f9188dafad759760afd27da935e564<br>-retina2x.jpg 2880w, /images/3/7/f/0/c/37f0ca845b3eb054374d6a1ac2e36e13c59e14f8<br>-retina.jpg 1440w" sizes="(max-width:26em)+100vw"&gt;
</pre>
      </div>
   </div>
</div>

######Résultat :

![Image Retina](https://learn.getgrav.org/images/3/7/f/0/c/37f0ca845b3eb054374d6a1ac2e36e13c59e14f8-retina.jpg)

<div class="notice warning">
En fonction de votre affichage et de l'implémentation de votre navigateur et de la prise en charge de <code>wsrcset</code>, vous ne verrez peut-être jamais de différence. Nous avons inclus le balisage HTML dans le quatrième onglet afin que vous puissiez voir ce qui se passe derrière les écrans.
Tailles avec media queries utilisant des dérivées
</div>

<h4 id="Tailles avec media queries utilisant des dérivées">Tailles avec media queries utilisant des dérivées
<a href="#Tailles avec media queries utilisant des dérivées" class="toc-anchor after"></a></h4>

Si vous souhaitez personnaliser les tailles des fichiers créés automatiquement, vous pouvez utiliser la méthode `derivatives()` (comme indiqué ci-dessous). Le premier paramètre est la largeur de la plus petite des images générées. La seconde est la largeur maximale des images générées. Le troisième, et seul paramètre facultatif, dicte les intervalles avec lesquels générer les photos (la valeur par défaut est 200). Par exemple, si vous définissez le premier paramètre sur `320` et le troisième sur `100`, Grav générera une image pour 320, 420, 520, 620, et ainsi de suite jusqu'à ce qu'il atteigne son maximum défini.

Dans notre exemple, nous avons défini le maximum sur `1600`. Cela se traduira par des incréments de 300 de `320` à `1520`, car `1620` serait au-dessus du seuil.

<div class="notice note">
Pour le moment, cela ne fonctionne pas à l'intérieur de Markdown, uniquement dans vos fichiers <code>twig</code>.
</div>

<div class="tabs">
   <div id="tab95"><a href="#tab95">Markdown</a>
      <div class="content">
<pre class="pre">![Retina Image](retina.jpg?derivatives=320,1600,300<br>&sizes=%28max-width%3A26em%29+100vw%2C+50vw)</pre>
      </div>
   </div>
   <div id="tab96"><a href="#tab96">Twig</a>
      <div class="content">
<pre class="pre">{{ page.media['retina.jpg'].derivatives(320,1600,300)<br>.sizes('(max-width:26em) 100vw, 50vw').html()|raw }}</pre>
      </div>
   </div>
   <div id="tab97"><a href="#tab97">HTML Code</a>
      <div class="content">
<pre class="pre">&lt;img alt="Retina Image" src="/images/3/7/f/0/c/37f0ca845b3eb054374d6a1ac2e36e13c59e14f8-retina.jpg" srcset="/images/b/a/c/1/9/bac199ed46f9188dafad759760afd27da935e564-retina2x.jpg 2880w, /images/d/c/e/f/7/dcef77ec0cc8efd0a66851a7750b530d8bfe093a-retina320w.jpg 320w, /images/1/5/d/a/1/15da17d9989ac18738474b1fab5ff6104b96be41-retina620w.jpg 620w,<br> /images/e/c/e/3/f/ece3fa30474b851808a485197b481d202d8f3811-retina920w.jpg920w, /images/3/8/8/2/4/3882463d358fc22a189380da7b7d14db2a5b260a-retina1220w.jpg 1220w, /images/6/0/5/0/e/6050e7409d6040b3737b6562cdec89854cac3f9a-retina1520w.jpg 1520w, /images/3/7/f/0/c/37f0ca845b3eb054374d6a1ac2e36e13c59e14f8-retina.jpg 1440w" sizes="(max-width:26em)+100vw"&gt;
</pre>
      </div>
   </div>
</div>

######Résultat :

![Image Retina](https://learn.getgrav.org/images/3/8/8/2/4/3882463d358fc22a189380da7b7d14db2a5b260a-retina1220w.jpg)

<div class="notice warning">
En fonction de votre affichage et de l'implémentation de votre navigateur et de la prise en charge de <code>srcset</code>, vous ne verrez peut-être jamais de différence. Nous avons inclus le balisage HTML dans le quatrième onglet afin que vous puissiez voir ce qui se passe derrière les écrans.
</div>

<h4 id="Définition manuelle de la taille">Définition manuelle de la taille
<a href="#Définition manuelle de la taille" class="toc-anchor after"></a></h4>

Au lieu de laisser Grav générer les tailles par étapes régulières entre des limites données, vous pouvez définir manuellement les tailles que Grav doit générer :

    ![Image rétine](retina.jpg?derivatives=[360,720,1200])

Cela générera des versions réduites de l'image `retina.jpg` en trois largeurs : 360, 720 et 1200px.

<h2 id="Métafichiers">Métafichiers
<a href="#Métafichiers" class="toc-anchor after"></a></h2>

Chaque média que vous référencez dans Grav, par ex. `image1.jpg`, `sample-trailer.mov` ou même `archive.zip` a la possibilité d'avoir des variables définies ou même remplacées via un **métafichier**. Ces fichiers prennent le format `<filename>.meta.yaml`. Par exemple, pour une image avec le nom de fichier `image1.jpg`, vous pouvez créer un métafichier appelé `image1.jpg.meta.yaml`.

Vous pouvez ajouter à peu près n'importe quel paramètre ou élément de métadonnées en utilisant cette méthode.

Le contenu de ce fichier doit être en syntaxe YAML, un exemple pourrait être :

```yaml
image:
   filters:
      default:
         - [cropResize, 300, 300]
         - sharp
alt_text: My Alt Text
```

Si vous utilisez cette méthode pour ajouter un style spécifique à un fichier ou des balises META pour un seul fichier, vous souhaiterez placer le fichier YAML dans le même dossier que le fichier référencé. Cela garantira que le fichier est extrait avec les données YAML. C'est un moyen pratique de définir même des métadonnées spécifiques à un fichier, car vous ne pouvez pas le faire à partir de la page elle-même.

Supposons que vous souhaitiez simplement extraire la valeur `alt_text` répertoriée pour le fichier image `sample-image.jpg`. Vous créerez ensuite un fichier appelé `sample-image.jpg.meta.yaml` et le placerez dans le dossier avec le fichier image référencé. Ensuite, insérez les données utilisées dans l'exemple ci-dessus et enregistrez ce fichier YAML. Dans le fichier Markdown de la page, vous pouvez afficher ces données en utilisant la ligne suivante :

    {{ page.media['sample-image.jpg'].meta.alt_text|e }}

Cela affichera l'exemple de phrase `My Alt Text` au lieu de l'image. Ceci n'est qu'un exemple de base. Vous pouvez utiliser cette méthode pour un certain nombre de choses, y compris la création d'une galerie avec plusieurs points de données uniques que vous souhaitez référencer pour chaque image. Vos images, par essence, ont un ensemble 
de données uniques qui peuvent être facilement référencées et extraites selon les besoins.

<h2 id="Options vidéo">Options vidéo
<a href="#Options vidéo" class="toc-anchor after"></a></h2>

Les options de contrôle vidéo en ligne sont une autre fonctionnalité intégrée à Grav. Ces options, ajoutées en ligne avec le nom du fichier, vous permettent de déterminer les paramètres de lecture ` autoplay` automatique, de contrôle `controls`et de boucle `loop` d'une vidéo intégrée.

Voici un exemple:

    ![video.mov]\(video.mov?loop=1&controls=0&autoplay=1&muet)

Les options sont les suivantes :

| Attribut        | Explications
| :------         | :------
| autoplay        | Active (`1`) ou désactive (`0`) la lecture automatique<br> de la vidéo lors du chargement de la page. 
| controls        | Active (`1`) ou désactive (`0`)  les contrôles multimédias<br> pour la vidéo intégrée.
| loop            | Active (`1`) ou désactive (`0`) la boucle automatique pour la<br> vidéo, en la rejouant à la fin.
| muted           | Coupe le son de la vidéo et autorise généralement sa lecture<br> automatique.


