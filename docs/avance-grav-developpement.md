<h1 class="rem">Développement Grav</h1>

Si vous souhaitez développer avec Grav, vous bénéficierez d'une configuration plus sophistiquée que celle requise pour un utilisateur régulier de Grav. Cela inclut à peu près n'importe quel type de développement, tel que : **Grav Core, Grav Plugins, Grav Skeletons** ou même **Grav Themes**.

Tout d'abord, décomposons les différents types de développement :

<h2 id="Noyau Grav">Noyau Grav.
<a href="#Noyau Grav" class="toc-anchor after"></a></h2>

Lorsque nous parlons du **Grav Core**, nous parlons effectivement de choses dans le dossier `system`. Ce dossier contrôle tout ce qui concerne Grav et est vraiment l'essence même du [flux de travail et du cycle de vie de Grav](/cycle-vie).

Grav se concentre intentionnellement sur le travail efficace avec les pages. La manipulation des pages et les fonctionnalités étendues sont souvent mieux servies en créant un plugin. Nous encourageons fortement notre communauté à apporter des corrections de bogues, et même à proposer le développement de fonctionnalités appropriées au cœur de Grav.

<h2 id="Tests en cours">Tests en cours.
<a href="#Tests en cours" class="toc-anchor after"></a></h2>

Tout d'abord, installez les dépendances de développement en exécutant composer install à partir de la racine Grav.

```console
$ | composer install
```

Ensuite, vous pouvez lancer les tests :

```console
$ | composer test
```

Cela exécutera la suite complète de tests existants qui doivent toujours être exécutés avec succès sur n'importe quel site.

Vous pouvez également exécuter un seul fichier de test unitaire, par ex.

```console
$ | composer test tests/unit/Grav/Common/Markdown/ParsedownTest::testAttributeLinks
```

Une méthode alternative à l'appel de ces tests est :

```console
$ | ./vendor/bin/codecept run
$ | ./vendor/bin/codecept run tests/unit/Grav/Common/Markdown/ParsedownTest::testAttributeLinks
```

<h2 id="Plugins Grav">Plugins Grav.
<a href="#Plugins Grav" class="toc-anchor after"></a></h2>

La plupart des efforts de développement prendront probablement la forme d'un **plugin Grav**. Parce que Grav a beaucoup de [crochets d'événement](/crochets-evenements), il est très facile de fournir des fonctionnalités améliorées et spécifiques via la création d'un plugin. Nous avons déjà développé de nombreux plugins qui fonctionnent de différentes manières en utilisant de nombreux événements différents pour montrer la puissance de cette fonctionnalité.

Il y a de nombreux avantages à fournir des fonctionnalités dans les plugins, mais quelques-uns des principaux avantages sont :

1. Le noyau Grav reste léger - Il vous suffit d'ajouter les plugins dont vous avez besoin pour un site particulier. Par exemple, un blog peut nécessiter beaucoup plus de plugins qu'une simple page de destination.
2. Développement tiers de nouvelles fonctionnalités - Vous n'avez pas à attendre que Grav obtienne les fonctionnalités que vous souhaitez. Vous pouvez simplement créer un plugin pour étendre Grav pour faire ce que vous voulez qu'il fasse.

<h3 id="Exigences relatives aux plugins">Exigences relatives aux plugins.
<a href="#Exigences relatives aux plugins" class="toc-anchor after"></a></h3>

Un plugin Grav approprié nécessite certains fichiers pour fonctionner correctement, être répertorié dans le référentiel Grav et être visible dans le plugin d'administration Grav. Veuillez vous assurer que votre plugin contient tous ces fichiers :

* **yourplugin.php**- fichier PHP du plugin qui doit porter le même nom que le dossier.
* **yourplugin.yaml** - fichier de configuration du plugin qui contient toutes les options et les informations d'héritage de flux.
* **blueprints.yaml** - fichier de définition de plugin et fichier de définition de formulaire.
* **CHANGELOG.md** - un fichier journal des modifications qui doit être au format Grav approprié pour un rendu cohérent.
* **README.md** - fichier requis pour expliquer et prévisualiser le plugin.
* **LICENSE** - fichier de licence, probablement MIT s'il est conforme au noyau Grav.
* **languages.yaml** (optionnel) - un fichier de définition de langue.

<h2 id="Squelettes Grav">Squelettes Grav.
<a href="#Squelettes Grav" class="toc-anchor after"></a></h2>

Un **Grav Skeleton** est en fait un **site d'échantillonnage tout-en-un**. Ils incluent le **Grav Core**, les **plugins** requis, ainsi que les **pages** appropriées pour le contenu et un **thème** pour tout rassembler.

Grav a été conçu pour rendre le processus de création d'un site aussi simple que possible. Pour cette raison, tout ce dont vous avez besoin pour un site peut être contenu dans le dossier `user`. Chacun des squelettes dont nous disposons actuellement est simplement un dossier `user` sur GitHub que nous regroupons avec diverses dépendances (plugins requis et thème) dans un package qui peut être simplement décompressé pour fournir un exemple de travail.

Ces squelettes sont une base sur laquelle vous pouvez développer votre site, rapidement et efficacement. Vous n'êtes pas enfermé dans un ensemble spécifique de fonctionnalités. Il est tout aussi flexible que n'importe quelle autre installation de Grav.

<h3 id="Exigences du squelette">Exigences du squelette.
<a href="#Exigences du squelette" class="toc-anchor after"></a></h3>

Un squelette Grav approprié nécessite certains fichiers pour fonctionner correctement, être répertorié dans le référentiel Grav et être visible dans le plugin d'administration Grav. Veuillez vous assurer que votre squelette contient tous ces fichiers :

* **.dependencies** - Un fichier pour définir les dépendances du thème et du plugin pour ce squelette.
* **blueprints.yaml** - fichier de définition de squelette et fichier de définition de formulaire.
* **CHANGELOG.md** - un fichier journal des modifications qui doit être au format Grav approprié pour un rendu cohérent.
* **README.md** - fichier requis pour expliquer et prévisualiser le plugin.
* **LICENSE** - fichier de licence, probablement MIT s'il est conforme au noyau Grav.
* **screenshot.jpg** - un aperçu au format 1:1 du thème. Doit être d'au moins 800px x 800px.

<h2 id="Thèmes Grav">Thèmes Grav.
<a href="#Thèmes Grav" class="toc-anchor after"></a></h2>

En raison du couplage étroit avec les pages et les **thèmes Grav**, un thème Grav fait partie intégrante et très importante d'un site Grav. Nous entendons par là que chaque page Grav fait référence à un modèle dans le thème, de sorte que votre thème doit fournir les **modèles Twig** appropriés que vos pages utilisent.

Le moteur de création de modèles Twig est un système très puissant, et parce qu'il n'y a vraiment aucune restriction de la part de Grav lui-même, vous êtes libre de créer n'importe quel type de design que vous souhaitez. C'est l'une des grandes choses qui distingue Grav d'un CMS traditionnel qui a un couplage lâche entre le contenu et la conception.

<h3 id="Exigences pour le théme">Exigences pour le théme.
<a href="#Exigences pour le théme" class="toc-anchor after"></a></h3>

Un thème Grav approprié nécessite certains fichiers pour fonctionner correctement, être répertorié dans le référentiel Grav et être visible dans le plugin d'administration Grav. Veuillez vous assurer que votre thème contient tous ces fichiers :

* **yourtheme.php** - fichier PHP du thème qui doit porter le même nom que le dossier.
* **yourtheme.yaml** - fichier de configuration du thème qui contient toutes les options et les informations d'héritage de flux.
* **blueprints.yaml** - fichier de définition de thème et fichier de définition de formulaire.
* **CHANGELOG.md** - un fichier journal des modifications qui doit être au format Grav approprié pour un rendu cohérent.
* **README.md** - fichier requis pour expliquer et prévisualiser le thème.
* **LICENSE** - fichier de licence, probablement MIT s'il est conforme au noyau Grav.
* **screenshot.jpg** - un aperçu au format 1:1 du thème. Doit être d'au moins 800px x 800px.
* **thumbnail.jpg** - une image miniature plus petite utilisée par le plug-in d'administration. Rapport d'aspect 1: 1 et devrait être à 300px x 300px.
* **languages.yaml** (optionnel) - un fichier de définition de langue.

<h2 id="Contenu de démonstration">Contenu de démonstration.
<a href="#Contenu de démonstration" class="toc-anchor after"></a></h2>

Vous pouvez fournir du contenu de démonstration dans le cadre d'un plugin ou d'un package thématique. Cela signifie que tout ce qui se trouve dans un dossier appelé `_demo/` sera copié dans le dossier `user/` dans le cadre de la procédure d'installation. Cela signifie que vous pouvez fournir des **pages**, une **configuration** ou tout autre élément se trouvant dans le dossier `user/`. L'utilisateur est invité à le faire, et c'est purement facultatif.

*Veuillez noter que le contenu de la démo n'est pas copié lorsque votre plugin ou thème est installé via le plugin `Admin`*.

<h2 id="Processus de publication de thème/plugin">Processus de publication de thème/plugin.
<a href="#Processus de publication de thème/plugin" class="toc-anchor after"></a></h2>

Lorsque vous avez créé votre nouveau thème ou plugin et que vous souhaitez le voir ajouté au [référentiel Grav](https://getgrav.org/downloads), vous devez vous assurer de quelques éléments standard :

1. Il est open source avec un fichier `LICENSE` qui fournit une licence compatible [MIT](https://en.wikipedia.org/wiki/MIT_License) [Exemple ici](https://github.com/getgrav/grav-theme-antimatter/blob/develop/LICENSE).
2. Contient un fichier `README.md` avec un résumé des fonctionnalités et des instructions sur la façon de l'installer et de le configurer. [Exemple ici](https://github.com/getgrav/grav-theme-antimatter/blob/develop/README.md).
3. Contient un fichier `blueprints.yaml` avec <span = "ancre">tous les champs obligatoires</span>. [Exemple ici](https://github.com/getgrav/grav-theme-antimatter/blob/develop/blueprints.yaml).
4. Fournissez un `CHANGELOG.md` au format correct un [exemple ici](https://github.com/getgrav/grav-theme-antimatter/blob/develop/CHANGELOG.md).
5. Fournit une attribution appropriée si vous utilisez d'autres bibliothèques, scripts ou codes.
6. [Créez une version](https://help.github.com/articles/creating-releases) pour votre plugin/thème fini. Le système de référentiel Grav nécessite une version et ne trouvera pas votre plugin/thème à moins qu'il n'y ait une version qui contient tout ce qui précède.
7. [Ajoutez un problème au suivi des problèmes Grav](https://github.com/getgrav/grav/issues/new?title=%5Badd-resource%5D%20New%20Plugin%2FTheme&body=I%20would%20like%20to%20add%20my%20new%20plugin%2Ftheme%20to%20the%20Grav%20Repository.%0AHere%20are%20the%20project%20details%3A%20%2A%2Auser%2Frepository%2A%2A) avec des détails sur votre plugin, et nous lui ferons un test rapide pour nous assurer qu'il fonctionne, puis l'ajouterons. Notez que vous n'avez pas à le faire si vous publiez une nouvelle version du plugin ou du thème qui est déjà dans le référentiel. Il sera récupéré automatiquement.

<div class = "notice info">
Assurez-vous que le <strong>nom de chaque balise est cohérent</strong>. GPM utilise ces informations pour déterminer si votre plugin/thème est plus récent que le précédent. Nous vous recommandons d'utiliser des <a href = "http://semver.org/">numéros de version sémantiques</a> pour les balises. Par exemple. <code>1.2.4</code>. La cohérence de toutes les balises est primordiale !
</div>

<h2 id="Format du journal des modifications">Format du journal des modifications.
<a href="#Format du journal des modifications" class="toc-anchor after"></a></h2>

Le site GetGrav.org utilise un format ChangeLog personnalisé qui est écrit en démarque standard mais peut être manipulé avec quelques CSS simples et [affiché dans un format attrayant](https://getgrav.org/downloads#changelog). Afin de vous assurer que vos ChangeLogs peuvent être analysés et formatés correctement, veuillez utiliser cette syntaxe :

```console
1  | # vX.Y.Z
2  | ## 01/01/2015
3  |
4  | 1. [](#nouveau)
5  |     * Nouvelles fonctionnalités ajoutées
6  |     * Une autre nouvelle fonctionnalité
7  | 2. [](#amélioré)
8  |     * Amélioration apportée
9  |     * Une autre amélioration
10 | 3. [](#bugfix)
11 |      * Correction de bug implémentée
12 |     * Une autre correction de bug
13 | 
14 | ...répéter...
```

Chaque section `#new, #improved, #bugfix` est facultative, incluez simplement les sections dont vous avez besoin.

<div class = "notice note">
Les dates peuvent utiliser soit le format de date <strong>américain</strong> <code>m/d/y</code>, soit le format <strong>européen</strong> <code>d-m-y</code>. Assurez-vous également qu'il y a une nouvelle ligne vide entre les en-têtes (version et date) et les listes (nouveau, amélioré, correction de bogue).
</div>

<h2 id="Configuration de GitHub">Configuration de GitHub.
<a href="#Configuration de GitHub" class="toc-anchor after"></a></h2>

Comme c'est le cas de nos jours, GitHub sera votre meilleur ami en matière de développement pour Grav. Nous avons créé des outils pour rendre cela aussi simple que possible, mais vous devez suivre certains modèles de développement pour simplifier le processus.

Clonez tous les référentiels avec lesquels vous prévoyez de travailler dans un seul dossier `Projets` ou `Développement` sur votre ordinateur. Cela permettra à nos outils fournis de trouver les référentiels dont ils ont besoin.

<div class = "notice info">
Nous utilisons le modèle de branchement <a href = "http://nvie.com/posts/a-successful-git-branching-model/">GitFlow</a> pour tous nos développements Grav. Le concept de base de la méthodologie GitFlow est que le développement se produit dans la branche <code>develop</code>, mais que de nouvelles fonctionnalités sont créées dans des branches <code>feature</code> distinctes qui sont fusionnées dans <code>develop</code> une fois terminées. Les versions fusionnent <code>develop</code> dans <code>master</code> et vous pouvez appliquer des branches <code>hotfix</code> selon vos besoins pendant le processus de publication. La plupart des clients git modernes le supportent. Cependant, nous recommandons <a href = "https://www.atlassian.com/software/sourcetree/overview" >Atlassian SourceTree</a> car il est gratuit, multiplateforme et facile à utiliser.
</div>

Grav a également quelques dépendances (dictées par le fichier `.dependencies`) qui incluent les plugins **Error** et **Problems**, ainsi que le thème **Antimatter**. Vous pouvez suivre ces instructions pour cloner ces bits sur votre propre ordinateur.

<div class = "notice warning">
Si vous souhaitez apporter des ajouts ou des modifications à l'un des référentiels <code>getgrav</code>, vous devrez <strong>forker</strong> le référentiel approprié, puis cloner <strong>l'url de votre fork</strong> plutôt que le référentiel <code>getgrav</code> directement. L'exemple ci-dessous utilise les référentiels <code>getgrav</code> directs à titre d'exemple uniquement.
</div>

```console
$ | cd
$ | mkdir Projects
$ | cd Projects
$ | mkdir Grav
$ | cd Grav
$ | git clone https://github.com/getgrav/grav.git
$ | git clone https://github.com/getgrav/grav-plugin-error.git
$ | git clone https://github.com/getgrav/grav-plugin-problems.git
$ | git clone https://github.com/getgrav/grav-theme-antimatter.git
```

Cela clonera **les 4 référentiels** dans votre dossier `~/Projects/Grav`.

Habituellement, la procédure normale pour configurer un site de test pour Grav consiste à utiliser la commande `bin/grav new-project`. Ceci est vrai pour le développement, sauf pour une différence importante. Parce que nous voulons pouvoir développer à partir de votre racine Web, mais que des modifications apparaissent dans votre code cloné, nous devons **lier symboliquement** les parties pertinentes. Pour ce faire, nous passons un indicateur `-s` à la commande `bin/grav new-project`.

Il y a une étape supplémentaire requise. Vous devez indiquer à la commande où elle peut trouver vos référentiels. Suivez donc ces étapes pour créer un fichier de configuration dans un nouveau dossier `.grav/` que vous devrez créer à la **racine de votre répertoire personnel** :

```console
$ | cd
$ | mkdir .grav
$ | vi .grav/config
```

Dans ce fichier : fournissez un mappage simple de l'emplacement des fichiers pertinents :

`github_repos : /Users/your_user/Projects/Grav/`

Assurez-vous d'avoir **enregistré** ce fichier et qu'il est lisible. Vous pouvez maintenant configurer votre site **symboliquement lié** où `~/www `est votre racine Web et `~/www/grav` est l'emplacement où votre nouveau site de test grav sera créé :

```console
$ | cd ~/Projets/Grav/grav
$ | bin/grav nouveau-projet -s ~/www/grav
```

Vous devriez voir une sortie à peu près comme celle-ci :

```console
rhukster@gibblets:~/Projects/Grav/grav(develop○) » bin/grav new-project -s ~/www/grav

Creating Directories
    /cache
    /logs
    /images
    /assets
    /user/accounts
    /user/config
    /user/pages
    /user/data
    /user/plugins
    /user/themes

Resetting Symbolic Links
    /index.php -> /Users/rhuk/www/grav/index.php
    /composer.json -> /Users/rhuk/www/grav/composer.json
    /bin -> /Users/rhuk/www/grav/bin
    /system -> /Users/rhuk/www/grav/system

Pages Initializing
    /Users/rhuk/Projects/Grav/grav/user/pages -> Created

File Initializing
    /.dependencies -> Created
    /.htaccess -> Created
    /user/config/site.yaml -> Created
    /user/config/system.yaml -> Created

Permissions Initializing
    bin/grav permissions reset to 755

read local config from /Users/rhuk/.grav/config

Symlinking Bits
===============

SUCCESS symlinked grav-plugin-problems -> user/plugins/problems

SUCCESS symlinked grav-plugin-error -> user/plugins/error

SUCCESS symlinked grav-theme-antimatter -> user/themes/antimatter
```

Comme vous pouvez le voir, un certain nombre de répertoires par défaut ont été créés et un dossier de `pages` initiales a également été créé. Une fois la base configurée, les autres dépendances sont symboliquement liées.

Vous devriez pouvoir pointer votre navigateur vers `http://localhost/grav` et voir le site de test que vous venez de configurer. Désormais, toutes les modifications que vous apportez dans votre dossier `~/www/grav` apparaîtront prêtes à être validées et insérées dans vos référentiels clonés.

<h2 id="Protocole des ressources abandonnées">Protocole des ressources abandonnées.
<a href="#Protocole des ressources abandonnées" class="toc-anchor after"></a></h2>

Les gens passent à autre chose et le contenu généré par les utilisateurs, comme les plugins et les thèmes, peut être abandonné. Si vous souhaitez reprendre la maintenance d'un thème ou d'un plugin existant, vous devez suivre ce protocole :

1. Soumettez une demande d'extraction bien formée et testée au référentiel d'origine.

2. Si le responsable ne répond pas du tout après 30 jours, ou si le responsable déclare qu'il abandonne la ressource et ne souhaite pas accorder à quelqu'un d'autre l'accès en écriture, passez à l'étape suivante.

3. [Soumettez un nouveau problème au référentiel GitHub de Grav](https://github.com/getgrav/grav/issues/new?title=%5Bchange-resource%5D%20Take%20over%20Plugin%2FTheme&body=I%20would%20like%20to%20take%20over%20an%20existing%20plugin%2Ftheme.%0AHere%20are%20the%20project%20details%3A%20%2A%2Auser%2Frepository%2A%2A) avec les détails suivants :
    * Titre : `[change-resource] Take over plugin/theme`
    
    * Indiquez le nom du plug-in et un lien vers le référentiel d'origine.
    
    * Lien vers votre demande d'extraction qui est restée sans réponse ou un lien vers la conversation dans laquelle le responsable a abandonné la ressource.

4. Les mainteneurs de Grav examineront le cas et vous feront savoir si la prise de contrôle est approuvée. Si l'approbation est accordée, passez à l'étape suivante.

5. Préparez votre référentiel forké avec une nouvelle version.

6. Ajoutez une note au README indiquant que ce référentiel est le nouveau référentiel et un lien vers l'ancien référentiel.

7. Répondez au problème en donnant aux mainteneurs la nouvelle URL du plugin.

8. Les mainteneurs mettront à jour GPM et les installations nouvelles et mises à jour proviendront désormais de votre référentiel forké.

