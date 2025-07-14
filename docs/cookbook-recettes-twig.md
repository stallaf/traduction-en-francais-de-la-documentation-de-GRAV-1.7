<h1 class="rem">Recettes Twig</h1>

Cette page contient un assortiment de problèmes et leurs solutions respectives liées à la création de modèles Twig.

<h2 id="Lister les 5 derniers articles de blog récents">Lister les 5 derniers articles de blog récents.
<a href="#Lister les 5 derniers articles de blog récents" class="toc-anchor after"></a></h2>

<h3 id="Problème-1 :">Problème :
<a href="#Problème-1 :" class="toc-anchor after"></a></h3>

Vous souhaitez afficher les 5 derniers articles de blog dans une barre latérale de votre site afin qu'un lecteur puisse voir l'activité récente du blog.

<h3 id="La solution-1 :">La solution :
<a href="#La solution-1 :" class="toc-anchor after"></a></h3>

Trouvez simplement la page `/blog`, obtenez ses enfants, classez-les par date dans un ordre décroissant, puis obtenez les 5 premiers à afficher dans une liste :

```html
1 | <ul>
2 | {% for post in page.find('/blog').children.published.order('date', 'desc').slice(0, 5) %}
3 |    li class="recent-posts">
4 |        <strong><a href="{{ post.url }}">{{ post.title }}</a></strong>
5 |    </li>
6 | {% endfor %}
7 | </ul>
```

lors de l'utilisation dans les pages, assurez-vous d'ajouter la configuration suivante à l'en-tête de la page :

```yaml
1 | twig_first: true
2 | process:
3 |    twig: true
```

<h2 id="Ajouter des liens de navigation non modulaires">Ajouter des liens de navigation non modulaires.
<a href="#Ajouter des liens de navigation non modulaires" class="toc-anchor after"></a></h2>

<h3 id="Problème-2 :">Problème :
<a href="#Problème-2 :" class="toc-anchor after"></a></h3>

Vous souhaitez afficher des liens de navigation à partir de pages non modulaires.

<h3 id="La solution-2 :">La solution :
<a href="#La solution-2 :" class="toc-anchor after"></a></h3>

```html
1  | <div class="desktop-nav__navigation">
2  |     {% for page in pages.children %}
3  |         {% if page.visible %}
4  |             {% set current_page = (page.active or page.activeChild) ? 'active' : '' %}
5  |             <a class="desktop-nav__nav-link {{ current_page }}" href="{{ page.url }}">
6  |                 {{ page.menu }}
7  |             </a>
8  |         {% endif %}
9  |     {% endfor %}
10 | </div>
```

<h2 id="Lister les articles de blog de l'année">Lister les articles de blog de l'année.
<a href="#Lister les articles de blog de l'année" class="toc-anchor after"></a></h2>

<h3 id="Problème-3 :">Problème :
<a href="#Problème-3 :" class="toc-anchor after"></a></h3>

Vous souhaitez afficher tous les articles de blog publiés au cours de cette année civile.

<h3 id="La solution-3 :">La solution :
<a href="#La solution-3 :" class="toc-anchor after"></a></h3>

Trouvez simplement la page `/blog`, obtenez ses enfants, filtrez par `dateRange()` appropriée et classez-les par date dans un ordre décroissant :

```html
1 | <ul>
2 | {% set this_year = "now"|date('Y') %}
3 | {% for post in page.find('/blog').children.dateRange('01/01/' ~ this_year, '12/31/' ~ 4 | this_year).order('date', 'desc') %}
5 | <li class="recent-posts">
6 |         <strong><a href="{{ post.url }}">{{ post.title }}</a></strong>
7 |     </li>
8 | {% endfor %}
9 | </ul>
```

<h2 id="Afficher un mois traduit">Afficher un mois traduit.
<a href="#Afficher un mois traduit" class="toc-anchor after"></a></h2>

<h3 id="Problème-4 :">Problème :
<a href="#Problème-4 :" class="toc-anchor after"></a></h3>

Dans certains templates de page, le filtre de `date` Twig est utilisé, et il ne gère pas les locales/multilingue. Ainsi, même si votre page est dans une langue différente de l'anglais, elle pourrait afficher le mois en anglais, si le modèle choisit d'afficher le nom du mois.

<h3 id="La solution-4 :">La solution :
<a href="#La solution-4 :" class="toc-anchor after"></a></h3>

Il y a deux solutions pour ce problème.

<h4 id="Première approche">Première approche
<a href="#Première approche" class="toc-anchor after"></a></h4>

La première implique l'utilisation de l'extension Twig intl.

Installez https://github.com/Perlkonig/grav-plugin-twig-extensions. Assurez-vous que l'extension PHP intl est installée.

Dans votre modèle twig, au lieu de par exemple (comme dans le thème Antimatière) `{{ page.date|date("M") }}` à `{{ page.date|localizeddate('long', 'none', 'it' , 'Europe/Rome', 'MMM') }}` (ajoutez votre langue et votre fuseau horaire ici)

<h4 id="Deuxième approche">Deuxième approche
<a href="#Deuxième approche" class="toc-anchor after"></a></h4>

Supposons que vous ayez une configuration de traductions de langues dans votre dossier `user/languages/` appelé `en.yaml` qui contient l'entrée :

`MONTHS_OF_THE_YEAR:` [January, February, March, April, May, June, July, August, September, October, November, December]

Et en `fr.yaml` :

`MONTHS_OF_THE_YEAR :` [Janvier, Février, Mars, Avril, Mai, Juin, Juillet, Août, Septembre, Octobre, Novembre, Décembre]

Ensuite, vous avez votre twig :

```html
1 | <li>
2 |    <a href='{{ post.url }}'><aside class="dates">{{ 'GRAV.MONTHS_OF_THE_YEAR'|ta(post.date|3 date('n') - 1) }} {{ post.date|date('d') }}</aside></a>
3 |    <a href='{{ post.url }}'>{{ post.title }}</a>
4 |</li>
```

Cela utilise le filtre Twig personnalisé Grav `|ta` qui signifie **Translate Array**. Dans la version anglaise, la sortie pourrait ressembler à :

    An Example Post  July 2015

Et en Français:

    Un exemple d'article Juillet 2015

<h2 id="Afficher le contenu de la page sans résumé">Afficher le contenu de la page sans résumé.
<a href="#Afficher le contenu de la page sans résumé" class="toc-anchor after"></a></h2>

<h3 id="Problème-5 :">Problème :
<a href="#Problème-5 :" class="toc-anchor after"></a></h3>

Vous souhaitez afficher le contenu d'une page sans le résumé en haut.

<h3 id="La solution-5 :">La solution :
<a href="#La solution-5 :" class="toc-anchor after"></a></h3>

Utilisez le filtre `slice` pour supprimer le résumé du contenu de la page :

    1 | {% set content = page.content|slice(page.summary|length) %}
    2 | {{ contenu|brut }}

<h2 id="Masquer l'e-mail aux robots de spam">Masquer l'e-mail aux robots de spam.
<a href="#Masquer l'e-mail aux robots de spam" class="toc-anchor after"></a></h2>

<h3 id="Problème-6 :">Problème :
<a href="#Problème-6 :" class="toc-anchor after"></a></h3>

Vous souhaitez masquer l'e-mail des robots de spam

<h3 id="La solution-6 :">La solution :
<a href="#La solution-6 :" class="toc-anchor after"></a></h3>

Activez le traitement Twig dans l'en-tête de la page :

```yaml
1 | process:
2 |    twig: true
```

Utilisez ensuite le filtre Twig `safe_email` :

```html
1 | <a href="mailto :{{'votre.email@server.com'|safe_email}}">
2 |    Envoyez moi un email
3 | </a>
```

<h2 id="Choisir un élément au hasard dans un tableau traduit">Choisir un élément au hasard dans un tableau traduit.
<a href="#Choisir un élément au hasard dans un tableau traduit" class="toc-anchor after"></a></h2>

<h3 id="Problème-7 :">Problème :
<a href="#Problème-7 :" class="toc-anchor after"></a></h3>

Vous voulez choisir un élément au hasard dans un tableau traduit dans une langue particulière. Pour que cela fonctionne, il est supposé que votre [site multilingue est réglé et configuré](/multi-langue) comme indiqué dans la documentation.

<h3 id="La solution-7 :">La solution :
<a href="#La solution-7 :" class="toc-anchor after"></a></h3>

Supposons également que vous ayez une configuration de traduction de langue dans votre dossier `user/languages/` appelé `en.yaml` qui contient l'entrée :

    FRUITS: [Banana, Cherry, Lemon, Lime, Strawberry, Raspberry]

Et en `fr.yaml` :

    FRUITS : [Banane, Cerise, Citron, Citron Vert, Fraise, Framboise]

Ensuite, vous avez votre Twig :

```html
1 | {% set langobj = grav['langue'] %}
2 | {% set curlang = langobj.getLanguage() %}
3 | {% set fruits = langobj.getTranslation(curlang,'FRUITS',true) %}
4 | <span data-ticker="{{ fruits|join(',') }}">{{ random(fruits) }}</span>
```

<h2 id="Afficher une image téléchargée dans un champ de fichier">Afficher une image téléchargée dans un champ de fichier.
<a href="#Afficher une image téléchargée dans un champ de fichier" class="toc-anchor after"></a></h2>

<h3 id="Problème-8 :">Problème :
<a href="#Problème-8 :" class="toc-anchor after"></a></h3>

Vous avez ajouté un champ `file` dans votre plan personnalisé et vous souhaitez afficher une image ajoutée dans ce champ.

<h3 id="La solution-8 :">La solution :
<a href="#La solution-8 :" class="toc-anchor after"></a></h3>

Comme le champ `file` permet de télécharger plusieurs images, il génère deux objets imbriqués dans votre frontmatter, le premier objet est la liste des images téléchargées, l'objet imbriqué à l'intérieur est un groupe de propriété/valeur pour l'image donnée.

*Notez que dans le cas où vous voudriez que votre utilisateur ne sélectionne qu'une seule image, il pourrait être plus facile d'utiliser le champ `filepicker`, qui stocke un seul objet avec les propriétés des images sélectionnées.*

Si vous avez une seule image, vous pouvez l'afficher dans votre modèle en utilisant :

    {{ page.media[header.yourfilefield|first.name] }}

Si vous avez autorisé votre utilisateur à télécharger plusieurs images, votre brindille pourrait ressembler à ceci :

```html
{% for imagesuploaded in page.header.yourfilefield %}
{{ page.media[imagesuploaded.name] }}
{% endfor %}
```
<h2 id="Afficher une image sélectionnée dans un champ mediapicker">Afficher une image sélectionnée dans un champ mediapicker.
<a href="#Afficher une image sélectionnée dans un champ mediapicker" class="toc-anchor after"></a></h2>

<h3 id="Problème-9 :">Problème :
<a href="#Problème-9 :" class="toc-anchor after"></a></h3>

Vous avez ajouté un champ `mediapicker` dans votre blueprint personnalisé, et vous souhaitez afficher l'image sélectionnée.

<h3 id="La solution-9 :">La solution :
<a href="#La solution-9 :" class="toc-anchor after"></a></h3>

Un champ `mediapicker` peut être ajouté à votre blueprint comme ci-dessous :

```yaml
1 | header.myimage:
2 |   type: mediapicker
3 |   folder: 'self@'
4 |   label: Select a file
5 |   preview_images: true
```

Le champ `mediapicker` stocke le chemin d'accès à l'image sous forme de chaîne telle que `/home/background.jpg` Afin d'accéder à cette image avec la fonctionnalité média de la page, vous devez diviser cette chaîne et obtenir :

* le chemin vers la page où cette image est stockée
* le nom de l'image.

Vous pouvez le faire via brindille en utilisant l'extrait ci-dessous :

```twig
1 | {% set image_parts = pathinfo(header.myimage) %}
2 | {% set image_basename = image_parts.basename %}
3 | {% set image_page = image_parts.dirname == '.' ? page : page.find(image_parts.dirname) %}
4 |
5 | {{ image_page.media[image_basename].html()|raw }}
```

<h2 id="Filtre/fonction Twig personnalisé">Filtre/fonction Twig personnalisé.
<a href="#Filtre/fonction Twig personnalisé" class="toc-anchor after"></a></h2>  

<h3 id="Problème-10 :">Problème :
<a href="#Problème-10 :" class="toc-anchor after"></a></h3>

Parfois, vous avez besoin d'une logique dans Twig qui ne peut être faite qu'en PHP, donc la meilleure solution est de créer un filtre ou une fonction Twig personnalisé. Un filtre est généralement ajouté à une chaîne au format : `"some string"|custom_filte`r et une fonction peut prendre une chaîne, ou tout autre type de variable : `custom_function("some string")`, mais ils sont essentiellement très similaires.

Vous pouvez également passer des paramètres supplémentaires comme : `"some string"|custom_filter('foo', 'bar')`, où les paramètres supplémentaires peuvent être utilisés à l'intérieur du filtre. Et la variation de la fonction serait : `custom_function("some string", 'foo', 'bar')`.

Pour cet exemple, nous allons créer un simple filtre Twig pour compter les prises d'une chaîne et la diviser en morceaux séparés par un délimiteur. Ceci est particulièrement utile pour des éléments tels que les numéros de carte de crédit, les clés de licence, etc.

<h3 id="La solution-10 :">La solution :
<a href="#La solution-10 :" class="toc-anchor after"></a></h3>

La meilleure façon d'ajouter cette fonctionnalité supplémentaire est d'ajouter la logique dans votre plugin personnalisé, bien que l'ajouter dans le fichier php de votre thème soit également une option. Nous utiliserons un plugin dans cet exemple pour plus de simplicité. Vous devez d'abord installer le plugin devtools pour faire de la création d'un plugin un simple processus basé sur un assistant :

    $ | bin/gpm install devtools

Ensuite, vous devez créer votre nouveau plugin personnalisé, puis remplir vos coordonnées lorsque vous y êtes invité.

```console
$ | bin/plugin devtools new-plugin
  |
  | Enter Plugin Name: ACME Twig Filters
  | Enter Plugin Description: Plugin for custom Twig filters
  | Enter Developer Name: ACME, Inc.
  | Enter GitHub ID (can be blank):
  | Enter Developer Email: hello@acme.com
  |
  | SUCCESS plugin ACME Twig Filters -> Created Successfully
  |
  | Path: /Users/joe/grav/user/plugins/acme-twig-filters
```

Par défaut, ce cadre squelette pour un nouveau plugin ajoutera un test factice à votre page via l'événement `onPageContentRaw()`. Vous devrez d'abord remplacer cette fonctionnalité par du code qui écoute l'événement` onTwigInitialized()` :

```php
1  | public function onPluginsInitialized()
2  |     {
3  |         // Don't proceed if we are in the admin plugin
4  |         if ($this->isAdmin()) {
5  |             return;
6  |         }
7  | 
8  |         // Enable the main event we are interested in
9  |         $this->enable([
10 |             'onTwigInitialized' => ['onTwigInitialized', 0]
11 |         ]);
12 |     }
13 | 
14 |     /**
15 |      * @param Event $e
16 |      */
17 |     public function onTwigInitialized(Event $e)
18 |     {
19 | 
20 |    }
```

Nous devons d'abord enregistrer le filtre dans la méthode `onTwigInitialized()` :

```php
1 | /**
2 |      * Break a string up into chunks
3 |      */
4 |     public function chunkString($string, $chunksize = 4, $delimiter = '-')
5 |     {
6 |         return (trim(chunk_split($string, $chunksize, $delimiter), $delimiter));
7 |    }
```

Le premier paramètre de la méthode enregistre le `chunker` comme nom de filtre et le `chunkString comme méthode PHP où la logique se produit. Nous devons donc créer ceci ensuite :`

```php
1 | /**
2 |      * Break a string up into chunks
3 |      */
4 |     public function chunkString($string, $chunksize = 4, $delimiter = '-')
5 |     {
6 |         return (trim(chunk_split($string, $chunksize, $delimiter), $delimiter));
7 |     }
```

Vous pouvez maintenant l'essayer dans vos modèles Twig comme ceci :

    {{ "ER27XV3OCCDPRJK5IVSDME6D6OT6QHK5"|chunker }}

Ce qui produirait :

    ER27-XV3O-CCDP-RJK5-IVSD-ME6D-6OT6-QHK5

ou vous pouvez passer des paramètres supplémentaires :

    {{ "ER27XV3OCCDPRJK5IVSDME6D6OT6QHK5"|chunker(8, '|') }}

qui produirait :

    {{ "ER27XV3OCCDPRJK5IVSDME6D6OT6QHK5"|chunker(8, '|') }}

Enfin, si vous voulez que cela soit disponible via une fonction et pas seulement un filtre, vous pouvez simplement enregistrer une fonction Twig avec le même nom dans la méthode `onTwigInitialized()` :

```php
1  | /**
2  |       * @param Événement $e
3  |       */
4  |      fonction publique onTwigInitialized(Event $e)
5  |      {
6  |          $this->grav['brindille']->brindille()->addFilter(
7  |              nouveau \Twig_SimpleFilter('chunker', [$this, 'chunkString'])
8  |          );
9  |          $this->grav['brindille']->brindille()->addFunction(
10 |              nouveau \Twig_SimpleFunction('chunker', [$this, 'chunkString'])
11 |          );
12 |     }
```

Et puis vous pouvez utiliser la syntaxe de la fonction :

    {{ chunker("ER27XV3OCCDPRJK5IVSDME6D6OT6QHK5", 8, '|') }}

<h2 id="Étendre le modèle de base du thème hérité">Étendre le modèle de base du thème hérité.
<a href="#Étendre le modèle de base du thème hérité" class="toc-anchor after"></a></h2>

<h3 id="Problème-11 :">Problème :
<a href="#Problème-11 :" class="toc-anchor after"></a></h3>

Parfois, vous devez étendre le modèle de base lui-même. Cela peut se produire lorsqu'il n'existe aucun moyen simple et évident d'étendre des blocs déjà présents dans un modèle. Utilisons Quark comme exemple pour le thème parent et vous souhaitez étendre themes`/quark/templates/partials/base.html.twig` à votre thème `myTheme`.

<h3 id="La solution-11 :">La solution :
<a href="#La solution-11 :" class="toc-anchor after"></a></h3>

Vous pouvez ajouter Quark en tant qu'espace de noms Twig en utilisant `my-theme.php` du thème pour écouter l'événement `onTwigLoader` et en ajoutant le répertoire de modèles de thème Quark. Le contenu de la classe devrait ressembler à ceci :

```php
1  | <?php
2  |     namespace Grav\Theme;
3  | 
4  |     use Grav\Common\Grav;
5  |     use Grav\Common\Theme;
6  |
7  | class MyTheme extends Quark {
8  |         public static function getSubscribedEvents() {
9  |             return [
10 |                 'onTwigLoader' => ['onTwigLoader', 10]
11 |             ];
12 |         }
13 |
14 |         public function onTwigLoader() {
15 |             parent::onTwigLoader();
16 | 
17 |             // add quark theme as namespace to twig
18 |             $quark_path = Grav::instance()['locator']->findResource('themes://quark');
19 |            $this->grav['twig']->addPath($quark_path . DIRECTORY_SEPARATOR . 'templates', 'quark');
20 |         }
21 |     }
```

Maintenant, dans `themes/my-theme/templates/partials/base.html.twig`, vous pouvez étendre le modèle de base Quarks comme ceci :

```twig
1 | {% étend '@quark/partials/base.html.twig' %}
2 |
3 |     {% block header %}
4 |     This is a new extended header.
5 |     {% endblock %}
```

