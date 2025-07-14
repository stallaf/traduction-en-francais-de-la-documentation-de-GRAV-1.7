<h1 class="rem">Flex Collection</h1>

**Flex Collection** est une **carte ordonnée d'objets** qui peut également être utilisée comme une liste.

Collection fournit quelques méthodes utiles, qui peuvent être utilisées pour rendre la sortie, récupérer des objets, filtrer et trier, etc.

<div class="notice note">
<strong>ASTUCE</strong> : Flex Collection étend <a href="https://www.doctrine-project.org/projects/doctrine-collections/en/1.6/index.html">Doctrine Collections</a>.
</div>

<h1 class="rem" id="Collection de rendu">Collection de rendu.
<a href="#Collection de rendu" class="toc-anchor after"></a></h1> 

<h2 id="render()">render().
<a href="#render()" class="toc-anchor after"></a></h2> 

`render( [layout], [context] ): Block` Rend la collection.

Paramètres:

* **layout** Nom de la mise en page (`string`)
* **context** Variables supplémentaires pouvant être utilisées dans le fichier de modèle Twig (`array`)

Retour:

* **Bloc** (`object`) Classe HtmlBlock contenant la sortie

<div class="notice tip">
<strong>REMARQUE</strong> : dans twig, il existe une balise <code>{% render %}</code>, qui doit être utilisée au lieu d'appeler directement la méthode. Cela permettra aux ressources JS/CSS de la collection de fonctionner correctement.
</div>

** - Twig**

```twig        
{% set contacts = grav.get('flex').collection('contacts') %}
{% set page = 2 %}
{% set limit = 10 %}
{% set start = (page - 1) * limit %}

<h2>Contacts:</h2>

{% render contacts.limit(start, limit) layout: 'cards' with { background: 'gray', color: 'white' } %}
```

** - PHP**

```php
use Grav\Common\Grav;
use Grav\Framework\ContentBlock\HtmlBlock;
use Grav\Framework\Flex\Interfaces\FlexCollectionInterface;

$page = 2;
$limit = 10;
$start = ($page-1)*$limit;

/** @var FlexCollectionInterface|null $collection */
$collection = Grav::instance()->get('flex')->getCollection('contacts');
if ($collection) {

    $collection = $collection->limit($start, $limit);

    /** @var HtmlBlock $block */
    $block = $collection->render('cards', ['background' =>'gray', 'color' => 'white']);

}
```

<h1 class="rem" id="Manipulation de collection">Manipulation de collection.
<a href="#Manipulation de collection" class="toc-anchor after"></a></h1> 

Toutes ces méthodes renvoient **une copie modifiée** de la collection. La collection originale reste inchangée.

<h2 id="sort()">sort().
<a href="#sort()" class="toc-anchor after"></a></h2>

`sort( orderings ): Collection` Trie la collection par liste paires de `property : direction`.

Paramètres:

* **orderings** Ordres Paires de `property : direction` où la direction est soit 'ASC' soit 'DESC' (`array`)

Retour:

* [Collection](avance-flex-collection.md) (`object`) Nouvelle instance triée de la collection

<div class="notice note">
<strong>ASTUCE</strong> : L'ordre de tri par défaut peut être défini pour le frontend dans les blueprints <strong>Flex Type</strong>.
</div>

** - Twig**
```console
{% set contacts = grav.get('flex').collection('contacts') %}

{% set contacts = contacts.sort({last_name: 'ASC', first_name: 'ASC'}) %}

<div>Displaying all contacts in alphabetical order:</div>
{% render contacts layout: 'cards' %}
```

** - PHP**
```php
use Grav\Common\Grav;
use Grav\Framework\Flex\Interfaces\FlexCollectionInterface;

/** @var FlexCollectionInterface|null $collection */
$collection = Grav::instance()->get('flex')->getCollection('contacts');
if ($collection) {

    $collection = $collection->sort(['last_name' => 'ASC', 'first_name' => 'ASC']);
    // Collection has now be sorted by last name, first name...

}
```

<h2 id="limit()">limit().
<a href="#limit()" class="toc-anchor after"></a></h2>

`limit( start, limit ): Collection` Renvoie un sous-ensemble de la collection à partir de `start` et n'incluant qu'un nombre `limit` d'objets.

Paramètres:

* **start** Indice de départ à partir de 0 (`int)
* **limit** Nombre maximal d'objets (`int`)

Retour:

* [Collection](avance-flex-collection.md) (`object`) Nouvelle instance filtrée de la collection

**- Twig**
```console
{% set contacts = grav.get('flex').collection('contacts') %}
{% set page = 3 %}
{% set limit = 6 %}
{% set start = (page - 1) * limit %}

{% set contacts = contacts.limit(start, limit) %}

<div>Displaying page {{ page|e }}:</div>
{% render contacts layout: 'cards' %}
```

** - PHP**
```php
use Grav\Common\Grav;
use Grav\Framework\Flex\Interfaces\FlexCollectionInterface;

$start = 0;
$limit = 6;

/** @var FlexCollectionInterface|null $collection */
$collection = Grav::instance()->get('flex')->getCollection('contacts');
if ($collection) {

    $collection = $collection->limit($start, $limit);
    // Collection contains only the objects in the current page...

}
```

<h2 id="filterBy()">filterBy().
<a href="#filterBy()" class="toc-anchor after"></a></h2>

`filterBy( filter ): Collection` Filtrer la collection par liste de paires `property : value`.

Paramètres:

* **filter** Paires de èproperty : value` qui sont utilisées pour filtrer la collection (`array`)

Retour:

* [Collection](avance-flex-collection.md) (`object`) Nouvelle instance filtrée de la collection

<div class="notice note">
<strong>ASTUCE</strong> : le filtrage par défaut peut être défini pour le frontend dans les blueprints <strong>Flex Type</strong>.
</div>

**- Twig**
```console
{% set contacts = grav.get('flex').collection('contacts') %}

{% set contacts = contacts.filterBy({'published': true}) %}

<div>Displaying only published contacts:</div>
{% render contacts layout: 'cards' %}
```

** - PHP**
```php
use Grav\Common\Grav;
use Grav\Framework\Flex\Interfaces\FlexCollectionInterface;

$start = 0;
$limit = 6;

/** @var FlexCollectionInterface|null $collection */
$collection = Grav::instance()->get('flex')->getCollection('contacts');
if ($collection) {

    $collection = $collection->filterBy(['published' => true]);
    // Collection contains only published objects...

}
```

<h2 id="reverse()">reverse().
<a href="#reverse()" class="toc-anchor after"></a></h2>

`reverse() : Collection` Inverse l'ordre des objets dans la Collection.

Retour:

* [Collection](avance-flex-collection.md) (`object`) Nouvelle instance inversée de la collection

<div class="notice note">
<strong>ASTUCE</strong> : si vous utilisez <code>sort()</code>, il est recommandé d'inverser l'ordre car cela permet d'économiser une étape supplémentaire.
</div>

**- Twig**
```console
{% set contacts = grav.get('flex').collection('contacts') %}

{% set contacts = contacts.reverse() %}

<div>Displaying contacts in reverse ordering:</div>
{% render contacts layout: 'cards' %}
```

** - PHP**
```php
use Grav\Common\Grav;
use Grav\Framework\Flex\Interfaces\FlexCollectionInterface;

$start = 0;
$limit = 6;

/** @var FlexCollectionInterface|null $collection */
$collection = Grav::instance()->get('flex')->getCollection('contacts');
if ($collection) {

    $collection = $collection->reverse();
    // Collection is now in reverse ordering...

}
```

<h2 id="shuffle()">shuffle().
<a href="#shuffle()" class="toc-anchor after"></a></h2>

`shuffle() : Collection` Mélange les objets dans un ordre aléatoire.

Retour:

* [Collection](avance-flex-collection.md) (`object`)  Nouvelle instance ordonnée au hasard de la collection.

**- Twig**
```console
{% set contacts = grav.get('flex').collection('contacts') %}

{% set contacts = contacts.shuffle().limit(0, 6) %}

<div>Displaying 6 random contacts:</div>
{% render contacts layout: 'cards' %}
```

** - PHP**
```php
use Grav\Common\Grav;
use Grav\Framework\Flex\Interfaces\FlexCollectionInterface;

/** @var FlexCollectionInterface|null $collection */
$collection = Grav::instance()->get('flex')->getCollection('contacts');
if ($collection) {

    $collection = $collection->shuffle()->limit(0,6);
    // Collection contains 6 random contacts...

}
```

<h2 id="select()">select().
<a href="#select()" class="toc-anchor after"></a></h2>

`select( keys ): Collection` Sélectionne des objets (par leurs clés) dans la collection.

Paramètres:

* **keys** Liste des clés d'objets utilisées pour sélectionner les objets (`array`)

Retour:

* [Collection](avance-flex-collection.md) (`object`) Nouvelle instance de la collection.

**- Twig**
```console
{% set contacts = grav.get('flex').collection('contacts') %}
{% set selected = ['gizwsvkyo5xtms2s', 'gjmva53uoncdo4sb', 'mfzwwtcugv5hkocd', 'k5nfctkeoftwi4zu'] %}

{% set contacts = contacts.select(selected) %}

<div>Displaying 4 selected contacts:</div>
{% render contacts layout: 'cards' %}
```

** - PHP**
```php
use Grav\Common\Grav;
use Grav\Framework\Flex\Interfaces\FlexCollectionInterface;

$selected = ['gizwsvkyo5xtms2s', 'gjmva53uoncdo4sb', 'mfzwwtcugv5hkocd', 'k5nfctkeoftwi4zu'];

/** @var FlexCollectionInterface|null $collection */
$collection = Grav::instance()->get('flex')->getCollection('contacts');
if ($collection) {

    $collection = $collection->select($selected);
    // Collection now contains the 4 selected contacts...

}
```

<h2 id="unselect()">unselect().
<a href="#unselect()" class="toc-anchor after"></a></h2>

`unselect( keys ): Collection` Supprime les objets (par leurs clés) de la collection.

Paramètres:

* **keys** Liste des clés d'objet utilisées pour supprimer les objets (`array`)

Retour:

* [Collection](avance-flex-collection.md) (`object`) Nouvelle instance de la collection.

**- Twig**
```console
{% set contacts = grav.get('flex').collection('contacts') %}
{% set ignore = ['gizwsvkyo5xtms2s', 'gjmva53uoncdo4sb', 'mfzwwtcugv5hkocd', 'k5nfctkeoftwi4zu'] %}

{% set contacts = contacts.unselect(ignore) %}

<div>Displaying all but 4 ignored contacts:</div>
{% render contacts layout: 'cards' %}
```

** - PHP**
```php
use Grav\Common\Grav;
use Grav\Framework\Flex\Interfaces\FlexCollectionInterface;

$ignore = ['gizwsvkyo5xtms2s', 'gjmva53uoncdo4sb', 'mfzwwtcugv5hkocd', 'k5nfctkeoftwi4zu'];

/** @var FlexCollectionInterface|null $collection */
$collection = Grav::instance()->get('flex')->getCollection('contacts');
if ($collection) {

    $collection = $collection->unselect($ignore);
    // Collection now contains all but 4 ignored contacts...

}
```

<h2 id="search()">search().
<a href="#search()" class="toc-anchor after"></a></h2>

`search( string, [properties], [options] ): Collection` Recherche une chaîne dans la collection.

Paramètres:

* **string** Chaîne de recherche (`string`)
* **property** Propriétés à rechercher, si null (ou non fournies), utilisez les valeurs par défaut (`array` ou `null`)
* **options** Options supplémentaires utilisées lors de la recherche (`array`)
    * commence_par : `bool`
    * se termine par : `bool`
    * contient : `bool`
    * sensible à la casse : `bool`

Retour:

* [Collection](avance-flex-collection.md) (`object`) Nouvelle instance filtrée de la collection.

**- Twig**
```console
{% set contacts = grav.get('flex').collection('contacts') %}

{% set contacts = contacts.search('Jack', ['first_name', 'last_name', 'email'], {'contains': true}) %}

<div>Displaying all search results for 'Jack':</div>
{% render contacts layout: 'cards' %}
```

** - PHP**
```php
use Grav\Common\Grav;
use Grav\Framework\Flex\Interfaces\FlexCollectionInterface;

/** @var FlexCollectionInterface|null $collection */
$collection = Grav::instance()->get('flex')->getCollection('contacts');
if ($collection) {

    $collection = $collection->search('Jack', ['first_name', 'last_name', 'email'], ['contains' => true]);
    // Collection now contains all search results...

}
```

<h2 id="copy()">copy().
<a href="#copy()" class="toc-anchor after"></a></h2>

`copy() : Collection` Crée une copie de la collection en clonant tous les objets de la collection.

Retour:

* [Collection](avance-flex-collection.md) (`object`)  Nouvelle instance de la collection, maintenant avec des objets clonés.

<div class="notice warning">
<strong>ATTENTION</strong> : Si vous modifiez des objets dans votre collection, vous devez toujours utiliser des copies !
</div>

**- Twig**
```console
{% set contacts = grav.get('flex').collection('contacts') %}

{% set contacts = contacts.shuffle().limit(0, 10) %}
{% set fakes = contacts.copy() %}

{% do fakes.setProperty('first_name', 'JACK') %}

<h2>Fake cards</h2>
{% render fakes layout: 'cards' %}

<h2>Original cards</h2>
{% render contacts layout: 'cards' %}
```

** - PHP**
```php
use Grav\Common\Grav;
use Grav\Framework\Flex\Interfaces\FlexCollectionInterface;

/** @var FlexCollectionInterface|null $collection */
$collection = Grav::instance()->get('flex')->getCollection('contacts');
if ($collection) {

    $collection = $collection->search('Jack', ['first_name', 'last_name', 'email'], ['contains' => true]);
    // Collection now contains all search results...

}
```

<h1 id="Itérer dans la collection">Itérer dans la collection.
<a href="#Itérer dans la collection" class="toc-anchor after"></a></h1>

Les **Collections** peuvent être itérées.

**- Twig**
```console
{% set contacts = grav.get('flex').collection('contacts') %}

<h2>All contacts:</h2>
<ul>
  {% for contact in contacts %}
    <li>{{ contact.first_name|e }} {{ contact.last_name|e }}</li>
  {% endfor %}
</ul>
```

** - PHP**
```php
use Grav\Common\Grav;
use Grav\Framework\Flex\Interfaces\FlexCollectionInterface;
use Grav\Framework\Flex\Interfaces\FlexObjectInterface;

/** @var FlexCollectionInterface|null $collection */
$collection = Grav::instance()->get('flex')->getCollection('contacts');
if ($collection) {

    /** @var FlexObjectInterface $object */
    foreach ($collection as $object) {
        // Do something with the object...
    }

}
```

<h2 id="first()">first().
<a href="#first()" class="toc-anchor after"></a></h2>

`first() : Object | false` Définit l'itérateur sur le premier objet de la collection et renvoie cet objet.

Retour:

* [Collection](avance-flex-collection.md) (`object`) Premier objet
* **false** Aucun objet dans la collection.

**- Twig**
```console
{% set contacts = grav.get('flex').collection('contacts') %}

{% set contact = contacts.first() %}

{% if contact %}
    <h2>First contact:</h2>
    <div>{{ contact.first_name|e }} {{ contact.last_name|e }}</div>
{% endif %}
</ul>
```

** - PHP**
```php
use Grav\Common\Grav;
use Grav\Framework\Flex\Interfaces\FlexCollectionInterface;
use Grav\Framework\Flex\Interfaces\FlexObjectInterface;

/** @var FlexCollectionInterface|null $collection */
$collection = Grav::instance()->get('flex')->getCollection('contacts');
if ($collection) {

    /** @var FlexObjectInterface|false $object */
    $object = $collection->first();
    if ($object) {
        // Do something with the object...
    }

}
```

<h2 id="last()">last().
<a href="#last()" class="toc-anchor after"></a></h2>

`last() : Object | false` Définit l'itérateur sur le dernier objet de la collection et renvoie cet objet.

Retour:

* [Collection](avance-flex-collection.md) (`object`) Dernier objet
* **false** Aucun objet dans la collection.

**- Twig**
```console
{% set contacts = grav.get('flex').collection('contacts') %}

{% set contact = contacts.last() %}

{% if contact %}
    <h2>Last contact:</h2>
    <div>{{ contact.first_name|e }} {{ contact.last_name|e }}</div>
{% endif %}
```

** - PHP**
```php
use Grav\Common\Grav;
use Grav\Framework\Flex\Interfaces\FlexCollectionInterface;
use Grav\Framework\Flex\Interfaces\FlexObjectInterface;

/** @var FlexCollectionInterface|null $collection */
$collection = Grav::instance()->get('flex')->getCollection('contacts');
if ($collection) {

    /** @var FlexObjectInterface|false $object */
    $object = $collection->last();
    if ($object) {
        // Do something with the object...
    }

}
```

<h2 id="next()">next().
<a href="#next()" class="toc-anchor after"></a></h2>

`next() : object | false` Déplace la position de l'itérateur vers l'objet suivant et renvoie cet élément.

Retour:

* [Collection](avance-flex-collection.md) (`object`) Objet suivant
* **false** Plus d'objets dans la collection.

**- Twig**
```console
{% set contacts = grav.get('flex').collection('contacts') %}
{% set first = contacts.first() %}
...

{% set contact = contacts.next() %}

{% if contact %}
    <h2>Next contact is:</h2>
    <div>{{ contact.first_name|e }} {{ contact.last_name|e }}</div>
{% endif %}
```

** - PHP**
```php
use Grav\Common\Grav;
use Grav\Framework\Flex\Interfaces\FlexCollectionInterface;
use Grav\Framework\Flex\Interfaces\FlexObjectInterface;

/** @var FlexCollectionInterface|null $collection */
$collection = Grav::instance()->get('flex')->getCollection('contacts');
if ($collection) {

    /** @var FlexObjectInterface|false $object */
    while ($object = $collection->next()) {
        // Do something with the object...
    }

}
```

<h2 id="current()">current().
<a href="#current()" class="toc-anchor after"></a></h2>

`current() : object | false` Obtient l'objet de la collection à la position actuelle de l'itérateur.

Retour:

* [Collection](avance-flex-collection.md) (`object`) Objet courant
* **false** Plus d'objets dans la collection.

**- Twig**
```console
{% set contacts = grav.get('flex').collection('contacts') %}
{% do contacts.next() %}
{% do contacts.next() %}
...

{% set contact = contacts.current() %}

{% if contact %}
    <h2>Current contact is:</h2>
    <div>{{ contact.first_name|e }} {{ contact.last_name|e }}</div>
{% endif %}
```

** - PHP**
```php
use Grav\Common\Grav;
use Grav\Framework\Flex\Interfaces\FlexCollectionInterface;
use Grav\Framework\Flex\Interfaces\FlexObjectInterface;

/** @var FlexCollectionInterface|null $collection */
$collection = Grav::instance()->get('flex')->getCollection('contacts');
if ($collection) {
    while ($collection->next()) {

        /** @var FlexObjectInterface|false $object */
        $object = $collection->current();
        // Do something with the object...

    }
}
```

<h2 id="key()">key().
<a href="#key()" class="toc-anchor after"></a></h2>

key`() : key | null` Obtient la clé de l'objet à la position actuelle de l'itérateur.

Retour:

* **key** (`string`) Clé d'objet
* `null` Plus d'objets dans la collection.

**- Twig**
```console
{% set contacts = grav.get('flex').collection('contacts') %}
{% do contacts.next() %}
{% do contacts.next() %}
...

{% set key = contacts.key() %}

{% if key %}
    Current contact key is: <strong>{{ key|e }}</strong>
{% endif %}
```

** - PHP**
```php
use Grav\Common\Grav;
use Grav\Framework\Flex\Interfaces\FlexCollectionInterface;

/** @var FlexCollectionInterface|null $collection */
$collection = Grav::instance()->get('flex')->getCollection('contacts');
if ($collection) {
    while ($collection->next()) {

        $key = $collection->key();
        // Do something with the key...

    }
}
```

<h1 id="Obtenir objet/clé">Obtenir objet/clé.
<a href="#Obtenir objet/clé" class="toc-anchor after"></a></h1>

<h2 id="Accès au tableau">Accès au tableau.
<a href="#Accès au tableau" class="toc-anchor after"></a></h2>

Les **Collections** sont accessibles comme des tableaux associatifs ou des cartes.

<div class="notice tip">
<strong>REMARQUE</strong> : <code>null</code> est renvoyé si l'objet avec la clé donnée n'est pas dans la collection.
</div>

**- Twig**
```console
{% set contacts = grav.get('flex').collection('contacts') %}

{% set contact = contacts['ki2ts4cbivggmtlj']

{# Do something #}
{% if contact %}
  {# Got Bruce Day #}
  Email for {{ contact.first_name|e }} {{ contact.last_name|e }} is {{ contact.email|e }}
{% else %}
  Oops, contact has been removed!
{% endif %}
```

** - PHP**
```php
use Grav\Common\Grav;
use Grav\Framework\Flex\Interfaces\FlexCollectionInterface;
use Grav\Framework\Flex\Interfaces\FlexObjectInterface;

/** @var FlexCollectionInterface|null $collection */
$collection = Grav::instance()->get('flex')->getCollection('contacts');
if ($collection) {

    /** @var FlexObjectInterface|null $object */
    $object = $collection['ki2ts4cbivggmtlj'];
    if ($object) {
        // Object exists, do something with it...
    }

}
```

<h2 id="get()">get().
<a href="#current()" class="toc-anchor after"></a></h2>

`get( clé ): Object | null` Obtient l'objet avec la clé spécifiée.

Paramètres:

* **key** Clé d'objet (`string`)

Retour:

* **Object** (`object`)
* `null` L'objet avec la clé donnée n'est pas dans la collection.

Alternative à [l'accès aux tableaux](#Accès au tableau).

**- Twig**
```console
{% set contacts = grav.get('flex').collection('contacts') %}

{% set contact = contacts.get('ki2ts4cbivggmtlj')

{# Do something #}
{% if contact %}
  {# Got Bruce Day #}
  Email for {{ contact.first_name|e }} {{ contact.last_name|e }} is {{ contact.email|e }}
{% else %}
  Oops, contact has been removed!
{% endif %}
```

** - PHP**
```php
use Grav\Common\Grav;
use Grav\Framework\Flex\Interfaces\FlexCollectionInterface;
use Grav\Framework\Flex\Interfaces\FlexObjectInterface;

/** @var FlexCollectionInterface|null $collection */
$collection = Grav::instance()->get('flex')->getCollection('contacts');
if ($collection) {

    /** @var FlexObjectInterface|null $object */
    $object = $collection->get('ki2ts4cbivggmtlj');
    if ($object) {
        // Object exists, do something with it...
    }

}
```

<h1 id="Collection en tant que tableau">Collection en tant que tableau.
<a href="#Collection en tant que tableau" class="toc-anchor after"></a></h1>

<h2 id="getKeys()">getKeys().
<a href="#getKeys()" class="toc-anchor after"></a></h2>

`getKeys() : array` Récupère toutes les clés de la collection.

Retour:

* `array` Liste des clés

**- Twig**
```console
{% set contacts = grav.get('flex').collection('contacts') %}

{% set keys = contacts.keys() %}

Keys are: {{ keys|join(', ')|e }}
```

** - PHP**
```php
use Grav\Common\Grav;
use Grav\Framework\Flex\Interfaces\FlexCollectionInterface;

/** @var FlexCollectionInterface|null $collection */
$collection = Grav::instance()->get('flex')->getCollection('contacts');
if ($collection) {

    /** @var string[] $keys */
    $keys = $collection->getKeys();
    $keysList = implode(', ', $keys);

}
```

<h2 id="GetObjectKeys()">GetObjectKeys().
<a href="#GetObjectKeys()" class="toc-anchor after"></a></h2>

`GetObjectKeys() : array` Alias ​​de la méthode `getKeys().`

Retour:

* `array` Liste des clés

<h2 id="getValues()">getValues().
<a href="#getValues()" class="toc-anchor after"></a></h2>

`getValues() : array` Récupère tous les objets de la collection.

Convertit la collection en tableau. Les clés ne sont pas conservées.

Retour:

* Liste des **Objects** (`array`)

**- Twig**
```console
{% set contacts = grav.get('flex').collection('contacts') %}

{% set list = contacts.values() %}
<ol>
{% for i,object in list %}
    <li>#{{ (i+1)|e }}: {{ object.email|e }}</li>
{% endfor %}
</ol>
```

** - PHP**
```php
use Grav\Common\Grav;
use Grav\Framework\Flex\Interfaces\FlexCollectionInterface;
use Grav\Framework\Flex\Interfaces\FlexObjectInterface;

/** @var FlexCollectionInterface|null $collection */
$collection = Grav::instance()->get('flex')->getCollection('contacts');
if ($collection) {

    /** @var FlexObjectInterface[] $objects */
    $objects = $collection->getValues();
    foreach ($objects as $pos => $object) {
        // Do something with the object and its position...
    }

}
```

<h2 id="toArray()">toArray().
<a href="#toArray()" class="toc-anchor after"></a></h2>

`toArray() : array` Obtient une représentation PHP native de tableau de la collection.

Similaire à `getValues()` mais préserve les clés.

Retour:

* `array` Liste de paires `key : Objects`.

**- Twig**
```console
{% set contacts = grav.get('flex').collection('contacts') %}

{% set list = contacts.toArray() %}
<ol>
{% for key,object in list %}
    <li>ID: {{ key|e }}: {{ object.email|e }}</li>
{% endfor %}
</ol>
```

** - PHP**
```php
use Grav\Common\Grav;
use Grav\Framework\Flex\Interfaces\FlexCollectionInterface;
use Grav\Framework\Flex\Interfaces\FlexObjectInterface;

/** @var FlexCollectionInterface|null $collection */
$collection = Grav::instance()->get('flex')->getCollection('contacts');
if ($collection) {

    /** @var array<string, FlexObjectInterface> $objects */
    $objects = $collection->toArray();
    foreach ($objects as $key => $object) {
        // Do something with the object and its key...
    }

}
```

<h2 id="slice()">slice().
<a href="#slice()" class="toc-anchor after"></a></h2>

`slice( offset, length ): array` Extrait une tranche de `lenght` éléments  commençant à la position `offset` de la Collection.

Paramètres:

* **offset** Décalage de départ à partir de 0 (int).
* **lenght** Nombre maximal d'objets (int).

Retour:

* `array` Liste de paires `key : Objects`.

<div class="notice note">
<strong>CONSEIL</strong> : Cette méthode peut être utilisée pour la pagination.
</div>

**- Twig**
```console
{% set contacts = grav.get('flex').collection('contacts') %}

{% set list = contacts.slice(10, 5) %}

<div>Displaying 5 emails starting from offset 10:</div>
<ol>
{% for key,object in list %}
    <li>ID: {{ key|e }}: {{ object.email|e }}</li>
{% endfor %}
</ol>
```

** - PHP**
```php
use Grav\Common\Grav;
use Grav\Framework\Flex\Interfaces\FlexCollectionInterface;
use Grav\Framework\Flex\Interfaces\FlexObjectInterface;

/** @var FlexCollectionInterface|null $collection */
$collection = Grav::instance()->get('flex')->getCollection('contacts');
if ($collection) {

    /** @var array<string, FlexObjectInterface> $objects */
    $objects = $collection->slice(10, 5);

    // Do something with the object and its key...

}
```

<h2 id="chunk()">chunk().
<a href="#chunk()" class="toc-anchor after"></a></h2>

`chunk( size ): array` Divise la collection en morceaux d'objets de taille.

Paramètres:

* `size` Taille des morceaux (int).

Retour:

* `array` Liste bidimensionnelle de paires `key : Objects`.

<div class="notice note">
<strong>CONSEIL</strong> : Cette méthode peut être utilisée pour diviser le contenu en lignes et en colonnes.
</div>

**- Twig**
```console
{% set contacts = grav.get('flex').collection('contacts') %}

{% set columns = contacts.limit(0, 10).chunk(5) %}

<div>Displaying two columns of 5 emails each:</div>
<div class="columns">
{% for column,list in columns %}
    <div class="column">
    {% for object in list %}
        <div>{{ object.email|e }}</div>
    {% endfor %}
    </div>
{% endfor %}
</div>
```

** - PHP**
```php
use Grav\Common\Grav;
use Grav\Framework\Flex\Interfaces\FlexCollectionInterface;
use Grav\Framework\Flex\Interfaces\FlexObjectInterface;

/** @var FlexCollectionInterface|null $collection */
$collection = Grav::instance()->get('flex')->getCollection('contacts');
if ($collection) {

    /** @var array $columns */
    $columns = $collection->limit(0, 10)->chunk(5);
    /** @var
        int $column
        array<string, FlexObjectInterface> $objects
    */
    foreach ($columns as $column => $objects) {
        // Do something with the objects...
    }
}

}
```

<h2 id="group()">group().
<a href="#group()" class="toc-anchor after"></a></h2>

`group( property ): array` Regrouper les objets de la collection par une propriété et les renvoyer sous forme de tableau associé.

Paramètres:

* **property** Nom de la propriété utilisée pour regrouper les objets (`string`).

Retour:

* `array` Liste bidimensionnelle de paires `key : Objects` où la valeur de la propriété est la clé du premier niveau

**- Twig**
```console
{% set contacts = grav.get('flex').collection('contacts') %}

{% set by_name = contacts.sort({last_name: 'ASC', first_name: 'ASC'}).group('last_name') %}

<div>Displaying contacts grouped by last name:</div>
<div>
{% for last_name,list in by_name %}
    {{ last_name|e }}:
    <ul>
    {% for object in list %}
        <li>{{ object.first_name|e }}</li>
    {% endfor %}
    </ul>
{% endfor %}
</div>
```

** - PHP**
```php
use Grav\Common\Grav;
use Grav\Framework\Flex\Interfaces\FlexCollectionInterface;
use Grav\Framework\Flex\Interfaces\FlexObjectInterface;

/** @var FlexCollectionInterface|null $collection */
$collection = Grav::instance()->get('flex')->getCollection('contacts');
if ($collection) {

    /** @var array $byName */
    $byName = $collection->group('last_name');
    /** @var
        string $lastName
        array<string, FlexObjectInterface> $objects
    */
    foreach ($byName as $lastName => $objects) {
        // Do something with the objects...
    }
}

}
```

<h1 id="Ajout et suppression d'objets">Ajout et suppression d'objets.
<a href="#Ajout et suppression d'objets" class="toc-anchor after"></a></h1>

<h2 id="add()">add().
<a href="#add()" class="toc-anchor after"></a></h2>

`add( Object )` Ajoute un Object à la fin de la collection.

Paramètres:

* [Objet](avance-flex-collection.md) Objet à ajouter (`objet`).

<h2 id="remove()">remove().
<a href="#remove()" class="toc-anchor after"></a></h2>

`remove (key): Objet | null` Supprime l'élément à l'index spécifié de la collection.

Paramètres:

* **key** Clé de l'objet à supprimer (`object`).

Retour:

* [Objet](avance-flex-collection.md) Objet supprimé (`object`) ou `null` s'il n'a pas été trouvé

<h2 id="removeElement()">removeElement().
<a href="#removeElement()" class="toc-anchor after"></a></h2>

`removeElement( Object ): bool` Supprime l'objet spécifié de la collection, s'il est trouvé.

Paramètres:

* [Objet](avance-flex-collection.md) Objet à supprimer (`object`).

Retour:

* `true` si l'objet était dans la collection, `false` sinon.

<h2 id="clear()">clear().
<a href="#clear()" class="toc-anchor after"></a></h2>

`clear()` Efface la collection, supprimant tous les éléments.

<h1 id="Essais">Essais.
<a href="#Essais" class="toc-anchor after"></a></h1>

<h2 id="containsKey()">containsKey().
<a href="#containsKey()" class="toc-anchor after"></a></h2>

`containsKey( key ): bool` Vérifie si la collection contient un objet avec la clé spécifiée.

Paramètres:

* **key** Clé à tester (`string`)

Retour:

* `true` si l'objet est dans la collection, `false` sinon

<h2 id="contains()">contains().
<a href="#contains()" class="toc-anchor after"></a></h2>

`contains( object ): bool` Vérifie si un élément est contenu dans la collection.

Paramètres:

* [Objet](avance-flex-collection.md) Objet à tester (`object`).

Retour:

* `true` si l'objet est dans la collection, `false` sinon

<h2 id="indesOf()">indesOf().
<a href="#indesOf()" class="toc-anchor after"></a></h2>

`indexOf( objet ): string | false` Obtient l'index/la clé d'un objet donné.

Paramètres:

* [Objet](avance-flex-collection.md) Objet à tester (`object`).

Retour:

* `string` index/key de l'objet, `false` si l'objet n'a pas été trouvé.

<h2 id="isEmpty()">isEmpty().
<a href="#isEmpty()" class="toc-anchor after"></a></h2>

`isEmpty() : bool` Vérifie si la collection est vide (ne contient aucun objet).

Retour:

* `true` si la collection est vide, `false` sinon.

<h2 id="count()">count().
<a href="#count()" class="toc-anchor after"></a></h2>

`count') : int`

Retour:

* `int` Nombre d'objets dans la collection.

<h1 id="Actions groupées pour les objets">Actions groupées pour les objets.
<a href="#Actions groupées pour les objets" class="toc-anchor after"></a></h1>

<h2 id="hasProperty()">hasProperty().
<a href="#hasProperty()" class="toc-anchor after"></a></h2>

`hasProperty( property ): array` Renvoie une liste de paires `key:boolean` si l'objet avec key a une propriété définie ou non.

Paramètres:

* **property** Nom de la propriété (`string`).

Retour:

* `array` de paires de `key: bool`, où `key` est la clé de l'objet et `bool` est soit vrai soit faux.

<h2 id="getProperty()">getProperty().
<a href="#getProperty()" class="toc-anchor after"></a></h2>

`getProperty( property, default ): array` Renvoie une liste de `key: value` pour chaque objet.

Paramètres:

* **property** Nom de la propriété (`string`).

Retour:

* `array` de paires de `key: value`, où `key` est la clé de l'objet et `value` est la valeur de la propriété.

<h2 id="setProperty()">setProperty().
<a href="#setProperty()" class="toc-anchor after"></a></h2>

`setProperty( property, value ): Collection` Définit une nouvelle valeur pour la propriété de chaque objet de la collection.

Paramètres:

* **property** Nom de la propriété (`string`).
* **value**  Nouvelle valeur (`mixed`).

Retour:

* **Collection** (`object`) La collection pour enchaîner les appels de méthode.

<div class="notice warning">
<strong>AVERTISSEMENT</strong> : Cette méthode modifie les instances d'objet partagées entre toutes les collections, si ce n'est pas prévu, veuillez <a href="#copy().">copy </a> la collection avant d'utiliser cette méthode.
</div>

<h2 id="defProperty()">defProperty().
<a href="#defProperty()" class="toc-anchor after"></a></h2>

`defProperty( property, default ): Collection` Définit la valeur par défaut de la propriété pour chaque objet de la collection.

Paramètres:

* **property** Nom de la propriété (`string`).
* **default** Valeur par défaut (`mixed`).

Retour:

* **Collection** (`object`) La collection pour enchaîner les appels de méthode.

<div class="notice warning">
<strong>AVERTISSEMENT</strong> : Cette méthode modifie les instances d'objet partagées entre toutes les collections, si ce n'est pas prévu, veuillez <a href="#copy().">copy </a> la collection avant d'utiliser cette méthode.
</div>

<h2 id="unsetProperty()">unsetProperty().
<a href="#unsetProperty()" class="toc-anchor after"></a></h2>

`unsetProperty( property ): Collection` Supprime la valeur de la propriété pour chaque objet de la collection.

Paramètres:

* **property** Nom de la propriété (`string`).

Retour:

* **Collection** (`object`) La collection pour enchaîner les appels de méthode.

<div class="notice warning">
<strong>AVERTISSEMENT</strong> : Cette méthode modifie les instances d'objet partagées entre toutes les collections, si ce n'est pas prévu, veuillez <a href="#copy().">copy </a> la collection avant d'utiliser cette méthode.
</div>

<h2 id="call()">call().
<a href="#call()" class="toc-anchor after"></a></h2>

`call( method, arguments): array` Appelle une méthode pour chaque objet de la collection. Renvoie les résultats de chaque appel.

Paramètres:

* **method** Nom de la méthode (`string`).
* **arguments** Liste des arguments (`array`).

Retour:

* Liste de paires `key: result` (`array`)

<div class="notice warning">
<strong>AVERTISSEMENT</strong> : Si la méthode modifie l'objet, veuillez <a href="#copy().">copy </a> la collection avant d'utiliser cette méthode.
</div>

<h2 id="getTimestamps()">getTimestamps().
<a href="#getTimestamps()" class="toc-anchor after"></a></h2>

`getTimestamps() : array` Renvoie une liste de `key : timestamp` pour chaque objet.

Retour:

* Liste de paires `key : timestamp` où l'horodatage est un entier (`array`).

<h2 id="getStorageKeys()">getStorageKeys().
<a href="#getStorageKeys()" class="toc-anchor after"></a></h2>

`getStorageKeys() : array` Renvoie une liste de key: storage_key pour chaque objet.

Retour:

* Liste de paires `key : storage_key` (`array`).

<h2 id="getFlexKeys()">getFlexKeys().
<a href="#getFlexKeys()" class="toc-anchor after"></a></h2>

`getFlexKeys() : array` Renvoie une liste de `key : flex_key` pour chaque objet.

Retour:

* Liste de paires `key : flex_key` (`array`).

<h2 id="withKeyField()">withKeyField().
<a href="#withKeyField()" class="toc-anchor after"></a></h2>

`withKeyField( field ): Collection` Renvoie une nouvelle collection avec une clé différente.

Paramètres:

* **field** Champ clé (`string`)
    * 'key' : clé par défaut utilisée par le répertoire
    * 'storage_key' : clé de la couche de stockage
    * 'flex_key' : clé unique pouvant être utilisée sans connaître le répertoire

Retour:

* **Collection** (`object`) La collection, mais indexée avec la nouvelle clé.

<h1 id="Tests de fermeture (PHP uniquement)">Tests de fermeture (PHP uniquement).
<a href="#Tests de fermeture (PHP uniquement)" class="toc-anchor after"></a></h1>

<h2 id="exist()">exist().
<a href="#exist()" class="toc-anchor after"></a></h2>

`exists( Closure ): bool` Teste l'existence d'un objet qui satisfait le prédicat donné.

Paramètres:

* **Closure** Méthode utilisée pour tester chaque objet.

Retour:

* `bool` True si votre fonction de rappel renvoie true pour n'importe quel objet.

<h2 id="forAll()">forAll().
<a href="#forAll()" class="toc-anchor after"></a></h2>

`forAll( Closure ): bool` Teste si le prédicat donné est valable pour tous les objets de cette collection.

Paramètres:

* **Closure** Méthode utilisée pour tester chaque objet.

Retour:

* `bool` True si votre fonction de rappel renvoie true pour n'importe quel objet.

<h1 id="Filtrage de fermeture (PHP uniquement)">Filtrage de fermeture (PHP uniquement).
<a href="#Filtrage de fermeture (PHP uniquement)" class="toc-anchor after"></a></h1>

<h2 id="filter()">filter.
<a href="#filter()" class="toc-anchor after"></a></h2>

`filter( Closure ): Collection` Renvoie tous les objets de cette collection qui satisfont le prédicat.

L'ordre des éléments est conservé.

Paramètres:

* **Closure** Méthode utilisée pour tester un seul objet.

Retour:

* **Collection** (`object`) Nouvelle collection avec tous les objets pour lesquels votre fonction de rappel renvoie true.

<h2 id="map()">map().
<a href="#map()" class="toc-anchor after"></a></h2>

`map( Closure ): Collection` Applique la fonction donnée à chaque objet de la collection et renvoie une nouvelle collection avec les objets renvoyés par la fonction.

Paramètres:

* **Closure** Méthode utilisée pour tester un seul objet.

Retour:

* **Collection** (`object`) Nouvelle collection avec des objets renvoyés par la fonction de rappel.

<h2 id="collectionGroup()">collectionGroup().
<a href="#collectionGroup()" class="toc-anchor after"></a></h2>

`collectionGroup( property ): Collection[]` Regrouper les objets de la collection par un champ et les renvoyer sous forme de tableau associé de collections.

Paramètres:

* **property** (`string`) Propriété utilisée pour regrouper les objets.

Retour:

* `array` Plusieurs collections dans un tableau, key étant la valeur de la propriété.

<h2 id="matching()">matching().
<a href="#matching()" class="toc-anchor after"></a></h2>

`matching( Criteria ): Collection` Sélectionne tous les objets qui correspondent à l'expression et renvoie une nouvelle collection contenant ces objets.

Paramètres:

* [Criteria](https://www.doctrine-project.org/projects/doctrine-collections/en/1.6/expression-builder.html#expression-builder) Expression.

Retour:

* **Collection** (`object`) Nouvelle collection avec des objets correspondant aux critères.

<div class="notice tip">
<strong>ASTUCE</strong> : consultez la documentation de Doctrine pour le <a href="(https://www.doctrine-project.org/projects/doctrine-collections/en/1.6/expression-builder.html#expression-builder">générateur d'expressions</a> et les <a href="https://www.doctrine-project.org/projects/doctrine-collections/en/1.6/expressions.html#expressions">expressions</a>.
</div>

<h2 id="orderBy()">orderBy().
<a href="#orderBy()" class="toc-anchor after"></a></h2>

`orderBy( array ): Collection` Réorganise la collection par liste de paires propriété/valeur.

Paramètres:

* `array`

Retour:

* **Collection** (`object`) Nouvelle collection avec le nouvel ordre.

<h2 id="partition()">partition().
<a href="#partition()" class="toc-anchor after"></a></h2>

`partition( Closure ): array` Partitionne cette collection en deux collections selon un prédicat.

Les clés sont conservées dans les collections résultantes.

Paramètres:

* **Closue** Méthode utilisée pour partitionner un seul objet. Renvoie vrai ou faux.

Retour:

* `array` Objets partitionnés `[[a, b], [c, d, e]]`.

