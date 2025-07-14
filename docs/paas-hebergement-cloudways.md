<h1 class="rem">Cloudways - Cloud géré pour l'hébergement PHP</h1>

[Cloudways](http://www.cloudways.com/) est une plate-forme cloud gérée pour les applications Web basées sur PHP. Sur Cloudways, vous pouvez choisir votre serveur parmi quatre fournisseurs de cloud : DigitalOcean, Vultr, Google Cloud Engine (GCE) et Amazon Web Services (AWS) pour y exécuter votre travail PHP. Il permet à l'utilisateur de lancer des serveurs cloud en quelques minutes pour le développement d'applications PHP. La gestion des serveurs cloud est le travail de Cloudways ; vous n'êtes responsable que de votre application Grav CMS.

Récemment, Cloudways a interviewé l'un des [principaux développeurs de Grav CMS Andy Miller](https://www.cloudways.com/blog/interview-andy-miller/).

![Cloudways](https://learn.getgrav.org/user/pages/09.webservers-hosting/03.paas/03.cloudways/cw-logo.png)

<h2 id="S'inscrire sur Cloudways">S'inscrire sur Cloudways.
<a href="#S'inscrire sur Cloudways" class="toc-anchor after"></a></h2>

Tout d''abord [créer un compte](https://platform.cloudways.com/signup) sur Cloudways en utilisant votre compte GitHub. Si vous ne souhaitez pas utiliser vos informations d'identification Grav CMS, vous pouvez créer un compte en utilisant une adresse e-mail. Après vous être inscrit sur Cloudways et avoir lancé une application PHP Stack, suivez ces étapes pour installer et exécuter Grav CMS sur votre serveur cloud :

<h2 id="Installer et exécuter Grav sur Cloudways">Installer et exécuter Grav sur Cloudways.
<a href="#Installer et exécuter Grav sur Cloudways" class="toc-anchor after"></a></h2>

Connectez-vous au terminal SSH et accédez au dossier public_html de votre application.

    $ | cd applications/<foldername>/public_html/

Accédez à la page de [téléchargement de Grav CMS](https://getgrav.org/downloads) et copiez le lien de téléchargement. Maintenant, allez dans le terminal et téléchargez-le en utilisant la commande suivante

    $ | wget https://github.com/getgrav/grav/releases/download/1.7.28/grav-admin-v1.7.28.zip

Après l'avoir téléchargé, décompressez le fichier.

    $ | unzip grav-admin-v1.7.28.zip

C'est ça! Grav CMS est prêt à être utilisé sur la plate-forme d'hébergement PHP Cloudways. Dirigez-vous vers l'URL de votre Application Staging et ajoutez /grav-admin à la fin de l'URL.

<h2 id="Maintenance et mise à jour de Grav sur Cloudways">Maintenance et mise à jour de Grav sur Cloudways.
<a href="#Maintenance et mise à jour de Grav sur Cloudways" class="toc-anchor after"></a></h2>

De temps en temps, vous pouvez rencontrer des problèmes avec Grav sur Cloudways en raison de leur structure d'autorisations inhabituelle. Pour réinitialiser les autorisations de vos fichiers, connectez-vous à votre compte Cloudways, accédez à votre application, accédez aux paramètres de l'application et cliquez sur [Réinitialiser l'autorisation](https://support.cloudways.com/using-the-reset-permissions-button-to-solve-permissions-denied-issues/). Cela peut résoudre tous les problèmes d'autorisations de fichiers liés à la mise en cache, à la connexion, aux mises à jour ou aux sauvegardes.

