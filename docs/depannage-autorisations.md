<h1 class="rem">Autorisations</h1>

Selon votre environnement d'hébergement, les autorisations peuvent ou non être un problème dont vous devez vous préoccuper. La chose importante à comprendre est qu'il y a un problème potentiel si l'utilisateur que vous utilisez pour éditer vos fichiers sur le système de fichiers est différent de l'utilisateur sous lequel PHP s'exécute (généralement le serveur Web), ou à tout le moins, les deux utilisateurs n'ont pas accès en **lecture/écriture** à ces fichiers.

Tout d'abord, découvrez avec quel utilisateur Apache ou Nginx s'exécute en exécutant la commande suivante pour Apache :

  `ps aux | grep -v root | grep apache | cut -d\  -f1 | sort | uniq`

Pour Nginx :

  `ps aux | grep -v root | grep nginx | cut -d\  -f1 | sort | uniq`

Et découvrez quel utilisateur possède le fichier dans votre répertoire grav en exécutant

  `ls -l`

En tant que CMS basé sur des fichiers, Grav doit écrire dans le système de fichiers afin de créer des fichiers de cache et de journalisation. Il existe trois scénarios principaux :

1. <h4>PHP/Webserver s'exécute avec le même utilisateur qui édite les fichiers. (Préféré)</h4>

     C'est l'approche utilisée par la plupart des configurations **d'hébergement partagé** et fonctionne également bien pour le développement local. Le billet de blog que nous avons écrit concernant [MacOS Yosemite, Apache et PHP](https://getgrav.org/blog/mac-os-x-apache-setup-multiple-php-versions) explique comment configurer Apache pour qu'il s'exécute en tant que compte d'utilisateur personnel. Cette approche n'est pas considérée comme suffisamment sécurisée pour être utilisée sur un hébergeur dédié, c'est pourquoi la deuxième ou la troisième option doit être utilisée.
     
2. <h4>PHP/Webserver fonctionne avec des comptes différents mais le même groupe</h4>

     En utilisant un groupe partagé entre votre utilisateur et votre compte PHP/Webserver avec les autorisations `775` et `664`, vous vous assurez que même si vous avez deux comptes différents, les deux auront un accès en **lecture/écriture** aux fichiers. Vous devriez également probablement définir un `umask 0002` à la racine afin que les nouveaux fichiers soient créés avec les autorisations appropriées.
     
3. <h4>Différents comptes, corrigez les autorisations manuellement</h4>

     La dernière approche consiste à avoir des comptes complètement différents et à mettre à jour la propriété et les autorisations des fichiers après l'édition pour s'assurer que l'utilisateur PHP/Webserver peut **lire/écrire** correctement.

Un simple script shell de **correction des autorisations** peut être utilisé pour cela :

```bash
1 | #!/bin/ch
2 | chown -R joeblow:staff .
3 | find . -type f -exec chmod 664 {} \;
4 | find ./bin -type f -exec chmod 775 {} \;
5 | find . -type d -exec chmod 775 {} \;
6 | find . -type d -exec chmod +s {} \;
```

Vous pouvez utiliser ce fichier et le modifier selon vos besoins pour l'utilisateur et le groupe appropriés qui fonctionnent pour votre configuration. Ce que fait essentiellement ce script, c'est :

1. Modifie le répertoire actuel, tous les fichiers et sous-dossiers en `joeblow` et la propriété `staff`
2. Recherche tous les fichiers du répertoire actuel et définit les autorisations sur `664` afin qu'ils soient `RW` pour Utilisateur et groupe et `R` pour Autres.
3. Recherche tous les dossiers du répertoire actuel et définit les autorisations sur `775` afin qu'ils soient `RWX` pour l'utilisateur et le groupe et `RX` pour les autres.
4. Définit la **propriété** de tous les répertoires pour s'assurer que les changements d'utilisateur et de groupe sont conservés



Si les fichiers image dans le dossier cache sont écrits avec les mauvaises autorisations, essayez de définir dans votre fichier `user/config/system.yaml`,

    1 | images:
    2 |    cache_perms : '0775'

si la propriété `images` est déjà présente, ajoutez simplement `cache_perms: '0775'` à la fin de celle-ci.

Si cela ne fonctionne toujours pas, créez un fichier `setup.php` dans le dossier racine Grav (celui avec `index.php`), et ajoutez-y :

    1 | <?php
    2 | masque(0002);


Si vous avez déjà un fichier `setup.php`, ajoutez simplement cette ligne en haut. Ce fichier est couramment utilisé pour la configuration multisite, mais étant appelé dans chaque appel Grav, vous pouvez également l'utiliser pour d'autres utilisations.

<h2 id="Co-hébergement avec un site WordPress">Co-hébergement avec un site WordPress.
<a href="#Co-hébergement avec un site WordPress" class="toc-anchor after"></a></h2>

En général, Grav peut être installé dans un dossier de niveau racine d'un site WordPress existant et les deux CMS coexisteront bien. (N'oubliez pas de définir Base Rewrite dans le htaccess du dossier Grav.) Si vous rencontrez des erreurs d'autorisations avec les fichiers de cache lors de l'accès aux pages Admin et/ou de la visualisation de Grav, vérifiez si WP-Engine est installé pour ce site WordPress. Si c'est le cas, vous devrez contacter leur support pour créer une exception pour le dossier Grav à partir de leur service de cache distribué agressif.

<h2 id="Conseils spécifiques à SELinux">Conseils spécifiques à SELinux.
<a href="#Conseils spécifiques à SELinux" class="toc-anchor after"></a></h2>

Si les suggestions ci-dessus ne fonctionnent toujours pas, exécutez

  `chcon -Rv system_u:object_r:httpd_sys_rw_content_t:s0 ./` dans le dossier racine Grav.

Les références:

* [https://unix.stackexchange.com/questions/337704/selinux-is-preventing-nginx-from-writing-via-php-fpm](https://unix.stackexchange.com/questions/337704/selinux-is-preventing-nginx-from-writing-via-php-fpm)
* [https://github.com/getgrav/grav/issues/912#issuecomment-227627196](https://github.com/getgrav/grav/issues/912#issuecomment-227627196)
* [http://stopdisablingselinux.com](http://stopdisablingselinux.com/)
* [http://stackoverflow.com/questions/28786047/failed-to-open-stream-on-file-put-contents-in-php-on-centos-7](https://stackoverflow.com/questions/28786047/failed-to-open-stream-on-file-put-contents-in-php-on-centos-7)
* [http://www.serverlab.ca/tutorials/linux/web-servers-linux/configuring-selinux-policies-for-apache-web-servers/](http://www.serverlab.ca/tutorials/linux/web-servers-linux/configuring-selinux-policies-for-apache-web-servers/)

