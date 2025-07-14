<h1 class="rem">Introduction ligne de commandes</h1>

Ce n'est pas un secret que Grav a été construit avec la ligne de commande à l'esprit. Bien que le plugin Admin facilite certainement la tâche sans ouvrir un terminal (MacOS et Linux) ou une invite de commande (Windows), il y a beaucoup à dire sur la vitesse et le niveau de contrôle qui accompagnent le travail à partir de la ligne de commande.

Cela est particulièrement vrai pour les personnes qui exécutent leurs propres serveurs de développement ou un serveur distant avec lequel ils ont la possibilité d'accéder à la ligne de commande. La quantité d'outils à votre disposition depuis la ligne de commande est incroyable. Vous pouvez contrôler pratiquement tous les aspects de l'hébergement de votre site, Grav, et ses plugins et thèmes avec une poignée de touches.

En fin de compte, tout se résume à des préférences personnelles. Sur cette page, nous énumérerons quelques excellentes ressources pour vous aider à vous familiariser avec la ligne de commande.

<div class="notice info">
Tous les systèmes d'exploitation ne sont pas compatibles les uns avec les autres en ce qui concerne les commandes. Il existe des différences mineures entre MacOS et de nombreuses distributions Linux, l'invite de commande de Windows ayant un ensemble de commandes très différent des deux autres.
</div>

<h2 id="Mac OS">Mac OS.
<a href="#Mac OS" class="toc-anchor after"></a></h2>

MacOS est basé sur Unix et est conforme aux normes POSIX. Cela signifie que la plupart des commandes que vous connaissez peut-être sur d'autres systèmes d'exploitation basés sur Unix ou Linux fonctionneront exactement comme prévu sous MacOS. Il existe quelques exceptions à la règle, et c'est pour cette raison que nous vous recommandons de rechercher les commandes Terminal pour le système d'exploitation spécifique avec lequel vous travaillez.

Voici quelques ressources intéressantes pour vous aider à vous habituer à utiliser le Terminal sous MacOS :

* [Guide des commandes de terminal MacOS de Michael Hogg](http://michael-hogg.co.uk/os_x_terminal.php) - Une ressource pratique pour les commandes de terminal compatibles MacOS, ce qu'elles font et comment les utiliser.
* [MacRumors Guide to Terminal](http://guides.macrumors.com/Terminal) - Une ressource utile pour naviguer et utiliser le Terminal, y compris des conseils pour l'utiliser avec l'interface graphique.
* [Envato Tuts + Terminal Trucs et astuces](http://computers.tutsplus.com/tutorials/40-terminal-tips-and-tricks-you-never-thought-you-needed--mac-51192) - 40 trucs et astuces astucieux pour maîtriser le terminal. Comprend des commandes que vous ne trouverez pas dans de nombreuses introductions de base.
* [Envato Tuts + Apprivoiser le terminal](http://computers.tutsplus.com/articles/new-mactuts-session-taming-the-terminal--mac-45471) - Un cours détaillé en plusieurs parties sur l'utilisation du terminal. Comprend des vidéos, des captures d'écran et plus encore.

<h2 id="Linux">Linux.
<a href="#Linux" class="toc-anchor after"></a></h2>

La grande majorité des distributions Linux (et Unix) ont un gros point commun : l'interface de ligne de commande Bash (Terminal). Que vous exécutiez ou non une interface graphique telle que Gnome, Unity ou KDE, il y a de fortes chances que si vous exécutez une distribution Linux sur votre ordinateur de bureau ou votre ordinateur portable, vous avez visité la ligne de commande.

Après tout, c'est puissant. Vous pouvez faire à peu près tout ce que vous pouvez avec l'interface graphique directement dans la ligne de commande, souvent avec plus de contrôle sur la façon dont les commandes sont exécutées. Voici quelques excellentes ressources pour vous aider à vous familiariser avec le Terminal sous Linux :

* [Guide du débutant de TechSpot sur la ligne de commande Linux](https://www.techspot.com/guides/835-linux-command-line-basics/) - Un excellent guide du débutant sur la ligne de commande.
* [Guide rapide de MakeUseOf pour démarrer avec la ligne de commande Linux](https://www.makeuseof.com/tag/a-quick-guide-to-get-started-with-the-linux-command-line/) - Une autre excellente ressource pour en savoir plus sur le terminal.
* [O'Reilly Linux DevCenter Répertoire des commandes Linux](http://www.linuxdevcenter.com/cmd/) - Un index des commandes disponibles dans le terminal.
* [Ryan's Tutorials Linux Tutorial](http://ryanstutorials.net/linuxtutorial/) - Un excellent guide tout-en-un sur Linux et l'interface de ligne de commande Bash (Terminal).

<h2 id="MWindows">Windows.
<a href="#Windows" class="toc-anchor after"></a></h2>

Windows se démarque du lot pour un certain nombre de raisons. De nombreuses commandes de la ligne de commande de Windows rappellent ses racines DOS. Les commandes courantes telles que `ls` pour une liste de répertoires ne fonctionnent pas ici. Au lieu de cela, vous taperiez `dir`. Voici quelques ressources pour vous aider à maîtriser l'invite de commande Windows :

* [Guide du débutant de MakeUseOf sur la ligne de commande Window](https://www.makeuseof.com/tag/a-beginners-guide-to-the-windows-command-line/)s - Une introduction bien écrite à la ligne de commande pour Windows.
* [DOSPrompt.info](http://dosprompt.info/) - Un site entier consacré à la familiarisation des utilisateurs avec l'invite de commande.

<div class="notice info">
Toutes les commandes CLI de Grav reposent sur PHP, mais ce n'est pas immédiatement disponible dans Windows. Vous pouvez savoir s'il est installé en ouvrant une console et en tapant `php -v` pour vérifier. Si `'php' n'est pas reconnu comme une commande interne ou externe ...` renvoie, ce n'est pas le cas.
</div>

Si vous souhaitez ajouter PHP à votre système Windows, vous devez trouver vos "Variables d'environnement", soit en les recherchant dans le menu Démarrer, soit en allant dans Panneau de configuration -> Paramètres système avancés -> Cliquez sur "Variables d'environnement" - bouton.

Sous "Variables système", recherchez "Chemin" et cliquez sur Modifier. Copiez la "valeur variable" dans le bloc-notes et ajoutez un point-virgule à la fin - pour séparer les variables. Trouvez ensuite le chemin d'accès à votre installation de PHP (à partir de zéro [from scratch](https://windows.php.net/) ou en utilisant une installation actuelle fournie avec votre environnement de développement), et ajoutez-le à la fin de cette longue liste de variables. Vous voulez le chemin du dossier, sans compter `php.exe`.

Lorsque cela est fait, ouvrez une nouvelle console (ou redémarrez votre console actuelle) afin que le nouveau chemin soit appliqué. Ensuite, essayez à nouveau `php -v`, vous devriez obtenir une sortie comme : `PHP 7.0.7 (cli) ....` Lorsque vous exécutez les commandes de Grav, vous devrez leur ajouter `php`, par exemple `php grav/gpm index`.

<h2 id="Commandes spécifiques à Grav">Commandes spécifiques à Grav.
<a href="#Commandes spécifiques à Grav" class="toc-anchor after"></a></h2>

L'une des choses les plus intéressantes à propos de Grav est que vous disposez d'une multitude de commandes puissantes pour tout faire, de l'installation de plugins et de thèmes supplémentaires à l'ajout d'utilisateurs à l'administrateur. Dans cette section, nous énumérerons la plupart des commandes les plus couramment utilisées.

Toutes les commandes répertoriées ci-dessous sont compatibles avec **n'importe quel système d'exploitation**.

| **COMMANDE**                | **DESCRIPTION**
| ---------                   | ---------
| `bin/grav list`             | Liste toutes les commandes disponibles dans Grav (hors GPM).
| `bin/grav help <command>`   | Vous donne de l'aide sur une commande spécifique.
| `bin/grav new-project <location>`  | Utilisé pour créer une nouvelle instance Grav propre dans un dossier différent. Peut être<br> exécuté à partir d'une installation Grav existante.
| `bin/grav install`          | Cette commande installe toutes les dépendances nécessaires pour exécuter votre<br> installation actuelle de Grav.
| `bin/grav cache`            | Cette commande efface le cache de votre installation Grav. Les options incluent :<br> `--all`, `--assets-only`, `--images-only` et `--cache-only`.
| `bin/grav backup`           | Crée une sauvegarde zip de votre site Grav actuel.
| `bin/grav composer`         | Met à jour les packages de fournisseurs basés sur composer installés manuellement.
| `bin/grav security`         | Exécute les vérifications de sécurité XSS configurées sur toutes les pages Grav.
| `bin/gpm list`              | Liste toutes les commandes disponibles via le GPM (Grav Package Manager) de Grav.
| `bin/gpm help <command>`    | Vous donne de l'aide sur une commande spécifique.
| `bin/gpm index`             |Affiche une liste de toutes les ressources disponibles dans le référentiel Grav, organisées<br> par thèmes et plugins.
| `bin/gpm info`              | Affiche les détails du package souhaité, tels que la description, l'auteur, la page<br> d'accueil, etc.
| `bin/gpm install`           | Installe une ressource du référentiel sur votre instance Grav actuelle avec une simple<br> commande.
| `bin/gpm update`            | Vérifie les plugins et les thèmes installés pour les mises à jour disponibles et les<br> répertorie.
| `bin/gpm uninstall`         | Supprime un thème ou un plugin installé et vide le cache.
| `bin/gpm self-upgrade`      | Vous permet de mettre à jour Grav vers la dernière version.
| `bin/gpm logviewer`         | Affichez facilement les journaux Grav avec des options de configuration pour choisir le<br> fichier journal, le nombre de lignes et la verbosité.
| `bin/gpm scheduler`         | Gérez les tâches planifiées et exécutez manuellement le processus du planificateur si<br> nécessaire.

<div class="notice info">
Ces commandes sont expliquées plus en détail dans la documentation <a href="../cli-commandes-grav">Grav CLI</a> et <a href="../cli-commandes-gpm">Grav GPM</a>.
</div>

Les commandes listées ci-dessous sont compatibles avec les **systèmes mac ou unix**.

| **COMMANDE**                            | **DESCRIPTION**
| ---------                               | ---------
| `index bin/gpm \| grep '\| Installé'`   | Répertorie tous les plugins et thèmes que vous avez actuellement installés.

<h2 id="Liens symboliques">Liens symboliques.
<a href="#Liens symboliques" class="toc-anchor after"></a></h2>

Les liens symboliques (également appelés symlinks) sont incroyablement utiles et faciles à exécuter dans la ligne de commande. Ce qu'il fait, il crée une copie virtuelle (clone) d'un dossier donné ou de son contenu et le place où vous voulez qu'il aille. Contrairement à une copie conforme, il s'agit simplement d'un tunnel vers l'original, de sorte que tout ce que vous voyez et modifiez se reflète à plusieurs endroits à la fois.

Un autre grand avantage de cette opération est qu'elle ne prend pratiquement aucun espace disque supplémentaire puisque vous n'avez pas plusieurs copies des mêmes fichiers.

En ce qui concerne Grav, les liens symboliques sont un excellent moyen d'ajouter des plugins, des thèmes et du contenu à plusieurs instances et de le faire d'une manière qui facilite infiniment la mise à jour et la modification. Vous apportez une modification une fois et elle apparaît partout où le ou les fichiers sont liés symboliquement.

Le processus d'exécution d'un lien symbolique est assez simple, avec des différences mineures entre les systèmes d'exploitation.

<h3 id="Liens symboliques sous MacOS et Linux">Liens symboliques sous MacOS et Linux.
<a href="#Liens symboliques sous MacOS et Linux" class="toc-anchor after"></a></h3>

<img class=img" src="https://learn.getgrav.org/user/pages/07.cli-console/01.command-line-intro/osx_symlink.png" alt="Liens">

La commande suit un modèle commun de `ln -s <original file, directory, or its contents> <put virtual copies here>`.

Les commandes qui lancent un lien symbolique diffèrent selon les systèmes d'exploitation. Pour MacOS et la majorité des distributions Unix et Linux, `ln -s` est la commande. La partie `ln` indique au système que vous souhaitez créer un lien. Le commutateur `-s` définit le lien comme symbolique.

<h3 id="Liens symboliques sous Windows">Liens symboliques sous Windows.
<a href="#Liens symboliques sous Windows" class="toc-anchor after"></a></h3>

La structure de base de la commande dans Windows est `mklink <type> <put virtual copies here> <original file, directory, or its contents>`. Contrairement à MacOS ou Linux, vous devrez définir l'argument pour le type de fichier que vous liez symboliquement. La source et la destination sont également inversées dans ce cas, où le nouveau lien symbolique vient avant le fichier vers lequel vous créez un lien. Il y a trois arguments que vous pouvez utiliser ici :

* `/j` - C'est l'argument le plus couramment utilisé. Il crée un lien symbolique d'un répertoire.
* `/h` - Cela crée un lien symbolique pour un fichier spécifique.
* `/d` - Cela crée un lien symbolique ou un raccourci. Il est peu probable qu'il soit utilisé aux fins décrites ici.

<h3 id="Exemples de commandes">Exemples de commandes.
<a href="#Exemples de commandes" class="toc-anchor after"></a></h3>

Fondamentalement, vous indiquez la commande qui lance le lien symbolique, ce que vous liez symboliquement et où vous placez les copies virtuelles. Ci-dessous, nous avons des exemples détaillés de ces commandes :

<h4 id="Lier le contenu d'un dossier à un autre">Lier le contenu d'un dossier à un autre.
<a href="#Lier le contenu d'un dossier à un autre" class="toc-anchor after"></a></h4>

| **MacOS ET LINUX**             | **WINDOWS**
| ---------                      | ---------
| `ln -s ~/folder1 ~/folder2`    | ` mklink /J C:\folder2 C:\folder1`

Cette commande crée un lien symbolique qui prend le contenu initialement placé dans **folder1** et en place une copie liée symboliquement dans **folder2**. Si **folder2** n'existe pas déjà, il est créé avec cette commande.

<h4 id="Lier des dossiers entiers d'un endroit à un autre">Lier des dossiers entiers d'un endroit à un autre.
<a href="#Lier des dossiers entiers d'un endroit à un autre" class="toc-anchor after"></a></h4>

| **MacOS ET LINUX**             | **WINDOWS**
| ---------                      | ---------
| `ln -s ~/folder1 ~/folder2/`   | ` mklink /J C:\folder2\ C:\folder1`

Cette commande copie l'intégralité du répertoire **folder1** et le place à l'emplacement cible (dans ce cas **folder2**). Dans ce cas, **folder2** devrait déjà exister car il ne sera pas créé avec cette commande. Observez la barre oblique ou la barre oblique inverse à la fin lorsque vous spécifiez **folder2**.

<h4 id="Lier des fichiers individuels d'un endroit à un autre">Lier des fichiers individuels d'un endroit à un autre.
<a href="#Lier des fichiers individuels d'un endroit à un autre" class="toc-anchor after"></a></h4>

| **MacOS ET LINUX**                   | **WINDOWS**
| ---------                            | ---------
| `ln -s ~/folder1/file.jpg ~/folder2` | `mklink /H C:\folder2\ C:\folder1\file.jpg`

C'est une commande utile pour lier symboliquement des fichiers individuels. Ceci est particulièrement utile si vous avez des fichiers partagés entre plusieurs répertoires et que vous souhaitez les mettre à jour partout en même temps. Gardez à l'esprit que le fichier d'origine est la seule copie réelle, il doit donc rester là où il se trouve pour que tous les liens symboliques fonctionnent.

