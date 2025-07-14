<h1 class="rem">Répertoire Flex</h1>

<h2 id="getTitle()">getTitle().
<a href="#getTitle()" class="toc-anchor after"></a></h2> 

`getTitle()` : chaîne Récupère le titre du répertoire

Retour:

* `string` Titre

<div class="tabs">
   <div id="tabs1"><a href="#tabs1">Twig</a>
      <div class="content">
         <pre class="pre">
         
{% set directory = grav.get('flex').directory('contacts') %}

&lt;h2&gt;{{ directory.title|e }}&lt;/h2&gt;
         </pre>
      </div>
   </div>
   <div id="tabs2"><a href="#tabs2">PHP</a>
      <div class="content">  
         <pre class="pre">
use Grav\Common\Grav;
use Grav\Framework\Flex\Interfaces\FlexDirectoryInterface;

/** @var FlexDirectoryInterface|null $directory */
$directory = Grav::instance()->get('flex')->getDirectory('contacts');
if ($directory) {

    /** @var string $title */
    $title = $directory->getTitle();

}
         </pre>
      </div>
   </div>  
</div>

<br>
<br>

<h2 id="getDescription()">getDescription().
<a href="#getDescription()" class="toc-anchor after"></a></h2> 

`getDescription() : string` Récupère la description du répertoire

Retour:

* `string` Description

<div class="tabs">
   <div id="tabs3"><a href="#tabs3">Twig</a>
      <div class="content">
         <pre class="pre">
         
{% set directory = grav.get('flex').directory('contacts') %}

&lt;p&gt;{{ directory.description|e }}&lt;/p&gt;
         </pre>
      </div>
   </div>
   <div id="tabs4"><a href="#tabs4">PHP</a>
      <div class="content">  
         <pre class="pre">
use Grav\Common\Grav;
use Grav\Framework\Flex\Interfaces\FlexDirectoryInterface;

/** @var FlexDirectoryInterface|null $directory */
$directory = Grav::instance()->get('flex')->getDirectory('contacts');
if ($directory) {

    /** @var string $title */
    $description = $directory->getDescription();

}
         </pre>
      </div>
   </div>  
</div>

<br>
<br>

<h2 id="getObjet()">getObjet().
<a href="#getObjet()" class="toc-anchor after"></a></h2> 

`getObject( id ): Object | null` Obtient un objet, renvoie null s'il n'a pas été trouvé.

Paramètres:

* **id** ID de l'objet (`string`)

Retour:

* [Objet](avance-flex-objets-flex.md) (`object`)
* `null` Objet introuvable

<div class="tabs">
   <div id="tabs5"><a href="#tabs5">Twig</a>
      <div class="content-bis">
         <pre class="pre">
{% set directory = grav.get('flex').directory('contacts') %}

{% set contact = directory.object('ki2ts4cbivggmtlj') %}

{# Do something #}
{% if contact %}
  {# Got Bruce Day #}
  Email for {{ contact.first_name|e }} {{ contact.last_name|e }} is {{ contact.email|e }}
{% else %}
  Oops, contact has been removed!
{% endif %}
         </pre>
      </div>
   </div>
   <div id="tabs6"><a href="#tabs6">PHP</a>
      <div class="content-bis">  
         <pre class="pre">
use Grav\Common\Grav;
use Grav\Framework\Flex\Interfaces\FlexDirectoryInterface;
use Grav\Framework\Flex\Interfaces\FlexObjectInterface;

/** @var FlexDirectoryInterface|null $directory */
$directory = Grav::instance()->get('flex')->getDirectory('contacts');
if ($directory) {

    /** @var FlexObjectInterface|null $object */
    $object = $directory->getObject('ki2ts4cbivggmtlj');
    if ($object) {
        // Object exists, do something with it...
    }

}
         </pre>
      </div>
   </div>  
</div>

<br>
<br>
<br>
<br>


<div class="notice tip">
Vérifiez ce que vous pouvez faire avec <a href="../avance-flex-objets-flex">Flex Object</a>.
</div>

<h2 id="getCollection()">getCollection().
<a href="#getCollection()" class="toc-anchor after"></a></h2> 

`getCollection() : Collection` Récupère la collection, renvoie null si elle n'a pas été trouvée.

Retour:

* [Collection](avance-flex-collection.md) (`object`)

<div class="tabs">
   <div id="tabs7"><a href="#tabs7">Twig</a>
      <div class="content-bis">
         <pre class="pre">
{% set directory = grav.get('flex').directory('contacts') %}

{% set contacts = directory.collection() %}

{# Do something #}
&lt;h2&gt;Ten first contacts:&lt;/h2&gt;
&lt;ul&gt;
  {% for contact in contacts.filterBy({published: true}).limit(0, 10) %}
    &lt;li&gt;{{ contact.first_name|e }} {{ contact.last_name|e }}&lt;li&gt;
  {% endfor %}
&lt;/ul&gt;
         </pre>
      </div>
   </div>
   <div id="tabs8"><a href="#tabs8">PHP</a>
      <div class="content-bis">  
         <pre class="pre">
use Grav\Common\Grav;
use Grav\Framework\Flex\Interfaces\FlexDirectoryInterface;
use Grav\Framework\Flex\Interfaces\FlexCollectionInterface;

/** @var FlexDirectoryInterface|null $directory */
$directory = Grav::instance()->get('flex')->getDirectory('contacts');
if ($directory) {

    /** @var FlexCollectionInterface $collection */
    $collection = $directory->getCollection();

    // Do something with the collection...

}        </pre>
      </div>
   </div>  
</div>

<br>
<br>
<br>
<br>

<div class="notice tip">
Vérifiez ce que vous pouvez faire avec <a href="../avance-flex-collection">Flex Collection<a>.
</div>


