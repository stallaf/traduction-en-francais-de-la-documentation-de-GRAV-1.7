<h1 class="rem">Extensions</h1>

Cette page fournit des guides sur la façon d'étendre le panneau d'administration, ainsi que les meilleures pratiques pour le faire.

<h2 id="Comprendre les thèmes d'administration">Comprendre les thèmes d'administration
<a href="#Comprendre les thèmes d'administration" class="toc-anchor after"></a></h2>

Tout comme lorsque vous étendez ou modifiez un thème Grav standard, vous remplacerez les modèles pour modifier la structure et l'apparence du plugin d'administration. Cela signifie que les modèles que votre plugin déclare être utilisés à la place de ceux par défaut doivent refléter exactement la structure du thème d'administration. Par exemple, si nous voulions changer l'avatar dans la navigation de gauche, nous changerions `nav-user-avatar.html.twig`.

Dans le plugin Admin, le chemin vers les templates est : `user/plugins/admin/themes/grav/templates`, ci-après dénommé *ADMIN_TEMPLATES*. Le fichier que nous recherchons est `ADMIN_TEMPLATES/partials/nav-user-avatar.html.twig`, qui contient `<img src="https://www.gravatar.com/avatar/{{ admin.user.email|md5 } }?s=47" />`.

Dans votre plugin, le chemin vers les templates doit être : user/plugins/myadminplugin/admin/themes/grav/templates, ci-après dénommé *PLUGIN_TEMPLATES*. Le fichier correspondant devrait alors être `PLUGIN_TEMPLATES/partials/nav-user-avatar.html.twig`, qui contiendrait quelque chose comme `<img src="{{ myadminplugin_avatar_image_path }}" />`.

Ainsi, nous remplaçons le chemin vers le modèle, mais de manière non destructive. Nous ciblons uniquement le modèle pertinent, d'une manière qui ne remplace pas les modèles inutiles ou n'empêche pas d'autres thèmes d'administration d'enregistrer leurs modèles alternatifs pour la même utilisation. Pour ce faire, nous enregistrons le chemin dans notre plugin comme ceci :

```php
1  | public static function getSubscribedEvents(): array
2  | {
3  |     return [
4  |        'onAdminTwigTemplatePaths' => ['onAdminTwigTemplatePaths', 0]
5  |     ];
6  | }
7  |     
8  | public function onAdminTwigTemplatePaths($event): void
9  | {
10 |     $paths = $event['paths'];
11 |     $paths[] = __DIR__ . '/admin/themes/grav/templates';
12 |     $event['paths'] = $paths;
13 |}
```

Il est important de se rappeler que le thème utilisé dans le plugin Admin est sensible aux modèles disponibles. En règle générale, vous ne devez modifier que les modèles à faible *impact*, c'est-à-dire apporter des modifications qui ne casseront pas l'interface pour tout utilisateur qui installe votre plugin. En ce sens, il est préférable de remplacer `nav-user-avatar.html.twig` que `nav.html.twig`, car ce dernier contient beaucoup plus de fonctionnalités mais utilise `{% include 'partials/nav-user-details.html.twig' % }` pour inclure le premier.

<div class="notice note">
<strong>ASTUCE</strong> : les fichiers de modèle d'administration ont la fonction d'échappement automatique activée. Vous n'avez pas besoin d'ajouter de filtres <code>|e</code> pour échapper au contenu HTML, mais vous devez ajouter <code>|raw</code> si votre entrée est du HTML valide.
</div>

<h2 id="Ajout d'un champ personnalisé">Ajout d'un champ personnalisé
<a href="#Ajout d'un champ personnalisé" class="toc-anchor after"></a></h2>

Pour créer un champ personnalisé, nous l'ajouterons à `PLUGIN_TEMPLATES/forms/fields/myfield`. Dans le dossier *myfield*, nous avons besoin d'un modèle Twig qui déclare comment le champ fonctionnera. Le moyen le plus simple d'ajouter un champ consiste à rechercher un champ similaire dans `ADMIN_TEMPLATES/forms/fields` et à le copier pour voir comment il est structuré. Par exemple, pour ajouter un curseur de plage HTML, nous créons `PLUGIN_TEMPLATES/forms/fields/range/range.html.twig`. Dans ce fichier, nous ajoutons :

```php
1 | {% extends "forms/field.html.twig" %}
2 |     
3 | {% block input_attributes %}
4 |      type="range"
5 |      {% if field.validate.min %}min="{{ field.validate.min }}"{% endif %}
6 |      {% if field.validate.max %}max="{{ field.validate.max }}"{% endif %}
7 |      {% if field.validate.step %}step="{{ field.validate.step }}"{% endif %}
8 |      {{ parent() }}
9 | {% endblock %}
```

Cela ajoute un type de champ appelé "range", avec le type *range*, qui permet à l'utilisateur de sélectionner une valeur en faisant [glisser un bouton](http://www.html5tutorial.info/html5-range.php). Pour utiliser le nouveau champ dans un blueprint, nous ajouterions simplement ceci dans [*blueprints.yaml*](plugin-tutoriel.md#Éléments requis pour fonctionner) :

```yaml
1  | form:
2  |   fields:
3  |     radius:
4  |       type: range
5  |       label: Radius
6  |       id: radius
7  |       default: 100
8  |       validate:
9  |         min: 0
10 |         max: 100
11 |         step: 10
```

Ce qui nous donne un curseur avec une valeur par défaut de 100, où les valeurs acceptées sont comprises entre 0 et 100, et chaque valeur augmente de 10 au fur et à mesure que nous la glissons.

Nous pourrions étendre cela davantage en utilisant les blocs `prepend` ou `append` disponibles, en ajoutant par exemple un indicateur visuel de la valeur sélectionnée. Nous changeons `range.html.twig` pour contenir ceci :

```twig
1  | {% extends "forms/field.html.twig" %}
2  |    
3  | {% block input_attributes %}
4  |     type="range"
5  |     style="display: inline-block;vertical-align: middle;"
6  |     {% if field.id is defined %}
7  |         oninput="{{ field.id }}_output.value = {{ field.id }}.value"
8  |     {% endif %}
9  |     {% if field.validate.min %}min="{{ field.validate.min }}"{% endif %}
10 |     {% if field.validate.max %}max="{{ field.validate.max }}"{% endif %}
11 |     {% if field.validate.step %}step="{{ field.validate.step }}"{% endif %}
12 |     { parent() }}
13 | {% endblock %}
14 | {% block append %}
15 |     {% if field.id is defined %}
16 |        <output
17 |           name="{{ (scope ~ field.name)|fieldName }}"
18 |           id="{{ field.id }}_output"
19 |           style="display: inline-block;vertical-align: baseline;padding: 0 0.5em 5px 0.5em;">
20 |        {{ value|join(', ')|e('html_attr') }}
21 |        </output>
22 |     {% endif %}
23 | {% endblock append %}
```

Ainsi, nous ajoutons une balise `<output>` qui contiendra la valeur sélectionnée, et y ajouterons, ainsi qu'au champ lui-même, un style simple pour les aligner correctement. Nous ajoutons également un attribut `oninput` au champ, afin que la modification des valeurs mette automatiquement à jour la balise `<output>` avec la valeur. Cela nécessite que chaque champ utilisant le range-slider ait une propriété `id` unique, comme `id: radius` que nous avons déclaré ci-dessus, qui devrait être quelque chose comme `id: myadminplugin_radius` pour éviter les conflits.

<div class="notice info">
Si ce nouveau modèle doit être partagé entre le frontend et le panneau d'administration (par exemple en utilisant le dossier <code>PLUGIN_TEMPLATES</code>), vous devez échapper à toutes les variables avec <code>|e</code>. Alternativement, vous pouvez simplement accéder à <code>Configuration</code> > <code>Twig Templating</code> > <code>Autoescape variables</code>automatique et le mettre sur <code>Yes</code>.
</div>

<h2 id="Création de modèles de pages personnalisés">Création de modèles de pages personnalisés
<a href="#Création de modèles de pages personnalisés" class="toc-anchor after"></a></h2>

Comme mentionné dans [les bases du thème](themes-base.md), il existe une relation directe entre les **pages** de Grav et **les fichiers de modèle Twig** fournis dans un thème/plugin. Pour créer un modèle de page personnalisé, vous aurez besoin d'un fichier de modèle pour définir les champs du plugin Admin et d'un fichier de modèle pour le rendu du contenu.

<h3 id="Ajouter un template personnalisé de page  à un thème/plugin">Ajouter un template personnalisé de page  à un thème/plugin
<a href="#Ajouter un template personnalisé de page  à un thème/plugin" class="toc-anchor after"></a></h3>

À la racine du dossier theme/plugin, créez un dossier nommé `templates`. Dans ce dossier, créez un nouveau fichier mypage.html.twig. Ce sera le modèle Twig pour le nouveau modèle de page "mypage".

Exemple mypage.html.twig :

```twig
1 | {% extends 'partials/base.html.twig' %}
2 |
3 | {% block content %}
4 |     {{ page.header.newTextField|e }}
5 |     {{ page.content|raw }}
6 | {% endblock %}
```

Vous trouverez plus d’informations sur les thèmes Twig dans la section [Twig Primer](bases-twig.md).

Les thèmes recherchent automatiquement les fichiers de modèles dans le dossier `templates` du thème. Si le modèle est ajouté via un plugin, vous devrez ajouter le modèle via l'événement `onTwigTemplatePaths` :

```php
1  | public function onPluginsInitialized(): void
2  | {
3  |     // If in an Admin page.
4  |     if ($this->isAdmin()) {
5  |         return;
6  |     }
7  |     // If not in an Admin page.
8  |     $this->enable([
9  |         'onTwigTemplatePaths' => ['onTwigTemplatePaths', 1],
10 |     ]);
11 | }
12 | 
13 | /**
14 |  * Add templates directory to twig lookup paths.
15 |  */
16 | public function onTwigTemplatePaths(): void
17 | {
18 |     $this->grav['twig']->twig_paths[] = __DIR__ . '/templates';
19 | }
```

<h3 id="Ajouter un blueprint personnalisé de page  à un thème/plugin">Ajouter un blueprint personnalisé de page  à un thème/plugin
<a href="#Ajouter un blueprint personnalisé de page  à un thème/plugin" class="toc-anchor after"></a></h3>

Pour que le plugin Admin fournisse une nouvelle option de page `mypage`, créez un dossier nommé `blueprints` à la racine du thème/plugin. Dans ce dossier, créez un nouveau fichier mypage.yaml. C'est ici que vous définirez les champs personnalisés que le plugin Admin affichera lors de la création d'une nouvelle page. Les champs de formulaire disponibles se trouvent dans le chapitre Formulaires.

L'exemple de plan `mypage.yaml` ci-dessous étend le modèle de page par défaut, puis ajoute header.newTextField sous l'onglet contenu. :

``` yaml
1  | title: My Page Blueprint
2  | '@extends':
3  |     type: default
4  |     context: blueprints://pages
5  |
6  | form:
7  |   fields:
8  |     tabs:
9  |       type: tabs
10 |       active: 1
11 |       fields:
12 |         content:
13 |           type: tab
14 |           fields:
15 |              header.newTextField:
16 |               type: text
17 |               label: 'New Text Field'
```

Comme pour le dossier des `templates`, un thème ajoutera automatiquement tous les fichiers yaml de `blueprints` trouvés dans le dossier des blueprints. Si le plan est ajouté via un plugin, vous devrez ajouter le plan via l'événement `onGetPageTemplates` :

```php
1  | public function onPluginsInitialized(): void
2  | {
3  |     // If in an Admin page.
4  |    if ($this->isAdmin()) {
5  |         $this->enable([
6  |             'onGetPageBlueprints' => ['onGetPageBlueprints', 0],
7  |             'onGetPageTemplates' => ['onGetPageTemplates', 0],
8  |         ]);
9  |     }
10 | }
11 | 
12 | /**
13 |  * Add blueprint directory.
14 |  */
15 | public function onGetPageBlueprints(Event $event): void
16 | {
17 |     $types = $event->types;
18 |     $types->scanBlueprints('plugin://' . $this->name . '/blueprints');
19 | }
20 | 
21 | /**
22 |  * Add templates directory.
23 |  */
24 | public function onGetPageTemplates(Event $event): void
25 | {
26 |     $types = $event->types;
27 |     $types->scanTemplates('plugin://' . $this->name . '/templates');
28 | }
```

<h3 id="Création d'une nouvelle page">Création d'une nouvelle page
<a href="#Création d'une nouvelle page" class="toc-anchor after"></a></h3>

Après avoir défini les fichiers de plan et de modèle, créez une nouvelle page dans le panneau d'administration en cliquant sur **Add** puis en sélectionnant "Mapage": 

![Configuration Administration 1](https://learn.getgrav.org/user/pages/05.admin-panel/08.extending/myPage.jpg)

Le formulaire d'édition Admin affiche maintenant le nouveau champ personnalisé "Nouveau champ de texte": 

![Configuration Administration 2](https://learn.getgrav.org/user/pages/05.admin-panel/08.extending/myPage-customField.jpg)

