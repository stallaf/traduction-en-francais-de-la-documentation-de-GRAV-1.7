
display: block;
border-style: none;
text-align: center;
background: white;
max-width: 100%;
margin: 3rem auto;
padding: 3px;
text-align: center;
}


<h1 class="rem">Autorisations de page</h1>

<div class="box">
Autorisations des pages
</div>

Les autorisations d'utilisateur/de groupe pour les pages sont :

| **OPTION**            | **VALEUR**            | **DESCRIPTION**
| ---------             | ---------             | ----------
| **Configuration**     | *admin.configuration* | Donne à l'utilisateur l'accès à la zone <br>de **configuration** de l'administration.
    | ⤿ **Pages Configuration** | *admin.configuration.pages* | Donne à l'utilisateur l'accès à <br>la **configuration des pages** trouvée <br>dans la zone **Pages** de l'administration.
| **Pages**             | *admin.pages*         | Donne à l'utilisateur un accès complet <br>à la zone **Pages** de l'administration.
    | ⤿ ** Create**     | *admin.pages.create*  | Donne à l'utilisateur l'accès **Créer** des <br>pages.
    | ⤿ **Read**        | *admin.pages.read*    | Donne à l'utilisateur l'accès **Lire** <br>aux pages.
    | ⤿ **Update**      | *admin.pages.update*  | Donne à l'utilisateur l'accès **Mise à <br>jour** des pages.
    | ⤿ **Delete**      | *admin.pages.delete*  | Donne à l'utilisateur l'accès <br>**Supprimer**aux pages.
    | ⤿ **List**        | *admin.pages.list*    | Donne à l'utilisateur l'accès à la <br>zone Pages de l'administration.

<div class="notice info">
<strong>AVERTISSEMENT</strong> : Toutes les actions dans Grav ne sont vérifiées que par rapport à un seul type d'autorisation. Si vous empêchez l'utilisateur de répertorier ou de lire des pages dans l'administration, mais autorisez toujours les utilisateurs à créer, mettre à jour et supprimer, ils peuvent effectuer ces actions. Cela signifie que même si les utilisateurs ne peuvent pas voir les <code>Pages</code> dans l'administration, ils peuvent visiter directement la page d'édition et effectuer ces actions à partir de là.
</div>

<div class="notice tip">
<strong>ASTUCE</strong> : À partir de Grav 1.7, vous pouvez et devez restreindre l'accès <strong>CRUD</strong> pour les pages individuelles et leurs enfants directement à partir des pages elles-mêmes.
</div>

Les valeurs possibles pour les autorisations sont :

| **OPTION**            | **VALEUR**            | **DESCRIPTION**
| ---------          | ---------       | ----------
| **Allowed**        | `true`          | **Autorise** l'exécution d'une action s'il n'y a pas d'autorisation **Refusée** <br>au même niveau.
| **Denied**         | `false`         | **Empêche** l'exécution de l'action. Si l'utilisateur a défini à la fois <br>**Autorisé** et **Refusé**, l'autorisation **Refusée** l'emporte.|
| **Not set**        | `null`          | Aucun effet, mais agit comme **Refusée** si aucune autre règle ne <br>s'applique.

Les autorisations définies spécifiquement pour le compte d'utilisateur ont priorité sur les autorisations de groupe. Si l'autorisation n'a pas été définie dans le compte d'utilisateur, la vérification d'accès sera effectuée pour tous les groupes d'utilisateurs auxquels l'utilisateur appartient. Si l'un des groupes d'utilisateurs a **refusé** l'action, l'utilisateur n'a aucune autorisation pour l'action. Sinon, si l'un des groupes d'utilisateurs a **autorisé** l'action, l'autorisation sera accordée. Si l'autorisation n'a été définie dans aucun des groupes de l'utilisateur, l'autorisation de **super utilisateur** agit comme universellement autorisée, sinon **refusée** sera appliquée.

Les autorisations définies pour les comptes d'utilisateurs et les groupes d'utilisateurs agissent comme des autorisations par défaut pour la gestion des pages. Toutes ces règles peuvent être remplacées dans l'onglet [Sécurité](dashboard-pages-editeur-securite.md) de n'importe quelle page.

<h2 id="Flux de travail de vérification de l'accès CRUD à la page">Flux de travail de vérification de l'accès CRUD à la page
<a href="#Flux de travail de vérification de l'accès CRUD à la page" class="toc-anchor after"></a></h2>

Le flux de travail de vérification d'autorisation CRUD pour une page individuelle est le suivant :

1. Définir l'**action** = `créer`, `lire`, `mettre à jour`, `supprimer` ou `lister`
2. Parcourir tous les `groupes de pages` à partir de la page actuelle
    * **match** au groupe d'`auteurs` spéciaux si l'utilisateur est répertorié dans les `auteurs de la page`
    * **match** au `groupe` spécial par défaut si l'utilisateur est connecté
    * **match** au groupe si l'utilisateur a également le groupe
    * si **match**
        * si l'**action** autorise le retour de `Refuser` : s'arrête immédiatement et _renvoie_ `false`
        * si l'action autorise le retour de `Autoriser` : définir le **drapeau d'autorisation** = `true`
    * passer au groupe suivant
3. Après avoir parcouru tous les groupes, vérifiez si l'utilisateur a l'autorisation pour l'action
    * si le **drapeau d'autorisation** est `true` : *renvoie* `true`
4. Vérifiez les autorisations d'administration globale des pages de l'utilisateur (une seule fois)
    * si l'**action** autorise le retour de `Refuser` : *renvoie* `false`
    * si l'**action** autorise le retour de  `Autoriser` : *renvoie* `true`
5. Vérifier si la page hérite des autorisations parentales
    * si `Inherit Permissions` = `Yes`, faire les mêmes vérifications avec la page parent
    * sinon *retourne* `null`

<h2 id="Page racine">Page racine
<a href="#Page racine" class="toc-anchor after"></a></h2>

<div class="box">
Page racine
</div>

La page racine est une page spéciale dans Grav 1.7+ qui permet aux administrateurs du site de définir des autorisations par défaut pour toutes les pages. Il ne peut être vu que par un **super utilisateur** ou un utilisateur disposant des droits de **configuration des pages**.

La page racine sera enregistrée dans le fichier `user/pages/root.md` et ne contient aucun contenu car la page est actuellement inaccessible (cela peut changer à l'avenir).

