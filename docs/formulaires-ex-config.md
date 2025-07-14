<h1 class="rem">Exemple : Plugin Configuration</h1>

Nous avons vu dans [l'exemple précédent](formulaires-ex-blueprint.md) comment définir un blueprint pour un plugin et/ou un thème.

Voyons maintenant comment proposer des options de configuration pour un plugin ou un thème, qui seront affichées par le plugin d'administration.

Si vous souhaitez que votre plugin (ou thème) ait des options directement configurables depuis l'interface d'administration, vous devez remplir le fichier *blueprints.yaml* avec des formulaires.

Par exemple, voici le fichier **archives.yaml** du plugin **Archives** :

```yaml
1  | enabled: true
2  | built_in_css: true
3  | date_display_format: 'F Y'
4  | show_count: true
5  | limit: 12
6  | order:
7  |     by: date
8  |     dir: desc
9  | filter_combinator: and
10 | filters:
11 | category: blog
```

Ce sont les paramètres par défaut du plugin. Sans le plugin Admin pour configurer ces paramètres, l'utilisateur doit copier ce fichier dans le dossier `/user/config/plugins/` et les y installer.

En fournissant un fichier **blueprints.yaml** correctement formaté, vous pouvez autoriser l'utilisateur à modifier les paramètres depuis l'interface d'administration. Lorsque les paramètres sont enregistrés, ils sont automatiquement écrits dans `/user/config/plugins/archives.yaml` (ou sous config/themes, s'il s'agit d'un thème). La structure commence comme suit :

```yaml
1  | name: Archives
2  | version: 1.3.0
3  | description: The **Archives** plugin creates links for pages grouped by month/year
4  | icon: university
5  | author:
6  |     name: Team Grav
7  |     email: devs@getgrav.org
8  |     url: https://getgrav.org
9  | homepage: https://github.com/getgrav/grav-plugin-archives
10 | demo: http://demo.getgrav.org/blog-skeleton
11 | keywords: archives, plugin, blog, month, year, date, navigation, history
12 | bugs: https://github.com/getgrav/grav-plugin-archives/issues
13 | license: MIT
14 |
15 | form:
16 |     validation: strict
17 |     fields:
```

Voici la pièce dont nous avons besoin. Chaque champ du fichier **archives.yaml** nécessite un élément de formulaire correspondant, par exemple :

<h6 id="Toggle">Toggle
<a href="#Toggle" class="toc-anchor after"></a></h6>

```yaml
1  | enabled:
2  |     type: toggle
3  |     label: Plugin status
4  |     highlight: 1
5  |     default: 1
6  |     options:
7  |        1: Enabled
8  |        0: Disabled
9  |     validate:
10 |        type: bool
```

<h6 id="Select">Select
<a href="#Select" class="toc-anchor after"></a></h6>

```yaml
1  | date_display_format:
2  |     type: select
3  |     size: medium
4  |     classes: fancy
5  |     label: Date Format
6  |     default: 'jS M Y'
7  |     options:
8  |        'F jS Y': "January 1st 2014"
9  |        'l jS of F': "Monday 1st of January"
10 |        'D, m M Y': "Mon, 01 Jan 2014"
11 |        'd-m-y': "01-01-14"
12 |        'jS M Y': "10th Feb 2014"
```

<h6 id="Text">Text
<a href="#Text" class="toc-anchor after"></a></h6>

```yaml
1 | limit:
2 |      type: text
3 |      size: x-small
4 |      label: Count Limit
5 |      validate:
6 |      type: number
7 |      min: 1
```

L'élément racine (dans ces exemples `enabled`, `date_display_format`, `limit`) est le nom de l'option. Les composants supplémentaires de chaque champ déterminent la façon dont ce champ est affiché. Par exemple, son type (`type`), sa taille (`size`), l'étiquette affichée (`label`) et une info-bulle utile facultative qui apparaît au survol (`help`). `default` et `placeholder` vous permettent de créer des valeurs par défaut et d'améliorer le rendu des champs pour l'utilisateur.

Le reste des champs peut changer selon le type de champ. Par exemple, le type de champ `select` requiert une liste `options`.

Les options imbriquées sont accessibles via la notation par points (par exemple, `order.dir`)

```yaml
1 | order.dir:
2 |      type: toggle
3 |      label: Order Direction
4 |      highlight: asc
5 |      default: desc
6 |      options:
7 |         asc: Ascending
8 |         desc: Descending
```

Le plugin Admin définit de nombreux autres types de champs qui peuvent être utilisés, dans `plugins/admin/themes/grav/templates/forms/fields`.

Il est important de noter que lorsque `form.validation` est défini sur `strict`, comme dans l'exemple du plug-in **Archives**, vous devez ajouter des plans de formulaire pour toutes les options, sinon une erreur apparaîtra lors de l'enregistrement. Si vous souhaitez plutôt autoriser la personnalisation de quelques champs dans l'interface d'administration, pas tous, définissez `form.validation` comme `loose`.

