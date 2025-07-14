<h1 class="rem">Crochets d'événements d'administration</h1>

Le plugin Admin a plusieurs crochets d'événement qui peuvent être utilisés pendant le [cycle de vie Grav](cycle-vie.md). Voir la documentation générale du plugin pour l'utilisation des hooks d'événement dans le chapitre Plugins.

<h2 id="Crochets d'événement d'administration disponibles">Crochets d'événement d'administration disponibles
<a href="#Crochets d'événement d'administration disponibles" class="toc-anchor after"></a></h2>

* [onAdminTaskExecute](#onAdminTaskExecute)
* [onAdminCreatePageFrontmatter](#onAdminCreatePageFrontmatter)
* [onAdminSave](#onAdminSave)
* [onAdminAfterSave](#onAdminAfterSave)
* [onAdminAfterSaveAs](#onAdminAfterSaveAs)
* [onAdminAfterDelete](#onAdminAfterDelete)
* [onAdminAfterAddMedia](#onAdminAfterAddMedia)
* [onAdminAfterDelMedia](#onAdminAfterDelMedia)

<h2 id="Activation d'un hook d'événement d'administration">Activation d'un hook d'événement d'administration
<a href="#Activation d'un hook d'événement d'administration" class="toc-anchor after"></a></h2>

Les hooks d'événement d'administration sont appelés de la [même manière](plugin-tutoriel.md) que les hooks d'événement de base.

<h3 id="onAdminTaskExecute">onAdminTaskExecute
<a href="#onAdminTaskExecute" class="toc-anchor after"></a></h3>

Le plug-in Admin déclenche diverses tâches, en fonction de l'interaction de l'utilisateur. Les tâches peuvent inclure la déconnexion, la connexion, l'enregistrement, 2faverify, etc. Une fois la tâche terminée, ce crochet d'événement se déclenche.

<h3 id="onAdminCreatePageFrontmatter">onAdminCreatePageFrontmatter
<a href="#onAdminCreatePageFrontmatter" class="toc-anchor after"></a></h3>

Lors de la création d'une nouvelle page, cet événement est déclenché après que les données d'en-tête ont été initialement définies pour permettre aux plugins de manipuler par programme le frontmatter.

<h3 id="onAdminSave">onAdminSave
<a href="#onAdminSave" class="toc-anchor after"></a></h3>

Utilisez l'événement d'administration `onAdminSave()` pour manipuler les données d'objet de la page `$object` avant qu'elles ne soient enregistrées dans le système de fichiers.

<h3 id="onAdminAfterSave">onAdminAfterSave
<a href="#onAdminAfterSave" class="toc-anchor after"></a></h3>

Après avoir enregistré la page dans le panneau d'administration, cet événement est déclenché.

<h3 id="onAdminAfterSaveAs">onAdminAfterSaveAs
<a href="#onAdminAfterSaveAs" class="toc-anchor after"></a></h3>

Lors de la création d'un dossier via le panneau, cet événement se déclenche immédiatement après la création du nouveau dossier et l'exécution d'un effacement standard du cache.

<h3 id="onAdminAfterDelete">onAdminAfterDelete
<a href="#onAdminAfterDelete" class="toc-anchor after"></a></h3>

Se déclenche après la suppression d'une page ou d'un dossier. Il est immédiatement suivi d'un effacement de cache standard.

<h3 id="onAdminAfterAddMedia">onAdminAfterAddMedia
<a href="#onAdminAfterAddMedia" class="toc-anchor after"></a></h3>

Se déclenche après la fin d'une tâche d'ajout de média, mais avant l'affichage du message de confirmation.

<h3 id="onAdminAfterDelMedia">onAdminAfterDelMedia
<a href="#onAdminAfterDelMedia" class="toc-anchor after"></a></h3>

Se déclenche après la fin d'une tâche de suppression de média, mais avant l'affichage du message de confirmation.

