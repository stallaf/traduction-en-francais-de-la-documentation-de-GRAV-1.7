<h1 class ="rem">Mise à niveau depuis Grav <1.6</h1>

<div class = "notice tip">
Ce guide a été testé pour Grav v1.2.0 et toutes les versions ultérieures.
</div>

<div class = "notice warning">
AVERTISSEMENT : La mise à niveau directe de Grav vers la dernière version fonctionne, mais n'est pas entièrement prise en charge et entraînera probablement la panne de votre site !
</div>

<h2 id="Les préparatifs">Les préparatifs.
<a href="#Les préparatifs" class="toc-anchor after"></a></h2>

Le moyen le plus simple de mettre à niveau les anciennes versions de Grav consiste à créer une copie de votre site sur un serveur Linux/Unix sur lequel **PHP 7.3** est installé et prend en charge la connexion SSH pour accéder aux commandes CLI. Ce guide fonctionne également avec le sous-système Linux pour Windows 10, mais si vous ne l'avez pas installé, vous devrez peut-être télécharger et renommer manuellement les packages de mise à jour.

**PHP 7.3** a été choisi car c'est la seule version de PHP qui peut être utilisée pour toutes les versions de Grav. Vous pouvez également utiliser PHP 7.1 ou 7.2, mais cela vous permet uniquement de mettre à jour Grav jusqu'à la v1.6.31. À ce stade, vous devrez passer à PHP 7.3 ou 7.4 avant de poursuivre le processus de mise à niveau. PHP 8 ne doit être utilisé qu'après la mise à niveau vers Grav 1.7 ou une version ultérieure.

Ce guide comprend des instructions pour mettre à jour **Grav** et les packages les plus couramment utilisés : **Problems, Error, Form, Email, Login et Admin**. Pour tous les autres plugins, assurez-vous qu'ils sont toujours maintenus et qu'ils prennent en charge à la fois Grav 1.6 et la dernière version de Grav. Au moment de la rédaction de ce guide, presque tous les plugins activement maintenus devraient entrer dans cette catégorie. Les plugins les plus sûrs sont ceux qui ont été mis à jour après la sortie de Grav 1.7.0 (18/01/2021) ou dont le fonctionnement dans Grav 1.7 a été confirmé.

Votre **thème** et tous les **plugins personnalisés ou non maintenus** nécessiteront plus de travail. Tout code personnalisé doit être vérifié pour s'assurer qu'il fonctionne toujours avec les versions actuelles de Grav et PHP. Il en va de même pour les fichiers **Markdown**, **YAML** et **twig** car les dernières versions des bibliothèques ont des correctifs qui détectent les erreurs, ce qui signifie généralement que l'analyse des fichiers endommagés échouera. Les nouvelles versions de Grav fournissent des outils pour vérifier ces fichiers, mais les vérifications ne détectent pas tous les problèmes, donc certains tests sont également nécessaires.

Liste de contrôle (en utilisant une console et Grav CLI):

* Faire une sauvegarde
* Déterminer la version Grav par `bin/gpm version grav`
* Vérifiez la version de PHP CLI par `php -v`, il devrait s'agir de **PHP 7.3** (>=7.3.6)
* Vérifiez la version du serveur PHP, elle devrait être la même que dans la CLI
* Répertoriez tous les plugins installés et classez-les (supportés, personnalisés, non maintenus)
* Faites de même avec votre thème

<h2 id="Étape vers Grav 1.6.31">Étape vers Grav 1.6.31.
<a href="#Étape vers Grav 1.6.31" class="toc-anchor after"></a></h2>

Cette partie suppose que vous avez déjà fait une copie de votre site et que les commandes CLI fonctionnent. Les utilisateurs Windows qui n'ont pas installé le sous-système Linux doivent télécharger les fichiers manuellement et les renommer.

Effectuez les commandes suivantes dans le dossier racine de votre site Grav (omettez le paramètre `-y` dans la commande GPM si vous utilisez Grav 1.2 ou une version inférieure) :

```console
$ | wget -q https://getgrav.org/download/core/grav-update/1.6.31 -O tmp/grav-update-v1.6.31.zip
$ |
$| bin/gpm direct-install -y tmp/grav-update-v1.6.31.zip
```

Grav peut également être mis à jour manuellement. Supprimez les dossiers suivants : `assets bin system vendor webserver-configs` et copiez/remplacez tous les fichiers du fichier zip de mise à jour Grav [trouvé à partir d'ici](https://getgrav.org/download/core/grav-update/1.6.31). Notez que les fichiers du fichier zip se trouvent dans un dossier `grav-update`.

Ensuite, nous devons mettre à jour les plugins de base.

```console
$ | wget -q https://getgrav.org/download/plugins/problems/2.0.3 -O tmp/grav-plugin-problems-v2.0.3.zip
$ | wget -q https://getgrav.org/download/plugins/error/1.7.1 -O tmp/grav-plugin-error-v1.7.1.zip
$ | wget -q https://getgrav.org/download/plugins/form/4.3.0 -O tmp/grav-plugin-form-v4.3.0.zip
$ | wget -q https://getgrav.org/download/plugins/email/3.1.0 -O tmp/grav-plugin-email-v3.1.0.zip
$ | wget -q https://getgrav.org/download/plugins/login/3.3.8 -O tmp/grav-plugin-login-v3.3.8.zip
$ | wget -q https://getgrav.org/download/plugins/admin/1.9.19 -O tmp/grav-plugin-admin-v1.9.19.zip
$ | 
$ | bin/gpm direct-install -y tmp/grav-plugin-problems-v2.0.3.zip
$ | bin/gpm direct-install -y tmp/grav-plugin-error-v1.7.1.zip
$ | bin/gpm direct-install -y tmp/grav-plugin-form-v4.3.0.zip
$ | bin/gpm direct-install -y tmp/grav-plugin-email-v3.1.0.zip
$ | bin/gpm direct-install -y tmp/grav-plugin-login-v3.3.8.zip
$ | bin/gpm direct-install -y tmp/grav-plugin-admin-v1.9.19.zip
```

Alternativement, les plugins peuvent être installés manuellement en supprimant simplement tous les fichiers dans `user/plugins/pluginnname` et en copiant les fichiers à partir du fichier zip. Notez que les fichiers du fichier zip se trouvent dans un dossier nommé de manière semi-aléatoire.

<h2 id="Mise à niveau vers Grav 1.7">Mise à niveau vers Grav 1.7.
<a href="#Mise à niveau vers Grav 1.7" class="toc-anchor after"></a></h2>

Exécutez les commandes CLI suivantes une par une et suivez leurs instructions :

```console
$ | bin/gpm self-upgrade
$ | bin/gpm update
```
Vous pouvez également mettre à jour d'autres plugins un par un vers la dernière version, mais veuillez ne le faire que si la dernière version du plugin prend en charge Grav 1.6. Le reste des plugins doit être désactivé si vous n'êtes pas sûr qu'ils fonctionneront. Ils peuvent être réactivés plus tard, lorsque vous testez le site.

<div class = "notice tip">
Si vous n'avez pas de plugins cassés, le panneau d'administration et le site devraient être pleinement opérationnels à ce stade.
</div>

<div class = "notice info">
<strong>AVERTISSEMENT</strong> : évitez de mettre à jour le plugin Grav ou Admin avant d'avoir lu <a href="../avance-grav-up-to-1-7">Mise à niveau vers Grav 1.7.</a> Vous risquez de casser à la fois votre site et votre panneau d'administration.
</div>


