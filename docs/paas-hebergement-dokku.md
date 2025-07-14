<h1 class="rem">Dokku</h1>

Dokku est un "mini-Heroku" auto-hébergé basé sur Docker que vous pouvez exécuter sur n'importe quelle machine virtuelle (VM), locale ou distante. Les principaux avantages de son utilisation seraient :

* Possibilité d'utiliser les packs de construction Heroku, les [Procfiles](https://devcenter.heroku.com/articles/procfile) et d'autres éléments architecturaux
* Possibilité d'utiliser les fichiers de composition Docker
* Auto-hébergé, ce qui vous permet de contrôler le coût des machines virtuelles ou de les exécuter localement sans frais
* Utiliser Git comme mécanisme de déploiement
* Certificats SSL gratuits via le mécanisme Let's Encrypt intégré
* Exécutez plusieurs sites Grav sur une seule machine virtuelle avec Dokku

La première étape consiste à l'installer dans une nouvelle machine virtuelle, en exécutant l'une de ces distributions de système d'exploitation :

* Ubuntu x64 - Toute version actuellement prise en charge
* Debian 8.2+ x64
* CentOS 7 x64 (expérimental)
* Arch Linux x64 (expérimental)

Pour installer la dernière version stable, connectez-vous via Secure Shell (SSH) à votre VM et exécutez les commandes suivantes en tant qu'utilisateur ayant accès à root (sudo) :

    $ | wget https://raw.githubusercontent.com/dokku/dokku/v0.17.9/bootstrap.sh
    $ | sudo DOKKU_TAG=v0.17.9 bash bootstrap.sh

Une fois le script d'installation terminé, vous devez accéder à l'adresse IP ou au nom de domaine de votre machine virtuelle dans un navigateur Web pour terminer l'installation. L'écran vous demandera :

* Une clé SSH publique - Elle est utilisée comme jeton d'authentification pour les déploiements (comme le fait Github ou Gitlab)

*Astuce de pro : si vous utilisez **Vultr** ou **Digital Ocean**, vous pouvez ajouter la clé SSH à la VM à partir du tableau de bord et elle sera automatiquement ajoutée par Dokku.*

* Nom d'hôte - doit correspondre au nom d'hôte de la VM
* Choisissez entre l'option d'utiliser la dénomination de l'hôte virtuel ou les sous-dossiers pour les applications

*Le nommage de l'hôte virtuel est généralement préféré, mais vous pouvez utiliser l'un ou l'autre.*

Si vous suivez le chemin de dénomination de l'hôte virtuel, vous devez ajouter un domaine ou un sous-domaine à votre machine virtuelle, via un fournisseur de service de nom de domaine comme **Cloudflare** et également ajouter un sous-domaine générique de ce domaine ou sous-domaine.

Pour plus de simplicité, vous pouvez utiliser la structure de sous-dossiers et l'adresse IP de la VM comme nom d'hôte

Une fois l'installation terminée, dans le terminal de votre VM, créez une nouvelle application pour votre site Web Grav :

    $ | dokku apps:create my-grav-site

Revenons maintenant à votre ordinateur local.

Consultez l'exemple PHP "Getting Started" fourni par Heroku avec Git, dans la racine Web de votre machine locale, afin que vous puissiez tester le site localement avant de le déployer.

    $ | git clone https://github.com/heroku/php-getting-started.git your-folder

    $ | cd your-folder

Ajoutez une télécommande Git à votre serveur Dokku en procédant comme suit :

    $ | git remote add dokku dokku@your-vm-hostname-or-ip:my-grav-site

et

    $ | git push dokku master

Après le déploiement, vous devriez voir une sortie similaire à celle-ci (ceci provient d'une application Rails, c'est donc un peu différent mais fonctionne comme un exemple):

```console
$ | Counting objects: 231, done.
$ | Delta compression using up to 8 threads.
$ | Compressing objects: 100% (162/162), done.
$ | Writing objects: 100% (231/231), 36.96 KiB | 0 bytes/s, done.
$ | Total 231 (delta 93), reused 147 (delta 53)
$ | -----> Cleaning up...
$ | -----> Building ruby-getting-started from herokuish...
$ | -----> Adding BUILD_ENV to build environment...
$ | -----> Ruby app detected
$ | -----> Compiling Ruby/Rails
$ | -----> Using Ruby version: ruby-2.2.1
$ | -----> Installing dependencies using 1.9.7
$ |        Running: bundle install --without development:test --path vendor/bundle --binstubs $ |  vendor/bundle/bin -j4 --deployment
$ |        Fetching gem metadata from https://rubygems.org/...........
$ |        Fetching version metadata from https://rubygems.org/...
$ |        Fetching dependency metadata from https://rubygems.org/..
$ |        Using rake 10.4.2
$ | 
$ | ...
$ | 
$ | =====> Application deployed:
$ |        http://ruby-getting-started.dokku.me
```

À la fin de la sortie de déploiement, vous verrez l'URL de votre nouvelle application. Allez-y et visitez-le.

Vous devriez maintenant voir l'exemple de projet PHP. Maintenant que tout est défini, vous êtes prêt à continuer et à exécuter Grav au lieu du site d'exemple.

Tout d'abord, supprimez le dossier web/ dans votre dossier de site actuel.

Copiez-y les fichiers de votre site Grav, en vous assurant que vous copiez également le fichier caché `.htaccess`. Remplacez tous les fichiers qui s'y trouvaient déjà.

Ouvrez maintenant le `Procfile`. Il s'agit d'un fichier spécifique à Heroku. Changez la ligne en

    web: vendor/bin/heroku-php-apache2 ./

Vous devez vous assurer que le site fonctionne localement, avant de le télécharger sur Heroku, juste pour vous assurer qu'il n'y a pas d'erreurs.

Validez maintenant dans le référentiel avec : `git add . ; git commit -am 'Added Grav'`

Modifiez ensuite `composer.json` et ajoutez une commande de post-déploiement à la section des `scripts` :

```yaml
1 | "scripts": {
2 |    "compiler": [
3 |      "installation bin/grav",
4 |      "bin/gpm installer quark -y"
5 |    ]
6 | }
```

et validez-le dans le référentiel avec:

    $ | git add . ; git commit -am 'Add post deploy bin/grav install'

Puis lancez

    $ | git push dokku master

 et le site devrait être prêt à partir !

En raison de la nature éphémère du système de fichiers de Dokku, tous les plugins ou thèmes nécessaires doivent être ajoutés à `composer.json` comme ci-dessus et y être conservés afin qu'ils soient installés chaque fois que le site est poussé vers Heroku. Vous pouvez rendre l'instance Heroku persistante, mais ce n'est pas une bonne idée pour faire évoluer l'application à l'avenir. Par exemple, si vous avez besoin du plugin Admin et d'un thème, ajoutez-les dans composer :

```yaml
1 | "scripts": {
2 |   "compile": [
3 |     "bin/grav install",
4 |     "bin/gpm install admin -y",
5 |     "bin/gpm install awesome-theme-name-here -y"
6 |   ]
7 | }
```

**Remarque** : Un merci spécial à l'auteur du guide Heroku, car il a été utilisé comme base pour celui-ci en raison des similitudes entre Dokku et Heroku.   

