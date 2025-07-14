<h1 class="rem">Installation du plugin</h1>

<h2 id="Installation !">Installation !
<a href="#Installation !" class="toc-anchor after"></a></h2> 

L'installation d'un plugin peut se faire de trois manières :

* La méthode d'installation **GPM** (Grav Package Manager) vous permet d'installer rapidement le plugin avec une simple commande de terminal.
* La méthode **manuelle** vous permet de le faire via un fichier zip.
* La méthode **d'administration** vous permet de le faire via le plugin d'administration.

Le plugin devrait vous permettre de connaître le **_NOM_ à utiliser** dans les étapes suivantes.

<h2 id="Installation GPM (Préférée)">Installation GPM (Préférée)
<a href="#Installation GPM (Préférée)" class="toc-anchor after"></a></h2> 

Pour installer le plugin via le [GPM](cli-commandes-gpm.md), via le terminal de votre système (également appelé la ligne de commande), accédez à la racine de votre installation Grav et entrez :

    bin/gpm install NAME

Cela installera le plugin dans votre répertoire `/user/plugins` dans Grav. Ses fichiers se trouvent sous `user/plugins/NAME`.

<h2 id="Installation manuelle">Installation manuelle
<a href="#Installation manuelle" class="toc-anchor after"></a></h2> 

Pour installer le plugin manuellement, téléchargez la version zip de ce référentiel et décompressez-la sous `user/plugins/`. Renommez ensuite le dossier en `NAME`.

Vous devriez maintenant avoir tous les fichiers du plugin sous `user/plugins/NAME`.

> REMARQUE : Ce plugin est un composant modulaire pour Grav qui peut nécessiter d'autres plugins pour fonctionner, veuillez consulter le fichier `user/plugins/NAME/blueprints.yaml` pour plus d'informations.

<h2 id="Plug-in d'administration "> Plug-in d'administration
<a href="#Plug-in d'administration" class="toc-anchor after"></a></h2>

Si vous utilisez le [plugin Admin](https://github.com/getgrav/grav-plugin-admin) et que le plugin est enregistré dans le référentiel Grav, vous pouvez installer le plugin directement en parcourant le menu `Plugins` et en cliquant sur le bouton `Add`.

