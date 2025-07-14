<h1 class="rem">pages</h1>

![Pages d'administration 1](https://learn.getgrav.org/user/pages/05.admin-panel/03.page/pages.png)

La page **Pages** vous donne un accès éditorial rapide au contenu de votre site. C'est ici que vous pouvez accéder à l'éditeur d'une page, supprimer des pages, créer de nouvelles pages et savoir si une page est visible en un coup d'œil.

<div class="notice info">
L'accès à la fonctionnalité Pages nécessite une autorisation <code>access.admin.super</code> ou <code>access.admin.pages.list</code>, voir <a href="../compte-utilisateur">Comptes d'utilisateurs</a> et <a href="../groupe-utilisateur">Groupes d'utilisateurs</a>.
</div>

Si vous créez ou modifiez fréquemment du contenu sur votre site, cette zone de l'admin vous deviendra très familière.

<h2 id="Ajout de nouvelles pages">Ajout de nouvelles pages
<a href="#Ajout de nouvelles pages" class="toc-anchor after"></a></h2>

![Pages d'administration 2](https://learn.getgrav.org/user/pages/05.admin-panel/03.page/add.png)

Trois boutons bordent le haut du panneau d'administration des **Pages**. Le bouton **Back** vous renvoie au **tableau de bord**, tandis que les boutons **Add Page** et **Add Modular** lancent la création de nouvelles pages pour votre site.

Ci-dessous, nous décomposons les options disponibles lorsque vous sélectionnez ces boutons.

<h2 id="Ajouter une page">Ajouter une page
<a href="#Ajouter une page" class="toc-anchor after"></a></h2>

![Pages d'administration 3](https://learn.getgrav.org/user/pages/05.admin-panel/03.page/add2.png)

Le bouton **Add Page** crée une page non modulaire pour votre site. Une fois sélectionné, une fenêtre contextuelle apparaîtra vous permettant de saisir un **Title** et un **Folder Name**, d'attribuer une **Parent Page** et un **Display Template**, ainsi que de définir si la page doit être visible ou masquée.

| **OPTION**         | **DESCRIPTION**
| ---------          | ---------
| Page Title         | C'est ici que vous entrez le titre de la page que vous créez.
| Folder Name        | Vous pouvez définir un nom de dossier personnalisé pour la page ou conserver celui <br>généré automatiquement en fonction du titre.
| Parent Page        | Ceci définit la page parent de la nouvelle page. Peut être un enfant d'une autre page <br>(comme la page d'accueil ou le blog) ou défini à la racine de votre site. En définissant <br>la valeur de l'option `child_type` dans le frontmatter d'une page parent, un `Display Template` par défaut sera automatiquement sélectionné.
| Display Template   | Vous pouvez choisir le modèle fourni par le thème que vous souhaitez appliquer <br>à la page.
| Visible            | Définit si vous voulez ou non que la page soit visible dans la navigation. Peut être <br>réglé sur Auto pour que cela soit déterminé pour vous. Dans le réglage automatique, <br>s'il y a une autre page sœur qui utilise un préfixe numérique, elle en utilise un et <br>est donc visible. Sinon, il ne l'affiche pas.

Une fois que vous avez rempli ces informations, sélectionnez **Continue** pour accéder à l'éditeur de la nouvelle page. Nous couvrirons l'éditeur de page plus en détail dans [le guide suivant](dashboard-pages-editeur-contenu.md).

<div class="notice info">
Le fait qu'une page soit visible ou non dans ces paramètres n'a d'effet que sur la navigation. La capacité d'une page à être visitée par un navigateur est déterminée par les paramètres de <a href="../en-tete-frontmatter#Published">publication de la page</a>.
</div>

<h2 id="Ajouter une page modulaire">Ajouter une page modulaire
<a href="#Ajouter une page modulaire" class="toc-anchor after"></a></h2>

![Pages d'administration 4](https://learn.getgrav.org/user/pages/05.admin-panel/03.page/add3.png)

Le deuxième bouton en haut de la zone **Page**s de l'administration vous permet d'ajouter une sous-page modulaire à votre site. Les pages modulaires sont différentes des pages ordinaires car elles sont en fait une collection de pages, organisées et affichées sur une seule page. Ce bouton permet notamment de créer des sous-pages et de les affecter à une page modulaire parente.

Voici une répartition des champs et des options qui apparaissent dans la fenêtre contextuelle du bouton **Add Modular Page**.

| **Option**         | **Description**
| ---------          | ---------
| Page Title         | Définit un titre pour la page modulaire.
| Folder Name        | Vous pouvez définir un nom de dossier personnalisé pour la page ou conserver celui généré automatiquement en fonction du titre.
| Page               | Définit la page parente de la nouvelle sous-page modulaire. Il s'agit de la page sur laquelle le contenu de votre nouvelle page modulaire apparaîtra.
| Modular Template   | Affiche une liste de modèles fournis par le thème pour les pages modulaires parmi lesquelles vous pouvez choisir pour la nouvelle page.

Une fois que vous avez rempli ces informations, sélectionnez **Continue** pour accéder à l'éditeur de la nouvelle page. Nous couvrirons l'éditeur de page plus en détail dans [le guide suivant](dashboard-pages-editeur-contenu.md).

<h2 id="Liste des pagesr">Liste des pages
<a href="#Liste des pages" class="toc-anchor after"></a></h2>

![Pages d'administration 5](https://learn.getgrav.org/user/pages/05.admin-panel/03.page/pages2.png)

La liste des pages qui apparaît dans cette zone vous donne un accès rapide à toutes vos pages actuelles, ainsi qu'une méthode en un coup d'œil pour voir si les pages sont visibles ou non.

La sélection du titre de n'importe quelle page vous amènera directement à l'éditeur de cette page. La grande icône **X** à droite de chaque page vous permet de supprimer la page.

Si vous survolez l'icône directement à gauche d'une page, elle vous indiquera son statut actuel. Par exemple, il peut indiquer **Page • Routable • Visible** si une page est routable (visible via l'URL) et visible (apparaît dans les menus de navigation).

Vous pouvez **filtrer** et **rechercher** vos pages pour trouver facilement la page exacte que vous recherchez. Par exemple, à l'aide de l'option **Add Filters**, vous pouvez filtrer les pages par type afin que seules les pages **modulaires**, **visibles** et/ou **routables** apparaissent dans la liste.

Si vous avez un titre de page spécifique (ou une partie de titre) en tête, vous pouvez utiliser la barre de recherche pour trouver rapidement la page spécifique que vous recherchez.

