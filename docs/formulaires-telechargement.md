<h1 class="rem">Comment ajouter un téléchargement de fichier</h1>

<h2 id="Téléchargements de fichiers">Téléchargements de fichiers
<a href="#Téléchargements de fichiers" class="toc-anchor after"></a></h2>

Vous pouvez ajouter une fonctionnalité de téléchargement de fichiers dans les plans Pages, Config, Plugins et Thèmes. Les téléchargements de fichiers sont toujours basés sur Ajax et permettent le glisser-déposer depuis le bureau ou de les sélectionner en tant que champs de fichiers normaux. Chaque fois qu'un fichier est ajouté au champ, il est automatiquement téléchargé dans un dossier temporaire et ne sera stocké que lorsque l'action Enregistrer (ou Soumettre) aura lieu.

Exemple d'utilisation :

```yaml
1 | custom_file:
2 |      name: myfile
3 |      type: file
4 |      label: A Label
5 |      destination: 'plugins://my-plugin/assets'
6 |      multiple: true
7 |      autofocus: false
8 |      accept:
9 |            - image/*
```

<div class="notice note">
Afin d'ajouter un téléchargement de fichier, vous devez avoir une commande de rendu javascript en bas dans votre modèle Twig de base. <code>{{ assets.js('bottom') }}</code>
</div>

<h2 id="Options">Options
<a href="#Options" class="toc-anchor after"></a></h2>

Un champ de fichier a plusieurs options disponibles, du type ou de l'extension MIME accepté à la taille de fichier autorisée :

<h3 id="Valeurs par défaut">Valeurs par défaut
<a href="#Valeurs par défaut" class="toc-anchor after"></a></h3>

```yaml
1  | custom_file:
2  |     type: file
3  |     label: A Label
4  |     multiple: false
5  |     destination: 'self@'
6  |     random_name: false
7  |     avoid_overwriting: false
8  |     limit: 10
9  |     accept:
10 |           - image/*
```

<id class="code-gras">multiple
<a href="#multiple" class="toc-anchor after"></a></id>

    multiple: false # [false | true]

Comme un champ de fichier HTML5 normal, lorsque l'option `multiple` est activée, elle permet de télécharger plus d'un seul fichier. Ce paramètre est également lié à l'option `limit`, qui détermine le nombre de fichiers multiples autorisés pour le champ.
<br>

<id class="code-gras">destination
<a href="#destination" class="toc-anchor after"></a></id>

    destination: 'self@' # [<path> | <stream> | self@ | page@:<path>]

La destination est l'emplacement où les fichiers téléchargés doivent être stockés. Cela peut être soit un `path` normal (relatif à la racine de Grav), un `stream` (tel que `theme://images`), `self@` ou le préfixe spécial `page@:` .

<div class="notice info">
<code>self@</code> n'est pas autorisé en dehors de la portée Pages ou Flex Objects, une erreur sera renvoyée. Si vous utilisez un champ de fichier en dehors d'une page ou d'un objet Flex, vous devez toujours modifier le paramètre de <code>destination</code>.
</div>

<h4 id="Exemples">Exemples
<a href="#Exemples" class="toc-anchor after"></a></h4>

   1. Si vous souhaitez télécharger des fichiers dans un dossier `testing` de plug-in (`user/plugins/testing`), la destination serait :

```yaml
destination : 'plugins://testing'
```

   2. En supposant que nous ayons un article de blog sur la route `/blog/ajax-upload` (l'emplacement physique étant `user/pages/02.blog/ajax-upload`), avec le préfixe `page@` : la destination serait :

```yaml
destination : 'page@:/blog/ajax-upload'
```

   3. En supposant que le thème actuel est `antimatter` et que nous voulons télécharger dans le dossier assets (l'emplacement physique étant `user/themes/antimatter/assets`), avec le flux de thème, la destination serait :

```yaml
destination : 'theme://assets'
```
<br>

<id class="code-gras">random_name
<a href="#random_name" class="toc-anchor after"></a></id>

    random_name: false # [false | true]

Lorsque le `random_name` est activé, le fichier téléchargé sera renommé avec une chaîne aléatoire de **15** caractères. Ceci est utile si vous souhaitez hacher vos fichiers téléchargés ou si vous cherchez un moyen de réduire les collisions de noms.

<h4 id="Exemple2">Exemple
<a href="#Exemple2" class="toc-anchor after"></a></h4>

    'my_file.jpg' => 'y5bqsGmE1plNTF2.jpg'
<br>

<id class="code-gras">avoid_overwritting
<a href="#avoid_overwritting" class="toc-anchor after"></a></id>

    avoid_overwriting: false # [false | true]

Lorsque `avoid_overwritting` est activé et qu'un fichier portant le même nom que celui téléchargé existe déjà dans la `destination`, il sera renommé. Le fichier nouvellement téléchargé sera précédé de la date et de l'heure actuelles, concaténées par un tiret.

<h4 id="Exemple3">Exemple
<a href="#Exemple3" class="toc-anchor after"></a></h4>

    'my_file.jpg' => '20160901130509-my_file.jpg'
<br>

<id class="code-gras">limit
<a href="#limit" class="toc-anchor after"></a></id>

    limit: 10 # [1...X | 0 (unlimited)]

Lorsque le paramètre `multiple` est activé, `limit` permet de restreindre le nombre de fichiers autorisés pour un champ individuel. Si `multipl`e n'est pas activé (non activé par défaut),  `limit` retombe automatiquement à **1**.

Lorsque la limite est définie sur **0**, cela signifie qu'il n'y a aucune restriction sur le nombre de fichiers autorisés pouvant être téléchargés.

<div class="notice info">
Il est recommandé de toujours s'assurer que vous disposez d'une limite définie de fichiers autorisés pouvant être téléchargés. De cette façon, vous avez plus de contrôle sur l'utilisation des ressources de votre serveur.
</div>

<id class="code-gras">accept
<a href="#accept" class="toc-anchor after"></a></id>

```yaml
1 | accept:
2 |      - 'image/*' # Array of MIME types and/or extensions. ['*'] for allowing any file. .
```

Le paramètre `accept` autorise un tableau de type MIME ainsi que des définitions d'extensions. Toutes les extensions doivent commencer par . (point) plus l'extension elle-même.

De plus, vous pouvez également autoriser n'importe quel fichier en utilisant simplement la notation * (étoile) `accep : ['*']`.

<h4 id="Exemple4">Exemple
<a href="#Exemple4" class="toc-anchor after"></a></h4>

1.Pour autoriser uniquement les fichiers `yaml` et `json `:

```yaml
1 | accept:
2 |      - .yaml
3 |      - .json
```

2.Pour autoriser uniquement les images et les vidéos :

```yaml
1 | accept:
2 |      - 'image/*'
3 |      - 'video/*'
```

3.Pour autoriser n'importe quelle image, n'importe quelle vidéo et uniquement les fichiers mp3 :

```yaml
1 | accept:
2 |      - 'image/*'
3 |      - 'video/*'
4 |      - .mp3
```

4.Pour autoriser n'importe quel fichier :

```yaml
1 | accept:
2 |      - '*'
```
 <br>
 
<id class="code-gras">filesize
<a href="#filesize" class="toc-anchor after"></a></id>

La taille maximale du fichier est limitée par :

1. `filesize` au niveau du champ :, puis ...

2. Fichiers de configuration au niveau du plug-in de formulaire `user/plugins/form.yaml` : `files: filesize:` , alors si aucun de ceux-ci n'est limitatif...

3. Configuration du niveau PHP pour `upload_max_filesize` pour les fichiers individuels qui sont téléchargés, et `post_max_size` pour la taille maximale de la publication du formulaire.

<h4 id="Exemple5">Exemple
<a href="#Exemple5" class="toc-anchor after"></a></h4>

1.Pour limiter un champ spécifique à `5M`

```yaml
1 | custom_file:
2 |      name: myfile
3 |      type: file
4 |      label: A Label
5 |      destination: 'plugins://my-plugin/assets'
6 |      filesize: 5
7 |      accept:
8 |            - image/*
```

2.Pour limiter tous les champs de fichier à `5 Mo`, modifiez votre fichier `user/config/form.yaml` :

```yaml
1 | files:
2 |      multiple: false
3 |      limit: 10
4 |      destination: 'self@'
5 |      avoid_overwriting: false
6 |      random_name: false
7 |      filesize: 5
8 |      accept:
9 |            - 'image/*
```
   
