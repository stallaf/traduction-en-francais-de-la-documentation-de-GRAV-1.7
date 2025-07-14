<h1 class = "rem">Configuration</h1>

Tous les fichiers de configuration Grav sont écrits en [syntaxe YAML](./avance-syntaxe-yaml.md) avec une extension de fichier `.yaml`. YAML est très intuitif, ce qui le rend très facile à lire et à écrire, cependant, vous pouvez consulter la [page YAML dans le chapitre Avancé](./avance-syntaxe-yaml.md) pour avoir une compréhension complète de la syntaxe disponible.

<div class = "notice tip">
<strong>ASTUCE</strong> : Voir <a href="/securite-config">Sécurité > Configuration</a> pour un guide rapide sur la manière de sécuriser et d'optimiser votre site de production.
</div>

<h2 id="Configuration du système">Configuration du système
<a href="#Configuration du système" class="toc-anchor after"></a></h2>

Grav s'attache à rendre les choses aussi simples que possible pour l'utilisateur, et il en va de même pour la configuration. Grav est livré avec quelques options par défaut, et celles-ci sont contenues dans un fichier qui réside dans le fichier `system/config/system.yaml`.

Cependant, **vous ne devez jamais modifier ce fichier**, à la place, toutes les modifications de configuration que vous devez apporter doivent être stockées dans un fichier appelé `user/config/system.yaml`. Tout paramètre de ce fichier ayant la même structure et le même nom remplacera le paramètre fourni dans le fichier de configuration système par défaut.

<div class = "notice warning">
De manière générale, vous ne devez <strong>JAMAIS</strong> modifier quoi que ce soit dans le dossier <code>system/</code>. Toutes les actions de l'utilisateur (création de contenu, installation de plugins, modification de la configuration, etc.) doivent être effectuées dans le dossier <code>user/</code>. De cette façon, cela permet une mise à niveau plus simple et conserve également vos modifications au même endroit pour la sauvegarde, la synchronisation, etc.
</div>

Voici les variables présentes dans le fichier par défaut `system/config/system.yaml` :

<h3 id="Options de base">Options de base
<a href="#Options de base" class="toc-anchor after"></a></h3>

```yaml
1  | absolute_urls: false
2  | timezone: ''
3  | default_locale:
4  | param_sep: ':'
5  | wrapped_site: false
6  | reverse_proxy_setup: false
7  | force_ssl: false
8  | force_lowercase_urls: true
9  | custom_base_url: ''
10 | username_regex: '^[a-z0-9_-]{3,16}$'
11 | pwd_regex: '(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{8,}'
12 | intl_enabled: true
13 | http_x_forwarded:
14 |    protocol: true
15 |    host: false
16 |    port: true
17 |    ip: true
```

Ces options de configuration n'apparaissent pas dans leurs propres sections enfant. Ce sont des options générales qui affectent le fonctionnement du site, son fuseau horaire et son URL de base.

| **PROPRIÉTÉS**          | **DESCRIPTION**
| :------------           | :-------------
| **absolue_urls** :      | URL absolues ou relatives pour `base_url`.
| **timezone** :          | Les valeurs valides peuvent être trouvées [ici](https://php.net/manual/en/timezones.php)
| **default_locale** :    | Paramètres régionaux par défaut (système par défaut).
| **param_sep** :         | Ceci est utilisé pour les paramètres Grav dans l'URL. Ne changez pas cela à moins que vous ne sachiez ce que vous faites.<br> Grav le définit automatiquement sur `;` pour les utilisateurs exécutant le serveur Web Apache sous Windows.
| **wrapped_site** :      | Pour les thèmes/plugins pour savoir si Grav est enveloppé par une autre plateforme. Peut être `true` ou `false`.
| **reverse_proxy_setup** : | Exécuté dans un scénario de proxy inverse avec des ports de serveur Web différents de ceux du proxy. Peut être `true` ou `false`. 
| **force_ssl** :         | Si activé, Grav force l'accès via HTTPS (REMARQUE : pas une solution idéale). Peut être `true` ou `false`.
| **force_lowercase_urls** :| Si vous souhaitez prendre en charge les URL à casse mixte, définissez-le sur `false`.
| **custom_base_url** :   | Définissez manuellement la base_url ici.
| **username_regex** :    | Uniquement les caractères minuscules, les chiffres, les tirets, les traits de soulignement. 3 - 16 caractères.
| **pwd_regex** :         | Au moins un chiffre, une lettre majuscule et minuscule, et au moins 8 caractères.
| **intl_enabled** :      | Logique spéciale pour l'extension internationale PHP (mod_intl).
| **http_x_forwarded** :  | Options de configuration pour les différents en-têtes HTTP_X_FORWARD (**Grav 1.7.0+**).

<h3 id="Langues">Langues
<a href="#Langues" class="toc-anchor after"></a></h3>

```yaml
1  | languages:
2  |   supported: []
3  |   default_lang:
4  |   include_default_lang: true
5  |   include_default_lang_file_extension: true
6  |   pages_fallback_only: false
7  |   translations: true
8  |   translations_fallback: true
9  |   session_store_active: false
10 |   http_accept_language: false
11 |   override_locale: false
12 |   content_fallback: {}
```

La zone **Langues** du fichier établit les paramètres de langue du site. Cela inclut les langues prises en charge, la désignation de la langue par défaut dans les URL et les traductions. Voici la répartition de la zone **Langues** du fichier de configuration système :

| **PROPRIÉTÉS**            | **DESCRIPTION**
| :------------             | :-------------
| **supported** :           | Liste des langues prises en charge. ex: `[en, fr, de]`.
| **default_lang** :        | La valeur par défaut est la première langue prise en charge. <br>Doit être l'une des langues prises en charge.
| **include_default_lang** :| Inclut le préfixe de langue par défaut dans toutes les URL. <br>Peut être `true` ou `false`.
| **include_default_lang_file_extension** : | Si cette option est activée, l'enregistrement d'une page ajoutera la langue <br>par défaut à l'extension de fichier (par exemple, `.en.md`).<br> Désactivez-le pour conserver la langue par défaut en utilisant l'extension de fichier `.md `. <br>Peut être `true` ou `false` (**Grav 1.7.0+**).
| **pages_fallback_only** :  | Seul recours pour rechercher le contenu de la page dans les langues prises en charge. <br>Peut être `true` ou `false`.
| **traductions** :          | Activez les traductions par défaut. Peut être `true` ou `false`.
| **translations_fallback** : | retour aux traductions prises en charge si la langue active n'existe pas. <br>Peut être `true` ou `false`.
| **session_store_active** : | Stocke la langue active dans la session. Peut être `true` ou `false`.
| **http_accept_language** : | Essayez de définir la langue en fonction de l'en-tête http_accept_language dans le navigateur. <br>Peut être `true` ou `false`.
| **override_locale** :      | Remplacez les paramètres régionaux par défaut ou système par des paramètres spécifiques <br> à la langue. Peut être `true` ou `false`.
| **content_fallback** :     | Par défaut, si le contenu n'est pas traduit, Grav affichera le contenu dans la langue par défaut. <br>Utilisez ce paramètre pour remplacer ce comportement par langue. (**Grav 1.7.0+**).

<h3 id="Home">Home
<a href="#Home" class="toc-anchor after"></a></h3>

```yaml
1 | home:
2 |   alias : '/home'
3 |   hide_in_urls : false
```

La section **Home** est l'endroit où vous définissez le chemin par défaut pour la page d'accueil du site. Vous pouvez également choisir de masquer la route d'accueil dans les URL.

| **PROPRIÉTÉS**            | **DESCRIPTION**
| :------------             | :-------------
| **alias** :               | Chemin par défaut pour home, c'est-à-dire : `/home` ou `/`.
| **hide_in_urls** :        | Masque la route d'accueil dans les URL. Peut être `true` ou `false`.

<h3 id="Pages">Pages
<a href="#Pages" class="toc-anchor after"></a></h3>

```yaml
1  | pages:
2  |   type: regular
3  |   theme: quark
4  |   order:
5  |     by: default
6  |     dir: asc
7  |   list:
8  |     count: 20
9  |   dateformat:
10 |     default:
11 |     short: 'jS M Y'
12 |     long: 'F jS \a\t g:ia'
13 |   publish_dates: true
14 |   process:
15 |     markdown: true
16 |     twig: false
17 |   twig_first: false
18 |   never_cache_twig: false
19 |   events:
20 |     page: true
21 |     twig: true
22 |   markdown:
23 |     extra: false
24 |     auto_line_breaks: false
25 |     auto_url_links: false
26 |     escape_markup: false
27 |     special_chars:
28 |       '>': 'gt'
29 |       '<': 'lt'
30 |     valid_link_attributes:
31 |       - rel
32 |       - target
33 |       - id
34 |       - class
35 |       - classes
36 |   types: [html,htm,xml,txt,json,rss,atom]
37 |   append_url_extension: ''
38 |   expires: 604800
39 |   cache_control:
40 |   last_modified: false
41 |   etag: false
42 |   vary_accept_encoding: false
43 |   redirect_default_route: false
44 |   redirect_default_code: 302
45 |   redirect_trailing_slash: true
46 |   ignore_files: [.DS_Store]
47 |   ignore_folders: [.git, .idea]
48 |   ignore_hidden: true
49 |   hide_empty_folders: false
50 |   url_taxonomy_filters: true
51 |   frontmatter:
52 |     process_twig: false
53 |     ignore_fields: ['form','forms']
```
La section **Pages** du fichier `system/config/system.yaml` est l'endroit où vous définissez un grand nombre des principaux paramètres liés au thème. Par exemple, c'est ici que vous définissez le thème utilisé pour afficher le site, l'ordre des pages, le traitement par défaut de twig et markdown, etc. C'est là que la plupart des décisions qui affectent la façon dont vos pages sont rendues sont prises.

| **PROPRIÉTÉS**        | **DESCRIPTION**
| :------------         | :-------------
| **type** :            | Paramètre expérimental pour activer les **Pages Flex** dans le frontend. Utilisez `flex` <br> pour activer, `regular` sinon. Par défaut `regular` (**Grav 1.7+**).
| **theme** :           | C'est ici que vous définissez le thème par défaut. Par défaut, c'est `quark`.
| **order** :           |
| **... by** :          | Ordonner les pages par `default`, `alpha` ou `date`.
| **... dir** :         | Sens d'ordre par défaut, `asc` ou `desc`.
| **list** :            |
| **... count** :       | Nombre d'éléments par défaut par page.
| **dateformat** :      |
| **... default** :     | Le format de date par défaut que Grav attend dans le champ `date`.
| **... short** :       | Format de date court. Exemple : `'jS M Y'`.
| **... long** :        | Format de date long. Exemple : `'F jS \a\t g:ia'`.
| **publish_dates** :   | Publie/dépublie automatiquement en fonction des dates. Peut être défini sur `true` ou `false`.
| **process** :         |
| **... markdown** :    | Activez ou désactivez le traitement de markdown sur le front-end. <br>Peut être défini sur `true` ou `false`.
| **... twig** :        | Activez ou désactivez le traitement de twig sur le front-end. <br>Peut être défini sur `true` ou `false`.
| **twig_first** :      | Traite Twig avant markdown lors du traitement des deux sur une page. <br>Peut être défini sur `true` ou `false`.
| **never_cache_twig** : | L'activation de cette option vous permettra d'ajouter une logique de traitement <br> qui peut changer dynamiquement à chaque chargement de page, plutôt que de mettre <br> en cache les résultats et de les stocker pour chaque chargement de page. Cela peut être activé/désactivé <br>à l'échelle du site dans le fichier **system.yaml**, ou sur une page spécifique. <br>Peut être défini sur `true` ou `false`.
| **events** :          |
| **... page** :        | Active les événements au niveau de la page. Peut être défini sur `true` ou `false`.
| **... twig** :        | Active les événements de niveau Twig. Peut être défini sur `true` ou `false`.
| **markdown** :        | 
| **... extra** :       | Activez le support de la prise en charge de Markdown Extra (GitHub-flavored <br>Markdown (GFM) par défaut). Peut être défini sur `true` ou `false`.
| **... auto_line_breaks** :| Active les sauts de ligne automatiques. Peut être défini sur `true` ou `false`.
| **... auto_url_links** :  | Active les liens HTML automatiques. Peut être défini sur `true` ou `false`.
| **... escape_markup** :   | Échappez les balises de balisage dans les entités. Peut être défini sur `true` ou `false`.
| **... special_chars** :   | Liste des caractères spéciaux à convertir automatiquement en entités. Chaque caractère <br>occupe une ligne sous cette variable. Exemple : `'>' : 'gt'`.
|**... valid_link_attributes** : | Aattributs valides à transmettre via des liens markdown  **(Grav 1.7+)**.
| **types** :               | Liste des types de page valides. Par exemple : `[txt,xml,html,htm,json,rss,atom]`.
**append_url_extension** :  | Ajoute l'extension de la page dans les URL de page (par exemple, `.html` donne `/chemin/page.html`).
| **expires** :             | La page expire le temps en secondes (604800 secondes = 7 jours) (`no cache`est également possible).
| **cache_control** :       | Peut être vide pour aucun paramètre, ou une valeur de texte de contrôle de cache valide.
| **last_modified** :       | Définit l'en-tête de date de la dernière modification en fonction de l'horodatage <br>de modification du fichier. Peut être défini sur `true` ou `false`.
| **etag** :                | Définit la balise d'en-tête etag. Peut être défini sur `true` ou `false`.
| **varie_accept_encoding** : | Ajouter l'en-tête `Vary : Accept-Encoding`. Peut être défini sur `true` ou `false`.
| **redirect_default_route** : | Redirige automatiquement vers la route par défaut d'une page. Peut être défini sur `true` ou `false`.
| **redirect_default_code** : | Code par défaut à utiliser pour les redirections. Par exemple : `302`.
| **redirect_trailing_slash** : | Gérer automatiquement ou rediriger 302 une URL de fin.
| **ignore_files** :        | Fichiers à ignorer dans Pages. Exemple : `[.DS_Store]`.
| **ignore_folders** :      | Dossiers à ignorer dans Pages. Exemple : `[.git, .idea]`.
| **ignore_hidden** :       | Ignore tous les fichiers et dossiers cachés. Peut être défini sur `true` ou `false`.
| **hide_empty_folders** :  | Si le dossier n'a pas de fichier .md, doit-il être masqué. Peut être défini sur `true` ou `false`.
| **url_taxonomy_filters** :| Activez les filtres de taxonomie automatiques basés sur les URL pour les collections de pages. <br>Peut être défini sur `true` ou `false`.
| **frontmatter** :         |
| **... process_twig** :    | Le frontmatter doit-il être traité pour remplacer les variables Twig ? Peut être défini sur `true` ou `false`.
| **... ignore_fields** :   | Champs pouvant contenir des variables Twig et ne devant pas être traités. Exemple : `['form','forms']`.

<h3 id="Cache">Cache
<a href="#Cache" class="toc-anchor after"></a></h3>

```yaml
1  | cache:
2  |   enabled: true
3  |   check:
4  |     method: file
5  |   driver: auto
6  |   prefix: 'g'
7  |   purge_at: '0 4 * * *'
8  |   clear_at: '0 3 * * *'
9  |   clear_job_type: 'standard'
10 |   clear_images_by_default: false
11 |   cli_compatibility: false
12 |   lifetime: 604800
13 |   gzip: false
14 |   allow_webserver_gzip: false
15 |   redis:
16 |     socket: false
17 |     password:
18 |     database:
```

La section **Cache** est l'endroit où vous pouvez configurer les paramètres de mise en cache du site. Vous pouvez activer, désactiver, choisir la méthode, etc.

| **PROPRIÉTÉS**        | **DESCRIPTION**
| :------------         | :-------------
| **enabled** :             | Défini sur true pour activer la mise en cache. Peut être défini sur  `true` ou `false`.
| **check** :                |
| **... méthod** :         | Méthode pour vérifier les mises à jour dans les pages. Options : `file`, `folder`, `hash` et `none`. plus de details.
| **driver** :              | Sélectionnez un pilote de cache. Les options sont : `auto`, file, `apcu`, `redis`, `memcache` et `wincache`.
| **prefix** :              | Chaîne de préfixe de cache (évite les conflits de cache). Exemple : `g`.
| **purge_at** :            | Planificateur : fréquence de purge de l'ancien cache à l'aide de la syntaxe cron `at`.
| **clear_at** :            | Scheduler : fréquence d'effacement du cache à l'aide de la syntaxe cron `at`.
| **clear_job_type** :      | Tyype à effacer lors du traitement de la tâche d'effacement planifiée. Option : `standard` | `all`.
| **clear_images_by_default** : | Par défaut, grav n'inclut pas les images traitées lorsque le cache est effacé, cela peut être activé en le définissant sur `true`.
| **cli_compatibility** :   | Garantit que seuls des pilotes non volatiles sont utilisés (file, redis, memcache, etc...).
| **lifetime** :            | Durée de vie des données mises en cache en secondes (`0` = infini). `604800` égal 7 jours.
| **gzip** :                | GZip compresse la sortie de la page. Peut être défini sur `true` ou `false`.
| **allow_webserver_gzip** : | Cette option changera l'en-tête en `Content-Encoding : identiy` permettant à gzip d'être défini de manière plus fiable<br> par le serveur Web, bien que cela rompe généralement la capacité `onShutDown()` hors processus.L'événement s'exécutera toujours, mais il ne<br> sera pas hors processus et peut bloquer la page jusqu'à ce que l'événement soit terminé.
| **redis** :               |
| **... socket** :          | Le chemin d'accès au fichier de socket. Redis.
| **... mot de passe** :    | Mot de passe facultatif.
| **... base de données** : | ID de base de données facultatif.

<h3 id="Twig">Twig
<a href="#Twig" class="toc-anchor after"></a></h3>

```yaml
1 | twig:
2 |   cache: true
3 |   debug: true
4 |   auto_reload: true
5 |   autoescape: false
6 |   0ndefined_functions: true
7 |   undefined_filters: true
8 |  umask_fix: false
```

La section **Twig** vous donne un ensemble rapide d'outils avec lesquels configurer Twig sur votre site pour le débogage, la mise en cache et l'optimisation.

| **PROPRIÉTÉS**           | **DESCRIPTION**
| :------------            | :-------------
| **cache** :              | Défini sur `true` pour activer la mise en cache de Twig. Peut être défini sur `true` ou `false`. 
| **debug** :              | Activez le débogage de Twig. Peut être défini sur `true` ou `false`.
| **auto_reload** :        | Actualise le cache lors des modifications. Peut être défini sur `true` ou `false`. 
| **autoescape** :         | Variables Autoescape Twig. Peut être défini sur `true` ou `false`. 
| **undefined_functions*** : | Autorise les fonctions non définies. Peut être défini sur `true` ou `false`. 
| **undefined_filters** :  | Autorise les filtres non définis. Peut être défini sur `true` ou `false` .
| **umask_fix** :          | Par défaut, Twig crée des fichiers en cache en tant que 755, le correctif passe à 775. <br>Peut être défini sur `true` ou `false`. 

<h3 id="Assets">Assets
<a href="#Assets" class="toc-anchor after"></a></h3>

```yaml
1  | assets:
2  |    css_pipeline: false
3  |    css_pipeline_include_externals: true
4  |    css_pipeline_before_excludes: true
5  |    css_minify: true
6  |    css_minify_windows: false
7  |    css_rewrite: true
8  |    js_pipeline: false
9  |    js_pipeline_include_externals: true
10 |    js_pipeline_before_excludes: true
11 |    js_module_pipeline: false
12 |    s_module_pipeline_include_externals: true
13 |    js_module_pipeline_before_excludes: true
14 |    js_minify: true
15 |    enable_asset_timestamp: false
16 |    enable_asset_sri: false
17 |    collections:
18 |       jquery: system://assets/jquery/jquery-2.x.min.js
```

La section **Assets** permet de paramétrer les options liées aux Assets Manager (JS, CSS).

| **PROPRIÉTÉS**           | **DESCRIPTION**
| :------------            | :-------------
| **css_pipeline** :       | Le pipeline CSS est l'unification de plusieurs ressources CSS dans un seul fichier. <br>Peut être défini sur `true` ou `false`.
| **css_pipeline_include_externals** : | Inclut les URL externes dans le pipeline par défaut. Peut être défini <br>sur `true` ou `false`.
| **css_pipeline_before_excludes** : | Rend le pipeline avant tout fichier exclu. Peut être défini sur `true` ou `false`.
| **css_minify** :             | Minifie le CSS pendant le pipelining. Peut être défini sur `true` ou `false`.
| **css_minify_windows** :     | Minify Override pour les platesformes Windows. `false` par défaut en raison <br>de ThreadStackSize. Peut être défini sur `true` ou `false.`
| **css_rewrite** :            | Réécrivez toutes les URL relatives CSS pendant le pipelining. Peut être défini <br>sur `true` ou `false`.
| **js_pipeline** :            | Le pipeline JS est l'unification de plusieurs ressources JS dans un seul fichier. <br>Peut être défini sur`true` ou `false`.
| **js_pipeline_include_externals** : | Inclut les URL externes dans le pipeline par défaut. Peut être défini <br>sur `true` ou `false`.
| **js_pipeline_before_excludes** : | Rend le pipeline avant tout fichier exclu. Peut être défini sur `true` ou `false`.
| **js_module_pipeline** :       | Le pipeline du module JS est l'unification de plusieurs ressources du module JS <br>dans un seul fichier. Peut être défini sur `true` ou `false`.
| **js_module_pipeline_include_externals** : | Inclure les URL externes dans le pipeline par défaut. Peut être défini <br>sur `true` ou `false`.
| **js_module_pipeline_before_excludes** : | Rendre le pipeline avant tout fichier exclu. Peut être défini sur `true` ou `false`.
| **js_minify** :              | Minifie le JS pendant le pipelining. Peut être défini sur `true` ou `false`.
| **enable_asset_timestamp** :  | Active les assets horodatages. Peut être défini sur `true` ou `false`.
| **enable_asset_sri** :       | Active les assets ISR. Peut être défini sur `true` ou `false`.
| **collections** :            | Elles contiennent des collections, désignées comme des sous-éléments. Par exemple : <br>`query : system://assets/jquery/jquery-3.x.min.js`. En savoir plus à ce sujet.

<h3 id="Errors">Errors
<a href="#Errors" class="toc-anchor after"></a></h3>

```yaml
1 | errors:
2 |   display: 0
3 |   log: true
```

La section **Errors** détermine comment Grav gère l'affichage et la journalisation des erreurs.

| **PROPRIÉTÉS**           | **DESCRIPTION**
| :------------            | :-------------
| **display** :            | Détermine comment les erreurs sont affichées. Entrez soit `1` pour la trace complète, `0` <br>pour une erreur simple ou `-1` pour une erreur système.
| **log** :                | Consigne les erreurs dans le dossier `/logs`. Peut être défini sur `true` ou `false`.

<h3 id="Log">Log
<a href="#Log" class="toc-anchor after"></a></h3>

```yaml
1 | log:
2 |   handler: file
3 |   syslog:
4 |     facility: local6
```

La section **Log** vous permet de configurer d'autres capacités de journalisation pour Grav.

| **PROPRIÉTÉS**           | **DESCRIPTION**
| :------------            | :-------------
| **handler** :            | Gestionnaire de journaux. Actuellement pris en charge : f`ile | syslog`.
| **syslog** :             |
| **...facility** :         | Sortie des installations Syslog.

<h3 id="Debugger">Debugger
<a href="#Debugger" class="toc-anchor after"></a></h3>

```yaml
1 | debugger:
2 |   enabled: false
3 |   provider: clockwork
4 |   censored: false
5 |   shutdown:
6 |     close_connection: true
```

La section **Debugger** vous donne la possibilité d'activer le débogueur de Grav. Un outil utile lors du développement.

| **PROPRIÉTÉS**           | **DESCRIPTION**
| :------------            | :-------------
| **enabled** :                | Activez le débogueur Grav et les paramètres suivants. Peut être défini sur `true` ou `false`.
| **provider** :               | Fournisseur de débogage : peut être défini sur `debugbar `ou sur `clockwork` (**Grav 1.7+**).
| **censored** :               | Censure les informations potentiellement sensibles (paramètres POST, cookies, fichiers, <br>configuration et la plupart des données de tableau/objet dans les messages de journal).<br> Peut être défini sur`true` ou `false` (**Grav 1.7+**).
| **shutdown** :                |
| **... close_connection** :   | Ferme la connexion avant d'appeler `onShutdown(`). `false` pour le débogage.

<h3 id="Images">Images
<a href="#Images" class="toc-anchor after"></a></h3>

```yaml
1  | images:
2  |   default_image_quality: 85
3  |   cache_all: false
4  |   cache_perms: '0755'
5  |   debug: false
6  |   auto_fix_orientation: false
7  |   seofriendly: false
8  |   cls:
9  |     auto_sizes: false
10 |     aspect_ratio: false
11 |     retina_scale: 1
12 |   defaults:
13 |     loading: auto
```

La section **Images** vous permet de définir la qualité d'image par défaut pour laquelle les images sont rééchantillonnées, ainsi que de contrôler les fonctionnalités de mise en cache et de débogage des images.

| **PROPRIÉTÉS**           | **DESCRIPTION**
| :------------            | :-------------
| **default_image_quality** :  | Qualité d'image par défaut à utiliser lors du rééchantillonnage des images. Par exemple : `85` = 85 %
| **cache_all** :              | Cache toutes les images par défaut. Peut être défini sur `true` ou `false`
| **cache_perms** :            | **Doit être entre guillemets !** Permissionss par défaut du dossier de cache. Généralement `'0755'` ou `'0775'`
| **debug** :                  | Affiche une superposition sur les images indiquant la profondeur en pixels de l'image <br>lorsque vous <br>travaillez avec retina, par exemple. Peut être défini sur `true` ou `false`
| **auto_fix_orientation** :   | Essayez de corriger automatiquement les images téléchargées avec une rotation non standard
| **seofriendly** :            | noms d'images traités optimisés pour le référencement
| **cls** :                    | Changement de mise en page cumulé. Plus de détails
| **... auto_sizes** :         | Ajoute automatiquement hauteur/largeur à l'image
| **... aspect_ratio** :       | Réserver de l'espace avec aspect ratio style
| **... retina_scale** :       | Mise à l'échelle pour ajuster les tailles automatiques pour une meilleure gestion des résolutions HiDPI
| **defaults** :               | (**Grav 1.7+**)
| **... loading** :            | Laissez le navigateur choisir : `auto`, `lazy` ou `eager` (**Grav 1.7+**).

<h3 id="Média">Média
<a href="#Média" class="toc-anchor after"></a></h3>

```yaml
1 | media:
2 |   enable_media_timestamp: false
3 |   unsupported_inline_types: []
4 |   allowed_fallback_types: []
5 |   auto_metadata_exif: false
```

La section **Média** gère les options de configuration pour les paramètres liés à la gestion des fichiers multimédias. Cela inclut l'affichage de l'horodatage, la taille du téléchargement, etc.

| **PROPRIÉTÉS**           | **DESCRIPTION**
| :------------            | :-------------
| **enable_media_timestamp** :   | Activer les horodatages des médias.
| **unsupported_inline_types** : | Tableau des types de médias pris en charge pour essayer d'afficher en ligne. <br>Ces types de fichiers sont placés entre crochets `[]`.
| **allow_fallback_types** :  | Tableau des types de médias autorisés pour les fichiers trouvés en cas d'accès via l<br>a route de la page. Ces types de fichiers sont placés entre crochets `[]`.
| **auto_metadata_exif** :    | Créez automatiquement des fichiers de métadonnées à partir de données Exif <br>lorsque cela est possible.

<h3 id="Session">Session
<a href="#Session" class="toc-anchor after"></a></h3>

```yaml
1  | session:
2  |   enabled: true
3  |   initialize: true
4  |   timeout: 1800
5  |   name: grav-site
6  |   uniqueness: path
7  |   secure: false
8  |   httponly: true
9  |   samesite: Lax
10 |   split: true
11 |   domain:
12 |   path:
```

Ces options déterminent les propriétés de session pour votre site.

| **PROPRIÉTÉS**           | **DESCRIPTION**
| :------------            | :-------------
| **enabled** :            | Activez la prise en charge de la session. Peut être défini sur `true` ou `false`.
| **initialize** :         | Initialise la session à partir de Grav (si `false`, le plugin doit démarrer la session).
| **timeout** :            | Délai d'attente en secondes. Par exemple : `1800`.
| **name** :               | Préfixe du nom du cookie de session. Utilisez uniquement des caractères alphanumériques, des tirets ou <br>des traits de soulignement. N'utilisez pas de points dans le nom de la session. Par exemple : `grav-site`.
| **iniqueness** :         | Les sessions doivent-elles être basées sur le `path` ou sur `security.salt`.
| **secure** :             | Définissez la session comme sécurisée. Si `true`, indique que la communication pour ce cookie doit se faire <br>via une transmission cryptée. Activez cette option uniquement sur les sites qui s'exécutent exclusivement sur HTTPS. <br>Peut être défini sur `true` ou `false`.
| **httponly** :           | Définit la session HTTP uniquement. Si `true`, indique que les cookies ne doivent être utilisés que sur HTTP et <br>que la modification de JavaScript n'est pas autorisée. Peut être défini sur `true` ou `false`.
| **samesite** :           | Définissez la session SameSite. Les valeurs possibles sont Lax, Strict et None. Vois ici.
| **domain** :             | Le domaine de session à utiliser dans les réponses. À utiliser uniquement si vous réécrivez le domaine du<br> site, par exemple dans un conteneur Docker.
| **path** :               | Chemin de la session à utiliser dans les réponses. À utiliser uniquement si vous réécrivez le sous-dossier<br> du site par exemple dans un conteneur Docker.

<h3 id="GPM">GPM
<a href="#GPM" class="toc-anchor after"></a></h3>

```yaml
1 | gpm:
2 |   releases: stable
3 |   proxy_url:
4 |   method: 'auto'
5 |   verify_peer: true
6 |   official_gpm_only: true
```

Les options de la section **GPM** contrôlent le GPM de Grav (Grav Package Manager). Par exemple, vous pouvez limiter GPM à l'utilisation de sources officielles et sélectionner la méthode utilisée par GPM pour récupérer les packages. Vous pouvez également choisir entre des versions stables et de test, ainsi que configurer une URL proxy.

| **PROPRIÉTÉS**           | **DESCRIPTION**
| :------------            | :-------------
| **releases** :           | Définir sur `stable` ou `testing` pour déterminer si vous souhaitez effectuer la mise à jour vers <br>la dernière version stable ou testing.
| **proxy_url** :          | Configurez une URL de proxy manuelle pour GPM. Par exemple : `127.0.0.1:3128`.
| **method** :             | Soit `'curl'`, `'fopen'` ou `'auto'`. `'auto'` essaiera d'abord fopen et s'il n'est pas disponible cURL.
| **verify_peer** :        | Sur certains systèmes (principalement Windows), GPM ne peut pas se connecter car le certificat SSL<br> ne peut pas être vérifié. La désactivation de ce paramètre peut aider.
| **official_gpm_only** :  | Par défaut, l'installation directe de GPM n'autorisera que les URL via le proxy officiel de GPM <br>pour assurer la sécurité, désactivez-le pour autoriser d'autres sources.

<h3 id="Accounts">Accounts
<a href="#Accounts" class="toc-anchor after"></a></h3>

```yaml
1 | accounts:
2 |   type: regular
3 |   storage: file
```

Les paramètres de compte vous permettent d'essayer les nouveaux utilisateurs Flex expérimentaux. Cela signifie essentiellement que les utilisateurs sont stockés sous forme d'objets Flex permettant plus de puissance et de performances.

| **PROPRIÉTÉS**           | **DESCRIPTION**
| :------------            | :-------------
| **type** :               | Type de compte : `regular` ou `flex`.
| **storage** :            | Type de stockage felx : `file` ou `folder`.

<h3 id="Flex">Flex
<a href="#Flex" class="toc-anchor after"></a></h3>

```yaml
1  | flex:
2  |   cache:
3  |     index:
4  |       enabled: true
5  |       lifetime: 60
6  |     object:
7  |       enabled: true
8  |       lifetime: 600
9  |     render:
10 |       enabled: true
11 |       lifetime: 600
```

Les paramètres de configuration du cache Flex Objects sont nouveaux dans **Grav 1.7**. Il s'agit de paramètres par défaut pour tous les types Flex, mais ils peuvent être remplacés pour chaque  `Flex Directory`.

| **PROPRIÉTÉS**           | **DESCRIPTION**
| :------------            | :-------------
| **cache** :              | **(Grav 1.7+)**
| **... indice** :         | **(Grav 1.7+)**
| **... ... enabled** :    | Défini sur `true` pour activer la mise en cache de l'index Flex. <br>Est utilisé pour mettre en cache les horodatages dans les fichiers **(Grav 1.7+)**
| .**.. ... lifetime** :   | Durée de vie de l'index en cache en secondes (0 = infini) **(Grav** 1.7+)
| **... object** :         | **(Grav 1.7+)**
| **... ... enabled** :    | Définir sur `true` pour actver la mise en cache des objets Flex. <br>Est utilisé pour mettre en cache les données d'objet **(Grav 1.7+)**
| **... ... lifetime** :   | Durée de vie des objets mis en cache en secondes (0 = infini) **(Grav** 1.7+)
| **... render** :         | **(Grav 1.7+)**
| **... ... enabled** :    | Défini sur `true` pour activer la mise en cache du rendu Flex. <br>Est utilisé pour mettre en cache la sortie rendue (**Grav 1.7+)**
| **... ... lifetime** :   | Durée de vie du cache HTML en secondes (0 = infini) (**Grav 1.7+**).

<h3 id="Strict Mode">Strict Mode
<a href="#Strict Mode" class="toc-anchor after"></a></h3>

```yaml
1 | strict_mode:
2 |   yaml_compat: true
3 |   twig_compat: true
4 |   blueprint_compat: false
```

Le mode strict permet une migration plus propre vers les futures versions de Grav en passant aux nouvelles versions des processeurs YAML et Twig. Celles-ci peuvent ne pas être compatibles avec toutes les extensions tierces.

| **PROPRIÉTÉS**           | **DESCRIPTION**
| :------------            | :-------------
| **yaml_compat** :        | Active la rétrocompatibilité YAML.
| **twig_compat** :        | Active le paramètre d'échappement automatique Twig. obsolète
| **blueprint_compat** :  | Permet une prise en charge stricte rétrocompatible pour blueprints.

<div class = "notice info">
Vous n'avez pas besoin de copier l'intégralité du fichier de configuration pour le remplacer, vous pouvez remplacer aussi peu ou autant que vous le souhaitez. Assurez-vous simplement d'avoir exactement la même structure de nommage pour le paramètre particulier que vous souhaitez remplacer.
</div>

<h2 id="Configuration du site">Configuration du site
<a href="#Configuration du site" class="toc-anchor after"></a></h2>

En plus du fichier `system.yaml`, Grav fournit également un fichier de configuration `site.yaml` par défaut qui est utilisé pour définir certaines configurations spécifiques au front-end telles que le nom de l'auteur, l'adresse e-mail de l'auteur, ainsi que certains paramètres de taxonomie clés. Vous pouvez les remplacer de la même manière que vous le feriez pour system.yaml en fournissant votre propre fichier de configuration dans `user/config/site.yaml`. Vous pouvez également utiliser ce fichier pour définir des options de configuration arbitraires que vous souhaiterez peut-être référencer à partir de votre contenu ou de vos modèles.

Le fichier `system/config/site.yaml` par défaut fourni avec Grav ressemble à ceci :

```yaml
1  | title : Grav                    # Nom du site.
2  | default_lang : en               # Langue par défaut pour le site (potentiellement 
   |                                 # utilisée par le thème).
3  |
4  | author:
5  |   name: John Appleseed          # Nom d'auteur par défaut.
6  |   email: 'john@example.com'     # E-mail de l'auteur par défaut.
7  |
8  | taxonomies : [category,tag]     # Liste arbitraire de types de taxonomie.
9  |
10 | metadata :
11 |   description: 'Mon site Grav'  # Description du site
12 |
13 | summary:
14 |   enabled : true                # activer ou désactiver le résumé de la page
15 |   format : short                # long = le délimiteur de résumé sera ignoré ; short = 
   |                                 # utiliser la première occurrence du délimiteur ou de la taille
16 |   size : 300                    # Longueur maximale du résumé (caractères)
17 |   délimiter : ===               # Le délimiteur récapitulatif
18 |
19 | redirects :
20 | # '/redirect-test': '/'         # Le test de redirection va à la page d'accueil
21 | # '/old/(.*)': '/new/$1'        # Redirigerait /old/ma-page vers /new/ma-page
22 |
23 | routes :
24 | # '/quelque chose/autre' : '/blog/sample-3'    # Alias ​​pour /blog/sample-3
25 | # '/new/(.*)': '/blog/$1'                      # Regex any /new/my-page URL
26 | to /blog/my-page Route
27 |
18 | blog:
29 |   route : '/blog'               # Valeur ajoutée personnalisée (accessible 
   | via site.blog.route)
30 |
31 | #menu :                         # Exemple de menu
32 | #     - text : Source
33 | #       icon : github
34 | #       url : https://github.com/getgrav/grav
34 | #     - icon : twitter
35 | #       url : http://twitter.com/getgrav
```

Décomposons les éléments de cet exemple de fichier :

| **PROPRIÉTÉS**               | **DESCRIPTION**
| :------------                | :-------------
| **title** :                  | Le titre est une simple variable de chaîne qui peut être référencée chaque fois que vous souhaitez afficher<br> le nom de ce site.
| **author** :                 |
| **... name** :               | Le nom de l'auteur du site, qui peut être référencé quand vous en avez besoin.
| **... email** :              | Un e-mail par défaut à utiliser sur votre site.
| **taxonomies** :             | Une liste arbitraire de types de haut niveau que vous pouvez utiliser pour organiser votre contenu. <br>Vous pouvez attribuer du contenu à des types de taxonomie spécifiques, par exemple, des catégories ou des balises. <br>N'hésitez pas à modifier ou à ajouter le vôtre.
| **metadata** :               | Définissez les métadonnées par défaut pour toutes vos pages, consultez la section des en-têtes de page <br>de contenu pour plus de détails.
| **summary** :                |
| **... size** :               | Une variable pour remplacer le nombre de caractères par défaut qui peut être utilisé pour définir <br>la taille du résumé lors de l'affichage d'une partie du contenu.
| **routes** :                 | Il s'agit d'une carte de base qui peut fournir des fonctionnalités d'alias d'URL simples dans Grav. <br>Si vous naviguez vers `/something/else`, vous serez en fait redirigé vers `/blog/sample-3`. <br>N'hésitez pas à modifier ou à ajouter les vôtres au besoin. Les **remplacements Regex** `((.*) - $1)` sont désormais <br>pris en charge à la fin des alias de route. Vous devez les mettre en bas de la liste pour des performances optimales.
|**(custom options)**          | Vous pouvez créer n'importe quelle option que vous aimez dans ce fichier et un bon exemple est l'option <br>`blog: route: '/blog'` qui est accessible dans vos modèles Twig avec `site.blog.route`.

<div class = "notice info">
Pour la plupart des gens, l'élément le plus important de ce fichier est la liste de <code>Taxonomy</code>. Les taxonomies de cette liste doivent être définies ici si vous souhaitez les utiliser dans votre contenu.
</div>>

<h3 id="Securité">Securité
<a href="#Securité" class="toc-anchor after"></a></h3>

Pour une sécurité accrue, il existe un fichier `system/config/security.yaml` qui définit des valeurs par défaut sensibles et est utilisé par le plugin Admin lors de **l'enregistrement** du contenu, ainsi que dans la nouvelle section **Rapports** des **Outils.**

La configuration par défaut ressemble à ceci :

```yaml
1  | xss_whitelist: [admin.super]
2  | xss_enabled:
3  |     on_events: true
4  |     invalid_protocols: true
5  |     moz_binding: true
6  |     html_inline_styles: true
7  |     dangerous_tags: true
8  | xss_invalid_protocols:
9  |     - javascript
10 |     - livescript
11 |     - vbscript
12 |     - mocha
13 |     - feed
14 |     - data
15 | xss_dangerous_tags:
16 |     - applet
17 |      - meta
18 |     - xml
19 |     - blink
20 |     - link
21 |     - style
22 |     - script
23 |     - embed
24 |     - object
25 |     - iframe
26 |     - frame
27 |     - frameset
28 |     - ilayer
39 |     - layer
30 |     - bgsound
31 |     - title
32 |     - base
33 | uploads_dangerous_extensions:
34 |     - php
35 |     - html
36 |     - htm
37 |     - js
38 |    - exe
39 | sanitize_svg: true
```

Si vous souhaitez apporter des modifications à ces paramètres, vous devez copier ce fichier dans `user/config/security.yaml` et y apporter des modifications.

<h2 id="Autres paramètres et fichiers de configuration">Autres paramètres et fichiers de configuration
<a href="#Autres paramètres et fichiers de configuration" class="toc-anchor after"></a></h2>

La configuration de l'utilisateur est entièrement facultative. Vous pouvez remplacer autant ou peu de paramètres par défaut que vous le souhaitez. Cela s'applique à la fois au système, au site et à toutes les configurations de plug-in de votre site.

Vous n'êtes pas non plus limité aux fichiers `user/config/system.yaml` ou `user/config/site.yaml` comme décrit ci-dessus. Vous pouvez créer n'importe quel fichier de configuration `.yaml` arbitraire dans le dossier `user/config` que vous souhaitez et il sera automatiquement récupéré par Grav.

Par exemple, si le nouveau fichier de configuration est nommé `user/config/data.yaml` et qu'une variable yaml dans ce fichier est appelée count :

   `compte: 39`

La variable serait accessible dans votre modèle Twig en utilisant la syntaxe suivante :

   `{{ config.data.count }}`

Il serait également accessible via PHP depuis n'importe quel plugin avec le code :

   `$ count_var = Grav::instance()['config']->get('data.count');`

<div class = "notice note">
Vous pouvez également fournir un plan personnalisé pour permettre à votre fichier personnalisé d'être modifiable dans le plug-in d'administration. Consultez la <a href="/cookbook-recettes-admin">recette correspondante dans la section Admin Cookbook</a>.
</div>

<h3 id="Espaces de nom des variables de configuration">Espaces de nom des variables de configuration
<a href="#Espaces de nom des variables de configuration" class="toc-anchor after"></a></h3>

Les chemins d'accès aux fichiers de configuration seront utilisés comme **espace de noms** pour vos options de configuration.

Vous pouvez également placer toutes les options dans un seul fichier et utiliser des structures YAML pour spécifier la hiérarchie de vos options de configuration. Cet espacement de noms est construit à partir d'une combinaison du **path + filename + option name**.

Par exemple : Une option telle que `author: Frank Smith` dans le fichier `plugins/myplugin.yaml` pourrait être accessible via : `plugins.myplugin.author`. Cependant, vous pourriez également avoir un fichier `plugins.yaml` et dans ce fichier avoir un nom d'option appelé `myplugin: author: Frank Smith` et il serait toujours accessible par le même espace de noms `plugins.myplugin.author`.

Certains exemples de fichiers de configuration pourraient être structurés :

| **FICHIER**                    | **DESCRIPTION**
| :------------                  | :-------------
| ** user/config/system.yaml**         | Fichier de configuration système global
| **user/config/site.yaml**            | Un fichier de configuration spécifique au site
| ** user/config/plugins/myplugin.yaml** | Fichier de configuration individuel pour le plugin myplugin
| **user/config/themes/mytheme.yaml**   | Fichier de configuration individuel pour le thème mythème

<div class = "notice info">
Avoir un fichier de configuration avec espace de noms remplacera ou masquera toutes les options ayant le même chemin dans les fichiers de configuration par défaut.
</div>

<h3 id="Configuration de plugins">Configuration de plugins
<a href="#Configuration de plugins" class="toc-anchor after"></a></h3>

La plupart des **plugins** sont livrés avec leur propre fichier de configuration YAML. Nous vous recommandons de copier ce fichier dans le répertoire `user/config/plugins/` plutôt que de modifier les options de configuration directement dans le fichier situé dans le répertoire du plugin. Cela garantira qu'une mise à jour du plug-in n'écrasera pas vos paramètres et conservera toutes vos options configurables en un seul endroit pratique.

Si vous avez un plugin appelé `user/plugins/myplugin` qui a un fichier de configuration appelé `user/plugins/myplugin/myplugin.yaml` alors vous devez copier ce fichier dans `user/config/plugins/myplugin.yaml` et y modifier le fichier.

Le fichier YAML qui existe dans le répertoire principal du plugin agira comme une solution de secours. Tous les paramètres répertoriés ici et non dans la copie du dossier utilisateur seront récupérés et utilisés par Grav.

<h3 id="Configuration des thèmes">Configuration des thèmes
<a href="#Configuration des thèmes" class="toc-anchor after"></a></h3>

Les mêmes règles pour les thèmes s'appliquent que pour les plugins. Donc, si vous avez un thème appelé `user/themes/mytheme` qui a un fichier de configuration appelé `user/themes/mytheme/mytheme.yaml`, vous devez alors copier ce fichier dans `user/config/themes/mytheme.yaml` et y modifier le fichier.

