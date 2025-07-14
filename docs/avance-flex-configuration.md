<h1 class="rem">Configuration</h1>

La **Configuration** a des paramètres communs pour le **Flex Directory**.

Ces paramètres sont généralement utilisés pour modifier le comportement du répertoire, définir des valeurs par défaut pour les objets ou modifier le rendu des mises en page.

<div class="notice info">
Les paramètres sont différents dans chaque répertoire, ce document ne contient que les paramètres communs trouvés dans chaque répertoire.
</div>

<h3 id="Les contrôles">Les contrôles.
<a href="#Les contrôles" class="toc-anchor after"></a></h3> 

En haut de la page, vous trouverez les contrôles administratifs.

* **Retour** : Retour à la [**Liste de Contenus**](avance-flex-liste-contenus.md).
* **Enregistrer** : Enregistrez la configuration et revenez à la [**Liste de Contenus**](avance-flex-liste-contenus.md).

<h2 id="Onglet Mise en cache">Onglet Mise en cache.
<a href="#Onglet Mise en cache" class="toc-anchor after"></a></h2> 

| **OPTION**                  |**DESCRIPTION**
| ---------                   | ---------
| **Enable Index Caching**    | La mise en cache de l'index accélère les recherches en créant des <br>index de recherche temporaires pour les requêtes.
| **Index Cache Lifetime (seconds)**      | Durée de vie de la mise en cache d'index en secondes.
| **Enable Object Caching**   | La mise en cache des objets accélère le chargement des données et <br>des images des objets.
| **Object Cache Lifetime (seconds)**     | Durée de vie de la mise en cache d'objets en secondes.
| **Enable Render Caching**   | La mise en cache du rendu accélère le rendu du contenu en mettant <br> en cache le code HTML résultant.
| **Render Cache Lifetime (seconds)**     | Durée de vie du cache de rendu en secondes.

Si le rendu HTML a un contenu dynamique, le cache de rendu peut être désactivé à partir du modèle Twig par `{% do block.disableCache() %}`.

