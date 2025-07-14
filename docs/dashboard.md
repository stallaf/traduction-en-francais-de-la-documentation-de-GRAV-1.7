<h1 class="rem">Tableau de bord</h1>

![Tableau de bord administrateur](https://learn.getgrav.org/user/pages/05.admin-panel/02.dashboard/grav-dashboard.png)

Le **tableau de bord** sert de centre d'informations pour le plug-in du **Panneau d'Administration**. À partir de cette page, vous pouvez vérifier les statistiques de trafic, les informations de maintenance, les mises à jour de Grav, créer de nouvelles sauvegardes, voir les dernières mises à jour de page, ainsi que vider rapidement le cache de Grav.

C'est un point de départ pour votre expérience administrative.

<div class="notice info">
Le contenu du tableau de bord changera en fonction des autorisations de l'utilisateur. Par exemple, donner <code>access.admin.super</code> déverrouille tout. Si ce niveau d'accès n'est pas accordé, <code>access.admin.maintenance</code> autorise l'effacement et les mises à jour du cache. <code>access.admin.pages</code> permet d'accéder aux pages. <code>access.admin.statistics</code> permet d'afficher les statistiques de consultation des pages du site.
</div>

<h2 id="Vérification du cache et des mises à jour">Vérification du cache et des mises à jour
<a href="#Vérification du cache et des mises à jour" class="toc-anchor after"></a></h2> 

<img alt="Tableau de bord" title="Tableau de bord" class="img" src="https://learn.getgrav.org/user/pages/05.admin-panel/02.dashboard/grav-dashboard-cache.png" width="325px">

En haut du tableau de bord, vous trouverez deux boutons. Le premier d'entre eux initie un vidage du cache Grav. Cliquer sur le bouton principal **Clear Cache** effacera tout le cache, y compris toute mise en cache des actifs et des images. À l'aide de la fonction de **drop-down** à droite, vous pouvez choisir parmi des types spécifiques de processus d'effacement du cache.

Par exemple, si vous souhaitez uniquement vider le **cache d'images** sans perturber les autres données mises en cache, vous pouvez le faire ici.

Le deuxième bouton lance une vérification de la mise à jour de votre site. Cela inclut tous les plugins, thèmes et Grav pris en charge. Si de nouvelles mises à jour sont découvertes, vous recevez une notification sur le tableau de bord. Ce n'est pas la seule méthode que Grav a pour vérifier les nouvelles mises à jour.

<div class="notice info">
Les vérifications de mise à jour sont également déclenchées chaque fois qu'une nouvelle page de l'administrateur est chargée et mise en cache pendant une journée. Si vous effacez tout le cache de Grav et chargez une nouvelle page dans l'administrateur, une vérification de la mise à jour aura automatiquement lieu.
</div>

<h2 id="Maintenance et Statistique de pages vues">Maintenance et Statistique de pages vues
<a href="#Maintenance et Statistique de pages vues" class="toc-anchor after"></a></h2> 

![Tableau de bord administrateur 3](https://learn.getgrav.org/user/pages/05.admin-panel/02.dashboard/grav-dashboard-maintenance.png)

Les sections **Maintenance** et **Page View Statistics** vous permettent d'accéder rapidement à des informations importantes sur votre site.

Du côté de la **Maintenance**, vous pouvez voir un graphique en pourcentage vous permettant de savoir combien de morceaux de Grav sont complètement à jour.

![Tableau de bord administrateur 4](https://learn.getgrav.org/user/pages/05.admin-panel/02.dashboard/grav-dashboard-maintenance-2.png)

Si de nouvelles mises à jour sont disponibles, un bouton <span class="fa fa-clound-download"></span> **Update** apparaîtra et vous permettra d'effectuer une mise à jour en un clic pour tous les plugins et thèmes. Ce bouton ne mettra pas à jour Grav lui-même, qui vous informe d'une mise à jour requise juste au-dessus des sections Maintenance et Page View Statistics.

Vous pouvez mettre à jour le noyau de Grav en sélectionnant le bouton <span class="fa fa-databse"></span> **Update Grav Now** dans sa barre de notification.

Il y a aussi un graphique indiquant combien de temps le site est resté sans être sauvegardé. La sélection du bouton **Backup** générera un fichier zip que vous pourrez télécharger et stocker en tant que sauvegarde des données de votre site.

<div class="notice info">
Les sauvegardes sont également stockées dans le dossier <code>backup/</code> de votre installation Grav. Vous pouvez les récupérer via FTP ou les outils de gestion Web fournis par votre société d'hébergement.
</div>

La section **Statistiques sur les pages vues** affiche des données de trafic simples et en un coup d'œil, décomposant le nombre de pages vues que le front-end du site a reçues au cours du dernier jour, de la semaine et du mois (30 jours). Page View Les statistiques de la semaine écoulée sont affichées dans un graphique à barres séparés par jours de la semaine.

<h2 id="Dernières mises à jour des pages">Dernières mises à jour des pages
<a href="#Dernières mises à jour des pages" class="toc-anchor after"></a></h2> 

![Tableau de bord administrateur 4](https://learn.getgrav.org/user/pages/05.admin-panel/02.dashboard/grav-dashboard-latest.png)

La zone **dernières mises à jour de page** de l'administrateur vous donne un aperçu des dernières modifications de contenu apportées aux pages de votre site Grav. Cette liste est triée par la dernière mise à jour et est générée chaque fois que vous actualisez la page. La sélection du titre d'une page dans cette liste vous amènera directement à l'éditeur de la page dans l'admin.

Le bouton **Gérer les pages** vous amène au panneau d'administration des **Pages**.

