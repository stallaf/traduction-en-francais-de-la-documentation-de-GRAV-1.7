<h1 class="rem">Flex</h1>

<div class="notice tip">
<stong>ASTUCE</strong> : la liste complète des méthodes est disponible dans la section Personnalisation des objets Flex.
</div>

<h2 id="compter()">compter().
<a href="#compter()" class="toc-anchor after"></a></h2> 

`count() : int` Compte le nombre de répertoires enregistrés dans Flex.

Retour:

* `int` Nombre de [Répertoires](avance-flex-repertoire.md).

<div class="tabs">
   <div id="tabs1"><a href="#tabs1">Twig</a>
      <div class="content">
         <pre class="pre">
{% set flex = grav.get('flex') %}
         
Flex has {{ flex.count() }} enabled directories.
         </pre>
      </div>
   </div>
   <div id="tabs2"><a href="#tabs2">PHP</a>
      <div class="content">  
         <pre class="pre">
use Grav\Common\Grav;
use Grav\Framework\Flex\Interfaces\FlexInterface;

/** @var FlexInterface $flex */
$flex = Grav::instance()->get('flex');

/** @var int $count */
$count = $flex->count();</pre>
      </div>
   </div>  
</div>


<br>
<br>

<h2 id="getDirectories()">getDirectories().
<a href="#getDirectories()" class="toc-anchor after"></a></h2> 

`getDirectories( [names] ): array` Récupère la liste des répertoires.

Paramètres:

* **names** Facultatif : Liste des noms de répertoires (`array`).

Retour:

* `array` liste des [Répertoires](avance-flex-repertoire.md).

<div class="notice note">
<strong>ASTUCE</strong> : Si aucune liste de noms n'a été fournie, la méthode renvoie tous les répertoires enregistrés dans Flex.
</div>

<div class="tabs">
   <div id="tabs3"><a href="#tabs3">Twig</a>
      <div class="content-ter">
         <pre class="pre">
         
{% set flex = grav.get('flex') %}

{# Get all directories #}
{% set directories = flex.directories() %}

{# Get listed directories #}
{% set listed_directories = flex.directories(['contacts', 'phonebook']) %}

{# Do something with the directories #}

         </pre>
      </div>
   </div>
   <div id="tabs4"><a href="#tabs4">PHP</a>
      <div class="content-ter">  
         <pre class="pre">
use Grav\Common\Grav;
use Grav\Framework\Flex\Interfaces\FlexInterface;
use Grav\Framework\Flex\Interfaces\FlexDirectoryInterface;

/** @var FlexInterface $flex */
$flex = Grav::instance()->get('flex');

/** @var FlexDirectoryInterface[] $directories */
$directories = $flex->getDirectories();
// = ['contacts' => FlexDirectory, ...]

/** @var FlexDirectoryInterface[] $directories */
$listedDirectories = $flex->getDirectories(['contacts', 'phonebook']);
// = ['contacts' => FlexDirectory]

/** @var array<FlexDirectoryInterface|null> $directories */
$listedDirectoriesWithMissing = $flex->getDirectories(['contacts', 'phonebook'], true);
// = ['contacts' => FlexDirectory, 'phonebook' => null]
         </pre>
      </div>
   </div>  
</div>

<br>
<br>
<br>
<br>
<br>

<div class="notice note">
<strong>ASTUCE</strong> : Vous voudrez peut-être vous assurer de ne renvoyer que les répertoires souhaités.
</div>

<h2 id="hasDirectory()">hasDirectory().
<a href="#hasDirectory()" class="toc-anchor after"></a></h2> 

`hasDirectory( name ): bool` : Vérifie si le répertoire existe.

Paramètres:

* **name** Nom du répertoire (`string`)

Retour:

* `bool` Vrai si trouvé, faux sinon

<div class="tabs">
   <div id="tabs5"><a href="#tabs5">Twig</a>
      <div class="content">
         <pre class="pre">
{% set flex = grav.get('flex') %}

Flex has {{ not flex.hasDirectory('contacts') ? 'not' }} contacts directory.
         </pre>
      </div>
   </div>
   <div id="tabs6"><a href="#tabs6">PHP</a>
      <div class="content">  
         <pre class="pre">
use Grav\Common\Grav;
use Grav\Framework\Flex\Interfaces\FlexInterface;

/** @var FlexInterface $flex */
$flex = Grav::instance()->get('flex');

/** @var bool $exists */
$exists = $flex->hasDirectory('contacts');
         </pre>
      </div>
   </div>  
</div>

<br>
<br>

<h2 id="getDirectory()">getDirectory().
<a href="#getDirectory()" class="toc-anchor after"></a></h2> 

`getDirectory( name ): Directory | null` obtient un répertoire, renvoie null s'il n'a pas été trouvé.

Paramètres:

* **name** Nom du répertoire (`string`)

Retour:

* [Répertoires](avance-flex-repertoire.md) (`objet`)
* `null` Répertoire introuvable

<div class="tabs">
   <div id="tabs7"><a href="#tabs7">Twig</a>
      <div class="content-bis">
         <pre class="pre">

{% set flex = grav.get('flex') %}

{# Get contacts directory (null if not found) #}
{% set directory = flex.directory('contacts') %}

{# Do something with the contacts directory #}
         </pre>
      </div>
   </div>
   <div id="tabs8"><a href="#tabs8">PHP</a>
      <div class="content-bis">  
         <pre class="pre">
use Grav\Common\Grav;
use Grav\Framework\Flex\Interfaces\FlexInterface;
use Grav\Framework\Flex\Interfaces\FlexDirectoryInterface;

/** @var FlexInterface $flex */
$flex = Grav::instance()->get('flex');

/** @var FlexDirectoryInterface|null $directory */
$directory = $flex->getDirectory('contacts');
if ($directory) {
    // Directory exists, do something with it...
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
Vérifiez ce que vous pouvez faire avec [Flex Directory](avance-flex-repertoire.md).
</div>

<h2 id="getObjet()">getObjet().
<a href="#getObjet()" class="toc-anchor after"></a></h2> 

`getObject( id, répertoire ): Objet | null` Obtient un objet, renvoie null s'il n'a pas été trouvé.

Paramètres:

* **id** ID de l'objet (`string`)
* **directory** Nom du répertoire (`string`)

Retour:

* [Object](avance-flex-objets-flex.md) (`Object`)
* `null` Objet introuvable

<div class="tabs">
   <div id="tabs9"><a href="#tabs9">Twig</a>
      <div class="content-bis">
         <pre class="pre">
{% set flex = grav.get('flex') %}

{% set contact = flex.object('ki2ts4cbivggmtlj', 'contacts') %}

{# Do something #}
{% if contact %}
  {# Got Bruce Day #}
  {{ contact.first_name|e }} {{ contact.last_name|e }} has a website: {{ contact.website|e }}
{% else %}
  Oops, contact has been removed!
{% endif %}
         </pre>
      </div>
   </div>
   <div id="tabs10"><a href="#tabs10">PHP</a>
      <div class="content-bis">  
         <pre class="pre">
use Grav\Common\Grav;
use Grav\Framework\Flex\Interfaces\FlexInterface;
use Grav\Framework\Flex\Interfaces\FlexObjectInterface;

/** @var FlexInterface $flex */
$flex = Grav::instance()->get('flex');

/** @var FlexObjectInterface|null $object */
$object = $flex->getObject('ki2ts4cbivggmtlj', 'contacts');
if ($object) {
    // Object exists, do something with it...
}
         </pre>
      </div>
   </div>  
</div>

<br>
<br>
<br>
<br>

<div class="notice note">
Vérifiez ce que vous pouvez faire avec<a href="../avance-flex-objets-flex">Flex Object</a>.
</div>

<h2 id="getCollection()">getCollection().
<a href="#getCollection()" class="toc-anchor after"></a></h2> 

`getCollection( répertoire ): Collection | null` Obtient la collection, renvoie null si elle n'a pas été trouvée.

Paramètres:

* `directory` Nom du répertoire (`string`)

Retour:

* [Collection](avance-flex-collection.md) (`object`)
* `null` Répertoire introuvable

<div class="tabs">
   <div id="tabs11"><a href="#tabs11">Twig</a>
      <div class="content-bis">
         <pre class="pre">
{% set flex = grav.get('flex') %}

{% set contacts = flex.collection('contacts') %}

{# Do something #}
&lt;h2&gt;Ten random contacts:&lt;/h2&gt;
&lt;ul&gt;
  {% for contact in contacts.filterBy({published: true}).shuffle().limit(0, 10) %}
    &lt;li&gt;{{ contact.first_name|e }} {{ contact.last_name|e }}&lt;/li&gt;
  {% endfor %}
&lt;/ul&gt;
         </pre>
      </div>
   </div>
   <div id="tabs12"><a href="#tabs12">PHP</a>
      <div class="content-bis">  
         <pre class="pre">
use Grav\Common\Grav;
use Grav\Framework\Flex\Interfaces\FlexCollectionInterface;
use Grav\Framework\Flex\Interfaces\FlexInterface;

/** @var FlexInterface $flex */
$flex = Grav::instance()->get('flex');

/** @var FlexCollectionInterface|null $collection */
$collection = $flex->getCollection('contacts');
if ($collection) {
    // Collection exists, do something with it...
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
Vérifiez ce que vous pouvez faire avec <a href="../avance-flex-collection">Collection Flexible</a>.
</div>

