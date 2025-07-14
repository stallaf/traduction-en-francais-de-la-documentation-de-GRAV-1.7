<h1 class="rem">Fonctionnalités avancées  Blueprint</h1>

Il existe des fonctionnalités avancées dans les plans qui vous permettent de les étendre et d'avoir des champs dynamiques.

<h2 id="Définir des règles de validation">Définir des règles de validation.
<a href="#Définir des règles de validation" class="toc-anchor after"></a></h2>

Si vous avez besoin des mêmes règles de validation plusieurs fois, vous pouvez créer votre propre règle personnalisée.

```yaml
1  | rules:
2  |     slug:
3  |        pattern : "[a-z][a-z0-9_\-]+"
4  |        min : 2
5  |        max : 80
6  | form:
7  |     fields:
8  |        folder:
9  |           type : texte
10 |           label : Nom du dossier
11 |           validate:
12 |              rule : limace
```

L'exemple ci-dessus crée une régle `slug`, qui est ensuite utilisé dans le champ de dossier du formulaire.

<h2 id="Type de base extensible (extends@)">Type de base extensible (extends@).
<a href="#Type de base extensible (extends@)" class="toc-anchor after"></a></h2>

Vous pouvez étendre le plan existant, ce qui vous permet d'ajouter de nouveaux champs ainsi que de modifier ceux existants à partir du plan de base.

```yaml
1 | extends@ : par défaut
```

Dans le format étendu, vous pouvez spécifier un contexte de recherche pour votre fichier de base :

```yaml
1 | extends@:
2 |      type: default
3 |      context: blueprints://pages
```

Vous pouvez également étendre le plan lui-même, s'il existe plusieurs versions du même plan.

```yaml
1 | extends@: parent@
```

Il n’y a aucune limite quant au nombre de plans que vous pouvez étendre. Les champs définis dans le premier plan seront remplacés par tous les plans ultérieurs de la liste.

```yaml
1 | extends@:
2 |   - parent@
3 |   - type: default
4 |      context: blueprints://pages
```

<h3 id="Comprendre les propriétés de type et de contexte">Comprendre les propriétés de type et de contexte.
<a href="#Comprendre les propriétés de type et de contexte" class="toc-anchor after"></a></h3>

Dans les exemples ci-dessus, le `type` fait référence à un fichier et `contex` à un chemin. La propriété `contaext` utilise [Streams](avance-config-multisite#streams.md), ce qui signifie qu'elle se résout en un emplacement physique.

`context: blueprints://` par défaut donnera `/user/plugins/admin/blueprints`, le dossier blueprints de l'administrateur. `type : default` donnera `default.yaml`, lors de la recherche de fichiers. Étant donné que ces deux propriétés sont utilisées ensemble, elles génèrent un chemin complet que Grav peut comprendre : `/user/plugins/admin/blueprints/default.yaml`.

Chaque fois que vous voyez la syntaxe `:/`/ dans ces documents, vous pouvez être sûr qu'elle fait référence à un flux. Et lors de l'utilisation de `context`, ce flux doit être résolu dans un dossier existant pour fonctionner.

<h2 id="Formulaire d'intégration (import@)">Formulaire d'intégration (import@).
<a href="#Formulaire d'intégration (import@)" class="toc-anchor after"></a></h2>

Parfois, vous souhaiterez peut-être partager certains champs ou sous-formulaires entre plusieurs formulaires.

Créons des `blueprints://partials/gallery.yaml` que nous souhaitons intégrer à notre formulaire :

```yaml
1 | form:
2 |   fields:
3 |   gallery.images:
4 |      type: list
5 |      label: Images
6 |      fields:
7 |         .src :
8 |            type : text
9 |            label : Image
```

Notre formulaire comporte ensuite une section dans laquelle nous souhaitons intégrer les images de la galerie :

```yaml
1 | form:
2 |   fields:
3 |      images:
4 |         type: section
5 |         title: Images
6 |         underline: true
7 |         import@:
8 |           type: partials/gallery
9 |           context: blueprints://
```

Bien que YAML ne permette pas d'utiliser la même clé `import@` plusieurs fois, vous pouvez toujours importer plusieurs plans en ajoutant un numéro unique après `@`, par exemple `import@1`, `import@2` et ainsi de suite. Le nombre n'a d'autre signification que d'empêcher l'analyseur YAML de générer des erreurs :

```yam
1  | form:
2  |   fields:
3  |     images:
4  |        type: section
5  |        title: Images
6  |        underline: true
7  |        import@1:
8  |           type: partials/gallery
9  |           context: blueprints://
10 |        import@2:
11 |           type: partials/another-gallery
12 |           context: blueprints://
```

<h2 id="Suppression de champs/propriétés (unset-*@)">Suppression de champs/propriétés (unset-*@).
<a href="#Suppression de champs/propriétés (unset-*@)" class="toc-anchor after"></a></h2>

Si vous souhaitez supprimer un champ, vous pouvez ajouter `unset@: true` à l'intérieur. Si vous souhaitez supprimer une propriété du champ, ajoutez simplement le nom de la propriété, par exemple : `unset-options@` supprime toutes les options.

<h2 id="Remplacement des champs/propriétés (replace-*@)">Remplacement des champs/propriétés (replace-*@).
<a href="#Remplacement des champs/propriétés (replace-*@)" class="toc-anchor after"></a></h2>

Par défaut, les plans utilisent une fusion approfondie de ses propriétés. Parfois, au lieu de fusionner le contenu du champ, vous souhaitez repartir d’une table propre. Si vous souhaitez remplacer l'intégralité du champ, votre nouveau champ doit commencer par `replace@ :`

```yaml
1 | author.name:
2 |   replace@: true
3 |   type: text
4 |   label: Author name
```

Comme résultat `author.name` n'aura que deux propriétés : `type et label` quel que soit ce que le formulaire avait auparavant. Vous pouvez faire la même chose pour des propriétés individuelles :

```yaml
1 | summary.enabled:
2 |   replace-options@: true
3 |   options:
4 |     0: Yeah
5 |     1: Nope
6 |     2: Do not care
```

Note : `replace-*@` est l'alias de `unset-*@`.

<h2 id="Utilisation de la configuration (config-*@)">Utilisation de la configuration (config-*@).
<a href="#Utilisation de la configuration (config-*@)" class="toc-anchor after"></a></h2>

Il peut arriver que vous souhaitiez obtenir la valeur par défaut de la configuration Grav. Par exemple, vous souhaiterez peut-être que le champ auteur soit par défaut l'auteur du site :

```yaml
1 | form:
2 |   fields:
3 |     author:
4 |       type: text
5 |       label: Author
6 |       config-default@: site.author.name
```

Si le nom de l'auteur de votre site est `John Doe`, le formulaire est équivalent à :

```yaml
1 | form:
2 |   fields:
3 |     author:
4 |       type: text
5 |       label: Author
6 |       default: "John Doe"
```

Vous pouvez utiliser `config-*@` pour n'importe quel champ ; par exemple, si vous souhaitez modifier le champ `type`, vous pouvez simplement utiliser `config-type@: site.forms.author.type` pour vous permettre de modifier le type de champ de saisie à partir de votre configuration.

<h2 id="Utilisation des appels de fonction (data-*@)">Utilisation des appels de fonction (data-*@).
<a href="#Utilisation des appels de fonction (data-*@)" class="toc-anchor after"></a></h2>

Vous pouvez effectuer des appels de fonction avec des paramètres de vos plans pour récupérer dynamiquement une valeur pour n'importe quelle propriété de votre champ. Vous pouvez le faire en utilisant la notation `data-*@:` comme clé, où `*` est le nom du champ que vous souhaitez remplir avec le résultat de l'appel de fonction.

À titre d'exemple, nous éditons une page et nous souhaitons avoir un champ qui nous permette de changer son parent ou, en d'autres termes, de déplacer la page vers un autre emplacement. Pour cela, nous avons besoin d'une valeur par défaut qui pointe vers l'emplacement actuel ainsi que d'une liste d'options comprenant tous les emplacements possibles. Pour cela, nous avons besoin d'un moyen de demander à Grav

```yaml
1  | form:
2  |   fields:
3  |     route:
4  |       type: select
5  |       label: Parent
6  |       classes: fancy
7  |       data-default@: '\Grav\Plugin\Admin::route'
8  |       data-options@: '\Grav\Common\Page\Pages::parentsRawRoutes'
9  |       options:
10 |        '/': '- Root -'
```

Si vous modifiiez la page des membres de l'équipe, le formulaire résultant ressemblerait à ceci :

```yaml
1  | form:
2  |   fields:
3  |     route:
4  |       type: select
5  |       label: Parent
6  |       classes: fancy
7  |       default: /team
8  |       options:
9  |         '/': '- Root -'
10 |         '/home': 'Home'
11 |         '/team': 'Team'
12 |         '/team/ceo': '  Meet Our CEO'
13 |        ...
```

Bien que `data-default@:` et `data-options@:` soient probablement les propriétés de champ dynamique les plus utilisées, vous n'êtes pas limité à celles-là. Il n'y a aucune limite quant aux propriétés que vous pouvez récupérer, y compris `type`, `label, `validation` et même `fields` sous le champ actuel.

De plus, vous pouvez transmettre des paramètres à l'appel de fonction simplement en utilisant un tableau où la première valeur est le nom de la fonction et les paramètres suivent :

```yam
1 | data-default@ : ['\Grav\Theme\ImaginaryClass::getMyDefault', 'default', false]
```

<h2 id="Modification de l'ordre des champs">Modification de l'ordre des champs.
<a href="#Modification de l'ordre des champs" class="toc-anchor after"></a></h2>

Lorsque vous étendez un plan ou importez un fichier, par défaut les nouveaux champs sont ajoutés à la fin de la liste. Parfois, ce n'est pas ce que vous souhaitez faire, vous souhaiterez peut-être ajouter un élément en premier ou après un champ existant.

Si vous souhaitez créer un champ, vous pouvez indiquer son ordre à l'aide de la propriété `ordering@`. Ce champ peut contenir soit un nom de champ, soit un entier (-1 = premier élément).

Voici un exemple:

```yaml
1  | form:
2  |   fields:
3  |     route:
4  |       ordering@: -1
5  |       type: select
6  |       label: Parent
7  |       classes: fancy
8  |       default: /team
9  |       options:
10 |         '/': '- Root -'
11 |         '/home': 'Home'
12 |         '/team': 'Team'
13 |         '/team/ceo': '  Meet Our CEO'
14 |         ...
```

Cela garantit que le champ itinéraire sera le premier champ à apparaître dans le formulaire. Cela facilite l'importation et/ou l'extension d'un champ existant et le placement de vos champs supplémentaires là où vous souhaitez qu'ils soient placés.

Voici un autre exemple :

```yaml
1 | form:
2 |   fields:
3 |     author:
4 |       ordering@: header.title
5 |       type: text
6 |       label: Author
7 |       default: "John Doe"
```

Dans l'exemple ci-dessus, nous avons utilisé le nom d'un autre champ pour définir l'ordre. Dans cet exemple, nous l'avons configuré pour que le champ `author` apparaisse après le champ `title` dans le formulaire.

<div class="notice info">
Lorsque vous commandez des champs dans un plan de page, vous devez toujours référencer les noms de champs préfixés par <code>header.</code>, par exemple : <code>header.title</code> pour que la commande fonctionne.
</div>

<h2 id="Création d'un nouveau type de champ de formulaire">Création d'un nouveau type de champ de formulaire.
<a href="#Création d'un nouveau type de champ de formulaire" class="toc-anchor after"></a></h2>

Si vous créez un type de champ de formulaire spécial, qui nécessite une gestion particulière dans les plans, il existe une fonction de plugin que vous pouvez utiliser.

```php
1  | /**
2  |      * Get list of form field types specified in this plugin. Only 3  | special types needs to be listed.
4  |      *
5  |      * @return array
6  |      */
7  |     public function getFormFieldTypes()
8  |     {
9  |         return [
10 |             'display' => [
11 |                 'input@' => false
12 |             ],
13 |             'spacer' => [
14 |                 'input@' => false
15 |             ]
16 |         ];
17 |    }
```

Vous n'avez pas besoin d'enregistrer cette fonction car ce n'est pas vraiment un événement, mais elle est déclenchée lorsque l'objet plugin est construit. Le but de cette fonction est de donner des instructions supplémentaires sur la manière de gérer le champ, par exemple, le code ci-dessus rend les types d'affichage et d'espacement virtuels, ce qui signifie qu'ils n'existeront pas dans les données réelles.

Vous pouvez ajouter n'importe quelle paire  `key: value`, y compris des propriétés dynamiques telles que `data-options@` qui seront automatiquement ajoutées aux champs.

<h2 id="onBlueprintCreated ou accès aux données du plan">onBlueprintCreated ou accès aux données du plan.
<a href="#onBlueprintCreated ou accès aux données du plan" class="toc-anchor after"></a></h2>

Étant donné que les plans sont constitués de champs avec des points, l'obtention d'un champ imbriqué à partir du plan utilise la notation `/` au lieu de `.`.

```php
1 | $tabs = $blueprint->get('form/fields/tabs');
```

Cela permet d'accéder à des champs de données spéciaux, comme :

```php
1 | $name = $blueprint->get('form/fields/content.name');
2 | $name = $blueprint->get('form/fields/content/fields/.name');
```

Pour une compatibilité ascendante, vous pouvez spécifier un diviseur dans le dernier (3ème) paramètre de `set()` et `get()`.

```php
1 | $tabs = $blueprint->get('form/fields/tabs', null, '/');
```

