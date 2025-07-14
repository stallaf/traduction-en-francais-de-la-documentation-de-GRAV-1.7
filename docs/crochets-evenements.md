<h1 class="rem">Crochets d'événement</h1>

Dans le chapitre précédent du [Didacticiel sur les Plugins](plugin-tutoriel.md), vous avez peut-être remarqué que notre logique de plugin était englobée dans deux méthodes. Chacune de ces méthodes `onPluginsInitialized` et `onPageInitialized` correspond à des hooks d'événement qui sont disponibles tout au long du cycle de vie de Grav.

Pour exploiter pleinement la puissance des plugins Grav, vous devez savoir quels crochets d'événement sont disponibles, dans quel ordre ces crochets sont appelés et ce qui est disponible pendant ces appels. Les **crochets d'événement** ont une relation directe avec le cycle de vie Grav global.

<h2 id="Ordre d'événement">Ordre d'événement
<a href="#Ordre d'événement" class="toc-anchor after"></a></h2> 

La plupart des événements dans Grav se déclenchent dans un ordre spécifique et il est important de comprendre cet ordre si vous créez des plugins :

1. [onFatalException](#onFatalException) _(pas de commande, peut survenir à tout moment)_
2. Classe `PluginsLoadedEvent` (1.7)
3. Classe `PluginsLoadedEvent` (1.7)
4. [onPluginsInitialized](#onPluginsInitialized)
5. Classe `FlexRegisterEvent` (1.7)
6. onThemeInitialized
7. onRequestHandlerInit (1.6)
8. onTask (1.6)
    1. onTask.{task}
9. onAction (1.6)
    1. onAction.{action} (1.6)
10. onBackupsInitialized
11. onSchedulerInitialized (1.6)
12. [onAssetsInitialized](#onAssetsInitialized)
13. [onTwigTemplatePaths](#onTwigTemplatePaths)
14. [onTwigLoader](#onTwigLoader)
15. [onTwigInitialized](#onTwigInitialized)
16. [onTwigExtensions](#onTwigExtensions)
17. [onBuildPagesInitialized](#onBuildPagesInitialized) _(une fois lorsque les pages sont retraitées)_
    1. [onPageProcessed](#onPageProcessed) _(chaque page n'est pas encore mise en cache)_
    2. onFormPageHeaderProcessed (1.6) _(chaque page n'est pas encore mise en cache)_
    3. [onFolderProcessed](#onFolderProcessed) _(pour chaque dossier trouvé)_
18. [onPagesInitialized](#onPagesInitialized)
19. [onPageInitialized](#onPageInitialized)
    1. [onPageContentRaw](#onPageContentRaw) _(chaque page n'est pas encore mise en cache)_
    2. [onMarkdownInitialized](#onMarkdownInitialized)
    3. [onPageContentProcessed](#onPageContentProcessed) _(chaque page n'est pas encore mise en cache)_
    4. onPageContent _(appelé la première fois que Page :: content() est appelé même lorsqu'il est mis en cache)_
20. [onPageNotFound](#onPageNotFound)
21. onPageAction (1.6)
    1. onPageAction.{action} (1.6)
22. onPageTask (1.6)
    1. onPageTask.{tâche} (1.6)
23. [onTwigPageVariables](#onTwigPageVariables) (chaque page n'est pas encore mise en cache)
24. onHttpPostFilter (1.5.2)
25. [onTwigSiteVariables](#onTwigSiteVariables)
26. [onCollectionProcessed](#onCollectionProcessed) (lorsque la collecte est demandée)
27. [onOutputGenerated](#onOutputGenerated)
28. [onOutputRendered](#onOutputRendered)
29. [onShutdown](#onShutdown)

Événements divers :

1. [onBlueprintCreated](#onBlueprintCreated)
2. onTwigTemplateVariables
3. onTwigStringVariables
4. [onBeforeDownload](#onBeforeDownload)
5. [onPageFallBackUrl](#onPageFallBackUrl)
6. [onMediaLocate](#onMediaLocate)
7. [onGetPageBlueprints](#onGetPageBlueprints)
8. [onGetPageTemplates](#onGetPageTemplates)
9. onFlexObjectRender (1.6)
10. onFlexCollectionRender (1.6)
11. onBeforeCacheClear
12. onImageMediumSaved (ImageFile)
13. onAfterCacheClear (1.7)
14. onHttpPostFilter (1.7)
15. PermissionsRegisterEventclass (1.7)

<h2 id="Crochets d'événement Core Grav">Crochets d'événement Core Grav
<a href="#Crochets d'événement Core Grav" class="toc-anchor after"></a></h2> 

Plusieurs crochets d'événement Grav principaux sont déclenchés lors du traitement d'une page :

<h3 id="onFatalException">onFatalException
<a href="#onFatalException" class="toc-anchor after"></a></h3> 

Il s'agit d'un événement qui peut être déclenché à tout moment si PHP lève une exception fatale. Ceci est actuellement utilisé par le plugin `problems` pour gérer l'affichage d'une liste des raisons potentielles pour lesquelles Grav lève l'exception fatale.

<h3 id="onPluginsInitialized">onPluginsInitialized
<a href="#onPluginsInitialized" class="toc-anchor after"></a></h3> 

Il s'agit du premier événement de plugin disponible. À ce stade, les objets suivants ont été lancés :

* Uri
* Configuration
* Débogueur
* Cache
* Plugins

<div class="notice warning">
Un plugin ne sera pas chargé du tout si l'option de configuration <code>enabled: false</code> a été définie pour ce plugin particulier.
</div>

<h3 id="onAssetsInitialized">onAssetsInitialized
<a href="#onAssetsInitialized" class="toc-anchor after"></a></h3> 

L'événement indique que le gestionnaire d'actifs a été initialisé et est prêt pour l'ajout et la gestion d'actifs.

<h3 id="onPagesInitialized">onPagesInitialized
<a href="#onPagesInitialized" class="toc-anchor after"></a></h3> 

Cet événement signifie que toutes les pages du dossier `user/pages` de Grav ont été chargées en tant qu'objets et sont disponibles dans l'objet Pages.

<h3 id="onPageNotFound">onPageNotFound
<a href="#onPageNotFound" class="toc-anchor after"></a></h3> 

Il s'agit d'un événement qui peut être déclenché si une page attendue n'est pas trouvée. Ceci est actuellement utilisé par le plugin `error` pour afficher une jolie page d'erreur 404.

<h3 id="onPageInitialized">onPageInitialized
<a href="#onPageInitialized" class="toc-anchor after"></a></h3> 

La page actuelle demandée par une URL a été chargée dans l'objet Page.

<h3 id="onOutputGenerated">onOutputGenerated
<a href="#onOutputGenerated" class="toc-anchor after"></a></h3>  

La sortie a été traitée par le **moteur de modèles Twig** et n'est plus qu'une chaîne de caractères HTML.

<h3 id="onOutputRendered">onOutputRendered
<a href="#onOutputRendered" class="toc-anchor after"></a></h3> 

La sortie a été entièrement traitée et envoyée à l'écran.

<h3 id="onShutdown">onShutdown
<a href="#onShutdown" class="toc-anchor after"></a></h3>  

Un nouvel événement très puissant qui vous permet d'effectuer des actions une fois que Grav a terminé le traitement et que la connexion au client a été fermée. Ceci est particulièrement utile pour effectuer des actions qui ne nécessitent pas d'interaction de l'utilisateur et qui pourraient potentiellement avoir un impact sur les performances. Les utilisations possibles incluent le suivi des utilisateurs et le traitement des travaux.

<h3 id="onBeforeDownload">onBeforeDownload
<a href="#onBeforeDownload" class="toc-anchor after"></a></h3> 

Ce nouvel événement passe dans un objet événement qui contient un `file`. Cet événement peut être utilisé pour effectuer une journalisation ou accorder/refuser l'accès pour télécharger ledit fichier.

<h3 id="onGetPageTemplates">onGetPageTemplates
<a href="#onGetPageTemplates" class="toc-anchor after"></a></h3> 

Cet événement permet aux plugins de fournir leurs propres modèles en plus de ceux rassemblés à partir de la structure de répertoire et du noyau du thème. Ceci est particulièrement utile si vous souhaitez que le plugin fournisse son propre modèle.

Exemple

```php
1 | /**
2 |  * Add page template types.
3 |  */
4 | public function onGetPageTemplates(Event $event)
5 | {
6 |     /** @var Types $types */
7 |     $types = $event->types;
8 |     $types->register('downloads');
9 | }
```

Cela permet à un plugin d'enregistrer un modèle (qu'il pourrait fournir) afin qu'il apparaisse dans la liste déroulante des types de modèles de page (comme lors de l'édition d'une page). Dans l'exemple ci-dessus, un type de `downloads` de modèle est ajouté car il existe un fichier `downloads.html.twig` dans le répertoire `downloads`.

<img class="img" src="https://learn.getgrav.org/user/pages/04.plugins/04.event-hooks/ongetpagetemplates.png" alt="exemple">

<h3 id="onGetPageBlueprints">onGetPageBlueprints
<a href="#onGetPageBlueprints" class="toc-anchor after"></a></h3> 

Cet événement, comme `onGetPageTemplates`, permet au plugin de fournir ses propres ressources en plus des ressources principales et spécifiques au thème. Dans ce cas, ce sont des plans.

Exemple :

```php
1  | $scanBlueprintsAndTemplates = function () use ($grav) {
2  |     // Scan blueprints
3  |     $event = new Event();3  | 
4  |     $event->types = self::$types;
5  |     $grav->fireEvent('onGetPageBlueprints', $event);
6  | 
7  |     self::$types->scanBlueprints('theme://blueprints/');
8  | 
9  |     // Scan templates
10 |     $event = new Event();
11 |     $event->types = self::$types;
12 |     $grav->fireEvent('onGetPageTemplates', $event);
13 | 
14 |     self::$types->scanTemplates('theme://templates/');
15 |};
```

Dans cet exemple, nous utilisons à la fois les crochets `onGetPageTemplates` et `onGetPageBlueprints` pour rendre ces ressources fournies par le plug-in (modèles et plans) disponibles pour Grav pour l'héritage et d'autres utilisations.

<h2 id="Twig Event Hooks">Twig Event Hooks
<a href="#Twig Event Hooks" class="toc-anchor after"></a></h2> 

Twig a son propre ensemble de hooks d'événement.

<h3 id="onTwigTemplatePaths">onTwigTemplatePaths
<a href="#onTwigTemplatePaths" class="toc-anchor after"></a></h3> 

Les emplacements de base des chemins de modèle ont été définis sur l'**objet Twig**. Si vous avez besoin d'ajouter d'autres emplacements où Twig recherchera des chemins de modèle, c'est l'événement à utiliser.

Exemple :

```php
1 | /**
2 |  * Add template directory to twig lookup path.
3 |  */
4 |  public function onTwigTemplatePaths()
5 |  {
6 |      $this->grav['twig']->twig_paths[] = __DIR__ . '/templates';
7 |  }
``` 

<h3 id="onTwigInitialized">onTwigInitialized
<a href="#onTwigInitialized" class="toc-anchor after"></a></h3> 

Le moteur de création de modèles Twig est maintenant initialisé à ce stade.

<h3 id="onTwigExtensions">onTwigExtensions
<a href="#onTwigExtensions" class="toc-anchor after"></a></h3> 

Les extensions principales de Twig ont été chargées, mais si vous avez besoin d'ajouter [votre propre extension Twig](https://twig.symfony.com/doc/3.x/advanced.html#id2), vous pouvez le faire avec ce hook d'événement.

<h3 id="onTwigPageVariables">onTwigPageVariables
<a href="#onTwigPageVariables" class="toc-anchor after"></a></h3> 

Où Twig traite une page directement, c'est-à-dire lorsque vous définissez `process: twig: true` dans les en-têtes YAML d'une page. C'est ici que vous devez ajouter toutes les variables à Twig qui doivent être disponibles pour Twig pendant ce processus.

<h3 id="onTwigSiteVariables">onTwigSiteVariables
<a href="#onTwigSiteVariables" class="toc-anchor after"></a></h3> 

Où Twig traite la hiérarchie complète des modèles de site. C'est ici que vous devez ajouter toutes les variables à Twig qui doivent être disponibles pour Twig pendant ce processus.

<h2 id="Crochets d'événement de collection">Crochets d'événement de collection
<a href="#Crochets d'événement de collection" class="toc-anchor after"></a></h2> 

<h3 id="onCollectionProcessed">onCollectionProcessed
<a href="#onCollectionProcessed" class="toc-anchor after"></a></h3> 

Si vous avez besoin de manipuler une collection après son traitement, c'est le moment de le faire.

<h2 id="Crochets d'événement de page">Crochets d'événement de page
<a href="#Crochets d'événement de page" class="toc-anchor after"></a></h2> 

<h3 id="onBuildPagesInitialized">onBuildPagesInitialized
<a href="#onBuildPagesInitialized" class="toc-anchor after"></a></h3> 

Cet événement est déclenché une fois lorsque les pages vont être retraitées. Cela se produit généralement si le cache a expiré ou doit être actualisé. C'est un événement utile à utiliser pour les plugins qui ont besoin de manipuler le contenu et de mettre en cache les résultats.

<h3 id="onBlueprintCreated">onBlueprintCreated
<a href="#onBlueprintCreated" class="toc-anchor after"></a></h3> 

Ceci est utilisé pour le traitement et la gestion des formulaires.

<h3 id="onPageContentRaw">onPageContentRaw
<a href="#onPageContentRaw" class="toc-anchor after"></a></h3> 

Une fois qu'une page a été trouvée, l'en-tête est traité, mais le contenu n'est **pas** traité. Ceci est déclenché pour **chaque page** du système Grav. Les performances ne sont pas un problème car cet événement ne s'exécutera pas sur une page mise en cache, uniquement lorsque le cache est vidé ou qu'un événement de suppression du cache se produit.

<h3 id="onPageProcessed">onPageProcessed
<a href="#onPageProcessed" class="toc-anchor after"></a></h3> 

Une fois qu'une page est analysée et traitée. Ceci est déclenché pour **chaque** page du système Grav. Les performances ne sont pas un problème car cet événement ne s'exécutera pas sur une page mise en cache, uniquement lorsque le cache est vidé ou qu'un événement de suppression du cache se produit.

<h3 id="onMarkdownInitialized">onMarkdownInitialized
<a href="#onMarkdownInitialized" class="toc-anchor after"></a></h3> 

Appelé lorsque Markdown a été initialisé. Permet de remplacer l'implémentation de traitement Parsedown par défaut. Voir [un exemple d'utilisation sur le PR qui l'a introduit](https://github.com/getgrav/grav/pull/747#issuecomment-206821370).

<h3 id="onPageContentProcessed">onPageContentProcessed
<a href="#onPageContentProcessed" class="toc-anchor after"></a></h3> 

Cet événement est déclenché après que la méthode `content()` de la page a traité le contenu de la page. Ceci est particulièrement utile si vous souhaitez effectuer des actions sur le contenu post-traité tout en vous assurant que les résultats sont mis en cache. Les performances ne sont pas un problème car cet événement ne s'exécutera pas sur une page mise en cache, uniquement lorsque le cache est vidé ou qu'un événement de suppression du cache se produit.

<h3 id="onFolderProcessed">onFolderProcessed
<a href="#onFolderProcessed" class="toc-anchor after"></a></h3> 

Une fois qu'un dossier est analysé et traité. Ceci est déclenché pour **chaque dossier** du système Grav. Les performances ne sont pas un problème car cet événement ne s'exécutera pas sur une page mise en cache, uniquement lorsque le cache est vidé ou qu'un événement de suppression du cache se produit.

<h3 id="onPageFallBackUrl">onPageFallBackUrl
<a href="#v" class="toc-anchor after"></a></h3> 

Si une route n'est pas reconnue comme une page, Grav essaie d'accéder à un élément multimédia de la page. L'événement est déclenché dès que la procédure commence, de sorte que les plugins peuvent s'accrocher et fournir des fonctionnalités supplémentaires.

<h3 id="onMediaLocate">onMediaLocate
<a href="#onMediaLocate" class="toc-anchor after"></a></h3> 

Ajoute la prise en charge des emplacements de médias personnalisés pour les extraits.

<h3 id="onTwigLoader">onTwigLoader
<a href="#onTwigLoader" class="toc-anchor after"></a></h3> 

Ajoute la prise en charge de l'utilisation des espaces de noms en conjonction avec deux nouvelles méthodes dans la classe Twig : `Twig::addPath($path, $namespace)` et `Twig::prependPath($path, $namespace)`.

