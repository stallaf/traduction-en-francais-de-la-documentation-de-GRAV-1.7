<h1 class="rem">Jeton de sécurité non valide</h1>

**Problème** : vous obtenez cette erreur dans le panneau d'administration lorsque vous vous connectez pour effectuer des opérations

Il y a quelques causes possibles du problème, toutes liées à la Session :

* Essayez de **recharger** votre navigateur pour obtenir un nouveau jeton

* Essayez d'effacer les cookies de session de votre navigateur, essayez de vous déconnecter et de vous reconnecter.

* Assurez-vous que vous exécutez sous SSL et une URL HTTPS si vous avez `session.secure: true` défini dans le `system.yaml` de Grav

* Vérifiez que PHP a le bon chemin tmp configuré. Cela peut être défini directement dans PHP ou en définissant le paramètre `system.yaml session.path` de Grav (il peut également être défini via Admin, dans la configuration du système) [problème signalé](https://github.com/getgrav/grav-plugin-admin/issues/958).

* Assurez-vous que la configuration de votre serveur Web est correcte et inclut la chaîne de requête [problème signalé](https://github.com/getgrav/grav-plugin-admin/issues/893).

