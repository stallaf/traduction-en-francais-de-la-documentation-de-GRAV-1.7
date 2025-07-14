<h1 class="rem">Procédure : formulaires dans des pages modulaires</h1>

<h2 id="Utiliser des formulaires dans des pages modulaires">Utiliser des formulaires dans des pages modulaires.
<a href="#Utiliser des formulaires dans des pages modulaires" class="toc-anchor after"></a></h2>

Si votre thème ne fournit pas de fichier `templates/forms/form.html.twig`, il n'est pas configuré pour utiliser des formulaires, mais pas de panique - copiez simplement les modèles de formulaire depuis Antimatter, le thème Grav par défaut :

* templates/form.html.twig
* templates/formdata.html.twig

Maintenant, créez un dossier modulaire avec le type de page `form.md`.

Par exemple : `01.votre-page-modulaire/_contact/form.md`.

La page `form.md` ne contiendra aucune définition de formulaire. C'est juste une indication que c'est la partie qui doit générer le formulaire.

Important : réglez

```yaml
1 | ---
2 | cache_enable: false
3 | ---
```

dans cet en-tête de page frontmatter, en raison du fonctionnement des pages modulaires, si vous l'oubliez, le formulaire sera mis en cache, ainsi que le nonce généré toutes les 12 heures. Ainsi, lorsque le changement de 12 heures est atteint, le formulaire cesse de fonctionner jusqu'à ce que le cache soit actualisé. Cette étape n'est pas nécessaire pour les formulaires de page autonomes.

Ajoutez maintenant l'en-tête du formulaire à la page modulaire principale, `modular.md`.

La page modular.md doit contenir toute la définition du formulaire, avec les champs, etc., comme s'il s'agissait d'un en-tête de fichier form.md "pleine page". Avec son propre chemin de page comme champ `form.action`.

<div class="notice tip">
Dans Form v2.0, vous pouvez désormais définir le formulaire directement dans la sous-page modulaire comme n'importe quel autre formulaire. Cependant, s'il n'est pas trouvé, le plugin de formulaire cherchera dans la "page actuelle", c'est-à-dire la page modulaire de niveau supérieur pour un formulaire, il est donc entièrement rétrocompatible avec la façon de faire 1.0. !!!
</div>

Par exemple:

```yaml
1  | ---
2  | content:
3  |     items: '@self.modular'
4  | 
5  | form:
6  |     action: /your-modular-page
7  |     name: my-nice-form
8  |     fields:
9  |         -
10 |             name: name
11 |             label: Name
12 |             placeholder: 'Enter your name'
13 |             autofocus: 'on'
14 |             autocomplete: 'on'
15 |             type: text
16 |             default: test
17 | 
18 |     buttons:
19 |        -
20 |           type: submit
21 |             value: Submit
22 | 
23 |     process:
24 |         -
25 |             message: 'Thank you for your feedback!'
26 |---
```

Dans l'en-tête du formulaire, assurez-vous d'ajouter le paramètre d'action `action`, avec la route de page modulaire.

Comme présent dans l'exemple ci-dessus. Cette étape est nécessaire car si vous n'ajoutez pas explicitement `form.action`, le code recherche généralement l'itinéraire de la page, mais étant le formulaire dans une sous-page modulaire, pas une page réelle, le chemin est erroné et interrompt la soumission du formulaire.

Donc, si la page modulaire est par ex. `site.com/my-page`, il suffit de mettre `form: action: /my-page` dans `modular.md`. Même si la page modulaire est la page d'accueil, utilisez la route de la page, par ex. `form : action : /home`.

<h3 id="Un exemple vivant">Un exemple vivant.
<a href="#Un exemple vivant" class="toc-anchor after"></a></h3>

Le squelette Deliver a une page de formulaire modulaire prête à être consultée lors de la lecture de ce didacticiel :

[Page en direct](http://demo.getgrav.org/deliver-skeleton/contact)

[Fichier markdown de la page](https://github.com/getgrav/grav-skeleton-deliver-site/blob/develop/pages/07.contact/modular_alt.md)

<h3 id="Dépannage des formulaires dans les pages modulaires">Dépannage des formulaires dans les pages modulaires.
<a href="#Dépannage des formulaires dans les pages modulaires" class="toc-anchor after"></a></h3>

La meilleure façon de dépanner un formulaire est de revenir d'abord aux racines et d'ajouter vos personnalisations une par une pour voir ce qui ne va pas.

* Je suggère de créer un "formulaire régulier", en veillant à ce qu'il fonctionne, puis essayez de le mettre sous une forme modulaire.
* Essayez de faire fonctionner le formulaire sur un squelette basé sur l'antimatière, qui fournit tous les fichiers dont vous avez déjà besoin.
* Si les champs du formulaire n'apparaissent pas, si vous avez installé le plugin Assets désactivez-le/désinstallez-le. Il y a un problème connu avec la rupture des formulaires modulaires qui sera bientôt corrigé.

