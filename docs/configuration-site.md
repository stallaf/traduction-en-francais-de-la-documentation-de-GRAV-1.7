<h1 class="rem">Paramétrage (site)</h1>

![Configuration de l'administrateur site 1](https://learn.getgrav.org/user/pages/05.admin-panel/02.dashboard/02.configuration-site/configuration-site.png)

La page **Configuration** vous donne accès aux paramètres de configuration du **System** et du **Site** de votre site. De plus, vous pouvez afficher une ventilation des propriétés de votre serveur dans un certain nombre de domaines, notamment PHP, SQL, l'environnement du serveur et divers autres composants qui déterminent le fonctionnement de votre site.

<div class="notice info">
La configuration nécessite un niveau d'accès <code>access.admin.super</code> ou <code>access.admin.configuration</code> && <code>access.admin.configuration_site</code>.
</div>

L'onglet **Site** vous permet de personnaliser les paramètres trouvés dans le fichier `/user/config/site.yaml`. Cet onglet vous donne accès aux options et aux champs qui déterminent les variables liées au site, telles que le nom, l'auteur par défaut et les métadonnées utilisées dans votre site.

Vous trouverez ci-dessous une ventilation des différentes sections de configuration qui apparaissent dans l'onglet **Site**.

<h2 id="Valeurs par défaut">Valeurs par défaut
<a href="#Valeurs par défaut" class="toc-anchor after"></a></h2>

![Configuration de l'administrateur site 2](https://learn.getgrav.org/user/pages/05.admin-panel/02.dashboard/02.configuration-site/configuration-site-defaults.png)

Cette section vous permet de définir les propriétés de base de la gestion du contenu de votre site. La page d'accueil, le thème par défaut et diverses autres options d'affichage de contenu sont définis ici.

| **OPTION**             | **DESCRIPTION**
| ----------             | ---------
| **Site Title**         | Titre par défaut de votre site, souvent utilisé par les thèmes.
| **Default Author**     | Un nom d'auteur par défaut, souvent utilisé dans les thèmes ou le contenu de la <br>page.
| **Default Email**      | Un e-mail par défaut à référencer dans les thèmes ou les pages.
| **Taxonomy Types**     | Les types de taxonomie doivent être définis ici si vous souhaitez les utiliser dans <br>des pages.

<h2 id="Sommaire des pages">Sommaire des pages
<a href="#Sommaire des pages" class="toc-anchor after"></a></h2>

![Configuration de l'administrateur site 3](https://learn.getgrav.org/user/pages/05.admin-panel/02.dashboard/02.configuration-site/configuration-site-page.png)

Les résumés de page sont un excellent moyen de donner un petit aperçu du contenu d'une page. Vous pouvez utiliser un délimiteur dans la page pour définir un point de "coupure" entre le contenu du résumé et le contenu complet de la page. Ces paramètres vous permettent de :

| **OPTION**             | **DESCRIPTION**
| ----------             | ---------
| **Enabled**            | Activer le résumé de la page (le résumé renvoie le même que le contenu de la page).
| **Summary Size**       | Le nombre de caractères d'une page à utiliser comme résumé du contenu.
| **Format**             | **short** = utiliser la première occurrence du délimiteur ou de la taille ; **long** = le délimiteur <br>récapitulatif sera ignoré
| **Delimiter**          | Le délimiteur récapitulatif (par défaut '==='). Vous placeriez généralement ceci après <br>un paragraphe d'ouverture, avec tout ce qui précède apparaissant dans le résumé de <br>la page.

<h2 id="Métadonnées">Métadonnées
<a href="#Métadonnées" class="toc-anchor after"></a></h2>

![Configuration de l'administrateur site 3](https://learn.getgrav.org/user/pages/05.admin-panel/02.dashboard/02.configuration-site/configuration-site-metadata.png)

Les métadonnées sont une partie importante de la composition des coulisses d'une page. Cela peut améliorer le référencement, la façon dont vos liens apparaissent dans divers moteurs de recherche et flux sociaux, et plus encore. Vous pouvez définir ici diverses propriétés de métadonnées.

| **OPTION**         | **DESCRIPTION**
| ----------         | ---------
| **Metadata**       | Valeurs de métadonnées par défaut qui seront affichées sur chaque page à moins qu'elles <br>ne soient remplacées par la page.

<h2 id="Redirections et routes">Redirections et routes
<a href="#Redirections et routes" class="toc-anchor after"></a></h2>

![Configuration de l'administrateur site 4](https://learn.getgrav.org/user/pages/05.admin-panel/02.dashboard/02.configuration-site/configuration-site-redirects.png)

Les redirections et le routage n'ont jamais été aussi simples. Il vous suffit de tout configurer dans cette section, et vous êtes prêt à partir.

| **OPTION**               | **DESCRIPTION**
| ----------               | ---------
| **Custom Redirects**     | Routes pour rediriger vers d'autres pages. Le remplacement standard de Regex <br>est valide.
| **Custom Routes**        | Routes vers des alias vers d'autres pages. Le remplacement standard de Regex <br>est valide.

