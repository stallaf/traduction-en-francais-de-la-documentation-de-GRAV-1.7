<h1 class="rem">Exemple : Page <em>Blueprints</em></h1>

**Les pages *Blueprints* ** s'étendent à partir de la page par défaut et vous permettent d'ajouter des options. Fondamentalement, les pages personnalisées peuvent prendre vie en utilisant des plans de page. Avec un plan de page, vous pouvez configurer à 100% le formulaire d'édition d'une page telle qu'elle apparaît dans l'Admin.

<h2 id="Un premier exemple">Un premier exemple
<a href="#Un premier exemple" class="toc-anchor after"></a></h2>

Si vous souhaitez utiliser le formulaire de page par défaut et ajouter simplement quelques zones de sélection par exemple, vous pouvez étendre la page par défaut.

Cela utilisera le formulaire de page par défaut et ajoutera un champ de texte à l'onglet **Avanced**, sous la section **Overrides** :

```yaml
1  | title: Gallery
2  | extends@:
3  |     type: default
4  |     context: blueprints://pages
5  |
6  | form:
7  |     fields:
8  |        tabs:
9  |           type: tabs
10 |           active: 1
11 | 
12 |           fields:
13 |              advanced:
14 |                 fields:
15 |                    overrides:
16 |                       fields:
17 |                          header.an_example_text_field:
18 |                             type: text
19 |                             label: Add a number
20 |                             default: 5
21 |                             validate:
22 |                                required: true
23 |                                type: int
```

Cela ajoutera à la place un nouvel onglet, appelé **Gallery**, avec certains champs.

```yaml
1  | title: Gallery
2  | '@extends':
3  |     type: default
4  |     context: blueprints://pages
5  | 
6  | form:
7  |     fields:
8  |        tabs:
9  |           type: tabs
10 |           active: 1
11 | 
12 |           fields:
13 |              gallery:
14 |                 type: tab
15 |                 title: Gallery
16 | 
17 |                 fields:
18 |                    header.an_example_text_field:
19 |                       type: text
20 |                       label: Add a number
21 |                       default: 5
22 |                       validate:
23 |                          required: true
24 |                          type: int
25 |
26 |                    header.an_example_select_box:
27 |                       type: select
28 |                       label: Select one of the following
29 |                       default: one
30 |                       options:
31 |                          one: One
32 |                          two: Two
33 |                          three: Three
```

Les types de champs que vous pouvez ajouter sont répertoriés dans [Champs de formulaire disponibles pour une utilisation dans l'administration](formulaires-references.md)

<h2 id="Comment nommer les champs">Comment nommer les champs
<a href="#Comment nommer les champs" class="toc-anchor after"></a></h2>

Il est important que les champs utilisent la structure `header.*`, afin que le contenu du champ soit enregistré dans l'en-tête de page lors de l'enregistrement.

<h2 id="Créer un formulaire de page entièrement personnalisé">Créer un formulaire de page entièrement personnalisé
<a href="#Créer un formulaire de page entièrement personnalisé" class="toc-anchor after"></a></h2>

Vous pouvez éviter d'étendre le formulaire par défaut et créer un formulaire de page complètement unique.

Par example:

```yaml
1  | title: Gallery
2  | 
3  | form:
4  |     fields:
5  |        tabs:
6  |           type: tabs
7  |           active: 1
88 | 
9  |           fields:
10 |              gallery:
11 |                 type: tab
12 |                 title: Gallery
13 | 
14 |                 fields:
15 |                    header.an_example_text_field:
16 |                       type: text
17 |                       label: Add a number
18 |                       default: 5
19 |                       validate:
20 |                          required: true
21 |                          type: int
22 | 
23 |                    header.an_example_select_box:
24 |                       type: select
25 |                       label: Select one of the following
26 |                       default: one
27 |                       options:
28 |                          one: One
29 |                          two: Two
30 |                          three: Three
31 | 
32 |                    route:
33 |                       type: parents
34 |                       label: PLUGIN_ADMIN.PARENT
35 |                       classes: fancy
```

<div class="notice info">
<strong>ATTENTION</strong> : le champ route a changé dans Grav 1.7. Veuillez mettre à jour vos blueprints existants pour utiliser le nouveau <code>type : parents</code>.
</div>

<h2 id="Une note pour le mode expert">Une note pour le mode expert
<a href="#Une note pour le mode expert" class="toc-anchor after"></a></h2>

Lors de la modification de pages en mode **Expert**, le **Blueprint** n'est pas lu et le formulaire de page est le même sur toutes les pages. En effet, en mode Expert, vous modifiez les champs de la page directement dans le champ **Frontmatter**, et il n'est pas nécessaire d'avoir une présentation personnalisée.

<h2 id="Où mettre les pages Blueprints">Où mettre les pages Blueprints
<a href="#Où mettre les pages Blueprints" class="toc-anchor after"></a></h2>

Pour que le plugin d'administration récupère les plans et affiche ainsi les nouveaux types de page, vous devez placer les plans au bon endroit.

<h3 id="Dans le dossier User Blueprints">Dans le dossier User Blueprints
<a href="#Dans le dossier User Blueprints" class="toc-anchor after"></a></h3>

Mettez-les dans `user/blueprints/pages/`. C'est un bon endroit pour les mettre lorsque vous voulez simplement que vos plans soient présents sur votre site.

<h3 id="Dans le thème">Dans le thème
<a href="#Dans le thème" class="toc-anchor after"></a></h3>

Mettez-les dans `user/themes/YOURTHEME/blueprints/`. C'est mieux lorsque vous avez également l'intention de diffuser votre thème : le thème fournira les pages blueprints et il sera plus facile à utiliser.

<h3 id="Dans le dossier Données">Dans le dossier Données
<a href="#Dans le dossier Données" class="toc-anchor after"></a></h3>

Si vous utilisez un thème basé sur Gantry5, le meilleur emplacement est `user/data/gantry5/themes/YOURTHEME/blueprints/`, sinon vos fichiers pourraient être perdus lors d'une mise à jour du thème.

<h3 id="Dans un plugin">Dans un plugin
<a href="#Dans un plugin" class="toc-anchor after"></a></h3>

Mettez-les dans `user/plugins/YOURPLUGIN/blueprints/`. C'est l'endroit où les mettre si vous définissez et ajoutez des pages personnalisées dans le plugin.

Ensuite, abonnez-vous à l'événement `onGetPageBlueprints` et ajoutez-les à Grav. L'exemple suivant ajoute les blueprints du dossier `blueprints/`.

```php
1  | public static function getSubscribedEvents()
2  | {
3  |     return [
4  |        'onGetPageBlueprints' => ['onGetPageBlueprints', 0]
5  | 
6  |     ];
7  | }
8  | 
9  | public function onGetPageBlueprints($event)
10 | {
11 |     $types = $event->types;
12 |     $types->scanBlueprints('plugins://' . $this->name . '/blueprints');
13 | }
```    

