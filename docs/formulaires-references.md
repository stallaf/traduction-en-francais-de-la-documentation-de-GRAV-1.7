<h1 class="rem">Référence : Champs de formulaire Blueprint</h1>

<h2 id="Champs de formulaire disponibles pour une utilisation dans l'administration">Champs de formulaire disponibles pour une utilisation dans l'administration
<a href="#Champs de formulaire disponibles pour une utilisation dans l'administration" class="toc-anchor after"></a></h2>

Les formulaires côté administrateur peuvent être créés avec une variété de champs. Ce document fournit une référence complète des propriétés de chaque champ et fournit des exemples d'utilisation.

En plus des champs listés ci-dessous, réservés à une utilisation dans l'Admin, vous pouvez également utiliser tous les champs disponibles dans les formulaires frontend décrits dans [la Référence des champs des formulaires frontend](formulaires-ref-index-formulaires.md).

<h3 id="Champs de formulaire spéciaux disponibles exclusivement dans l'administration">Champs de formulaire spéciaux disponibles exclusivement dans l'administration
<a href="#Champs de formulaire spéciaux disponibles exclusivement dans l'administration" class="toc-anchor after"></a></h3>


| **CHAMPS**                         | **DESCRIPTION**
| ---------                          | ---------
| [Array](#Champ de tableau)         | utilisé pour créer des tableaux clé-valeur.
| **BackupHistory**                  |
| **Blueprint**                      |
| [Colorpicker](#Champ de sélecteur de couleurs) | affiche un sélecteur de couleurs.
| [Columns](#Colonnes / Champs de colonne)  | utilisées pour diviser le formulaire en plusieurs colonnes.
| [Column](#Colonnes / Champs de colonne)   | utilisée pour afficher une seule colonne (utilisée avec le champ `Colonnes`).
| **Cron**                          |
| **CronStatus**                    |
| [Dateformat](#Champ de format de date)    | une sélection spéciale qui rend la date/heure actuelle dans les formats passés.
| [Datetime](#Champ de format de date)      | un champ de sélection de date et d'heure.
| [Editor](#Champ de l'éditeur)     | afficher un éditeur de markdown.
| [Elements](#Champ d'éléments)   | un champ conditionnel et organisationnel pour afficher/masquer les enfants en <br>fonction de la valeur sélectionnée du "déclencheur". Ceci est extrêmement utile <br>pour réduire l'encombrement lorsqu'il y a beaucoup d'options à afficher.
| [Groupe de champs](#Champ d'ensemble de champs)  | un ensemble de champs à l'intérieur d'un accordéon pliable.
| [File](#Champ de fichier)      | dans Admin, **File** est spécialisé pour être utilisé dans les configurations de plugins <br>et de thèmes (blueprints). Gère le téléchargement d'un fichier vers un emplacement, <br>sa suppression et sa suppression de la configuration du thème / plugin.
| [Filepicker](#Champ de sélecteur de fichiers) | **Filepicker** permet de choisir des fichiers à partir d'un emplacement dans le <br>système de fichiers du serveur Web.
| **Frontmatter**                   | 
| **IconPicker**                    |
| [List](Champ de liste)            | utilisée pour créer des collections de champs.
| **Markdown**                      |
| **MediaPicker**                   |
| **Multilevel**                    |
| **Order**                         |
| **PageMedia**                     |
| [PageMediaSelect](#Champ PageMediaSelect) | affiche une sélection avec tous les médias de la page. Utilisé dans les plans Pages <br>pour permettre à l'utilisateur de choisir un fichier multimédia à affecter à un champ.
| [Pages](#Champ Pages)             |  affiche une liste des pages du site
| ** Parents**                      |
| **Permissions**                   |
| **Range**                         |
| [Section](#Champ de section)      | utilisée pour diviser une page de paramètres en sections ; chaque section est <br>accompagnée d'un titre
| [Selectize](#Champ de sélection)  | Sélectionnez un hybride d'une zone de texte et d'une zone de sélection. <br>Principalement utile pour le balisage et d'autres champs de sélection d'éléments.
| **SelectUnique **                 |
| [Taxonomie](#Champ de taxonomie)  | une sélection spéciale préconfigurée pour sélectionner une ou plusieurs <br> taxonomies
| **ThemeSelect**                   |
| **Userinfo**                      | 
| **Xss**                           |

<h2 id="Attributs des champs communs">Attributs des champs communs
<a href="#Attributs des champs communs" class="toc-anchor after"></a></h2>

Chaque champ accepte une liste d'attributs que vous pouvez utiliser. Chaque champ peut partager ces attributs communs, mais des champs particuliers peuvent les ignorer. La meilleure façon de vérifier quels attributs sont autorisés sur un champ est de vérifier la description du champ dans cette page et de voir quels attributs sont mentionnés.

Cette liste fournit un terrain d'entente, il n'est donc pas nécessaire de répéter la description d'un champ commun.

| **ATTRIBUTS**            | **DESCRIPTION**
| ---------                | ---------
| `autocomplete`           | la saisie semi-automatique accepte `on` ou `off`
| `autofocus`              | si activé, autofocus sur ce champ
| `classes`                | accepte une chaîne avec une ou plusieurs classes CSS à ajouter
| `default`                | définit la valeur par défaut du champ. Cela garantit que vous récupérerez toujours <br>soit une valeur spécifiée par l'utilisateur, soit cette valeur par défaut. Voir aussi <br>`placeholder`.
| `disabled`               | définit l'état désactivé du champ
| `help`                   | Ajoute une info-bulle au champ
| `id`                     | définit l'identifiant du champ ainsi que l'attribut `for` sur l'étiquette
| `label`                  | définit le libellé du champ
| `name`                   | définit le nom du champ
| `novalidate`             | définit l'état novalidate du champ
| `placeholder`            | définit la valeur de l'espace réservé du champ. Il s'agit de définir une valeur que <br>l'utilisateur peut voir comme une invite pour sa propre valeur, mais cela n'influence <br>pas la valeur finalement écrite. Voir aussi par `default`.
| `readonly`               | définit l'état du champ en lecture seule
| `size`                   | définit la taille du champ, qui à son tour ajoute une classe à son conteneur. Les <br>valeurs valides sont `large`, `x-small`, `medium`, `long`, `small`. Vous pouvez <br>bien sûr en ajouter plus dans le modèle que vous voyez, lorsqu'il est utilisé dans <br>le frontend
| `style`                  | définit le style de champ. S'il est défini sur vertical, le champ peut apparaître en <br>pleine largeur. C'est un moyen facile de nettoyer le formulaire.
| `title`                  | définit la valeur du titre du champ
| `toggleable`             | ajouter une case à cocher qui basculera l'attribut activé/désactivé du champ
| `validate.required`      | si défini sur une valeur positive, définit le champ comme requis
| `validate.pattern`       | définit un modèle de validation
| `validate.message`       | définit le message affiché si la validation échoue
| `validate.type`          | définit le type de champ utilisé lors de la validation

<h2 id="En savoir plus sur les champs">En savoir plus sur les champs
<a href="#En savoir plus sur les champs" class="toc-anchor after"></a></h2>

Vous pouvez lire comment les champs sont construits à partir de la source : [champs ajoutés par le plug-in de formulaire](https://github.com/getgrav/grav-plugin-form/tree/master/templates/forms) et [champs uniquement disponibles dans Admin](https://github.com/getgrav/grav-plugin-admin/tree/master/themes/grav/templates/forms).

<h2 id="Validation">Validation
<a href="#Validation" class="toc-anchor after"></a></h2>

La plupart des champs permettent la validation.

```yaml
1 | validate:
2 |   required: true
```

fera que le champ sera marqué comme requis.

```yaml
1 | validate:
2 |   message: 'Some message'
```

affichera le message défini lorsque le champ n'est pas correctement rempli.

```yaml
1 | validate:
2 |   pattern: 'Some pattern'
```

validera la valeur du champ par rapport au modèle regex passé. Exemples : `Pattern : "[1-9][0-9]*", modèle : '[A-Za-z0-9-]+', modèle : '[a-z0-9-]+', modèle : '^[a-z0-9_-]{3,16}$', modèle : '(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{8 ,}'`

<h3 id="validate.type">validate.type
<a href="#validate.type" class="toc-anchor after"></a></h3>

`validate.type` indique le type par rapport auquel il doit être validé.

Quelques exemples:

Un éditeur se traduira par une `textarea` :

```yaml
1 | content:
2 |    type: editor
3 |    validate:
4 |       type: textarea
```

Un selectize sera un `commalist` :

```yaml
1  | taxonomies:
2  |    type: selectize
3  |    size: large
4  |    label: PLUGIN_ADMIN.TAXONOMY_TYPES
5  |    classes: fancy
6  |    help: PLUGIN_ADMIN.TAXONOMY_TYPES_HELP
7  |    validate:
8  |       type: commalist
9  |
10 | filters.category:
11 |    type: selectize
12 |    label: Category filter
13 |    help: Comma separated list of category names
14 |    validate:
15 |       type: commalist
```

Valider une adresse e-mail :

```yaml
1 | author.email:
2 |    type: text
3 |    size: large
4 |    label: PLUGIN_ADMIN.DEFAULT_EMAIL
5 |    help: PLUGIN_ADMIN.DEFAULT_EMAIL_HELP
6 |    validate:
7 |       type: email
```

Assurez-vous qu'une valeur est un booléen :

```yaml
1  | summary.enabled:
2  |    type: toggle
3  |    label: PLUGIN_ADMIN.ENABLED
4  |    highlight: 1
5  |    help: PLUGIN_ADMIN.ENABLED_HELP
6  |    options:
7  |        1: PLUGIN_ADMIN.YES
8  |        0: PLUGIN_ADMIN.NO
9  |     validate:
10 |        type: bool
```

Assurez-vous qu'une valeur est un entier compris entre 0 et 65536 :

```yaml
1 | summary.size:
2 |     type: text
3 |     size: x-small
4 |     label: PLUGIN_ADMIN.SUMMARY_SIZE
5 |     help: PLUGIN_ADMIN.SUMMARY_SIZE_HELP
6 |     validate:
7 |        type: int
8 |        min: 0
9 |        max: 65536
```

Assurez-vous qu'une valeur est un nombre > 1 :

```yaml
1 | pages.list.count:
2 |    type: text
3 |    size: x-small
4 |    label: PLUGIN_ADMIN.DEFAULT_PAGE_COUNT
5 |    help: PLUGIN_ADMIN.DEFAULT_PAGE_COUNT_HELP
6 |    validate:
7 |       type: number
8 |       min: 1
```

Validez un type de taxonomie en tant que tableau :

```yaml
1 | header.taxonomy:
2 |    type: taxonomy
3 |    label: PLUGIN_ADMIN.TAXONOMY
4 |    multiple: true
6 |    validate:
7 |       type: array
```

Validez un champ de texte en tant que slug :

```yaml
1 | folder:
2 |    type: text
3 |    label: PLUGIN_ADMIN.FOLDER_NAME
4 |    validate:
5 |       type: slug
```

<h2 id="Champ de tableau">Champ de tableau
<a href="#Champ de tableau" class="toc-anchor after"></a></h2>

![Déployer](https://learn.getgrav.org/user/pages/06.forms/01.blueprints/01.fields-available/array_field_bp.gif)

Le type de champ `attay` est utilisé pour créer une simple liste d'objets clé-valeurs, ou simplement une liste de valeurs si vous utilisez l'option `value_only`.

Exemple:

```yaml
1 | metadata:
2 |   type: array
3 |   label: PLUGIN_ADMIN.METADATA
4 |   help: PLUGIN_ADMIN.METADATA_HELP
5 |   placeholder_key: PLUGIN_ADMIN.METADATA_KEY
6 |   placeholder_value: PLUGIN_ADMIN.METADATA_VALUE
7 |   required: true
```

| **ATTRIBUT**          | **DESCRIPTION**
| ---------             | ---------
| `placeholder_key`     |
| `placeholder_value`   |
| `value_only`          | N'exige pas ou ne stocke pas de clés de tableau, stocke juste un simple <br>tableau de valeurs.
| `value_type`          | Définissez sur `textarea` pour afficher un [champ de zone de texte](formulaires-ref-index-formulaires.md#Champ de zone de texte) pour <br>saisir les valeurs du tableau plutôt que le [champ de texte](formulaires-ref-index-formulaires.md#Champ de texte) plus petit.

| **ATTRIBUT COMMUN AUTORISÉ**                             
| ---------                                           
| [default](#Attributs des champs communs)            
| [help](#Attributs des champs communs)               
| [label](#Attributs des champs communs)              
| [name](#Attributs des champs communs)               
| [style](#Attributs des champs communs)              
| [toggleable](#Attributs des champs communs)         
| [validate.required](#Attributs des champs communs)  
| [validate.type](#Attributs des champs communs)      

<h2 id="Champ de sélecteur de couleurs">Champ de sélecteur de couleurs
<a href="#Champ de sélecteur de couleurs" class="toc-anchor after"></a></h2>

![Pipette à couleurs](https://learn.getgrav.org/user/pages/06.forms/01.blueprints/01.fields-available/colorpicker_field.png)

Le type de champ `colorpicker` est utilisé pour afficher un champ color picker

Exemple:

```yaml
1 | color:
2 |   type: colorpicker
3 |   label: Choose a color
4 |   default: '"FFFFFF'
```

| **ATTRIBUT COOMUN AUTORISÉ**                
| ---------                            
| [autocomplete](#Attributs des champs communs) 
| [autofocus](#Attributs des champs communs) 
| [classes](#Attributs des champs communs) 
| [default](#Attributs des champs communs) 
| [disabled](#Attributs des champs communs) 
| [help](#Attributs des champs communs) 
| [id](#Attributs des champs communs) 
| [label](#Attributs des champs communs) 
| [name](#Attributs des champs communs) 
| [placeholder](#Attributs des champs communs) 
| [style](#Attributs des champs communs) 
| [title](#Attributs des champs communs) 
| [toggleable](#Attributs des champs communs) 
| [validate.message](#Attributs des champs communs) 
| [validate.required](#Attributs des champs communs) 
| [validate.type](#Attributs des champs communs) 

<h2 id="Colonnes / Champs de colonne">Colonnes / Champs de colonne
<a href="#Colonnes / Champs de colonne" class="toc-anchor after"></a></h2>

![Colonnes](https://learn.getgrav.org/user/pages/06.forms/01.blueprints/01.fields-available/columns_field_bp.gif)

Les `columns` et les types de champ `column` sont utilisés pour diviser les champs de formulaire contenus en colonnes

Exemple:

```yaml
1  | columns:
2  |     type: columns
3  |     fields:
4  |        column1:
5  |           type: column
6  |           fields:
7  | 
8  |           # .... subfields
9  | 
10 |        column2:
11 |           type: column
12 |           fields:
13 |
14 |           # .... other subfields
```

| **ATTRIBUT**        | **DESCRIPTION**
| ---------           | ---------
| `fields`            | Les colonnes / sous-champs de colonne

<h2 id="Champ de format de date">Champ de format de date
<a href="#Champ de format de date" class="toc-anchor after"></a></h2>

![Format de date](https://learn.getgrav.org/user/pages/06.forms/01.blueprints/01.fields-available/dateformat_field_bp.gif)

Le type de champ `dateformat` est utilisé pour

Exemple:

```yaml
1  | pages.dateformat.short:
2  |     type: dateformat
3  |     size: medium
4  |     classes: fancy
5  |     label: PLUGIN_ADMIN.SHORT_DATE_FORMAT
6  |     help: PLUGIN_ADMIN.SHORT_DATE_FORMAT_HELP
7  |     default: "jS M Y"
8  |     options:
9  |        "F jS \\a\\t g:ia": Date1
10 |        "l jS \\of F g:i A": Date2
11 |        "D, d M Y G:i:s": Date3
12 |        "d-m-y G:i": Date4
13 |        "jS M Y": Date5
```

| **ATTRIBUT**          | **DESCRIPTION**
| ---------             | ---------
| `options`             | Le champ options de valeur-clé disponibles
| `multiple`            | Boléen. Si positif, le champ accepte plusieurs valeurs
| `selectize`           |

|  **ATTRIBUT COMMUN AUTORISÉ**               
| ---------                            
| [autofocus](#Attributs des champs communs)
| [classes](#Attributs des champs communs)
| [default](#Attributs des champs communs)
| [disable](#Attributs des champs communs)
| [help](#Attributs des champs communs)
| [label](#Attributs des champs communs)
| [name](#Attributs des champs communs)
| [novalidate](#Attributs des champs communs)
| [size](#Attributs des champs communs)
| [style](#Attributs des champs communs)
| [toggleable](#Attributs des champs communs)
| [validate.required](#Attributs des champs communs)
| [validate.type](#Attributs des champs communs)

<h2 id="Champ de date">Champ de date
<a href="#Champ de date" class="toc-anchor after"></a></h2>

![DateTime](https://learn.getgrav.org/user/pages/06.forms/01.blueprints/01.fields-available/datetime_field.gif)

The `datetime` field type is used to store and present a date and time field.

Example:

```yaml
1 | header.date:
2 |   type: datetime
3 |   label: PLUGIN_ADMIN.DATE
4 |   toggleable: true
5 |   help: PLUGIN_ADMIN.DATE_HELP
```
| **ATTRIBUT**          | **DESCRIPTION**
| ---------             | ---------
| `format`              | Une valeur de format datetime, vous pouvez utiliser n'importe lequel des formats [datetime PHP disponibles](https://www.php.net/manual/en/datetime.format.php).
| `validate.min`        | Une valeur valide minimale.
| `validate.max`        | Une valeur maximale valide.

| **ATTRIBUT COMMUN AUTORISÉ**               
| ---------                           
| [default](#Attributs des champs communs)
| [help](#Attributs des champs communs)
| [label](#Attributs des champs communs)
| [name](#Attributs des champs communs)
| [size](#Attributs des champs communs)
| [style](#Attributs des champs communs)
| [toggleable](#Attributs des champs communs)
| [alidate.required](#Attributs des champs communs)
| [validate.type](#Attributs des champs communs)

<h2 id="Champ de l'éditeur">Champ de l'éditeur
<a href="#Champ de l'éditeur" class="toc-anchor after"></a></h2>

![Champ de l'éditeur](https://learn.getgrav.org/user/pages/06.forms/01.blueprints/01.fields-available/editor_field_bp.gif)

Le type de champ `editor` est utilisé pour présenter l'éditeur Codemirror

Exemple:

```yaml
1  | frontmatter:
2  |     classes: frontmatter
3  |     type: editor
4  |     label: PLUGIN_ADMIN.FRONTMATTER
5  |     autofocus: true
6  |     codemirror:
7  |        mode: 'yaml'
8  |        indentUnit: 4
9  |        autofocus: true
10 |        indentWithTabs: false
11 |        lineNumbers: true
12 |        styleActiveLine: true
13 |        gutters: ['CodeMirror-lint-markers']
14 |        lint: true
```

| **ATTRIBUT**          | **DESCRIPTION**
| ---------             | ---------
| `codemirror`          |  Un ensemble de valeurs à définir dans [l'éditeur de codemirror](https://codemirror.net/doc/manual.html#config). Par défaut, utilise le <br>mode : gfm (démarquage aromatisé github).
| `resizer`             | Si positif, active le resizer. Sinon l'éditeur est fixe.

| **ATTRIBUT COMMUN AUTORISÉ**               
| ---------                            
| [default](#Attributs des champs communs)
| [help](#Attributs des champs communs)
| [label](#Attributs des champs communs)
| [name](#Attributs des champs communs)
| [style](#Attributs des champs communs)
| [validate.required](#Attributs des champs communs)
| [validate.type](#Attributs des champs communs)
| [autofocus](#Attributs des champs communs)
| [classes](#Attributs des champs communs)
| [disabled](#Attributs des champs communs)
| [id](#Attributs des champs communs)
| [novalidate](#Attributs des champs communs)
| [placeholder](#Attributs des champs communs)
| [readonly](#Attributs des champs communs)

<h2 id="Champ d'éléments">Champ d'éléments
<a href="#Champ d'éléments" class="toc-anchor after"></a></h2>

![Champ éléments](https://learn.getgrav.org/user/pages/06.forms/01.blueprints/01.fields-available/elements_field.gif)

Ce champ est uniquement organisationnel et permet de regrouper des éléments au sein d'un groupe nommé qui ne sera affiché que si la valeur de l'élément sélectionné correspond au groupe.

Exemple:

```yaml
1  | header.elements-demo.type:
2  |     type: elements
3  |     label: 'Elements Demo'
4  |     size: small
5  |     default: gelato
6  |     options:
7  |        gelato: Gelato Flavors
8  |        color: Color
9  |        planets: Planets
10 |     fields:
11 |        gelato:
12 |           type: element
13 |           fields:
14 |              .flavours:
15 |                 type: array
16 |                 default:
17 |                    pistacchio: Pistacchio
18 |                    vanilla: Vanilla
19 |                    chocolate: Chocolate
20 |                    stracciatella: Stracciatella
21 |        color:
22 |           type: element
23 |           fields:
24 |              .description:
25 |                 type: textarea
26 |                 rows: 10
27 |                 default: Color (American English) or colour (Commonwealth English) is the  
   | visual perceptual property corresponding in humans to the categories called blue, green,  
   | red, etc. Color derives from the spectrum of light (distribution of light power versus  
   | wavelength) interacting in the eye with the spectral sensitivities of the light receptors.  
   | Color categories and physical specifications of color are also associated with objects  
   | or materials based on their physical properties such as light absorption, reflection,  
   | or emission spectra. By defining a color space colors can be identified numerically by  
   | their coordinates.
28 |     planets:
29 |        type: element
30 |        fields:
31 |           .favorites:
32 |              type: text
33 |              placeholder: What are your favorite planets?
34 |              markdown: true
35 |              description: 'Find a list of planets from [Wikipedia](https://en.wikipedia  
   | .org/wiki/Planet)'
```

<h2 id="Champ d'ensemble de champs">Champ d'ensemble de champs
<a href="#Champ d'ensemble de champs" class="toc-anchor after"></a></h2>


![Champ d'ensemble de champs](https://learn.getgrav.org/user/pages/06.forms/01.blueprints/01.fields-available/fieldset-gif.gif)

The champs `fieldset` regroupe un ensemble de champs à l'intérieur d'une boîte réductible.

Exemple:

```yaml
1  | header.fieldset:
2  |     type: fieldset
3  |     title: Your title
4  |     help: Help text
5  |     info: Info text
6  |     text: Text inside fieldset and before other fields
7  |     icon: comments       # Fork Awesome icons system (see : forkaweso.me).
8  |     collapsed: true      # Initial state of fieldset (see collapsible option)
9  |     collapsible: true    # Whether one can expand the fieldset or not
10 |     fields:
11 |        header.fieldset.an_example_text:
12 |           type: text
13 |           label: text
14 |        header.fieldset.an_example_textarea:
15 |           type: textarea
16 |           label: textarea
```

Les ensembles de champs doivent également être enregistrés dans le frontmatter, avec `header`, afin que leurs états de sous-champ soient correctement mémorisés !

<div class="notice info">
<strong>Problème connu</strong> : si les champs d'un fieldset utilisent un <code>toggleable</code>:, leur état ne sera pas mémorisé si le fieldset nommé n'est pas préfixé par <code>header</code>. Voici un exemple de structure valide avec une modification de l'option pagination :
</div>

```yaml
1  | header.fieldset:
2  |      type: fieldset
3  |     ... etc...
4  |     fields:
5  |        header.content.pagination:
6  |           type: toggle
7  |           toggleable: true
8  |           label: "Activate Pagination ?"
9  |           highlight: 1
10 |           default: 0
11 |           options:
12 |              1: Yes
13 |              0: No
14 |           validate:
15 |              type: bool
```

<h2 id="Icône du jeu de champs">Icône du jeu de champs
<a href="#Icône du jeu de champs" class="toc-anchor after"></a></h2>

Vous pouvez utiliser une icône à placer dans l'en-tête de l'ensemble de champs. Le système d'icônes utilisé est [Fork Awesome](https://forkaweso.me/).

| **ATTRIBUT**          | **DESCRIPTION**
| ---------             | ---------
| `icon`                | Une icône pour la boîte.
| `collapsed`           | Si `true`, la liste s'ouvre réduite. Par défaut, il est développé.
| `collapsible`         | Si l'on peut étendre ou non le jeu de champs.

| **ATTRIBUT COMMUN AUTORISÉ**              
| ---------                            
| [disabled](#Attributs des champs communs)
| [help](#Attributs des champs communs)
| [label](#Attributs des champs communs)
| [name](#Attributs des champs communs)
| [size](#Attributs des champs communs)
| [style](#Attributs des champs communs)
| [toggleable](#Attributs des champs communs)

<h2 id="Champ de fichier">Champ de fichier
<a href="#Champ de fichier" class="toc-anchor after"></a></h2>

![Champ de fichier](https://learn.getgrav.org/user/pages/06.forms/01.blueprints/01.fields-available/file_field_bp.gif)

<div class="notice info">
Le champ <code>field</code> est destiné à être utilisé par les plans de <strong>configuration de thème</strong> et de <strong>plugins</strong>, <strong>PAS les plans de page</strong>. Pour les pages, vous devez utiliser le champ <code>pagemedia</code> existant, puis utiliser le champ <a href="#Champ de sélecteur de fichiers">filepicker</a> pour sélectionner les fichiers.
</div>

<div class="notice info">
Le champ <code>field</code> ne fonctionne pas actuellement comme prévu dans un champ de liste. Utilisez un seul champ <code>pagemedia</code> distinct de la liste avec un ou plusieurs champs <code>filepicker</code> dans la liste.
</div>

<div class="notice note">
Plus de détails peuvent être trouvés dans la section dédiée <a href="../formulaires-telechargement">Comment ajouter un téléchargement de fichiers</a>. Notez également que l'affichage d'une image téléchargée dans un champ de fichier ne se fait pas de la même manière qu'avec un champ filepicker. Plus de détails sur la façon d'accéder aux images téléchargées dans un champ de fichier peuvent être trouvés sur cette entrée de <a href="../cookbook-recettes-twig">livre de recettes</a>.
</div>

Exemple:

```yaml
1 | custom_logo_login_screen:
2 |      type: file
3 |      label: Custom Logo Login Screen
4 |      destination: 'plugins://admin/assets'
5 |      accept:
6 |         - image/*
```
```yaml
1 | custom_file:
2 |      type: file
3 |      label: A Label
4 |      destination: 'theme://assets'
5 |      multiple: true
6 |      limit: 5
7 |      filesize: 1
8 |      accept:
9 |      - image/*
```

| **ATTRIBUT**          | **DESCRIPTION**
| ---------             | ---------
| `destination`         | Le dossier où les fichiers seront stockés, en utilisant un flux ou par rapport à la <br>racine Grav. Par exemple : `plugins://mon-plugin/assets`.
| `multiple`            | S'il faut ou non autoriser plus d'un fichier par champ.
| `limit`               | Lorsque `multiple` est activé, permet de limiter le nombre de fichiers autorisés à <br>être téléchargés.
| `filesize`            | La taille en Mo de chaque fichier est autorisée.
| `accept`              | Ajouter une liste des types mime de page et des extensions acceptés. Par exemple <br>`["image/*", '.mp3']`.
| `random_name`         | Utilise un nom de fichier aléatoire pour chaque fichier.
| `avoid_overwriting`   | Ajoute un horodatage avant chaque nom de fichier en cas de conflit.

| **ATTRIBUT COMMUN AUTORISÉ**            
| ---------                          
| [default](#Attributs des champs communs)
| [disabled](#Attributs des champs communs)
| [help](#Attributs des champs communs)
| [label](#Attributs des champs communs)
| [name](#Attributs des champs communs)
| [size](#Attributs des champs communs)
| [style](#Attributs des champs communs)
| [toggleable](#Attributs des champs communs)
| [validate.required](#Attributs des champs communs)
| [validate.type](#Attributs des champs communs)

<h2 id="Champ de sélecteur de fichiers">Champ de sélecteur de fichiers
<a href="#Champ de sélecteur de fichiers" class="toc-anchor after"></a></h2>

![Champ de sélecteur de fichiers](https://learn.getgrav.org/user/pages/06.forms/01.blueprints/01.fields-available/filepicker_field.png)

Le type de champ `filepicker` peut être utilisé dans les configurations de pages, de plugins et de thèmes (blueprints). Gère la sélection d'un fichier à partir d'un emplacement et l'enregistre dans les en-têtes de page ou la configuration du thème/plug-in.

Exemple:

```yaml
1 | picked_image:
2 |      type: filepicker
3 |      folder: 'theme://images/pages'
4 |      label: Select a file
5 |      preview_images: true
6 |      accept:
7 |         - .png
8 |         - .jpg
```

```yaml
1 | header.a_file :
2 |      type : sélecteur de fichiers
3 |      dossier : 'self@'
4 |      preview_images : vrai
5 |      label : sélectionnez un fichier
```

| **ATTRIBUT**          | **DESCRIPTION**
| ---------             | ---------
| `folder`              | Le dossier où les fichiers seront recherchés, en utilisant un flux ou par rapport à la <br>racine Grav. Accepte n'importe quelle valeur dans le [format de destination du champ de fichier](formulaires-telechargement#destination.md).
| `accept`              | Une liste des extensions de fichiers acceptées.
| `preview_images`      | Si activé, les fichiers image auront un petit aperçu.
| `on_demand`           | Si activé, ne chargera les fichiers et les images que lorsque le sélecteur de fichiers <br>est focalisé. Ceci est utile pour réduire les temps de chargement de la page d'édition <br>de l'administrateur lorsqu'il y a de gros médias ou de nombreux champs de sélecteur <br>de fichiers.

| **ATTRIBUT COMMUN AUTORISÉ**             
| ---------                            
| [default](#Attributs des champs communs)
| [disabled](#Attributs des champs communs)
| [help](#Attributs des champs communs)
| [label](#Attributs des champs communs)
| [name](#Attributs des champs communs)
| [size](#Attributs des champs communs)
| [style](#Attributs des champs communs)
| [toggleable](#Attributs des champs communs)
| [validate.required](#Attributs des champs communs)

<h2 id="Champ de pot de miel">Champ de pot de miel
<a href="#Champ de pot de miel" class="toc-anchor after"></a></h2>

Le type de champ `honeypot` crée un champ caché qui, une fois rempli, renverra une erreur. C'est un moyen utile d'empêcher les robots de remplir et de soumettre un formulaire.

Exemple:

```yaml
1 | fields:
2 |      - name: honeypot
3 |      type: honeypot
```

Il s'agit d'un simple champ de texte qui n'apparaît pas sur le front-end. Les robots, qui détectent les champs dans le code et les remplissent automatiquement, rempliront probablement le champ. L'erreur empêche ce formulaire d'être correctement soumis. L'erreur revient à côté de l'élément de formulaire, plutôt qu'en haut dans un bloc de message.

Un champ de pot de miel est une alternative populaire aux champs de captcha.
Champ de liste

<h2 id="Champ de liste">Champ de liste
<a href="#Champ de liste" class="toc-anchor after"></a></h2>

![Champ de liste 1](https://learn.getgrav.org/user/pages/06.forms/01.blueprints/01.fields-available/list_field_bp.gif)

Le type de champ `list` est utilisé pour créer des collections de champs. Le champ accepte un attribut de `fields` qui hébergera des sous-champs, et il y aura un bouton "Ajouter un élément" pour permettre à l'utilisateur d'ajouter plus d'éléments à la collection.

Exemple:

```yaml
1  | header.buttons:
2  |     name: buttons
3  |     type: list
4  |     style: vertical
5  |     label: Buttons
6  |     fields:
7  |        .text:
8  |           type: text
9  |           label: Text
10 |        .url:
11 |           type: text
12 |           label: URL
13 |        .primary:
14 |           type: toggle
15 |           label: Primary
16 |           highlight: 1
17 |           default: 1
18 |           options:
19 |              1: 'Yes'
20 |              0: 'No'
21 |           validate:
22 |              type: bool
```

Cet exemple générera cette interface d'administration :

![Champ de liste 2](https://learn.getgrav.org/user/pages/06.forms/01.blueprints/01.fields-available/field_list_1.png)

Lors de l'enregistrement de la page, nous verrons le YAML suivant ajouté au frontmatter de la page :

```yaml
1 | buttons:
2 |      -
3 |         text: 'Some text'
4 |         url: 'https://getgrav.org'
5 |         primary: false
6 |      -
7 |         text: 'Another text'
8 |         url: 'https://another-url.com'
9 |         primary: true
```

Cela sera utilisé dans le thème Twig pour afficher la liste d'une manière agréable.

Un autre exemple de cette définition de champ est cette liste de fonctionnalités, utilisée par la page enfant modulaire des fonctionnalités d'Antimatière. Chaque fonctionnalité a une icône, un en-tête et du texte :

```yaml
1  | header.features:
2  |     name: features
3  |     type: list
4  |     label: Features
5  | 
6  |     fields:
7  |        .icon:
8  |           type: text
9  |           label: Icon
10 |        .header:
11 |           type: text
12 |           label: Header
13 |        .text:
14 |           type: text
15 |           label: Text
```

L'accès et l'affichage des données d'un champ `list` se fait avec une simple boucle for twig, comme dans l'exemple ci-dessous :

```twig
1 | {% for feature in page.header.features %}
2 |      {{ feature.icon }}
3 |      {{ feature.header }}
4 |      {{ feature.text }}
5 | {% endfor %}
```
    
| **ATTRIBUT**        | **DESCRIPTION**
| ---------           | ---------
| `fields`            | Les sous-champs.
| `collapsed`         | Si `true`, la liste s'ouvre réduite. Par défaut, il est développé.
| `style`             | Peut être réglé sur `vertical` pour conserver l'espace horizontal.
| `btnLabel`          | Le texte de l'étiquette "ajouter un nouvel élément".
| `sort`              | Booléen. Si négatif, désactive la possibilité de trier les éléments.
| `controls`          | Décide où le bouton "Ajouter un élément" sera placé. Peut être défini sur `[top|bottom|both]`. <br>Par défaut sur `bottom`.
| `placement`         | Décide où l'élément ajouté sera placé. Peut être défini sur `[top|bottom|position]`. <br>Par défaut sur `bottom`. Si la valeur de `placement` est `top` ou `bottom`, les deux boutons <br>ajoutent respectivement l'élément en haut ou en bas. Si la valeur de `placement` est <br> `position`, l'élément est ajouté en fonction de la position du bouton cliqué - si le <br>bouton du haut est cliqué, l'élément sera ajouté en haut et si c'est le bouton du bas - en bas.
| `min`               | Nombre minimum d'éléments autorisés dans la liste.
| `max`               | Nombre maximum d'éléments autorisés dans la liste. Le bouton "Ajouter un élément" <br>ne fonctionnera pas au-delà de ce numéro.

| **ATTRIBUT COMMUN AUTORISÉ**      
| ----------      
| [default](#Attributs des champs communs)
| [help](#Attributs des champs communs)
| [label](#Attributs des champs communs)
| [name](#Attributs des champs communs)
| [validate.required](#Attributs des champs communs)
| [validate.type](#Attributs des champs communs)
| [size](#Attributs des champs communs)

<h2 id="Champ PageMediaSelect">Champ PageMediaSelect
<a href="#Champ PageMediaSelect" class="toc-anchor after"></a></h2>

Le type de champ `pagemediaselect` est utilisé pour permettre aux utilisateurs de choisir un média parmi l'un des médias de la page déjà téléchargé via FTP ou à l'aide du gestionnaire de médias de la page.

Exemple

```yaml
1 | header.img_link:
2 |      label: Choose media
3 |      type: pagemediaselect
```

| **ATTRIBUT**          | **DESCRIPTION**
| ---------             | ---------
| `multiple`            | Sélectionnez plusieurs fichiers

| **ATTRIBUT COMMUN AUTORISÉ**           
| ---------                           
| [default](#Attributs des champs communs)
| [help](#Attributs des champs communs)
| [label](#Attributs des champs communs)
| [name](#Attributs des champs communs)
| [style](#Attributs des champs communs)
| [toggleable](#Attributs des champs communs)
| [validate.required](#Attributs des champs communs)
| [autofocus](#Attributs des champs communs)
| [classes](#Attributs des champs communs)
| [disabled](#Attributs des champs communs)
| [id](#Attributs des champs communs)
| [novalidate](#Attributs des champs communs)
| [size](#Attributs des champs communs)

<h2 id="Champ Pages">Champ Pages
<a href="#Champ Pages" class="toc-anchor after"></a></h2>

![Champ Pages](https://learn.getgrav.org/user/pages/06.forms/01.blueprints/01.fields-available/pages_field_bp.gif)

Le type de champ `pages` affiche une liste des pages du site.

Exemple

```yaml
1  | home.alias:
2  |     type: pages
3  |     size: medium
4  |     classes: fancy
5  |     label: PLUGIN_ADMIN.HOME_PAGE
6  |     start_route: '/some_page'
7  |      show_all: false
8  |     show_modular: false
9  |     show_root: false
10 |     help: PLUGIN_ADMIN.HOME_PAGE_HELP
```

| **ATTRIBUT**          | **DESCRIPTION**
| ---------             | ---------
| `start_route`         | Choisissez une route racine pour la liste.
| `show_fullpath`       | Afficher le chemin de la page au lieu du titre.
| `show_slug`           | Afficher le slug.
| `show_all`            | Affiche toutes les pages.
| `show_modular`        | Affiche les pages modulaires.
| `show_root`           | Affiche la page racine.
| `options`             | Une liste facultative de choix supplémentaires.
| `multiple`            | Sélectionnez plusieurs pages.
| `limit_levels`        | Combien de niveaux afficher.
| `selectize`           |

Si vous définissez `multiple` sur true, vous devez ajouter `validate.type: array`. Sinon, le tableau des pages sélectionnées ne sera pas enregistré correctement.

| **ATTRIBUT COMMUN AUTORISÉ**             
| ---------                            
| [default](#Attributs des champs communs)
| [help](#Attributs des champs communs)
| [label](#Attributs des champs communs)
| [name](#Attributs des champs communs)
| [style](#Attributs des champs communs)
| [toggleable](#Attributs des champs communs)
| [validate.required](#Attributs des champs communs)
| [validate.type](#Attributs des champs communs)
| [autofocus](#Attributs des champs communs)
| [classes](#Attributs des champs communs)
| [disabled](#Attributs des champs communs)
| [novalidate](#Attributs des champs communs)
| [size](#Attributs des champs communs)

<h2 id="Champ de section">Champ de section
<a href="#Champ de section" class="toc-anchor after"></a></h2>

Le type de champ `Section` est utilisé pour diviser une page de paramètres en sections.

Exemple:

```yaml
1 | content:
2 |      type: section
3 |      title: PLUGIN_ADMIN.DEFAULTS
4 |      underline: true
5 | 
6 |      fields:
7 | 
8 |         #..... subfields
```

| **ATTRIBUT**          | **DESCRIPTION**
| ---------             | ---------
| `title`               | Un titre de rubrique.
| `underline`           | Ajouter un soulignement après le titre.
| `text`                | Un texte à afficher en dessous.
| `security`            | Un tableau d'informations d'identification dont un utilisateur a besoin pour visualiser <br>cette section.
| `title_level`         | Définissez une balise de titre personnalisée. Par défaut : h3.

<h2 id="Champ de sélection">Champ de sélection
<a href="#Champ de sélection" class="toc-anchor after"></a></h2>

![Champ de sélection](https://learn.getgrav.org/user/pages/06.forms/01.blueprints/01.fields-available/selectize_field_bp.gif)

Le type de champ `selectize` est utilisé pour afficher un hybride d'une zone de texte et d'une zone de sélection. Principalement utile pour le balisage et d'autres champs de sélection d'éléments.

Exemple:

```yaml
1  | taxonomies:
2  |     type: selectize
3  |     selectize:
4  |        options:
5  |           - text: "test"
6  |              value: "real value 1"
7  |           - text: "test-2"
8  |              value: "real value 2"
9  |           - text: "test-3"
10 |              value: "real value 3"
11 |     size: large
12 |     label: PLUGIN_ADMIN.TAXONOMY_TYPES
13 |     classes: fancy
14 |     help: PLUGIN_ADMIN.TAXONOMY_TYPES_HELP
15 |     validate:
16 |        type: commalist
```

| **ATTRIBUT COMMUN AUTORISÉ**            
| ---------                           
| [default](#Attributs des champs communs)
| [help](#Attributs des champs communs)
| [label](#Attributs des champs communs)
| [name](#Attributs des champs communs)
| [style](#Attributs des champs communs)
| [toggleable](#Attributs des champs communs)
| [validate.required](#Attributs des champs communs)
| [validate.type](#Attributs des champs communs)

| **Attribut communs DANS LE BLOC D'ENTRÉE**            
| ---------                                        
| [autocomplete](#Attributs des champs communs)
| [autofocus](#Attributs des champs communs)
| [classes](#Attributs des champs communs)
| [disabled](#Attributs des champs communs)
| [id](#Attributs des champs communs)
| [placeholder](#Attributs des champs communs)
| [readonly](#Attributs des champs communs)
| [size](#Attributs des champs communs)
| [title](#Attributs des champs communs)
| [validate.pattern](#Attributs des champs communs)
| [validate.message](#Attributs des champs communs)

<h2 id="Champ de taxonomie">Champ de taxonomie
<a href="#Champ de taxonomie" class="toc-anchor after"></a></h2>

![Taxonomie](https://learn.getgrav.org/user/pages/06.forms/01.blueprints/01.fields-available/taxonomy_field_bp.gif)

Le type de champ `taxonomy` est une sélection spéciale préconfigurée pour sélectionner une ou plusieurs valeurs de taxonomie.

Exemple:

```yaml
1 | header.taxonomy:
2 |   type: taxonomy
3 |   label: PLUGIN_ADMIN.TAXONOMY
4 |   multiple: true
5 |   validate:
6 |      type: array
```

| **ATTRIBUT**          | **DESCRIPTION**
| ---------             | ---------
| `multiple`            | Boléen. Si positif, le champ accepte plusieurs valeurs.

| **ATTRIBUT COMMUN AUTORISÉ**               
| ---------                              
| [autofocus](#Attributs des champs communs)
| [classes](#Attributs des champs communs)
| [default](#Attributs des champs communs)
| [disabled](#Attributs des champs communs)
| [help](#Attributs des champs communs)
| [id](#Attributs des champs communs)
| [label](#Attributs des champs communs)
| [name](#Attributs des champs communs)
| [novalidate](#Attributs des champs communs)
| [outerclasses](#Attributs des champs communs)
| [size](#Attributs des champs communs)
| [style](#Attributs des champs communs)
| [validate.required](#Attributs des champs communs)
| [validate.pattern](#Attributs des champs communs)
| [validate.message](#Attributs des champs communs)
   
