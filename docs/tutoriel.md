<h1 class = "rem">Tutoriel de base</h1>

En supposant que vous avez [installé Grav](./installation.md) avec succès avec les instructions répertoriées dans le chapitre précédent, nous pouvons continuer et jouer un peu avec Grav pour vous mettre plus à l'aise.

Parce que Grav ne nécessite pas de base de données, il est assez facile de travailler avec, sans avoir à se soucier de causer des problèmes entre votre installation Grav et toute autre source de données importante. Si quelque chose ne va pas, vous pouvez généralement le récupérer très facilement.

<h2 id="Contenu de base">Contenu de base
<a href="#Contenu de base" class="toc-anchor after"></a></h2>

Tout d'abord, familiarisons-nous avec l'endroit où Grav stocke le contenu. Nous reviendrons plus en détail dans un [prochain chapitre](./structure-dossiers.md), mais pour le moment, vous devez savoir que tout notre contenu utilisateur est stocké dans le dossier `user/pages/` de votre installation Grav.

Actuellement, il y a deux dossiers dans le dossier pages, le premier s'appelle `01.home` et le second `02.typography`. La partie `01.` du dossier est facultative mais fournit quelques éléments qui peuvent être utiles.

Tout d'abord, il vous permet de définir expressément l'ordre de vos pages. Par exemple, `01` viendra avant `02`, mais `00` viendra avant `01`.

L'autre chose que fait la partie numérique du nom du dossier est d'informer explicitement Grav que cette page doit être visible dans le menu. Il est important de noter que la partie numérique jusqu'au `.` seront supprimés des URL.

<h2 id="Configuration de la page d'accueil">Configuration de la page d'accueil
<a href="#Configuration de la page d'accueil" class="toc-anchor after"></a></h2>

Il existe une option dans le fichier `user/config/system.yaml` qui définit l'emplacement de la **page d'accueil**, en d'autres termes, où Grav pointe lorsque vous référencez la racine de votre site : `http://votresite.com`.

Si vous examinez ce fichier de configuration dans votre installation, vous verrez qu'il pointe déjà vers l'alias de `/home`. Nous pouvons le laisser comme ça dans cet exemple.

<h2 id="Édition de pages">Édition de pages
<a href="#Édition de pages" class="toc-anchor after"></a></h2>

Les pages dans **Grav** sont composées dans la syntaxe **Markdown**. Markdown est une syntaxe de formatage de texte brut qu'un ordinateur peut facilement analyser et convertir en HTML. Il utilise des symboles de texte de base pour indiquer la présentation (par exemple, **gras**, *italique*, titres, listes, etc.), ce qui facilite l'écriture sans avoir besoin de connaître les complexités du HTML. Les avantages de Markdown incluent un taux d'erreur plus faible, la lisibilité, la facilité d'apprentissage et d'utilisation, etc.

Vous pouvez lire une [description détaillée de la syntaxe disponible](./syntaxe-markdown.md) avec des exemples dans la documentation, mais pour l'instant, suivez-nous.

Ouvrez la page d'accueil dans votre éditeur de texte. Le fichier qui contrôle la page d'accueil se trouve dans le dossier `user/pages/01.home/` et s'appelle `default.md`. Tout le contenu que vous créez sera créé dans le dossier `user/pages/` de votre installation Grav.

Lorsque vous modifiez la page dans un éditeur de texte, le contenu ressemblera à ceci :

```markdown
1  | ---
2  | titre : Accueil
3  | body_classes : titre-centre titre-h1h2
4  | ---
5  | # Dites bonjour à Grav !
6  | ## installation réussie...
7  |
8  | Toutes nos félicitations! Vous avez installé le 
   | **Base Grav Package**qui fournit une **page simple** et le 
   | thème **Quark** par défaut pour vous aider à démarrer.
9  | 
10 | !! Si vous voyez une **erreur 404** lorsque vous cliquez sur 
   | `Typography` dans le menu, veuillez vous reporter au 
   | [guide de dépannage] (https://learn.getgrav.org/troubleshooting/page-not-found).
```

Décomposons cela un peu pour que vous puissiez voir à quel point il est facile d'écrire dans Markdown. Les éléments entre les indicateurs `---` sont les [en-têtes de page](./en-tete-frontmatter.md), et ceux-ci sont écrits dans un format simple appelé [YAML](./avance-syntaxe-yaml.md) Ce bloc de configuration qui se trouve dans le fichier `.md` est communément appelé **YAML Front Matter**.

```yaml
1 | title: Home
2 | body_classes: title-center title-h1h2
```

Ce bloc définit la balise de titre HTML de la page (le texte que vous voyez dans l'onglet du navigateur). Vous pouvez également y accéder depuis vos thèmes via l'attribut `page.title`. Il existe quelques [en-têtes standard](./en-tete-frontmatter.md) qui vous permettent de configurer une variété d'options pour cette page. Un autre exemple est le `menu : Something` qui vous permet de remplacer le texte utilisé pour afficher le nom de la page dans un menu. Par défaut, Grav utilisera le titre pour la valeur du menu.

```console
1  | # Dites bonjour à Grav !
2  | ## installation réussie...
```

La syntaxe `#` ou `hashes` dans Markdown indique un titre. Un seul `#` avec un espace puis du texte est converti en un en-tête` <h1>` en HTML. `##` ou un double hash serait converti en une balise `<h2>`. Bien sûr, cela va jusqu'à la balise HTML valide `<h6>` qui, bien sûr, serait composée de six hashes : `###### Mon en-tête de niveau H6`.

```md
1 | Félicitations! Vous avez installé le **Base Grav Package** qui fournit une 
2 | **page simple** et le thème **Quark** par défaut  pour vous aider à démarrer.
```

Il s'agit d'un simple paragraphe qui aurait été enveloppé dans des balises `<p>` normales lors de sa conversion en HTML. Les marqueurs `**` indiquent un texte en gras ou `<strong>`, anciennement `<b>`, en HTML. Le texte en italique est indiqué en enveloppant le texte dans des marqueurs `_`.

```md
1 | !! Si vous voyez une **erreur 404** lorsque vous cliquez sur `Typography` dans le menu, 
2 | veuillez vous reporter au [guide de dépannage](https://learn.getgrav.org/troubleshooting/page-not-found)
```

Cette section utilise une fonctionnalité de markdown personnalisée fournie par le plugin `Markdown-notices` inclus. Cela vous permet de créer des notices simples en préfixant un paragraphe de texte avec un nombre de symbole `!` (point d'exclamation), à partir de `!` jusqu'à `!!!!`.

Cet aperçu devrait vous fournir quelques conseils clés pour écrire en Markdown, mais vous devriez consulter nos [explications plus détaillées](./syntaxe-markdown.md) pour obtenir une compréhension approfondie.

<div class = "notice info">
Assurez-vous d'enregistrer vos fichiers <code>.md</code> en tant que fichiers <code>UTF8</code>. Cela garantira qu'ils fonctionnent avec des caractères spéciaux spécifiques à la langue.
</div>

<h2 id="Ajouter une nouvelle page">Ajouter une nouvelle page
<a href="#Ajouter une nouvelle page" class="toc-anchor after"></a></h2>

Créer une nouvelle page est une affaire simple dans **Grav**. Suivez simplement ces étapes :

   1. Accédez à votre dossier de pages : `user/pages/` et créez un nouveau dossier. Dans cet exemple, nous utiliserons un [ordre par défaut explicite](./pages.md) et appellerons le dossier `03.mypage`.
   2. Lancez votre éditeur de texte, créez un nouveau fichier et collez-y l'exemple de code suivant :

```md
1  | ---
2  | titre : Ma nouvelle page
3  | ---
4  | # Ma nouvelle page !
5  |
6  | Ceci est le corps de **ma nouvelle page** et je peux 
7  | facilement utiliser la syntaxe _Markdown_ ici.
```

   3. Enregistrez ce fichier dans le dossier `user/pages/03.mypage/` sous `default.md`. Cela indiquera à **Grav** d'afficher la page en utilisant le modèle par **défaut** dans le thème actuel : `user/themes/quark/templates/default.html.twig`.
   4. C'est ça! Rechargez votre navigateur pour voir votre nouvelle page dans le menu en haut.

La page apparaîtra automatiquement dans le menu après l'élément de menu **"Typography"**. Si vous souhaitez modifier le nom qui s'affiche dans le menu, ajoutez : `menu : My Page` entre les tirets dans le frontmatter de la page.

**Félicitations**, vous avez maintenant créé avec succès une nouvelle page dans Grav. Vous pouvez faire beaucoup plus avec Grav, alors continuez à lire pour en savoir plus sur les capacités avancées et les fonctionnalités approfondies.

<div class = "notice info">
Si vous rencontrez des problèmes pour accéder à cette nouvelle page, il vous manque un fichier <code>.htaccess</code> (serveur Web Apache uniquement) ou vous devrez peut-être modifier la commande  <code>RewriteBase</code> dans le fichier <code>.htaccess</code>. Veuillez consulter le chapitre <a href="/depannage-404">Dépannage </a>pour plus d'informations.
</div>

