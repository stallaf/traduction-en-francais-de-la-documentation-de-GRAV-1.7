<h1 class="rem">Déployer avec Git</h1>

En utilisant le système de contrôle de version distribué [Git](https://git-scm.com/) sur vos environnements de développement et de serveur, vous pouvez mettre en place un flux de travail simple via un service Git hébergé comme [Github](https://github.com/) ou [GitLab](https://about.gitlab.com/). Essayez ceci si vous êtes à l'aise avec Git et ses outils clients.

Ses avantages incluent :

* c'est plus **propre** : vous n'avez qu'à émettre quelques instructions en ligne de commande et celles-ci peuvent être automatisées à n'importe quel degré
* **plus fiable** : vous n'avez pas besoin de vous souvenir des fichiers à télécharger et vous pouvez être sûr de ne faire remonter que les modifications souhaitées (particulièrement utile lorsque vous souhaitez uniquement télécharger certaines modifications dans les fichiers)
* **sécurité** : en utilisant un hôte cloud pour votre référentiel canonique ("origine"), les sauvegardes de sources (versionnées) sont gratuites ; vous pouvez même gérer vos tâches à l'aide de problèmes.

<h2 id="Mise en place">Mise en place.
<a href="#Mise en place" class="toc-anchor after"></a></h2>

Un flux de travail basé sur Git nécessite une certaine configuration. Voici un aperçu général de la configuration. Selon que vous souhaitez ou non valider des dossiers contenant du code tiers comme des `plugins`, il peut y avoir quelques étapes supplémentaires lors de la première configuration sur votre serveur.

* Dans votre environnement de développement, votre dossier `user` est un référentiel Git.
* Votre référentiel de dossiers `user` est également hébergé dans le cloud. Choisissez un fournisseur qui prend en charge les référentiels privés si vous ne souhaitez pas partager votre code avec le monde.
* Votre copie hébergée est l'`origin` "distante" de votre environnement local et serveur.
* Poussez les modifications sur votre site Grav depuis l'environnement local vers `origin` sur votre hôte cloud Git.
* Sur votre serveur, vous avez installé Grav et son dossier `user` est un clone de votre référentiel distant.
* Lorsque vous êtes prêt à mettre à jour votre site Grav sur votre serveur, utilisez Git pour extraire `origin`.

<h2 id="Mises à jour">Mises à jour.
<a href="#Mises à jour" class="toc-anchor after"></a></h2>

Après la configuration initiale, vous n'avez vraiment besoin d'effectuer que deux étapes après chaque mise à jour importante :

* pousser depuis votre environnement local,
* récupérez les modifications sur votre serveur.

<h2 id="Extension de votre configuration">Extension de votre configuration.
<a href="#Extension de votre configuration" class="toc-anchor after"></a></h2>

Si vous souhaitez une automatisation plus avancée, vous pouvez configurer [Git Hooks](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks) ou utiliser une fonctionnalité telle que les [webhooks de Github](https://docs.github.com/en/developers/webhooks-and-events/webhooks/about-webhooks). Vous pouvez également intégrer les modifications de contenu des éditeurs Web effectuant des modifications sur leurs propres installations via la console d'administration. Vous pouvez conserver des enregistrements (presque) immuables de ce qui est publié à l'aide des balises Git.

Les outils disponibles prennent en charge toutes sortes de workflows et d'automatisations multi-environnements.

<div class = "notice tip">
Vous pouvez également exploiter Git pour votre flux de travail de contenu à l'aide du <a href ="https://github.com/trilbymedia/grav-plugin-git-sync">plugin Git Sync</a>, afin que vos éditeurs de contenu puissent déployer des modifications via la console d'administration.
</div>

Voici une suggestion pour votre fichier `.gitignore` dans votre référentiel de dossiers `user`. Cela vous aidera à garder votre déploiement propre :

```configuration
accounts/*
!accounts/.*
data/*
!data/.*
languages/*
!languages/.*
plugins/*
!plugins/.*
themes/*
!themes/.*
!themes/MY_CUSTOM_THEME/
**/config/security.yaml
```

<div class = "notice info">
Si vous utilisez un thème personnalisé ou hérité que vous souhaitez inclure dans votre contrôle de code source, remplacez <code>MY_CUSTOM_THEME</code> ci-dessus par le nom du thème. Pensez à faire de même pour tous les plugins personnalisés spécifiques au site.
</div>

