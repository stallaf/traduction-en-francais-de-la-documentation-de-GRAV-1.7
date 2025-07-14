<h1 class="rem">Microsoft Azure</h1>

[Microsoft Azure](https://azure.microsoft.com/) est une plate-forme de cloud computing de niveau entreprise, ouverte et flexible. Il existe plusieurs façons de déployer Grav dans Azure, mais ce didacticiel vous guidera à l'aide de l'application Web Azure (PaaS).

<h2 id="Choses dont vous aurez besoin">Choses dont vous aurez besoin.
<a href="#Choses dont vous aurez besoin" class="toc-anchor after"></a></h2>

* Un compte Azure
* Compte Github
* Une copie de Grav

![Logo Azure](https://learn.getgrav.org/user/pages/09.webservers-hosting/03.paas/04.azure/Azure.png)

<h2 id="Inscription sur Azure">Inscription sur Azure.
<a href="#Inscription sur Azure" class="toc-anchor after"></a></h2>

Commencez par [créer un compte](https://azure.microsoft.com/en-gb/free/) sur Azure, vous aurez accès à des services gratuits plus 150 £ (Royaume-Uni) de crédit à utiliser pendant les 30 premiers jours.

<h2 id="Inscription sur GitHub">Inscription sur GitHub.
<a href="#Inscription sur GitHub" class="toc-anchor after"></a></h2>

Si vous n'avez pas de compte GitHub, [veuillez en créer un](https://github.com/join?source=header-home), le plan gratuit est suffisant.

<h2 id="Cloner le code source">Cloner le code source.
<a href="#Cloner le code source" class="toc-anchor after"></a></h2>

Vous avez besoin d'une copie de Grav pour suivre ce tutoriel, je suggérerais de télécharger les fichiers Grav et Admin Plugin de base, et de créer un référentiel Github avec ces fichiers

Vous devriez maintenant disposer de tous les composants nécessaires pour déployer une copie de travail de Grav dans Azure.

<h2 id="Fichier Web.Config">Fichier Web.Config.
<a href="#Fichier Web.Config" class="toc-anchor after"></a></h2>

En plus du code Grav, vous avez besoin d'un fichier web.config. Le fichier web.config est un fichier XML qui se trouve dans le dossier racine de l'application Web et contient généralement les principaux paramètres et la configuration de l'application Web.

Un exemple de fichier web.config est disponible sur le site. Ce fichier web.config couvre ce que l'application Web doit faire avec des formats de fichiers tels que .woff et .woff2, qui font désormais partie des derniers [packs Font Awesome](https://fontawesome.com/).

Grav a également inclus des exemples de fichiers web.config dans leurs fichiers source, vous pouvez les trouver dans le dossier webserver-configs.

Une fois que vous avez configuré votre web.config, vous devez le télécharger sur votre référentiel Grav GitHub, il doit être au niveau racine.

<h2 id="Installation et exécution de Grav sur Azure">Installation et exécution de Grav sur Azure.
<a href="#Installation et exécution de Grav sur Azure" class="toc-anchor after"></a></h2>

<h3 id="Configuration de votre application Web">Configuration de votre application Web.
<a href="#Configuration de votre application Web" class="toc-anchor after"></a></h3>

* La première étape consiste à vous [connecter au portail Azure](https://portal.azure.com/), cliquez sur Créer une ressource dans le menu de gauche.

![Étape 1](https://learn.getgrav.org/user/pages/09.webservers-hosting/03.paas/04.azure/step1.png)

* Recherchez l'application Web et sélectionnez le service

![Étape 2](https://learn.getgrav.org/user/pages/09.webservers-hosting/03.paas/04.azure/step2.png)

* Un nouveau panneau s'ouvrira, décrivant le service Web App. Au bas de la page, vous trouverez un bouton de création, lorsque vous initiez qu'une autre lame s'ouvrira. Plusieurs questions vous seront posées.
    * Le nom de l'application fera partie de l'URL publique que votre site Web aura lors de sa première création,
    * L'abonnement est le plan dans lequel votre application Web sera hébergée et d'où proviendra le paiement du service
    * Un groupe de ressources au sein d'Azure est un moyen de regrouper logiquement vos services, le nom de ce groupe est privé et vous seul le verrez
    * Une application Web Azure peut s'exécuter sur une plateforme Windows, Linux ou Docker. Pour Grav sélectionnez Windows
    * Le plan de service d'application/l'emplacement détermine le centre de données dans lequel votre application Web résidera dans Azure et le coût de celui-ci
    * Application Insights est le service sur Azure qui peut vous aider à surveiller les problèmes de votre application Web et à comprendre comment vos utilisateurs finaux interagissent avec elle.

Ma recommandation concernant le plan App Service serait de sélectionner le plan Dev/Test F1 à des fins de test. Le plan a quelques limitations mais il vous donnera la possibilité de déployer votre premier site Grav sur Azure sans encourir de frais. En termes d'emplacement, je choisirais celui qui est proche de votre emplacement. Dans cet exemple également, j'éviterais de déployer Application Insights car il doit être codé pour s'intégrer à Grav.

![Étape 3](https://learn.getgrav.org/user/pages/09.webservers-hosting/03.paas/04.azure/step3.png)

Votre application Web devrait se déployer en quelques minutes.

<h3 id="Installer Composer">Installer Composer.
<a href="#Installer Composer" class="toc-anchor after"></a></h3>

Composer est un gestionnaire de dépendances pour PHP. Composer gérera les dépendances dont vous avez besoin projet par projet, ce qui signifie que Composer intégrera toutes les bibliothèques, dépendances et dépendances requises pour votre application. Comme Grav est une application PHP, nous devons nous assurer que Composer est installé sur l'application Web pour que Grav fonctionne correctement.

Pour ce faire, suivez ces étapes :

`- Ouvrez votre application Web`  
`- Cliquez sur le paramètre Extensions`  
`- Cliquez sur Ajouter`  
`- Sélectionnez Composer`  
`- Cliquez sur OK`

Une fois Composer installé sur votre application Web, vous êtes maintenant prêt à déployer votre code.

<h3 id="Déploiement de votre code">Déploiement de votre code.
<a href="#Déploiement de votre code" class="toc-anchor after"></a></h3>

Maintenant que votre application Web est opérationnelle et que vous avez le code, il est temps de la déployer. Pour ce faire, ouvrez l'application Web dans le portail Azure.

* Accédez au panneau *Options de déploiement*

![Étape 4](https://learn.getgrav.org/user/pages/09.webservers-hosting/03.paas/04.azure/step4.png)

* Sélectionnez GitHub comme source

* Il vous sera demandé des informations d'identification pour votre compte GitHub, puis des options sur le référentiel et la branche à extraire, sélectionnez les options qui vous conviennent.

* *Azure va maintenant commencer à extraire votre code de GitHub, dans quelques minutes, votre site devrait être en ligne*

<h3 id="Informations Complémentaires">Informations Complémentaires.
<a href="#Informations Complémentaires" class="toc-anchor after"></a></h3>

<h4 id="Domaine personnalisé">Domaine personnalisé.
<a href="#Domaine personnalisé" class="toc-anchor after"></a></h4>

Si vous souhaitez utiliser votre propre URL de site Web, veuillez suivre la [documentation officielle.](https://docs.microsoft.com/en-gb/azure/app-service/app-service-web-tutorial-custom-domain)

<h4 id="Toujours activé">Toujours activé.
<a href="#Toujours activé" class="toc-anchor after"></a></h4>

Par défaut, toutes les applications Web Azure sont déchargées si elles sont inactives pendant un certain temps. Il s'agit d'aider à préserver les ressources. Si vous avez sélectionné un plan de base ou standard, vous pouvez activer le mode *Always On*, qui gardera l'application chargée tout le temps. Le paramètre Toujours activé se trouve dans le panneau *Paramètres d'application* de votre application Web.

<h4 id="Quotas">Quotas.
<a href="#Quotas" class="toc-anchor after"></a></h4>

Si vous avez sélectionné l'un des plans d'application Web gratuite ou partagée pour votre déploiement, vous serez limité en ce qui concerne l'espace de stockage et les ressources de calcul que vous pouvez utiliser. Pour surveiller ces paramètres, vous devez surveiller le panneau Quotas.

