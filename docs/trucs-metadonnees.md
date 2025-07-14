<h1 class="rem">Métadonnées des blogs</h1>

Lorsque vous utilisez Grav comme plate-forme de blogs, vous souhaiterez inclure des métadonnées qui aident à remplir les descriptions et les images lorsque les gens partagent votre message sur les réseaux sociaux tels que Facebook, Twitter, etc.

Vous ajouterez ces informations dans la section [En-tête](/en-tete_frontmatter) de votre page Grav.

Dans la documentation, il y a une référence aux métadonnées que vous devez ajouter dans la section d'en-tête, sous [Meta Page Headers](/en-tete-frontmatter/#En-têtes de méta-page). Cependant, si vous êtes passé d'une plate-forme telle que WordPress où vous utilisez un plugin pour cela, vous ne réaliserez peut-être pas l'importance des métadonnées.

Au début de chacun de vos articles de blog, vous voudrez avoir ce qui suit :

```yaml
1  | ---
2  | title: Blog Post Title
3  | publish_date: Date the blog post will go live
4  | date: Date the blog post was written
5  | metadata:
6  |     'og:title': Blog Post Title
7  |     'og:type': article
8  |     'og:description': Description of what your blog post is covering.  This will be visible when people share your post on social media.
9  |     'og:url': The URL of the blog post
10 |     'og:site_name': The name of the overall site the blog post belongs to. 
11 |     'og:locale': The language your blog post is written in
12 |     'og:image': The image you reference here will be visible when shared on social media. 
13 |     'twitter:card' : The type of Twitter card that should be used. 
14 |     'twitter:site' : Your Twitter handle
15 |     'twitter:title' : Blog Post Title
16 |     'twitter:description' : Description of what your blog post is covering.  This will be visible when people share your post on social media.
17 |     'twitter:image' : The image you reference here will be visible when shared on social media. 
18 |     'twitter:creator': The twitter handle of the blog post author. 
19 | taxonomy:
20 |     category: [Blog post category]
21 |     tag: [Tag 1, Tag 2, Tag 3, Tag 4]
22 |    author: Author's name
23 | ---
```

En savoir plus sur les [cartes Twitter](https://developer.twitter.com/en/docs/tweets/optimize-with-cards/guides/getting-started.html)

En savoir plus sur [le protocole Open Graph](https://ogp.me/)

