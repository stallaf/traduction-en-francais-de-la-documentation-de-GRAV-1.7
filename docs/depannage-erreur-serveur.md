<h1 class="rem">Erreur du serveur Grav</h1>

![Erreur Serveur](https://learn.getgrav.org/user/pages/11.troubleshooting/02.server-error/grav-server-error.png)

Les erreurs de serveur sont presque toujours causées par une mauvaise configuration de Grav. Quelque chose d'inattendu s'est produit et à cause de cela, Grav est incapable de récupérer et de servir la page.

Lorsque vous voyez ce message, cela signifie que votre serveur fonctionne en mode `Production` pour empêcher l'affichage d'informations potentiellement sensibles à vos utilisateurs. L'erreur elle-même sera stockée dans le fichier `logs/grav.log`. Veuillez examiner ce fichier pour déterminer la nature exacte de l'erreur.

Les raisons possibles incluent :

* Les erreurs de serveur sont causées par une configuration obsolète
* Autorisations de fichier incorrectes qui empêchent Grav d'écrire des données
* Changements dans le système de fichiers dont Grav n'est pas encore au courant
* Erreurs dans l'analyse de la configuration en raison de fichiers de configuration au format incorrect

<div class = "notice tip">
Si vous avez installé le plugin <strong>Grav Administration</strong>, vous pouvez parcourir les erreurs de serveur à partir de là. En cliquant sur les erreurs individuelles, vous pouvez voir les pages de débogage même si le débogueur a été désactivé.
</div>

<h2 id="Configuration obsolète">Configuration obsolète.
<a href="#Configuration obsolète" class="toc-anchor after"></a></h2>

La première chose à faire est de vider le cache pour vous assurer que la configuration est à jour :

    $ | bin/grav clearcache

<div class = "notice info">
Avant de continuer, assurez-vous que vous n'avez pas d'autres problèmes d'autorisation de fichier comme celui-ci.
</div>

<h2 id="Problèmes d'installation et de configuration">Problèmes d'installation et de configuration.
<a href="#Problèmes d'installation et de configuration" class="toc-anchor after"></a></h2>

* Configuration requise
* autorisations de fichiers
* problèmes d'installation
* problèmes de configuration

