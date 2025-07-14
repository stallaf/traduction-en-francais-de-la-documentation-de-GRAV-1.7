<h1 class="rem">Thème de base</h1>

Les thèmes dans Grav sont assez simples et très flexibles car ils sont construits avec le puissant [moteur Twig Template](https://twig.symfony.com/). Chaque thème est créé avec une combinaison de fichiers twig (un mélange de code PHP et HTML de type brindille), appelés modèles, et CSS. Nous utilisons généralement l' [extension CSS Sass](http://sass-lang.com/) pour générer nos fichiers CSS, mais rien ne vous empêche d'utiliser [Less](http://lesscss.org/), ou même le CSS standard. Cela dépend simplement de vos préférences personnelles.

<h2 id="Pages de contenu et modèles twig">Pages de contenu et modèles twig
<a href="#Pages de contenu et modèles twig" class="toc-anchor after"></a></h2>

La première chose à comprendre est la relation directe entre les **pages** dans Grav et les **fichiers de modèle Twig** fournis dans un thème.

Chaque page que vous créez fait référence à un fichier de modèle spécifique, soit par le nom du fichier de page, soit en définissant la variable d'en-tête de modèle pour la page. Pour une maintenance plus simple, nous vous conseillons d'utiliser le nom de la page plutôt que de le remplacer par la variable d'en-tête, dans la mesure du possible.

Travaillons à travers un exemple simple. Si vous avez [installé le package **Grav Base**](./installation.md), vous remarquerez que dans le dossier `user/pages/01.home`, vous avez un fichier appelé `default.md` qui contient le contenu basé sur le démarquage pour la page. Le nom de ce fichier, c'est-à-dire `default` indique à Grav que cette page doit être rendue avec le modèle Twig appelé `default.html.twig` qui se trouve dans le dossier `templates/` du thème.

<div class="notice info">
Les modèles de page doivent être en minuscules, comme "default", "blog", etc.
</div>

Si vous deviez avoir un fichier de page appelé `blog.md`, Grav essaierait de le rendre avec le modèle Twig : `<your_theme>/templates/blog.html.twig`.

<div class="notice info">
Les noms de fichiers dans Grav n'apparaissent pas sur le frontend de Grav. Seuls les noms de dossier le font. Ne vous inquiétez pas si tous les articles de votre blog portent le même nom de fichier. C'est normal.
</div>

<h2 id="Organisation du thème">Organisation du thème
<a href="#Organisation du thème" class="toc-anchor after"></a></h2>

<h3 id="Définition et configuration">Définition et configuration
<a href="#Définition et configuration" class="toc-anchor after"></a></h3>

Chaque thème doit avoir un fichier de définition appelé `blueprints.yaml` qui contient des informations sur le thème. Il peut éventuellement fournir des définitions de **formulaire** à utiliser dans le panneau d'administration pour permettre la modification des options de thème. Le thème **Antimatter** contient le fichier `blueprints.yaml` suivant :

```yaml
name: Antimatter
   slug: antimatter
   type: theme
   version: 1.6.7
   description: "Antimatter is the default theme included with **Grav**"
   icon: empire
   author:
      name: Team Grav
      email: devs@getgrav.org
      url: https://getgrav.org
   homepage: https://github.com/getgrav/grav-theme-antimatter
   demo: https://demo.getgrav.org/blog-skeleton
   keywords: antimatter, theme, core, modern, fast, responsive, html5, css3
   bugs: https://github.com/getgrav/grav-theme-antimatter/issues
   license: MIT

   dependencies:
      - { name: grav, version: '>=1.6.0' }

   form:
      validation: loose
      fields:
         dropdown.enabled:
            type: toggle
            label: Dropdown in navbar
            highlight: 1
            default: 1
            options:
               1: Enabled
               0: Disabled
            validate:
               type: bool
```

Si vous souhaitez utiliser les options de configuration de thème, vous devez fournir les paramètres par défaut dans un fichier appelé `<your_theme>.yaml`. Par example:

```yaml
enabled: true
color: blue
```

<div class="notice info">
L'option de configuration <code>color: blue</code> ne fait en fait rien. Il est simplement utilisé comme exemple de la manière de remplacer un paramètre.
</div>

Pour en savoir plus sur les formulaires disponibles que vous pouvez créer, reportez-vous [au chapitre 6. Formulaires](./formulaires-blueprints.md) Vous devez également fournir une image `300px` x `300px` de votre thème et l'appeler `thumbnail.jpg` à la racine du thème. Il apparaîtra dans la section thème de votre panneau d'administration.

<h3 id="Modèles">Modèles
<a href="#Modèles" class="toc-anchor after"></a></h3>

Il n'y a **pas de règles définies** concernant la structure d'un thème Grav, sauf qu'il doit y avoir des modèles Twig appropriés fournis dans le dossier `templates/` pour chacun des types de page que vous utilisez dans votre contenu.

<div class="notice info">
En raison de ce couplage étroit entre le contenu de la page et les modèles Twig dans un thème, il est souvent judicieux de développer des thèmes en conjonction avec le contenu avec lequel ils sont destinés à être utilisés. Un bon moyen de créer des thèmes généraux consiste à prendre en charge les types de modèles utilisés par les packages Skeleton disponibles sur notre <a href="https://getgrav.org/downloads">page de téléchargement</a>. Exemple de prise en charge : <strong>default</strong>, <strong>blog</strong>, <strong>error</strong>, <strong>item</strong> et <strong>modular</strong>.
</div>

De manière générale, la racine du dossier `templates/` doit être utilisée pour héberger les principaux modèles pris en charge, puis créer un sous-dossier appelé `partials/` pour contenir des parties ou des morceaux de modèles plus petits.

Si vous souhaitez prendre en charge les modèles **modulaire**s dans votre thème, vous devez également créer un sous-dossier de modèles appelé `modular/` et y stocker vos fichiers de modèles Twig modulaires.

L'histoire pour les **formulaires** de support est la même. Créez un autre sous-dossier appelé `forms/` et stockez-y tous les modèles de formulaires personnalisés.

<h2 id="SCSS / LESS / CSS">SCSS / LESS / CSS
<a href="#SCSS / LESS / CSS" class="toc-anchor after"></a></h2>

Encore une fois, il n'y a rien de gravé dans le marbre ici, mais une pratique solide consiste à avoir un sous-dossier appelé `scss/` si vous voulez développer avec Sass, ou `less/` si vous préférez Less avec un dossier `css/` pour mettre les fichiers CSS statiques , et un dossier `css-compiled/` pour tous les fichiers générés automatiquement à partir de vos compilations Sass ou Less.

La façon dont vous organisez vos fichiers ici dépend entièrement de vous. N'hésitez pas à suivre notre exemple dans le thème **antimatter** par défaut fourni avec le package Grav Base pour quelques idées. Nous utilisons la variante **scss** de Sass qui ressemble plus à CSS, et franchement plus naturelle à écrire.

Pour installer Sass sur votre ordinateur, [suivez simplement les instructions sur le site sass-lang.com](http://sass-lang.com/install).

1. Exécutez le script shell scss simple fourni en tapant `./scss.sh` à p
artir de la racine du thème.
2. Exécuter directement la commande `scss --source-map --watch scss:css-compiled` qui revient au même.

Par défaut, cela compilera vos fichiers scss dans le dossier `css-compiled/`. Vous pouvez ensuite référencer le fichier CSS résultant dans votre thème.

<h3 id="Blueprints">Blueprints
<a href="#Blueprints" class="toc-anchor after"></a></h3>

Le dossier `blueprints/` est utilisé pour définir des formulaires d'options et de configuration pour chacun des fichiers modèles. Ceux-ci sont utilisés par le **panneau d'administration** et sont facultatifs. Le thème est 100% fonctionnel sans ceux-ci, mais ils ne seront pas modifiables via le panneau d'administration, sauf s'ils sont fournis.

<h3 id="Événements thématiques et plugins">Événements thématiques et plugins
<a href="#Événements thématiques et plugins" class="toc-anchor after"></a></h3>

Une autre fonctionnalité puissante qui est purement facultative est la possibilité pour un thème d'interagir avec Grav via l'architecture des **plugins**. En bref, lors de la séquence d'initialisation de Grav, il y a plusieurs points dans la séquence où vous pouvez "accrocher" votre propre morceau de code. Cela peut être utile, par exemple, pour définir des raccourcis de chemin supplémentaires dans votre thème lors de l'initialisation de Twig, afin que vous puissiez les utiliser dans vos modèles Twig. Ces hooks sont mis à votre disposition au travers d'un ensemble de fonctions "vides" aux noms prédéfinis par le système Grav, que vous pouvez remplir à votre convenance. Le [chapitre 4. Plugins](./plugin-bases.md) contient plus d'informations sur le système de plugins et les crochets d'événement disponibles. Pour utiliser ces crochets dans votre thème, créez simplement un fichier appelé `mytheme.php` et utilisez le format suivant :

```php
<?php
namespace Grav\Theme;

use Grav\Common\Theme;

class MyTheme extends Theme
{

    public static function getSubscribedEvents(): array
    {
        return [
            'onThemeInitialized' => ['onThemeInitialized', 0]
        ];
    }

    public function onThemeInitialized(): void
    {
        if ($this->isAdmin()) {
            $this->active = false;
            return;
        }

        $this->enable([
            'onTwigSiteVariables' => ['onTwigSiteVariables', 0]
        ]);
    }

    public function onTwigSiteVariables(): void
    {
        $this->grav['assets']
            ->addCss('plugin://css/mytheme-core.css')
            ->addCss('plugin://css/mytheme-custom.css');

        $this->grav['assets']
            ->add('jquery', 101)
            ->addJs('theme://js/jquery.myscript.min.js');
    }
}
```

Comme vous pouvez le constater, pour utiliser les hooks d'événement, vous devez d'abord les enregistrer dans une liste avec la fonction `getSubratedEvents`, puis les définir avec votre propre code. Si vous vous abonnez à un événement pour l'utiliser, définissez-le également. Sinon, vous obtiendrez une erreur.

<h3 id="Autres dossiers">Autres dossiers
<a href="#Autres dossiers" class="toc-anchor after"></a></h3>

Nous vous recommandons de créer des dossiers individuels à la racine de votre thème pour `images/`, `fonts/` et `js/` afin de contenir vos images de thème personnalisées, toutes les polices Web personnalisées et les fichiers javascript requis.

<h2 id="Exemple de thème">Exemple de thème
<a href="#Exemple de thème" class="toc-anchor after"></a></h2>

Prenons le thème **antimatter** par défaut comme exemple, ci-dessous vous pouvez voir la structure globale de ce thème :

<div class="img">
<img src ="https://learn.getgrav.org/user/pages/03.themes/01.theme-basics/theme-folders.png"
alt="Dossier thématique">
</div>

Dans cet exemple, les `css`, les `css-compiled`, les `fonts`, les `images`, les `js`, `scss` et les `templates` ont été ignorés pour le rendre plus lisible. La chose importante à noter est la structure globale du thème.

