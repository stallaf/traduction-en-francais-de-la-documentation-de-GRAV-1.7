<h1 class="rem">Linode</h1>

[Linode](https://www.linode.com/?r=300c424631b602daaa0ecef22912c1c26c81e3af) est dans le jeu VPS depuis un certain temps et se concentre sur la fourniture de **serveurs Linux équipés de SSD ultra-rapides** pour les développeurs. Il existe un processus simple et rapide pour mettre en place un serveur et le faire fonctionner : choisir un **plan tarifaire**, choisir une **distribution Linux**, puis choisir un **emplacement de nœud** le mieux adapté à vos besoins.

<div class = "notice tip">
Vous pouvez maintenant <a href = "https://www.linode.com/docs/guides/grav-marketplace-app/" installer Grav directement sur un nouveau serveur Linode</a> privé virtuel en utilisant leur <a href = "https://www.linode.com/marketplace/apps/linode/grav/">application Linode Maketplace</a>.
</div>

![Linode](https://learn.getgrav.org/user/pages/09.webservers-hosting/02.vps/02.linode/linode.png)

Après avoir créé un compte et navigué vers le **Linode Manager**, vous devez d'abord ajouter un Linode. Pour ce test, nous choisirons l'option la plus petite et la moins chère à 10 $/mois pour 1 cœur de processeur et 24 Go d'espace disque SSD. Il existe de nombreuses options de mise à l'échelle jusqu'à 20 cœurs de processeur et 2 Go d'espace disque ! N'oubliez pas également de choisir un emplacement approprié dans la liste déroulante :

![Linode manager](https://learn.getgrav.org/user/pages/09.webservers-hosting/02.vps/02.linode/add-linode.png)

Une fois le Linode créé, vous devrez cliquer sur le lien **Tableau de bord** dans la colonne des options. Cela vous amènera à la page où vous pourrez maintenant choisir votre distribution. Dans le tableau de bord, choisissez **Déployer une image**.

![Déployez image 1](https://learn.getgrav.org/user/pages/09.webservers-hosting/02.vps/02.linode/deploy-image.png)

Pour des raisons de compatibilité et de facilité d'utilisation, j'aime choisir une distribution stable d'Ubuntu. C'est donc **Ubuntu 18.04 LTS !** Laissez le reste par défaut et fournissez un **mot de passe fort,** puis cliquez sur déployer :

![Déployez image 2](https://learn.getgrav.org/user/pages/09.webservers-hosting/02.vps/02.linode/pick-distro.png)

La création de votre serveur devrait prendre environ 30 secondes, après quoi vous pourrez cliquer sur le bouton **Boot** pour le faire fonctionner :

![Bouton](https://learn.getgrav.org/user/pages/09.webservers-hosting/02.vps/02.linode/booted.png)

Vous pouvez cliquer sur l'onglet Accès à distance dans le Linode Manager pour obtenir des informations pertinentes sur la façon de se connecter à distance à l'instance VPS que vous venez de configurer. Vous pouvez vous connecter en SSH via la commande fournie dans cet onglet en utilisant le mot de passe que vous avez saisi lors de la création de l'instance de distribution. L'authentification par clé publique est recommandée, et Linode a une bonne **documentation d'authentification par clé publique SSH **qui vous guide à travers les étapes requises.

<h2 id="Mettre à jour et mettre à niveau les packages">Mettre à jour et mettre à niveau les packages.
<a href="#Mettre à jour et mettre à niveau les packages" class="toc-anchor after"></a></h2>

À ce stade, vous souhaiterez peut-être soit configurer une entrée `/etc/hosts` locale pour donner à l'adresse IP fournie un nom convivial tel que `linode.dev`. De cette façon, vous pouvez plus facilement vous connecter en SSH à votre serveur avec `ssh root@linode.dev`.

Après avoir réussi à vous connecter en SSH à votre serveur en tant que root, la première chose à faire est de mettre à jour et de mettre à niveau tous les packages installés. Cela garantira que vous utilisez le dernier et le meilleur :

```console
$ | # apt update
$ | # apt upgrade
```

Répondez simplement `Y` si vous y êtes invité.

Avant d'aller plus loin, supprimons **Apache2** que nous remplacerons par **Nginx** :

```console
$ | # apt remove apache2*
$ | # apt autoremove
```

<div class = "notice info">
REMARQUE : Vous ne l'avez peut-être pas installé. Mais mieux vaut prévenir que guérir !
</div>

Ensuite, vous voudrez installer certains packages essentiels :

    $ | # apt install vim zip unzip nginx git php-fpm php-cli php-gd php-curl php-mbstring php-xml php-zip php-apcu

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
3  | utilisateur = grav
4  | groupe = grav
5  | 
6  | écouter = /var/run/php/php7.2-fpm.sock
7  | 
8  | listen.owner = www-data
9  | listen.group = www-data
10 | 
11 | pm = dynamique
12 | pm.max_enfants = 5
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
$ | $ mkdir -p www/html
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
1  - server {
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
23 |     location ~* /(system|vendor)/.*\.(txt|xml|md|html|yaml|yml|php|pl|py|cgi|twig|sh|bat)$ { return 403; }
24 |     # deny running scripts inside user folder
25 |     location ~* /user/.*\.(txt|md|yaml|yml|php|pl|py|cgi|twig|sh|bat)$ { return 403; }
26 |     # deny access to specific files in the root folder
27 |     location ~ /(LICENSE\.txt|composer\.lock|composer\.json|nginx\.conf|web\.config|28 - htaccess\.txt|\.htaccess) { return 403; }
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

Il s'agit du fichier stock `nginx.conf` fourni avec Grav avec 2 modifications. 1) `root` a été adaptée à notre utilisateur/dossier que nous venons de créer et l'option `fastcgi_pass` a été définie sur le socket que nous avons défini dans notre pool `grav`. Il ne nous reste plus qu'à lier ce fichier de manière appropriée pour qu'il soit activé :

```console
$ | # cd ../sites-enabled
$ | # ln -s ../sites-available/grav
$ | # rm default
```

Vous pouvez tester la configuration avec la commande `nginx -t`. Il devrait renvoyer ce qui suit.

```console
$ | nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
$ | nginx: configuration file /etc/nginx/nginx.conf test is successful
```

Maintenant, tout ce que nous avons à faire est de redémarrer Nginx et le processus php7-fpm et de tester pour nous assurer que nous avons correctement configuré Nginx et le pool de connexion PHP :

```console
$ | # systemctl restart nginx 
$ | # systemctl restart php7.2-fpm
```

Pointez maintenant votre navigateur vers votre serveur : `http://linode.dev` et vous devriez voir le texte : **Working !**

Vous pouvez également tester pour vous assurer que PHP est installé et fonctionne correctement en pointant votre navigateur vers : `http://linode.dev/info.php`. Vous devriez voir une page d'informations PHP standard avec APCu, Opcache, etc.

<h2 id="Installation de Grav">Installation de Grav.
<a href="#Installation de Grav" class="toc-anchor after"></a></h2>

C'est la partie facile ! Nous devons d'abord revenir à l'utilisateur Grav, donc SSH en tant que `grav@linode.dev` ou `su - grav` à partir de la connexion root. puis suivez ces étapes :

```console
$ | $ cd ~/www
$ | $ wget -O grav.zip https://getgrav.org/download/core/grav/latest
$ | $ unzip grav.zip
$ | $ rm -Rf html
$ | $ mv grav html
```

Maintenant que c'est fait, vous pouvez confirmer que Grav est installé en pointant votre navigateur sur `http://linode.dev` et vous devriez être accueilli avec la page **Grav est en cours d'exécution !**.

Parce que vous avez suivi ces instructions avec diligence, vous pourrez également utiliser les commandes[Grav CLI](/cli-commandes-grav) et [Grav GPM](/cli-commandes-gpm) telles que :

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

et commandes GPM :

    $ | $ bin/gpm index
    
