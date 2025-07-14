<h1 class="rem">Introduction</h1>

Le plug-in du **Panneau d'Administration** pour [Grav](https://github.com/getgrav/grav) est une interface graphique Web (interface utilisateur graphique) qui fournit un moyen pratique de configurer Grav et de créer et de modifier facilement des pages. Cela restera un plugin totalement facultatif, et n'est en aucun cas requis ou nécessaire pour utiliser Grav efficacement. En fait, l'interface d'administration offre une vue volontairement limitée pour s'assurer qu'elle reste facile à utiliser et non écrasante. Les utilisateurs expérimentés préféreront toujours travailler directement avec les fichiers de configuration.

![acceuil-admin](https://learn.getgrav.org/user/pages/05.admin-panel/01.introduction/admin-dashboard.png)

<h2 id="Caractéristiques">Caractéristiques
<a href="#Caractéristiques" class="toc-anchor after"></a></h2> 

* Connexion utilisateur avec hachage automatique du mot de passe
* Fonctionnalité mot de passe oublié
* Gestion des utilisateurs connectés
* Mises à jour du noyau Grav en un clic
* Tableau de bord avec l'état de la maintenance, l'activité du site et les dernières mises à jour des pages
* Capacité de sauvegarde alimentée par Ajax
* Capacité d'effacement du cache alimentée par Ajax
* Gestion de la configuration du système
* Gestion de la configuration du site
* Modes Normal et Expert qui permettent l'édition via des formulaires ou YAML
* Liste des pages avec filtrage et recherche
* Création, modification, déplacement, copie et suppression de pages
* Puissant éditeur de code de mise en évidence de la syntaxe avec aperçu instantané alimenté par Grav
* Fonctionnalités de l'éditeur, touches de raccourci, barre d'outils et mode plein écran sans distraction
* Téléchargement par glisser-déposer des fichiers multimédias de la page, y compris le placement par glisser-déposer dans l'éditeur
* Mises à jour du thème et du plugin en un clic
* Gestionnaire de plugins qui permet de lister et de configurer les plugins installés
* Gestionnaire de thèmes qui permet de lister et de configurer les thèmes installés
* Installation alimentée par GPM de nouveaux plugins et thèmes
* ACL pour l'accès des utilisateurs administrateurs aux fonctionnalités

<h2 id="Support">Support
<a href="#Support" class="toc-anchor after"></a></h2> 

Le panneau d'administration est un plugin assez ambitieux avec de nombreuses fonctionnalités qui vous donneront beaucoup de puissance et de flexibilité lors de la création de vos sites Grav. Donc, si vous avez des questions, des problèmes, des suggestions ou si vous trouvez l'un de ces bogues rares, veuillez utiliser l'un des moyens suivants pour obtenir de l'aide de notre part.

Pour le **chat en direct**, veuillez utiliser le [Serveur Discord Chat](https://chat.getgrav.org/) pour les discussions liées au plugin d'administration.

Pour les **bogues, les fonctionnalités, les améliorations**, assurez-vous de [créer des signalements dans le référentiel GitHub du plugin d'administration](https://github.com/getgrav/grav-plugin-admin).

<h2 id="Installation">Installation
<a href="#Installation" class="toc-anchor after"></a></h2> 

Assurez-vous d'abord que vous utilisez la dernière version de Grav, **1.7.28 ou supérieur**. Ceci est nécessaire pour que le plugin d'administration fonctionne correctement. Recherchez et mettez à niveau vers les nouvelles versions de Grav comme celle-ci (`-f` force une actualisation de l'index GPM) :

```bash
$ | bin/gpm version -f
$ | bin/gpm selfupgrade
```

Le plugin d'administration nécessite en fait l'aide de 3 autres plugins, donc pour que le plugin **admin** fonctionne, vous devez d'abord installer les plugins **login**, **forms** et **email**. Ceux-ci sont disponibles via GPM, et parce que le plugin a des dépendances, il vous suffit de continuer et d'installer le plugin d'administration, et d'accepter lorsque vous êtes invité à installer les autres :

```bash
$ | bin/gpm install admin
```

Vous pouvez également installer le plug-in manuellement si vous ne parvenez pas à utiliser GPM sur votre système.

<h2 id="Création d'un utilisateur">Création d'un utilisateur
<a href="#Création d'un utilisateur" class="toc-anchor after"></a></h2> 

Avec la dernière version de l'administrateur, vous serez invité à créer un compte d'utilisateur administrateur lorsque vous pointez votre navigateur sur votre site. Vous devez terminer cette étape pour vous assurer immédiatement qu'un utilisateur administrateur valide est sous votre contrôle.

![installation plugin admin](https://learn.getgrav.org/user/pages/05.admin-panel/01.introduction/new-user.png)

Remplissez simplement le formulaire et cliquez sur le bouton`Create user`.

Les informations utilisateur sont stockées dans le dossier `user/accounts/` de votre installation Grav. Vous pouvez modifier les valeurs manuellement ou via le plugin Admin lui-même. Vous pouvez également créer de nouveaux utilisateurs manuellement ou via la commande CLI `bin/plugin login newuser`. Plus d'informations sont contenues dans la FAQ Admin.

<h2 id="Complexité du nom d'utilisateur et du mot de passe">Complexité du nom d'utilisateur et du mot de passe
<a href="#Complexité du nom d'utilisateur et du mot de passe" class="toc-anchor after"></a></h2> 

Les modèles Regex pour les noms d'utilisateur et les mots de passe sont définis dans `system/config/system.yaml`.

Le modèle par défaut pour les utilisateurs (`system.username_regex`) ne comprend que des caractères minuscules, des chiffres, des tirets et des traits de soulignement. Les noms d'utilisateur doivent comporter entre 3 et 16 caractères.

Le modèle par défaut pour les mots de passe (`system.pwd_regex`) est un minimum de huit (8) caractères, avec au moins un chiffre, une majuscule et une lettre minuscule.

<h2 id="Usage">Usage
<a href="#Usage" class="toc-anchor after"></a></h2> 

Par défaut, vous pouvez accéder à l'administration en pointant votre navigateur sur `http://votresite.com/admin`. Vous pouvez simplement vous connecter avec `username` et le `password` définis dans le fichier YAML que vous avez configuré précédemment.

Une fois connecté, votre **mot de passe en clair** sera supprimé et remplacé par un mot de passe **crypté**.
   
