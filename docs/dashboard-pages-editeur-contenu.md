<h1 class="rem">Éditeur (contenu)</h1>

![Éditeur de page d'administration 1](https://learn.getgrav.org/user/pages/05.admin-panel/03.page/01.editor/page-editor.png)

**L'éditeur de page** dans l'administration est un puissant éditeur de texte et gestionnaire de page qui vous permet de créer le contenu de votre page (y compris les fichiers multimédias), ses options de publication et de taxonomie, ses paramètres, ses remplacements et ses options spécifiques au thème.

Il s'agit essentiellement d'un guichet unique pour gérer une page spécifique.

<div class="notice info">
L'accès à la fonctionnalité Pages nécessite une autorisation <code>access.admin.super</code> ou <code>access.admin.pages.list</code>, voir <a href="../compte-utilisateur">Comptes d'utilisateurs</a> et <a href="../groupe-utilisateur">Groupes d'utilisateurs</a>.
</div>

Les onglets qui apparaissent dans **l'éditeur de page** ne sont pas universels. Il existe un ensemble par défaut de champs de formulaire que l'on trouve couramment dans les thèmes Grav, mais ceux-ci peuvent varier d'un thème à l'autre. L'administration extrait les informations de champ de formulaire d'un certain nombre de sources, notamment le thème et le modèle utilisés pour la page spécifique.

<div class="notice info">
Les onglets et options représentés dans cette documentation sont par défaut. Les développeurs de thèmes ont la possibilité d'ajouter leurs propres options à ces onglets, ou même de supprimer ces onglets et de les remplacer par quelque chose de complètement différent. Nous documentons un scénario de cas courant basé sur le thème Antimatière pour servir d'exemple.
</div>

Dans cette page, nous passerons en revue les caractéristiques et fonctionnalités trouvées dans l'onglet **Content** de **l'éditeur de page**.

<h2 id="Contrôles">Contrôles
<a href="Contrôles" class="toc-anchor after"></a></h2>

![Éditeur de page d'administration 2](https://learn.getgrav.org/user/pages/05.admin-panel/03.page/01.editor/page-editor-1.png)

En haut de la page, vous trouverez les contrôles administratifs qui vous permettent d'enregistrer, de supprimer, de copier et de déplacer votre page. De plus, vous pouvez appuyer sur le bouton **Back** pour revenir à la zone principale des **Pages** de l'administration.

Les boutons **Save** et **Delete** sont assez explicites. Ils enregistrent et suppriment respectivement la page actuellement consultée.

La sélection du bouton **Move** active une fenêtre contextuelle qui vous permet d'attribuer un nouveau parent à la page. Vous avez la même option dans l'onglet **Advanced**.

**Copy** crée une copie de votre page actuelle, en ajoutant un  `-2` (ou un autre préfixe numérique si `-2` est déjà utilisé) à la fin du nom du dossier. Vous pouvez modifier à la fois le nom et le titre du dossier comme bon vous semble.

<h2 id="Titre">Titre
<a href="#Titre" class="toc-anchor after"></a></h2>

![Éditeur de page d'administration 3](https://learn.getgrav.org/user/pages/05.admin-panel/03.page/01.editor/page-editor-2.png)

Le titre d'une page est généralement défini lors de la création de cette page, mais vous pouvez le modifier après coup ici. Notez que changer le titre de la page ici n'aura pas d'impact direct sur le nom du dossier (qui est utilisé à des fins de navigation) mais cela changera ce que les gens voient sur le front-end.

<h2 id="Contenu de l'éditeur de page">Contenu de l'éditeur de page
<a href="#Contenu de l'éditeur de page" class="toc-anchor after"></a></h2>

![Éditeur de page d'administration 4](https://learn.getgrav.org/user/pages/05.admin-panel/03.page/01.editor/page-editor-3.png)

C'est le cœur de l'éditeur de page. C'est là que le corps du contenu de votre page est écrit et modifié. Il comporte de nombreux outils puissants que l'on ne trouve généralement que dans les éditeurs de texte haut de gamme basés sur un navigateur.

Par exemple, vous pouvez basculer entre les vues d'édition et d'aperçu à l'aide des boutons <i class="fa fa-code"></i> et <i class="fa fa-eye"></i> situés dans la zone supérieure droite de l'éditeur.

Étant donné que le contenu de Grav est principalement basé sur markdown, les raccourcis d'édition ajoutent automatiquement des balises markdown à votre contenu. Par exemple, la mise en surbrillance d'un bloc de texte et la sélection de l'icône **B** entourent la zone en surbrillance de balises `**(zone sélectionnée)**` en gras.

Voici une ventilation des outils trouvés dans l'éditeur de contenu :

| **OUTIL**                       | **DESCRIPTION**
| ---------                       | ---------
| <i class="fa fa-bold"></i>      | Ajoute des balises en **gras** à votre contenu.
| <i class="fa fa-italic"></i>    | Ajoute des balises *italiques* à votre contenu.
| <i class="fa fa-strikethrough"></i>   | Ajoute des balises ~~barrées~~ à votre contenu.
| <i class="fa  fa-chain"></i>    | Ajoute des [liens](https://getgrav.org) vers votre contenu.
| <i class="fa fa-image"></i>     | Ajoute des médias à votre contenu.
| <i class="fa fa-quote-right"></i>     | Ajoute des balises de citation à votre contenu.
| <i class="fa fa-list-ul"></i>   | Crée une liste non ordonnée.
| <i class="fa fa-list-ol"></i>   | Crée une liste ordonnée.
| <i class="fa fa-code"></i>      | Active la vue d'édition.
| <i class="fa fa-eye"></i>       | Active l'aperçu du contenu.
| <i class="fa fa-expand"></i>    | Bascule vers une vue d'édition ou d'aperçu pleine page.

<h2 id="Page média">Page média
<a href="#Page média" class="toc-anchor after"></a></h2>

![Éditeur de page d'administration 5](https://learn.getgrav.org/user/pages/05.admin-panel/03.page/01.editor/page-editor-4.png)

La section **Page Media** en bas de l'onglet **Content** concerne les fichiers multimédias de votre page. Ces fichiers existent dans le même dossier que le fichier Markdown de la page. Le téléchargement de nouveaux fichiers multimédias est aussi simple que de **glisser-déposer** un fichier ou de **taper** dans la zone blanche de la section. Cela fera apparaître un sélecteur de fichiers qui vous permettra de choisir des fichiers à télécharger.

Vous avez déjà des fichiers multimédias que vous aimeriez insérer dans votre page ? Déplacez simplement le curseur de votre souris sur la vignette de l'image et faites **glisser-déposer** l'image dans l'éditeur.

Vous pouvez également sélectionner l'option *Insert** sur la vignette de l'image. Cela insérera le média directement dans le contenu de votre page.

Vous pouvez également supprimer des fichiers multimédias en sélectionnant **Delete**.

