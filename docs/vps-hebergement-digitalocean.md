<h1 class="rem">DigitalOcean</h1>

Peut-être le plus populaire et le plus utilisé de tous les fournisseurs de VPS, [DigitalOcean](https://www.digitalocean.com/) propose une gamme d'options VPS. À partir de **5 $/mois pour un système à 1 processeur, 1024 Mo** jusqu'à 960 $/mois pour une configuration à 32 processeurs, 192 Go, [DigitalOcean](https://www.digitalocean.com/) propose des solutions qui peuvent évoluer avec vous. Tous leurs serveurs sont construits avec des **disques SSD RAID, du matériel hexa-core moderne, une virtualisation KVM** et une **bande passante de niveau 1** fiable pour garantir des performances maximales. Ils constituent une option fantastique pour héberger votre site basé sur Grav.

![Digitalocean](https://learn.getgrav.org/user/pages/09.webservers-hosting/02.vps/01.digitalocean/digitalocean.png)

Après avoir créé un compte et y avoir déposé du crédit, vous pouvez commencer. DigitalOcean vous permet de créer des **Droplets** qui représentent une instance VPS. Cliquez simplement sur le bouton **Create Droplet** dans votre panneau de configuration et remplissez le formulaire :

![formulaire](https://learn.getgrav.org/user/pages/09.webservers-hosting/02.vps/01.digitalocean/step-1.png)

Choisissez simplement un nom pour votre Droplet et **choisissez une taille** en fonction du prix et des besoins du serveur. Grav fonctionnera bien sur n'importe quelle configuration, même l'option de base à 5 $/mois exécutera Grav rapidement et efficacement.

![choix région](https://learn.getgrav.org/user/pages/09.webservers-hosting/02.vps/01.digitalocean/step-2.png)

Ensuite, **sélectionnez une région** où votre VPS sera situé. Il est préférable de choisir une région qui servira le mieux votre public cible. Si le serveur est uniquement à des fins de développement, choisissez celui qui est le plus proche de vous.

![choix image](https://learn.getgrav.org/user/pages/09.webservers-hosting/02.vps/01.digitalocean/step-3.png)

Enfin, vous devrez sélectionner une image à installer. DigitalOcean vous permet de choisir parmi une grande variété de distributions Linux en stock, ainsi que des applications complètes et même des instantanés enregistrés précédemment. Pour les besoins de ce guide, nous allons installer la dernière version **d'Ubuntu 18.04 LTS** qui est très populaire et très bien prise en charge.

Vous pouvez laisser toutes les autres options à leurs valeurs par défaut. Après avoir cliqué sur **Créer un droplet**, votre droplet sera créé dans les 55 secondes et vous le verrez répertorié dans votre liste de droplets. Vous devriez recevoir un email avec votre mot de passe root. En cliquant sur le droplet que vous venez de créer, vous verrez différentes options.

![options](https://learn.getgrav.org/user/pages/09.webservers-hosting/02.vps/01.digitalocean/droplet.png)

L'onglet **Accès** du Droplet Manager vous permet de vous connecter rapidement à votre instance, mais l'utilisation de SSH est une expérience plus agréable. L'authentification par clé publique est également recommandée, et DigitalOcean dispose d'une excellente [documentation d'authentification par clé publique SSH](https://www.digitalocean.com/community/tutorials/how-to-use-ssh-keys-with-digitalocean-droplets) qui vous guide à travers les étapes requises.

<h2 id="Mettre à jour et mettre à niveau les packages">Mettre à jour et mettre à niveau les packages.
<a href="#Mettre à jour et mettre à niveau les packages" class="toc-anchor after"></a></h2>

À ce stade, vous souhaiterez peut-être soit configurer une entrée `/etc/hosts` locale pour donner à l'adresse IP fournie un nom convivial tel que `digitalocean.dev.` De cette façon, vous pouvez plus facilement vous connecter en SSH à votre serveur avec `ssh root@digitalocean.dev.`

Après avoir réussi à vous connecter en SSH à votre serveur en tant que **root**, la première chose à faire est de mettre à jour et de mettre à niveau tous les packages installés. Cela garantira que vous utilisez le dernier et le meilleur :

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

Décommentez cette ligne et changez '1' en '0' pour qu'elle ressemble à ceci :

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

Les éléments clés ici sont que `user` et `droup` sont définis sur un utilisateur appelé `grav` et que le socket d'écoute a un nom unique à partir du socket standard. Enregistrez et quittez ce fichier.

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

```configation
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
23 |     location ~* /(system|vendor)/.*\.(txt|xml|md|html|yaml|yml|php|pl|py|cgi|twig|sh| bat)$ { return 403; }
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
Pointez maintenant votre navigateur sur votre serveur : `http://digitalocean.dev` et vous devriez voir le texte : **Working !**

Vous pouvez également tester pour vous assurer que PHP est installé et fonctionne correctement en pointant votre navigateur vers : `http://digitalocean.dev/info.php`. Vous devriez voir une page d'informations PHP standard avec APCu, Opcache, etc.

<h2 id="Installation de Grav">Installation de Grav.
<a href="#Installation de Grav" class="toc-anchor after"></a></h2>

C'est la partie facile ! Nous devons d'abord revenir à l'utilisateur Grav, donc SSH en tant que `grav@digitalocean.dev` ou `su - grav` à partir de la connexion root. puis suivez ces étapes :

```console
$ | $ cd ~/www
$ | $ wget -O grav.zip https://getgrav.org/download/core/grav/latest
$ | $ unzip grav.zip
$ | $ rm -Rf html
$ | $ mv grav html
```

Maintenant que c'est fait, vous pouvez confirmer que Grav est installé en pointant votre navigateur sur `http://digitalocean.dev` et vous devriez être accueilli avec la page **Grav est en cours d'exécution !**.

Parce que vous avez suivi ces instructions avec diligence, vous pourrez également utiliser les commandes [Grav CLI](/cli-commandes-grav) et [Grav GPM](/cli-commandes-gpm) telles que :

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
    
