<h1 class="rem">Paramétrage (Système)</h1>

![Configuration de l'administrateur](https://learn.getgrav.org/user/pages/05.admin-panel/02.dashboard/01.configuration-system/configuration.png)

La page **Configuration** vous donne accès aux paramètres de configuration du **système** et du **site**. De plus, vous pouvez afficher une ventilation des propriétés de votre serveur dans un certain nombre de domaines, notamment PHP, l'environnement du serveur et d'autres composants divers qui déterminent le fonctionnement de votre site.

<div class="notice info">
La configuration nécessite un niveau d'accès <code>access.admin.super</code> ou <code>access.admin.configuration</code>.
</div>

L'onglet **Système** vous permet de personnaliser les paramètres trouvés dans le fichier `/user/config/system.yaml`. Ces paramètres affectent le nombre de fonctionnalités principales liées au système de Grav qui fonctionnent. La page d'accueil du site, les paramètres de mise en cache, etc. peuvent être configurés ici.

Ces paramètres sont séparés en plusieurs sections, chacune se concentrant sur un aspect spécifique du fonctionnement de Grav.

Vous trouverez ci-dessous une ventilation des différentes sections de configuration qui apparaissent dans l'onglet **Système**.

<h2 id="Contenu">Contenu
<a href="#Contenu" class="toc-anchor after"></a></h2>

![Configuration de l'administrateur 2](https://learn.getgrav.org/user/pages/05.admin-panel/02.dashboard/01.configuration-system/configuration-system-content.png)

Cette section vous permet de définir les propriétés de base de la gestion du contenu de votre site. La page d'accueil, le thème par défaut et diverses autres options d'affichage de contenu sont définis ici.

| **OPTIONS**              | **DESCRIPTION**
| -------------            | -------------
| **Home Page**            | Sélectionnez la page que vous souhaitez voir apparaître comme page <br>d'accueil de votre site.
| **Default Theme**        | Définit le thème principal par défaut utilisé dans votre site.
| **Process**              | Contrôlez la façon dont les pages sont traitées. Peut être défini par <br>page plutôt que globalement.
| **Timezone**             | Remplace le fuseau horaire par défaut du serveur.
| **Short Date Format**    | Définissez le format de date courte qui peut être utilisé par les thèmes.
| **Long Date Format**     | Définissez le format de date long pouvant être utilisé par les thèmes.
| **Default Ordering**     | Les pages dans une liste seront rendues en utilisant cet ordre à moins <br>qu'il ne soit remplacé.
| **Default Order Direction** | Sens des pages dans une liste.
| **Default Page Count**      | Nombre maximal de pages par défaut dans une liste.
| **Date-based Publishing**   | (dé)publiez automatiquement les articles en fonction de leur date.
| **Events**               |Activer ou désactiver des événements spécifiques. Les désactiver peut <br>casser les plugins.
| **Redirect Default Route**  | Rediriger automatiquement vers la route par défaut d'une page.

<h2 id="Langues">Langues
<a href="#Langues" class="toc-anchor after"></a></h2>

![Configuration de l'administrateur 3](https://learn.getgrav.org/user/pages/05.admin-panel/02.dashboard/01.configuration-system/configuration-system-languages.png)

Les fonctionnalités multilingues sont définies dans cette section.

| **OPTIONS**              | **DESCRIPTION**
| -------------            | -------------
| **Supported**            | Prise en charge Liste séparée par des virgules de codes de langue à <br>2 lettres (par exemple 'en, fr, de').
| **Translations Enabled** | Prise en charge des traductions dans Grav, plugins et extensions.
| **Translations Fallback**         | Replis sur les traductions prises en charge si la langue active <br>n'existe pas.
| **Active Language in Section**    | Langue active dans la section Enregistrez la langue active dans la <br>session.
| **Home Redirect Include Language**| Inclut la langue dans la redirection d'accueil (/en).
| **Home Redirect Include Route**   | La redirection à la maison inclut la route.

<h2 id="En-têtes HTTP">En-têtes HTTP
<a href="#En-têtes HTTP" class="toc-anchor after"></a></h2>

![Configuration de l'administrateur 4](https://learn.getgrav.org/user/pages/05.admin-panel/02.dashboard/01.configuration-system/configuration-system-http.png)

Les options d'en-tête HTTP peuvent être définies dans cette section. Ceci est utile pour la mise en cache et l'optimisation basées sur le navigateur.

| **OPTIONS**              | **DESCRIPTION**
| -------------            | -------------
| **Expires**              | Définit l'en-tête d'expiration. La valeur est en secondes.
| **Last Modified**        | Définit le dernier en-tête modifié qui peut aider à optimiser la mise en <br>cache du proxy et du navigateur.
| **ETag**                 | Définit l'en-tête etag pour aider à identifier quand une page a été modifiée.
| **Vary Accept Encoding** | Définit l'en-tête Vary: Accept Encoding pour faciliter la mise en cache du <br>proxy et du CDN.

<h2 id="Markdown">Markdown
<a href="#Markdown" class="toc-anchor after"></a></h2>

![Configuration de l'administrateur 5](https://learn.getgrav.org/user/pages/05.admin-panel/02.dashboard/01.configuration-system/configuration-system-markdown.png)

Markdown constitue l'essentiel du contenu de la page de Grav. Cette section vous donne des options pour activer Markdown Extra, ainsi que pour définir comment Grav gère Markdown.

| **OPTIONS**              | **DESCRIPTION**
| -------------            | -------------
| **Markdown Extra **      | Activez la prise en charge par défaut de [Markdown Extra](https://michelf.ca/projects/php-markdown/extra/).
| **Auto Line Break**      | Activez la prise en charge des sauts de ligne automatiques dans Markdown.
| **Auto URL Links**       | Activez la conversion automatique des URL en hyperliens HTML.
| **Escape Markup**        | Échappez les balises de balisage dans les entités HTML.

<h2 id="Mise en cache">Mise en cache
<a href="#Mise en cache" class="toc-anchor after"></a></h2>

![Configuration de l'administrateur 6](https://learn.getgrav.org/user/pages/05.admin-panel/02.dashboard/01.configuration-system/configuration-system-caching.png)

La fonction de mise en cache intégrée de Grav en fait l'une des options de CMS à fichier plat les plus rapides du marché. Vous pouvez configurer les principales fonctions de mise en cache de votre site dans cette section.

| **OPTIONS**              | **DESCRIPTION**
| -------------            | -------------
| **Caching**              | Commutateur ON/OFF global pour activer/désactiver la mise en cache Grav.
| **Cache Check Method**   | Définit la méthode de vérification du cache. Les options sont **File**, **Folder** et <br>**None**.
| **Cache Driver**         | Choisissez le pilote de cache que Grav doit utiliser. 'Auto Detect' tente de <br>trouver le meilleur pour vous.
| **Cache Prefix**         | Un identifiant pour une partie de la clé Grav. Ne changez pas à moins que <br>vous ne sachiez ce que vous faites.
| **Lifetime**             | Définit la durée de vie du cache en secondes. 0 = infini.
| **Gzip Compression**     | Activez la compression GZip de la page Grav pour des performances accrues.

<h2 id="Twig Templating">Twig Templating
<a href="#Twig Templating" class="toc-anchor after"></a></h2>

![Configuration de l'administrateur 7](https://learn.getgrav.org/user/pages/05.admin-panel/02.dashboard/01.configuration-system/configuration-system-twig.png)

Cette section se concentre sur la fonctionnalité de création de modèles de Grav's Twig. Vous pouvez définir la mise en cache Twig, le débogage et modifier les paramètres de détection ici.

| **OPTIONS**              | **DESCRIPTION**
| -------------            | -------------
| **Twig Caching**         | Contrôlez le mécanisme de mise en cache Twig. Laissez cette option activée <br>pour de meilleures performances.
| **Twig Debug**           | Permet de ne pas charger l'extension Twig Debugger.
| **Detect Changes**       | Twig recompilera automatiquement le cache Twig s'il détecte des changements <br>dans les modèles Twig.
| **Autoescape Variables** | Échappe automatiquement toutes les variables. Cela cassera probablement <br>votre site.

<h2 id="Assets">Assets
<a href="#Assets" class="toc-anchor after"></a></h2>

![Configuration de l'administrateur 8](https://learn.getgrav.org/user/pages/05.admin-panel/02.dashboard/01.configuration-system/configuration-system-assets.png)

Cette section traite de la gestion des actifs, y compris les actifs CSS et JavaScript.

| **OPTIONS**              | **DESCRIPTION**
| -------------            | -------------
| **CSS Pipeline**         | Le pipeline CSS est l'unification de plusieurs ressources CSS dans un <br>seul fichier.
| **CSS Minify**           | Minify le CSS pendant le pipelining.
| **CSS Minify Windows Override** | Minify Override pour les plates-formes Windows. False par <br>défaut en raison de ThreadStackSize.
| **CSS Rewrite**          | Réécrivez toutes les URL relatives CSS pendant le pipelining.
| **JavaScript Pipeline**  | Le pipeline JS est l'unification de plusieurs ressources JS en un seul <br>fichier.
| **JavaScript Minify**    | Minify le JS pendant le pipelining.
| **Enable Timestamps on Assets** | Activer les horodatages des actifs.
| **Collections**          | Ajoutez des collections de ressources individuelles.

<h2 id="Gestionnaire d'erreurs">Gestionnaire d'erreurs
<a href="#Gestionnaire d'erreurs" class="toc-anchor after"></a></h2>

![Configuration de l'administrateur 9](https://learn.getgrav.org/user/pages/05.admin-panel/02.dashboard/01.configuration-system/configuration-system-error.png)

Vous pouvez définir comment Grav gère le rapport d'erreurs et l'affichage ici. Il s'agit d'un outil utile à avoir lors du développement du site.

| **OPTIONS**              | **DESCRIPTION**
| -------------            | -------------
| **Display Error**        | Journaliser les erreurs 
| **Log Errors**           | Journaliser les erreurs dans le dossier /logs.

<h2 id="Débogueur">Débogueur
<a href="#Débogueur" class="toc-anchor after"></a></h2>

![Configuration de l'administrateur 10](https://learn.getgrav.org/user/pages/05.admin-panel/02.dashboard/01.configuration-system/configuration-system-debugger.png)

Comme la gestion des erreurs, les outils de débogage intégrés de Grav vous permettent de localiser et de résoudre les problèmes. Ceci est particulièrement utile lors du développement.

| **OPTIONS**              | **DESCRIPTION**
| -------------            | -------------
| **Debugger**             | Activez le débogueur Grav et les paramètres suivants.
| **Debug Twig**           | Activer le débogage des modèles Twig.
| **Shutdown Close Connection** | Fermez la connexion avant d'appeler onShutdown(). <br>false pour le débogage.

<h2 id="Médias">Médias
<a href="#Médias" class="toc-anchor after"></a></h2>

![Configuration de l'administrateur 11](https://learn.getgrav.org/user/pages/05.admin-panel/02.dashboard/01.configuration-system/configuration-system-media.png)

Cette section détermine comment Grav gère le contenu multimédia. La qualité d'image et les autres options de gestion des supports sont configurées ici.

| **OPTIONS**              | **DESCRIPTION**
| -------------               | -------------
| **Default Image Quality**   | Qualité d'image par défaut à utiliser lors du rééchantillonnage ou <br>de la mise en cache des images (85 %).
| **Cache All Images**        | Exécute toutes les images via le système de cache de Grav même si <br>elles n'ont pas de manipulations de média.
| **Image Debug Watermark**   | Afficher une superposition sur les images indiquant la profondeur <br>en pixels de l'image lorsque vous travaillez avec Retina par exemple.
| **Enable Timestamps on Media** | Ajoute un horodatage basé sur la date de la dernière modification <br>à chaque élément multimédia.

<div class="notice info">
La mise en cache d'images qui ont déjà été optimisées (en dehors de Grav) peut donner au fichier de sortie une taille de fichier beaucoup plus grande que l'original. Cela est dû à un bogue dans la bibliothèque d'images Gregwar et n'est pas directement lié à Grav. Voir ce <a href="https://github.com/Gregwar/Image/issues/115">problème ouvert</a> pour plus d'informations. L'alternative consiste à définir "Cache All Images" sur Non.
</div>

<h2 id="Session">Session
<a href="#Session" class="toc-anchor after"></a></h2>

![Configuration de l'administrateur 12](https://learn.getgrav.org/user/pages/05.admin-panel/02.dashboard/01.configuration-system/configuration-system-session.png)

Cette section vous donne la possibilité d'activer la prise en charge de session, de définir des délais d'expiration et le nom du cookie de session utilisé pour gérer ces informations.

| **OPTIONS**        | **DESCRIPTION**
| -------------      | -------------
| **Enabled**        | Activer le support de session dans Grav.
| **Timeout**        | Définit le délai d'expiration de la session en secondes.
| **Name**           | Un identifiant utilisé pour former le nom du cookie de session. Utilisez uniquement des <br>caractères alphanumériques, des tirets ou des traits de soulignement. Ne pas utiliser de <br>points dans le nom de la session.

<h2 id="Avancé">Avancé
<a href="#Avancé" class="toc-anchor after"></a></h2>

![Configuration de l'administrateur 13](https://learn.getgrav.org/user/pages/05.admin-panel/02.dashboard/01.configuration-system/configuration-system-advanced.png)

Cette section contient les options système avancées.

| **OPTIONS**        | **DESCRIPTION**
| -------------      | -------------
| **Absolute URLs**  | URL absolues ou relatives pour base_url.
| **Parameter Separator** | Séparateur pour les paramètres passés qui peuvent être modifiés pour <br>Apache sous Windows.

