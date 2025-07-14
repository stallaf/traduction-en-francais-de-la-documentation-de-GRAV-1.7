<h1 class="rem">Cycle de vie Grav</h1>

Il est souvent utile de savoir comment Grav traite afin de bien comprendre comment étendre au mieux Grav via des plugins. Voici le cycle de vie Grav :

<div class="lifecycle level level-1">
<h3>index.php</h3>
<ol>
<li>Vérifiez la version de PHP afin d'exécuter au moins la version <strong>7.1.3</strong></li>
<li>Initialisation du chargeur de classe</li>
<li>Obtenir l'instance Grav
<br>
<div class="lifecycle level level-2">
<h3>Grav.php</h3>
<ol>
   <li>Aucune instance n'existe, donc appelez <code>load()</code></li>
   <li>Ajouter <code>loader</code></li>
   <li>Ajouter et initialiser <code>debugger</code></li>
   <li>Ajouter <code>grav (obsolète)</code></li>
   <li>Enregistrer les services par défaut</li>
   <li>Enregistrer les fournisseurs de services
   <ol>
      <li>Fournisseur de services de comptes
      <ol>
         <li>Ajouter <code>permissions</code> (1.7)</li>
         <li>Ajouter <code>accounts</code> (1.6)</li>
         <li>Ajouter <code>user_groups</code> (1.7)</li>
         <li>Ajouter <code>users</code> <em>(obsolète)</em></li>
      </ol>
      <li>Fournisseur de services d'actifs
      <ol>
         <li>Ajouter <code>assets</code></li>
      </ol>
      <li>Fournisseur de services de sauvegardes
      <ol> 
         <li>Ajouter <code>backups</code> (1.6)</li>
      </ol>
      <li>Fournisseur de services de configuration
      <ol>
         <li>Ajouter <code>setup</code></li>
         <li>Ajouter <code>blueprints</code></li>
         <li>Ajouter <code>config</code></li>
         <li>Ajouter <code>langueges</code></li>
         <li>Ajouter <code>language</code></li>
      </ol>
      <li>Fournisseur de services d'erreur
      <ol>
         <li>Ajouter <code>error</code></li>
      </ol>
      <li>Fournisseur de services de système de fichiers
      <ol>
         <li>Ajouter <code>filesystem</code></li>
      </ol>
      <li>Fournisseur de services flexibles
      <ol>
         <li>Ajouter <code>flex</code> (1.7)
      </ol>
      <li>Fournisseur de services d'inflecteur
      <ol>
         <li>Ajouter <code>inflector</code></li>
      </ol>
      <li>Fournisseur de services d'enregistrement
      <ol>
         <li>Ajouter <code>log</code></li>
      </ol>
      <li>Fournisseur de services de sortie
      <ol>
         <li>Ajouter <code>output</code></li>
      </ol>
      <li>Fournisseur de services de pages
      <ol>
         <li>Ajouter <code>pages</code></li>
         <li>Ajouter <code>page</code></li>
      </ol>
      <li>Demander un fournisseur de services
      <ol>
         <li>Ajouter <code>request</code> (1.7)</li>
      </ol>
      <li>Fournisseur de services de planification
      <ol>
         <li>Ajouter <code>scheduler</code> (1.6)</li>
      </ol>
      <li>Fournisseur de services de session
      <ol>
         <li>Ajouter <code>session</code></li>
         <li>Ajouter <code>messages</code></li>
      </ol>
      <li>Fournisseur de services de flux
      <ol>
         <li>Ajouter <code>locator</code></li>
         <li>Ajouter <code>actor</code></li>
      </ol>
      <li>Fournisseur de services de tâches
      <ol>
         <li>Ajouter <code>task</code></li>
         <li>Ajouter <code>action</code></li>
      </ol>
      <li>Fournisseurs de services simples
      <ol>
         <li>Ajouter <code>browser</code></li>
         <li>Ajouter <code>cache</code></li>
         <li>Ajouter <code>events</code></li>
         <li>Ajouter <code>exif</code></li>
         <li>Ajouter <code>plugins</code></li>
         <li>Ajouter <code>taxonomy</code></li>
         <li>Ajouter <code>themes</code></li>
         <li>Ajouter <code>twig</code></li>
         <li>Ajouter <code>uri</code></li>
      </ol>
      </li>
   </ol>
</ol>  
</div> 
<li>appeler Grav::process()
<br>

<div class="lifecycle level level-2">
<h3>Grav.php</h3>
<ol>
<li>Exécutez l'initialisation du processeur
   <ol>
      <li>Configuration
      <ol>
         <li>Initialiser <code>$grav['config']</code></li>
         <li>Initialiser <code>$grav['plugins']</code></li>  
      </ol>
      <li>Enregistreur
      <ol>
         <li>Initialiser <code>$grav['log']</code></li>
      </ol>
      <li> Erreurs
      <ol>
         <li>Initialiser <code>$grav['errors']</code></li>
         <li>Enregistre les gestionnaires d'erreurs PHP</li>
      </ol>
      <li>Débogueur
      <ol>
         <li>Initialiser <code>$grav['debugger']</code></li>
      </ol>
      <li>Gérer les requêtes du débogueur
      <li>Démarrer la mise en mémoire tampon de sortie
      <li>Localisation
      <ol>
         <li>Définir les paramètres régionaux et le fuseau horaire</li>
      </ol>
      <li>Plugins
      <ol>
         <li>Initialiser <code>$grav['plugins']</code></li>
      </ol>
      <li>pages
      <ol>
         <li>Initialiser <code>$grav['pages']</code></li>
      </ol>
      <li>Uri
      <ol>
         <li>Initialiser <code>$grav['uri']</code></li>
         <li>Ajouter <code>$grav['base_url_absolute']</code></li>
         <li>Ajouter <code>$grav['base_url_relative']</code></li>
         <li>Ajouter <code>$grav['base_url']</code></li>
      </ol>
      <li>Gérer la redirection
      <ol>
         <li>Rediriger si <code>system.pages.redirect_trailing_slash</code> est <code>true</code> et la barre oblique finale dans l'URL
      </ol>
      <li>Comptes
      <ol>
         <li>Initialiser <code>$grav['accounts']</code></li>
      </ol>
      <li>Session
      <ol>
         <li>Initialiser <code>$grav['session']</code> si <code>system.session.initialize</code> est <code>true</code></li>
      </ol>
   </ol>
   <li>Exécuter le processeur de plugins
   <ol>
      <li>Événement Fire <strong>onPluginsInitialized</strong></li>
   </ol>
   <li>Exécuter le processeur de thèmes
   <ol>
      <li>Initialiser <code>$grav['themes']</code></li>
      <li>vénement Fire <strong>onThemeInitialized</strong>
   </ol>
   <li>Exécuter le processeur de requêtes
   <ol>
      <li>Initialiser <code>$grav['request']</code></li>
      <li>Lancer l'événement onRequestHandlerInit avec [request, handler]</code></li>
      <li>Si la réponse est définie à l'intérieur de l'événement, arrêter le traitement ultérieur et générer la réponse</li>
   </ol>
   <li>Exécuter le processeur de tâches
   <ol>
      <li>Si la demande a l'attribut controller.class et une tâche ou une action :
      <ol>
         <li>Exécutez le contrôleur</li>
         <li>Si <code>NotFoundException</code> : continuer (vérifier la tâche et l'action)</li>
         <li>Si code de réponse 418 : continuer (ignorer la tâche et l'action)</li>
         <li>Sinon : arrêter le traitement ultérieur et générer la réponse</li>
      </ol>
      <li>Si <em>task</em> :
      <ol>
         <li>Événement Fire <strong>onTask</strong></li>
         <li>Evénement Fire <strong>onTask.[TASK]</strong>/li>
      </ol>
      <li>Si <em>action</em> :
      <ol>
         <li>Événement Fire <strong>onAction</strong></li>
         <li>Evénement Fire <strong>onAction.[ACTION]</strong></li>
      </ol>
   <li>Exécuter le processeur de sauvegardes
   <ol>
      <li>Initialiser <code>$grav['backups']</code></li>
      <li>Événement Fire <strong>onBackupsInitialized</strong></li>
   </ol>
   <li>Exécuter le processeur du planificateur
   <ol>
      <li>Initialiser <code>$grav['scheduler']</code></li>
      <li>Événement Fire <strong>onSchedulerInitialized</strong></li>
   </ol>
   <li>Exécuter le processeur d'actifs
   <ol>
      <li>Initialiser <code>$grav['assets']</code></li>
      <li>Événement Fire onAssetsInitialized</li>
   </ol>
   <li>Exécuter le processeur Twig
   <ol>
      <li>Initialiser <code>$grav['twig']</code></li>

<div class="lifecycle level level-3">
<h3>Twig.php</h3>
<ol>
   <li>Définir les chemins du modèle Twig en fonction de la configuration</li>
   <li>Gérer les modèles de langue si disponibles</li>
   <li>Événement Fire <strong>onTwigTemplatePaths<</strong>/li>
   <li>Événement Fire <strong>onTwigLoader</strong></li>
   <li>Charger la configuration Twig et la chaîne du chargeur</li>
   <li>Événement Fire <strong>onTwigInitialized</strong></li>
   <li>Charger les extensions Twig</li>
   <li>Événement Fire<strong> onTwigExtensions</strong></li>
   <li>Définissez des variables Twig standard (config, uri, taxonomy, actifs, navigateur, etc.)</li>
   </ol>
</div>

   <li>Exécuter le processeur de pages
   <ol>
      <li>Initialiser <code>$grav['pages']</code>
      <br>
      
   <div class="lifecycle level level-3">           
   <h3>Pages.php</h3>
   <ol>
      <li>Appelez <code>buildPages()</code></li>
      <li>(la logique diffère quelque peu pour les Flex Pages, mais l'idée est la même))</li>
      <li>Vérifiez si le cache est bon)</li>
      <li>Si le cache est bon, les pages de chargement datent de</li>
      <li>Si le cache n'est pas bon, appelez <code>recurse())</code></li>
      <li>Lancer l'événement onBuildPagesInitialized dans <code>recurse())</code></li>
      <li>Si un fichier <code>.md</code> est trouvé :
      <br>
      
      <div class="lifecycle level level-4"> 
      <h3>Pages.php</h3>
      <ol>
         <li>Appelez <code>init()</code> pour charger les détails du fichier</li>
         <li>Définissez le <code>filePath</code>, <code>modified</code>, <code>id</code></li>
         <li>Appelez <code>header()</code> pour initialiser les variables d'en-tête</li>
         <li>Appelez <code>slug()</code> pour définir le slug d'URL</li>
         <li>Appelez <code>visible()</code> pour définir l'état visible</li>
         <li>Définissez le statut de <code>modularTwig()</code> en fonction du fait que 
         le dossier commence par _</li>
      </ol>
      </div>

      <li>Événement Fire <strong>onPageProcessed</strong></li>
      <li>Si un <code>folder</code> est trouve <code>recurse()</code> les enfants</li>
      <li>Événement Fire <strong>onFolderProcessed</code></strong></li>
      <li>Appelez <code>buildRoutes()</code></li>
      <li>Initialiser la <code>taxonomy</code> pour toutes les pages</li>
      <li>Créer une table <code>route</code> pour une recherche rapide</li>
   </ol>
   </div>
   
      <li>Événement Fire <strong>onPagesInitialized</strong> avec [pages]</li>
      <li>Lancer l'événement <strong>onPageInitialized</strong> avec [page]</li>
      <li>Si la page n'est pas routable :
      <ol>
         <li>Lancer l'événement <strong>onPageNotFount</strong> avec [page]</li>
      </ol>
      <li>Si tâche :
      <ol>
         <li>Lancer l'événement <strong>onPageTask</strong> avec [task, page]/LI>
         <li>Lancer l'événement <strong>onPageTask.[TASK]</strong> avec [task, page]</li>
      <li>Si action :
      <ol>
         <li>Lancer l'événement <strong>onPageAction</strong> avec [action, page]</li>
         <li>Lancer l'événement <strong>onPageAction.[ACTION]</strong> avec [action, page]</li>
      </ol>
   </ol>
<li>Exécuter le processeur d'actifs du débogueur
<ol>
   <li>Barre de débogage uniquement : ajoutez le débogueur CSS/JS aux ressources</li>
</ol>
   <li>Exécuter le processeur de rendu
   <ol>
      <li>Initialiser $grav['sortie']</li>
      <li>Si instance de sortie de ResponseInterface :</li>
      <ol>
         <li>Arrêter le traitement ultérieur et générer la réponse</li>
      </ol>
      <li>Sinon:
      <ol>
         <li>Rendre la page avec la méthode processSite() de Twig
         
         <div class="lifecycle level level-3">       
         <h3>Twig.php</h3>
         <ol>
            <li>Événement Fire onTwigSiteVariables</li>
            <li>Obtenir la sortie de la page</li>
            <li>Fire onTwigPageVariables, également appelé pour chaque sous-page modulaire</li>
            <li>Si une page n'est pas trouvée ou n'est pas routable, déclenchez 
            d'abord l'événement onPageFallBackUrl pour voir si nous avons une solution 
            de secours pour un élément multimédia, puis déclenchez onPageNotFound si 
            ce n'est pas le cas.</li>
            <li>Définir toutes les variables Twig sur l'objet Twig</li>
            <li>Définissez le nom du modèle en fonction des informations de 
            fichier /en-tête/extension</li>
            <li>Appelez la méthode render()</li>
            <li>Renvoyer le HTML résultant</li>
         </div>
         
         <li>Événement Fire onOutputGenerated</li>
         <li>Faites écho à la sortie dans le tampon de sortie</li>
         <li>Événement Fire onOutputRendered</li>
         <li>Construire un objet de réponse</li>
         <li>Arrêter le traitement ultérieur et générer la réponse</li>
      </ol>
   </ol>
<li>En-tête et corps HTTP de sortie</li>
<li>Débogueur de rendu (si activé)</li>
<li>Fermer
   <ol>
   <li>Fermer la session</li>
   <li>Fermer la connexion au client</li>
   <li>Événement Fire onShutdown</li>
</li>
</ol>
</div>
</div>

Chaque fois qu'une page a sa méthode `content()` appelée, le cycle de vie suivant se produit :

<div class="lifecycle level level-1">
<h3>Page.php</h3>

<ol>
   <li>Si le contenu n'est <strong>PAS</strong> mis en cache :
   <ol>
      <li>Lancer l'événement <strong>onPageContentRaw</strong></li>
      <li>Traitez la page en fonction des paramètres Markdown et Twig.
      Fire <strong>onMarkdownInitialized</strong> event</li>
      <li>Fire onMarkdownInitialized</li>
   </ol>
   <li>Événement Fire <strong>onPageContent</strong></li>
</ol>
</div>               

