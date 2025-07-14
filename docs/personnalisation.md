<h1 class="rem">Personnalisation</h1>

Il existe de nombreuses façons de personnaliser un thème, et Grav ne limite vraiment pas votre créativité à ce sujet. Cependant, il existe plusieurs fonctionnalités et certaines fonctionnalités fournies par Grav pour faciliter ce processus.

<h2 id="CSS personnalisé">CSS personnalisé
<a href="#CSS personnalisé" class="toc-anchor after"></a></h2> 

Le moyen le plus simple de personnaliser un thème consiste à fournir votre propre fichier `custom.css`. Le thème par défaut de **Quark** fournit une référence à un fichier `css/custom.css` via **Asset Manager**. Heureusement, **Asset Manager** s'en charge pour nous, et si le fichier n'est pas trouvé, la référence n'est pas ajoutée au HTML.

Cependant, si vous fournissez un fichier appelé `custom.css` dans le dossier `css/` de Quark, il sera récupéré et référencé. Vous devez juste vous assurer que vous fournissez des éléments CSS avec suffisamment de [spécificité](http://www.smashingmagazine.com/2007/07/27/css-specificity-things-you-should-know/) pour remplacer le CSS par défaut. Par exemple:

<h6 id="h6SCSS/LESS personnalisé">SCSS/LESS personnalisé
<a href="#h6SCSS/LESS personnalisé" class="toc-anchor after"></a></h6> 

```css
1 | body a {
2 |   color: #CC0000;
3 | }
```

Cela remplacera la couleur de lien par défaut et utilisera une couleur **rouge** à la place.

<h2 id="SCSS/LESS personnalisé">SCSS/LESS personnalisé
<a href="#SCSS/LESS personnalisé" class="toc-anchor after"></a></h2> 

La prochaine étape après avoir fourni un fichier CSS personnalisé consiste à utiliser un fichier `_custom.scss`. Quark est écrit à l'aide de [SCSS](http://sass-lang.com/), qui est un préprocesseur compatible CSS qui vous permet d'écrire du CSS plus efficacement via l'utilisation de [variables, de structures imbriquées, de partiels, d'importations, d'opérateurs et de mix-ins](http://sass-lang.com/guide).

Cela peut sembler un peu intimidant au début, mais vous pouvez utiliser autant ou aussi peu de SCSS que vous le souhaitez, et une fois que vous aurez commencé, vous aurez du mal à revenir au CSS traditionnel. Promis !

Le thème Quark a un dossier `scss/` qui contient une variété de fichiers `.scss`. Ceux-ci doivent être compilés dans le dossier `css-compiled/`.

Vous pouvez créer un fichier appelé `scss/theme/_custom.scss` et l'importer dans le fichier `theme.scss` en bas en utilisant `@import 'theme/custom';`. Il y a plusieurs grands avantages à mettre votre code dans ce fichier :

1. Les modifications résultantes seront compilées dans le fichier `css-compiled/theme.min.css` avec tous les autres CSS.
2. Vous avez accès à toutes les variables et mix-ins disponibles pour l'un des autres SCSS utilisés dans le thème.
3. Vous avez accès à toutes les fonctionnalités et fonctionnalités SCSS standard pour faciliter le développement.

Un exemple de ce fichier serait :

<h6 id="_custom.scss">_custom.scss
<a href="#_custom.scss" class="toc-anchor after"></a></h6> 

```css
1 | body {
2 |     a {
3 |          color: darken($core-accent, 30%);
4 |     }
5 | }
```

L'inconvénient de cette approche est que ce fichier est écrasé lors de toute _mise à niveau de thème_, vous devez donc vous assurer de créer une sauvegarde de tout travail personnalisé que vous effectuez. Ce problème est résolu en utilisant l'héritage de thème comme décrit ci-dessous.

<h2 id="Wellington SCSS">Wellington SCSS
<a href="#Wellington SCSS" class="toc-anchor after"></a></h2> 

[Wellington](https://github.com/wellington/wellington) est un wrapper natif pour [libsass](http://libsass.org/) disponible pour Linux et MacOS. Il fournit une solution beaucoup plus rapide pour compiler SCSS que le compilateur scss basé sur Ruby par défaut. Par plus rapide, nous entendons environ **20 fois plus rapide !**. C'est super facile à installer (via brew):

    $ | brew install wellington

Pour en profiter pour compiler un dossier `scss` dans un `css-compiled` par css comme dans l'exemple ci-dessus, vous pouvez [utiliser ce gist](https://gist.github.com/rhukster/bcfe030e419028422d5e7cdc9b8f75a8).

<div class="notice info">
Wellington est ce que nous utilisons pour tous les thèmes <em>Team Grav</em> et cela fonctionne très bien !
</div>

<h2 id="Héritage du thème">Héritage du thème
<a href="#Héritage du thème" class="toc-anchor after"></a></h2> 

C'est l'approche préférée pour modifier ou personnaliser un thème, mais cela nécessite un peu plus de configuration.

Le concept de base est que vous définissez un thème comme le **thème de base** dont vous héritez, et ne fournissez **que les éléments que vous souhaitez modifier** et laissez le thème de base gérer le reste. Le grand avantage de cela est que vous pouvez plus facilement maintenir le thème de base à jour et à jour sans affecter directement votre thème hérité personnalisé.

Il existe deux manières d'hériter d'un thème existant :

1. Utilisation de l'interface de ligne de commande (CLI) avec le plug-in DevTools.
2. Manuellement.

<h2 id="Héritage à l'aide de la CLI">Héritage à l'aide de la CLI
<a href="#Héritage à l'aide de la CLI" class="toc-anchor after"></a></h2> 

Comme indiqué dans le [didacticiel sur les thèmes](tutoriel-theme.md), vous pouvez créer un nouveau thème à l'aide du plug-in DevTools. Mais vous pouvez aussi hériter d'un thème existant. La procédure est simple.

1. [Installez le plugin DevTools](tutoriel-theme.md#Étape 1 - Installer le plugin DevTools) si ce n'est déjà fait.
2. Suivez ensuite la procédure [créer un thème de base](tutoriel-theme.md#Étape 2 - Créer un thème de base), mais lorsqu'on vous demande de choisir un type de modèle (`Please choose a template type`) , tapez héritage (`inheritance`). Si Quark est le seul thème, il sera affiché en tant qu'option 0. Tapez donc `0` pour hériter de Quark. Votre nouveau thème hérité sera créé.
3. Copiez toutes les options du fichier YAML du thème dont vous héritez (ou du dossier `user/config/themes` si vous l'avez personnalisé) en haut du fichier de configuration YAML nouvellement créé de votre thème : `/user/themes/mytheme/ mythème.yaml`.
4. Copiez la section "form" du fichier `/user/themes/quark/blueprints.yaml` dans `user/themes/mytheme/blueprints.yaml` afin d'inclure les éléments personnalisables du thème dans l'admin. (Ou remplacez simplement le fichier et modifiez son contenu.)
5. Modifiez votre thème par défaut pour utiliser votre nouveau *mytheme* en éditant l'option `pages : thème :` dans votre fichier de configuration `user/config/system.yaml` :

```yaml
1 | pages :
2 |   thème : mythème
```

<h2 id="Héritage manuel">Héritage manuel
<a href="#Héritage manuel" class="toc-anchor after"></a></h2> 

Pour y parvenir, vous devez suivre ces étapes :

1. Créez un nouveau dossier : `user/themes/mytheme` pour héberger votre nouveau thème.

2. Copiez le fichier YAML du thème du thème dont vous héritez (ou du dossier `user/config/themes` si vous l'avez personnalisé) dans `/user/themes/mytheme/mytheme.yaml` et ajoutez le contenu suivant (en remplaçant `user/themes/quark` avec le nom du thème dont vous héritez) :

```yaml
1 | streams:
2 |  schemes:
3 |    theme:
4 |      type: ReadOnlyStream
5 |      prefixes:
6 |        '':
7 |          - user/themes/mytheme
8 |          - user/themes/quark
```

3. Copiez le fichier `/user/themes/quark/blueprints.yaml` dans `/user/themes/mytheme/blueprints.yaml` afin d'inclure les éléments personnalisables du thème dans l'admin.

4. Modifiez votre thème par défaut pour utiliser votre nouveau **mytheme** en éditant l'option `pages : thème :` dans votre fichier de configuration `user/config/system.yaml :`

```yaml
1 | pages :
2 |   thème : mythème
```

5. Créez un nouveau fichier de classe de thème qui peut être utilisé pour ajouter des fonctionnalités avancées pilotées par les événements. Créez un fichier `user/themes/mytheme/mytheme.php` :

```php
1 | <?php
2 | namespace Grav\Theme;
3 | 
4 | class Mytheme extends Quark
5 | {
6 |    // Some new methods, properties etc.
7 | }
8 | ?>
```

Vous avez maintenant créé un nouveau thème appelé **mytheme** et configuré les flux de sorte qu'il regarde d'abord dans le thème **mytheme**, puis essaie **quark**. Donc, en substance, Quark est le thème de base de ce nouveau thème.

Vous pouvez ensuite fournir uniquement les fichiers dont vous avez besoin, y compris **JS**, **CSS** ou même des modifications aux **fichiers de modèle Twig** si vous le souhaitez.

<h2 id="Utilisation de SCSS">Utilisation de SCSS
<a href="#Utilisation de SCSS" class="toc-anchor after"></a></h2> 

Afin de modifier des fichiers **SCSS** spécifiques, nous devons utiliser une petite configuration pour qu'il sache d'abord regarder dans votre nouvel emplacement `mytheme`, puis `quark` ensuite. Cela nécessite plusieurs choses.

1. Tout d'abord, vous devez copier le fichier SCSS principal de quark qui contient tous les appels `@import` pour divers sous-fichiers. Donc, copiez le fichier `theme.scss` de `quark/scss/` vers le dossier `mytheme/scss/`.
2. Dans le fichier `theme.scss`, remplacez le début de toutes les lignes d'importation par `@import '../../quark/scss/theme/';` il saura donc utiliser les fichiers du thème quark. Ainsi, par exemple, la première ligne sera `@import '../../quark/scss/theme/variables';`.
3. Ajoutez `@import 'theme/custom';` tout en bas du fichier `theme.scss`.
4. L'étape suivante consiste à créer un fichier situé dans `mytheme/scss/theme/_custom.scss`. C'est là que vos modifications iront.
5. Copiez les fichiers `gulpfile.js` et `package.json` dans le dossier de base du nouveau thème.

Afin de compiler le nouveau scss pour le **mytheme**, vous devrez ouvrir un terminal et accéder au dossier du thème. Quark utilise gulp pour compiler le sass, vous aurez donc besoin de ceux installés et du fil pour les dépendances. Exécutez `npm install -g gulp, yarn install`, puis `gulp watch`. Désormais, toutes les modifications apportées aux fichiers seront recompilées.

