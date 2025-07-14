<h1 class="rem">htaccess</h1>

Grav est livré avec son propre fichier `.htaccess`. Ce fichier permet à Grav de fonctionner correctement et doit être conservé dans son dossier racine. Vous pouvez rencontrer des problèmes qui peuvent être résolus à l'aide du fichier `.htaccess`.

Apache est l'une des solutions de serveur les plus populaires disponibles aujourd'hui. C'est gratuit et largement disponible un peu partout. Malheureusement, Apache n'est pas parfait, et parfois le fichier `.htaccess` peut vous donner mal à la tête. Ne vous inquiétez pas, c'est presque toujours réparable.

<h2 id="Comment modifier .htaccess sous Windows et macOS">Comment modifier .htaccess sous Windows et macOS.
<a href="#Comment modifier .htaccess sous Windows et macOS" class="toc-anchor after"></a></h2>

Le fichier `.htaccess` est un fichier caché, ce qui signifie que par défaut, les utilisateurs de macOS et Windows ne pourront pas voir ce fichier dans le gestionnaire de fichiers (Finder) à moins qu'ils n'autorisent l'affichage des fichiers cachés.

Sous **macOS** :

1. **Terminal** ouvert.
2. Entrez `defaults write com.apple.finder AppleShowAllFiles YES` dans le **Terminal** et appuyez sur **Return**.
3. Entrez `killall Finder` dans le **Terminal` et appuyez sur **Return**.

Vous devriez maintenant pouvoir voir le fichier `.htaccess` dans le répertoire racine du dossier Grav décompressé. Vous pouvez remettre vos paramètres à leur état caché d'origine en répétant le processus et en saisissant `NO` à la fin de l'étape 2 au lieu de `YES`.

Sous **Windows 10** :

1. Ouvrez **l'Explorateur de fichiers**.
2. Sélectionnez l'onglet **Affichage**.
3. Cochez la case à côté des **éléments masqués**.

Décocher cette case masquera à nouveau ces fichiers cachés, ramenant **l'explorateur de fichiers** à son état par défaut.

<h2 id="Tester .htaccess">Tester .htaccess.
<a href="#Tester .htaccess" class="toc-anchor after"></a></h2>

Disons que vous allez dans votre navigateur et naviguez vers votre nouveau site Grav et... il n'y est pas ! Un gros message audacieux indiquant que `Not Found` est l'endroit où votre beau site Grav devrait être. Ce n'est pas un problème amusant à avoir, mais la solution pourrait être aussi simple que d'ajuster votre fichier `.htaccess`.

La première étape du dépannage des problèmes avec le fichier `.htaccess` doit être de s'assurer que le fichier est effectivement récupéré et utilisé par le serveur. Assurez-vous que le fichier se trouve dans le répertoire racine de votre site Grav où il devrait se trouver, et qu'il est correctement nommé `.htaccess` avec un point (.) devant.

Si le fichier est là, votre prochaine étape consiste à le tester et à vous assurer que votre serveur le récupère. Il s'agit d'un processus simple qui consiste à ajouter une seule ligne en haut du fichier.

Pour tester, ouvrez le fichier `.htaccess` dans un éditeur de texte. Ensuite, vous voudrez créer une nouvelle première ligne et placer le texte `Test.` Et enregistrer.

![Test HTACCESS](https://learn.getgrav.org/user/pages/11.troubleshooting/07.htaccess/test.png)

Cette erreur ne résout pas votre problème par elle-même, mais elle vous permet de savoir que le `.htaccess` dans le répertoire racine de votre site Grav est celui que votre serveur analyse.

Si vous ne recevez pas cette erreur, assurez-vous que le fichier se trouve dans le répertoire racine de votre site. Cela devrait être le fichier inclus avec l'installation originale de Grav. C'est l'une des raisons pour lesquelles nous vous recommandons de décompresser le répertoire Grav compressé et de déplacer ce répertoire là où vous voulez que votre site soit sur votre serveur, plutôt que de copier les fichiers et de les coller. Cela garantit que tous les fichiers et la structure de répertoires restent identiques, évitant ainsi des problèmes de ce type.

<h2 id="Dépannage d'un fichier .htaccess cassé">Dépannage d'un fichier .htaccess cassé.
<a href="#Dépannage d'un fichier .htaccess cassé" class="toc-anchor after"></a></h2>

Si rien n'a changé lorsque vous avez modifié le fichier .htaccess, vous devrez peut-être vous assurer que `.htaccess` est activé. Sinon, votre serveur ne le cherchera même pas en premier lieu.

Voici ce que vous pouvez faire :

Recherchez et ouvrez le fichier `httpd.conf` ou `apache.conf` dans un éditeur de texte. Sous Windows, ce sera probablement le Bloc-notes ou un éditeur de texte conçu pour le développement. Les traitements de texte peuvent ajouter des informations inutiles qui pourraient aggraver le problème.

Ensuite, vous voudrez rechercher la zone `Directory` du fichier. Il devrait y avoir un bloc de texte comme celui-ci :

```configuration
1 | #
2 |     # AllowOverride controls what directives may be placed in .htaccess files.
3 |     # It can be "All", "None", or any combination of the keywords:
4 |     #   Options FileInfo AuthConfig Limit
5 |     #
6 |     AllowOverride All
```

Si `AllowOverride` est défini sur `None` ou autre chose que `All`, vous devrez le changer en `All` et enregistrer. Cette modification nécessitera une réinitialisation de votre serveur Apache pour s'enregistrer.

Une fois que vous avez fait cela, donnez à votre site un autre test.

Nous avons également inclus des guides de dépannage pour vous aider si vous rencontrez une erreur de serveur interne [404](/depannage-404) ou [500](/depannage-500) lors de l'utilisation de Grav.

