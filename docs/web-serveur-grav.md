<h1 class="rem">Serveur Web intégré Grav</h1>

Pour que Grav soit opérationnel rapidement, vous pouvez exécuter Grav à l'aide d'une simple commande à partir du terminal / invite de commande en utilisant le serveur PHP intégré disponible tant que PHP est installé.

Tout ce que vous avez à faire est d'accéder à la racine de votre installation Grav à l'aide du terminal ou de l'invite de commande et d'entrer le serveur `bin/grav server`.

<div class = "notice info">
Bien que techniquement tout ce dont vous avez besoin est PHP installé, si vous installez <a href = https://symfony.com/download>l'application Symfony CLI</a>, le serveur fournira un certificat SSL afin que vous puissiez utiliser <code>https://</code> et utiliser PHP-FPM pour de meilleures performances.
</div>

La saisie de cette commande vous présentera une sortie semblable à la suivante :

```console
$ | ➜ bin/grav server
  | 
  | Grav Web Server
  | ===============
  | 
  | Tailing Web Server log file (/Users/joeblow/.symfony/log/   96e710135f52930318e745e901e4010d0907cec3.log)
  | Tailing PHP-FPM log file (/Users/joeblow/.symfony/log/96e710135f52930318e745e901e4010d0907cec3/53fb8ec204547646acb3461995e4da5a54cc7575.log)
  | Tailing PHP-FPM log file (/Users/joeblow/.symfony/log/96e710135f52930318e745e901e4010d0907cec3/53fb8ec204547646acb3461995e4da5a54cc7575.log)
  | 
  | [OK] Web server listening
  | The Web server is using PHP FPM 8.0.8
  | https://127.0.0.1:8000
  |  
  | [Web Server ] Jul 30 14:54:53 |DEBUG  | PHP    Reloading PHP versions
  | [Web Server ] Jul 30 14:54:54 |DEBUG  | PHP    Using PHP version 8.0.8 (from default version in $PATH)
  | [PHP-FPM    ] Jul  6 14:40:17 |NOTICE | FPM    fpm is running, pid 64992
  | [PHP-FPM    ] Jul  6 14:40:17 |NOTICE | FPM    ready to handle connections
  | [PHP-FPM    ] Jul  6 14:40:17 |NOTICE | FPM    fpm is running, pid 64992
  | [PHP-FPM    ] Jul  6 14:40:17 |NOTICE | FPM    ready to handle connections
  | [Web Server ] Jul 30 14:54:54 |INFO   | PHP    listening path="/usr/local/Cellar/php/8.0.8_2/sbin/php-fpm" php="8.0.8" port=65140
  | [PHP-FPM    ] Jul 30 14:54:54 |NOTICE | FPM    fpm is running, pid 73709
  | [PHP-FPM    ] Jul 30 14:54:54 |NOTICE | FPM    ready to handle connections
  | [PHP-FPM    ] Jul 30 14:54:54 |NOTICE | FPM    fpm is running, pid 73709
  | [PHP-FPM    ] Jul 30 14:54:54 |NOTICE | FPM    ready to handle connections
```

Votre terminal vous donnera également des mises à jour en temps réel de toute activité sur ce serveur de style ad hoc. Vous pouvez copier l'URL fournie dans la ligne `[OK] Web Server listening` et la coller dans le navigateur de votre choix pour accéder à votre site, y compris l'administrateur.

`https://127.0.0.1:8000`

<div class = "notice warning">
Il s'agit d'un outil utile pour un développement rapide et ne doit <strong>pas</strong> être utilisé à la place d'un serveur Web dédié tel qu'Apache ou Nginx.
</div>
        
