<h1 class="rem">Éditeur (avancé)</h1>

![Éditeur de page d'administration 1](https://learn.getgrav.org/user/pages/05.admin-panel/03.page/03.advanced/page-advanced.png)

**L'éditeur de page** dans l'administration est un puissant éditeur de texte et gestionnaire de page qui vous permet de créer le contenu de votre page (y compris les fichiers multimédias), ses options de publication et de taxonomie, ses paramètres, ses remplacements et ses options spécifiques au thème.

Il s'agit essentiellement d'un guichet unique pour gérer une page spécifique.

Dans cette page, nous passerons en revue les caractéristiques et fonctionnalités trouvées dans l'onglet **Advanced** de **Page Editor**.

<div class="notice info">
L'accès à la fonctionnalité Pages nécessite une autorisation <code>access.admin.supe</code>r ou <code>access.admin.pages.list</code>, voir  <a href="../compte-utilisateur">Comptes d'utilisateurs</a> et <a href="../groupe-utilisateur">Groupes d'utilisateurs</a>
</div>

<div class="notice note">
Vous remarquerez peut-être les cases à cocher à gauche de certaines des options dans cette zone de l'administrateur. Ces cases indiquent que vous souhaitez remplacer les valeurs par défaut pour cette page. Si vous ne les cochez pas, vous revenez à l'état vide ou par défaut.
</div>

<h2 id="Réglages">Réglages
<a href="#Réglages" class="toc-anchor after"></a></h2>

![Éditeur de page d'administration 2](https://learn.getgrav.org/user/pages/05.admin-panel/03.page/03.advanced/page-advanced-settings.png)

La zone **Paramètre**s se concentre sur diverses options critiques pour votre page. C'est là que vous iriez pour changer le nom du dossier dans lequel la page est stockée, son parent et le modèle utilisé lors de l'affichage de la page.

| **OPTION**            | **DESCRIPTION**
| ---------             | ---------
| Folder Numeric Prefix | Préfixe numérique qui permet un tri manuel et implique la visibilité.
| Folder Name           | Définit le nom du dossier dans lequel la page est contenue.
| Parent                | Définit le parent de la page actuelle. Cela peut être - Racine - pour <br>les pages de niveau supérieur, ou des pages spécifiques pour les faire <br>apparaître en tant que sous-pages.
| Display Template      | Définit le modèle (fourni par le thème) à appliquer à la page. Cela a un <br>effet direct sur l'apparence de la page.
| Body Classes          | Les classes saisies dans ce champ sont appliquées au corps de la page.

<h2 id="Ordering">Ordering
<a href="#Ordering" class="toc-anchor after"></a></h2>

![Éditeur de page d'administration 3](https://learn.getgrav.org/user/pages/05.admin-panel/03.page/03.advanced/page-advanced-ordering.png)

La section **Ordering** vous permet de configurer l'ordre des pages des dossiers non numérotés.

| **OPTION**      | **DESCRIPTION**
| ---------       | ---------
| Page Order      | Vous permet de configurer l'ordre de la page.

<h2 id="Override">Override
<a href="#Override" class="toc-anchor after"></a></h2>

![Éditeur de page d'administration 4](https://learn.getgrav.org/user/pages/05.admin-panel/03.page/03.advanced/page-advanced-overrides.png)

Les remplacements sont ces options qui donnent à votre page des fonctionnalités supplémentaires, définissent son slug sur quelque chose de différent de celui par défaut en fonction du nom du dossier, des paramètres de mise en cache, de la visibilité de la navigation et rendent une page inaccessible via une URL directe.

Vous pouvez également utiliser cette zone pour activer et désactiver divers processus pour la page, tels que Twig qui vous permet d'injecter Twig dans le contenu de votre page et de le rendre.

| **OPTION**            | **DESCRIPTION**
| ---------          | ---------
| Menu               | La chaîne à utiliser dans un menu. S'il n'est pas défini, le titre sera <br>utilisé.
| Slug               | La variable slug vous permet de définir spécifiquement la partie de <br>l'URL de la page.
| Process            | Processus que vous aimeriez voir exécutés et rendus disponibles dans <br>le contenu de la page.
| Default Child Template   | Définit un type de page par défaut pour les pages enfants.
| Routable           | Définit si cette page est accessible ou non par une URL. Si elle est <br>désactivée, la page ne sera pas accessible sur le front-end.
| Caching            | Active ou désactive la mise en cache de la page.
| Visible            | Détermine si une page est visible dans la navigation.

<h2 id="Remplacements d'itinéraire">Remplacements d'itinéraire
<a href="#Remplacements d'itinéraire" class="toc-anchor after"></a></h2>

![Éditeur de page d'administration 5](https://learn.getgrav.org/user/pages/05.admin-panel/03.page/03.advanced/page-advanced-route.png)

| **OPTION**            | **DESCRIPTION**
| ---------       | ---------
| Canonical Route | Entrez une nouvelle valeur à utiliser pour le routage canonique.
| Route Aliases   | Créez des alias de route.

<h2 id="Remplacements spécifiques à l'administration">Remplacements spécifiques à l'administration
<a href="#Remplacements spécifiques à l'administration" class="toc-anchor after"></a></h2>

![Éditeur de page d'administration 6](https://learn.getgrav.org/user/pages/05.admin-panel/03.page/03.advanced/page-advanced-admin.png)

| **OPTION**               | **DESCRIPTION**
| ---------                | ---------
| Children Display Order   |  Définissez l'ordre d'affichage des enfants. Vous pouvez choisir <br>le nom du dossier ou la définition de la collection.

