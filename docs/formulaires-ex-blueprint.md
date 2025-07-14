<h1 class="rem">Exemple: Plugin Blueprint</h1>

Un plugin blueprint donne à Grav un aperçu de ce qu'est un plugin, sa source, ses informations de support et d'auteur, ses dépendances et les champs de formulaire utilisés pour administrer le plugin dans Grav Admin.

A titre d'exemple, voici le Blueprint pour un plugin :

```yaml
1  | name: Assets
2  | slug: assets
3  | type: plugin
4  | version: 1.0.4
5  | description: "This plugin provides a convenient way to add CSS and JS assets directly from your pages."
6  | icon: list-alt
7  | author:
8  |     name: Team Grav
9  |     email: devs@getgrav.org
10 |     url: https://getgrav.org
11 | homepage: https://github.com/getgrav/grav-plugin-assets
12 | demo: https://learn.getgrav.org
13 | keywords: assets, javascript, css, inline
14 | bugs: https://github.com/getgrav/grav-plugin-assets/issues
15 | license: MIT
16 |
17 | dependencies:
18 |   - { name: afterburner2 }
19 |   - { name: github }
20 |  - { name: email, version: '~2.0' }
```

Il existe différentes propriétés que vous pouvez utiliser pour donner une identité à votre ressource. Certains sont **obligatoires**, d'autres sont facultatifs.

| **PROPRIÉTÉ**            | **DESCRIPTION**
| ---------                | ---------
| **name**\*               | Il s'agit du nom de la ressource. Évitez d'ajouter un plugin ou un thème, cela n'est <br>pas nécessaire.
| **slug**\*               | Il s'agit de l'identifiant unique de la ressource, il est également utilisé pour déterminer <br>le nom du dossier dans lequel la ressource est stockée, par ex. `user/plugins/__slug__`
| **type**\*               | Il s'agit du type de ressource, il doit s'agir soit d'un plugin, soit d'un thème
| **version**\*            | La version de la ressource. Cette valeur doit toujours changer à chaque version, <br>de manière incrémentielle. Vous devez également suivre la norme semver.
| **description**\*        | La description de votre ressource. Veuillez ne pas dépasser **200** caractères. Une <br>description doit être courte et aller droit au but. Vous pouvez utiliser la syntaxe <br>Markdown si nécessaire. C'est aussi une bonne idée d'envelopper votre description <br>entre guillemets.
| **icon**\*              | L'icône est ce qui sera utilisé sur [getgrav.org](https://getgrav.org/). À ce stade, nous utilisons la bibliothèque <br>d'icônes [FontAwesome](https://fortawesome.github.io/Font-Awesome/icons/), donc si vous développez un nouveau plugin ou thème, il devrait <br>être de votre devoir de vous assurer que l'icône que vous avez choisie n'est pas déjà <br>utilisée. Sinon, nous devrons le changer pour vous.
| *screeshot*             | *(facultatif)* La capture d'écran n'est évaluée que pour les *thèmes* et complètement ignorée <br>pour les *plugins*. Pour les *thèmes*, il s'agirait du nom de fichier de la capture d'écran <br>fournie avec le thème (par défaut : screenshot.jpg). Si vous avez une image <br>`screenshot.jpg` à la racine de votre thème, alors vous pouvez éviter d'utiliser cette <br>propriété. Notre référentiel le récupérera automatiquement.
| **author.name**\*       | Le nom complet du développeur
| *author.email*          | *(facultatif)* L'adresse e-mail du développeur.
| *author.url*            | *(facultatif)* La page d'accueil du développeur.
| *homepage*              | *(facultatif)* Si vous avez une page d'accueil dédiée à votre ressource, c'est l'endroit qu'il <br>vous faut.
| *docs*                  | *(facultatif)* Si vous avez écrit de la documentation pour votre ressource, vous pouvez les <br>lier ici.
| *demo*                  | *(facultatif)* Si vous avez une démo en cours d'exécution sur votre ressource, liez-la ici.
| *guide*                 | *(facultatif)* Si vous avez des didacticiels ou des guides pratiques pour votre ressource, <br>liez-les ici.
| *keywords*              | *(facultatif) *Bien qu'il n'y ait pas encore d'utilisation réelle des mots-clés, vous pouvez <br>lister ici les mots-clés relatifs à votre ressource, séparés par des virgules.
| *bugs*                  | *(facultatif)* L'URL où les bogues peuvent être signalés, généralement ce serait le lien des <br>problèmes GitHub.
| *license*               | *(facultatif)* Le type de licence de votre ressource (MIT, GPL, etc.). Il est conseillé de <br>toujours fournir un fichier LICENCE avec votre ressource.
| *dependencies*          | *(facultatif)* Une liste des dépendances requises par le plugin/thème. Le processus par <br>défaut consiste à utiliser GPM pour les installer, cependant, si une URL de référentiel GIT <br>facultative est fournie, l'installation directe à partir du référentiel sera également une <br>option. De même, si vous utilisez un tableau, vous pouvez définir explicitement un nom <br>et une version à l'aide de versions de package de style Composer.
| *gpm*                   | *(facultatif)* Indique s'il faut obtenir les mises à jour du GPM. Définir sur false pour <br>désactiver les mises à jour GPM pour les ressources non-GPM.

Voici un exemple de la partie identité du [plugin GitHub](https://github.com/getgrav/grav-plugin-github) *blueprints* :

```yaml
1  | name: GitHub
2  | slug: github
3  | type: plugin
4  | version: 1.0.1
5  | description: "This plugin wraps the [GitHub v3 API](https://developer.github.com/v3/)  
   | and uses the [php-github-api](https://github.com/KnpLabs/php-github-api/) library to  
   | add a nice GitHub touch to your Grav pages."
6  | icon: github
7  | author:
8  |     name: Team Grav
9  |      email: devs@getgrav.org
10 |     url: https://getgrav.org
11 | homepage: https://github.com/getgrav/grav-plugin-github
12 | keywords: github, plugin, api
13 | bugs: https://github.com/getgrav/grav-plugin-github/issues
14 | license: MIT
```

Le thème *blueprints* fonctionnent à peu près de la même manière que les plugins.

