<h1 class="rem">Problèmes de proxy</h1>

L'exécution de commandes GPM derrière un proxy peut entraîner une erreur.

cURL vous permet de définir le proxy en tant que variable d'environnement (`http_proxy et https_proxy`), sans modifications nécessaires dans Grav.

Voir [http://stackoverflow.com/questions/7559103/how-to-setup-curl-to-permanently-use-a-proxy](https://stackoverflow.com/questions/7559103/how-to-setup-curl-to-permanently-use-a-proxy)

Mais d'abord, si votre environnement a activé `fopen`, vous devez le désactiver en désactivant `allow_url_fopen` via php.ini.

En effet, si `fopen` est disponible, Grav l'utilise automatiquement sur `curl`.

