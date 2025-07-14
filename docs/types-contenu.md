<h1 class="rem">Types de contenu</h1>

<h2 id="Type de contenu par défaut">Type de contenu par défaut
<a href="#Type de contenu par défaut" class="toc-anchor after"></a></h2>

Comme c'est généralement le cas avec la plupart des plates-formes Web, le type de contenu par défaut de Grav est **HTML**. Cela signifie que lorsqu'un utilisateur demande un itinéraire dans son navigateur, par exemple : `/blog/new-macbook-pros-soon`, parce qu'il n'y a pas d'extension de fichier, Grav suppose que vous demandez une page HTML. Si votre page a été définie par une page avec le nom de fichier `blog-item.md`, Grav recherche à son tour un modèle Twig appelé `blog-item.html.twig` pour afficher la page.

Si l'utilisateur demandait explicitement le type via `/blog/new-macbook-pros-soon.html`, Grav chercherait toujours ce même fichier `blog-item.html.twig`.

<h2 id="Autres types de contenu">Autres types de contenu
<a href="#Autres types de contenu" class="toc-anchor after"></a></h2>

Grav est une plate-forme flexible cependant, et peut réellement servir n'importe quel type de contenu que vous pourriez souhaiter (`xml, rss, json, pdf`, etc.), il vous suffit de fournir un moyen de le rendre de manière appropriée.

Si vous deviez demander une route avec une extension `.xml`, par exemple : `/blog.xml`, au lieu d'utiliser le modèle `blog.html.twig` habituel pour le rendre, Grav recherche un modèle appelé `blog.xml.twig`. Vous devez vous assurer que le modèle génère la structure XML appropriée.

<h3 id="Exemple avec des fichiers JSON">Exemple avec des fichiers JSON
<a href="#Exemple avec des fichiers JSON" class="toc-anchor after"></a></h3>

Un moyen particulièrement courant d'accéder aux fichiers consiste à utiliser une extension `.json`. Cela permet de demander des données via des fichiers JSON qui sont facilement traités par JavaScript.

Supposons que vous vouliez le **frontmatter** et le **contenu** d'une page particulière au format JSON, et que cette page a été définie dans un fichier appelé `item.md`. Tout ce que vous auriez à faire est de fournir un modèle Twig appelé `item.json.twig`. Vous pouvez le mettre dans le dossier `templates/` de votre thème, ou si vous utilisiez un plugin pour charger des modèles personnalisés, vous pouvez l'ajouter ici.

Le contenu de ce fichier `item.json.twig` pourrait ressembler à :

    {% set payload = {frontmatter: page.header, content: page.content}  %}
    {{ payload|json_encode|raw }}

Tout ce que fait ce fichier Twig est de créer un tableau avec l'en-tête de page comme **frontmatter** et le **contenu** de la page, puis utilise le filtre Twig `json_encode` pour l'encoder.

Lorsqu'un utilisateur demande l'url `/blog/new-macbook-pros-soon.json`, ce nouveau fichier Twig serait utilisé et la sortie envoyée serait au format :

```yaml
{
   "frontmatter":{
      "title":"New Macbook Pros Arriving Soon",
      "date": "14:23 08/01/2016",
      "taxonomy":{
         "category":[
            "blog"
         ],
         "tag":[
            "apple",
            "mbpr",
            "laptops"
         ]
      }
   },
   "content":"<p>this has an -&gt; arrow here and <strong>bold</strong> here</p>\n<blockquote>
       \n<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec ultricies tristique nulla et
       mattis. Phasellus id massa eget nisl congue blandit sit amet id ligula. Praesent et nulla eu augue 
       tempus sagittis. Mauris faucibus nibh et nibh cursus in vestibulum sapien egestas. Curabitur ut lectus 
       tortor. Sed ipsum eros, egestas ut eleifend non, elementum vitae eros.\n-- <cite> Ronald Wade</cite>
       </p>\n</blockquote>\n<p>Mauris felis diam, pellentesque vel lacinia ac, dictum a nunc. Mauris mattis 
       nunc sed mi sagittis et facilisis tortor volutpat. Etiam tincidunt urna mattis erat placerat placerat 
       ac eu tellus.</p>\n<p>This is a new paragraph</p>\n<p>Lorem ipsum dolor sit amet, consectetur 
       adipiscing elit. Donec ultricies tristique nulla et mattis.</p>"
}
```

Il s'agit d'un JSON valide qui peut facilement être analysé et traité par JavaScript. Peasy facile!

<h2 id="Types de contenu personnalisés">Types de contenu personnalisés
<a href="#Types de contenu personnalisés" class="toc-anchor after"></a></h2>

Afin d'envoyer les données avec le type de contenu approprié, Grav doit connaître le type MIME que le navigateur attend pour qu'il restitue ce type de contenu. Grav connaît la plupart des types de contenu standard tels que définis dans le fichier `system/config/media.yaml`. Si vous souhaitez manipuler un type de contenu qui n'est pas fourni, il vous suffit d'ajouter une entrée dans ce fichier.

Par exemple, si vous souhaitez pouvoir restituer les événements du calendrier iCal, vous devez ajouter ce type de média au `media.yaml` :

```yaml
ics:
   type: iCal
   thumb: media/thumb.png
   mime: text/calendar
```

Cela définit l'extension de fichier `.ics` comme un fichier` iCal` avec le type mime : `texte/calendar`. Ensuite, tout ce que vous avez à faire est de fournir le modèle `.ical.twig` approprié pour rendre tout fichier de ce type que vous demandez.

