<h1 class="rem">Héroku</h1>

Heroku est un hébergeur très connu pour les applications web. Il a un plan gratuit utile à des fins de test et des options payantes pour déployer le site Web.

Il offre une grande variété d'addons et c'est l'un des PAAS les plus flexibles du marché.

Ils sont compatibles avec PHP, et ils ont un excellent guide "Démarrer avec PHP sur Heroku" à [https://devcenter.heroku.com/articles/getting-started-with-php#introduction](https://devcenter.heroku.com/articles/getting-started-with-php#introduction), et ce sera la base du jeu d'instructions.

Voyons comment installer Grav sur Heroku.

Tout d'abord, inscrivez-vous à Heroku.

Téléchargez la [CLI Heroku](https://devcenter.heroku.com/articles/heroku-cli), qui est un utilitaire de ligne de commande nécessaire pour déployer, créer et déployer votre site.

Une fois installé, tapez

    $ | heroku login

Entrez vos informations d'identification.

Maintenant, consultez l'exemple PHP "Getting Started" qu'ils fournissent dans votre racine Web locale, afin que vous puissiez tester localement le site avant de le déployer.

    $ | git clone https://github.com/heroku/php-getting-started.git your-folder

    $ | cd your-folder

Déployez maintenant votre application avec

    $ | heroku create

et

    $ | git push heroku master

Assurez-vous qu'au moins une instance de l'application est en cours d'exécution :

    $ | heroku ps:scale web=1

et ouvrez le site dans le navigateur :

    $ | heroku open

Vous devriez maintenant voir l'exemple de projet PHP. Maintenant que tout est défini, vous êtes prêt à continuer et à exécuter Grav au lieu du site d'exemple.

Tout d'abord, supprimez le dossier web/ dans votre dossier de site actuel.

Copiez-y les fichiers de votre site Grav, en vous assurant que vous copiez également le fichier caché `.htaccess`. Remplacez tous les fichiers qui existaient.

Ouvrez maintenant le fichier `Profile`. Il s'agit d'un fichier spécifique à Heroku. Changez la ligne en

    web: vendor/bin/heroku-php-apache2 ./

Vous devez vous assurer que le site fonctionne localement, avant de le télécharger sur Heroku, juste pour vous assurer qu'il n'y a pas d'erreurs.

Validez maintenant dans le référentiel avec

`git add . ; git commit -am 'Added Grav'`

Ensuite, modifiez `composer.json` et ajoutez la commande de post-déploiement à la section des `scripts` comme dans

```yaml
1 | "scripts": {
2 |    "compiler": [
3 |      "installation bin/grav",
4 |      "bin/gpm installer quark -y"
5 |    ]
6 | }
```

et validez-le dans le référentiel avec

    $ | git add . ; git commit -am 'Add post deploy bin/grav install'

Puis lancez

    $ | git push heroku master

et le site devrait être prêt à partir !

En raison de la nature éphémère du système de fichiers de Heroku, tous les plugins ou thèmes nécessaires doivent être ajoutés à `composer.json` comme ci-dessus et y être conservés afin qu'ils soient installés chaque fois que le site est envoyé à Heroku. Par exemple, si vous avez besoin du plugin `admin` et d'un thème, ajoutez-les dans composer comme dans

```yaml
1 | "scripts": {
2 |   "compile": [
3 -     "php bin/grav install",
4 |     "php bin/gpm install admin -y",
5 |     "php bin/gpm install awesome-theme-name-here -y"
6 |   ]
7 | }
```

