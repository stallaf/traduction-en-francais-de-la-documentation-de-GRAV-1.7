<h1 class="rem">Serveur Web PHP intégré</h1>

<h1 class="rem">Tester l'hébergement avec le serveur Web intégré PHP</h1>

La ligne de commande PHP (CLI SAPI) dispose d'un serveur Web intégré qui est utile pour les tests ou les démonstrations rapides du site Grav. Il ne s'agit pas d'un serveur Web complet et ne doit pas être utilisé sur un réseau public.

<h2 id="Utilisation du serveur Web CLI">Utilisation du serveur Web CLI.
<a href="#Utilisation du serveur Web CLI" class="toc-anchor after"></a></h2>

1. Sur la ligne de commande, accédez au dossier [GRAV_ROOT].

2. Exécutez `php -S localhost:8080 system/router.php` pour démarrer le serveur. Vous devriez voir une réponse comme celle ci-dessous.

`php -S localhost:8080 system/router.php`  
`PHP 7.3.27 Development Server started at Thu Jun 17 09:24:46 2021`  
`Listening on http://localhost:8080`  
`Document root is /Users/somerandom/Desktop/quick-grav-test`  
`Press Ctrl-C to quit.`

Browse to the URL specified, e.g., 

3. Accédez à l'URL spécifiée, par exemple, `http://localhost:8080/`.

4. Pour arrêter le serveur Web, appuyez sur Ctrl-C.

<h3 id="Erreur d'adresse déjà utilisée">Erreur d'adresse déjà utilisée.
<a href="#Erreur d'adresse déjà utilisée" class="toc-anchor after"></a></h3>

Si vous obtenez une erreur "Adresse déjà utilisée" lors de l'exécution de la commande `php -S`, il existe déjà un serveur Web en cours d'exécution sur votre machine au numéro de port spécifié (par exemple, `:8080`). Vous pouvez résoudre ce problème en modifiant le numéro de port dans votre commande (par exemple `:8181`) et en réessayant.

<h2 id="Affichage du journal en temps réel">Affichage du journal en temps réel.
<a href="#Affichage du journal en temps réel" class="toc-anchor after"></a></h2>

Le serveur Web CLI affiche son journal en temps réel lorsque vous naviguez sur le site, ce qui peut être utile pour des tests rapides.

`PHP 7.3.27 Development Server started at Thu Jun 17 09:24:46 2021`  
`Listening on http://localhost:8080`  
`Document root is /Users/somerandom/Desktop/quick-grav-test`  
`Press Ctrl-C to quit.`  
`[Thu Jun 17 09:26:15 2021] 127.0.0.1:63965 [200]: / `  
`[Thu Jun 17 09:26:15 2021] 127.0.0.1:64007 [200]: /assets/fd2c5827e1f18bb54d20265f4fc56b59.css?g-74e4c5a3`  
`[Thu Jun 17 09:26:15 2021] 127.0.0.1:64008 [200]: /assets/d87a2d24fae663a8c55e144c963a1915.js?g-74e4c5a3`  
`[Thu Jun 17 09:26:15 2021] 127.0.0.1:64014 [200]: /assets/1d8c5ea92966046d4649472f1630a253.js?g-74e4c5a3`  
`[Thu Jun 17 09:26:16 2021] 127.0.0.1:64024 [200]: /user/images/navigation/logo_small.png`  
`[Thu Jun 17 09:26:16 2021] 127.0.0.1:64028 [200]: /user/images/navigation/bgdark.svg`  
`[Thu Jun 17 09:26:16 2021] 127.0.0.1:64030 [200]: /user/images/navigation/bglight_50.png`  
`[Thu Jun 17 09:26:16 2021] 127.0.0.1:64032 [200]: /user/images/navigation/brand.svg`

<h2 id="Apprendre encore plus">Apprendre encore plus.
<a href="#Apprendre encore plus" class="toc-anchor after"></a></h2>

Visitez le site Web PHP pour en savoir plus sur le [serveur Web CLI](https://www.php.net/manual/en/features.commandline.webserver.php).

