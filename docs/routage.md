<h1 class="rem">Routage</h1>

Comme nous l'avons déjà décrit dans la section d'ouverture [Page -> Dossiers](./pages.md#Dossiers), le **routage** dans Grav est principalement contrôlé par la structure de dossiers que vous utilisez lorsque vous créez le contenu de votre site.

Il existe certains scénarios où vous avez besoin de plus de flexibilité et Grav est livré avec une variété d'outils et d'options de configuration pour vous simplifier la vie à cet égard.

Imaginez que vous déplaciez votre site d'une autre plate-forme CMS vers Grav, vous avez plusieurs choix pour configurer votre nouveau site :

1. Essayez de répliquer les URL de votre ancien site en créant la structure de dossiers correspondante.
2. Créez votre nouveau site comme vous le souhaitez, puis demandez à votre serveur Web de "réécrire" les anciennes URL pour rediriger les clients vers les nouveaux emplacements.
3. Créez votre nouveau site comme vous le souhaitez et configurez Grav pour rediriger les clients des anciennes URL vers les nouveaux emplacements.

Il existe de nombreux autres cas d'utilisation dans lesquels vous souhaiterez peut-être que le site Grav réponde à des URL différentes de celles dictées par la structure des dossiers, et Grav dispose des fonctionnalités suivantes pour vous aider à atteindre vos objectifs.

<h2 id="Remplacements de routage et de redirection au niveau de la page">Remplacements de routage et de redirection au niveau de la page
<a href="#Remplacements de routage et de redirection au niveau de la page" class="toc-anchor after"></a></h2>

Comme indiqué dans la section [En-têtes -> Routes](./en-tete-frontmatter.md#Routes), vous pouvez fournir des options de routage explicites pour la **route par défaut** ainsi qu'un tableau **d'alias de route** :

```yaml
routes:
   default: '/my/example/page'
   canonical: '/canonical/url/alias'
   aliases:
      - '/some/other/route'
      - '/can-be-any-valid-slug'
```

Ceux-ci sont traités et mis en cache par page, et sont disponibles avec ce que nous appelons la **route brute** qui est la route basée sur les **slugs** de la hiérarchie des pages (c'est ainsi que Grav établit une route par défaut). Ainsi, même si vous fournissez des routes de page personnalisées, la **route brute** est toujours valide également.

Semblable au routage au niveau de la page, Grav prend également en charge les redirections au niveau de la page en spécifiant la page cible dans l'en-tête de la page. Voir la section [En-têtes -> Redirection](./en-tete-frontmatter.md#Redirection) pour plus de détails.

    redirect: '/some/custom/route[303]'

<h2 id="Routes et redirections au niveau du site">Routes et redirections au niveau du site
<a href="#Routes et redirections au niveau du site"
class="toc-anchor after"></a></h2>

Grav dispose d'un puissant mécanisme basé sur les regex pour gérer les **alias de route** et les **redirections** d'une page à une autre. Cette fonctionnalité est particulièrement utile lorsque vous migrez un site vers Grav et que vous souhaitez vous assurer que les anciennes URL fonctionneront toujours avec le nouveau site. Ceci est souvent mieux réalisé via des **règles de réécriture** à l'aide de votre serveur Web, mais il est parfois plus pratique et flexible de laisser Grav les gérer.

Ceux-ci sont gérés via la [Configuration du Site](./configuration.md#Configuration du site). Grav est livré avec un exemple `system/config/site.yaml` mais vous pouvez remplacer ou ajouter n'importe lequel de vos propres paramètres en éditant le fichier `user/config/site.yaml`.

<div class="notice info">
Toutes les règles de redirection s'appliquent sur le slug-path commençant après la partie langue (si vous utilisez des pages multilingues)
</div>

<div class="notice warning">
Vous devez échapper certains caractères dans tous les itinéraires que vous souhaitez faire correspondre. Ceci est particulièrement important à savoir si vous migrez un ancien site qui utilisait des liens contenant des extensions de fichiers héritées (par exemple <code>.php</code>) ou des paramètres d'URL (<code>?foo=bar`</code>). Dans ces exemples, le point et le point d'interrogation <strong>doivent être échappés</strong> comme <code>/index\.php\?foo=bar: '/new/location'</code>.
</div>

<h3 id="Alias ​​de route">Alias ​​de route
<a href="#Alias ​​de route" class="toc-anchor after"></a></h3>

<h4 id="Alias ​​simples">Alias ​​simples
<a href="#Alias ​​simples" class="toc-anchor after"></a></h4>

Le type d'alias le plus basique est un mappage direct un à un. Dans la section `routes:` du `site.yaml`, vous pouvez créer une liste de mappages pour indiquer l'alias et la route réelle à utiliser.

<div class="notice info">
Il est important de noter que ces alias ne sont utilisés que si aucune page valide n'est trouvée avec la route fournie.
</div>

    routes:
      /something/else: '/blog/focus-and-blur'

Si vous avez demandé une URL `http://mysite.com/something/else` et que ce n'était pas une page valide, la définition des itinéraires vous servirait en fait la page située à `/blog/focus-and-blur`, en supposant qu'elle existe. Cela ne **redirige** pas réellement l'utilisateur vers la page fournie, il affiche simplement la page lorsque vous demandez l'alias.

<div class="notice info">
L'indentation est la clé ici, sans elle, la redirection de route ne fonctionnera pas.
</div>

<h4 id="Alias ​​basés sur Regex">Alias ​​basés sur Regex
<a href="#Alias ​​basés sur Regex" class="toc-anchor after"></a></h4>

Un type plus avancé de redirection d'alias permet l'utilisation d'une **expression régulière** simple pour mapper une partie d'un alias sur une route. Par exemple, si vous aviez :

    routes:
       /another/(.*): '/blog/$1'

Cela acheminerait le caractère générique de l'alias vers la route, donc `http://mysite.com/another/focus-and-blur` afficherait en fait la page trouvée sur la route `/blog/focus-and-blur`. C'est un moyen puissant de mapper un ensemble d'URL à un autre. Idéal pour déplacer votre site de WordPress vers Grav :)

Vous pouvez également effectuer la correspondance pour capturer n'importe quel alias et le mapper à une route spécifique :

    routes:
      /one-ring/(.*): '/blog/sunshine-in-the-hills'

Avec cet alias de route, toute URL qui confirme le caractère générique : /`one-ring/to-rule-them-all`ou `/one-ring/is-mine.html` affichera toutes les deux le contenu de la page avec la route `/blog/sunshine-in-the-hills`.

Vous pouvez même être beaucoup plus créatif et mapper plusieurs éléments ou utiliser n'importe quelle syntaxe regex :

    routes:
      /complex/(category|section)/(.*): /blog/$1/folder/$2

Cela correspondrait et réécrirait ce qui suit :

    /complex/category/article-1      -> /blog/category/folder/article-1
    /complex/section/article-2.html  -> /blog/section/folder/article-2.html

Cet itinéraire ne correspondra à rien qui ne commence pas par `complex/catégory` ou `complex/section`. 

Pour plus d'informations, [Regexr.com](https://regexr.com/) est une ressource fantastique pour apprendre et tester les expressions régulières.

<h4 id="Redirections">Redirections
<a href="#Redirections" class="toc-anchor after"></a></h4>

L'autre option corollaire pour **router les alia**s est fournie par les **redirections**. Celles-ci sont similaires, mais plutôt que de conserver l'URL et de simplement servir le contenu de la route aliasée, Grav redirige en fait le navigateur vers la page mappée.

Il existe trois options de configuration au niveau du système qui affectent les redirections :

```yaml
pages:
   redirect_default_route: false
   redirect_default_code: 302
   redirect_trailing_slash: true
```

* `redirect_default_route` permet à Grav de rediriger automatiquement vers la route par défaut de la page.
* `redirect_default_code` vous permet de définir les codes de redirection HTTP par défaut :
    * **301** : redirection permanente. Les clients effectuant des demandes ultérieures pour cette ressource doivent utiliser le nouvel URI. Les clients ne doivent pas suivre automatiquement la redirection pour les requêtes POST/PUT/DELETE.
    * **302** : Rediriger pour une raison indéfinie. Les clients effectuant des demandes ultérieures pour cette ressource ne doivent pas utiliser le nouvel URI. Les clients ne doivent pas suivre automatiquement la redirection pour les requêtes POST/PUT/DELETE.
    * **303** : Rediriger pour une raison indéfinie. Typiquement, 'L'opération est terminée, continuez ailleurs.' Les clients effectuant des demandes ultérieures pour cette ressource ne doivent pas utiliser le nouvel URI. Les clients doivent suivre la redirection pour les requêtes POST/PUT/DELETE.
    * **307** : Redirection temporaire. La ressource peut revenir à cet emplacement ultérieurement. Les clients effectuant des demandes ultérieures pour cette ressource doivent utiliser l'ancien URI. Les clients ne doivent pas suivre automatiquement la redirection pour les requêtes POST/PUT/DELETE.
* L'option `redirect_trailing_slash` vous permet de rediriger vers une version slash non finale de l'URL actuelle

Par exemple :

```yaml
redirects:
   /jungle: '/blog/the-urban-jungle'
```

Vous pouvez également transmettre explicitement le code de redirection entre crochets `[]` dans le cadre de l'URL :

```yaml
redirects:
   /jungle: '/blog/the-urban-jungle[303]'
```

Si vous deviez pointer votre navigateur vers `http://mysite.com/jungle`, vous seriez en fait redirigé et vous vous retrouveriez sur la page : `http://mysite.com/blog/the-urban-jungle`.

Les mêmes fonctionnalités d'expression régulière qui existent pour les Alias de Route existent également pour les redirections. Par example:

```yaml
redirects:
   /redirect-test/(.*): /$1
   /complex/(category|section)/(.*): /blog/$1/folder/$2
```   

<h2 id="Masquer l'itinéraire d'origine">Masquer l'itinéraire d'origine
<a href="#Masquer l'itinéraire d'origine" class="toc-anchor after"></a></h2>

Lorsque vous définissez une certaine page comme page d'accueil de votre site via le fichier `system.yaml` :

```yaml
home:
   alias: '/home'
```

Vous dites effectivement à Grav d'ajouter une route de / comme alias pour cette page. Cela signifie que lorsque Grav demande la page pour l'URL `/`, il trouve la page que vous avez définie.

Cependant, Grav ne fait vraiment rien de spécial pour les pages qui se trouvent sous cette page d'accueil. Donc, si vous avez une page appelée `/blog` qui affiche une liste de vos articles de blog et que vous la définissez comme votre page d'accueil, cela fonctionnera comme prévu. Si toutefois, vous cliquez sur un article de blog qui se trouve sous le dossier `/blog`, l'URL peut être `/blog/my-blog-post`. C'est un comportement attendu, mais ce n'est peut-être pas ce que vous avez l'intention de faire. Il y a une nouvelle option disponible via le `system.yaml` qui vous permet de cacher ce `/blog` de niveau supérieur de la route si activé.

Vous pouvez activer ce comportement en basculant la valeur suivante :

```yaml
home:
  hide_in_urls: true
```
      
