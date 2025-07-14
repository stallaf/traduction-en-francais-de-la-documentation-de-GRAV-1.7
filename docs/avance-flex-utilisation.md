<h1 class="rem">Utilisation d'objets flexibles</h1>

**Flex Objects** est conçu pour être facile à utiliser. L'affichage de la collection et des groupes dans vos pages peut principalement être effectué à partir de modèles Twig.

<div class="notice note">
<strong>ASTUCE</strong> : Pour activer et afficher Flex Directory, veuillez lire <strong><a href="../avance-flex-intro">Activation d'un Répertoire</a></strong>.
</div>

<h2 id="Utilisation du type de page `flex-objects`">Utilisation du type de page <code>flex-objects</code>.
<a href="#Utilisation du type de page `flex-objects`" class="toc-anchor after"></a></h2> 

Affichez plusieurs répertoires dans votre page `directories/flex-objects.md` :

```console
title: Directories
flex:
  layout: default
  list:
  - contacts
  - services
---

# Directories
```

Vous pouvez également transmettre des paramètres distincts pour chaque répertoire :

```console
title: Directories
flex:
  layout: default
  directories:
    contacts:
      collection:
        title: '{{ directory.title }}'
        layout: default
        object:
          layout: list-default
      object:
        title: 'Contact: {{ object.first_name }} {{ object.last_name }}'
        layout: default
    services:
---
```

Affichez un seul répertoire dans votre page `contacts/flex-objects.md` :

```console
title: Contacts
flex:
  directory: contacts
  collection:
    title: '{{ directory.title }}'
    layout: default
    object:
      layout: list-default
  object:
    title: 'Contact: {{ object.first_name }} {{ object.last_name }}'
    layout: default
---
```

Affichez un seul objet dans votre page `my-contact/flex-objects.md` :

```console
title: Contact
flex:
  directory: contacts
  id: ki2ts4cbivggmtlj
  object:
    title: 'Contact: {{ object.first_name }} {{ object.last_name }}'
    layout: default
---

# Contacts
```

Par défaut, le type de page `flex-objects` prend deux paramètres d'URL, **directory** et **id**. Ils sont utilisés pour naviguer dans les répertoires. Voici des exemples d'URL :

```console
https://www.domain.com/directories/directory:contacts/id:ki2ts4cbivggmtlj

https://www.domain.com/contacts/id:ki2ts4cbivggmtlj
```

<div class="notice note">
<strong>ASTUCE</strong> : vous pouvez transmettre vos propres paramètres à l'intérieur de <code>flex</code> et les utiliser dans vos fichiers de modèle de collection et d'objet.
</div>

<h2 id="Rendu des collections et des objets">Rendu des collections et des objets.
<a href="#Rendu des collections et des objets" class="toc-anchor after"></a></h2> 

Les collections et les objets prennent en charge le rendu de leur sortie en HTML. La sortie peut être personnalisée avec deux paramètres : mise en page et contexte. La mise en page vous permet de définir des apparences personnalisées, par exemple, vous pouvez avoir une liste de cartes, puis une sortie plus détaillée pour les détails. Le contexte vous permet de passer des variables à utiliser dans les fichiers de modèle.

```console
{% render collection layout: 'custom' with { context_variable: true } %}

{% render object layout: 'custom' with { context_variable: true } %}
```

Voir la documentation plus détaillée : [Render Collection](avance-flex-collection.md#Rendu) et [Render Object](avance-flex-objets-flex.md#Rendu).

<h2 id="Notions de base sur les modèles">Notions de base sur les modèles.
<a href="#Notions de base sur les modèles" class="toc-anchor after"></a></h2> 

Les modèles Flex se trouvent dans le dossier `templates/flex `:

```yaml
templates/
  flex/
    contacts/
      collection/
        default.html.twig
      object/
        default.html.twig
```

Chaque type a deux dossiers, un pour le rendu de la collection et un pour le rendu de l'objet. Les fichiers à l'intérieur sont des mises en page, nommées d'après le nom du fichier. Dans notre exemple, nous avons une mise en page par `default` pour la collection et l'objet.

<h3 id="Modèle de collecte">Modèle de collecte.
<a href="#Modèle de collecte" class="toc-anchor after"></a></h3> 

Le modèle de collection `flex/contacts/collection/default.html.twig` est responsable du rendu de tous les objets de la collection. La sortie rendue est mise en cache par défaut. La clé de cache est définie par la collection et le contexte transmis à la méthode r`ender()`.

<div class="notice warning">
<strong>AVERTISSEMENT</strong> : si le contexte contient des valeurs non scalaires, la mise en cache sera désactivée. Essayez de garder le contexte aussi simple que possible !
</div>

Voici un exemple de Type de contacts :

```html
<div id="flex-objects">
  <div class="text-center">
    <input class="form-input search" type="text" placeholder="Search by name, email, etc" />
    <button class="button button-primary sort asc" data-sort="name">
      Sort by Name
    </button>
  </div>

  <ul class="list">
    {% for object in collection.filterBy({ published: true }) %}
      <li>
        {% render object layout: layout with { options: options } %}
      </li>
    {% endfor %}
  </ul>
</div>

<script>
    var options = {
        valueNames: [ 'name', 'email', 'website', 'entry-extra' ]
    };
    var flexList = new List('flex-objects', options);
</script>
```

<div class="notice note">
<strong>ASTUCE</strong> : Si le HTML rendu a un contenu dynamique, le cache de rendu peut être désactivé à partir du modèle Twig par <code>{% do block.disableCache() %}</code>.
</div>

<h3 id="Modèle d'objet">Modèle d'objet.
<a href="#Modèle d'objet" class="toc-anchor after"></a></h3> 

Le modèle d'objet `flex/contacts/object/default.html.twig` est responsable du rendu d'un seul objet. La sortie rendue est mise en cache par défaut. La clé de cache est définie par l'objet et le contexte transmis à la méthode `render()`.

<div class="notice warning">
<strong>AVERTISSEMENT</strong> : si le contexte contient des valeurs non scalaires, la mise en cache sera désactivée. Essayez de garder le contexte aussi simple que possible !
</div>

Voici un exemple de Type de contacts :

```html
<div class="entry-details">
    {% if object.website %}
        <a href="{{ object.website|e }}"><span class="name">{{ object.last_name|e }}, {{ object.first_name|e }}</span></a>
    {% else %}
        <span class="name">{{ object.last_name|e }}, {{ object.first_name|e }}</span>
    {% endif %}
    {% if object.email %}
        <p><a href="mailto:{{ object.email|e }}" class="email">{{ object.email|e }}</a></p>
    {% endif %}
</div>
<div class="entry-extra">
    {% for tag in object.tags %}
        <span>{{ tag|e }}</span>
    {% endfor %}
</div>
```

<div class="notice note">
<strong>ASTUCE</strong> : Si le HTML rendu a un contenu dynamique, le cache de rendu peut être désactivé à partir du modèle Twig par <code>{% do block.disableCache() %}</code>.
</div>

<h3 id="Dispositions personnalisées">Dispositions personnalisées.
<a href="#Dispositions personnalisées" class="toc-anchor after"></a></h3> 

En utilisant des mises en page personnalisées, vous pouvez créer une quantité infinie de vues différentes dans vos collections et vos objets.

Vous pouvez créer vos mises en page personnalisées en ajoutant simplement un nouveau fichier à côté du fichier `default.html.twig`. Le nom de base du fichier est le même que le nom de votre mise en page.

<div class="notice note">
<strong>ASTUCE</strong> : dans les mises en page de collection, il est recommandé d'appeler <code>{% render object layout: 'xxx' %}</code> au lieu de sortir les variables d'objet directement dans le modèle de collection.

