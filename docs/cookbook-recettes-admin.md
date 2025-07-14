<h1 class="rem">Recettes d'administration</h1>

Cette page contient un assortiment de problèmes et leurs solutions respectives liées aux modifications de Grav Admin.

<h2 id="Ajouter un fichier YAML personnalisé">Ajouter un fichier YAML personnalisé.
<a href="#Ajouter un fichier YAML personnalisé" class="toc-anchor after"></a></h2>

<h3 id="Problème-1">Problème :
<a href="#Problème-1" class="toc-anchor after"></a></h3>

Vous souhaitez fournir un groupe de champs d'entreprise modifiables par l'utilisateur à l'échelle du site, comme `system.yaml` ou `site.yaml`, mais dans son propre fichier dédié.

<h3 id="La solution-1 :">La solution :
<a href="#La solution-1 :" class="toc-anchor after"></a></h3>

Comme indiqué dans la section [Bases / Configuration](/configuration), la première étape consiste à fournir votre nouveau fichier de données YAML, par exemple : `user/config/details.yaml` :

```config
1 | name: 'ABC Company Limited'
2 | address: '8732 North Cumbria Street, Golden, CO, 80401'
3 | email:
4 |   general: 'hello@abc-company.com'
5 |   support: 'support@abc-company.com'
6 |   sales: 'sales@abc-company.com'
7 | phone:
8 |   default: '555-123-1111'
```

Vous devez maintenant fournir le fichier de plan approprié pour définir le formulaire. Le blueprint peut être fourni par un plugin, mais l'approche la plus simple consiste simplement à mettre le blueprint dans un fichier : `user/blueprints/config/details.yaml`

Si vous vouliez fournir le plan via un plugin, vous devez d'abord ajouter ce code à votre plugin juste après la définition de la classe :

```php
1 | class MyPlugin extends Plugin
2 | {
3 |     public $features = [
4 |         'blueprints' => 1000,
5 |     ];
6 |     protected $version;
7 |    ...
```

Ajoutez ensuite ce code à votre méthode `onPluginsInitialized()` :

```php
1 | if ($this->isAdmin()) {
2 |     // Store this version and prefer newer method
3 |     if (method_exists($this, 'getBlueprint')) {
4 |         $this->version = $this->getBlueprint()->version;
5 |     } else {
6 |         $this->version = $this->grav['plugins']->get('admin')->blueprints()->version;
7 |     }
8 | }
```

Créez ensuite un fichier appelé` user/plugins/myplugin/blueprints/config/details.yaml`

Le fichier de plan réel doit contenir une définition de formulaire qui correspond aux données de configuration :

```yaml
1  | title: Company Details
2  | form:
3  |     validation: loose
4  |     fields:
5  | 
6  |         content:
7  |             type: section
8  |            title: 'Details'
9  |            underline: true
10 |         name:
11 |             type: text
12 |             label: 'Company Name'
13 |             size: medium
14 |             placeholder: 'ACME Corp'
15 | 
16 |         address:
17 |             type: textarea
18 |             label: 'Address'
19 |             placeholder: '555 Somestreet,\r\nNewville, TX, 77777'
20 |             size: medium
21 | 
22 |         email:
23 |             type: array
24 |             label: 'Email Addresses'
25 |             placeholder_key: Key
26 |             placeholder_value: Email Address
27 | 
28 |         phone:
29 |             type: array
30 |             label: 'Phone Numbers'
31 |             placeholder_key: Key
32 |             placeholder_value: Phone Number
```

L'utilisation du type de champ `array` vous permettra d'ajouter des champs d'e-mail et de téléphone arbitraires selon vos besoins.

<h2 id="Ajouter une création de page personnalisée modale">Ajouter une création de page personnalisée modale.
<a href="#Ajouter une création de page personnalisée modale" class="toc-anchor after"></a></h2>

<h3 id="Problème-2">Problème :
<a href="#Problème-2" class="toc-anchor after"></a></h3>

Vous souhaitez fournir un moyen simple de créer un nouvel article de blog ou une page d'image de galerie. Nous utiliserons le billet de blog pour cet exemple. Supposons que vous souhaitiez créer un blog et créer facilement un article de blog dans le bon dossier en cliquant sur un bouton.

<h3 id="La solution-2 :">La solution :
<a href="#La solution-2 :" class="toc-anchor after"></a></h3>

Tout d'abord, créez le formulaire pour notre page modale. Créez un nouveau fichier : `user/blueprints/admin/pages/new_post.yaml`.

```yaml
1  | form:
2  |   validation: loose
3  |   fields:
4  |     section:
5  |         type: section
6  |         title: Add Post
7  | 
8  |     title:
9  |       type: text
10 |       label: Post Title
11 |       validate:
12 |         required: true
13 |
14 |     folder:
15 |       type: hidden
16 |       default: '@slugify-title'
17 | 
18 |     route:
19 |       type: hidden
20 |       default: /posts
21 | 
22 |     name:
23 |       type: hidden
24 |       default: 'post'
25 | 
26 |     visible:
27 |       type: hidden
28 |       default: ''
29 |
30 |     blueprint:
31 |       type: blueprint
```

Ce formulaire imite le formulaire par défaut modal `Add Page`. Pour le **dossier**, comme vous pouvez le voir, nous avons une valeur spéciale : `@slugify-title`. Cela signifie que le **dossier** utilisera par défaut la version simplifiée de l'entrée du formulaire de **titre**. route est `/posts` donc il le placera dans le dossier `/posts`.

le **nom** est `post` donc il utilisera le plan de la page `post`.

La deuxième étape consiste à modifier la configuration du plugin Admin. Pour ajouter du code personnalisé au fichier de configuration `admin.yaml` du plugin d'administration, créez le fichier `user/config/plugins/admin.yam` et ajoutez cet extrait :

```yaml
1 | add_modals:
2 |   -
3 |     label: Add Post
4 |     blueprint: admin/pages/new_post
5 |    show_in: bar
```

Clés/valeurs de configuration disponibles pour `add_modals` :

* `label` - texte à afficher dans le bouton
* `show_in` (default: bar) (values: bar|dropdown) - s'il faut afficher le bouton dans la **barre** ou la **liste déroulante**
* `blueprint` - blueprint utilisé par le modèle
* `template` - modèle utilisé par le modal (par défaut : partials/blueprints-new.html.twig)
* `with` - données transmises au modèle
* `link_classes` - classes à ajouter à l'élément de lien
* `modal_classes` - classes à ajouter à l'élément modal

<h2 id="Ajouter un champ de sélection personnalisé">Ajouter un champ de sélection personnalisé.
<a href="#Ajouter un champ de sélection personnalisé" class="toc-anchor after"></a></h2>

<h3 id="Problème-3">Problème :
<a href="#Problème-3" class="toc-anchor after"></a></h3>

Vous souhaitez ajouter un champ de sélection avec une grande liste de valeurs. Dans cet exemple, nous supposerons que vous souhaitez afficher une liste de pays.

<h3 id="La solution-3 :">La solution :
<a href="#La solution-3 :" class="toc-anchor after"></a></h3>

Vous pouvez créer une fonction statique et appeler le tableau depuis votre blueprint. Vous pouvez coder cette fonction soit dans le fichier php de votre thème, soit dans un plugin personnalisé.

Dans cet exemple, nous allons ajouter la fonction au thème Antimatière, nous allons donc éditer le fichier `antimatter.php` qui se trouve dans le dossier `user/themes/antimatière`.

```php
1  | <?php
2  | namespace Grav\Theme;
3  | 
4  | use Grav\Common\Theme;
5  | 
6  | class Antimatter extends Theme
7  | {
8  |     public static function countryCodes()
9  |     {
10 |         return array (
11 |             'AF' => 'Afghanistan',
12 |             'AX' => 'Åland Islands',
13 |             'AL' => 'Albania',
14 |             'DZ' => 'Algeria',
15 |             'AS' => 'American Samoa',
16 |             'AD' => 'Andorra',
17 |             'AO' => 'Angola',
18 |             'AI' => 'Anguilla',
19 |             'AQ' => 'Antarctica',
20 |             'AG' => 'Antigua & Barbuda',
21 |             'AR' => 'Argentina',
22 |             'AM' => 'Armenia',
23 |             'AW' => 'Aruba',
24 |             'AC' => 'Ascension Island',
25 |             'AU' => 'Australia',
26 |             'AT' => 'Austria',
27 |             'AZ' => 'Azerbaijan',
28 |             'BS' => 'Bahamas',
29 |             'BH' => 'Bahrain',
30 |             'BD' => 'Bangladesh',
31 |             'BB' => 'Barbados',
32 |             'BY' => 'Belarus',
33 |             'BE' => 'Belgium',
34 |             'BZ' => 'Belize',
35 |             'BJ' => 'Benin',
36 |             'BM' => 'Bermuda',
37 |             'BT' => 'Bhutan',
38 |         );
39 |     }
40 | }
```

<div class = "notice note">
Il s'agit d'une liste réduite pour une visualisation facile, mais vous pouvez copier/coller la liste complète des pays depuis <a href="https://github.com/umpirsky/country-list/blob/master/data/en_US/country.php">umpirsky/count-list</a>
</div>

Ensuite, nous appelons la fonction à partir d'un blueprint ou d'une définition de formulaire frontal comme ceci :

```yaml
1 | country:
2 |   type: select
3 |   label: Country
4 |   data-options@: '\Grav\Theme\Antimatter::countryCodes'
```

Voici à quoi cela ressemblera dans l'admin :

![Exemple](https://learn.getgrav.org/user/pages/10.cookbook/04.admin-recipes/countrylist.png)

