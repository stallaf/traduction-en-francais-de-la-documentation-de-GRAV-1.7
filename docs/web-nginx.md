<h1 class="rem">Nginx</h1>

*Nginx* est un logiciel de serveur HTTP axé sur les fonctionnalités de base du serveur Web et du proxy. Il est très courant en raison de son efficacité des ressources et de sa réactivité sous charge. Nginx génère des processus de travail, chacun pouvant gérer des milliers de connexions. Chacune des connexions gérées par le travailleur est placée dans une boucle d'événements où elle existe avec d'autres connexions. Dans la boucle, les événements sont traités de manière asynchrone, ce qui permet de gérer le travail de manière non bloquante. Lorsque la connexion se ferme, elle est supprimée de la boucle. Ce style de traitement des connexions permet à Nginx d'évoluer incroyablement loin avec des ressources limitées.

<h2 id="Conditions">Conditions.
<a href="#Conditions" class="toc-anchor after"></a></h2>

Cette page explique comment exécuter Grav avec *Nginx* en tant que serveur HTTP et *PHP-FPM* (FastCGI Process Manager) pour traiter les scripts PHP. Ces packages doivent donc être installés sur votre serveur :

* `nginx`
* `php-fpm`

<h2 id="Configuration">Configuration.
<a href="#Configuration" class="toc-anchor after"></a></h2>

Si vous êtes nouveau sur Nginx et que vous n'avez pas encore une compréhension de base des directives/contexte de bloc, il est recommandé de lire le [Guide du débutant](https://nginx.org/en/docs/beginners_guide.html) Nginx, en particulier les sections [Structure du fichier de configuration](https://nginx.org/en/docs/beginners_guide.html#conf_structure) et [Servir du contenu statique](https://nginx.org/en/docs/beginners_guide.html#static).

Il est supposé que votre configuration Nginx se trouve dans `/etc/nginx/` et que votre installation Grav est stockée dans `/var/www/grav/`. La structure de la configuration est un bloc `http` qui contient des directives générales pertinentes pour toutes les pages servies par Nginx, ainsi qu'un ou plusieurs blocs de `server` pour chaque page, contenant des directives spécifiques au site. Le fichier de configuration principal du serveur est `nginx.conf` et stocke le bloc `http`, tandis que les configurations spécifiques au site (blocs de `server`) sont stockées dans `sites-available` et liées symboliquement aux `sites-enabled`.

<h3 id="Autorisations de fichier">Autorisations de fichier.
<a href="#Autorisations de fichier" class="toc-anchor after"></a></h3>

Le répertoire `/var/www` et tous les fichiers et dossiers contenus doivent appartenir à `$USER:www-data` (ou quel que soit le nom que vous donnez à l'utilisateur/groupe Nginx). La section [Dépannage/Autorisations](/depannage-autorisations) explique comment configurer les autorisations de fichiers et de répertoires pour Grav, dans ce cas en utilisant un groupe partagé. Fondamentalement, ce que vous voulez, c'est `775` pour les répertoires et `664` pour les fichiers dans le répertoire Grav, donc Grav est autorisé à modifier le contenu et à se mettre à niveau. Vous devez ajouter votre utilisateur au groupe `www-data` afin de pouvoir accéder aux fichiers créés par Grav/Nginx.

<h3 id="Exemple nginx.conf">Exemple nginx.conf.
<a href="#Exemple nginx.conf" class="toc-anchor after"></a></h3>

La configuration suivante est une version améliorée du fichier `/etc/nginx/nginx.conf` par défaut, principalement avec des améliorations de [github.com/h5bp/server-configs-nginx](https://github.com/h5bp/server-configs-nginx). Consultez leur référentiel pour obtenir des explications sur ces paramètres ou la documentation du [module principal](https://nginx.org/en/docs/ngx_core_module.html) Nginx et du [module http](https://nginx.org/en/docs/http/ngx_http_core_module.html) pour rechercher des directives spécifiques.

<div class = "notice info">
Il est recommandé d'utiliser un fichier de définition de types MIME mis à jour (<code>mime.types</code>) à partir de <a href = "https://github.com/h5bp/server-configs-nginx">github.com/h5bp/server-configs-nginx</a>. Cela garantira que les types sont correctement définis pour la compression gzip.
</div>

nginx.conf :

```conf
1  | user www-data;
2  | worker_processes auto;
3  | worker_rlimit_nofile 8192; # should be bigger than worker_connections
4  | pid /run/nginx.pid;
5  | 
6  | events {
7  |     use epoll;
8  |     worker_connections 8000;
9  |     multi_accept on;
10 | }
11 | 
12 | http {
13 |     sendfile on;
14 |     tcp_nopush on;
15 |     tcp_nodelay on;
16 | 
17 |     keepalive_timeout 30; # longer values are better for each ssl client, but take up a  worker connection longer
18 |     types_hash_max_size 2048;
19 |     server_tokens off;
20 |
21 |     # maximum file upload size
22 |    # update 'upload_max_filesize' & 'post_max_size' in /etc/php/fpm/php.ini accordingly
23 |     client_max_body_size 32m;
24 |     # client_body_timeout 60s; # increase for very long file uploads
25 | 
26 |     # set default index file (can be overwritten for each site individually)
27 |     index index.html;
28 | 
29 |     # load MIME types
30 |     include mime.types; # get this file from https://github.com/h5bp/server-configs-nginx
31 |     default_type application/octet-stream; # set default MIME type
32 | 
33 |     # logging
34 |     access_log /var/log/nginx/access.log;
35 |     error_log /var/log/nginx/error.log;
36 |
37 |     # turn on gzip compression
38 |     gzip on;
39 |     gzip_disable "msie6";
40 |     gzip_vary on;
41 |     gzip_proxied any;
42 |     gzip_comp_level 5;
43 |     gzip_buffers 16 8k;
44 |     gzip_http_version 1.1;
45 |     gzip_min_length 256;
46 |     gzip_types
47 |         application/atom+xml
48 |         application/javascript
49 |         application/json
50 |         application/ld+json
51 |         application/manifest+json
52 |         application/rss+xml
53 |         application/vnd.geo+json
34 |         application/vnd.ms-fontobject
55 |         application/x-font-ttf
56 |         application/x-web-app-manifest+json
57 |         application/xhtml+xml
58 |         application/xml
59 |         font/opentype
60 |         image/bmp
61 |         image/svg+xml
32 |         image/x-icon
63 |         text/cache-manifest
64 |         text/css
65 |         text/plain
66 |         text/vcard
67 |         text/vnd.rim.location.xloc
68 |        text/vtt
69 |       text/x-component
70 |         text/x-cross-domain-policy;
71 | 
72 |     # disable content type sniffing for more security
73 |     add_header "X-Content-Type-Options" "nosniff";
74 |
75 |     # force the latest IE version
76 |     add_header "X-UA-Compatible" "IE=Edge";
77 | 
78 |     # enable anti-cross-site scripting filter built into IE 8+
79 |     add_header "X-XSS-Protection" "1; mode=block";
80 | 
81 |     # include virtual host configs
82 |     include sites-enabled/*;
83 | }
```

<h3 id="Configuration du site Grav">Configuration du site Grav.
<a href="#Configuration du site Grav" class="toc-anchor after"></a></h3>

Grav est livré avec un fichier de configuration pour votre site dans le répertoire `webserver-configs` de votre installation Grav. Vous pouvez copier ce fichier dans votre répertoire de configuration nginx :

```console
$ | cp /var/www/grav/webserver-configs/nginx.conf /etc/nginx/sites-available/grav-site
```

Ouvrez ce fichier avec un éditeur et remplacez "example.com" par votre domaine/IP (ou "localhost" si vous voulez simplement l'exécuter localement), remplacez la ligne "root" par "root /var/www/grav/; " puis créez un lien symbolique de votre site-config dans `sites-enabled` :

```console
$ | ln -s /etc/nginx/sites-available/grav-site /etc/nginx/sites-enabled/grav-site
```

Enfin laissez Nginx recharger sa configuration :

```console
$ | nginx -s reload
```

<h3 id="Correction contre la vulnérabilité httpoxy">Correction contre la vulnérabilité httpoxy.
<a href="#Correction contre la vulnérabilité httpoxy" class="toc-anchor after"></a></h3>

<div class="notice defaut">
httpoxy est un ensemble de vulnérabilités qui affectent le code d'application s'exécutant dans des environnements CGI ou de type CGI. Source : <a href="https://httpoxy.org/">https://httpoxy.org</a>.
</div>

Afin de sécuriser votre site contre cette vulnérabilité, vous devez bloquer l'en-tête `Proxy`. Cela peut être fait en ajoutant un paramètre FastCGI à votre config. Ouvrez simplement le fichier `/etc/nginx/fastcgi.conf` et ajoutez cette ligne à la fin :

```config
fastcgi_param  HTTP_PROXY         "";
```

<h3 id="Utilisation de SSL (avec un certificat existant)">Utilisation de SSL (avec un certificat existant).
<a href="#Utilisation de SSL (avec un certificat existant)" class="toc-anchor after"></a></h3>

Si vous souhaitez utiliser un certificat SSL existant pour chiffrer le trafic de votre site Web, cette section fournit les étapes nécessaires pour modifier votre configuration Nginx pour cela.

Tout d'abord, créez un fichier `/etc/nginx/ssl.conf` avec le contenu suivant et ajustez les chemins vers votre certificat et votre fichier de clé. La dernière section concerne l'agrafage OSCP et vous demande de fournir un certificat chain+root. Si vous ne le souhaitez pas, vous pouvez commenter ou supprimer tout ce qui se trouve sous le commentaire `OCSP Stapling`. Si votre site Web est SSL uniquement (y compris les sous-domaines), vous pouvez soumettre votre domaine pour le préchargement dans les navigateurs à l'adresse [https://hstspreload.appspot.com](https://hstspreload.appspot.com/). Si ce n'est pas le cas, vous pouvez supprimer `preload` ; à partir de la ligne qui ajoute l'en-tête "Strict-Transport-Security". Assurez-vous de vérifier si toutes ces options fonctionnent pour votre configuration.

**ssl.conf** :

```config
1  | # set the paths to your cert and key files here
2  | ssl_certificate /etc/ssl/certs/example.com.crt;
3  | ssl_certificate_key /etc/ssl/private/example.com.key;
4  |
5  | ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
6  |
7  | ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-DSS-AES128-GCM-SHA256:kEDH+AESGCM:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-DSS-AES128-SHA256:DHE-RSA-AES256-SHA256:DHE-DSS-AES256-SHA:DHE-RSA-AES256-SHA:ECDHE-RSA-DES-CBC3-SHA:ECDHE-ECDSA-DES-CBC3-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:AES:CAMELLIA:DES-CBC3-SHA:!aNULL:!eNULL:!EXPORT:!DES:!RC4:!MD5:!PSK:!aECDH:!EDH-DSS-DES-CBC3-SHA:!EDH-RSA-DES-CBC3-SHA:!KRB5-DES-CBC3-SHA;
8  | ssl_prefer_server_ciphers on;
9  | 
10 | ssl_session_cache shared:SSL:10m; # a 1mb cache can hold about 4000 sessions, so we can hold 40000 sessions
11 | ssl_session_timeout 24h;
12 | 
13 | # Use a higher keepalive timeout to reduce the need for repeated handshakes
14 | keepalive_timeout 300s; # up from 75 secs default
15 | 
16 | # submit domain for preloading in browsers at: https://hstspreload.appspot.com
17 | add_header Strict-Transport-Security "max-age=31536000; includeSubDomains; preload;";
18 | 
19 | # OCSP stapling
20 | # nginx will poll the CA for signed OCSP responses, and send them to clients so clients don't make their own OCSP calls.
21 | # see https://sslmate.com/blog/post/ocsp_stapling_in_apache_and_nginx on how to create the chain+root
22 | ssl_stapling on;
23 | ssl_stapling_verify on;
24 | ssl_trusted_certificate /etc/ssl/certs/example.com.chain+root.crt;
25 | resolver 198.51.100.1 198.51.100.2 203.0.113.66 203.0.113.67 valid=60s;
26 | resolver_timeout 2s;
```

Modifiez maintenant le contenu de votre configuration spécifique à Grav `/etc/nginx/sites-available/grav-site` pour rediriger les requêtes HTTP non chiffrées vers HTTPS, c'est-à-dire vers un bloc `server` écoutant sur le port 443 et incluant votre `ssl.conf` (remplacez "exemple .com" avec votre domaine/IP). Vous pouvez également modifier cela pour rediriger la version non-www vers la version www de votre domaine.

**grav-site** :

```config
1  | # redirect http to non-www https
2  - server {
3  |     listen [::]:80;
4  |     listen 80;
5  |     server_name example.com www.example.com;
6  | 
7  |     return 302 https://example.com$request_uri;
8  | }
9  | 
10 | # redirect www https to non-www https
11 | server {
12 |     listen [::]:443 ssl;
13 |     listen 443 ssl;
14 |     server_name www.example.com;
15 | 
16 |     # add ssl cert & options
17 |     include ssl.conf;
18 | 
19 |     return 302 https://example.com$request_uri;
20 | }
21 | 
22 | # serve website
23 | server {
24 |     listen [::]:443 ssl;
25 |     listen 443 ssl;
26 |     server_name example.com;
27 | 
28 |     # add ssl cert & options
29 |     include ssl.conf;
30 | 
31 |     root /var/www/example.com;
32 | 
33 |     index index.html index.php;
34 | 
35 |     # ...
36 |     # the rest of this server block (location directives) is identical to the one from the shipped config
37 | }
```

Rechargez enfin votre configuration Nginx :

```console
$ | nginx -s reload
```

<h2 id="En-têtes de cache Nginx pour les ressources">En-têtes de cache Nginx pour les ressources.
<a href="#En-têtes de cache Nginx pour les ressources" class="toc-anchor after"></a></h2>

Il est également recommandé d'activer ceux en production. Ces ajouts au fichier de configuration les géreront. 'expires' définit le délai d'expiration du cache, 30 jours dans ce cas. Veuillez consulter la documentation complète sur les en-têtes http pour nginx ici [http://nginx.org/en/docs/http/ngx_http_headers_module.html](http://nginx.org/en/docs/http/ngx_http_headers_module.html).

```config
1  | location ~* ^/forms-basic-captcha-image.jpg$ {
2  |                 try_files $uri $uri/ /index.php$is_args$args;
3  |         }
4  | 
5  |         location ~* \.(?:ico|css|js|gif|jpe?g|png)$ {
6  |                 expires 30d;
7  |                 add_header Vary Accept-Encoding;
8  |                 log_not_found off;
9  |         }
10 | 
11 |         location ~* ^.+\.(?:css|cur|js|jpe?g|gif|htc|ico|png|html|xml|otf|ttf|eot|woff|woff2|svg)$ {
12 |                 access_log off;
13 |                 expires 30d;
14 |                 add_header Cache-Control public;
15 | 
16 | ## No need to bleed constant updates. Send the all shebang in one
17 | ## fell swoop.
18 |                 tcp_nodelay off;
19 | 
20 | ## Set the OS file cache.
21 |                 open_file_cache max=3000 inactive=120s;
22 |                 open_file_cache_valid 45s;
23 |                 open_file_cache_min_uses 2;
24 |                 open_file_cache_errors off;
25 |        }
```
        
