<h1 class = "rem">Mise à jour de Grav & Plugins</h1>

La méthode préférée pour maintenir Grav, les plugins et les thèmes à jour est d'utiliser le **gestionnaire de paquets Grav (GPM)**. Des informations complètes peuvent être trouvées dans la [documentation Grav GPM](./cli-commandes-gpm.md).

Nous avons également **GPM** intégré dans notre plugin du [panneau d'administration](./panneau-admin-intro.md) qui vérifiera, demandera et installera automatiquement toutes les mises à jour.

<h2 id="Quelle version ai-je ?">Quelle version ai-je ?
<a href="#Quelle version ai-je ?" class="toc-anchor after"></a></h2>

Il existe plusieurs façons de savoir quelle version de Grav et des plugins le site utilise :

   * **Panneau d'administration** : La version de Grav est répertoriée dans le pied de page de n'importe quelle page. Les versions de plugins et de thèmes peuvent être trouvées dans leurs propres sections.
   * **CLI** : Exécutez la commande `bin/gpm version grav`. Pour obtenir la liste des versions de thèmes et de plugins, vous pouvez les répertorier par leurs noms.
   * **Système de fichiers** : Le moyen le plus simple de voir la version est de rechercher le fichier `CHANGELOG.md` à la racine de l'installation de Grav. Il en va de même pour les plugins et les thèmes, ils peuvent généralement être trouvés dans les dossiers `user/plugins` et `user/themes`.

<h2 id="Mise à niveau depuis Grav 1.5 ou une version antérieure">Mise à niveau depuis Grav 1.5 ou une version antérieure
<a href="#Mise à niveau depuis Grav 1.5 ou une version antérieure" class="toc-anchor after"></a></h2>

La mise à jour d'une ancienne version de Grav peut nécessiter des préparations et du travail supplémentaires en raison des exigences minimales accrues et des incompatibilités potentielles.

La marche à suivre est la suivante :

   * Copiez le site sur un serveur prenant en charge **PHP 7.3** et le support **CLI**.
   * Mettre à niveau manuellement **vers Grav 1.6.31**.
   * Mettre à niveau vers la dernière version.

Un guide détaillé [Mise à niveau depuis Grav <1.6](./avance-grav-up-1-6.md) devrait vous aider dans le processus.

<h2 id="Mise à niveau vers la version suivante">Mise à niveau vers la version suivante
<a href="#Mise à niveau vers la version suivante" class="toc-anchor after"></a></h2>

Pour la mise à niveau vers la version suivante, il existe des guides spéciaux pour vous aider à vous assurer que tout fonctionne toujours après la mise à niveau.

   * [Mise à niveau vers Grav 1.7](https://learn.getgrav.org/17/advanced/grav-development/grav-17-upgrade-guide)
   * [Mise à niveau vers Grav 1.6](https://learn.getgrav.org/17/advanced/grav-development/grav-16-upgrade-guide)
  
<div class = "notice note">   
<strong>REMARQUE</strong> : Il est recommandé de lire les guides de mise à niveau avant d'installer la prochaine version de Grav.
</div>

<h2 id="Mises à jour Grav CMS">Mises à jour Grav CMS
<a href="#Mises à jour Grav CMS" class="toc-anchor after"></a></h2>

La méthode préférée pour mettre à jour Grav consiste à utiliser le **gestionnaire de packages Grav (GPM)**. Tout ce que vous avez à faire est de naviguer jusqu'à la racine de votre site Grav et de taper :

    $ | bin/gpm selfupgrade -f

<div class = "notice tip"> 
<strong>ASTUCE</strong> : Vous trouverez plus d'informations sur la commande dans <a href="/grav-cli-gpm#self-upgrade">Commande GPM > Mise à jour automatique</a>
</div>

<h2 id="Mises à jour des plugins et des thèmes">Mises à jour des plugins et des thèmes
<a href="#Mises à jour des plugins et des thèmes" class="toc-anchor after"></a></h2>

Les plugins et les thèmes peuvent être tenus à jour en exécutant la commande suivante à partir de la racine de votre site Grav :

    $ | bin/gpm update

<div class = "notice tip"> 
<strong>ASTUCE</strong> : Vous trouverez plus d'informations sur la commande dans <a href="/grav-cli-gpm#self-upgrade">Commande GPM > Mettre à jour</a>.
</div>

  
