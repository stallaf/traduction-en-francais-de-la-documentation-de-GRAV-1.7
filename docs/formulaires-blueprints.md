<h1 class="rem">Blueprints</h1>

<h2 id="Qu'est-ce qu'un Blueprint ?">Qu'est-ce qu'un Blueprint ?
<a href="#Qu'est-ce qu'un Blueprint ?" class="toc-anchor after"></a></h2>

Les *blueprints* sont un aspect important de Grav. Ils sont essentiellement la base de l'interaction d'un thème ou d'un plugin avec l'administrateur Grav. Ils indiquent à Grav ce qu'est un thème ou un plugin, son nom, où il peut être trouvé sur GitHub, etc. Il génère également les options de configuration pour ce thème ou ce plugin dans l'administration Grav.

Un Blueprint est défini dans un fichier YAML et peut généralement héberger des propriétés ainsi que des définitions de formulaire.

La grande majorité des utilisateurs de Grav n'auront jamais à travailler avec Blueprints. En termes simples, ils déterminent comment les plugins et les thèmes apparaissent dans le back-end du site. Pour la plupart des utilisateurs, c'est là qu'ils récupèrent, configurent leurs thèmes et plugins à l'aide de l'administrateur Grav ou manipulent les options dans le fichier YAML principal du thème ou du plugin.

Les personnes qui travailleront le plus avec Blueprints sont les développeurs qui créent de nouveaux thèmes et plugins et personnalisent les options d'une ressource dans le back-end. Il s'agit d'un outil puissant qui définit quelle est votre ressource, où Grav peut trouver des mises à jour pour celle-ci et quelles options de configuration vous devriez pouvoir définir dans le back-end.

<h2 id="Types de Blueprints">Types de Blueprints
<a href="#Types de Blueprints" class="toc-anchor after"></a></h2>

Grav utilise Blueprints pour :

* définir les informations sur les thèmes et les plugins.
* définir les options de configuration de thème/plugin à afficher dans l'Admin.
* définir les formulaires Pages dans l'Admin.
* définir les options affichées dans la section Configuration Admin.

À ce stade, nous décrirons des détails supplémentaires sur le fonctionnement des Blueprints dans Grav.

<h3 id="Thèmes et plugins">Thèmes et plugins
<a href="#Thèmes et plugins" class="toc-anchor after"></a></h3>

Lorsqu'il est utilisé avec des thèmes et des plugins, la convention consiste à mettre un **blueprints.yaml** dans le package. Cela indique à Grav les métadonnées de cette ressource, ce qui la présente à l'administrateur Grav.

Un fichier **blueprints.yaml** est une partie importante de tout thème et plugin. Il est essentiel pour le système GPM (Grav Package Manager). GPM utilise les informations stockées dans le plan pour mettre le plugin à la disposition des utilisateurs.

Dans notre [exemple de plugin blueprint](formulaires-ex-blueprint.md), nous plongeons dans le plan du plugin **Assets**. Ce plan définit le nom, les informations sur l'auteur, les mots-clés, la page d'accueil, le lien du rapport de bogue et d'autres métadonnées qui non seulement indiquent à Grav où il peut localiser les mises à jour du plug-in, mais fournissent une ressource utile à l'utilisateur accessible depuis l'administrateur Grav.

Une fois ces informations renseignées, plus bas dans la page du Blueprint, vous trouverez des informations sur les formulaires. Ces informations créent les formulaires d'administration accessibles par l'utilisateur dans le backend de Grav. Par exemple, si vous vouliez ajouter une bascule qui active ou désactive une fonctionnalité particulière de ce plugin, vous l'ajouteriez ici.

![Formulaires d'administration 1](https://learn.getgrav.org/user/pages/06.forms/01.blueprints/blueprints_1.png)

Le fichier **blueprints.yaml** fonctionne avec le fichier YAML nommé du plugin (exemple : **assets.yaml**). Le blueprint définit les options configurables et le fichier YAML nommé de la ressource définit leurs valeurs. C'est ce fichier YAML nommé qui est ensuite dupliqué dans la section `user/config` de l'instance Grav pour remplacer ces valeurs par défaut manuellement ou via l'administrateur Grav.

Donc, essentiellement, lorsqu'il s'agit d'une option de configuration pour un thème ou un plugin, le fichier **blueprints.yaml** le définit, et le fichier YAML nommé pour cette ressource vous indique à quoi il est défini.

<h3 id="Pages">Pages
<a href="#Pages" class="toc-anchor after"></a></h3>

Les pages Grav peuvent vraiment être n'importe quoi. Une page peut être une liste de blog, un article de blog, une page de produit, une galerie d'images, etc.

Ce qui détermine ce qu'une page doit faire et comment elle doit apparaître est le **Plan de Page**.

Grav fournit quelques plans de page de base : par défaut et modulaire. Ce sont les deux principaux éléments constitutifs de Grav.

Des plans de page supplémentaires sont ajoutés et configurés par le thème, qui peut décider d'ajouter autant de plans de page que possible, ou de se concentrer sur certains plans de page particuliers axés sur ce qu'il doit faire.

Un thème Grav est beaucoup plus flexible et puissant que ce à quoi vous pourriez être habitué sur d'autres plates-formes.

Cela permet aux thèmes d'être spécifiques à l'application. Par exemple, un thème peut se spécialiser dans l'un de ces objectifs :

* construire un site de documentation, comme celui que vous êtes en train de lire.
* création d'un site e-commerce.
* construire un blog.
* construire un site de portefeuille.

Un thème peut également permettre à ses utilisateurs de tous les créer, mais généralement, un thème affiné créé dans un seul but peut mieux satisfaire cet objectif qu'un thème générique.

Un fichier de page est utilisé par une page en définissant son nom de fichier de démarquage, par ex. `blog.md`, `default.md` ou `form.md`.

Chacun de ces fichiers utilisera un fichier de page différent. Vous pouvez également modifier le type de fichier [à l'aide de la propriété d'en-tête template](en-tete-frontmatter.md).

Le modèle utilisé par une page détermine non seulement le "look and feel" dans le frontend, mais détermine également comment le plugin d'administration le rendra, vous permettant d'ajouter des options, de sélectionner des cases, des entrées personnalisées et des bascules.

Comment faire : dans votre thème, ajoutez un dossier `blueprints/` et ajoutez un fichier YAML avec le nom du modèle de page que vous avez ajouté. Par exemple, si vous ajoutez un modèle de page de `blog`, ajoutez un fichier `blueprints/blog.yaml`. Vous pouvez trouver un [exemple de ce répertoire dans le thème **Antimatter**](https://github.com/getgrav/grav-theme-antimatter/tree/develop/blueprints).

<h2 id="Composants d'un Blueprint">Composants d'un Blueprint
<a href="#Composants d'un Blueprint" class="toc-anchor after"></a></h2>

Deux ensembles d'informations sont présentés dans un fichier **blueprints.yaml**. Le premier ensemble d'informations de métadonnées est l'identité de la ressource elle-même, le second ensemble concerne les formulaires. Toutes ces informations sont stockées dans un seul fichier **blueprints.yaml** stocké à la racine de chaque plugin et thème.

Voici un exemple de la partie métadonnées d'un fichier **blueprints.yaml** :

```yaml
1  | name: GitHub
2  | slug: github
3  | type: plugin
4  | version: 1.0.1
5  | description: "This plugin wraps the [GitHub v3 API](https://developer.github.com/v3/) 6  | and uses the [php-github-api](https://github.com/KnpLabs/php-github-api/) library to 7  | add a nice GitHub touch to your Grav pages."
8  | icon: github
9  | author:
10 |   name: Team Grav
11 |  email: devs@getgrav.org
12 |  url: https://getgrav.org
13 | homepage: https://github.com/getgrav/grav-plugin-github
14 | keywords: github, plugin, api
15 | bugs: https://github.com/getgrav/grav-plugin-github/issues
16 | license: MIT
```

Comme vous pouvez le voir ici, cette zone contient de nombreuses informations d'identification générales sur le plugin, y compris son nom, son numéro de version, sa description, les informations sur l'auteur, la licence, les mots-clés et les URL où vous pouvez trouver plus d'informations ou signaler des bogues. Vous pouvez voir cette section en action dans la capture d'écran prise à partir de Grav Admin ci-dessous.

![Formulaires d'administration 2](https://learn.getgrav.org/user/pages/06.forms/01.blueprints/blueprints_2.png)

La section suivante est la zone des formulaires, qui se trouve juste quelques espaces en dessous des données répertoriées ci-dessus. Cette zone du blueprint génère des formulaires et des champs utilisés pour configurer le plugin à partir de Grav Admin. Voici un exemple rapide de cette zone du fichier **blueprints.yaml**.

```yaml
1  | form:
2  |  validation: strict
3  |  fields:
4  |     enabled:
5  |        type: toggle
6  |        label: Plugin status
7  |        highlight: 1
8  |        default: 1
9  |        options:
10 |           1: Enabled
11 |           0: Disabled
12 |        validate:
13 |           type: bool
```

Cette zone du fichier crée toutes les options administratives accessibles dans Grav Admin. Dans ce cas particulier, nous avons créé une simple bascule** d'état du plugin** qui permet à l'utilisateur d'activer ou de désactiver le plugin depuis l'administrateur (illustré ci-dessous).

![Formulaires d'administration 3](https://learn.getgrav.org/user/pages/06.forms/01.blueprints/blueprints_3.png)

<h2 id="Débogage des Blueprints">Débogage des Blueprints
<a href="#Débogage des Blueprints" class="toc-anchor after"></a></h2>

Les erreurs dans les fichiers Blueprint peuvent entraîner des résultats inattendus.

<div class="notice tip">
<strong>ASTUCE</strong> : vous pouvez exécuter la commande <strong>CLI command</strong> <code>bin/grav yamllinter</code> pour obtenir un rapport sur les erreurs dans les fichiers yaml. Cela peut être d'une valeur inestimable lors de la modification de fichiers yaml.
</div>

