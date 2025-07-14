<h1 class="rem">Configuration multisite</h1>

<div class="notice info">Grav a un support multisite préliminaire disponible. Cependant, le plugin Admin doit encore être mis à jour pour prendre pleinement en charge les configurations multisites. Nous continuerons à travailler là-dessus dans les prochaines versions de Grav.
</div>

<h2 id="Qu'est-ce qu'une configuration multisite ?">Qu'est-ce qu'une configuration multisite ?
<a href="#Qu'est-ce qu'une configuration multisite ?" class="toc-anchor after"></a></h2>

<div class="notice defaut">
Une configuration multisite vous permet de créer et de gérer un réseau de plusieurs sites Web, tous exécutés sur une seule installation.
</div>

Grav a un support multisite intégré. Cette fonctionnalité étend la [configuration de l'environnement de base](/avance-config-environnement), ce qui vous permet de définir des environnements personnalisés pour vos sites de production et de développement.

Une configuration multisite complète vous donne le pouvoir de changer la façon dont et d'où Grav charge tous ses fichiers.

<h2 id="Exigences pour une configuration multisite Grav">Exigences pour une configuration multisite Grav.
<a href="#Exigences pour une configuration multisite Grav" class="toc-anchor after"></a></h2>

La chose la plus importante dont vous aurez besoin pour faire fonctionner un réseau multisite Grav est un bon hébergement de site Web. Si vous ne prévoyez pas de créer de nombreux sites et que vous n'attendez pas beaucoup de visiteurs, vous pouvez vous en tirer avec un hébergement mutualisé. Cependant, en raison de la nature des multisites, vous aurez probablement besoin d'un VPS ou d'un serveur dédié à mesure que vos sites se développent.

<h2 id="Configuration et installation">Configuration et installation.
<a href="#Configuration et installation" class="toc-anchor after"></a></h2>

Avant de commencer, vous devez vous assurer que votre serveur Web est capable d'exécuter plusieurs sites Web, c'est-à-dire que vous avez accès à votre répertoire racine Grav.

Ceci est essentiel car servir plusieurs sites Web à partir de la même installation est basé sur un fichier `setup.php` situé dans votre racine Grav.

<h3 id="Démarrage rapide (pour les débutants)">Démarrage rapide (pour les débutants).
<a href="#Démarrage rapide (pour les débutants)" class="toc-anchor after"></a></h3>

Une fois créé, le `setup.php` est appelé chaque fois qu'un utilisateur demande une page. Afin de servir plusieurs sites Web à partir d'une seule installation, ce script (en gros) doit indiquer à Grav où se trouvent les fichiers (pour les configurations, les thèmes, les plugins, les pages, etc.) d'un sous-site spécifique.

Les extraits fournis ci-dessous configurent votre installation Grav de telle manière qu'une demande comme

    https://<subsite>.example.com   -->   user/env/<subsite>.example.com

ou

    https://example.com/<subsite>   -->   user/env/<subsite>

utilisera le répertoire `user/env` comme chemin "utilisateur" de base au lieu du répertoire `user`.

Si vous choisissez des sous-répertoires ou des URL basées sur des chemins pour les sous-sites, la seule chose dont vous avez besoin est de créer un répertoire pour chaque sous-site dans le répertoire `user/env` contenant au moins les dossiers requis `config`, `pages`, `plugins` et  `themes`.

Si vous choisissez des sous-domaines pour structurer votre réseau de sites Web, vous devrez alors configurer des sous-domaines (wildcard) sur votre serveur en plus de la configuration de vos sous-sites dans votre répertoire `user/env`.

Dans tous les cas, choisissez la configuration qui vous convient le mieux.

<h4 id="Extraits">Extraits.
<a href="#Extraits" class="toc-anchor after"></a></h4>

Pour les sous-sites accessibles via des sous-domaines copiez le fichier `setup_subdomain.php`, sinon pour les sous-sites accessibles via des sous-répertoires le fichier `setup_subdirectory.php` dans votre `setup.php`.

<div class="notice tip">
Le fichier <code>setup.php</code> doit être placé dans le dossier racine Grav, le même dossier où vous pouvez trouver <code>index.php</code>, <code>README.md</code> et les autres fichiers Grav.
</div>

<h5 id="setup_subdomain.php :">setup_subdomain.php :
<a href="#setup_subdomain.php :" class="toc-anchor after"></a></h5>

```php
1  | <?php
2  | /**
3  |  * Multisite setup for subsites accessible via sub-domains.
4  |  *
5  |  * DO NOT EDIT UNLESS YOU KNOW WHAT YOU ARE DOING!
6  |  */
7  | 
8  | use Grav\Common\Utils;
9  | 
10 | // Get subsite name from sub-domain
11 | $environment = isset($_SERVER['HTTP_HOST'])
12 |     ? $_SERVER['HTTP_HOST']
13 |     : (isset($_SERVER['SERVER_NAME']) ? $_SERVER['SERVER_NAME'] : 'localhost');
14 | // Remove port from HTTP_HOST generated $environment
15 | $environment = strtolower(Utils::substrToString($environment, ':'));
16 | $folder = "env/{$environment}";
17 | 
18 | if ($environment === 'localhost' || !is_dir(ROOT_DIR . "user/{$folder}")) {
19 |     return [];
20 | }
21 | [
22 | return [
23 |    'environment' => $environment,
24 |     'streams' => [
25 |         'schemes' => [
26 |             'user' => [
27 |                'type' => 'ReadOnlyStream',
28 |                'prefixes' => [
29 |                    '' => ["user/{$folder}"],
30 |                ]
31 |             ]
32 |         ]
33 |     ]
34 | ];
```

<h5 id="setup_subdirectory.php :">setup_subdirectory.php :
<a href="#setup_subdirectory.php :" class="toc-anchor after"></a></h5>

```php
1  | <?php
2  | /**
3  |  * Multisite setup for sub-directories or path based
4  |  * URLs for subsites.
5  |  *
6  |  * DO NOT EDIT UNLESS YOU KNOW WHAT YOU ARE DOING!
7  |  */
8  | 
9  | use Grav\Common\Filesystem\Folder;
10 | 
11 | // Get relative path from Grav root.
12 | $path = isset($_SERVER['PATH_INFO'])
13 |    ? $_SERVER['PATH_INFO']
14 |    : Folder::getRelativePath($_SERVER['REQUEST_URI'], ROOT_DIR);
15 | 
16 | // Extract name of subsite from path
17 | $name = Folder::shift($path);
18 | $folder = "env/{$name}";
19 | $prefix = "/{$name}";
20 | 
21 | if (!$name || !is_dir(ROOT_DIR . "user/{$folder}")) {
22 |      return [];
23 | }
24 | 
25 | // Prefix all pages with the name of the subsite
26 | $container['pages']->base($prefix);
27 | 
28 | return [
29 |     'environment' => $name,
30 |     'streams' => [
31 |         'schemes' => [
32 |             'user' => [
33 |                'type' => 'ReadOnlyStream',
34 |                'prefixes' => [
35 |                    '' => ["user/{$folder}"],
36 |                ]
37 |             ]
38 |         ]
39 |     ]
40 | ];
```

Lorsque vous utilisez des sous-répertoires pour changer de contexte de langue, vous devrez peut-être charger différentes configurations en fonction de la langue. Vous pouvez placer vos configurations spécifiques à la langue dans `config/<lang-context>/site.yaml` en utilisant l'exemple pour `setup_subdir_config_switch.php` ci-dessous. De cette façon `yoursite.com/de-AT/index.html` chargerait `config/de-AT/site.yaml`, `yoursite.com/de-CH/index.html` chargerait 
`config/de-CH/site.yaml` et ainsi de suite.

<h5 id="setup_subdir_config_switch.php :">setup_subdir_config_switch.php :
<a href="#setup_subdir_config_switch.php :" class="toc-anchor after"></a></h5>

```php
1  | <?php
2  | /**
3  |  * Switch config based on the language context subdir
4  |  *
5  |  * DO NOT EDIT UNLESS YOU KNOW WHAT YOU ARE DOING!
6  |  */
7  | 
8  | use Grav\Common\Filesystem\Folder;
9  | 
10 | $languageContexts = [
11 |     'de-AT',
12 |     'de-CH',
13 |     'de-DE',
14 | ];
15 | 
16 | // Get relative path from Grav root.
17 | $path = isset($_SERVER['PATH_INFO'])
18 |     ? $_SERVER['PATH_INFO']
19 |     : Folder::getRelativePath($_SERVER['REQUEST_URI'], ROOT_DIR);
20 | 
21 | // Extract name of subdir from path
22 | $name = Folder::shift($path);
23 | 
24 | if (in_array($name, $languageContexts)) {
25 |     return [
26 |         'streams' => [
27 |             'schemes' => [
28 |                 'config' => [
29 |                     'type' => 'ReadOnlyStream',
30 |                     'prefixes' => [
31 |                         '' => [
32 |                             'environment://config',
33 |                             'user://config/' . $name,
34 |                             'user://config',
35 |                             'system/config',
36 |                         ],
37 |                     ],
38 |                 ],
39 |             ],
40 |         ],
41 |     ];
42 | }
43 | 
44 | return [];
```

<h4 id="GPM (Grav Package Manager) et plusieurs configurations">GPM (Grav Package Manager) et plusieurs configurations.
<a href="#GPM (Grav Package Manager) et plusieurs configurations" class="toc-anchor after"></a></h4>

Si vous avez besoin de gérer les plugins et les thèmes de vos sous-sites avec le [GPM](/cli-commandes-gpm), conservez à la fois `user/themes` + `user/plugins`, afin que le GPM les récupère et les mette à jour à un seul endroit. Ensuite, créez un lien symbolique vers les éléments nécessaires sous `user/env/my.site.com/themes` ou `user/env/my.site.com/plugins`. Ensuite, configurez les configurations yaml individuelles `user/env/my.site.com/config/plugins` pour chaque sous-sites.

<h3 id="Configuration avancée (pour les Experts)">Configuration avancée (pour les Experts).
<a href="#Configuration avancée (pour les Experts)" class="toc-anchor after"></a></h3>

Une fois créé, un `setup.php` a accès à deux variables importantes : (i) `$container`, qui est [l'instance Grav](https://github.com/getgrav/grav/blob/develop/system/src/Grav/Common/Grav.php) pas encore correctement initialisée et (ii) `$self`, qui est une instance de la [classe ConfigServiceProvider](https://github.com/getgrav/grav/blob/develop/system/src/Grav/Common/Service/ConfigServiceProvider.php).

Dans ce script, vous pouvez faire n'importe quoi, mais sachez que `setup.php` est appelé chaque fois qu'un utilisateur demande une page. Cela signifie que les opérations d'initialisation critiques en termes de mémoire ou chronophages entraînent un ralentissement de l'ensemble de votre système et doivent donc être évitées.

Au final, le `setup.php` doit retourner un tableau associatif avec le nom **d'environnement** optionnel environment et une collection de flux **streams** (pour plus d'informations et afin de les paramétrer correctement, voir la section [Streams](/avance-config-multisite) :

```php
1  | return [
2  |   'environment' => '<name>',            // A name for the environment
3  |   'streams' => [
4  |     'schemes' => [
5  |       '<stream_name>' => [              // The name of the stream
6  |         'type' => 'ReadOnlyStream',     // Stream object e.g. 'ReadOnlyStream' or 'Stream'
7  |         'prefixes' => [
8  |           '<prefix>' => [
9  |             '<path1>',
10 |             '<path2>',
11 |             '<etc>'
12 |           ]
13 |         ],
14 |         'paths' => [                    // Paths (optional)
15 |           '<paths1>',
16 |           '<paths2>',
17 |           '<etc>'
18 |         ]
19 |       ]
20 |     ]
21 |   ]
22 | ]
```

<div class="notice warning">
Veuillez noter qu'à ce stade très précoce, vous n'avez accès ni à la configuration ni à l'instance URI et donc tout appel à une classe non initialisée peut entraîner un blocage du système, des erreurs inattendues ou une perte (complète) de données .
</div>

<h3 id="Streams">Streams.
<a href="#Streams" class="toc-anchor after"></a></h3>

Grav utilise des URI comme des flux pour définir tous les chemins de fichiers dans Grav. L'utilisation de flux facilite la personnalisation des chemins de recherche pour n'importe quel fichier.

Par défaut, les flux ont été configurés comme ceci :

* `user://` - dossier utilisateur. par exemple. `user/`
* `page://` - dossier de pages. par exemple. `user://pages/`
* `image://` - dossier d'images. par exemple. `user://images/`, `system://images/`
* `account://` - dossier des comptes. par exemple. `user://accounts/`
* `environment://` - emplacement multisite actuel.
* `asset://` - dossier JS/CSS compilé. par exemple. des `assets/`
* `blueprints://` - dossier blueprints. par exemple. `environment://blueprints/`, `user://blueprints/`, `system://blueprints/`
* `config://` - dossier de configuration. par exemple. `environment://config/`, `user://config/`, `system://config/`
* `plugins://` - dossier des plugins. par exemple. `user://plugins/`
* `themes://` - thème actuel. par exemple. `user://themes/`
* `theme://` - thème actuel. par exemple. `themes://antimatter/`
* `languages://` - dossier langues. par exemple. `environment://languages/`, `user://languages/`, `system://languages/`
* `user-data://` - dossier de données. par exemple. `user/data/`
* `system://` - dossier système. par exemple. `system/`
* `cache://` - dossier cache. par exemple. `cache/`, `images/`
* `log://` - dossier du journal. par exemple. `logs/`
* `backup://` - dossier de sauvegarde. par exemple. `backups/`
* `tmp://` - dossier temporaire. par exemple. `tmp/`

Dans les configurations multi-sites, certains de ces paramètres par défaut peuvent ne pas correspondre à ce que vous souhaitez. Grav fournit un moyen simple de personnaliser les flux à partir de la configuration de l'environnement à l'aide de `config/streams.yaml`. De plus, vous pouvez créer et utiliser vos propres flux si nécessaire.

Le mappage des répertoires physiques sur un périphérique logique peut être effectué en configurant des `prefixes`. Voici un exemple où nous séparons les pages, les images, les comptes, les données, le cache et les journaux du reste des sites, mais faisons en sorte que tout le reste soit recherché à partir des emplacements par défaut :

`user/env/domain.com/config/streams.yaml `:

```yaml
1  | schemes:
2  |   account:
3  |     type: ReadOnlyStream
4  |     prefixes:
5  |       '': ['environment://accounts']
6  |   page:
7  |     type: ReadOnlyStream
8  |     prefixes:
9  |       '': ['environment://user']
10 |   image:
11 |     type: Stream
12 |     prefixes:
13 |       '': ['environment://images', 'system://images/']
14 |   'user-data':
15 |     type: Stream
16 |     prefixes:
17 |       '': ['environment://data']
18 |   cache:
19 |     type: Stream
20 |     prefixes:
21 |       '': ['cache/domain.com']
22 |       images: ['images/domain.com']
23 |   log:
24 |     type: Stream
25 |     prefixes:
26 |       '': ['logs/domain.com']
```

Dans Grav, les flux sont des objets, mappant un ensemble de répertoires physiques du système à un périphérique logique. Ils sont classés via leur attribut de `type`. Pour les flux en lecture seule, il s'agit du type `ReadOnlyStream` et pour les flux en lecture-écriture, il s'agit du type `Stream`.

Par exemple, si vous utilisez le flux `image://mountain.jpg`, Grav recherche `environment://images` (`user/env/domain.com/images`) et `system://images` (`system/images`). Cela signifie que les flux peuvent être utilisés pour définir d'autres flux.

Les préfixes vous permettent de combiner plusieurs chemins physiques en un seul flux logique. Si vous regardez attentivement la définition du flux de `cache`, c'est un peu différent. Dans ce cas, `cache://` se résout en `cache`, mais `cache://images` se résout en `images`.

<h2 id="Configuration multisite basée sur serveur">Configuration multisite basée sur serveur.
<a href="#Configuration multisite basée sur serveur" class="toc-anchor after"></a></h2>

Grav 1.7 ajoute le support pour personnaliser l'environnement initial à partir de la configuration de votre serveur.

Cette fonctionnalité est pratique si vous souhaitez utiliser par exemple des conteneurs Docker et que vous souhaitez les rendre indépendants du domaine que vous utilisez. Ou si vous ne souhaitez pas stocker de secrets dans la configuration, mais les stocker dans la configuration de votre serveur.

Les variables d'environnement suivantes peuvent être utilisées pour personnaliser les chemins par défaut que Grav utilise pour configurer l'environnement. Après l'initialisation, les flux peuvent pointer vers un emplacement différent.

<div class="notice tip">
Remarque : Vous pouvez utiliser des variables d'environnement ou des constantes PHP, mais elles doivent être définies avant l'exécution de Grav.
</div>

| Variable                 | Par défaut         | Description
| ---------                | ---------          | ---------
| **GRAV_SETUP_PATH**      | AUTO DETECT        | Un chemin personnalisé vers le fichier `setup.php`<br> incluant le nom du fichier. Par défaut, Grav regarde le<br> fichier de  `GRAV_ROOT/setup.php` et `GRAV_ROOT/`<br>`GRAV_USER_PATH/setup.php`.
| **GRAV_USER_PATH**       | `user`             | Un chemin relatif pour le flux `user://`.
| **GRAV_CACHE_PATH**      | `cache`            | Un chemin relatif pour le flux `cache://`.
| **GRAV_LOG_PATH**        | `logs`             | Un chemin relatif pour le flux `log://`.
| **GRAV_TMP_PATH**        | `tmp`              | Un chemin relatif pour le flux `tmp://`.
| **GRAV_BACKUP_PATH**     | `backup`           | Un chemin relatif pour le flux `backup://`.

De plus, il existe des variables pour personnaliser les environnements. Une meilleure documentation pour ceux-ci peut être trouvée dans [Configuration de l'environnement basé sur le serveur](/avance-config-environnement).

<div class="notice tip">
Remarque : Ceux-ci fonctionnent également à partir du fichier <code>setup.php</code>. Vous pouvez en faire des constantes en utilisant <code>define()</code> ou des variables d'environnement avec <code>putenv()</code>. Les constantes sont préférées aux variables d'environnement.
</div>

| Variable                    | Par défaut         | Description
| ---------                   | ---------          | ---------
| **GRAV_ENVIRONMENT**        | DOMAIN NAME        | Nom de l'environnement. Peut être utilisé par<br> exemple dans les conteneurs Docker pour<br> définir un environnement personnalisé qui ne<br> repose pas sur le nom de domaine, comme<br> ` production` et `develop`.
| **GRAV_ENVIRONMENTS_PATH**  | `user://env`       | Chemin de recherche pour tous les environnements<br> si vous préférez quelque chose comme<br> `user://sites`. Il peut s'agir d'un flux<br> ou d'un chemin relatif à partir de` GRAV_ROOT`.
| **GRAV_ENVIRONMENT_PATH**   | `user://env/ENVIRONMENT` | Parfois, il peut être utile d'avoir un emplacement<br> personnalisé pour votre environnement.

<h3 id="Remplacements de la configuration basée sur le serveur">Remplacements de la configuration basée sur le serveur.
<a href="#Remplacements de la configuration basée sur le serveur" class="toc-anchor after"></a></h3>

Si vous ne souhaitez pas stocker d'informations d'identification secrètes dans la configuration, vous pouvez également les fournir en utilisant des variables d'environnement de votre serveur.

Comme les variables d'environnement ont des exigences de nommage strictes (elles ne peuvent contenir que A-Z, a-z, 0-9 et _), certaines astuces sont nécessaires pour que les remplacements de configuration fonctionnent.

Voici un exemple de remplacement de configuration simple utilisant le format YAML pour la présentation :

```yaml
GRAV_CONFIG: true                           # If false, the configuration here will be ignored.
    
GRAV_CONFIG_ALIAS__GITHUB: plugins.github   # Create alias GITHUB='plugins.github' to shorten the variable names below
    
GRAV_CONFIG__GITHUB__auth__method: api      # Override config.plugins.github.auth.method = api
GRAV_CONFIG__GITHUB__auth__token: xxxxxxxx  # Override config.plugins.github.auth.token = xxxxxxxx
```

Dans l'exemple ci-dessus, `__` (double trait de soulignement) représente une variable imbriquée, qui dans twig est représentée par `.` (point).

Vous pouvez également utiliser des variables d'environnement dans `setup.php`. Cela vous permet par exemple de stocker des secrets en dehors de la configuration :

`user/setup.php` :

```php
<?php

// Use following environment variables in your server configuration:
//
// DYNAMODB_SESSION_KEY: DynamoDb server key for the PHP session storage
// DYNAMODB_SESSION_SECRET: DynamoDb server secret
// DYNAMODB_SESSION_REGION: DynamoDb server region
// GOOGLE_MAPS_KEY: Google Maps secret key

return [
    'plugins' => [
        // This plugin does not exist
        'dynamodb_session' => [
            'credentials' => [
                'key' => getenv('DYNAMODB_SESSION_KEY') ?: null,
                'secret' => getenv('DYNAMODB_SESSION_SECRET') ?: null
            ],
            'region' => getenv('DYNAMODB_SESSION_REGION') ?: null
        ],
        // This plugin does not exist
        'google_maps' => [
            'key' => getenv('GOOGLE_MAPS_KEY') ?: null
        ]
    ]
];
```

<div class="notice warning">
ATTENTION : <code>setup.php</code> est utilisé pour définir la configuration initiale. Si le plug-in ou votre configuration remplacent ultérieurement ces paramètres, les valeurs initiales sont perdues.
</div>

Après avoir défini les variables dans `setup.php`, vous pouvez ensuite définir celles de votre serveur :

**- Apache 2**

```txt
1  | <VirtualHost 127.0.0.1:80>
2  |     ...
3  |
4  |     SetEnv GRAV_SETUP_PATH         user/setup.php
5  |     SetEnv GRAV_ENVIRONMENT        production
6  |     SetEnv DYNAMODB_SESSION_KEY    JBGARDQ06UNJV00DL0R9
7  |     SetEnv DYNAMODB_SESSION_SECRET CVjwH+QkfnPhKgVvJvrG24s0ABi343cJ7WTPxvb7
8  |     SetEnv DYNAMODB_SESSION_REGION us-east-1
9  |     SetEnv GOOGLE_MAPS_KEY         XWIozB2R2GmYInTqZ6jnKuUrdELounUb4BIxYmp
10 | </VirtualHost>
```

**- GINX php-fpm**

```txt
location / {
    ...

    fastcgi_param GRAV_SETUP_PATH         user/setup.php;
    fastcgi_param GRAV_ENVIRONMENT        production;
    fastcgi_param DYNAMODB_SESSION_KEY    JBGARDQ06UNJV00DL0R9;
    fastcgi_param DYNAMODB_SESSION_SECRET CVjwH+QkfnPhKgVvJvrG24s0ABi343cJ7WTPxvb7;
    fastcgi_param DYNAMODB_SESSION_REGION us-east-1;
    fastcgi_param GOOGLE_MAPS_KEY         XWIozB2R2GmYInTqZ6jnKuUrdELounUb4BIxYmp;
}
```

**- NGINX php-cgi**

```txt
location / {
...

    env[GRAV_SETUP_PATH]          = user/setup.php
    env[GRAV_ENVIRONMENT]         = production
    env[DYNAMODB_SESSION_KEY]     = JBGARDQ06UNJV00DL0R9
    env[DYNAMODB_SESSION_SECRET]  = CVjwH+QkfnPhKgVvJvrG24s0ABi343cJ7WTPxvb7
    env[GDYNAMODB_SESSION_REGION] = us-east-1
    env[GGOOGLE_MAPS_KEY]         = XWIozB2R2GmYInTqZ6jnKuUrdELounUb4BIxYmp
}
```

</div>
</div>

**- Docker**

```yaml
web:
  environment:
    - GRAV_SETUP_PATH=user/setup.php
    - GRAV_ENVIRONMENT=production
    - DYNAMODB_SESSION_KEY=JBGARDQ06UNJV00DL0R9
    - DYNAMODB_SESSION_SECRET=CVjwH+QkfnPhKgVvJvrG24s0ABi343cJ7WTPxvb7
    - DYNAMODB_SESSION_REGION=us-east-1
    - GOOGLE_MAPS_KEY=XWIozB2R2GmYInTqZ6jnKuUrdELounUb4BIxYmp
```

**- PHP**

```php
putenv('GRAV_SETUP_PATH', 'user/setup.php');
putenv('GRAV_ENVIRONMENT', 'production');
putenv('DYNAMODB_SESSION_KEY', 'JBGARDQ06UNJV00DL0R9');
putenv('DYNAMODB_SESSION_SECRET', 'CVjwH+QkfnPhKgVvJvrG24s0ABi343cJ7WTPxvb7');
putenv('DYNAMODB_SESSION_REGION', 'us-east-1');
putenv('GOOGLE_MAPS_KEY', 'XWIozB2R2GmYInTqZ6jnKuUrdELounUb4BIxYmp');    
```

Dans cet exemple, le serveur utilisera également l'environnement `production` stocké dans le dossier `user/env/production`.

