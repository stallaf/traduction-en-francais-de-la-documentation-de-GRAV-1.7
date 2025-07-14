<h1 class="rem">Introduction</h1>

Cette section contient une procédure pas à pas pour activer rapidement  le **Répertoire** existant et comment l'afficher sur votre site. Dans nos exemples, nous utilisons le Répertoire de **Contacts** qui existe déjà sur votre site.

<h2 id="Activer un répertoire">Activer un répertoire.
<a href="#Activer un répertoire" class="toc-anchor after"></a></h2> 

![Flex - Configuration des plugins](https://learn.getgrav.org/user/pages/08.advanced/01.flex/01.administration/01.introduction/flex-objects-options.png)

Pour activer un **Répertoire Flex** personnalisé, vous devez accéder à **Plugins** > **Flex Objects**.

Nous sommes intéressés par l'option **Répertoires** dans le plugin, qui répertorie tous les **Répertoires Flex** détectés. Sélectionnez simplement les répertoires qui vous intéressent et assurez-vous que l'option `Enabledè est cochée.

<div class="notice tip">
<strong>ASTUCE</strong> : Activez le répertoire appelé <strong>Contacts</strong>.
</div>

Appuyez sur **Enregistrer** et le répertoire devrait apparaître après le chargement d'une page.

<h2 id="Installer des exemples de données (facultatif)">Installer des exemples de données (facultatif).
<a href="#Installer des exemples de données (facultatif)" class="toc-anchor after"></a></h2> 

Pour notre exemple, nous supposons que vous avez copié l'exemple d'ensemble de données pour le Répertoire des **Contacts** :

```console
$ | cp user/plugins/flex-objects/data/flex-objects/contacts.json user/data/flex-objects/contacts.json
```

<h2 id="Créer une page">Créer une page.
<a href="#Créer une page" class="toc-anchor after"></a></h2> 

Accédez à [**Pages**](dashboard-pages.md) et [ajoutez une nouvelle page](dashboard-pages.md#Ajouter une page). Saisissez les valeurs suivantes :

* **Titre de la page** : `Flex-Objects`
* **Modèle de page** : `Flex-Objects`

Après cela, vous pouvez cliquer sur le bouton **Continuer**.

Dans [**l'éditeur de contenu**](avance-flex-editeur-contenus.md), entrez le répertoire et ajoutez du contenu :

* **Répertoire Flex** : `Contacts`
* **Contenu** :

```console
 # Directory Example
```

Lorsque vous êtes satisfait de la page, appuyez sur **Enregistrer**.

<div class="notice note">
<strong>ASTUCE</strong> : Si vous ne spécifiez pas <code>Flex Directory</code>, la page listera tous les répertoires au lieu d'afficher les entrées d'un seul répertoire.
</div>

<h2 id="Afficher la page">Afficher la page.
<a href="#Afficher la page" class="toc-anchor after"></a></h2> 

Vous pouvez simplement aller sur votre site et regarder le menu. Il doit contenir des **Contacts**. L'accès à cette page devrait afficher :

![Flex - Exemple de Répertoires 1](https://learn.getgrav.org/user/pages/08.advanced/01.flex/01.administration/01.introduction/flex-objects-site.png)

Au cas où vous n'auriez sélectionné aucun répertoire, voici ce que vous verriez à la place :

![Flex - Exemple de Répertoires 2](https://learn.getgrav.org/user/pages/08.advanced/01.flex/01.administration/01.introduction/flex-objects-directory.png)

