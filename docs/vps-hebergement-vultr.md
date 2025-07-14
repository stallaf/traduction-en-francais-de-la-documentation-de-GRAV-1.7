<h1 class="rem">Vultr</h1>

[Vultr](https://www.vultr.com/) est une autre entrée sur le marché des **VPS alimentés par SSD** et offre des caractéristiques et des fonctionnalités similaires à celles des autres fournisseurs de VPS, mais pour un prix légèrement inférieur. Vous bénéficiez d'un **stockage 100 % SSD**, de **processeurs Intel ultra rapides** et de **14 emplacements à faible latence**.

![Vultr](https://learn.getgrav.org/user/pages/09.webservers-hosting/02.vps/04.vultr/vultr.png)

Après avoir créé un compte et accédé à **Vultr Control Manager**, cliquez sur l'onglet **Déployer**. Laissez le type de serveur sur les **instances de calcul** et choisissez un emplacement :

![Deploy](https://learn.getgrav.org/user/pages/09.webservers-hosting/02.vps/04.vultr/deploy.png)

Sélectionnez ensuite le **système d'exploitation** dans la liste disponible. Nous avons sélectionné **Ubuntu 18.04 LTS** pour être cohérent avec les autres guides et parce qu'il s'agit de la distribution la plus récente, très populaire, fiable et bien prise en charge.

Pour la **taille du serveur**, vous pouvez choisir une taille qui vous convient, mais Grav fonctionnera très bien sur n'importe lequel d'entre eux. Pour les besoins de ce guide, nous utiliserons l'option 1 CPU, 1 Go de mémoire qui est actuellement au prix de 8 $/mois.

![Configuration](https://learn.getgrav.org/user/pages/09.webservers-hosting/02.vps/04.vultr/os-and-server.png)

Vous pouvez choisir les fonctionnalités facultatives que vous souhaitez, puis fournir une simple **étiquette de serveur** pour identifier l'instance VPS. Nous utiliserons `Grav`. Cliquez ensuite sur **Passer une commande** pour créer et installer le serveur.

![Server](https://learn.getgrav.org/user/pages/09.webservers-hosting/02.vps/04.vultr/manage-server.png)

Après environ une minute, votre serveur devrait être prêt et vous pouvez cliquer sur le lien de **gestion** à côté de votre serveur dans la liste pour obtenir les détails du serveur. Sur cette page, vous verrez un panneau de contrôle pour le serveur qui vous permet de contrôler l'état, ainsi que le nom d'utilisateur et le mot de passe root initiaux. Vous pouvez vous connecter en SSH via la commande fournie dans cet onglet en utilisant le mot de passe que vous avez saisi lors de la création de l'instance de distribution. L'authentification par clé publique est recommandée et Vultur dispose d'une [documentation d'authentification par clé publique SSH](https://www.vultr.com/docs/using-your-ssh-key-to-login-to-non-root-users) qui vous guide à travers les étapes requises.

<h2 id="Mettre à jour et mettre à niveau les packages">Mettre à jour et mettre à niveau les packages.
<a href="#Mettre à jour et mettre à niveau les packages" class="toc-anchor after"></a></h2>

À ce stade, vous souhaiterez peut-être soit configurer une entrée `/etc/hosts` locale pour donner à l'adresse IP fournie un nom convivial tel que `vultr.dev`. De cette façon, vous pouvez plus facilement vous connecter en SSH à votre serveur avec `ssh root@vultr.dev`.

Après avoir réussi à vous connecter en SSH à votre serveur en tant que **root**, la première chose à faire est de mettre à jour et de mettre à niveau tous les packages installés. Cela garantira que vous utilisez le dernier et le meilleur :

    $ | # apt update
    $ | # apt upgrade

Répondez simplement `Y` si vous y êtes invité.

Avant d'aller plus loin, supprimons **Apache2** que nous remplacerons par **Nginx**:

    $ | # apt remove apache2*
    $ | # apt autoremove

<div class = "notice info">
REMARQUE : Vous ne l'avez peut-être pas installé. Mais mieux vaut prévenir que guérir !
</div>

Ensuite, vous voudrez installer certains packages essentiels :

    $ |# apt install vim zip unzip nginx git php-fpm php-cli php-gd php-curl php-mbstring php-xml php-zip php-apcu

Cela installera l'éditeur VIM complet (plutôt que la version mini fournie avec Ubuntu), le serveur Web Nginx, les commandes GIT et **PHP 7.2**.

<h2 id="Configurer PHP7.2 FPM">Configurer PHP7.2 FPM.
<a href="#Configurer PHP7.2 FPM" class="toc-anchor after"></a></h2>

Une fois php-fpm installé, il y a un léger changement de configuration qui doit avoir lieu pour une configuration plus sécurisée.

    $ | # vim /etc/php/7.2/fpm/php.ini

Recherchez `cgi.fix_pathinfo`. Ceci sera commenté par défaut et mis à '1'.

Il s'agit d'un paramètre extrêmement peu sûr car il indique à PHP d'essayer d'exécuter le fichier le plus proche qu'il peut trouver si le fichier PHP demandé est introuvable. Cela permettrait essentiellement aux utilisateurs de créer des requêtes PHP d'une manière qui leur permettrait d'exécuter des scripts qu'ils ne devraient pas être autorisés à exécuter.

Décommentez cette ligne et changez '1' en '0' pour qu'elle ressemble à ceci

    $ | cgi.fix_pathinfo=0

Enregistrez et fermez le fichier, puis redémarrez le service.

    $ | # systemctl restart php7.2-fpm

<h2 id="Configurer le pool de connexions Nginx">Configurer le pool de connexions Nginx.
<a href="#Configurer le pool de connexions Nginx" class="toc-anchor after"></a></h2>

Nginx a déjà été installé, mais vous devez le configurer pour qu'il utilise un pool de connexions PHP spécifique à l'utilisateur. Cela garantira votre sécurité et évitera toute autorisation de fichier potentielle lorsque vous travaillez sur les fichiers en tant que compte d'utilisateur et via le serveur Web.

Accédez au répertoire du pool et créez une nouvelle configuration `grav` :

```console
$ | # cd /etc/php/7.2/fpm/pool.d
$ | # mv www.conf www.conf.bak
$ | # vim grav.conf
```

Dans Vim, vous pouvez coller la configuration de pool suivante :

```configuration
1  | [grav]
2  |
3  | user = grav
4  | group = grav
5  |
6  | listen = /var/run/php/php7.2-fpm.sock
7  |
8  | listen.owner = www-data
9  | listen.group = www-data
10 |
11 | pm = dynamic
12 | pm.max_children = 5
13 | pm.start_servers = 2
14 | pm.min_spare_servers = 1
15 | pm.max_spare_servers = 3
16 |
17 | chdir = /
```

Les éléments clés ici sont que `user` et `group` sont définis sur un utilisateur appelé `grav` et que le socket d'écoute a un nom unique à partir du socket standard. Enregistrez et quittez ce fichier.

Nous devons créer l'utilisateur `grav` dédié maintenant :

    $ | # adduser grav

Fournissez un mot de passe fort et laissez les autres valeurs par défaut. Nous devons ensuite créer un emplacement approprié pour que Nginx serve les fichiers, alors changeons d'utilisateur et créons ces dossiers, et créons quelques fichiers de test :

```console
$ | # su - grav
$ | mkdir -p www/html
$ | $ cd www/html
```

Créez un `index.html` simple avec le contenu de :

    <h1>Ça marche !</h1>

...et un fichier appelé `info.php` avec le contenu de :

    <?php phpinfo();

Nous pouvons maintenant quitter cet utilisateur et revenir à root afin de configurer la configuration du serveur Nginx :

```console
$ | $ exit
$ | # cd /etc/nginx/sites-available/
$ | # vim grav
```

Ensuite, collez simplement cette configuration :

```configuration
1  | server {
2  |     #listen 80;
3  |     index index.html index.php;
4  | 
5  |     ## Begin - Server Info
6  |     root /home/grav/www/html;
7  |     server_name localhost;
8  |     ## End - Server Info
9  | 
10 |     ## Begin - Index
11 |     # for subfolders, simply adjust:
12 |     # `location /subfolder {`
13 |     # and the rewrite to use `/subfolder/index.php`
14 |     location / {
15 |         try_files $uri $uri/ /index.php?$query_string;
16 |     }
17 |     ## End - Index
18 | 
19 |     ## Begin - Security
20 |     # deny all direct access for these folders
21 |     location ~* /(\.git|cache|bin|logs|backup|tests)/.*$ { return 403; }
22 |     # deny running scripts inside core system folders
23 |    location ~* /(system|vendor)/.*\.(txt|xml|md|html|yaml|yml|php|pl|py|cgi|twig|sh|bat)$ { return 403; }
24 |     # deny running scripts inside user folder
25 |     location ~* /user/.*\.(txt|md|yaml|yml|php|pl|py|cgi|twig|sh|bat)$ { return 403; }
26 |     # deny access to specific files in the root folder
27 |     location ~ /(LICENSE\.txt|composer\.lock|composer\.json|nginx\.conf|web\.config|htaccess\.txt|\.htaccess) { return 403; }
28 |     ## End - Security
29 | 
30 |     ## Begin - PHP
31 |     location ~ \.php$ {
32 |         # Choose either a socket or TCP/IP address
33 |         fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
34 |         # fastcgi_pass unix:/var/run/php5-fpm.sock; #legacy
35 |         # fastcgi_pass 127.0.0.1:9000;
36 | 
37 |         fastcgi_split_path_info ^(.+\.php)(/.+)$;
38 |         fastcgi_index index.php;
39 |         include fastcgi_params;
40 |         fastcgi_param SCRIPT_FILENAME $document_root/$fastcgi_script_name;
41 |     }
42 |     ## End - PHP
43 | }
```

Il s'agit du fichier stock `nginx.conf` fourni avec Grav avec 2 modifications. `root` a été adaptée à notre utilisateur/dossier que nous venons de créer et l'option `fastcgi_pass` a été définie sur le socket que nous avons défini dans notre pool `grav`. Il ne nous reste plus qu'à lier ce fichier de manière appropriée pour qu'il soit **activé** :

```console
$ | # cd ../sites-enabled
$ | # ln -s ../sites-available/grav
$ | # rm default
```

Vous pouvez tester la configuration avec la commande `nginx -t`. Il devrait renvoyer ce qui suit.

    $ | nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
    $ | nginx: configuration file /etc/nginx/nginx.conf test is successful

Maintenant, tout ce que nous avons à faire est de redémarrer Nginx et le processus php7-fpm et de tester pour nous assurer que nous avons correctement configuré Nginx et le pool de connexion PHP :

    $ | # systemctl restart nginx 
    $ | # systemctl restart php7.2-fpm

Pointez maintenant votre navigateur sur votre serveur : `http://vultr.dev` et vous devriez voir le texte : **Working !**

Vous pouvez également tester pour vous assurer que PHP est installé et fonctionne correctement en pointant votre navigateur vers : `http://vultr.dev/info.php`. Vous devriez voir une page d'informations PHP standard avec APCu, Opcache, etc.

<h2 id="Installation de Grav">Installation de Grav.
<a href="#Installation de Grav" class="toc-anchor after"></a></h2>

C'est la partie facile ! Nous devons d'abord revenir à l'utilisateur Grav, donc SSH en tant que `grav@vultr.dev` ou `su - grav` à partir de la connexion root. puis suivez ces étapes :

```console
$ | $ cd ~/www
$ | $ wget -O grav.zip https://getgrav.org/download/core/grav/latest
$ | $ unzip grav.zip
$ | $ rm -Rf html
$ | $ mv grav html
```

Maintenant que c'est fait, vous pouvez confirmer que Grav est installé en pointant votre navigateur sur `http://vultr.dev` et vous devriez être accueilli avec la page **Grav est en cours d'exécution !**

Parce que vous avez suivi ces instructions avec diligence, vous pourrez également utiliser les commandes [Grav CLI](/cli-commandes-grav) et [Grav GPM](/cli-commandes-gpm) telles que :

```console
$ | $ cd ~/www/html
$ | $ bin/grav clear
$ | 
$ | Clearing cache
$ | 
$ | Cleared:  cache/twig/*
$ | Cleared:  cache/compiled/*
$ | 
$ | Touched: /home/grav/www/html/user/config/system.yaml
```

et commandes GPM :

    $ | $ bin/gpm index
    
