<h1 class="rem">Éditeur (Sécurité)</h1>

<div class="box">
Onglet Sécurité > Accès à la page
</div>

Cette section définit l'accès frontal à la page.

| **OPTION**                        | **DESCRIPTION**
| ---------                         | ---------
| Menu Visibility Requires Access   | Réglez sur Oui si la page doit être affichée dans les <br>menus uniquement si l'utilisateur peut y accéder.
| Page Access                       | L'utilisateur avec les autorisations d'accès suivantes peut <br>accéder à la page.

<div class="box">
Onglet Sécurité > Autorisations de page
</div>

Cette section définit l'accès administratif à la page.

CRUD ACL spécifique à la page fonctionne en utilisant uniquement des groupes d'utilisateurs. De plus, il a deux groupes spéciaux nommés `autors` et `defaults`, qui donnent un accès spécial aux propriétaires de pages, et un repli par défaut à tous les utilisateurs connectés.

| **OPTION**                        | **DESCRIPTION**
| ---------             | ---------
| Inherit Permissions   | Hériter l'ACL de la page parent.
| Page Authors          | Les membres des auteurs de la page ont un accès de niveau propriétaire <br>à cette page définie dans le groupe de pages spécial « Auteurs ».
| Page Groups           | Les membres des groupes de pages ont un accès spécial à cette page.

Pour plus d'informations sur le fonctionnement des autorisations CRUD, veuillez consulter les [autorisations de page](dashboard-pages-permissions.md).

