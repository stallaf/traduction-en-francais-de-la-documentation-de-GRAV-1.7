<h1 class="rem">Éditeur (Options)</h1>

![Éditeur de page d'administration 1](https://learn.getgrav.org/user/pages/05.admin-panel/03.page/02.options/page-options.png)

**L'éditeur de page** dans l'administration est un puissant éditeur de texte et gestionnaire de page qui vous permet de créer le contenu de votre page (y compris les fichiers multimédias), ses options de publication et de taxonomie, ses paramètres, ses remplacements et ses options spécifiques au thème.

Il s'agit essentiellement d'un guichet unique pour gérer une page spécifique.

Dans cette page, nous passerons en revue les caractéristiques et fonctionnalités trouvées dans l'onglet **Options** de **l'éditeur de page**.

<div class="notice info">
L'accès à la fonctionnalité Pages nécessite une autorisation <code>access.admin.super</code> ou <code>access.admin.pages.list</code>, voir <a href="../compte-utilisateur">Comptes d'utilisateurs</a> et <a href="../groupe-utilisateur">Groupes d'utilisateurs</a>.
</div>

<div class="notice note">
Vous remarquerez peut-être les cases à cocher à gauche de certaines des options dans cette zone de l'administrateur. Ces cases indiquent que vous souhaitez remplacer les valeurs par défaut pour cette page. Si vous ne les cochez pas, vous revenez à l'état vide ou par défaut.
</div>

<h2 id="Édition">Édition
<a href="#Édition" class="toc-anchor after"></a></h2>

![Éditeur de page d'administration 2](https://learn.getgrav.org/user/pages/05.admin-panel/03.page/02.options/page-options-publishing.png)

Cette section concerne le contrôle de la manière dont votre contenu est publié. Vous pouvez publier (ou dépublier) du contenu, définir des dates de publication ainsi que des dates et heures de dépublication et créer des valeurs de métadonnées spécifiques à la page.

| **OPTION**         | **DESCRIPTION**
| ---------          | ---------
| Published          | Par défaut, une page est publiée à moins que vous ne définissiez <br>explicitement publié : faux ou via une `publish_date` située <br>dans le futur, ou une `unpublish_date` dans le passé.
| Date               | La variable date permet de définir spécifiquement une date associée à <br>cette page.
| Published Date     | Il s'agit de la date de publication officielle de la page. Il peut fournir une <br>date pour déclencher automatiquement la publication.
| Unpublished Date   | Il s'agit de la date/heure que vous souhaitez marquer pour que la page <br>déclenche automatiquement la dépublication.
| Metadata           | Valeurs de métadonnées par défaut qui seront affichées sur chaque page <br>à moins qu'elles ne soient remplacées par la page.

<h2 id="Taxonomies">Taxonomies
<a href="#Taxonomies" class="toc-anchor after"></a></h2>

![Éditeur de page d'administration 3](https://learn.getgrav.org/user/pages/05.admin-panel/03.page/02.options/page-options-publishing.png)

La zone Taxonomies est l'endroit où vous pouvez configurer les propriétés organisationnelles de votre page. Dans quelle(s) catégorie(s) la page apparaîtra, ses balises, etc. peuvent être configurées ici.

| **OPTION**         | **DESCRIPTION**
| ---------          | ---------
| Category           | Ce champ vous permet de définir une ou plusieurs catégories pour la page. Il est <br>utile pour le tri et le filtrage de contenu.
| Tag                | Les balises sont un excellent moyen de fournir des informations sur le contenu <br>de votre page. Il est utile pour les sites axés sur le contenu en tant que mécanisme <br>d'organisation et de filtrage.
| Month              |

<h2 id="Plan du site">Plan du site
<a href="#Plan du site" class="toc-anchor after"></a></h2>

![Éditeur de page d'administration 4](https://learn.getgrav.org/user/pages/05.admin-panel/03.page/02.options/page-options-publishing.png)

Avoir un bon sitemap propre est important pour plusieurs raisons. Parmi eux, la navigation des utilisateurs et l'optimisation des moteurs de recherche (SEO). La mise en place d'un sitemap rend votre site intrinsèquement plus convivial pour les moteurs de recherche, ce qui peut avoir un impact direct sur le classement.

Cette zone de la page d'options n'est disponible que si vous installez le [plugin Sitemap](https://github.com/getgrav/grav-plugin-sitemap).

| **OPTION**               | **DESCRIPTION**
| ---------                | ---------
| Sitemap Change Frequency | Ce menu déroulant vous permet de définir une fréquence à <br>laquelle le plan du site de la page est mis à jour. Cela peut être à <br>chaque fois qu'un changement est effectué, toutes les heures, <br>tous les jours, toutes les semaines, tous les mois, tous les ans <br>ou jamais. Par défaut, les options globales du sitemap <br>sont utilisées.
| Sitemap Priority         | Définit la priorité de cette page dans votre plan du site.

