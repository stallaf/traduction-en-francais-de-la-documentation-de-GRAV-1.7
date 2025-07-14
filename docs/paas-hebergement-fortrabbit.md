<h1 class="rem">Fortrabbit - Hébergement PHP</h1>

[Fortrabbit](http://www.fortrabbit.com/) - sécurisé comme fort knox, rapide comme un lapin - est un service d'hébergement cloud géré dédié à PHP. Il prend en charge un développement PHP moderne avec une infrastructure d'hébergement orientée micro-services - parfait pour Grav. Fortrabbit est une plate-forme en tant que service, donc un peu différente de l'hébergement traditionnel.

![Fortrabbit](https://learn.getgrav.org/user/pages/09.webservers-hosting/03.paas/01.fortrabbit/fortrabbit-website.png)

<h2 id="S'inscrire">S'inscrire.
<a href="#S'inscrire" class="toc-anchor after"></a></h2>

Pour vous inscrire à Fortrabbit, il vous suffit de vérifier votre adresse e-mail et de configurer un mot de passe.

<h2 id="Faites tourner une application">Faites tourner une application.
<a href="#Faites tourner une application" class="toc-anchor after"></a></h2>

Choisissez un préréglage ou configurez vous-même les mises à l'échelle d'un seul composant. Grav - sans plugins - ne nécessite pas d'énormes quantités de RAM. Grav n'a pas besoin d'une base de données MySQL — alors désélectionnez-la. Commencez avec le plus petit plan et augmentez-le si nécessaire.

Il existe également un **essai gratuit** - qui est complet mais limité dans le temps. Votre application sera détruite lorsque l'application sera terminée. Ensuite, vous pouvez commencer un nouvel essai. Vous pouvez également demander à prolonger une période d'essai.

<h2 id="Installer localement">Installer localement.
<a href="#Installer localement" class="toc-anchor after"></a></h2>

Commencez par [télécharger](https://getgrav.org/downloads) et décompresser le dernier Grav localement. Il décompresse dans le sous-dossier `grav`. Vous pouvez configurer votre localhost pour servir le site grav localement maintenant.

<h2 id="Déployer sur Fortrabbit">Déployer sur Fortrabbit.
<a href="#Déployer sur Fortrabbit" class="toc-anchor after"></a></h2>

Maintenant, vous pouvez le pousser. Accédez au dossier du projet et configurez-le avec votre télécommande Git sur Fortrabbit :

```console
$ | $ cd grav
$ | $ git init .
$ | $ git remote add fortrabbit git@deploy.eu2.frbit.com:your-app.git
```

Avant de valider quoi que ce soit, vous devez exclure le dossier `vendor/` et le dossier `cache/` . Créez le fichier `.gitignore` avec le contenu suivant :

```configuration
vendeur
cache/*
!cache/.gitkeep
```

Vous pouvez maintenant tout ajouter et tout pousser dans votre application :

```console
$ | $ git add -A
$ | $ git commit -m 'Initial'
$ | $ git push -u fortrabbit master
```

Terminé : le premier déploiement déclenche une installation de Composer à distance, ce qui peut prendre quelques minutes. Vous pouvez le regarder se construire dans le flux de sortie Git. Maintenant, votre site Grav est en ligne. Chaque application est livrée avec une URL d'application (votre-application.frb.io) que vous pouvez visiter dans le navigateur.

Répétez : créez votre thème et votre contenu localement et poussez-le simplement vers la branche principale de Fortrabbit pour le déployer. Le deuxième déploiement ne prendra que quelques secondes.

<h2 id="Lectures complémentaires">Lectures complémentaires.
<a href="#Lectures complémentaires" class="toc-anchor after"></a></h2>

Ce ne sont que les bases, visitez le [guide d'installation et de réglage de Fortrabbit Grav](http://help.fortrabbit.com/install-grav) pour en savoir plus sur les thèmes, les plugins et les bizarreries.

