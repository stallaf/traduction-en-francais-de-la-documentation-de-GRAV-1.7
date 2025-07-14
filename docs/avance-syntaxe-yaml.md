<h1 class="rem">Syntaxe YAML</h1>

<h2 id="Introduction">Introduction.
<a href="#Introduction" class="toc-anchor after"></a></h2>

YAML signifie *"YAML Ain't Markup Language"* et il est largement utilisé dans Grav pour ses fichiers de configuration, ses plans et également dans les paramètres de page.

YAML est à la configuration ce que le démarquage est au balisage. Il s'agit essentiellement d'un format de données structuré lisible par l'homme. Il est moins complexe et disgracieux que XML ou JSON, mais offre des fonctionnalités similaires. Il vous permet essentiellement de fournir des paramètres de configuration puissants, sans avoir à apprendre un type de code plus complexe comme CSS, JavaScript et PHP.

YAML est conçu dès le départ pour être simple à utiliser. À la base, un fichier YAML est utilisé pour décrire les données. L'un des avantages de l'utilisation de YAML est que les informations contenues dans un seul fichier YAML peuvent être facilement traduites dans plusieurs types de langues.

Fondamentalement, les données que vous entrez dans un fichier YAML sont utilisées conjointement avec une bibliothèque pour créer les pages que vous voyez dans Grav.

<h2 id="Règles de base de YAML">Règles de base de YAML.
<a href="#Règles de base de YAML" class="toc-anchor after"></a></h2>

YAML a mis en place certaines règles pour éviter les problèmes liés à l'ambiguïté par rapport à divers langages et programmes d'édition. Ces règles permettent à un seul fichier YAML d'être interprété de manière cohérente, quelle que soit l'application et/ou la bibliothèque utilisée pour l'interpréter.

* Les fichiers YAML doivent se terminer par `.yaml` dans la mesure du possible dans Grav.
* YAML est sensible à la casse.
* YAML n'autorise pas l'utilisation d'onglets.

<h2 id="Types de données de base">Types de données de base.
<a href="#Types de données de base" class="toc-anchor after"></a></h2>

YAML excelle dans le travail avec les **mappages** (hachages/dictionnaires), les **séquences** (tableaux/listes) et les **scalaires** (chaînes/nombres). Bien qu'il puisse être utilisé avec la plupart des langages de programmation, il fonctionne mieux avec les langages construits autour de ces types de structure de données. Cela inclut : PHP, Python, Perl, JavaScript et Ruby.

<h3 id="Scalaires">Scalaires.
<a href="#Scalaires" class="toc-anchor after"></a></h3>

Les scalaires sont un concept assez basique. Ce sont les chaînes et les nombres qui composent les données de la page. Un scalaire peut être une propriété booléenne, telle que `true`, un entier (nombre) tel que `5`, ou une chaîne de texte, telle qu'une phrase ou le titre de votre site Web.

Les scalaires sont souvent appelés variables en programmation. Si vous faisiez une liste de types d'animaux, ce seraient les noms donnés à ces animaux.

La plupart des scalaires ne sont pas entre guillemets, mais si vous tapez une chaîne qui utilise la ponctuation et d'autres éléments qui peuvent être confondus avec la syntaxe YAML (tirets, deux-points, etc.), vous pouvez citer ces données en utilisant des guillemets simples " ou doubles ". Double les guillemets vous permettent d'utiliser des échappements pour représenter les caractères ASCII et Unicode.

```yaml
1 | integer: 25
2 | string: "25"
3 | float: 25.0
4 | boolean: true
```

<div class = "notice note">
<strong>ASTUCE</strong> : les mots <code>true</code>, <code>false</code>, <code>null</code>, <code>~</code> et les dates ont une signification particulière dans YAML. Veuillez les citer si vous ne souhaitez pas les utiliser comme type booléen, nul ou date/heure. Il en va de même pour les numéros de version, ils doivent être entre guillemets pour les séparer des valeurs flottantes.
</div>

<h3 id="Séquences">Séquences.
<a href="#Séquences" class="toc-anchor after"></a></h3>

Voici une séquence simple que vous pourriez trouver dans Grav. Il s'agit d'une liste de base dans laquelle chaque élément de la liste est placé sur sa propre ligne avec un tiret d'ouverture.

```yaml
1 | - Cat
2 | - Dog
3 | - Goldfish
```

Cette séquence place chaque élément de la liste au même niveau. Si vous souhaitez créer une séquence imbriquée avec des éléments et des sous-éléments, vous pouvez le faire en plaçant un seul espace avant chaque tiret dans les sous-éléments. YAML utilise des espaces, **PAS** des tabulations, pour l'indentation. Vous pouvez en voir un exemple ci-dessous.

```yaml
1 | -
2 |  - Cat
3 |  - Dog
4 |  - Goldfish
5 | -
6 |  - Python
7 |  - Lion
8 |  - Tiger
```

Si vous souhaitez imbriquer vos séquences encore plus profondément, il vous suffit d'ajouter plus de niveaux.

```yaml
1 | -
2 |  -
3 |   - Cat
4 |   - Dog
5 |   - Goldfish
```

Des séquences peuvent être ajoutées à d'autres types de structure de données, tels que des mappages ou des scalaires.

<h3 id="Mappages">Mappages.
<a href="#Mappages" class="toc-anchor after"></a></h3>

Le mappage vous donne la possibilité de répertorier les clés avec des valeurs. Ceci est utile dans les cas où vous attribuez un nom ou une propriété à un élément spécifique.

```yaml
1 | animal: pets
```

Cet exemple mappe la valeur de `pets` à la clé `animal`. Lorsqu'il est utilisé en conjonction avec une séquence, vous pouvez voir que vous commencez à construire une liste de `pets`. Dans l'exemple suivant, le tiret utilisé pour étiqueter chaque élément compte comme une indentation, ce qui fait des éléments de ligne l'enfant et de la ligne de mappage pets le parent.

```yaml
1 | pets:
2 |  - Cat
3 |  - Dog
4 |  - Goldfish
```

<h3 id="Ressources et documentation supplémentaire">Ressources et documentation supplémentaire.
<a href="#Ressources et documentation supplémentaire" class="toc-anchor after"></a></h3>

Pour plus d'informations sur YAML, y compris une documentation détaillée sur son fonctionnement, consultez les ressources liées ci-dessous.

* [Introduction à YAML de Dave](https://github.com/darvid/trine/wiki/YAML-Primer)
* [Documentation officielle YAML 1.2](https://yaml.org/spec/1.2/spec.html)
* [Carte de référence YAML](https://yaml.org/refcard.html)
* [Tutoriel YAML de Xavier Shay](http://rhnh.net/2011/01/31/yaml-tutorial)
* [YAMLLint](http://www.yamllint.com/)

