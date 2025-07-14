<h1 class="rem">Configuration du thème</h1>

Dans Grav, vous pouvez facilement accéder à la configuration du thème et aux informations de plan à partir de vos fichiers Twig et PHP.

<h2 id="Accéder aux informations sur le Blueprint du thème">Accéder aux informations sur le Blueprint du thème
<a href="#Accéder aux informations sur le Blueprint du thème" class="toc-anchor after"></a></h2> 

Les informations du fichier `blueprints.yaml` du thème actuellement actif peuvent être obtenues à partir de l'objet `theme`. Utilisons le fichier `blueprints.yaml` suivant comme exemple :

```yaml
name: Antimatter
slug: antimatter
type: theme
version: 1.7.0
description: "Antimatter is the default theme included with **Grav**"
icon: empire
author:
  name: Team Grav
  email: devs@getgrav.org
  url: https://getgrav.org
homepage: https://github.com/getgrav/grav-theme-antimatter
demo: http://demo.getgrav.org/blog-skeleton
keywords: antimatter, theme, core, modern, fast, responsive, html5, css3
bugs: https://github.com/getgrav/grav-theme-antimatter/issues
license: MIT
```

Vous pouvez accéder à n'importe lequel de ces éléments via `theme` en utilisant le standard **dot-syntax**.

```twig
1 | Author Email: {{ theme.author.email }}
2 | Theme License: {{ theme.license }}
```

Vous pouvez également atteindre ces mêmes valeurs depuis un plugin Grav avec la syntaxe PHP :

```twig
1 | Author Email: {{ theme.author.email }}
2 | Theme License: {{ theme.license }}
```

<h2 id="Accéder à la configuration du thème">Accéder à la configuration du thème
<a href="#Accéder à la configuration du thème" class="toc-anchor after"></a></h2> 

Les thèmes ont aussi des fichiers de configuration. Le fichier de configuration d'un thème est nommé `<themename>.yaml`. Le fichier par défaut réside dans le dossier racine du thème (`user/themes/<themename>`).

Il est **fortement** recommandé de ne pas modifier le fichier YAML par défaut du thème, mais de remplacer les paramètres du dossier `user/config/themes`. Cela garantira que les paramètres d'origine du thème restent intacts, vous permettant d'accéder rapidement aux modifications et/ou de revenir en arrière si nécessaire.

Par exemple, considérons le thème Antimatière. Par défaut, il existe un fichier appelé `antimatter.yaml` dans le dossier racine du thème. Le contenu de ce fichier de configuration ressemble à ceci :

```yaml
1 | enabled: true
2 | color: blue
```

Il s'agit d'un fichier simple, mais il vous donne une idée de ce que vous pouvez faire avec les paramètres de configuration du thème. Remplaçons ces paramètres et ajoutons-en un nouveau.

Créez donc un fichier à l'emplacement suivant : `user/config/themes/antimatter.yaml`. Dans ce fichier mettez le contenu suivant :

> _Je note que `enabled` n'est pas répété ici. Si les fichiers de configuration sont fusionnés et non simplement remplacés, cela doit être explicitement indiqué_.

```yaml
1 | color: red
2 | info: Grav is awesome!
```

Ensuite, dans vos modèles de thème, vous pouvez accéder à ces variables à l'aide de l'objet : `grav.theme.config`.

`<h1 style="color :{{ grav.theme.config.color|e }}">{{ grav.theme.config.info|e }}</h1>`

Cela devrait se traduire par :

<div class="awesome">
Grav is awesome!
</div>

En PHP vous pouvez accéder à la configuration du thème en cours avec :

```php
1 | $color = $this->grav['theme']->config()['color'];
2 | $info = $this->grav['theme']->config()['info'];
```

Simple! Le ciel est la limite concernant la configuration de vos thèmes. Vous pouvez les utiliser pour tout ce que vous voulez ! :)

<h3 id="Notation alternative">Notation alternative
<a href="#Notation alternative" class="toc-anchor after"></a></h3> 

Les alias suivants fonctionnent également :

```twig
1 | Theme Color Option: {{ config.theme.color_option|e }}
2 |    or
3 | Theme Color Option: {{ theme_var(color_option)|e }}
4 |    or
5 | Theme Color Option: {{ grav.themes.antimatter.color_option|e }} [AVOID!]
```

**Même si** `grav.themes.<themename>` **est pris en charge, il doit être évité car il rend impossible l'héritage correct du thème.**

