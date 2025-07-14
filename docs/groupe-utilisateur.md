<h1 class="rem">Groupes d'utilisateurs</h1>

<div class="box">Liste de groupe</div>

Les **User Groups** définissent des rôles communs pour les utilisateurs. C'est le moyen préféré de définir des autorisations pour les utilisateurs, car la gestion des rôles est plus facile que de modifier les règles individuellement pour chaque **user account**.

Après avoir créé un groupe d'utilisateurs, vous pouvez l'affecter à des comptes d'utilisateurs à partir de [l'onglet Accès](compte-utilisateur.md#Onglet Accès).

<h2 id="Groupe d'utilisateurs">Groupe d'utilisateurs
<a href="#Groupe d'utilisateurs" class="toc-anchor after"></a></h2>

<div class="box">Modifier le groupe</div>

| **OPTION**         | **DESCRIPTION**
| ---------          | ---------
| **Group Name**     | Le nom du groupe est l'identifiant du groupe. Il ne peut pas être modifié après la <br>création du groupe.
| **Display Name**   | Le nom d'affichage est le nom visible du groupe.
| **Description**
| **Icon**
| **Enabled**        | Si défini sur **Yes**, le groupe a été activé sur votre site. Si **No**, les autorisations définies <br>par le groupe ne s'appliquent pas.
| **Permissions**    | Liste de toutes les autorisations de votre site. [Voir ci-dessous](#Autorisations).

<h3 id="Autorisations">Autorisations
<a href="#Autorisations" class="toc-anchor after"></a></h3>

Les administrateurs trouveront la zone des autorisations particulièrement utile. C'est ici que vous pouvez configurer exactement ce à quoi un utilisateur pourra accéder et faire au sein de l'administrateur.

Voici une ventilation rapide des options d'autorisations et de ce qu'elles permettent à quelqu'un de faire.

<h4 id="Site">Site
<a href="#Site" class="toc-anchor after"></a></h4>

<div class="box">Autorisations du site</div>

| **OPTION**        | **VALEUR**       | **DESCRIPTION**
| ---------         | ---------        | --------
| **Login to Site** | *site.login*     | Permet à l'utilisateur de se connecter au frontal.

<h4 id="Administrateur">Administrateur
<a href="#Administrateur" class="toc-anchor after"></a></h4>

<div class="box">Autorisations d'administration</div>

| **OPTION**            | **VALEUR**             | **DESCRIPTION**
| ---------             | ---------             | --------
| **Login to Admin**    | *admin.login*         | Permet à l'utilisateur de se connecter à l'administrateur. Il doit être défini sur **Yes** pour permettre à l'utilisateur de se connecter.
| **Super User**        | *admin.super*         | Désigne l'utilisateur en tant que super administrateur, lui donnant la possibilité de voir et de configurer toutes les zones du site.
| **Clear Cach**        | *admin.cache*         | Permet à l'utilisateur d'accéder aux boutons de réinitialisation du cache.
| **Configuration**     | *admin.configuration* | Donne à l'utilisateur l'accès à la zone de **Configuration** de l'administration.     
    | ⤿ **Manage System Configuration** | *admin.configuration.system*  | Donne à l'utilisateur l'accès à l'onglet **Systeme** dans la zone **Configuration** de l'administration.
    | ⤿ **Manage Site Configuration**   | *admin.configuration.site*    | Donne à l'utilisateur l'accès à l'onglet **Sit** dans la zone **Configuration** de l'administration.
    | ⤿ **Manage Media Configuration**  | a*dmin.configuration.media*   | Donne à l'utilisateur l'accès à l'onglet *Media* dans la zone **Configuration** de l'administration.
    |⤿ **See Server Information**       | *admin.configuration.info*    | Donne à l'utilisateur l'accès à l'onglet **Info** dans la zone **Configuration** de l'administration.
    | ⤿ **Pages Configuration**         | *dmin.configuration.pages*    | Donne à l'utilisateur l'accès à la **configuration des pages** trouvée dans la zone Pages de l'administration.
    | ⤿ **Accounts Configuration**      | *admin.configuration.accounts* | Donne à l'utilisateur l'accès à la **configuration des comptes* qui se trouvent dans la zone Comptes de l'administration.
| **Pages**             | *admin.pages*      | Donne à l'utilisateur un accès complet à la zone Pages de l'administration.
| **Site Maintenance**  | *admin.maintenance* | Permet à l'utilisateur d'accéder à la zone **Maintenance** du **tableau de bord**.
| **Site Statistics**   | *admin.statistics* | Permet à l'utilisateur d'accéder à la zone **Statistiques** du **tableau de bord**.
| **Manage Plugins**    | *admin.plugins*    | Donne à l'utilisateur l'accès à la zone **Plugins** de l'administration.
| **Manage Themes**     | *admin.themes*     | Donne à l'utilisateur l'accès à la zone **Thèmes** de l'administration.
| **Access to Tools**   | *admin.tools*      | Accès aux outils d'administration.
| **User Accounts**     | *admin.accounts*   | Donne à l'utilisateur un accès complet à la zone Comptes de l'administration.


