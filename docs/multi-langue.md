<h1 class="rem">Multi-langue</h1>

Le support multilingue dans Grav est le résultat direct d'une excellente [discussion communautaire](https://github.com/getgrav/grav/issues/170) sur le sujet. Nous allons maintenant les décomposer et fournir des exemples sur la façon dont vous pouvez configurer votre site Grav avec plusieurs langues.

<h2 id="Langue unique différente de l'anglais">Langue unique différente de l'anglais
<a href="#Langue unique différente de l'anglais" class="toc-anchor after"></a></h2>

Si vous n'utilisez qu'une seule langue, activez les traductions et ajoutez votre code de langue dans le fichier `user/config/system.yaml` :

```yaml
languages:
   supported:
      - fr
```

ou dans la configuration du système dans l'Admin :

![Paramètres de traduction de l'administrateur](https://learn.getgrav.org/user/pages/02.content/11.multi-language/translations-settings.png)

Cela garantira que Grav utilise les chaînes de langue correctes dans le frontend. De plus, si le thème le prend en charge, il ajoutera votre code de langue à la balise HTML.

<h2 id="Bases multilingues">Bases multilingues
<a href="#Bases multilingues" class="toc-anchor after"></a></h2>

Comme vous devriez déjà savoir comment Grav utilise les fichiers Markdown dans les dossiers pour définir la structure architecturale ainsi que pour définir les options de page importantes ainsi que le contenu, nous n'aborderons pas directement ces mécanismes. Cependant, sachez que par défaut, Grav recherche un **seul** fichier `.md` dans un dossier pour représenter la page. Si vous n'êtes pas sûr d'être suffisamment familiarisé avec ce principe, veuillez vous référer à la section [Tutoriel de Base](./tutoriel.md) avant de continuer. Avec la prise en charge multilingue activée, Grav recherchera le fichier basé sur la langue appropriée, par exemple `default.en.md` ou `default.fr.md`.

<h3 id="Configuration de la langue">Configuration de la langue
<a href="#Configuration de la langue" class="toc-anchor after"></a></h3>

Pour que Grav fasse cela, vous devez d'abord configurer une configuration de langage de base dans votre fichier `user/config/system.yaml` (avec des commentaires pour une meilleure lisibilité) :

```yaml
languages:
   supported: # Supported languages:
      - en # English language
      - fr # French language
   default_lang: en # Set default language to English
   include_default_lang: true # If true, use /en/path instead of /path for default English language.
   include_default_lang_file_extension: true # If true, use .en.md file extension instead of .md for default langauge.
   content_fallback:
      en: ['en'] # No fallback for English.
      fr: ['fr', 'en'] #  French falls back to English version of the page.
```

En fournissant un bloc `langages` avec une liste des langues `supported` prises en charge, vous avez effectivement activé la prise en charge multilingue dans Grav.

Dans cet exemple, vous pouvez voir que deux langues prises en charge ont été décrites (`en` et `fr`). Ceux-ci vous permettront de prendre en charge les langues **Anglaise** et **Française**.

Si aucune langue n'est explicitement demandée (via l'URL ou par code), Grav utilisera l'ordre des langues fournies pour sélectionner la bonne langue. Ainsi, dans l'exemple ci-dessus, la langue par **défaut** est `en` ou Anglais. Si vous aviez `fr` en premier, le Français serait la langue par défaut.

Par défaut, toutes les langues reviennent à la langue par défaut. Si vous ne voulez pas faire cela, vous pouvez ignorer les replis de langue en utilisant `content_fallback`, où la clef est la langue et la valeur,  un tableau de langues.

<div class="notice info">
Vous pouvez bien sûr fournir autant de langues que vous le souhaitez et vous pouvez même utiliser des codes de type de paramètres régionaux tels que <code>en-GB</code>, <code>en-US</code> et <code>fr-FR</code>. Si vous utilisez cette dénomination basée sur les paramètres régionaux, vous devrez remplacer tous les codes de langue abrégés par les versions locales.
</div>

<h3 id="Pages multilingues">Pages multilingues
<a href="#Pages multilingues" class="toc-anchor after"></a></h3>

Par défaut dans Grav, chaque page est représentée par un fichier de démarquage, par exemple `default.md`. Lorsque vous activez la prise en charge multilingue, Grav recherchera le fichier Markdown nommé de manière appropriée. Par exemple, comme l'anglais est notre langue par défaut, il cherchera d'abord `default.en.md`.

Si ce fichier n'est pas trouvé, il reviendra à la valeur par défaut de Grav et recherchera `default.md` pour fournir des informations à la page.

<div class="notice info">
Ce comportement par défaut a changé dans <strong>Grav 1.7</strong>. Dans le passé, Grav affichait une page Anglaise inexistante en Français, maintenant toutes les langues retombent uniquement sur la langue par défaut si elles ne sont pas spécifiées autrement dans <code>content_fallback</code>. Ainsi, si la page est introuvable dans les langues de secours, la <strong>page d'erreur 404</strong> s'affiche à la place.
</div>

Si nous avions le plus basique des sites Grav, avec un seul fichier `01.home/default.md`, nous pourrions commencer par renommer `default.md` en `default.en.md`, et son contenu pourrait ressembler à ceci :

```yaml
---
title: Homepage
---

This is my Grav-powered homepage!
```

Ensuite vous pourriez créer une nouvelle page située dans le même dossier `01.home/` appelé `default.fr.md` avec le contenu :

```yaml
---
titre : Page d'accueil
---

Ceci est ma page d'accueil générée par Grav !
```

Vous avez maintenant défini deux pages pour votre page d'accueil actuelle en plusieurs langues.

<div class="notice note">
Si vous convertissez un site existant pour utiliser plusieurs langues, vous pouvez également définir <code>include_default_lang_file_extension : false</code> pour continuer à utiliser l'extension de fichier <code>.md</code> ordinaire pour votre langue principale. <a href="#Extension de fichier par défaut">Lire la suite....</a>
</div>

<h3 id="Langue active via URL">Langue active via URL
<a href="#Langue active via URL" class="toc-anchor after"></a></h3>

Comme l'Anglais est la langue par défaut, si vous deviez pointer votre navigateur sans spécifier de langue, vous obtiendriez le contenu tel que décrit dans le fichier `default.en.md`, mais vous pourriez également demander explicitement l'anglais en pointant votre navigateur vers

    http://yoursite.com/en

Pour accéder à la version française, vous utiliserez bien sûr

    http://yoursite.com/fr

<div class="notice note">
Si vous préférez ne pas utiliser le préfixe de langue pour la langue par défaut, définissez <code>include_default_lang : false</code>. <a href="#Préfixe de langue par défaut">Lire la suite...</a>
</div>

<h3 id="Langue active via le navigateur">Langue active via le navigateur
<a href="#Langue active via le navigateur" class="toc-anchor after"></a></h3>

La plupart des navigateurs vous permettent de configurer les langues dans lesquelles vous préférez voir le contenu. Grav a la capacité de lire ces valeurs `http_accept_language` et de les comparer aux langues actuellement prises en charge pour le site, et si aucune langue spécifique n'a été détectée, vous montrer le contenu dans votre langue préférée.

Pour que cela fonctionne, vous devez activer l'option dans votre fichier `user/system.yaml` dans la section `languages ​​:` :

    languages:
      http_accept_language: true

<h3 id="Langage actif basé sur la session">Langage actif basé sur la session
<a href="#Langage actif basé sur la session" class="toc-anchor after"></a></h3>

Si vous souhaitez mémoriser la langue active indépendamment de l'URL, vous pouvez activer le stockage basé sur la **session** de la langue active. Pour l'activer, vous devez vous assurer que vous avez `session : enabled : true` dans le fichier `system.yaml`. Ensuite, vous devez activer le paramètre de langue :

    languages:
      session_store_active: true
 
Cela stockera alors la langue active dans la session.

<h3 id="Définir les paramètres régionaux sur la langue active">Définir les paramètres régionaux sur la langue active
<a href="#Définir les paramètres régionaux sur la langue active" class="toc-anchor after"></a></h3>

Le paramètre booléen définira la méthode PHP `setlocale()` qui contrôle des éléments tels que les valeurs monétaires, les dates, les comparaisons de chaînes, les classifications de caractères et d'autres paramètres spécifiques aux paramètres régionaux sur celui de la langue active. La valeur par défaut est `false`, puis elle utilisera les paramètres régionaux du système. Si vous définissez cette valeur sur `true`, elle remplacera les paramètres régionaux par la langue active actuelle.

    languages:
       override_locale: false

<h3 id="Préfixe de langue par défaut">Préfixe de langue par défaut
<a href="#Préfixe de langue par défaut" class="toc-anchor after"></a></h3>

Par défaut, le code de langue par défaut est préfixé dans toutes les URL. Par exemple, si vous avez un support pour l'Anglais et le Français (`en` et `fr`), et que la valeur par défaut est l'Anglais. Une route de page peut ressembler à `/en/my-page` en Anglais et `/fr/ma-page` en Français. Cependant, il est souvent préférable d'avoir la langue par défaut sans le préfixe, vous pouvez donc simplement définir cette option sur `false` et la page en Anglais apparaîtra sous la forme `/my-page`.

    languages:
       override_locale: false

<h3 id="Extension de fichier par défaut">Extension de fichier par défaut
<a href="#Extension de fichier par défaut" class="toc-anchor after"></a></h3>

Si vous convertissez un site existant pour utiliser plusieurs langues, il peut être intimidant de convertir toutes les pages existantes pour utiliser la nouvelle extension de fichier de langue `.en.md` (si vous utilisez l'Anglais). Dans ce cas, vous pouvez désactiver l'extension de langue sur votre langue d'origine.

    languages:
        include_default_lang_file_extension: false

<h3 id="Routage multilingue">Routage multilingue
<a href="#Routage multilingue" class="toc-anchor after"></a></h3>

Grav utilise généralement les noms des dossiers pour produire un itinéraire URL pour une page particulière. Cela permet à l'architecture du site d'être facilement comprise et mise en œuvre sous la forme d'un ensemble de dossiers imbriqués. Cependant, avec un site multilingue, vous souhaiterez peut-être utiliser une URL qui a plus de sens dans cette langue particulière.

Si nous avions la structure de dossiers suivante :

```yaml
- 01.animals
   - 01.mammals
      - 01.bats
      - 02.bears
      - 03.foxes
      - 04.cats
   - 02.reptiles
   - 03.birds
   - 04.insets
   - 05.aquatic
```

Cela produirait des URL telles que `http://yoursite.com/animals/mammals/bears`. C'est parfait pour un site en Anglais, mais si vous souhaitez avoir une version française, vous préféreriez qu'elle soit traduite de manière appropriée. Le moyen le plus simple d'y parvenir est d'ajouter un [slug](./en-tete-frontmatter.md#Slug) personnalisé pour chacun des fichiers de page `fr.md`. par exemple, la page des mammifères pourrait ressembler à :

```yaml
---
title: Mammifères
slug: mammiferes
---
    
Les mammifères (classe des Mammalia) forment un taxon inclus dans les vertébrés,
traditionnellement une classe, définie dès la classification de Linné. Ce taxon 
est considéré comme monophylétique...
```

Ceci combiné avec les **slug-overrides** appropriés dans les autres fichiers devrait aboutir à une URL de `http://yoursite.com/animaux/mammiferes/ours` qui ressemble beaucoup plus au Français !

Une autre option consiste à utiliser la prise en charge des [routes au niveau de la page](./en-tete-frontmatter.md#Routes) et à fournir un alias de route complet pour la page.

<h3 id="Page d'accueil basée sur la langue">Page d'accueil basée sur la langue
<a href="#Page d'accueil basée sur la langue" class="toc-anchor after"></a></h3>

Si vous remplacez la route/slug pour la page d'accueil, Grav ne pourra pas trouver la page d'accueil telle que définie par votre option `home.alias` dans votre `system.yaml`. Il recherchera `/homepage` et votre page d'accueil Française pourrait avoir un itinéraire de `/page-d-accueil`.

Afin de prendre en charge les pages d'accueil multilingues, Grav a une nouvelle option qui peut être utilisée à la place de `home.alias` et qui est simple `home.aliases` et cela pourrait ressembler à ceci :

```yaml
home:
   aliases:
      en: /homepage
      fr: /page-d-accueil
```

De cette façon, Grav sait comment vous diriger vers la page d'accueil si la langue active est l'Anglais ou le Français.

<h3 id="Modèles Twig basés sur la langue">Modèles Twig basés sur la langue
<a href="#Modèles Twig basés sur la langue" class="toc-anchor after"></a></h3>

Par défaut, Grav utilise le nom de fichier Markdown pour déterminer le modèle Twig à utiliser pour le rendu. Cela fonctionne avec plusieurs langues de la même manière. Par exemple, `default.fr.md` recherchera un fichier Twig appelé `default.html.twig` dans les chemins de modèle Twig appropriés du thème actuel et de tous les plugins qui enregistrent les chemins de modèle Twig. Avec le multi-langue, Grav ajoute également la langue active actuelle à la structure du chemin. Cela signifie que si vous avez besoin d'un fichier Twig spécifique à une langue, vous pouvez simplement les placer dans un dossier de langue de niveau racine. Par exemple, si votre thème actuel utilise un modèle situé dans templates`/default.html.twig`, vous pouvez créer un dossier `templates/fr/` et y placer votre fichier Twig spécifique au français : `templates/fr/default.html.twig`.

Une autre option qui nécessite une configuration manuelle consiste à remplacer le paramètre `template:` dans les en-têtes de page. Par example:

    template: default.fr

Cela recherchera un modèle situé à `templates/default.fr.html.twig`.

Cela vous offre deux options pour fournir des remplacements Twig spécifiques à la langue.

<div class="notice info">
Si aucun modèle Twig spécifique à la langue n'est fourni, celui par défaut sera utilisé.
</div>

<h3 id="Traduction via Twig">Traduction via Twig
<a href="#Traduction via Twig" class="toc-anchor after"></a></h3>

La façon la plus simple d'utiliser ces chaînes de traduction dans vos modèles Twig consiste à utiliser le filtre Twig `|t`. Vous pouvez également utiliser la fonction  Twig`t()`, mais franchement, le filtre est plus propre et fait la même chose :

```html
<h1 id="site-name">{{ "SITE_NAME"|t|e }}</h1>
<section id="header">
   <h2>{{ "HEADER.MAIN_TEXT"|t|e }}</h2>
   <h3>{{ "HEADER.SUB_TEXT"|t|e }}</h3>
</section>
```

En utilisant la fonction Twig `t()`, la solution est similaire :

```html
<h1 id="site-name">{{ t("SITE_NAME")|e }}</h1>
<section id="header">
   <h2>{{ t("HEADER.MAIN_TEXT")|e }}</h2>
   <h3>{{ t("HEADER.SUB_TEXT")|e }}</h3>
</section>
```

Un autre nouveau filtre/fonction Twig vous permet de traduire à partir d'un tableau. Ceci est particulièrement utile si vous avez une liste de valeurs telles que les mois de l'année ou les jours de la semaine. Par exemple, disons que vous avez cette traduction :

```yaml
en:
   GRAV:
      MONTHS_OF_THE_YEAR: [January, February, March, April, May, June, July, August, September, October, November, December]
```

Vous pouvez obtenir la traduction appropriée pour le mois d'un article avec ce qui suit :

    {{ 'GRAV.MONTHS_OF_THE_YEAR'|ta(post.date|date('n') - 1e }}

Vous pouvez également l'utiliser comme fonction Twig avec `ta()`.

<h3 id="Traductions avec variables">Traductions avec variables
<a href="#Traductions avec variables" class="toc-anchor after"></a></h3>

Vous pouvez également utiliser des variables dans vos traductions Twig en utilisant la syntaxe [sprintf de PHP](https://php.net/sprintf) :

    SIMPLE_TEXT: There are %d monkeys in the %s

Et puis vous pouvez remplir ces variables avec le Twig :

    {{ "SIMPLE_TEXT"|t(12, "London Zoo")|e }}

aboutissant à la traduction :

    There are 12 monkeys in the London Zoo

<h3 id="Traductions complexes">Traductions complexes
<a href="#Traductions complexes" class="toc-anchor after"></a></h3>

Parfois, il est nécessaire d'effectuer des traductions complexes avec remplacement dans des langues spécifiques. Vous pouvez utiliser toute la puissance de la méthode `translate(`) des objets langage avec le filtre/fonction `tl`. Par exemple:

    {{ ["SIMPLE_TEXT", 12, 'London Zoo']|tl(['fr'])|e }}

Traduira la chaîne `SIMPLE_TEXT` et remplacera les espaces réservés par `12` et `London Zoo` respectivement. Il existe également un tableau transmis avec des traductions de langue à essayer dans l'ordre premier trouvé, premier utilisé. Cela affichera le résultat en français :

    Il y a 12 singes dans le Zoo de Londres

<h3 id="Traductions PHP">Traductions PHP
<a href="#Traductions PHP" class="toc-anchor after"></a></h3>

En plus du filtre et des fonctions Twig, vous pouvez utiliser la même approche dans votre plugin Grav :

    $translation = $this->grav['language']->translate(['HEADER.MAIN_TEXT']);

Vous pouvez également spécifier une langue :

    $translation = $this->grav['language']->translate(['HEADER.MAIN_TEXT'], 'fr');

Pour traduire un élément spécifique dans un tableau, utilisez :

    $translation = $this->grav['language']->translateArray('GRAV.MONTHS_OF_THE_YEAR', 3);

<h3 id="Traductions de plugins et de thèmes">Traductions de plugins et de thèmes
<a href="#Traductions de plugins et de thèmes" class="toc-anchor after"></a></h3>

Vous pouvez également fournir vos propres traductions dans les plugins et les thèmes. Cela se fait en créant un fichier `languages.yaml` à la racine de votre plugin ou thème (par exemple `/user/plugins/error/languages.yaml`, ou `user/themes/antimatter/languages.yaml`), et doit contenir toutes les langues prises en charge préfixé par le code de langue ou de paramètres régionaux :

```yaml
en:
   PLUGIN_ERROR:
      TITLE: Error Plugin
      DESCRIPTION: The error plugin provides a simple mechanism for handling error pages within Grav.
fr:
   PLUGIN_ERROR:
      TITLE: Plugin d'Erreur
      DESCRIPTION: Le plugin d'erreur fournit un mécanisme simple de manipulation des pages d'erreur au sein de Grav.
```

<div class="notice note">
La convention pour les plugins est d'utiliser PLUGIN_PLUGINNAME. <em>comme préfixe pour toutes les chaînes de langue, pour éviter tout conflit de nom. Les thèmes sont moins susceptibles d'introduire des conflits de chaînes de langue, mais c'est une bonne idée de préfixer les chaînes ajoutées dans les thèmes avec THEME_THEMENAME</em>.
</div>

<h3 id="Remplacement de traduction">Remplacement de traduction
<a href="#Remplacement de traduction" class="toc-anchor after"></a></h3>

Si vous souhaitez remplacer une traduction particulière, placez simplement la paire clé/valeur modifiée dans un fichier de langue approprié dans votre dossier `user/languages/`. Par exemple, un fichier appelé `user/languages/en.yaml` pourrait contenir :

```yaml
PLUGIN_ERROR:
   TITLE: My Error Plugin
```

Cela garantira que vous pouvez toujours remplacer une chaîne de traduction sans déranger les plugins ou les thèmes eux-mêmes, et évitera également d'écraser une traduction personnalisée lors de leur mise à jour.

<h2 id="Avancé">Avancé
<a href="#Avancé" class="toc-anchor after"></a></h2>

<h3 id="Gestion de la langue basée sur l'environnement">Gestion de la langue basée sur l'environnement
<a href="#Gestion de la langue basée sur l'environnement" class="toc-anchor after"></a></h3>

Vous pouvez tirer parti de la [configuration de l'environnement de Grav](./avance-config-environnement.md) pour acheminer automatiquement les utilisateurs vers la version correcte de votre site en fonction de l'URL. Par exemple, si vous aviez une URL telle que `http://french.monsite.comè qui était un alias pour votre `http://www.monsite.com` standard, vous pourriez configurer une configuration d'environnement :

`/user/french.monsite.com/config/system.yaml`

```yaml
languages:
   supported:
      - fr
      - en
```

Ceci utilise un **ordre de langue inversé** donc la langue par défaut est maintenant `fr` donc la langue française s'affichera par défaut.

<h3 id="Routes d'alias de langue">Routes d'alias de langue
<a href="#Routes d'alias de langue" class="toc-anchor after"></a></h3>

Étant donné que chaque page peut avoir son propre itinéraire personnalisé, il serait difficile de basculer entre différentes versions linguistiques de la même page. Cependant, il existe une nouvelle méthode **Page.rawRoute()** sur l'objet Page qui obtiendra le même itinéraire brut pour l'une des différentes traductions linguistiques d'une même page. Tout ce que vous auriez à faire est de mettre le code lang devant pour obtenir le bon chemin vers une version linguistique spécifique d'une page.

Par exemple, supposons que vous êtes sur une page en anglais avec une route personnalisée :

    /my-custom-english-page

La page française a la route personnalisée de :

    /ma-page-francaise-personnalisee

Vous pourriez obtenir la page brute de la page anglaise et cela pourrait être :

    /blog/custom/my-page

Ensuite, ajoutez simplement la langue que vous voulez et c'est votre nouvelle URL ;

    /fr/blog/custom/ma-page 

Cela récupérera la même page que `/ma-page-francaise-personnalisee`.

<h2 id="Assistance à la traduction">Assistance à la traduction
<a href="#Assistance à la traduction" class="toc-anchor after"></a></h2>

Grav fournit un mécanisme simple mais puissant pour fournir des traductions dans Twig et également via PHP pour une utilisation dans les thèmes et les plugins. Ceci est activé par défaut et utilisera la langue en si aucune langue n'est définie. Pour activer ou désactiver manuellement les traductions, il existe un paramètre dans votre `system.yaml` :

```yaml
languages:
   translations: true
```

Les traductions utilisent la même liste de langues que celle définie par `languages: supported:`  dans votre `system.yaml`.

Le système de traduction fonctionne de manière similaire à la configuration Grav et il existe plusieurs endroits et façons de fournir des traductions.

Le premier endroit où Grav recherche les fichiers de traduction est dans le dossier `system/languages`. Les fichiers doivent être créés au format : `en.yaml`, `fr.yaml`, etc. Chaque fichier yaml doit contenir un tableau ou des tableaux imbriqués de paires clé/valeur :

```yaml
SITE_NAME: My Blog Site
HEADER:
   MAIN_TEXT: Welcome to my new blog site
   SUB_TEXT: Check back daily for the latest news
```

Pour faciliter l'identification, Grav préfère l'utilisation de chaînes de langue en majuscules, car cela aide à déterminer les chaînes non traduites et les rend également plus claires lorsqu'elles sont utilisées dans les modèles Twig.

Grav a la capacité de parcourir les langues prises en charge pour trouver une traduction si aucune pour la langue active n'est trouvée. Ceci est activé par défaut mais peut être désactivé via l'option `translations_fallback` :

```yaml
languages:
   translations_fallback: true
```

<div class="notice tip">
Aidez Grav à atteindre une communauté d'utilisateurs plus large en fournissant des traductions <strong>dans votre langue</strong>. Nous utilisons la <a href="https://crowdin.com/">plateforme de traduction Crowdin</a> pour faciliter la traduction des plugins <a href="https://crowdin.com/project/grav-core">Grav Core</a> et <a href="https://crowdin.com/project/grav-admin[Grav Admin]">Grav Admin</a>. <a href="https://crowdin.com/join">Enregistrez-vous</a> et commencez à traduire dès aujourd'hui !
</div>

<h3 id="Sélecteur de langue">Sélecteur de langue
<a href="#Sélecteur de langue" class="toc-anchor after"></a></h3>

Vous pouvez télécharger un simple plugin **Language Switching** via le plugin Admin, ou via le GPM avec :

    bin/gpm install langswitcher

La [documentation pour la configuration et la mise en œuvre se trouve sur GitHub](https://github.com/getgrav/grav-plugin-langswitcher).

<h3 id="Configuration avec des domaines spécifiques à une langue">Configuration avec des domaines spécifiques à une langue
<a href="#Configuration avec des domaines spécifiques à une langue" class="toc-anchor after"></a></h3>

Configurez votre site avec la gestion des langues basée sur l'environnement pour attribuer des langues par défaut (la première langue) aux domaines.

Assurez-vous que l'option

    pages.redirect_default_route: true

est défini sur `true` dans votre `system.yaml`.

Ajoutez ce qui suit à votre fichier **.htaccess** et adaptez les slugs de langage et les noms de domaine à vos besoins :

```console
# http://www.cheat-sheets.org/saved-copy/mod_rewrite_cheat_sheet.pdf
# http://www.workingwith.me.uk/articles/scripting/mod_rewrite

# handle top level e.g. http://grav-site.com/de
RewriteRule ^en/?$ "http://grav-site.com" [R=302,L]
RewriteRule ^de/?$ "http://grav-site.de" [R=302,L]

# handle sub pages, exclude admin path
RewriteCond %{REQUEST_URI} !(admin) [NC]
RewriteRule ^en/(.*)$ "http://grav-site.com/$1" [R=302,L]
RewriteCond %{REQUEST_URI} !(admin) [NC]
RewriteRule ^de/(.*)$ "http://grav-site.de/$1" [R=302,L]
```

Si vous savez comment simplifier les règles de réécriture, veuillez modifier cette page sur GitHub en cliquant sur le lien **Edit** en haut de la page.

<div class="notice note">
Assurez-vous d'ajouter ces règles avant les règles par défaut fournies avec Grav CMS.
</div>

<h3 id="Logique de langage dans les modèles Twig">Logique de langage dans les modèles Twig
<a href="#Logique de langage dans les modèles Twig" class="toc-anchor after"></a></h3>

Il est souvent nécessaire d'accéder à l'état et à la logique de la langue à partir des modèles Twig. Par exemple, si vous avez besoin d'accéder à un certain fichier image qui est différent pour une langue particulière et qui porte un nom différent (`myimage.en.jpg` et `myimage.fr.jpg`).

Pour afficher la version correcte de l'image, vous devez connaître la langue active actuelle. Ceci est possible dans Grav en accédant à l'objet `Language` via l'objet `Grav` et en appelant la méthode appropriée. Dans l'exemple ci-dessus, cela pourrait être réalisé avec le code Twig suivant :

    {{ page.media.images['myimage.'~grav.language.getActive~'.jpg'].html()|raw }}

L'appel `getActive` dans Twig appelle effectivement `Language->getActive()` pour renvoyer le code de langue actif actuel. Voici quelques méthodes linguistiques utiles :

- `getLanguages()` - Renvoie un tableau de toutes les langues prises en charge
- `getLanguage()` - Renvoie l'actif actuel, sinon renvoie la langue par défaut
- `getActive()` - Renvoie la langue active actuelle
- `getDefault()` - Renvoie la (première) langue par défaut

Pour une liste complète des méthodes disponibles, vous pouvez regarder dans le fichier <code><grav root>/system/src/Grav/Common/Language/Language.php</code>.



