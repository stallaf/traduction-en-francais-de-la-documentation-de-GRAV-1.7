<h1 class="rem">Mise à niveau vers Grav 1.6</h1>

Grav 1.6 est la plus grande mise à jour depuis la version initiale de Grav. Il introduit quelques nouvelles fonctionnalités, améliorations, corrections de bogues et fournit de nombreux changements architecturaux qui ouvrent la voie vers Grav 2.0.

<div class = "notice warning">
<strong>IMPORTANT</strong> : Pour la plupart des gens, Grav 1.6 devrait être une simple mise à niveau sans aucun problème, mais comme toute mise à niveau, il est recommandé de <strong>faire une sauvegarde</strong> de votre site et de <strong>tester la mise à niveau dans un environnement</strong> de test avant de mettre à niveau votre site en ligne.
</div>

Que vous soyez développeur ou administrateur de votre site, votre site de test doit avoir la [barre de débogage](/avance-debogage-journalisation/#Barre de débogage) activée. En effet, la barre de débogage dispose d'outils utiles qui vous aident à vous assurer que votre site sera mieux préparé pour fonctionner sur les versions ultérieures de Grav.

<div class = "notice tip">
<strong>ASTUCE</strong> : Pour plus d'informations sur l'activation de la fonctionnalité, veuillez consulter la section <a href="../avance-debogage-journalisation">Débogage et journalisation</a> dans la documentation.
</div>

<h2 id="Onglet de barre de débogage obsolète">Onglet de barre de débogage obsolète.
<a href="#Onglet de barre de débogage obsolète" class="toc-anchor after"></a></h2>

Pour nos besoins, nous recherchons l'onglet **Obsolète** dans la barre de débogage, qui vous permet d'identifier les problèmes obsolètes et de les corriger ou de les signaler avant de passer aux versions ultérieures de Grav. La résolution des problèmes détectés dans l'onglet **Obsolète** vous aidera à rendre votre site plus rapide et vous fera gagner du temps lors des futures mises à jour.

![Onglet obsolète](https://learn.getgrav.org/user/pages/08.advanced/09.grav-development/02.grav-16-upgrade-guide/deprecated-tab.png)

<div class = "notice note">
<strong>REMARQUE</strong> : cet onglet <strong>Obsolète</strong> s'affiche uniquement si des appels obsolètes sont détectés dans la page.
</div>

Pour vous assurer que vous détectez tous les problèmes, vous devez soit vider le cache et exécuter Grav avec la mise en cache désactivée pour maximiser les chances de détecter toutes les erreurs. Même en suivant ces étapes, vous remarquerez peut-être que certaines des erreurs YAML/Twig n'apparaissent qu'après avoir vidé le cache.

L'onglet **Obsolète** contient une liste des fonctionnalités obsolètes qui ont été trouvées. Chaque problème est cliquable et ouvre un message d'obsolescence, qui contient une brève explication du problème ainsi qu'une trace, ce qui vous permet de localiser et de corriger le code. Dans le côté droit, vous pouvez voir le type d'erreur obsolète et dans le coin inférieur droit, vous pouvez filtrer les types affichés en cliquant sur les badges.

Lorsque vous ouvrez le message d'obsolescence, le contenu peut d'abord sembler écrasant. Mais dans la plupart des cas, vous pouvez ignorer la majeure partie du contenu et lire simplement les premières lignes : message, fichier et ligne (si disponible).

Il existe plusieurs types de problèmes d'obsolescence :

* `yaml` : le fichier YAML ou Markdown utilise une syntaxe YAML obsolète.
* `twig` : le fichier Twig contient une syntaxe Twig obsolète ou il y a eu un autre problème lié à Twig.
* `grav` : quelque chose appelle la méthode Grav obsolète ou utilise une propriété obsolète.
* `vrndor` : quelque chose utilise un code de bibliothèque tiers obsolète.
* `unknowm` : message obsolète inconnu.

<h2 id="Analyse YAML">Analyse YAML.
<a href="#Analyse YAML" class="toc-anchor after"></a></h2>

<div class = "notice note">
<strong>REMARQUE</strong> : dans Grav 1.6, YAML a une analyse plus stricte avec une solution de secours pour une compatibilité descendante.
</div>

Grav 1.6 utilise un **analyseur Symfony 4.2 YAML**, qui suit la [spécification standard YAML](https://yaml.org/spec) beaucoup plus étroitement que l'ancien analyseur de Symfony **3.4**. Cela signifie que les fichiers YAML qui fonctionnaient correctement auparavant peuvent provoquer des erreurs résultant d'un YAML non valide. Cependant, si le fichier ne se charge pas avec la nouvelle version **4.2** de l'analyseur, Grav reviendra par défaut à l'ancienne version **3.4** de l'analyseur pour maintenir votre site opérationnel. Cependant, cela réduira les performances du site et vous devez détecter et résoudre les problèmes pour garantir des performances optimales.

<div class = "notice note">
<strong>REMARQUE</strong> : Ce mécanisme de secours de rétrocompatibilité sera supprimé dans Grav 2.0.
</div>

**Grav 1.6.7** et toutes les versions ultérieures ont une nouvelle commande CLI pour détecter les problèmes d'analyse YAML, veuillez exécuter `bin/grav yamllinter` pour trouver et corriger les erreurs d'analyse YAML sur votre site. Il est recommandé d'exécuter cette commande juste après la mise à niveau vers Grav 1.6 ou une version ultérieure.

**Admin 1.9.3** et toutes les versions ultérieures ont **YAML Linter** intégré à **Tools> Reports**, si vous préférez l'utiliser à la place de la commande CLI.

<h3 id="Recherchez ces erreurs YAML :">Recherchez ces erreurs YAML :.
<a href="#Recherchez ces erreurs YAML :" class="toc-anchor after"></a></h3>

* N'utilisez pas `@`, `\`, `|`, `%` et `>` au début d'une chaîne sans guillemets : n'utilisez pas `@data-options : []`, utilisez `data-options@ : []` à la place.
* Ajoutez toujours un espace blanc après deux-points : pour les clés : n'utilisez pas `key:value`, utilisez `key: value` à la place.
* Citez toujours null, true, false, 2.0 (flottants) dans les clés ; les clés ne peuvent être que des entiers ou des chaînes.
* Citez également null, true, false, 2 et 2.0 dans les valeurs si elles sont censées être des chaînes.
* Lorsque vous entourez des chaînes de guillemets doubles, vous devez maintenant échapper les caractères \.

La barre de débogage peut également être utilisée pour repérer tout YAML obsolète. Ouvrez simplement la barre de débogage et regardez l'onglet Obsolète. Si l'onglet est introuvable, aucun problème n'a été détecté.

<div class = "notice tip">
<strong>ASTUCE</strong> : Vous pouvez filtrer tous les problèmes <strong>YAML</strong> en consultant les <strong>badges dans le coin inférieur droit</strong> de la barre de débogage. Filtrez simplement pour n'afficher que les problèmes <strong>YAML</strong> en cliquant sur les autres boutons pour les désactiver.
</div>

<div class = "notice note">
<strong>REMARQUE</strong> : les erreurs YAML nécessitent que vous vidiez le cache. Les erreurs ne seront détectées que lorsque les fichiers YAML seront décodés.
</div>

<h2 id="Mode de compatibilité YAML">Mode de compatibilité YAML.
<a href="#Mode de compatibilité YAML" class="toc-anchor after"></a></h2>

Par défaut, le mode de compatibilité YAML a été activé dans Grav 1.6. Cela permettra aux sites plus anciens de continuer à fonctionner après la mise à niveau, mais ce n'est pas idéal pour être utilisé dans de nouveaux sites ou si vous avez déjà corrigé et testé votre site contre toutes les erreurs d'analyse YAML.

Vous pouvez modifier ce paramètre dans `user/config/system.yaml` :

```yaml
strict_mode :
  yaml_compat : false
```

Notre recommandation est de ne pas toucher à ce paramètre sur les sites existants pour l'instant, mais plutôt de créer des sites de test avec le mode de compatibilité **false**. De plus, tout nouveau site créé avec Grav 1.6 ou une version ultérieure doit avoir le mode de compatibilité désactivé pendant le développement car il vous permet de gagner beaucoup de temps lorsqu'il est temps de passer à Grav 2.0.

<h2 id="Twig">Twig.
<a href="#Twig" class="toc-anchor after"></a></h2>

<h3 id="Blocages différés">Blocages différés.
<a href="#Blocages différés" class="toc-anchor after"></a></h3>

Vous devez mettre à jour votre thème vers une version qui ajoute la prise en charge des blocs d'actifs différés pour fournir une prise en charge complète de Grav 1.6. Alternativement, si vous avez un thème modifié personnalisé ou développé le vôtre, vous devez le mettre à jour vous-même afin de vous assurer qu'il continue de fonctionner avec les nouvelles fonctionnalités et les versions ultérieures de Grav et de ses plugins en suivant le guide dans le billet de blog sur les [mises à jour importantes du thème](https://getgrav.org/blog/important-theme-updates).

<h3 id="Twig obsolète">Twig obsolète.
<a href="#Twig obsolète" class="toc-anchor after"></a></h3>

Grav 2.0 utilisera **Twig 2** au lieu de Twig 1 qui est actuellement utilisé dans les versions Grav 1.x. Il y a quelques fonctionnalités obsolètes qui ont été supprimées dans Twig 2, c'est pourquoi vous devez vous assurer que vous détectez et corrigez tous ces problèmes avant de passer à Grav 2.0 sur la route.

La barre de débogage peut être utilisée pour repérer tout problème Twig obsolète. Ouvrez simplement la barre de débogage et cliquez sur l'onglet **Deprecated**.

<h4 id="Recherchez ces problèmes Twig :">Recherchez ces problèmes Twig :
<a href="#Recherchez ces problèmes Twig :" class="toc-anchor after"></a></h4>

* Les macros importées dans un fichier ne seront plus disponibles dans les modèles enfants (via un appel include par exemple). Vous devez importer les macros explicitement dans chaque fichier où vous les utilisez.
* Le filtre `|replace()` ne fonctionnera qu'avec le tableau associé comme paramètre : `{ "I like this and that."|replace({'this': 'foo', 'that': 'bar'}) }}`.
* Le test `sameas()` doit maintenant être écrit comme `same as()`.

Vous [trouverez plus d'informations](https://twig.symfony.com/doc/1.x/deprecated.html) sur ce qui a été obsolète ici.

<h3 id="Échappement automatique">Échappement automatique.
<a href="#Échappement automatique" class="toc-anchor after"></a></h3>

Grav a été plutôt à l'abri des vulnérabilités, à l'exception des attaques XSS, qui peuvent être déclenchées sans trop d'effort sur n'importe quel code qui n'échappe pas correctement aux entrées non fiables d'un utilisateur. Twig est un moyen facile d'écrire des fichiers modèles, mais en même temps, il est trop facile d'oublier que la plupart des variables utilisées dans les fichiers modèles ne sont pas filtrées avant d'être utilisées. Même s'ils sont filtrés et sûrs, ils peuvent contenir des caractères spéciaux qui doivent être échappés pour rendre le code HTML valide.

Par exemple, vous pouvez avoir un modèle Twig comme celui-ci :

```twig
1 | {% set my_string = '<script>echo("bonjour!");<script>' %}
2 | <p>
3 |     {{ ma_chaîne }}
4 | </p>
```

Par défaut, Grav **a désactivé l'échappement automatique Twig** pour la simplicité et la clarté des modèles, mais malheureusement, ce fut une mauvaise décision car personne, y compris nous, ne se souvient de toujours échapper les variables qui peuvent contenir des caractères spéciaux ou provenir d'une source non fiable. Pour aggraver les choses, on ne sait généralement pas si la variable est compatible HTML ou non. Pour vous assurer qu'un site est protégé contre la plupart des vulnérabilités XSS, vous devez activer l'échappement automatique dans votre configuration. Malheureusement, les thèmes et les plugins qui utilisent les modèles Twig ont tendance à ne pas fonctionner avec le paramètre activé - et les modèles écrits sans échappement explicite sont très probablement vulnérables au contenu malveillant.

Avec l'exemple ci-dessus, comme l'échappement automatique est désactivé, la sortie sera rendue en HTML pur et une boîte d'alerte avec `"bonjour!"` apparaîtra. Cependant, cela doit être échappé à l'aide du filtre d'échappement `|e` Twig (ou `|e('html')` :

```twig
1 | {% set my_string = '<script>echo("bonjour!");<script>' %}
2 | <p>
3 |     {{ ma_chaîne|e }}
4 | </p>
```

Parce que la plupart des gens ont tendance à oublier d'échapper les variables dans Twig et parce que l'utilisation de `|e` partout peut rendre les fichiers de modèle plus difficiles à lire, il y a un nouveau paramètre dans `user/config/system.yaml` :

```yaml
1 | strict_mode:
2 |   twig_compat: false
```

Ce paramètre force l'activation de `auto-escaping` dans tous les fichiers de modèle Twig et désactive l'ancien paramètre pour l'activer et le désactiver. L'effet secondaire du paramètre est que votre site contiendra probablement quelques éléments de contenu échappés, que vous devrez corriger en utilisant le filtre `|raw` pour tout le contenu qui doit contenir uniquement du HTML et du HTML. De nombreux modèles et plugins n'ont pas encore été mis à jour pour fonctionner avec l'échappement forcé, veuillez donc signaler tout bogue dans ceux-ci pour permettre leur correction en temps opportun.

La transition vers l'échappement automatique ne sera pas facile. Pendant la transition, tous les fichiers de modèle doivent soit contenir à la fois des filtres |e et |raw sur chaque variable pour s'assurer que le fichier de modèle peut être utilisé en toute sécurité dans les deux modes, ou vous pouvez entourer tout le code du modèle avec les tags Twig`{% autoescape %}`.

Voir le [manuel Twig](https://twig.symfony.com/doc/1.x/tags/autoescape.html) pour plus d'informations.

