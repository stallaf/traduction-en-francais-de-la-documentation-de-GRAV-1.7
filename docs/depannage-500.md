<h1 class="rem">500 Erreur de serveur interne</h1>

<div class="notice defaut">
Le serveur a rencontré une erreur interne ou une mauvaise configuration et n'a pas pu traiter votre demande.
<br><br>
Veuillez contacter l'administrateur du serveur à l'adresse webmaster@localhost pour l'informer de l'heure à laquelle cette erreur s'est produite et des actions que vous avez effectuées juste avant cette erreur.
<br><br>
Plus d'informations sur cette erreur peuvent être disponibles dans le journal des erreurs du serveur. *Serveur Apache/2.4.7 sur le port 80 de l'hôte local*
</div>

Cette erreur peut être déclenchée par les éléments suivants :

* mauvaise configuration du serveur (httpd.conf)
* problèmes .htaccess
* mod_security ou similaire

<h2 id="Tester que PHP fonctionne">Tester que PHP fonctionne.
<a href="#Tester que PHP fonctionne" class="toc-anchor after"></a></h2>

La première chose à faire est de vous assurer que PHP fonctionne correctement sur votre serveur et que Grav n'est pas la cause directe du problème. Pour tester cela, créez simplement un fichier temporaire (supprimez-le ensuite pour des raisons de sécurité !) à la racine de votre dossier Grav appelé `info.php`. Ce fichier doit contenir le code PHP suivant :

    1 | <?php phpinfo();

Pointez ensuite votre navigateur vers ce fichier : `http://yoursite.com/your_grav_directory/info.php`. Vous devriez obtenir une page de rapport répertoriant toutes les informations relatives à la configuration PHP, y compris la version et les extensions chargées.

<h2 id="Vérifier les autorisations">Vérifier les autorisations.
<a href="#Vérifier les autorisations" class="toc-anchor after"></a></h2>

Une erreur 500 peut être déclenchée en ayant les mauvaises autorisations. Consultez le [guide des autorisations](/depannage-autorisations).

<h2 id="Enregistrer le problème Globals">Enregistrer le problème Globals.
<a href="#Enregistrer le problème Globals" class="toc-anchor after"></a></h2>

Certaines personnes qui ont récemment mis à niveau vers PHP 5.5 à partir de la version 5.4 ou 5.3 peuvent encore avoir des paramètres obsolètes dans leur fichier php.ini. Un élément qui peut provoquer une **erreur 500 interne du serveur** est le paramètre `register_globals`. Supprimez ou commentez simplement la ligne :

    1 | register_global = On

Redémarrez ensuite votre serveur Apache.

<h2 id="ThreadStackSize sous Windows">ThreadStackSize sous Windows.
<a href="#ThreadStackSize sous Windows" class="toc-anchor after"></a></h2>

Si votre serveur fonctionne sous Windows, vous pourriez obtenir une erreur de serveur interne 500 en raison du fait que **ThreadStackSize** est beaucoup trop petit. Ajoutez simplement ce code au bas de votre fichier httpd.conf :

```configuration
1 | <IfModule mpm_winnt_module>
2 |   ThreadStackSize 8388608
3 | </IfModule>
```

Redémarrez ensuite votre serveur Apache.

<h2 id="Options -Index">Options -Index.
<a href="#Options -Index" class="toc-anchor after"></a></h2>

Grav utilise une option `-Indexes` pour ne forcer aucune liste de répertoires de dossiers. Certains hôtes n'aiment pas qu'Apache `.htaccess` manipule le paramètre Options.

Nous avons vu des rapports selon lesquels le simple fait de commenter cette ligne dans le fichier `.htaccess` de Grav peut résoudre les problèmes d'erreur du serveur interne pour les utilisateurs dans cette situation :

    1 | # Prevent file browsing
    2 | # Options -Indexes

<h2 id="Problèmes de réécriture de base">Problèmes de réécriture de base.
<a href="#Problèmes de réécriture de base" class="toc-anchor after"></a></h2>

Ont eu des rapports de 500 erreurs de serveur internes sans définir la RewriteBase, sur l'hébergement 1 & 1 (mais peuvent également s'appliquer à d'autres). Essayez de changer

    1 | # RewriteBase /

à

    1 | RewriteBase /

Crédit : [http://ahcox.com/webdev/1and1-internal-server-error-grav](http://ahcox.com/webdev/1and1-internal-server-error-grav/)

<h2 id="Navigation dans le panneau d'administration">Navigation dans le panneau d'administration.
<a href="#Navigation dans le panneau d'administration" class="toc-anchor after"></a></h2>

Lorsque vous naviguez dans le panneau d'administration de Grav, le message **d'erreur interne du serveur** apparaît en haut à gauche. Cela est dû à des autorisations incorrectes sur votre dossier /cache.

![Erreur interne du serveur](https://i.imgur.com/vyPfoZ7.png)

Si cette erreur apparaît, il est probable que vous n'ayez pas défini la bonne autorisation sur le dossier /cache, plutôt que de simplement rendre le dossier inscriptible, vous devez le rendre récursivement inscriptible. L'exécution de la commande ci-dessous à partir de votre répertoire Grav devrait résoudre le problème.

    $ | sudo chmod 755 cache/-R
    
