<h1 class="rem">Configuration</h1>

<div class="box">
Onglet Compatibilité
</div>

| **OPTION**                | **DESCRIPTION**
| ---------                 | ---------
| Admin event compatibility | Active les événements `onAdminSave` et `onAdminSaveAfter` pour <br>les plugins. Activé par défaut.

<div class="box">
Onglet Mise en cache
</div>

Pour plus d'informations, voir Objets Flex.

| **OPTION**               | **DESCRIPTION**
| ---------                | ---------
| Enable Index Caching     | La mise en cache de l'index accélère les recherches en <br>créant des index de recherche temporaires pour les <br>requêtes.
| Index Cache Lifetime (seconds) | Durée de vie de la mise en cache d'index en secondes.
| Enable Object Caching    | La mise en cache des objets accélère le chargement <br>des données et des images des objets.
| Object Cache Lifetime (seconds) |Durée de vie de la mise en cache d'objets en secondes.
| Enable Render Caching    | La mise en cache du rendu accélère le rendu du  <br>contenu en mettant en cache le code HTML résultant.
| Render Cache Lifetime (seconds)  | Durée de vie du cache de rendu en secondes.

Si le rendu HTML a un contenu dynamique, le cache de rendu peut être désactivé à partir du modèle Twig par {% verbatim %}{% `do block.disableCache() %}`{% endverbatim %}.
