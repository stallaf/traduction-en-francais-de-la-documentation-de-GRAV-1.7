<h1 class="rem">Plugin les bases</h1>

Grav a été conçu pour être **simple** et **ciblé**, ne traitant que des pages. L'idée est que Grav lui-même est **super léger**, fournissant juste assez de fonctionnalités pour faire les bases : routage, compilation Markdown vers HTML, création de modèles Twig et mise en cache.

Cependant, nous savions que nous voulions nous assurer que Grav puisse se développer et fournir des fonctionnalités puissantes en cas de besoin, nous avons donc construit **des crochets d'événement** dans tout le système afin que tout puisse être étendu avec des **plugins**.

<h2 id="Puissant !">Puissant !
<a href="#Puissant !" class="toc-anchor after"></a></h2> 

Tous les objets clés de Grav sont accessibles via un puissant [conteneur d'injection de dépendances](https://en.wikipedia.org/wiki/Dependency_injection). Avec les crochets d'événement de Grav tout au long du cycle de vie, vous pouvez accéder à tout ce que Grav connaît et le manipuler selon vos besoins. Avec ce système, vous avez le contrôle total pour ajouter autant de fonctionnalités que vous le souhaitez.

Les plugins se sont avérés si faciles à écrire, et si flexibles et puissants, que nous ne pouvons pas arrêter de les créer ! Nous avons déjà [plus de 300 plugins téléchargeables gratuitement](https://getgrav.org/downloads/plugins#extras) qui font tout, depuis l'affichage d'un **plan du site**, la fourniture de **fils d'Ariane**, l'affichage **d'archives** de blog, un simple **moteur de recherche**, jusqu'à la fourniture d'un **panier d'achat** entièrement fonctionnel alimenté par JavaScript !

La meilleure façon d'apprendre ce qui peut être fait avec les plugins est de télécharger certains d'entre eux et de regarder ce qu'ils font et comment ils le font. Dans le prochain chapitre, nous allons [créer un plugin simple à partir de zéro !](plugin-tutoriel.md)

<h2 id="Essentiel">Essentiel
<a href="#Essentiel" class="toc-anchor after"></a></h2> 

Tous les plugins sont situés dans votre dossier `user/plugins`. Avec l'installation de base de Grav, il n'y a que deux plugins fournis : `error` et `problems`.

Le plugin `error` est utilisé pour gérer les erreurs HTTP, comme **404 Page Not Found**.

Le plugin `problems` est utile pour les nouvelles installations de Grav car il détecte tout problème avec la **configuration de votre hébergement, les dossiers manquants** ou les **autorisations** qui pourraient causer des problèmes avec Grav. Seul le plugin `error` est vraiment indispensable au bon fonctionnement.

Chaque plugin dans le dossier `user/plugins` doit avoir un nom unique, et ce nom doit définir étroitement la fonction du plugin. Veuillez ne pas utiliser d'espaces, de traits de soulignement ou de majuscules dans le nom du plugin.

<h2 id="Accéder aux valeurs de configuration du plugin via Twig">Accéder aux valeurs de configuration du plugin via Twig
<a href="#Accéder aux valeurs de configuration du plugin via Twig" class="toc-anchor after"></a></h2> 

Pour accéder aux paramètres de configuration du plugin via Twig (c'est-à-dire dans un thème), le format général est :

    config.plugins.pluginname.pluginproperty

Si le nom du plugin contient des tirets, vous devez vous référer à ses propriétés en utilisant :

    config.plugins['plugin-name'].pluginproperty
    
