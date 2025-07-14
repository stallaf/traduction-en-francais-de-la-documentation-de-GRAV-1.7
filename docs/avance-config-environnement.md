<h1 class="rem">Configuration de l'environnement</h1>

Grav a la capacité d'étendre les [puissantes capacités de configuration](/configuration) pour différents environnements afin de prendre en charge différentes configurations pour les scénarios de **développement**, de **mise en scène** et de **production**.

<div class="notice info">
Jusqu'à Grav 1.6, les environnements étaient stockés dans le dossier <code>user/</code>. Grav 1.7 déplace les environnements vers <code>user/env/</code> pour faciliter la maintenance des environnements. Il est fortement recommandé de déplacer tous les environnements vers ce nouvel emplacement sur vos sites existants.
</div>

<h2 id="Configuration automatique de l'environnement">Configuration automatique de l'environnement.
<a href="#Configuration automatique de l'environnement" class="toc-anchor after"></a></h2>

Cela signifie que vous pouvez fournir aussi peu ou autant de modifications de configuration par environnement que nécessaire. Un bon exemple de ceci est la [barre de débogage](/avance-debogage-journalisation). Par défaut, la nouvelle barre de débogage est désactivée dans le fichier core `system/config/system.yaml`, ainsi que dans le fichier de remplacement utilisateur :

    $ | user/config/system.yaml

Si vous vouliez l'activer, vous pouvez facilement l'activer dans votre fichier `user/config/system.yaml`, mais une meilleure solution pourrait être de l'activer pour votre environnement de développement lors de l'accès via **localhost**, mais désactivé sur votre serveur de **production**.

Cela peut être facilement accompli en fournissant un remplacement de ce paramètre dans le fichier :

    $ | user/env/localhost/config/system.yaml

où `localhost` est le nom d'hôte de l'environnement (c'est ce que l'hôte que vous entrez dans votre navigateur, par exemple http://localhost/votre-site) et votre fichier de configuration contient :

```yaml
1 |debugger:
2 |   enabled: true
```

De même, vous pouvez activer CSS, Link, JS et JS Module Asset Pipelining (combinant + minification) pour votre site de production uniquement (`user/env/www.mysite.com/config/system.yaml`) :

```yaml
1 | assets:
2 |   css_pipeline: true
3 |   js_pipeline: true
4 |   js_module_pipeline: true
```

Si votre serveur de production était accessible via `http://www.mysite.com`, vous pouvez également fournir une configuration spécifique à ce site de production avec un fichier situé à `user/env/www.mysite.com/config/system.yaml`.

Bien sûr, vous n'êtes pas limité aux modifications apportées à `system.yaml`, vous pouvez en fait fournir des remplacements pour **n'importe quel** paramètre Grav dans le `site.yaml` ou même dans n'importe quelle [configuration de plug-in](/plugin-bases) !

<div class="notice info">
Si vous utilisez le Grav <a href="../avance-planificateur">Scheduler</a>, sachez qu'il utilise l'environnement <code>localhost</code> et donc sa configuration.
</div>

<h2 id="Remplacements de plugins">Remplacements de plugins.
<a href="#Remplacements de plugins" class="toc-anchor after"></a></h2>

Remplacer un fichier YAML de configuration de plug-in est simplement le même processus que remplacer un fichier normal. Si le fichier de configuration standard se trouve dans :

    $ | user/config/plugins/email.yaml

Ensuite, vous pouvez remplacer cela par un paramètre qui remplace uniquement les options spécifiques que vous souhaitez utiliser pour les tests locaux :

    $ | user/env/localhost/config/plugins/email.yaml

Avec la configuration :

```yaml
1 | mailer:
2 |   engine: smtp
3 |   smtp:
4 |      server: smtp.mailtrap.io
5 |      port: 2525
6 |      encryption: none
7 |      user: '9a320798e65135'
8 |      password: 'a13e6e27bc7205'
```

<h2 id="Remplacements de thème">Remplacements de thème.
<a href="#Remplacements de thème" class="toc-anchor after"></a></h2>

Vous pouvez remplacer les thèmes de la même manière :

    $ | user/config/themes/antimatter.yaml

Peut être remplacé pour n'importe quel environnement, disons un site de production (`http://www.mysite.com`) :

    $ | user/env/www.mysite.com/config/themes/antimatter.yaml

<h2 id="Configuration de l'environnement basé sur le serveur">Configuration de l'environnement basé sur le serveur.
<a href="#Configuration de l'environnement basé sur le serveur" class="toc-anchor after"></a></h2>

À partir de Grav 1.7, il est possible de définir l'environnement en utilisant la configuration du serveur. Dans ce scénario d'utilisation, vous définissez des variables d'environnement à partir du serveur ou d'un script qui s'exécute avant Grav pour sélectionner l'environnement à utiliser.

Le moyen le plus simple de définir l'environnement consiste à utiliser `GRAV_ENVIRONMENT`. La valeur de `GRAV_ENVIRONMENT` doit être un nom de serveur valide avec ou sans domaine.

L'exemple suivant sélectionne l'environnement de **développement** pour l'hôte local :

**- Apache 2**

```php
1 | <VirtualHost 127.0.0.1:80>
2 |   ...
3 |
4 |   SetEnv GRAV_ENVIRONMENT development
5 | </VirtualHost>
```

**- NGINX php-fpm**

```php
location / {
   ...

   fastcgi_param GRAV_ENVIRONMENT development;
}
```

**- NGINX php-cgi**

```php
location / {
   ...

   env[GRAV_ENVIRONMENT] = development
}
```

**- Docker**

```yaml
web:
   environment:
      - GRAV_ENVIRONMENT=development
```

**- PHP**

```php
// Set environment in setup.php or make sure it runs before Grav.
define('GRAV_ENVIRONMENT', 'development');
```

<h2 id="Chemins d'environnement personnalisés">Chemins d'environnement personnalisés.
<a href="#Chemins d'environnement personnalisés" class="toc-anchor after"></a></h2>

À partir de Grav 1.7, vous pouvez également modifier l'emplacement des environnements. Deux possibilités s'offrent à vous : soit vous configurez un emplacement commun à tous les environnements, soit vous les définissez un par un.

<h3 id="Emplacement personnalisé pour tous les environnements">Emplacement personnalisé pour tous les environnements.
<a href="#Emplacement personnalisé pour tous les environnements" class="toc-anchor after"></a></h3>

Si, pour une raison quelconque, vous n'êtes pas satisfait de l'emplacement `user/env` par défaut pour vos environnements, vous pouvez le modifier à l'aide de la variable d'environnement `GRAV_ENVIRONMENTS_PATH`.

La valeur de `GRAV_ENVIRONMENTS_PATH` doit être un chemin existant sous `GRAV_ROOT`. N'utilisez pas de barre oblique à la fin.

Dans l'exemple suivant, tous les environnements seront situés dans `user/sites/GRAV_ENVIRONMENT`, où `GRAV_ENVIRONMENT` est soit détecté automatiquement, soit défini manuellement dans la configuration du serveur :

**- Apache 2**

```php
1 | <VirtualHost 127.0.0.1:80>
2 | ...
3 |
4 |    SetEnv GRAV_ENVIRONMENTS_PATH user://sites
5 | </VirtualHost>
```
**- NGINX php-fpm**

```yaml
location / {
   ...

fastcgi_param GRAV_ENVIRONMENTS_PATH user://sites;
}
```

**- NGINX php-cgi**

```yaml
location / {
...

   env[GRAV_ENVIRONMENTS_PATH] = user://sites
}
```

**- Docker**

```yaml
web:
   environment:
      - GRAV_ENVIRONMENTS_PATH=user://sites
```

**- PHP**

```php
// Set environments path in setup.php or make sure that the following code runs before Grav.
define('GRAV_ENVIRONMENTS_PATH', 'user://sites');
```

<h2 id="Emplacement personnalisé pour l'environnement actuel">Emplacement personnalisé pour l'environnement actuel.
<a href="#Emplacement personnalisé pour l'environnement actuel" class="toc-anchor after"></a></h2>

Parfois, il peut être utile d'avoir un emplacement personnalisé pour votre environnement

La valeur de `GRAV_ENVIRONMENT_PATH` doit être un chemin existant sous `GRAV_ROOT`. N'utilisez pas de barre oblique à la fin.

Dans l'exemple suivant, seul l'environnement courant sera situé dans `user/development` :

**- Apache 2**

```php
1 | <VirtualHost 127.0.0.1:80>
2 | ...
3 |
4 |   SetEnv GRAV_ENVIRONMENT_PATH user://development
5 | </VirtualHost>
```

**- NGINX php-fpm**

```yaml
location / {
   ...

fastcgi_param GRAV_ENVIRONMENT_PATH user://development;
}
```

**- NGINX php-cgi**

```yaml
location / {
...

   env[GRAV_ENVIRONMENT_PATH] = user://development
}
```

**- Docker**

```yaml
web:
   environment:
      - GRAV_ENVIRONMENT_PATH=user://development
```

**- PHP**

```php
// Set environment path in setup.php or make sure that the following code runs before Grav.
define('GRAV_ENVIRONMENT_PATH', 'user://development');
```

Notez que `GRAV_ENVIRONMENT_PATH` est distinct de `GRAV_ENVIRONMENT`, vous pouvez donc également définir le nom de l'environnement si vous ne souhaitez pas qu'il soit automatiquement défini pour correspondre au nom de domaine actuel.

<h2 id="Personnalisation supplémentaire">Personnalisation supplémentaire.
<a href="#Personnalisation supplémentaire" class="toc-anchor after"></a></h2>

Les environnements peuvent être personnalisés bien plus loin que décrit dans cette page.

Pour plus d'informations, veuillez passer à la page suivante : [Configuration multisite](/avance-config-multisite).

