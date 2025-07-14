<h1 class="rem">Utilisateurs</h1>

Lors de l'exécution de Grav, avec ou sans panneau d'administration installé, il y a quelques bonnes pratiques à garder à l'esprit. Celles-ci concernent qui peut accéder à quoi sur votre site Web et les risques potentiels de ne pas limiter les facteurs de risque à cet égard.

<h2 id="Utilisateurs Grav et panneau d'administration">Utilisateurs Grav et panneau d'administration.
<a href="#Utilisateurs Grav et panneau d'administration" class="toc-anchor after"></a></h2>

Lors de la création d'utilisateurs qui auront accès au [panneau d'administration](/dashboard), vous devez d'abord déterminer à quoi ils auront accès. Le plugin Admin offre des [autorisations](/profil/#Niveaux d'accès) solides qui doivent être définies pour restreindre ce que les nouveaux utilisateurs peuvent faire avec le site. Si vous avez de nombreux utilisateurs, et que certains d'entre eux n'écriront que du contenu pour le site, ceux-ci ne devraient généralement avoir besoin que de la permission `admin.pages` - ainsi que des permissions normales comme `admin.login`.

De plus, il est toujours préférable que vos utilisateurs n'aient pas un seul mot de passe qu'ils utilisent partout. Si quelqu'un volait son mot de passe, il pourrait alors se connecter partout. Un bon mot de passe est un mot de passe fort, mais même une longue phrase utilisant des mots du dictionnaire est plus facile à déchiffrer qu'un mot de passe composé de symboles, de lettres et de chiffres aléatoires. Cependant, tout humain aura du mal à se souvenir d'un mot de passe long et [aléatoire](https://xkcd.com/936/) - encore moins plusieurs - donc la [meilleure pratique](https://support.google.com/accounts/answer/32040) consiste à utiliser un [gestionnaire de mots de passe](https://alternativeto.net/tag/encrypted-passwords/) et **jamais le même mot de passe deux fois**. De nombreuses personnes aiment également que leur navigateur se souvienne des mots de passe de chaque site et ne se souviennent que d'un seul mot de passe fort pour les déverrouiller. Pour générer un mot de passe aléatoire, il vous suffit d'ouvrir le Bloc-notes et d'appuyer furieusement sur des touches aléatoires de votre clavier.

Dites à vos utilisateurs d'utiliser des mots de passe aléatoires et de créer un mot de passe fort et long dont ils se souviendront. À l'occasion, ce long mot de passe doit également être modifié. Et parce que le plugin Admin le propose, ils doivent toujours utiliser [l'authentification à 2 facteurs](/dashboard-authentification). Pour empêcher les attaques par force brute contre le panneau d'administration, l'administrateur doit également activer la [protection contre le flood](/dashboard-fllod).

<h2 id="Les utilisateurs du serveur et le Webmaster">Les utilisateurs du serveur et le Webmaster.
<a href="#Les utilisateurs du serveur et le Webmaster" class="toc-anchor after"></a></h2>

Le webmaster est la personne responsable de la maintenance du site Web et y a généralement accès au niveau du serveur. Cette personne doit bien sûr [sécuriser le serveur](/securite-server), mais également s'assurer qu'elle - et toute autre personne ayant accès au serveur - n'accède au serveur que de manière sécurisée. Cela signifie **qu'en aucun cas le protocole FTP ne doit être utilisé**, uniquement [SFTP](https://www.ssh.com/ssh/sftp/) avec des [paires de clés](https://www.ssh.com/ssh/public-key-authentication) sécurisées par phrase. L'hôte du serveur dispose généralement d'informations sur la désactivation du FTP standard et l'accès au serveur via SFTP, la création de paires de clés est[ bien documentée](https://www.linode.com/docs/security/authentication/use-public-key-authentication-with-ssh/#generating-keys).

Plus généralement, déterminez si un autre utilisateur a besoin d'un accès au serveur. Chaque utilisateur supplémentaire ayant accès est un risque potentiel, non seulement par son propre comportement, mais par le danger que si ses clés ou mots de passe sont volés, d'autres peuvent accéder directement au serveur. Dans le même esprit, **aucun utilisateur ne doit avoir un accès root** au serveur, et l'utilisateur système qui exécute PHP pour Grav doit être un utilisateur distinct auquel seul le système a accès.

Compte tenu de l'importance du serveur pour votre site Web ou votre service, il est primordial de veiller à le protéger, ainsi que son contenu.

