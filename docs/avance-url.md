<h1 class="rem">Modifier l'URL du site</h1>

En définissant `custom_base_url` dans system.yaml (ou URL de base personnalisée dans les paramètres système, dans Admin), nous pouvons avoir Grav dans un dossier mais l'exécuter à la racine du domaine.

<h2 id="Scénario 1, exécuté dans le dossier racine du domaine.">Scénario 1, exécuté dans le dossier racine du domaine.
<a href="#Scénario 1, exécuté dans le dossier racine du domaine." class="toc-anchor after"></a></h2>
Scénario 1, exécuté dans le dossier racine du domaine.

Grav est installé dans `http://localhost:8080/grav-develop` mais vous voulez qu'il réponde sur `http://localhost:8080`.

Dans system.yaml, définissez

`custom_base_url: 'http://localhost:8080'`

et définissez le chemin de la session sur le nouveau chemin du site Grav,

```yaml
1 | session:
2 |   path: /
```

Et dans la racine du domaine, définissez la redirection, par ex. avec .htaccess :

```yaml
1 | RewriteEngine On
2 | RewriteCond %{REQUEST_URI} !^/grav-develop/
3 | RewriteRule ^(.*)$ /grav-develop/$1
```

où `grav-develop` est le sous-dossier où se trouve Grav.

<h2 id="Scénario 2, exécuter dans un sous-dossier différent">Scénario 2, exécuter dans un sous-dossier différent.
<a href="#Scénario 2, exécuter dans un sous-dossier différent" class="toc-anchor after"></a></h2>

Grav est installé dans `http://localhost:8080/grav-develop` mais vous voulez qu'il réponde sur `http://localhost:8080/xxxxx`.

Dans system.yaml, définissez

`custom_base_url : 'http://localhost:8080/xxxxx'`

et définissez le chemin de la session sur le nouveau chemin du site Grav,

```yaml
1 | session:
2 |   path: /xxxxx
```

Et dans le nouveau dossier racine, /xxxxx, définissez la redirection, par ex. avec .htaccess :

```yaml
1 | RewriteEngine On
2 | RewriteCond %{REQUEST_URI} !^/grav-develop/
3 | RewriteRule ^(.*)$ /grav-develop/$1
```
où `grav-develop` est le sous-dossier frère où se trouve Grav.

