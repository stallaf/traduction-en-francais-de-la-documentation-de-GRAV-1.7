<h1 class="rem">Blueprint</h1>

La structure de base de **Flex Blueprint** contient le `title`, la `description` et le `type`, qui décrivent le type et trois sections : `config`, `blueprints` et `form`, qui décrivent différents aspects du type de répertoire.

Structure principale de `contacts.yaml` :

```yaml
title: Contacts
description: Simple contact directory with tags.
type: flex-objects  # do not change

# Flex Configuration
config: {}

# Flex Directory Forms
blueprints: {}

# Flex Object Form
form: {}
```

Pour créer votre propre répertoire personnalisé, vous devez commencer par nommer votre `type` (nom de fichier) et remplir le `title` et la description``.

Après avoir créé le fichier et rempli les informations de base, l'étape suivante consiste à copier votre formulaire existant ou à ajouter des champs dans le fichier.

<div class="notice note">
<strong>CONSEIL</strong> : Nous supposons que vous savez déjà comment créer vos propres <a href="../avance-flex-type-blueprint">formulaires et blueprints</a>.
</div>

<div class="notice warning">
<strong>ATTENTION</strong> : Il est préférable de ne pas utiliser le format liste simple pour décrire les champs comme décrit dans <a href="../formulaires-frontaux/#Créer un formulaire unique simple">Créer un formulaire unique simple</a>. Ne transmettez pas non plus la section `process` du formulaire à ce fichier, elle ne sera pas utilisée par Flex.
</div>

<h2 id="Form">Form.
<a href="#Form" class="toc-anchor after"></a></h2>

Dans notre exemple de contacts, la section de formulaire ressemble à ceci :

```yaml
# Flex Object Form
form:
   validation: loose

   fields:
      published:
         type: toggle
         label: Published
         highlight: 1
         default: 1
         options:
            1: PLUGIN_ADMIN.YES
            0: PLUGIN_ADMIN.NO
         validate:
            type: bool
            required: true

      last_name:
         type: text
         label: Last Name
         validate:
            required: true

      first_name:
         type: text
         label: First Name
         validate:
            required: true

      email:
         type: email
         label: Email Address
         validate:
            required: true

      website:
         type: url
         label: Website URL
   
      tags:
         type: selectize
         size: large
         label: Tags
         classes: fancy
         validate:
            type: commalist       
```

Le formulaire a le même aspect, qu'il provienne d'une page ou d'un fichier de configuration, de plug-in ou de thème. Il s'agit du plan directeur principal de chaque objet de votre répertoire et il doit contenir tous les champs définis dans l'objet. Considérez-le comme un formulaire qui s'affiche pour l'administrateur.

<div class="notice warning">
<strong>ATTENTION</strong> : Soyez prudent lorsque vous modifiez un Blueprint pour un type Flex existant. Assurez-vous que les objets que vous avez déjà enregistrés sont compatibles avec la nouvelle version du plan - ce qui signifie que vous devriez pouvoir à la fois enregistrer et afficher les anciens objets.
</div>

Nous n'avons pas encore tout à fait terminé. Il reste encore deux choses à faire pour que l'annuaire fonctionne : nous devons configurer la couche de stockage des données et définir les champs à afficher dans la vue Liste d'administration. Nous pouvons faire les deux dans la section `config`.

<h2 id="Config">Config.
<a href="#Config" class="toc-anchor after"></a></h2>

La section Config est la partie la plus compliquée de Flex Blueprint, bien qu'une grande partie ne soit destinée qu'à permettre la personnalisation. Il contient des sections `data`, `admin` et `site`.

```yaml
# Flex Configuration
config:
    
   # Data Settings
   data: {}

   # Admin Settings
   admin: {}
    
   # Site Settings
   site: {}
```

La configuration minimale ressemble à ceci :

```yaml
# Flex Configuration
config:

   # Data Settings
   data:
      storage: user-data://flex-objects/contacts.json

   # Admin Settings
   admin:
      # List view
      list:
         # List of fields to display
         fields:
            last_name:
               link: edit # Edit link
            first_name:
               link: edit # Edit link
            email:
            website:
```
  
Il existe deux sections obligatoires dans la configuration : `config.data.storage` et `config.admin.list.fields`. Ce dernier définit les champs affichés dans la vue de la liste d'administration. Le stockage des données, quant à lui, définit la manière dont les données seront stockées.

<h2 id="Config > Data">Config > Data.
<a href="#Config > Data" class="toc-anchor after"></a></h2>

**Flex Directory** est hautement personnalisable. Vous pouvez utiliser vos propres classes PHP `object`, `collection` et `index` pour ajouter votre propre comportement. De plus, vous pouvez configurer la couche `storage` pour répondre au mieux à vos besoins. Directory a également sa fonctionnalité `ordering` et `search` par défaut.

```yaml
config:
   data:
      # Flex Object Class
      object: CLASSNAME
      # Flex Collection Class
      collection: CLASSNAME
      # Flex Index Class
      index: CLASSNAME
      # Storage Options
      storage: {}
      # Ordering Options
      ordering: {}
      # Search Options
      search: {}
```

L'objet, la collection et l'index prennent des noms de classe. S'ils ne sont pas fournis, Grav utilisera la configuration par défaut suivante :

```yaml
config:
   data:
      object: 'Grav\Common\Flex\Types\Generic\GenericObject'
      collection: 'Grav\Common\Flex\Types\Generic\GenericCollection'
      index: 'Grav\Common\Flex\Types\Generic\GenericIndex'   
```

Ces classes définiront ensemble le comportement de votre type. Si vous souhaitez personnaliser votre propre type, c'est possible en étendant ces classes et en passant vos propres classes ici.

L'une des parties les plus importantes consiste à définir où et comment les données sont stockées :

```yaml
config:
   data:
      storage:
         class: 'Grav\Framework\Flex\Storage\SimpleStorage'
         options:
            formatter:
               class: 'Grav\Framework\File\Formatter\JsonFormatter'
            folder: user-data://flex-objects/contacts.json   
```

Ci-dessus, un cas particulier qui peut aussi s'écrire sous une forme courte :

```yaml
config:
   data:
      storage: user-data://flex-objects/contacts.json
```

Grav 1.7 propose 3 stratégies de stockage différentes, bien que vous puissiez facilement créer la vôtre :

| **Nom**         | **Classe**                     | **Description**
| ---------       | ---------                      | ---------
| Simple Storage  | Grav\Framework\Flex\Storage<br>\SimpleStorage  | Tous les objets sont stockés dans un seul fichier.<br> Ne prend pas en charge les médias.
| File Storage    | Grav\Framework\Flex\Storage<br>\FileStorage    | Les objets sont stockés dans des fichiers<br> séparés dans un dossier unique.
| Folder Storage | Grav\Framework\Flex\Storage<br>\FolderStorage   | Chaque objet est stocké dans un dossier séparé.

De plus, vous pouvez fournir un format de fichier avec `options.formatter.class` :

| **Nom**         | **Classe**                  | **Description**
| ---------       | ---------                   | ---------
| JSON            | Grav\Framework\File\Formatter<br>\JsonFormatter  | Utilisez le format de fichier JSON.
| YAML            | Grav\Framework\File\Formatter<br>\YamlFormatter  | Utilisez le format de fichier YAML.
| Markdown        | Grav\Framework\File\Formatter<br>\MarkdownFormatter    | Utilisez le format de fichier<br> Markdown de Grav avec le frontmatter YAML.
| Serialize       | Grav\Framework\File\Formatter<br>\SerializeFormatter   | Utiliser le sérialiseur PHP.<br>Rapide mais pas lisible par l'homme.
| INI             | Grav\Framework\File\Formatter<br>\IniFormatter   | Utilise le format de fichier INI.<br> Non recommandé.
| CSV             | Grav\Framework\File\Formatter<br>\CsvFormatter   | Utiliser le format de fichier CSV.<br> Non recommandé.

Les options de configuration (avec les valeurs par défaut) pour les formateurs par défaut se trouvent ci-dessous dans les onglets :

**- JSON**

```yaml
# JSON
  formatter:
    class: 'Grav\Framework\File\Formatter\JsonFormatter'
    options:
      file_extension: '.json'
      encode_options: '' # See https://www.php.net/manual/en/function.json-encode.php (separate options with space)
      decode_assoc: true # Decode objects as arrays
      decode_depth: 512  # Decode up to 512 levels
      decode_options: '' # See https://www.php.net/manual/en/function.json-decode.php (separate options with space)
```     

**- YAML**

```yaml
# YAML
formatter:
  class: 'Grav\Framework\File\Formatter\YamlFormatter'
  options:
    file_extension: '.yaml'
    inline: 5           # Save with up to 4 expanded levels
    indent: 2           # Indent with 2 spaces
    native: true        # Use native YAML decoder if available
    compat: true        # If YAML cannot be decoded, use compatibility mode (SLOW)
```

**- Markdown**

```yaml
# Markdown
formatter:
  class: 'Grav\Framework\File\Formatter\MarkdownFormatter'
  options:
    file_extension: '.md'
    header: 'header'    # Header variable eg. header.title
    body: 'markdown'    # Body variable
    raw: 'frontmatter'  # RAW YAML variable
    yaml:
      inline: 20        # YAML options, see YAML formatter from above
```
      
**- Serailize**

```yaml
# PHP Serialize
formatter:
   class: 'Grav\Framework\File\Formatter\SerializeFormatter'
   options:
      file_extension: '.ser'
      decode_options:
         allowed_classes: ['stdClass'] # List of allowed / safe classes during unserialize
```

**- INI**

```yaml
# INI
formatter:
   class: 'Grav\Framework\File\Formatter\IniFormatter'
   options:
     file_extension: '.ini'
```

**- CSV**

```yaml
# CSV
formatter:
   class: 'Grav\Framework\File\Formatter\CsvFormatter'
   options:
     file_extension: ['.csv', '.tsv']
     delimiter: ','      # Delimiter to separate the values
     mime: 'text/x-csv'  # MIME type for downloading file
```

Vous pouvez également définir l'ordre par défaut, qui est défini par la paire <code>key: ASC|DESC</code> : 

```yaml
config:
   data:
     # Ordering Options
     ordering:
        key: ASC
        timestamp: ASC
```
 
Enfin, vous pouvez ajouter des champs de recherche, qui sont consultés lorsque vous appelez <code>collection.search()</code> :
 
```yaml
config:
   data:
      search:
         # Fields to be searched
         fields:
            - last_name
            - first_name
            - email
         # Search Options
         options:
            - contains: 1   # If field contains the search string, assign weight 1 to the object
```

Les <strong>Champs</strong> contiennent une liste de champs à rechercher.

Les options de recherche peuvent être :

| **Nom**         | **Valeur**         | **Description**
| ---------       | ---------          | ---------
| case_sensible   | `true` ou `false`  | Si true, toutes les vérifications sont sensibles à la casse, la valeur<br> par défaut est false.
| same_as         | 0 ... 1            | La valeur du champ doit être identique à la chaîne de recherche.
| starts_with     | 0 ... 1            | La valeur du champ doit commencer par la chaîne de recherche.
| ends_with       | 0 ... 1            | La valeur du champ doit se terminer par la chaîne de recherche.
| contains        | 0 ... 1            | La valeur du champ doit contenir la chaîne de recherche.

La fonction de recherche renvoie 0 si le champ ne correspond pas et pondère entre 0 et 1 s'il y a une correspondance. Le poids est utilisé pour classer les résultats de la recherche. L'objet qui obtient le noyau le plus élevé a une meilleure correspondance que celui avec un score inférieur.

<div class="notice tip">
Actuellement, la recherche ne prend pas en charge les pondérations ou stratégies différentes par champ.
</div>

<h2 id="Config > Admin">Config > Admin.
<a href="#Config > Admin" class="toc-anchor after"></a></h2>

La section Admin contient diverses options de configuration pour personnaliser l'administration de l'annuaire. Il contient quelques sections principales : `router`, `actions`, `permissions`, `menu`, `template` et `views`.

```yaml
config:
   # Admin Settings
   admin:
      # Admin router
      router: {}
      # Allowed admin actions
      actions: {}
      # Permissions
      permissions: {}
      # Admin menu
      menu: {}
      # Admin template type
      template: pages
      # Admin views
      views: {}
```

La section facultative `router` peut être utilisée pour personnaliser les routes d'administration de **Flex Directory**. Le routage prend en charge un chemin de base, des itinéraires personnalisables pour chaque action ainsi que des redirections pour gérer, par exemple, la rétrocompatibilité. Tous les chemins sont relatifs à l'URL de base de l'administrateur.

```yaml
config:
   admin:
      # Admin router
      router:
         path: '/contacts' # Custom path to the directory
         actions:
            configure: # Action name
               path: '/contacts/configure' # New path to the action.
         redirects: # List of redirects (from: to)
            '/flex-objects/contacts': '/contacts'
```

Parfois, vous souhaitez restreindre l'administration pour n'afficher que les entrées ou, par exemple, pour autoriser uniquement la modification de celles existantes. Pour ces `actions`, vous pouvez modifier les opérations CRUD autorisées pour mieux répondre à vos besoins.

```yaml
config:
   admin:
      # Allowed admin actions (for all users, including super user)
      actions:
         list: true   # Needs to be true (may change in the future)
         create: true # Set to false to disable creating new objects
         read: true   # Set to false to disable link to edit / details of the objects
         update: false # Set to false to disable saving existing objects
         delete: false # Set to false to disable deleting objects
```

L'exemple ci-dessus empêchera la sauvegarde des objets existants et leur suppression pour chaque utilisateur, y compris le super administrateur.

La section Autorisations vous permet d'ajouter de nouvelles règles d'autorisation pour Grav. Ces règles apparaîtront dans l'administration des utilisateurs/groupes. Vous pouvez créer autant de règles d'autorisation que vous le souhaitez, mais vous devez ajouter votre propre logique ou `authorize` les règles dans ce fichier pour les utiliser.

```yaml
config:
   admin:
      # Permissions
      permissions:
         # Primary permissions (used for the objects)
         admin.contacts:
            type: crudl # Create, Read, Update, Delete, List
            label: Contacts Directory
         # Secondary permissions (you need to assign these to a view, otherwise these will not be used)
         admin.configuration.contacts:
            type: default # Simple permission
            label: Contacts Configuration
```

Si vous ne souhaitez pas afficher votre répertoire dans l'administration des `Flex Objects`, une option consiste à afficher l'élément de `menu` dans la navigation principale. 


```yaml
config:
   admin:
      # Admin Menu
      menu:
         list:
            hidden: false # If true, hide the menu item.
            route: '/contacts' # Alias to `config.admin.router.path` if router path is not set.
            title: Contacts
            icon: fa-address-card-o
            authorize: ['admin.contacts.list', 'admin.super'] # Authorization needed to access the menu item.
            priority: 2 # Priority -10 .. 10 (highest goes up)
```

L'exemple ci-dessus crée un élément de menu **Contacts** pointant vers `/admin/contacts`.

Lorsque vous créez vos propres répertoires Flex, vous souhaiterez peut-être parfois partager les mêmes modèles entre tous vos répertoires personnalisés. Vous pouvez le faire avec le `template` :

```yaml
config:
   admin:
      # Admin template type (folder)
      template: contacts
```

**Flex Admin** dispose de plusieurs vues sur les objets. Par défaut, les vues suivantes sont prises en charge : `list`, `edit`, `configure` et éventuellement `preview` & `export`. Il est également possible d'ajouter vos propres vues.

```yaml
config:
   admin:
      views:
         # List view
         list: {}
         # Edit view
         edit: {}
         # Configure view
         configure: {}
         # Preview
         preview: {}
         # Data Export
         export: {}
```

<h2 id="List View">List View.
<a href="#List View" class="toc-anchor after"></a></h2>

La première vue dont vous aurez besoin est celle qui répertorie tous vos objets. Par défaut, la vue `list` utilise *VueTable* et *AJAX* pour paginer les objets. Il a besoin d'une liste de `fields` à afficher ainsi que d'`options` pour définir le nombre d'articles à afficher en même temps ainsi que le champ par défaut utilisé pour la commande.

```yaml
config:
   admin:
      views:
         # List view
         list:
            icon: fa-address-card-o
            title: Site Contacts
            fields: {}        # See below
            options:
               per_page: 20    # Default number of items per page
               order:
                  by: last_name # Default field used for ordering
                  dir: asc      # Default ordering direction
```

**Icon** et **title** sont utilisés pour personnaliser l'icône et le titre de la page de liste. **Title** prend également en charge l'utilisation du modèle Twig en utilisant le format suivant :

```yaml
            title:
               template: "{{ 'PLUGIN_CONTACTS.CONTACTS_TITLE'|tu }}"
```

**Fields** contient les champs que vous souhaitez afficher dans la liste du répertoire. Chaque champ possède une clé, qui est le nom du champ. La valeur peut être omise ou contenir les options de configuration suivantes :

| **Nom**      | **Valeur**   | **Exemple**  | **Description** |
| ---------    | ---------    | ---------    | ----------
| width 	      | `integer`    | 8 	         | Largeur du champ en pixels.
| alias 	      | `string`     | 'header.published' | Nom du champ du formulaire à utiliser. VueTable<br> n'aime pas les points dans les noms, alors définissez<br> un alias pour les variables imbriquées.
| field 	      | `array`      | | Remplacement du champ du formulaire. Écrit comme<br> n’importe quel champ de formulaire, mais sans clé.
| link 	      | `string`     | 'edit' 	| Ajoute un lien d'édition au texte.
| search 	   | `boolean`    | true 	   | Ajoute un lien d'édition au texte.
| sort 	      | `array`      | field: 'first_name' |Vous pouvez spécifier une valeur différente si vous<br> utilisez un nom de champ différent lors de<br> l'interrogation de données côté serveur, par<br> exemple : prénom.
| title_class 	| `string`     | 'center' 	| Classes CSS utilisées dans les titres.
| data_class 	| `string`     | 'left' 	| Classes CSS utilisées dans les colonnes de données.

<h2 id="Edit View">Edit View.
<a href="#Edit View" class="toc-anchor after"></a></h2>

La vue Modifier possède les mêmes options de configuration de base que la vue Liste :

```yaml
config:
   admin:
      views:
         # Edit view
         edit:
            icon: fa-address-card-o
            title:
               template: '{{ object.last_name ?? ''Last'' }}, {{ object.first_name ?? ''First Name'' }}'
```

<h2 id="Configure view">Configure view.
<a href="#Configure view" class="toc-anchor after"></a></h2>

La vue Configurer vous permet d'ajouter des options de configuration à l'échelle du répertoire, qui peuvent ensuite être utilisées dans les fichiers modèles.

```yaml
config:
   admin:
      views:
      # Configure view
      configure:
         hidden: false # Configuration button can be hidden, for example if you have custom tab to replace it, like in Accounts.
         authorize: 'admin.configuration.contacts' # Optional custom authorize rule for this view.
         file: 'config://flex/contacts.yaml' # Optional file where the configuration is saved.

         icon: fa-cog
         title:
            template: "{{ directory.title }} {{ 'PLUGIN_ADMIN.CONFIGURATION'|tu }}"
```

<h2 id="Preview view">Preview view.
<a href="#Preview view" class="toc-anchor after"></a></h2>

Flex prend également en charge l'aperçu, bien qu'il fonctionne actuellement en restituant une page à partir du frontend, qui peut être définie dans le fichier de plan.

```yaml
# Preview View
preview:
   enabled: true
   route:
      template: '/contacts' # Twig template to create URL. In this case we use the list view

      icon: fa-address-card-o
         title:
            template: "{{ object.form.getValue('title') ?? object.title ?? key }}"
```

<h2 id="Export view">Export view.
<a href="#Export view" class="toc-anchor after"></a></h2>

Tous les objets peuvent être exportés dans un seul fichier, voici un exemple de configuration pour exporter des données dans un fichier YAML :

```yaml
# Data Export
export:
   enabled: true
   method: 'jsonSerialize'
   formatter:
      class: 'Grav\Framework\File\Formatter\YamlFormatter'
   filename: 'contacts'
```

<h2 id="Config > Site">Config > Site.
<a href="#Config > Site" class="toc-anchor after"></a></h2>

```yaml
config:
   # Site Settings
   site:
      templates:
         collection:
            # Lookup for the template layout files for collections of objects
            paths:
               - 'flex/{TYPE}/collection/{LAYOUT}{EXT}'
         object:
            # Lookup for the template layout files for objects
            paths:
               - 'flex/{TYPE}/object/{LAYOUT}{EXT}'
         defaults:
            # Default template variable {TYPE}; overridden by filename of this blueprint if template folder exists
         type: contacts
         # Default template variable {LAYOUT}; can be overridden in render calls (usually Twig in templates)
         layout: default
```

Les paramètres du modèle vous permettent de personnaliser les chemins de recherche du modèle et de définir le type et le nom de mise en page par défaut dans l'interface.

<h2 id="Blueprints">Blueprints.
<a href="#Blueprints" class="toc-anchor after"></a></h2>

La section Blueprints définit les options de configuration communes pour l’ensemble du Répertoire. Les options vous permettent de personnaliser un répertoire commun pour mieux répondre aux besoins du site sans nécessiter une édition manuelle des fichiers.

```yaml
blueprints:
   # Blueprint for configure view.
   configure:
      # We are inside TABS field.
      fields:
         # Add our own tab
         compatibility:
            type: tab
            title: Compatibility
            fields:
               # Fields should be prefixed with object, collection etc..
               object.compat.events:
                  type: toggle
                  toggleable: true
                  label: Admin event compatibility
                  help: Enables onAdminSave and onAdminAfterSave events for plugins
                  highlight: 1
                  default: 1
                  options:
                     1: PLUGIN_ADMIN.ENABLED
                     0: PLUGIN_ADMIN.DISABLED
                  validate:
                     type: bool
```

<div class="notice note">
ASTUCE : Ces options de configuration peuvent être modifiées dans la section <a href="../avance-flex-configuration">Configuration</a> de <a href="../avance-flex-administration">Flex Directory Administration</a>.
</div>

<div class="notice tip">
NOTE: Currently the only used configuration options are inside the cache section. For your custom settings, you need to add logic to use them by yourself.
</div>


