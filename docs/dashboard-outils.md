<h1 class="rem">Outils</h1>

![Outils 1](https://learn.getgrav.org/user/pages/05.admin-panel/07.tools/tools.png)

Dans certains cas, vous souhaiterez peut-être télécharger un package de thème ou de plug-in qui ne se trouve pas actuellement dans le répertoire principal des plug-ins Grav. Vous avez peut-être un thème premium disponible dans un fichier zip, ou vous développez un plugin et souhaitez télécharger votre dernière version sur votre installation Grav pour le tester. Dans tous les cas, vous pouvez le faire via FTP, mais l'administrateur vous propose une solution encore plus simple.

À l'aide de l'outil d'**installation directe**, vous pouvez télécharger un package compressé directement sur votre installation Grav et le rendre disponible pour une utilisation en quelques secondes. Cela ne se limite pas aux plugins et aux thèmes. Vous pouvez même télécharger Grav lui-même de cette façon et mettre à niveau (ou rétrograder) en faisant cela également. Ceci est particulièrement utile pour les contributeurs Grav qui souhaitent tester leur travail facilement.

Comme pour tout processus d'installation, nous vous recommandons d'avoir une sauvegarde récente de votre installation Grav avant d'utiliser cet outil - surtout si vous prévoyez de l'utiliser pour modifier l'ensemble de votre installation Grav.

<h2 id="Téléchargement">Téléchargement
<a href="#Téléchargement" class="toc-anchor after"></a></h2>

![Outils 2](https://learn.getgrav.org/user/pages/05.admin-panel/07.tools/tools1.png)

La première méthode d'installation directe disponible est un téléchargement de fichier. Vous pouvez télécharger un package zip directement sur Grav à l'aide de cet outil. Sélectionnez simplement le bouton **Choisir Fichier** (ou faites glisser votre package zip vers le bouton avec certains navigateurs) et sélectionnez votre fichier de package local. Une fois que vous avez sélectionné votre fichier, appuyez simplement sur **Télécharger et Installer** pour installer votre package.

![Outils 3](https://learn.getgrav.org/user/pages/05.admin-panel/07.tools/tools1b.png)

Une fois votre package installé avec succès, vous serez accueilli par une alerte vous en informant. C'est tout ce qu'on peut en dire !

<h2 id="Emplacement distant">Emplacement distant
<a href="#Emplacement distant" class="toc-anchor after"></a></h2>

![Outils 4](https://learn.getgrav.org/user/pages/05.admin-panel/07.tools/tools1.png)

La deuxième méthode consiste à créer un lien direct vers un fichier de package. Par exemple, si vous avez un package hébergé sur un serveur distant, vous pouvez saisir l'URL de ce package dans le champ. Les liens de téléchargement conviviaux GPM tels que `https://getgrav.org/download/themes/bootstrap/1.6.0` devraient fonctionner correctement.

Par défaut, ces téléchargements sont limités aux liens officiels du référentiel GPM. Mais vous pouvez accéder à **Configuration > Système** et basculer l'option **GPM officiel uniquement** sur **Non** pour déverrouiller ce champ et activer les liens directs vers les packages zip qui ne sont pas des référentiels GPM officiels. Par exemple : `http://example.com/mypackage.zip`. Il s'agit d'une fonctionnalité avancée et ne doit être utilisée que dans les cas où vous avez correctement vérifié que le colis est sûr.

