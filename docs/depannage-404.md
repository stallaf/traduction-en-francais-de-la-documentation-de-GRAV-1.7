<h1 class="rem">404 Non trouvé</h1>

Il y a plusieurs raisons pour lesquelles vous pourriez recevoir une erreur **Not Found**, et elles sont chacune causées par différents facteurs.

![404](https://learn.getgrav.org/user/pages/11.troubleshooting/01.page-not-found/404-not-found.png)

<div class = "notice info">
Les exemples ci-dessous concernent le serveur Web Apache, qui est le logiciel serveur le plus couramment utilisé.
</div>

<h2 id="Utilisation IIS du fichier .htaccess">Utilisation IIS du fichier .htaccess.
<a href="#Utilisation IIS du fichier .htaccess" class="toc-anchor after"></a></h2>

Après avoir ajouté la réécriture d'URL au serveur IIS à l'aide de Web Platform Installer, redémarrez le serveur IIS. Allez dans l'interface de gestion, IIS, double-cliquez sur URL Rewrite, sous Inbound Rules, cliquez sur Import Rules, sous Rules to Import, naviguez jusqu'au fichier de configuration, choisissez le fichier .htaccess à la racine, puis cliquez sur Import. Redémarrez le serveur IIS. Accédez à Grav maintenant.

<h2 id="Fichier .htaccess manquant">Fichier .htaccess manquant.
<a href="#Fichier .htaccess manquant" class="toc-anchor after"></a></h2>

La première chose à vérifier est si vous avez le fichier `.htaccess` fourni à la racine de votre installation Grav. Comme il s'agit d'un fichier **caché**, vous ne le verrez normalement pas dans les fenêtres de votre explorateur ou de votre Finder. Si vous avez extrait Grav puis **sélectionné** et **déplacé** ou **copié** les fichiers, vous avez peut-être oublié ce fichier très important.

Il est **fortement conseillé** de décompresser Grav et de déplacer le **dossier entier** en place, puis de simplement renommer le dossier. Cela garantira que tous les fichiers conservent leurs positions appropriées.

<h2 id="Autoriser tout remplacer">Autoriser tout remplacer.
<a href="#Autoriser tout remplacer" class="toc-anchor after"></a></h2>

Pour que le `.htaccess` fourni par Grav puisse définir les règles de réécriture nécessaires au bon fonctionnement du routage, Apache doit d'abord lire le fichier. Lorsque votre directive `<Directory>` ou `<VirtualHost>` est configurée avec `AllowOverride None`, le fichier `.htaccess` est complètement ignoré. La solution la plus simple consiste à changer cela en `AllowOverride All` où RewriteRule est utilisé, **FollowSymLinks** ou **SymLinksIfOwnerMatch** doit être défini dans la directive Options. Ajoutez simplement sur la même ligne '+FollowSymlinks' après 'Options'

Vous trouverez plus de détails sur `AllowOverride` et toutes les options de configuration possibles dans la documentation Apache.

<h2 id="Problème de réécriture de base">Problème de réécriture de base.
<a href="#Problème de réécriture de base" class="toc-anchor after"></a></h2>

Si la page d'accueil de votre site Grav se charge, mais que **toute autre page** affiche cette erreur très grossière de style Apache, la cause la plus probable est qu'il y a un problème avec votre fichier `.htaccess`.

Le `.htaccess` par défaut fourni avec Grav fonctionne correctement dans la plupart des cas. Cependant, il existe certaines configurations impliquant des hôtes virtuels où le système de fichiers ne correspond pas directement à la configuration de l'hébergement virtuel. Dans ces cas, vous devez configurer l'option `RewriteBase` dans le `.htaccess` pour pointer vers le chemin correct.

Il y a une courte explication à cela dans le fichier `.htaccess` lui-même :

```configuration
1 | ##
2 | # If you are getting 404 errors on subpages, you may have to uncomment the RewriteBase entry
3 | # You should change the '/' to your appropriate subfolder. For example if you have
4 | # your Grav install at the root of your site '/' should work, else it might be something
5 | # along the lines of: RewriteBase /<your_sub_folder>
6 | ##
7 |
8 |# RewriteBase /
```

Supprimez simplement le `#` avant la directive `RewriteBase /` pour le décommenter et ajustez le chemin pour qu'il corresponde à votre environnement de serveur.

Nous avons inclus des informations supplémentaires pour vous aider à localiser et à dépanner votre fichier `.htaccess dans notre [guide htaccess](/depannage-htaccess).

<h2 id="Modules de réécriture manquants">Modules de réécriture manquants.
<a href="#Modules de réécriture manquants" class="toc-anchor after"></a></h2>

Certains packages de serveur Web (je regarde votre EasyPHP et WAMP !) ne sont pas livrés avec le module de **réécriture** Apache activé par défaut. Ils peuvent généralement être activés à partir des paramètres de configuration d'Apache, ou vous pouvez le faire manuellement via le `httpd.conf` en décommentant cette ligne (ou quelque chose de similaire) afin qu'ils soient chargés par Apache :

    #LoadModule rewrite_module modules/mod_rewrite.so

Redémarrez ensuite votre serveur Apache.

<h2 id="Script de test .htaccess">Script de test .htaccess.
<a href="#Script de test .htaccess" class="toc-anchor after"></a></h2>

Pour aider à isoler `.htaccess` et **réécrire** les problèmes, vous pouvez télécharger ce fichier [htaccess_tester.php](https://gist.githubusercontent.com/rhukster/a727fb70d9341536d49980d1239bd97e/raw/a3078da16b894ba86f9d000bcfc4850e098199fc/htaccess_tester.php) et le déposer dans votre répertoire racine Grav.

Pointez ensuite votre navigateur sur `http://votresite.com/htaccess_tester.php`. Vous devriez obtenir un message de réussite et une copie du fichier Grav `.htaccess` affiché.

![.htaccess](https://learn.getgrav.org/user/pages/11.troubleshooting/01.page-not-found/htaccess_tester.png)

Ensuite, vous pouvez tester si les réécritures fonctionnent en sauvegardant le fichier .htaccess existant :

    $ | mv .htaccess .htaccess-backup

Et puis essayez ce simple fichier `.htaccess` :

```configuration
1 | <IfModule mod_rewrite.c>
2 |     RewriteEngine On
3 |     RewriteRule ^.*$ htaccess_tester.php
4 | </IfModule>
```

Essayez ensuite cette URL : `http://votresite.com/test`. En fait, tout chemin que vous utilisez devrait afficher un message de réussite vous indiquant que `mod_rewrite` fonctionne.

Une fois les tests terminés, vous devez supprimer le fichier de test et restaurer votre fichier `.htaccess` :

    $ | rm htaccess_tester.php
    $ | mv .htaccess-backup .htaccess

<h2 id="Page Grav d'erreur 404">Page Grav d'erreur 404.
<a href="#Page Grav d'erreur 404" class="toc-anchor after"></a></h2>

![404 Non trouvé](https://learn.getgrav.org/user/pages/11.troubleshooting/01.page-not-found/error-404.png)

Si vous recevez une erreur de *style Grav* disant **Erreur 404**, votre `.htaccess` fonctionne correctement, mais vous essayez d'atteindre une page que Grav ne peut pas trouver.

La cause la plus fréquente de ceci est simplement que la page a été déplacée ou renommée. Une autre chose à vérifier est si la page a un `slug` défini dans les en-têtes YAML de la page. Cela remplace le nom de dossier explicite utilisé par défaut pour construire l'URL.

Une autre cause pourrait être que votre page n'est pas routable. L'option routable d'une page peut être définie dans les [en-têtes de page](/en-tete-frontmatter)</span>.

<h2 id="404 Page introuvable sur Nginx">404 Page introuvable sur Nginx.
<a href="#404 Page introuvable sur Nginx" class="toc-anchor after"></a></h2>404 Page introuvable sur Nginx

Si votre site se trouve dans un sous-dossier, assurez-vous que votre emplacement nginx.conf pointe vers ce sous-dossier. [L'exemple nginx.conf](https://github.com/getgrav/grav/blob/master/webserver-configs/nginx.conf) de Grav a un commentaire dans le code qui explique comment.

Si votre page d'accueil fonctionne mais que d'autres pages sont introuvables, assurez-vous que votre nginx.conf est configuré conformément à l'exemple nginx.conf.

