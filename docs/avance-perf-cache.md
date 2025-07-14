<h1 class="rem">Performances & mise en cache</h1>

L'une des principales caractéristiques qui rendent Grav si convaincant est sa rapidité. Cela a toujours été une considération clé dans la conception inhérente de Grav et est principalement dû à la mise en cache, mais inclut plusieurs autres composants.

<h2 id="Performance">Performance.
<a href="#Performance" class="toc-anchor after"></a></h2>

1. **La mise en cache PHP est essentielle**. Vous devez exécuter un **opcache** PHP et un **userchache** (comme APCu) afin d'obtenir les meilleures performances de Grav.

2. **Les disques SSD** peuvent faire une grande différence. La plupart des choses peuvent être mises en cache dans le cache utilisateur PHP, mais certaines sont stockées sous forme de fichiers, de sorte que les disques SSD peuvent avoir un impact important sur les performances. Évitez d'utiliser des systèmes de fichiers réseau tels que NFS avec Grav.

3. **L'hébergement natif** sera toujours plus rapide qu'une machine virtuelle. Les VM sont un excellent moyen pour les hébergeurs de proposer des environnements flexibles de type « cloud ». Ceux-ci ajoutent une couche de traitement qui affectera toujours les performances. Grav peut toujours être rapide sur une machine virtuelle (beaucoup plus rapide que wordpress, joomla, etc.), mais néanmoins, pour des performances optimales, vous ne pouvez pas battre une option d'hébergement native.

4. **Une mémoire plus rapide** est meilleure. Étant donné que Grav est si rapide et que bon nombre de ses solutions de mise en cache utilisent fortement la mémoire, la vitesse de la mémoire sur votre serveur peut avoir un impact important sur les performances. Grav n'utilise pas de grandes quantités de mémoire par rapport à certaines plates-formes, de sorte que la quantité de mémoire n'est pas aussi importante et n'a pas autant d'impact sur les performances, que le type de mémoire et la vitesse.

5. **Les processeurs multicœurs rapides** sont meilleurs. Des processeurs plus rapides et plus avancés aideront toujours, mais pas autant que les autres points.

6. **L'hébergement partagé** est bon marché et facilement disponible, mais le partage des ressources ralentira toujours un peu les choses. Encore une fois, Grav peut très bien fonctionner sur un serveur partagé (mieux que les autres CMS), mais pour une vitesse ultime, un serveur dédié est la solution.

7. **Analyseur PECL Yaml**. L'installation de l'analyseur PHP PECL Yaml natif peut augmenter la vitesse d'analyse YAML jusqu'à 400 % ! Cela vaut la peine d'être examiné si vous recherchez une vitesse supplémentaire.

<div class="notice info">
Le getgrav.org fonctionne sur un seul serveur dédié avec des processeurs quad core, 16 Go de mémoire et des disques SSD 6G. Nous exécutons également PHP 7.4 avec Zend opcache et le cache utilisateur APCu. Les serveurs Web exécutent quelques autres sites Web, mais pas autant que vous en trouveriez dans un environnement d'hébergement partagé.
</div>

<h2 id="Options de mise en cache">Options de mise en cache.
<a href="#Options de mise en cache" class="toc-anchor after"></a></h2>

La mise en cache est une fonctionnalité intégrale de Grav qui a été intégrée dès le départ. Le mécanisme de mise en cache utilisé par Grav est la principale raison pour laquelle Grav est aussi rapide qu'il l'est. Cela dit, il y a certains facteurs à prendre en compte.

Grav utilise la bibliothèque [Doctrine Cache](https://www.doctrine-project.org/projects/doctrine-cache/en/latest/index.html) établie et respectée. Cela signifie que Grav prend en charge tous les mécanismes de mise en cache pris en charge par Doctrine Cache. Cela signifie que Grav prend en charge :

* **Auto** (par défaut) - Trouve automatiquement la meilleure option.
* **File** - Stocke dans les fichiers de cache dans le dossier `cache/`.
* **APCu** - [https://php.net/manual/en/book.apcu.php](https://php.net/manual/en/book.apcu.php).
* **Memcache** - [https://php.net/manual/en/book.memcache.php](https://php.net/manual/en/book.memcache.php).
* **Redis** - [https://redis.io](https://redis.io/).
* **WinCache** - [https://www.iis.net/downloads/microsoft/wincache-extension](https://www.iis.net/downloads/microsoft/wincache-extension).

Par défaut, Grav est préconfiguré pour utiliser le réglage `auto`. Cela va essayer **APC**, puis **WinCache** et enfin **File**. Vous pouvez, bien sûr, configurer explicitement le cache dans votre fichier `/config/system.yaml`, ce qui pourrait rendre les choses un peu plus rapides.

<h2 id="Types de mise en cache">Types de mise en cache.
<a href="#Types de mise en cache" class="toc-anchor after"></a></h2>

Il existe en fait **5 types** de mise en cache dans Grav. Elles sont:

1. Mise en cache de la configuration YAML dans PHP.
2. Mise en cache Core Grav pour les objets de page.
3. Mise en cache Twig des fichiers de modèle en tant que classes PHP.
4.  Mise en cache des images pour les ressources multimédias.
5.  Mise en cache des ressources CSS et JQuery avec pipeline.

La mise en cache de la configuration YAML n'est pas configurable et compilera et mettra toujours en cache la configuration dans le dossier `/cache`. La mise en cache des images est également toujours activée et stocke ses images traitées dans le dossier `/images`.

<h2 id="Mise en cache Grav Core">Mise en cache Grav Core.
<a href="#Mise en cache Grav Core" class="toc-anchor after"></a></h2>

La mise en cache Core Grav a les options de configuration suivantes, telles qu'elles sont configurées dans votre fichier `user/config/system.yaml` :

```yaml
1 | cache:
2 |   enabled: true            # Set to true to enable caching
3 |   check:
4 |      method: file          # Method to check for updates in pages: file|folder|hash|none
5 |   driver: auto             # One of: auto|file|apc|xcache|memcache|wincache
6 |   prefix: 'g'              # Cache prefix string (prevents cache conflicts)
```

Comme vous pouvez le voir, les options sont documentées dans le fichier de configuration lui-même. Pendant le développement, il est parfois utile de désactiver la mise en cache pour vous assurer que vous disposez toujours des dernières modifications de page.

Par défaut, Grav utilise la méthode de vérification de `file` pour sa mise en cache. Cela signifie que chaque fois que vous demandez une URL Grav, Grav utilise un routage hautement optimisé pour parcourir tous les **fichiers** du dossier `user/pages` afin de déterminer si quelque chose a changé.

La vérification du cache `folder` sera légèrement plus rapide que celle de `file`, mais ne fonctionnera pas de manière fiable dans tous les environnements. Vous devrez vérifier si Grav récupère les modifications apportées aux pages de votre serveur lors de l'utilisation de l'option `folder`.

La vérification du hachage `hash` utilise un algorithme de hachage rapide sur tous les fichiers de chaque dossier de page. Cela peut être plus rapide que la vérification des fichiers dans certaines situations et prend en compte chaque fichier du dossier.

Si la remise en cache automatique des pages modifiées n'est pas critique pour vous (ou si votre site est plutôt volumineux), la définition de cette valeur sur `none` accélérera encore plus un environnement de production. Il vous suffira <span class="ancre">d'effacer manuellement le cache</span> une fois les modifications apportées. Il s'agit d'un paramètre de **Production uniquement**.

<div class="notice warning">
La suppression d'une page n'efface pas le cache car les effacements de cache sont basés sur des horodatages modifiés par dossier.
</div>

<div class="notice tip">
Vous pouvez facilement forcer le cache à se vider en touchant/enregistrant simplement un fichier de configuration.
</div>

<h2 id="Options spécifiques au cache mémoire">Options spécifiques au cache mémoire.
<a href="#Options spécifiques au cache mémoire" class="toc-anchor after"></a></h2>

Certaines options de configuration supplémentaires sont requises si vous vous connectez à un serveur **memcache** via l'option de pilote `memcache`. Ces options doivent aller sous le groupe `cache` : dans votre `user/config/system.yaml` :

```yaml
1 | cache:
2 |   ...
3 |   memcache:
4 |      server: localhost
5 |      port: 11211
```

<h2 id="Options spécifiques de Memcached">Options spécifiques de Memcached.
<a href="#Options spécifiques de Memcached" class="toc-anchor after"></a></h2>

Semblable à memcache, memcached a quelques options de configuration supplémentaires qui sont requises si vous vous connectez à un serveur **memcached** via l'option de pilote `memcached`. Ces options doivent aller sous le groupe `cache` : dans votre `user/config/system.yaml` :

```yaml
cache:
   ...
   memcached:
      server: localhost
      port: 11211
```

<h2 id="Options spécifiques à Redis">Options spécifiques à Redis.
<a href="#Options spécifiques à Redis" class="toc-anchor after"></a></h2>

Certaines options de configuration supplémentaires sont requises si vous vous connectez à un serveur **redis** via l'option de pilote `redis`. Ces options doivent aller sous le groupe `cache` : dans votre `user/config/system.yaml` :

```yaml
1 | cache:
2 |   ...
3 |   redis:
4 |      server: localhost
5 |      port: 6379
```

Alternativement, vous pouvez utiliser une connexion socket :

```yaml
1 | cache:
2 |   ...
3 |   redis:
4 |     socket: '/tmp/redis.sock'
```

Si votre serveur Redis a un mot de passe ou un ensemble secret, vous pouvez également le définir dans cette configuration :

```yaml
1 | cache:
2 |   ...
3 |   redis:
4 |      password: your-secret
```

*vous aurez également besoin du php-redis installé sur votre système*.

<h2 id="Options spécifiques à Twig">Options spécifiques à Twig.
<a href="#Options spécifiques à Twig" class="toc-anchor after"></a></h2>

Le moteur de création de modèles Twig utilise son propre système de cache basé sur des fichiers, et quelques options lui sont associées.

```twig
1 |twig:
2 |   cache: false             # Set to true to enable twig caching
3 |   debug: true              # Enable Twig debug
4 |   auto_reload: true        # Refresh cache on changes
5 |   autoescape: false        # Autoescape Twig vars
```

Pour de légers gains de performances, vous pouvez désactiver l'extension `debug`, ainsi que désactiver `auto_reload` qui exécute une fonction similaire à `cache: check: method: none` car il ne recherchera pas les modifications dans les fichiers `.html.twig` pour déclencher des actualisations du cache.

<h2 id="Mise en cache et événements">Mise en cache et événements.
<a href="#Mise en cache et événements" class="toc-anchor after"></a></h2>

Pour la plupart, [les événements sont toujours déclenchés](/crochets-evenements) même lorsque la mise en cache est activée. Cela est vrai pour tous les événements sauf pour `onPageContentRaw`, `onPageProcessed`, `onPageContentProcessed`, `onTwigPageVariables` et `onFolderProcessed`. Ces événements sont exécutés lorsque toutes les pages et tous les dossiers sont récursifs et ils se déclenchent sur chaque page ou dossier trouvé. Comme leur nom l'indique, ils ne sont exécutés que pendant le **traitement**, et non après la mise en cache de la page.

