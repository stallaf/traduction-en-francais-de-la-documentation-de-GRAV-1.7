<h1 class="rem">Les filtres Twig</h1>

Les filtres Twig sont appliqués aux variables Twig en utilisant le `|` caractère suivi du nom du filtre. Les paramètres peuvent être transmis comme les fonctions Twig en utilisant des parenthèses.

<div class="typo" id="absolute_url">
absolute_url
<a href="#absolute_url" class="toc-anchor after"></a>
</div>

Prend un extrait de code HTML contenant un attribut `src` ou `href` qui utilise un chemin relatif. Convertit la chaîne de chemin d'accès en un format d'URL absolu, y compris le nom d'hôte.

`<img src="/some/path/to/image.jpg"/>|absolute_url` → 

<div class="typo" id="array_unique">
array_unique
<a href="#array_unique" class="toc-anchor after"></a>
</div>

Wrapper pour PHP `array_unique()` qui supprime les doublons d'un tableau.

`['foo', 'bar', 'foo', 'baz']|array_unique` **→ Array ( [0] => foo [1] => bar [3] => baz )**

<div class="typo" id="base32_encode">
base32_encode
<a href="#base32_encode" class="toc-anchor after"></a>
</div>

Effectue un encodage base32 sur la variable `'some variable here'|base32-encode` **→  ONXW2ZJAOZQXE2LBMJWGKIDIMVZGK**

<div class="typo" id="base32_decode">
base32_decode
<a href="#base32_decode" class="toc-anchor after"></a>
</div>

Effectue un décodage en base32 sur la variable
`'ONXW2ZJAOZQXE2LBMJWGKIDIMVZGK'|base32_decode` **→ some variable here**

<div class="typo" id="base64_encode">
base64_encode
<a href="#base64_encode" class="toc-anchor after"></a>
</div>

Effectue un encodage base64 sur la variable `'some variable here'|base64_encode` **→ c29tZSB2YXJpYWJsZSBoZXJl**

<div class="typo" id="base64_decode">
base64_decode
<a href="#base64_decode" class="toc-anchor after"></a>
</div>

Effectue un décodage base64 sur la variable
`'c29tZSB2YXJpYWJsZSBoZXJl'|base64_decode` **→ some variable here**

<div class="typo" id="basename">
basename
<a href="#basename" class="toc-anchor after"></a>
</div>

Renvoie le nom de base d'un chemin.

`'/etc/sudoers.d'|basename` **→ sudoers.d**

<div class="typo" id="camelize">
camelize
<a href="#camelize" class="toc-anchor after"></a>
</div>

Convertit une chaîne au format "CamelCase"

`'send_email'|camelize` **→ SendEmail**l

<div class="typo" id="chunk_split">
chunk_split
<a href="#chunk_split" class="toc-anchor after"></a>
</div>

Divise une chaîne en plus petits morceaux d'une certaine taille

`'ONXW2ZJAOZQXE2LBMJWGKIDIMVZGKA'|chunk_split(6, '-')` **→ ONXW2Z-JAOZQX-E2LBMJ-WGKIDI-MVZGKA-**

<div class="typo" id="contains">
contains
<a href="#contains" class="toc-anchor after"></a>
</div>

Déterminer si une chaîne particulière contient une autre chaîne

`'une chaîne avec des choses dedans'|contains('things')` **→ 1**

<h2 id="Casting Values">Casting Values
<a href="#Casting Values" class="toc-anchor after"></a></h2>

PHP 7 obtient des vérifications de type plus strictes, ce qui signifie que le passage d'une valeur de type incorrect peut désormais lever une exception. Pour éviter cela, vous devez utiliser des filtres qui garantissent que la valeur transmise à une méthode est valide :

<div class="typo" id="string">
string
<a href="#string" class="toc-anchor after"></a>
</div>

Utilisez `|string` pour convertir la valeur en chaîne.

<div class="typo" id="int">
int
<a href="#int" class="toc-anchor after"></a>
</div>

Utilisez `|int` pour convertir la valeur en entier.

<div class="typo" id="bool">
bool
<a href="#bool" class="toc-anchor after"></a>
</div>

Utilisez `|bool` pour convertir la valeur en booléen.

<div class="typo" id="float">
float
<a href="#float" class="toc-anchor after"></a>
</div>

Utilisez `|float` pour convertir la valeur en nombre à virgule flottante.

<div class="typo" id="array">
array
<a href="#array" class="toc-anchor after"></a>
</div>

Utilisez `|array` pour transtyper la valeur dans un tableau.

<div class="typo" id="defined">
defined
<a href="#defined" class="toc-anchor after"></a>
</div>

Parfois, vous voulez vérifier si une variable est définie, et si ce n'est pas le cas, fournissez une valeur par défaut. Par example:

`set header_image_width = page.header.header_image_width|defined(900)`

Cela définira la variable `header_image_width` sur la valeur `900` si elle n'est pas définie dans l'en-tête de la page.

<div class="typo" id="dirname">
dirname
<a href="#dirname" class="toc-anchor after"></a>
</div>

Renvoie le dirname d'un chemin.

`'/etc/sudoers.d'|dirname` **→ /etc**

<div class="typo" id="ends_with">
ends_with
<a href="#ends_with" class="toc-anchor after"></a>
</div>

Prend une aiguille et une botte de foin et détermine si la botte de foin se termine par l'aiguille. Fonctionne également maintenant avec un tableau d'aiguilles et renverra `true` si une botte de foin se termine par l'aiguille.

`'le renard brun rapide'|ends_with('fox')` **→ true**

<div class="typo" id="fieldName">
fieldName
<a href="#fieldName" class="toc-anchor after"></a>
</div>

Filtre le nom du champ en changeant la notation par points en notation par tableau

`'field.name'|fieldName field[name]` **→ field[name]**

<div class="typo" id="gate_type">
gate_type
<a href="#gate_type" class="toc-anchor after"></a>
</div>

Obtient le type d'une variable :

`page|get_type` **→ object**

<div class="typo" id="humanize">
humanize
<a href="#humanize" class="toc-anchor after"></a>
</div>

Convertit une chaîne dans un format plus "lisible par l'homme"

`'something_text_to_read'|humanise` **→ something text to read**

<div class="typo" id="hyphenize">
hyphenize
<a href="#hyphenize" class="toc-anchor after"></a>
</div>

Convertit une chaîne en une version avec trait d'union.

`'Something text to read'|hyphenize` **→ something text to read**

<div class="typo" id="json_decode">
json_decode
<a href="#json_decode" class="toc-anchor after"></a>
</div>

Vous pouvez décoder JSON en appliquant simplement ce filtre :

`array|json_decode`

```twig
1 | {% set array = '{"first_name": "Guido", "last_name":"Rossum"}'|json_decode %}
2 | {{ print_r(array) }}
```

```twig
stdClass Object
(
    [first_name] => Guido
    [last_name] => Rossum
)
```

<div class="typo" id="ksort">
ksort
<a href="#ksort" class="toc-anchor after"></a>
</div>

Trier une carte de tableau par chaque clé

`array|ksort`

```twig
1 | {% set items = {'orange':1, 'apple':2, 'peach':3}|ksort %}
2 | {{ print_r(items) }}
```

```twig
Array
(
    [apple] => 2
    [orange] => 1
    [peach] => 3
)
```

<div class="typo" id="ltrim">
ltrim
<a href="#ltrim" class="toc-anchor after"></a>
</div>

`'/strip/leading/slash/'|ltrim('/')` → strip/leading/slash/

Trim gauche supprime les espaces de fin au début d'une chaîne. Il peut également supprimer d'autres caractères en définissant le masque de caractère (voir [https://php.net/manual/en/function.ltrim.php](https://php.net/manual/en/function.ltrim.php))

<div class="typo" id="markdown">
markdown
<a href="#markdown" class="toc-anchor after"></a>
</div>

Prenez une chaîne arbitraire contenant du démarquage et convertissez-la en HTML à l'aide de l'analyseur de démarquage de Grav. Paramètre `boolean` facultatif :

- `true` (par défaut) : traiter comme un bloc (mode texte, le contenu sera enveloppé dans des balises `<p>`)
- `false` : traiter comme une ligne (le contenu ne sera pas encapsulé)

`string|markdown($is_block)`

```html
1 | <div class="div">
2 | {{ 'Un paragraphe avec **markdown** et [un lien](http://www.cnn.com)'|markdown }}
3 | </div>
4 |
5 | <p class="paragraph">{{'Une ligne avec **markdown** et [un lien](http://www.cnn.com)'|markdown(false)}}</p>
```

```html
</p>
<div class="div">
<p>Un paragraphe avec <strong>markdown</strong> et <a href="http://www.cnn.com">un lien</a></p>
</div>
<p class="paragraph">Une ligne avec <strong>markdown</strong> et <a href="http://www.cnn.com">un lien</a></p>
<p>
```

<div class="typo" id="md5">
md5
<a href="#md5" class="toc-anchor after"></a>
</div>

Crée un hachage md5 pour la chaîne

`'n'importe quoi'|md5` **→ f0e166dc34d14d6c228ffac576c9a43c**

<div class="typo" id="modulus">
modulus
<a href="#modulus" class="toc-anchor after"></a>
</div>

Effectue la même fonctionnalité que le symbole Modulo `%` en PHP. Il fonctionne sur un nombre en transmettant un diviseur numérique et un tableau facultatif d'éléments à sélectionner.

`7|modulus(3, ['rouge', 'bleu', 'vert'])` **→ bleu**

<div class="typo" id="monthize">
monthize
<a href="#monthize" class="toc-anchor after"></a>
</div>

Convertit un nombre entier de jours en nombre de mois

`'181'|monthize` **→ 5**

<div class="typo" id="nicecron">
nicecron
<a href="#nicecron" class="toc-anchor after"></a>
</div>

Obtient une sortie lisible par l'homme pour la syntaxe cron

`"2 * * * *"|nicecron` **→ Toutes les heures à 2 minutes après l'heure**

<div class="typo" id="nicefilesize">
nicefilesize
<a href="#nicefilesize" class="toc-anchor after"></a>
</div>

Sortie d'une taille de fichier dans un format de bonne taille lisible par l'homme

`612394|nicefilesize` **→ 598,04 Ko**

<div class="typo" id="nicenumber">
nicenumber
<a href="#nicenumber" class="toc-anchor after"></a>
</div>

Générer un nombre dans un format de nombre gentil lisible par l'homme

`12430|nicenumber` **→ 12,43 k**

<div class="typo" id="nicetime">
nicetime
<a href="#nicetime" class="toc-anchor after"></a>
</div>

Afficher une date dans un format d'heure agréable lisible par l'homme

`page.date|nicetime(false)` **→ Il y a 4 mois**

Le premier argument spécifie s'il faut utiliser une description de date au format complet. C'est `true` par défaut.

Vous pouvez fournir un deuxième argument de `false` si vous souhaitez supprimer le descripteur relatif au temps (comme « il y a » ou « à partir de maintenant » dans votre langue) du résultat.

<div class="typo" id="of_type">
of_type
<a href="#of_type" class="toc-anchor after"></a>
</div>

Vérifie le type d'une variable au paramètre :

`page|of_type('string')` **→ false**

<div class="typo" id="ordinalise">
ordinalise
<a href="#ordinalise" class="toc-anchor after"></a>
</div>

Ajoute un ordinal à l'entier (tel que 1er, 2e, 3e, 4e)

`'10'|ordinalize` **→ 10**

<div class="typo" id="pad">
pad
<a href="#pad" class="toc-anchor after"></a>
</div>

Complète une chaîne à une certaine longueur avec un autre caractère. Il s'agit d'un wrapper pour la fonction PHP str_pad().

`'foobar'|pad(10, '-')` **→ foobar----**

<div class="typo" id="pluralize">
pluralize
<a href="#pluralize" class="toc-anchor after"></a>
</div>

Convertit une chaîne en version plurielle anglaise

`'person'| → pluralize` **→ les gens**

`pluralize` prend également un paramètre numérique facultatif que vous pouvez transmettre lorsque vous ne savez pas à l'avance à combien d'éléments le nom fera référence. Sa valeur par défaut est 2, donc fournira la forme plurielle si elle est omise. Par example:

    <p>We have {{ num_vacancies }} {{ 'vacancy'|pluralize(num_vacancies) }} right now.</p>

<div class="typo" id="print_r">
print_r
<a href="#print_r" class="toc-anchor after"></a>
</div>

Imprime des informations lisibles par l'homme sur une variable

`page.header|print_r`

```twig
Objet stdClass
(
    [title] => Filtres Brindille
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
            [title] => Filtres Brindille
            [image] => https://webshot.getgrav.org/?url=aHR0cHM6Ly9sZWFybi5nZXRncmF2Lm9yZy8xNy90aGVtZXMvdHdpZy10YWdzLWZpbHRlcnMtZnVuY3Rpb25zL2ZpbHRlcnM=
            [robots] => tous
            [fb:app_id] =>
            [og:url] => https://learn.getgrav.org/17/themes/twig-tags-filters-functions/filters
            [og:site_name] => Documentation Grav
            [og:title] => Filtres Twig
            [og:description] =>
            [og:type] => article
            [og:image] => https://webshot.getgrav.org/?url=aHR0cHM6Ly9sZWFybi5nZXRncmF2Lm9yZy8xNy90aGVtZXMvdHdpZy10YWdzLWZpbHRlcnMtZnVuY3Rpb25zL2ZpbHRlcnM=
            [og:image:largeur] => 1200
            [og:image:hauteur] => 630
            [og:image:secure] => https://webshot.getgrav.org/?url=aHR0cHM6Ly9sZWFybi5nZXRncmF2Lm9yZy8xNy90aGVtZXMvdHdpZy10YWdzLWZpbHRlcnMtZnVuY3Rpb25zL2ZpbHRlcnM=
            [twitter:card] => summary_large_image
            [gazouillement:site] =>
            [twitter:title] => Filtres Twig
            [twitter:description] =>
            [twitter:image] => https://webshot.getgrav.org/?url=aHR0cHM6Ly9sZWFybi5nZXRncmF2Lm9yZy8xNy90aGVtZXMvdHdpZy10YWdzLWZpbHRlcnMtZnVuY3Rpb25zL2ZpbHRlcnM=
            [twitter:image:alt] => Filtres Twig
        )
)
```

<div class="typo" id="randomize">
randomize
<a href="#randomize" class="toc-anchor after"></a>
</div>

Randomise la liste fournie. Si une valeur est fournie en tant que paramètre, il ignorera les n premières valeurs et les conservera dans l'ordre.

`array|randomiser`

```twig
1 | {% set ritems = ['one', 'two', 'three', 'four', 'five', 'six', 'seven', 'eight', 'nine', 'ten']|randomize(2) %}
2 | {{ print_r(ritems) }}
```

```
Array
(
    [0] => one
    [1] => two
    [2] => four
    [3] => seven
    [4] => ten
    [5] => three
    [6] => eight
    [7] => five
    [8] => nine
    [9] => six
)
```

<div class="typo" id="regex_replace">
regex_replace
<a href="#regex_replace" class="toc-anchor after"></a>
</div>

Un wrapper utile pour la méthode PHP [preg_replace()](https://php.net/manual/en/function.preg-replace.php), vous pouvez effectuer des remplacements complexes de Regex sur du texte via ce filtre :

`'The quick brown fox jumps over the lazy dog.'|regex_replace(['/quick/','/brown/','/fox/','/dog/'], ['slow','black', 'bear','turtle'])` **→ The slow black bear jumps over the lazy turtle**.


<div class="notice note">
Utilisez le délimiteur <code>~</code> plutôt que le délimiteur <code>/</code> si possible. Sinon, vous devrez probablement <a href="https://github.com/getgrav/grav/issues/833">double-échapper certains caractères</a>. Par exemple <code>~\/\/#.*~</code> plutôt que <code>'/\\/\\#.*/'</code>, qui se conforme plus étroitement à la <a href="https://www.php.net/manual/en/regexp.reference.delimiters.php">syntaxe PCRE</a> utilisée par PHP.
</div>

<div class="typo" id="rtrim">
rtrim
<a href="#rtrim" class="toc-anchor after"></a>
</div>

`'/strip/trailing/slash/'|rtrim('/')` **→ /strip/trailing/slash**

Supprime les espaces de fin à la fin d'une chaîne. Il peut également supprimer d'autres caractères en définissant le masque de caractère (voir [https://php.net/manual/en/function.rtrim.php](https://php.net/manual/en/function.rtrim.php)).

<div class="typo" id="singularize">
singularize
<a href="#singularize" class="toc-anchor after"></a>
</div>

Convertit une chaîne en version anglaise singulière

`'shoes'|singularize` **→ shoe**

<div class="typo" id="safe_email">
safe_email
<a href="#safe_email" class="toc-anchor after"></a>
</div>

Le filtre de messagerie sécurisé convertit une adresse e-mail en caractères ASCII pour la rendre plus difficile à reconnaître et à capturer par les robots de spam.

`"someone@domaine.com"|safe_email` **→ somone@domaine.com**

Exemple d'utilisation avec un lien mailto :

```html
1 | <a href="mailto:{{ 'your.email@server.com'|safe_email }}">
2 |    Email me
3 | </a>
```

Vous ne remarquerez peut-être pas de différence au début, mais l'examen de la source de la page (sans utiliser les outils de développement du navigateur, la source de la page réelle) révélera l'encodage des caractères sous-jacents.

<div class="typo" id="sort_by_key">
sort_by_key
<a href="#sort_by_key" class="toc-anchor after"></a>
</div>

Trier une carte de tableau par une clé particulière

`array|sort_by_key`

```twig
1 | {% set people = [{'email':'fred@yahoo.com', 'id':34}, {'email':'tim@exchange.com', 'id':21}, 
2 | {'email':'john@apple.com', 'id':2}]|sort_by_key('id') %}
3 | {% for person in people %}{{ person.email }}:{{ person.id }}, {% endfor %}
```

**john@apple.com:2, tim@exchange.com:21, fred@yahoo.com:34,**

<div class="typo" id="starts_with">
starts_with
<a href="#starts_with" class="toc-anchor after"></a>
</div>

Prend une aiguille et une botte de foin et détermine si la botte de foin commence par l'aiguille. Fonctionne également maintenant avec un tableau d'aiguilles et renverra `true` si aucune botte de foin commence par l'aiguille.

`'The quick brown fox'|starts_with('the')` **→ true**

<div class="typo" id="titleize">
titleize
<a href="#titleize" class="toc-anchor after"></a>
</div>

Convertit une chaîne au format "Title Case"

`'welcome page'|titleize` **→ Welcome Page**

<div class="typo" id="t">
t
<a href="#t" class="toc-anchor after"></a>
</div>

Traduire une chaîne dans la langue courante

`'MY_LANGUAGE_KEY_STRING'|t` **→ 'Du texte en anglais'**

Cela suppose que ces chaînes de langue sont traduites sur votre site et que vous avez activé la prise en charge de -language. Veuillez vous référer à la [documentation multilingue](./multi-langue.md) pour des informations plus détaillées.

<div class="typo" id="tu">
tu
<a href="#tu" class="toc-anchor after"></a>
</div>

Traduire une chaîne dans la langue actuelle définie dans les préférences utilisateur de l'interface d'administration

`'MY_LANGUAGE_KEY_STRING'|tu` **→ Quelque texte en anglais'**

Cela utilise le champ de langue défini dans l'utilisateur yaml.

<div class="typo" id="ta">
ta
<a href="#ta" class="toc-anchor after"></a>
</div>

Traduit un tableau avec une langue utilise le filtre `|ta`. Voir la [documentation multilingue](./multi-langue.md) pour un exemple détaillé.

`'MONTHS_OF_THE_YEAR'|ta(post.date|date('n') - 1)` **→ april**

<div class="typo" id="tl">
tl
<a href="#tl" class="toc-anchor after"></a>
</div>

Traduit une chaîne dans une langue spécifique. Pour plus de détails, consultez la [documentation multilingue](./multi-langue.md).

`'SIMPLE_TEXT'|tl(['en'])`

<div class="typo" id="truncate">
truncate
<a href="#truncate" class="toc-anchor after"></a>
</div>

Vous pouvez facilement générer une version raccourcie et tronquée d'une chaîne à l'aide de ce filtre. Il prend un certain nombre de caractères comme seul champ obligatoire, mais a quelques autres options :

`'one sentence. two sentences'|truncate(5)|raw` **→ one s…**

Tronque simplement à 5 caractères.

`'one sentence. two sentences'|truncate(5, true)|raw` **→ one sentence.…**

<div class="notice note">
Le filtre <code>|raw</code> Twig doit être utilisé avec la valeur par défaut <code>&hellip;</code> (points de suspension) élément de remplacement afin qu'il soit rendu avec l'échappement automatique de Twig.
</div>

Tronque à la fin de phrase la plus proche après 5 caractères.

Vous pouvez également tronquer le texte HTML, mais vous devez d'abord utiliser le filtre `|striptags` pour supprimer toute mise en forme HTML qui pourrait être endommagée si vous terminez entre des balises :

`'<span>une <strong>sentence</strong>. deux phrases</span>' |raw|striptags|truncate(25)` **→ one sentence. two sentenc…**

<h2 id="Versions spécialisées :">Versions spécialisées :
<a href="#Versions spécialisées :" class="toc-anchor after"></a></h2>

<div class="typo" id="safe_truncate">
safe_truncate
<a href="#safe_truncate" class="toc-anchor after"></a>
</div>

Utilisez `|safe_truncate` pour tronquer le texte par nombre de caractères d'une manière "word-safe".

<div class="typo" id="truncate_html">
truncate_html
<a href="#truncate_html" class="toc-anchor after"></a>
</div>

Utilisez `|truncate_html` pour tronquer le code HTML par nombre de caractères. pas "mot-sûr" !

<div class="typo" id="safe_truncate_html">
safe_truncate_html
<a href="#safe_truncate_html" class="toc-anchor after"></a>
</div>

Utilisez `|safe_truncate_html` pour tronquer le code HTML par nombre de caractères d'une manière « sans danger pour les mots ».

<div class="typo" id="underscorize">
underscorize
<a href="#underscorize" class="toc-anchor after"></a>
</div>

Convertit une chaîne au format "under_scored"

`'CamelCased'|underscorize` **→ camel_cased**

<div class="typo" id="yaml_encode">
yaml_encode
<a href="#yaml_encode" class="toc-anchor after"></a>
</div>

Vider/encoder une variable dans la syntaxe YAML

```twig
1 | {% set array = {foo: [0, 1, 2, 3], baz: 'qux' } %}
2 | {{ array|yaml_encode }}
```

```
foo :
  - 0
  - 1
  - 2
  - 3
baz: qux
```

<div class="typo" id="yaml_decode">
yaml_decode
<a href="#yaml_decode" class="toc-anchor after"></a>
</div>

Décoder/analyser une variable à partir de la syntaxe YAML

```twig
1 | {% set yaml = "foo: [0, 1, 2, 3]\nbaz: qux" %}
2 | {{ yaml|yaml_decode|var_dump }}
```

```yaml
array(2) {
  ["foo"]=>
  array(4) {
    [0]=>
    int(0)
    [1]=>
    int(1)
    [2]=>
    int(2)
    [3]=>
    int(3)
  }
  ["baz"]=>
  string(3) "qux"
}
```

