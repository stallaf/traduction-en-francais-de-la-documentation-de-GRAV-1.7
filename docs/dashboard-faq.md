<h1 class="rem">FAQ</h1>

Cette FAQ est destinée à fournir des didacticiels, des conseils et des astuces utiles pour vous aider à tirer le meilleur parti du plug-in Admin.

<h2 id="Installation manuelle de l'administration">Installation manuelle de l'administration
<a href="#Installation manuelle de l'administration" class="toc-anchor after"></a></h2>

L'installation manuelle n'est pas la méthode d'installation recommandée, cependant, il est toujours possible d'installer le plugin d'administration manuellement. Fondamentalement, vous devez télécharger chacun des plugins suivants individuellement :

* [admin](https://github.com/getgrav/grav-plugin-admin/archive/master.zip)
* [login](https://github.com/getgrav/grav-plugin-login/archive/master.zip)
* [form](https://github.com/getgrav/grav-plugin-form/archive/master.zip)
* [e-mail](https://github.com/getgrav/grav-plugin-email/archive/master.zip)

Extrayez chaque fichier d'archive dans votre dossier `user/plugins`, puis assurez-vous que les dossiers sont renommés en `admin/`, `login/`, `form/` et `email/`. Suivez ensuite les **instructions d'utilisation ci-dessous**.

<h2 id="Ajouter et gérer des utilisateurs">Ajouter et gérer des utilisateurs
<a href="#Ajouter et gérer des utilisateurs" class="toc-anchor after"></a></h2>

Lorsque vous installez le plugin Admin pour la première fois, vous serez invité à créer un utilisateur administrateur lorsque vous pointez votre navigateur sur votre site. Il s'agit d'un simple formulaire Web qui garantit qu'au moins un utilisateur administrateur a été créé.

![Nouvel utilisateur 1](https://learn.getgrav.org/user/pages/05.admin-panel/01.introduction/new-user.png)

Vous pouvez facilement ajouter d'autres utilisateurs, mais cela nécessite un peu de travail dans le terminal/l'invite de commande. Depuis votre terminal, accédez à la racine du site Grav auquel vous souhaitez ajouter des utilisateurs et tapez la commande suivante :

    $ | bin/plugin login newuser

![Nouvel utilisateur 2](https://learn.getgrav.org/user/pages/05.admin-panel/09.faq/faq_1.png)

Cela lancera une série d'invites pour vous guider dans la création d'un nouvel utilisateur. Celles-ci incluent la création d'un nom d'utilisateur, d'un mot de passe, d'une adresse e-mail et la définition du niveau d'autorisation du nouvel utilisateur.

<div class="notice warning">
Vous aurez besoin d'un nom d'utilisateur composé de 3 à 16 caractères pouvant inclure des lettres minuscules, des chiffres, des traits de soulignement et des traits d'union. Les majuscules, les espaces et les caractères spéciaux ne sont pas autorisés pour le nom d'utilisateur.
</div>

À un moment du processus de création, il vous sera demandé de choisir un ensemble d'autorisations pour le nouvel utilisateur. Il existe actuellement trois options :

| **RÔLE(S)**           | **DESCRIPTION**
| ---------             | ---------
| Admin                 | Donne à l'utilisateur l'accès au backend Admin. Ce rôle n'inclut pas l'accès <br>frontal aux pages protégées.
| Site                  | Permet à l'utilisateur d'accéder à toutes les pages du front-end. C'est <br>l'équivalent d'un utilisateur connecté.
| Admin and Site        | Permet à l'utilisateur d'accéder à l'intégralité du site, front-end et back-end.

Les données de l'utilisateur sont stockées dans `SITE_ROOT/user/accounts/` et chaque utilisateur reçoit un fichier **YAML** contenant les informations de connexion de cet utilisateur et d'autres détails. Voici un exemple des données contenues dans un fichier de compte utilisateur. Par exemple, cela pourrait être le contenu de `SITE_ROOT/user/accounts/tester.yaml`.

<div class="notice info">
Le nom de fichier <code>tester.yaml</code> indique que le nom d'utilisateur est <code>tester</code>
</div>

```yaml
1  |email: test@rockettheme.com
2  |access:
3  | admin:
4  | login: true
5  | super: true
6  | site:
7  | login: true
8  | fullname: 'Tester McTesting'
9  | title: Admin
10 | hashed_password: $2y$10$5RAUI6ZCISWR.4f0D6FILu3efYq3078ZX/.9vtAnZbjxS/4PXN/WW
```

Vous pouvez modifier ces informations directement dans le fichier YAML de l'utilisateur ou en sélectionnant votre avatar d'utilisateur dans la barre latérale de l'administrateur. Cela vous amènera à une page où vous pourrez facilement gérer les informations de l'utilisateur.

![Nouvel utilisateur 3](https://learn.getgrav.org/user/pages/05.admin-panel/09.faq/faq_2.png)

<div class="notice info">
Les photos d'avatar sont automatiquement générées par <a href="https://gravatar.com">Gravatar</a>, en fonction de l'adresse e-mail de l'utilisateur.
</div>

Pour des raisons de sécurité, les mots de passe des utilisateurs sont stockés sous forme de hachage. Si vous souhaitez modifier votre mot de passe, nous vous recommandons de le faire depuis l'administrateur.

<h2 id="Gestion des ACL">Gestion des ACL
<a href="#Gestion des ACL" class="toc-anchor after"></a></h2>

Chaque fichier yaml utilisateur a une propriété `access`. En définissant cette propriété de manière appropriée, vous pouvez accorder à un utilisateur spécifique l'accès à une partie spécifique de l'administrateur.

Voici les niveaux d'accès actuellement pris en charge expliqués :

* `admin.login` : permet à un utilisateur de se connecter à l'administrateur
* `admin.super` : accorde à un utilisateur des pouvoirs de super administrateur, permettant l'accès à toute l'interface et aux fonctionnalités d'administration en ignorant les autres propriétés d'accès à l'exception de admin.login
* `admin.pages` : permet à un utilisateur d'afficher des pages, de les modifier et d'en ajouter de nouvelles
* `admin.maintenance` : permet à un utilisateur de mettre à jour Grav du côté administrateur, de vérifier les mises à jour et de vider le cache
* `admin.plugins` : permet à un utilisateur d'accéder à la fonctionnalité des plugins, de modifier les paramètres des plugins, de désactiver les plugins ou d'en ajouter de nouveaux
* `admin.themes` : permet à un utilisateur d'accéder à la fonctionnalité des thèmes, de modifier les paramètres du thème, de modifier les thèmes et d'en ajouter de nouveaux
* `admin.statistics` : permet à un utilisateur de voir les statistiques du site
* `admin.cache` : permet à un utilisateur de vider le cache
* `admin.configuration` : permet à un utilisateur d'accéder à la configuration de l'instance. L'autorisation pour les différentes parties doit être donnée séparément via les variables énumérées ci-dessous. Seule l'activation des "sous-variables" sans activer cette variable n'activera pas le menu de configuration pour l'utilisateur.
    * `admin.configuration_system` : permet à un utilisateur de modifier les paramètres système
    * `admin.configuration_site` : permet à un utilisateur de modifier les paramètres du site
    * `admin.configuration_media` : permet à un utilisateur de modifier les types de médias   disponibles
    * `admin.configuration_info` : permet à un utilisateur d'afficher les informations sur cette instance
* les autres niveaux d'accès, qui n'ont pas encore été expliqués sont :
    * `admin.tools`
    * `admin.settings`
    * `admin.users`

<div class="notice info">
Les modifications apportées à un fichier user.yaml alors que cet utilisateur est connecté ne prendront effet qu'après sa déconnexion et sa reconnexion.
</div>

<h2 id="URL d'administration personnalisée">URL d'administration personnalisée
<a href="#URL d'administration personnalisée" class="toc-anchor after"></a></h2>

Une façon d'aider à garder votre panneau d'administration sécurisé est de masquer son emplacement. Pour ce faire, il faudrait changer l'URL de

    http://yourwebsite.com/admin

à quelque chose de plus ambigu qui sera plus difficile à deviner pour quelqu'un. Pour ce faire, vous devez localiser `admin.yaml` qui se trouve dans le dossier `user/plugins/admin/` et le copier dans `user/config/plugins/admin.yaml`.

Changez ensuite la ligne `route: '/admin'` en quelque chose de plus ambigu, par exemple `route: '/myspecialplace`', de cette façon si vous avez besoin d'accéder au panneau d'administration de votre site grav, vous entrerez

    http://yourwebsite.com/myspecialplace

<h2 id="Mode hors-ligne">Mode hors-ligne
<a href="#Mode hors-ligne" class="toc-anchor after"></a></h2>

![Nouvel utilisateur 4](https://learn.getgrav.org/user/pages/05.admin-panel/09.faq/offline.png)

Dans le cas où votre serveur perd sa connexion à Internet, Grav Admin entre automatiquement en mode hors ligne. Ce mode est indiqué par un avis sous le bas dans le coin supérieur gauche de l'administrateur.

En mode hors ligne, l'administrateur ne tentera pas de récupérer les mises à jour du CMS, des plugins ou des thèmes. Cela évite les raccrochages et autres problèmes qui résulteraient autrement de l'impossibilité d'atteindre les serveurs de mise à jour.

Une fois la connexion Internet rétablie, l'avis disparaîtra et le mode hors ligne se terminera automatiquement.

