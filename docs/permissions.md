<h1 class="rem">Autorisations</h1>

<div class="box">Autorisations des comptes</div>

Les autorisations d'utilisateur et de groupe pour la gestion des informations relatives au compte sont :

| **OPTION**         | **VALEUR**               | **DESCRIPTION**
| ---------          | ---------                | ---------
| **Configuration**  | *admin.configuration*    | Donne à l'utilisateur l'accès à la zone de <br>**configuration** de l'administration.
|⤿ **Accounts Configuration** | *admin.configuration.accounts* | Donne à l'utilisateur l'accès à la <br>**configuration des comptes** qui se trouve <br>dans la zone **Comptes** de l'administration.
| **Accounts**       | *admin.accounts*         | Donne à l'utilisateur un accès complet à <br>la zone **Comptes** de l'administration.
|⤿ **Create**        | *admin.accounts.create*  | Donne à l'utilisateur l'accès pour **Créer** <br>des comptes d'utilisateurs et des groupes.
|⤿ **Read**          | *admin.accounts.read*    | Donne à l'utilisateur l'accès pour **Lire** <br>les comptes et groupes d'utilisateurs.
|⤿ **Update**         |*admin.accounts.upodate*  | Donne à l'utilisateur l'accès pour <br>**Mettre à jour** des comptes et des <br>groupes d'utilisateurs.
|⤿ **Delete**         | *admin.accounts.delete*  | Donne à l'utilisateur l'accès pour <br>**Supprimer** des comptes d'utilisateurs <br>et des groupes.
|⤿ **List**          | *admin.accounts.list*    | Donne à l'utilisateur l'accès à la zone <br>*Comptes* de l'administration.

Les valeurs possibles pour les autorisations sont :

| **OPTION**         | **VALEUR**               | **DESCRIPTION**
| ---------          | ---------       | ---------
| **Allowed**        | `true`          | **Autorise** l'exécution d'une action s'il n'y a pas d'autorisation **Refusée** <br>au même niveau.
| **Denied**         |`false`          | **Empêche** l'exécution de l'action. Si l'utilisateur a défini à la fois **Autorisée** <br>et **Refusée**, l'autorisation **Refusée** l'emporte.
| **Not set**        | `null`          | Aucun effet, mais agit comme **Refusé** si aucune autre règle ne s'applique.

Les autorisations définies spécifiquement pour le compte d'utilisateur ont priorité sur les autorisations de groupe. Si l'autorisation n'a pas été définie dans le compte d'utilisateur, la vérification d'accès sera effectuée pour tous les groupes d'utilisateurs auxquels l'utilisateur appartient. Si l'un des groupes d'utilisateurs a **refusé** l'action, l'utilisateur n'a aucune autorisation pour l'action. Sinon, si l'un des groupes d'utilisateurs a **Autorisé** l'action, l'autorisation sera accordée. Si l'autorisation n'a été définie dans aucun des groupes de l'utilisateur, l'autorisation de **super utilisateur** agit comme universellement **autorisée**, sinon **refusée** sera appliquée.

