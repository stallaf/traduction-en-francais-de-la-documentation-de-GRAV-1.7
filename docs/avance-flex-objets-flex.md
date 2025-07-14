<h1 class="rem">Objet flexible</h1>

<h1 id="Objet de rendu">Objet de rendu.
<a href="#Objet de rendu" class="toc-anchor after"></a></h1>

<h2 id="render()">render().
<a href="#render()" class="toc-anchor after"></a></h2>

`render( [layout], [context] ): Block` Rend l'objet.

Paramètres:

* **layout** Nom de la mise en page (`string`).
* **context** Variables supplémentaires pouvant être utilisées dans le fichier de modèle Twig (`array`)

Retour:

* **Block** (`object`)  Classe HtmlBlock contenant la sortie.

<div class="notice tip">
<strong>REMARQUE</strong> : dans twig, il existe une balise <code>{% render %}</code>, qui doit être utilisée au lieu d'appeler directement la méthode. Cela permettra aux ressources JS/CSS de l'objet de fonctionner correctement.
</div>

** - Twig**
```console
{% set contact = grav.get('flex').object('gizwsvkyo5xtms2s', 'contacts') %}

{% render contact layout: 'details' with { my_variable: true } %}
```

** - PHP**
```php
use Grav\Common\Grav;
use Grav\Framework\ContentBlock\HtmlBlock;
use Grav\Framework\Flex\Interfaces\FlexObjectInterface;

/** @var FlexObjectInterface|null $collection */
$object = Grav::instance()->get('flex')->getObject('gizwsvkyo5xtms2s', 'contacts');
if ($object) {

    /** @var HtmlBlock $block */
    $block = $object->render('details', ['my_variable' => true]);

}
```

<h1 id="Other">Other.
<a href="#Other" class="toc-anchor after"></a></h1>

<h2 id="getKey()">getKey().
<a href="#getKey()" class="toc-anchor after"></a></h2>

`getKey() : `string` Récupère la clé de l'objet.

Retour:

* `string` Clé d'objet

hasKey()

`hasKey() : bool` Renvoie vrai si la clé de l'objet a été définie.

Retour:

* `true` si l'objet a une clé, `false` sinon

<h2 id="getFlexType()">getFlexType().
<a href="#getFlexType()" class="toc-anchor after"></a></h2>

`getFlexType() : string` Récupère le type de l'objet.

Retour:

* `string` Nom du répertoire Flex auquel appartient l'objet.

hasProperty()

`hasProperty( property ): bool` Renvoie vrai si la propriété de l'objet a été définie et a une valeur (non nulle).

Paramètres:

* **property** Nom de la propriété (`string`).

Retour:

* `true` si la propriété a une valeur, `false` sinon.

<h2 id="getProperty()">getProperty().
<a href="#getProperty()" class="toc-anchor after"></a></h2>

`getProperty( property, default ): mixed` Renvoie la valeur de la propriété de l'objet.

Paramètres:

* **property** Nom de la propriété (`string`).

Retour:

* `mixed Valeur du bien.
* `null` si la propriété n'est pas définie ou n'a pas de valeur.

<h2 id="setProperty()">setProperty().
<a href="#setProperty()" class="toc-anchor after"></a></h2>

`setProperty( property, value ): Object` Définit une nouvelle valeur pour la propriété de l'objet.

Paramètres:

* **property** Nom de la propriété (`string`).
* **value** Nouvelle valeur (`mixed`).

Retour:

* **Object** (`object`) L'objet pour enchaîner les appels de méthode.

<div class="notice warning">
<strong>ATTENTION</strong> : Cette méthode modifie l'instance d'objet partagée entre toutes les collections. Si ce n'est pas prévu, veuillez cloner <code>clone</code>l'objet avant d'utiliser cette méthode.
</div>

<h2 id="defProperty()">defProperty().
<a href="#defProperty()" class="toc-anchor after"></a></h2>

`defProperty( property, default ): Object` Définit la valeur par défaut de la propriété de l'objet.

Paramètres:

* **property** Nom de la propriété (`string`).
* **default** Valeur par défaut (`mixed`).

Retour:

* **Object** (`object`) L'objet pour enchaîner les appels de méthode.

<div class="notice warning">
<strong>ATTENTION</strong> : Cette méthode modifie l'instance d'objet partagée entre toutes les collections. Si ce n'est pas prévu, veuillez cloner <code>clone</code>l'objet avant d'utiliser cette méthode.
</div>

<h2 id="unsetProperty()">unsetProperty().
<a href="#unsetProperty()" class="toc-anchor after"></a></h2>

`unsetProperty( property ): Object` Supprime la valeur de la propriété de l'objet.

Paramètres:

* **property** Nom de la propriété (`string`).

Retour:

* **Object** (`object`) L'objet pour enchaîner les appels de méthode.

<div class="notice warning">
<strong>ATTENTION</strong> : Cette méthode modifie l'instance d'objet partagée entre toutes les collections. Si ce n'est pas prévu, veuillez cloner <code>clone</code>l'objet avant d'utiliser cette méthode.
</div>

<h2 id="isAuthorized()">isAuthorized().
<a href="#isAuthorized()" class="toc-anchor after"></a></h2>

`isAuthorized( action, [portée], [utilisateur] ): bool | null` Vérifie si l'utilisateur est autorisé pour l'action.

Paramètres:

* **action** (`string`)
    * L'un des suivants : `create`, `read`, `update`, `delete`, `list`.
* **scope** Facultatif (`string`)
    * Généralement `admin` ou `site`.
* **user** Objet utilisateur facultatif (`object`)

Retour:

* `true` Autoriser l'action.
* `false` Refuser l'action.
* `nullè Non défini (agit comme un refus)

<div class="notice note">
<strong>NOTE</strong> : Il existe deux valeurs de refus : refusé (faux), non défini (null). Cela permet d'enchaîner plusieurs règles lorsque les règles précédentes ne correspondent pas.
</div>

<h2 id="getFlexDirectory()">getFlexDirectory().
<a href="#getFlexDirectory()" class="toc-anchor after"></a></h2>

`getFlexDirectory() : Directory`

Retour:

* [Directory](avance-flex-repertoire.md) (`object`)

<h2 id="getTimestamp()">getTimestamp().
<a href="#getTimestamp()" class="toc-anchor after"></a></h2>

`getTimestamp() : int` Récupère le dernier horodatage modifié pour l'objet.

Retour:

* `int` Horodatage.

<h2 id="search()">search().
<a href="#search()" class="toc-anchor after"></a></h2>

`search(string, [properties], [options] ): float` Recherche une chaîne à partir de l'objet, renvoie un poids compris entre 0 et 1.

Paramètres:

* **string** Chaîne de recherche (`string`)
* **property** Propriétés à rechercher, si null (ou non fournies), utilisez les valeurs par défaut * **options** Options supplémentaires utilisées lors de la recherche (tableau)
    * commence_par : `bool`
    * se termine par : `bool`
    * contient : `bool`
    * sensible à la casse : `bool`

Retour:

* `float` Poids de la recherche entre 0 et 1, utilisé pour ordonner les résultats
* `0` L'objet ne correspond pas à la recherche.

<div class="notice note">
<strong>NOTE</strong> : Si vous remplacez cette fonction, assurez-vous de renvoyer une valeur comprise entre 0 et 1 !
</div>

<h2 id="getFlexKey()">getFlexKey().
<a href="#getFlexKey()" class="toc-anchor after"></a></h2>

`getFlexKey() : string` Récupère une clé unique pour l'objet.

Retour:

* `string` **Clé flexible** de l'objet

Les clés flexibles peuvent être utilisées sans connaître le répertoire auquel appartient l'objet.

<h2 id="getStorageKey()">getStorageKey().
<a href="#getStorageKey()" class="toc-anchor after"></a></h2>

`getStorageKey() : string` Récupère une clé de stockage unique (dans le répertoire) utilisée pour déterminer le nom du fichier ou l'identifiant de la base de données.

Retour:

* `string` Clé de stockage de l'objet.

<h2 id="exist()">exist().
<a href="#exist()" class="toc-anchor after"></a></h2>

`exists() : bool` Renvoie vrai si l'objet existe dans le stockage.

Retour:

* `true` L'objet existe dans le stockage.
* `false` L'objet n'a pas été enregistré.

<h2 id="update()">update().
<a href="#update()" class="toc-anchor after"></a></h2>

`update( data, files ): Object` Met à jour l'objet dans la mémoire.

Paramètres:

* **data** (`array`) Tableaux imbriqués de propriétés avec leurs valeurs.
* **files** (`array`) Tableau d'objets `Psr\Http\Message\UploadedFileInterface`.

Retour:

* **Object** (`object`) L'objet pour enchaîner les appels de méthode.

<div class="notice note">
<strong>NOTE</strong> : Vous devez enregistrer l'objet après avoir appelé cette méthode.
</div>

<h2 id="create()">create().
<a href="#create()" class="toc-anchor after"></a></h2>

`create( [key] ): Object` Crée un nouvel objet dans le stockage.

Paramètres:

* **key** (`string`) Clé facultative.

Retour:

* **Object** (`object`) Objet enregistré.

<h2 id="createCopy()">createCopy().
<a href="#createCopy()" class="toc-anchor after"></a></h2>

`createCopy( [key] ): Object` Crée un nouvel objet à partir de l'actuel et enregistre-le dans le stockage.

* **key** (`string`) Clé facultative.

Retour:

* **Object** (`object`) Objet enregistré.

<h2 id="save()">save().
<a href="#save()" class="toc-anchor after"></a></h2>

`save() : Object` Enregistrer l'objet dans le stockage.

Retour:

* **Object** (`object`) Objet enregistré.

<h2 id="delete()">delete().
<a href="#delete()" class="toc-anchor after"></a></h2>

`delete() : Object` Supprime l'objet du stockage.

Retour:

* **Object** (`object`) Objet supprimé.

<h2 id="getBlueprint()">getBlueprint().
<a href="#getBlueprint()" class="toc-anchor after"></a></h2>

`getBlueprint( [name] ): Blueprint` Renvoie le plan de l'objet.

Paramètres:

* **name** (`string`) Nom facultatif du blueprint.

Retour:

* **Blueprint** (`object`)

<h2 id="getForm()">getForm().
<a href="#getForm()" class="toc-anchor after"></a></h2>

`getForm( [name], [options] ): Form` Renvoie une instance de formulaire pour l'objet.

Paramètres:

* **name** (`string`) Nom facultatif du formulaire.
* **options** (`array`) Options facultatives du formulaire.

Retour:

* **Form** (`object`)

<h2 id="getDefaultValue()">getDefaultValue().
<a href="#getDefaultValue()" class="toc-anchor after"></a></h2>

`getDefaultValue( name, [separator] ): mixed` Renvoie la valeur par défaut pouvant être utilisée dans un formulaire pour la propriété donnée.

Paramètres:

* **name** (`string`) Nom de la propriété.
* **separator** (`string`) Caractère séparateur facultatif pour les propriétés imbriquées, par défaut `.` (dot)

Retour:

* `mixed` Valeur par défaut de la propriété.

<h2 id="getDefaultValues()">getDefaultValues().
<a href="#getDefaultValues()" class="toc-anchor after"></a></h2>

`getDefaultValues() : array` Renvoie les valeurs par défaut pouvant être utilisées dans un formulaire pour la propriété donnée.

Retour:

* `array` Toutes les valeurs par défaut.

<h2 id="getFormValue()">getFormValue().
<a href="#getFormValue()" class="toc-anchor after"></a></h2>

`getFormValue( name, [default], [separator] ): mixed` Renvoie une valeur brute pouvant être utilisée dans un formulaire pour la propriété donnée.

Paramètres:

* **name** (`string`) Nom de la propriété.
* **default**  `(`mixed) Valeur optionnelle par défaut du champ, `null` si non renseigné.
* **separator** (`string`) Caractère séparateur facultatif pour les propriétés imbriquées, par défaut `.` (dot)

Retour:

* |mixed` Valeur du champ du formulaire.

<h2 id="triggerEvent()">triggerEvent().
<a href="#triggerEvent()" class="toc-anchor after"></a></h2>

`triggerEvent( name, [Event] ): Object` Déclenche un événement de votre choix.

Paramètres:

* **name** (`string`) Nom de l'événement.
* **Event** (`object`) Classe d'événement facultative.

Retour:

* **Object** (`object`) L'objet pour enchaîner les appels de méthode.

