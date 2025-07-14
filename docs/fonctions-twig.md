<h1 class="rem">Les fonctions Twig</h1>

Les fonctions Twig sont appelées directement avec tous les paramètres passés entre parenthèses.

<div class="typo" id="array">
array
<a href="#array" class="toc-anchor after"></a>
</div>

Convertir une valeur en tableau

    {% set value = array(value) %}

<div class="typo" id="array_dyff">
array_dyff
<a href="#array_dyff" class="toc-anchor after"></a>
</div>

Calcule la différence des tableaux.

1 | {% set diff = array_diff(array1, array2...) %}

<div class="typo" id="array_key_value">
array_key_value
<a href="#array_key_value" class="toc-anchor after"></a>
</div>

La fonction `array_key_value` permet d'ajouter une paire clé/valeur à un tableau associé

```twig
1 | {% set my_array = {fruit: 'apple'} %}
2 | {% set my_array = array_key_value('meat','steak', my_array) %}
3 | {{ print_r(my_array)}}
```

sorties : **Array ( [fruit] => apple [meat] => steak )**

<div class="typo" id="array_key_exists">
array_key_exists
<a href="#array_key_exists" class="toc-anchor after"></a>
</div>

Wrapper pour la fonction `array_key_exists` de PHP qui renvoie si oui ou non une clé existe dans un tableau associatif.

```twig
1 | {% set my_array = {fruit: 'apple', meat: 'steak'} %}
2 | {{ array_key_exists('meat', my_array) }}
```

sorties : **1**

<div class="typo" id="array_intersect">
array_intersect
<a href="#array_intersect" class="toc-anchor after"></a>
</div>

La fonction `array_intersect` fournit l'intersection de deux tableaux ou collections Grav.

```twig
1 | {% set array_1 = {fruit: 'apple', meat: 'steak'} %}
2 | {% set array_2 = {fish: 'tuna', meat: 'steak'} %}
3 | {{ print_r(array_intersect(array_1, array_2)) }}
```

sorties : **Array ( [meat] => steak )**

<div class="typo" id="array_unique">
array_unique
<a href="#array_unique" class="toc-anchor after"></a>
</div>

Wrapper pour PHP `array_unique()` qui supprime les doublons d'un tableau.

`array_unique(['foo', 'bar', 'foo', 'baz'])` **→ Array ( [0] => foo [1] => bar [3] => baz )**

<div class="typo" id="authorize">
authorize
<a href="#authorize" class="toc-anchor after"></a>
</div>

Autorise un utilisateur authentifié à voir une ressource. Accepte une seule chaîne d'autorisation ou un tableau de chaînes d'autorisation.

`autorize(['admin.statistiques', 'admin.super'])`

<div class="typo" id="body_class">
body_class
<a href="#body_class" class="toc-anchor after"></a>
</div>

Prend un tableau de classes, et si elles ne sont pas définies sur `body_classes`, regardez si elles sont définies dans la configuration actuelle du thème.

`set body_classes = body_class(['header-fixed', 'header-animated', 'header-dark', 'header-transparent', 'sticky-footer'])`

<div class="typo" id="cron">
cron
<a href="#cron" class="toc-anchor after"></a>
</div>

Créer un objet "Cron" à partir de la syntaxe cron

`cron("3 * * * *").getNextRunDate()|date(config.date_format.default)`

<div class="typo" id="dump">
dump
<a href="#dump" class="toc-anchor after"></a>
</div>

Prend une variable Twig valide et la vide dans le panneau du débogueur Grav. Le débogueur doit être **activé** pour voir les valeurs dans l'onglet messages.

`dump(page.header)`

<div class="typo" id="debug">
debug
<a href="#debug" class="toc-anchor after"></a>
</div>

Identique à `dump()`

<div class="typo" id="evaluate">
evaluate
<a href="#evaluate" class="toc-anchor after"></a>
</div>

La fonction d'évaluation peut être utilisée pour évaluer une chaîne en tant que Twig :

`evaluate('grav.language.getLanguage')`

<div class="typo" id="evaluate_twig">
evaluate_twig
<a href="#evaluate_twig" class="toc-anchor after"></a>
</div>

Semblable à evaluate, mais évaluera et traitera avec Twig

`evaluate_twig('Ceci est une variable brindille : {{ foo }}', {foo: 'bar'}))` **→ This is a twig variable: bar**

<div class="typo" id="exif">
exif
<a href="#exif" class="toc-anchor after"></a>
</div>

Sortez les données EXIF ​​​​d'une image en fonction de son chemin de fichier. Cela nécessite que `media: auto_metadata_exif: true` soit défini dans `system.yaml`. Par exemple, dans un modèle Twig :

```twig
1 | {% set image = page.media['sample-image.jpg'] %}
2 | {% set exif = exif(image.filepath, true) %}
3 | {{ exif.MaxApertureValue }}
```

Cela écrirait la valeur `MaxApertureValue` définie dans l'appareil photo, par exemple "40/10". Vous pouvez toujours utiliser `{{ dump(exif)}}S ?` pour afficher toutes les données disponibles dans le débogueur.

<div class="typo" id="get_cookie">
get_cookie
<a href="#get_cookie" class="toc-anchor after"></a>
</div>

Récupérez la valeur d'un cookie avec cette fonction :

`get_cookie('your_cookie_key')`

<div class="typo" id="get_type">
get_type
<a href="#get_type" class="toc-anchor after"></a>
</div>

Obtient le type d'une variable :

`get_type(page)` **→ object **

<div class="typo" id="gist">
gist
<a href="#gist" class="toc-anchor after"></a>
</div>

Prend un identifiant Github Gist et crée le code d'intégration Gist approprié

`gist('bc448ff158df4bc56217')` **→ &lt;script src="https://gist.github.com/bc448ff158df4bc56217.js"&gt;>&lt;/script&gt;**

<div class="typo" id="http_response_code">
http_response_code
<a href="#http_response_code" class="toc-anchor after"></a>
</div>

Si response_code est fourni, le code d'état précédent sera renvoyé. Si response_code n'est pas fourni, le code d'état actuel sera renvoyé. Ces deux valeurs prendront par défaut un code d'état 200 si elles sont utilisées dans un environnement de serveur Web.

`http_response_code(404)`

<div class="typo" id="isajaxrequest">
isajaxrequest
<a href="#isajaxrequest" class="toc-anchor after"></a>
</div>

la fonction `isajaxrequest()` peut être utilisée pour vérifier si l'option d'en-tête `HTTP_X_REQUESTED_WITH` est définie :

<div class="typo" id="json_decode">
json_decode
<a href="#json_decode" class="toc-anchor after"></a>
</div>

Vous pouvez décoder JSON en appliquant simplement ce filtre :

`json_decode({"first_name": "Guido", "last_name":"Rossum"})`

<div class="typo" id="media_directory">
media_directory
<a href="#media_directory" class="toc-anchor after"></a>
</div>

Renvoie un objet média pour un répertoire arbitraire. Une fois obtenu, vous pouvez manipuler les images de la même manière que les pages.

`media_directory('theme://images')['some-image.jpg'].cropResize(200,200).html`

<div class="typo" id="nicefilesize">
nicefilesize
<a href="#nicefilesize" class="toc-anchor after"></a>
</div>

Sortie d'une taille de fichier dans un format de bonne taille lisible par l'homme.

`nicefilesize(612394)` **→ 598,04 Ko**

<div class="typo" id="nicenumber">
nicenumber
<a href="#nicenumber" class="toc-anchor after"></a>
</div>

Générer un nombre dans un format de nombre gentil lisible par l'homme

`nicenumber(12430)` **→ 12,43 k**

<div class="typo" id="nicetime">
nicetime
<a href="#nicetime" class="toc-anchor after"></a>
</div>

Afficher une date dans un format d'heure agréable lisible par l'homme

`nicetime(page.date)` **→ il y a 9 mois**

<div class="typo" id="nonce_field">
nonce_field
<a href="#nonce_field" class="toc-anchor after"></a>
</div>

Générez un champ nonce de sécurité Grav pour un formulaire avec une `action` requise :

`nonce_field('action')` **→ &lt;input type="hidden" name="nonce" value="f4b4c7a6f7b9cc18ca81db4080ad14f2"/&gt;**

<div class="typo" id="of_type">
of_type
<a href="#of_type" class="toc-anchor after"></a>
</div>

Vérifie le type d'une variable au paramètre :

`of_type(page, 'string')` **→ false**

<div class="typo" id="pethinfo">
pethinfo
<a href="#pethinfo" class="toc-anchor after"></a>
</div>

Analyse un chemin dans un tableau.

```twig
1 | {% set parts = pathinfo('/www/htdocs/inc/lib.inc.php') %}
2 | {{ print_r(parts) }}
```

affiche : **Array ( [nom_répertoire] => /www/htdocs/inc [nom_base] => lib.inc.php [extension] => php [nom_fichier] => lib.inc )**

<div class="typo" id="print_r">
print_r
<a href="#print_r" class="toc-anchor after"></a>
</div>

Imprime une variable dans un format lisible

`print_r(page.header)`

```twig
Objet stdClass
(
    [title] => Fonctions Twig
    [body_classes] => twig__headers
    [page-toc] => Tableau
        (
            [actif] => 1
            [début] => 3
            [profondeur] => 1
        )

    [processus] => Tableau
        (
            [brindille] => 1
        )

    [taxonomie] => Tableau
        (
            [catégorie] => docs
        )

    [métadonnées] => Tableau
        (
            [description] =>
            [générateur] => GravCMS
            [title] => Fonctions Twig
            [image] => https://webshot.getgrav.org/?url=aHR0cHM6Ly9sZWFybi5nZXRncmF2Lm9yZy8xNy90aGVtZXMvdHdpZy10YWdzLWZpbHRlcnMtZnVuY3Rpb25zL2Z1bmN0aW9ucw==
            [robots] => tous
            [fb:app_id] =>
            [og:url] => https://learn.getgrav.org/17/themes/twig-tags-filters-functions/functions
            [og:site_name] => Documentation Grav
            [og:title] => Fonctions Twig
            [og:description] =>
            [og:type] => article
            [og:image] => https://webshot.getgrav.org/?url=aHR0cHM6Ly9sZWFybi5nZXRncmF2Lm9yZy8xNy90aGVtZXMvdHdpZy10YWdzLWZpbHRlcnMtZnVuY3Rpb25zL2Z1bmN0aW9ucw==
            [og:image:largeur] => 1200
            [og:image:hauteur] => 630
            [og:image:secure] => https://webshot.getgrav.org/?url=aHR0cHM6Ly9sZWFybi5nZXRncmF2Lm9yZy8xNy90aGVtZXMvdHdpZy10YWdzLWZpbHRlcnMtZnVuY3Rpb25zL2Z1bmN0aW9ucw==
            [twitter:card] => summary_large_image
            [gazouillement:site] =>
            [twitter:title] => Fonctions Twig
            [twitter:description] =>
            [twitter:image] => https://webshot.getgrav.org/?url=aHR0cHM6Ly9sZWFybi5nZXRncmF2Lm9yZy8xNy90aGVtZXMvdHdpZy10YWdzLWZpbHRlcnMtZnVuY3Rpb25zL2Z1bmN0aW9ucw==
            [twitter:image:alt] => Fonctions Twig
        )
)
```

<div class="typo" id="random_string">
random_string
<a href="#random_string" class="toc-anchor after"></a>
</div>

Génèrera une chaîne aléatoire du nombre de caractères requis. Particulièrement utile pour créer un identifiant ou une clé unique.

`random_string(10)` **→ CKHT5LGcFA**

<div class="typo" id="unique_id">
unique_id
<a href="#unique_id" class="toc-anchor after"></a>
</div>

Génère une chaîne aléatoire avec une longueur, un préfixe et un suffixe configurables. Contrairement à la fonction intégrée PHP `uniqid()` et aux utilitaires `random_string`, cette chaîne sera générée vraiment unique et non conflictuelle.

`unique_id(9)` **→ 5b065bd9e** `unique_id(11, { préfixe : 'user_' })` **→ uniqueid(11, { préfixe : 'user' }) }}** `unique_id(13, { suffixe : '.json' })` **→ unique_id(13, { suffixe : '.json' }) }}**

<div class="typo" id="range">
range
<a href="#range" class="toc-anchor after"></a>
</div>

Génère un tableau contenant une plage d'éléments, éventuellement échelonnés

`range(25, 300, 50)` **→ Array ( [0] => 25 [1] => 75 [2] => 125 [3] => 175 [4] => 225 [5] => 275 )**

<div class="typo" id="read_file">
read_file
<a href="#read_file" class="toc-anchor after"></a>
</div>

Fonction simple pour lire un fichier basé sur un chemin de fichier et le sortir.

`read_file('plugins://admin/README.md')|markdown`

```
# Grav Standard Administration Panel Plugin

This **admin plugin** for [Grav](https://github.com/getgrav/grav) is 
an HTML user interface that provides a convenient way to configure 
Grav and easily create and modify pages...
```

<div class="typo" id="redirect_me">
redirect_me
<a href="#redirect_me" class="toc-anchor after"></a>
</div>

Redirige vers une URL de votre choix

`redirect_me('http://google.com', 304)`

<div class="typo" id="regex_filter">
regex_filter
<a href="#regex_filter" class="toc-anchor after"></a>
</div>

Effectue un `preg_grep` sur un tableau avec un motif regex

`regex_filter(['pâtes', 'poisson', 'steak', 'pommes de terre'], "/p.*/")`

```twig
Array
(
    [0] => pasta
    [3] => potatoes
)
```

<div class="typo" id="regex_replace">
regex_replace
<a href="#regex_replace" class="toc-anchor after"></a>
</div>

Un wrapper utile pour la méthode PHP [preg_replace()](https://php.net/manual/en/function.preg-replace.php), vous pouvez effectuer des remplacements complexes de Regex sur du texte via ce filtre :

`regex_replace('Le renard brun rapide saute par-dessus le chien paresseux.', ['/quick/','/brown/','/fox/','/dog/'], ['slow','black', 'ours', 'tortue'])`

```console
L'ours noir lent saute par-dessus la tortue paresseuse.
```

<div class="typo" id="regex_match">
regex_match
<a href="#regex_match" class="toc-anchor after"></a>
</div>

Un wrapper utile pour la méthode PHP [preg_match()](https://php.net/manual/en/function.preg-math.php), vous pouvez effectuer une correspondance d'expression régulière complexe sur du texte via ce filtre :

`regex_match('http://www.php.net/index.html', '@^(?:http://)?([^/]+)@i')`

```twig
Array
(
    [0] => http://www.php.net
    [1] => www.php.net
)
```

<div class="typo" id="regex_split">
regex_split
<a href="#regex_split" class="toc-anchor after"></a>
</div>

Un wrapper utile pour la méthode PHP [preg_split()](https://php.net/manual/en/function.preg-split.php). Divisez la chaîne par une expression régulière sur le texte via ce filtre :

`regex_split('hypertext language, programming', '/\\s*,\\s*/u')`

```
Array
(
    [0] => hypertext language
    [1] => programming
)
```

<div class="typo" id="repeat">
repeat
<a href="#repeat" class="toc-anchor after"></a>
</div>

Répétera tout ce qui est passé dans un certain nombre de fois.

`repeat ('blah', 10)` **→ bla bla bla bla bla bla bla bla bla bla**

<div class="typo" id="string">
string
<a href="#string" class="toc-anchor after"></a>
</div>

Renvoie une chaîne à partir d'une valeur. Si la valeur est un tableau, renvoyez-le encodé en json

`string(23)` => **"23"**

`string(['test' => 'x'])` => **{"test":"x"}**

<div class="typo" id="svg_image">
svg_image
<a href="#svg_image" class="toc-anchor after"></a>
</div>

Renvoie le contenu d'une image SVG et ajoute des classes supplémentaires si nécessaire. Fournit les avantages du svg en ligne sans avoir à coller le code directement sur la page. Utile pour les images réutilisables telles que les icônes de médias sociaux.

`{{ svg_image(path, classes, strip_style) }}`

strip_style = supprimer le style svg en ligne - utile pour le style avec les classes CSS.

Exemple:

`{{ svg_image('theme://images/something.svg', 'my-class-here mb-10', true) }}`

<div class="typo" id="theme_var">
theme_var
<a href="#theme_var" class="toc-anchor after"></a>
</div>

Obtenez une variable de thème à partir de l'en-tête de la page si elle existe, sinon utilisez la configuration du thème :

`theme_var('grid-size')`

Cela essaiera d'abord `page.header.grid-size`, si cela n'est pas défini, il essaiera `theme.grid-size` à partir du fichier de configuration du thème. il peut éventuellement prendre une valeur par défaut :

`theme_var('grid-size', 1024)`

<div class="typo" id="t">
t
<a href="#t" class="toc-anchor after"></a>
</div>

Traduire une chaîne, comme le filtre `|t`.

`t('SITE_NAME')` **→ Site Name**

<div class="typo" id="ta">
ta
<a href="#ta" class="toc-anchor after"></a>
</div>

Fonctionne de la même manière que le filtre `|ta`.

<div class="typo" id="tl">
tl
<a href="#tl" class="toc-anchor after"></a>
</div>

Traduit une chaîne dans une langue spécifique. Pour plus de détails, consultez la [documentation multilingue](./multi-langue.md).

`tl('SIMPLE_TEXT', ['fr'])`

<div class="typo" id="url">
url
<a href="#url" class="toc-anchor after"></a>
</div>

Créera une URL et convertira tous les flux d'URL PHP en ressources HTML valides. Une valeur par défaut peut être transmise dans le cas où l'URL ne peut pas être résolue.

`url('theme://images/logo.png')|default('http://www.placehold.it/150x100/f4f4f4')` **→ http://www.placehold.it/150x100/f4f4f4**

<div class="typo" id="vardump">
vardump
<a href="#vardump" class="toc-anchor after"></a>
</div>

La fonction `vardump()` affiche la variable actuelle à l'écran (plutôt que dans le débogueur comme avec `dump()`).

```twig
1 | {% set my_array = {foo: 'bar', baz: 'qux'} %}
2 | {{ vardump(my_array) }}
```
```twig
[
  "foo" => "barre"
  "baz" => "qux"
]
```

<div class="typo" id="xss">
xss
<a href="#xss" class="toc-anchor after"></a>
</div>

Autoriser une vérification manuelle d'une chaîne pour les vulnérabilités XSS

`xss('cette chaîne contient un <script>alerte("bonjour");</script>  XSS vulnerability')` **→ dangereux_tags**

