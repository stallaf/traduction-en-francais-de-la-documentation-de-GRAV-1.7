<h1 class="rem">Exemple : Config Blueprints</h1>

Il est courant d'ajouter des options de configuration dans `site.yaml`, à afficher dans le contenu du site.

Pour rendre ces options configurables via le panneau d'administration, ajoutez des champs à `user/blueprints/config/site.yaml`. Par example:

```yaml
1  | @extends:
2  |     '@parent'
3  | 
4  | form:
5  |     fields:
6  |        content:
7  | 
8  |           fields:
9  |              myfield:
10 |                 type: text
11 |                 label: My Field
```

Ajoute le type d'entrée "Mon champ", en l'ajoutant à la section Contenu de la configuration du site.

Vous pouvez également ajouter de nouvelles sections entières, par exemple :

```yaml
1  | @extends:
2  |     '@parent'
3  | 
4  | form:
5  |     fields:
6  |        anothersection:
7  |           type: section
8  |           title: Another Section
9  |           underline: true
10 | 
11 |           fields:
12 |              myfield:
13 |                 type: text
14 |                 label: A label
15 |                 size: large
```
                    
