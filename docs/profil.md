<h1 clas="rem">Profil</h1>

![Profil administrateur 1](https://learn.getgrav.org/user/pages/05.admin-panel/02.dashboard/03.profile/grav-profile.png)

La page de profil dans l'administrateur vous permet d'afficher et de mettre à jour les paramètres de votre profil individuel. C'est là que votre avatar, votre adresse e-mail, votre nom, votre langue, etc. sont définis. Pour les administrateurs, c'est également là que vous pouvez ajuster les groupes et les niveaux d'autorisation pour les utilisateurs individuels.

L'accès à la page de profil est simple. Une fois connecté à l'administrateur, vous pouvez accéder à votre profil en sélectionnant la zone de la barre latérale avec votre image d'avatar et votre nom. Cela vous mènera directement à votre propre profil.

De plus, les administrateurs apprécieront la facilité d'accéder à la page de profil d'un autre utilisateur en ajoutant `admin/user/example` à l'URL de leur site. Remplacement de `example` par le nom d'utilisateur de l'utilisateur pour lequel il souhaite modifier les informations de profil et/ou les autorisations.

<h2 id="Photo de profil">Photo de profil
<a href="#Photo de profil" class="toc-anchor after"></a></h2>

![Profil administrateur 2](https://learn.getgrav.org/user/pages/05.admin-panel/02.dashboard/03.profile/grav-profile2.png)

La zone **Profil** de l'administrateur vous donne un aperçu rapide et stylé de votre avatar, de votre nom et de votre titre. Votre avatar est automatiquement généré via [Gravatar](https://fr.gravatar.com/), un service d'avatar mondial qui vous permet de télécharger une seule image de profil et de l'utiliser sur plusieurs sites et services.

![Profil administrateur 3](https://learn.getgrav.org/user/pages/05.admin-panel/02.dashboard/03.profile/grav-profile2b.png)

Si vous n'avez pas d'image téléchargée sur Gravatar, ou si vous préférez utiliser une image de votre choix, vous pouvez télécharger une image ici en faisant glisser et en déposant le fichier image dans la section **Déposez vos fichiers ici ou cliquez sur cette zone** de la page. Vous pouvez également cliquer sur la zone pour afficher un sélecteur de fichier qui vous permettra de rechercher, sélectionner et télécharger un fichier image à partir de votre système.

Une fois que vous avez téléchargé une nouvelle image, sélectionnez simplement le bouton **Save** dans le coin supérieur droit de la page.

<h2 id="Compte">Compte
<a href="#Compte" class="toc-anchor after"></a></h2>

![Profil administrateur 4](https://learn.getgrav.org/user/pages/05.admin-panel/02.dashboard/03.profile/grav-profile3.png)

La section **Compte** de la page de profil est l'endroit où vous pouvez mettre à jour vos coordonnées, votre nom, votre langue, etc. Vous ne pouvez pas modifier votre **nom d'utilisateur** ici, car il est directement lié à l'endroit où vos informations d'utilisateur sont stockées, mais vous pouvez modifier tout ce dont vous avez besoin.

<h2 id="Authentification à 2 facteurs">Authentification à 2 facteurs
<a href="#Authentification à 2 facteurs" class="toc-anchor after"></a></h2>

![Profil administrateur 5](https://learn.getgrav.org/user/pages/05.admin-panel/02.dashboard/03.profile/grav-profile5.png)

**L'authentification à 2 facteurs** fournit une couche de sécurité supplémentaire pour votre site Web. En savoir plus sur cette fonctionnalité dans la section Sécurité de ce guide.

<h2 id="Niveaux d'accès">Niveaux d'accès
<a href="#Niveaux d'accès" class="toc-anchor after"></a></h2>

![Profil administrateur 6](https://learn.getgrav.org/user/pages/05.admin-panel/02.dashboard/03.profile/grav-profile4.png)

Les administrateurs trouveront la zone des autorisations particulièrement utile. C'est ici que vous pouvez configurer exactement ce à quoi un utilisateur pourra accéder et faire au sein de l'administrateur.

Voici une ventilation rapide des options d'autorisations et de ce qu'elles permettent à quelqu'un de faire.

| **OPTION**            | **DESCRIPTION**
|---------              | ---------
| **admin.super**       | Désigne l'utilisateur en tant que super administrateur, lui donnant la <br>possibilité de voir et de configurer toutes les zones du site.
| **admin.login**       | Permet à l'utilisateur de se connecter à l'administrateur. Il doit être <br>défini sur **Yes** pour permettre à l'utilisateur de se connecter.
| **admin.cache**       | Donne à l'utilisateur l'accès aux boutons de réinitialisation du cache.
| **admin.configuration** | Donne à l'utilisateur l'accès à la zone de **configuration** de <br>l'administrateur. Cela n'inclut aucun onglet ou sous-section.
| **admin.configuration_system** | Donne à l'utilisateur l'accès à l'onglet **System** dans la zone **Configuration** <br>de l'administrateur.
| **admin.configuration_site**   | Donne à l'utilisateur l'accès à l'onglert **Site** dans la zone **Configuration** <br>de l'administrateur.
| **admin.configuration_media**  | Donne à l'utilisateur l'accès à l'onglet **Media** dans la zone **Configuration** <br>de l'administrateur.
| **admin.configuration_info**   | Donne à l'utilisateur l'accès à l'onglet **Info** dans la zone **Configuration** <br>de l'administrateur.
| **admin.pages**       | Donne à l'utilisateur l'accès à la zone **Pages** de l'administrateur.
| **admin.maintenance** | Permet à l'utilisateur d'accéder à la zone **Maintenance** du **Dashboard**.
| **admin.statistics**  | Donne à l'utilisateur la possibilité d'accéder à la zone **Statistics** du <br>**Dashboard**.
| **admin.plugins**     | Donne à l'utilisateur l'accès à la zone **Plugins** de l'administrateur.
| **admin.themes**      | Donne à l'utilisateur l'accès à la zone **Themes** de l'administrateur.
| **admin.users**       | Permet à l'utilisateur d'accéder et de modifier les informations de profil <br>d'autres utilisateurs. Cela n'inclut pas les autorisations.
| **site.login**        | Permet à l'utilisateur de se connecter au frontal.

