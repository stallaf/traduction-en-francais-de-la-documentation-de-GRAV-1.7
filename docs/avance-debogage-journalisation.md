<h1 class="rem">Débogage et journalisation</h1>

Lors du développement de thèmes et de plugins, il est souvent nécessaire d'afficher des informations de **débogage**. Grav dispose de puissantes capacités de débogage via une variété de fonctionnalités :

<h2 id="Barre de débogage">Barre de débogage.
<a href="#Barre de débogage" class="toc-anchor after"></a></h2>

Grav est livré avec un excellent outil pour faciliter cet effort appelé via une **Barre de Débogage**. Cette fonctionnalité est **désactivée** par défaut, mais peut être activée globalement ou pour votre [environnement de développement](/avance-config-environnement) uniquement via le fichier de configuration `system.yaml` :

```yaml
1 | debugger:
2 |   enabled: true             # Enable Grav debugger and following settings
3 |   provider: debugbar        # Set provider to debugbar
4 |   shutdown:
5 |      close_connection: true  # Close the connection before calling onShutdown(). false for debugging
```

![débogage 1](https://learn.getgrav.org/user/pages/08.advanced/03.debugging/config.png)

<div class="notice info">
La première fois que vous l'activez, la barre de débogage peut apparaître sous la forme d'une petite icône Grav en bas à gauche de la page. Cliquez dessus pour afficher la barre de débogage complète.
</div>

La barre de débogage PHP fournit toujours un **temps de traitement** global ainsi que **l'utilisation de la mémoire**, mais elle comporte désormais plusieurs onglets qui fournissent des informations plus détaillées.

Le premier onglet est pour les **messages** et vous pouvez l'utiliser pour aider à déboguer votre processus de développement Grav en publiant des informations sur cet onglet à partir de votre code.

En plus des informations sur les **requêtes**, les **exceptions** et la **configuration**, vous pouvez également voir une ventilation détaillée de la synchronisation Grav dans le panneau **Chronologie** :

![débogage 2](https://learn.getgrav.org/user/pages/08.advanced/03.debugging/timeline.png)

<h2 id="Commande de vidage pour PHP">Commande de vidage pour PHP.
<a href="#Commande de vidage pour PHP" class="toc-anchor after"></a></h2>

Si vous essayez de déboguer du PHP, par exemple un plugin personnalisé que vous développez, et que vous souhaitez examiner rapidement un objet ou une variable, vous pouvez utiliser la puissante commande `dump()`. Cela accepte pratiquement toutes les variables PHP valides et affichera les résultats dans un affichage bien formaté et colorisé dans votre navigateur.

Par exemple, vous pouvez facilement vider une variable ou un objet PHP :

    dump($myvariable);

et voir les résultats dans votre navigateur :

![débogage 3](https://learn.getgrav.org/user/pages/08.advanced/03.debugging/dump.png)

Vous pouvez également vider des variables dans l'onglet **Messages** de la barre de débogage en utilisant la syntaxe :

    $this->grav['debugger']->addMessage($myvariable)

<h2 id="Commande de vidage pour Twig">Commande de vidage pour Twig.
<a href="#Commande de vidage pour Twig" class="toc-anchor after"></a></h2>

Vous pouvez également afficher les variables Twig à partir de vos modèles Twig. Cela se fait de la même manière, mais les résultats sont affichés dans le panneau **Messages** de la barre de débogage. Cette fonctionnalité est **désactivée** par défaut, mais peut être activée globalement ou pour votre [environnement de développement](/avance-config-environnement) uniquement via le fichier de configuration `system.yaml`:

```twig
1 | twig:
2 |   debug: true              # Enable Twig debugger
```

Par exemple, vous pouvez facilement vider une variable ou un objet Twig :

    1 | {{ dump(page.header) }}

et voir les résultats dans la barre de débogage :

![débogage 4](https://learn.getgrav.org/user/pages/08.advanced/03.debugging/dump.png)

Il est possible de générer plusieurs commandes de vidage en même temps, mais il peut être difficile de les distinguer. Ajoutez du texte statique comme ceci :

    1 | {{ dump('page.header output:',page.header) }}

<h2 id="Dump à la console du navigateur depuis Twig">Dump à la console du navigateur depuis Twig.
<a href="#Dump à la console du navigateur depuis Twig" class="toc-anchor after"></a></h2>

Pour afficher des variables avant qu'une page ne soit renvoyée par Grav ou dans le cas où aucune actualisation de page ne se produit, comme lors de l'utilisation d'AJAX, il existe une autre alternative. En utilisant une seule ligne de Javascript, n'importe quelle variable peut être affichée dans la console développeur de votre navigateur, par exemple :

    <script> console.log({{ page.header|json_encode|raw }}) </script>

Examinez ensuite la valeur dans la console du navigateur :

![débogage 5](https://learn.getgrav.org/user/pages/08.advanced/03.debugging/console-dump.png)

<h2 id="Affichage d'erreurs">Affichage d'erreurs.
<a href="#Affichage d'erreurs" class="toc-anchor after"></a></h2>

Notre nouvelle page d'affichage des erreurs fournit des informations détaillées, des backtraces et même des blocs de code pertinents. Cela permet d'isoler, d'identifier et de résoudre plus rapidement les erreurs critiques. Par défaut dans Grav, ceux-ci sont désactivés par défaut, vous devrez donc les activer pour profiter de cette gestion des erreurs utile pour le développement :

```yaml
1 | errors:
2 |   display: true
```

![débogage 6](https://learn.getgrav.org/user/pages/08.advanced/03.debugging/error.png)

Pour les environnements de production, vous pouvez désactiver la page d'erreur détaillée avec quelque chose de plus subtil en configurant les options d'erreurs dans votre fichier `user/config/system.yaml` et compter sur les erreurs enregistrées dans le fichier :

```yaml
1 | errors:
2 |   display: false
3 |   log: true
```

![débogage 7](https://learn.getgrav.org/user/pages/08.advanced/03.debugging/error2.png)

<h2 id="Enregistrement">Enregistrement.
<a href="#Enregistrement" class="toc-anchor after"></a></h2>

La possibilité de consigner des informations est souvent utile, et encore une fois, Grav nous fournit une fonctionnalité de journalisation simple et puissante. Utilisez l'une des syntaxes suivantes :

```php
1 | $this->grav['log']->info('My informational message');
2 | $this->grav['log']->notice('My notice message');
3 | $this->grav['log']->debug('My debug message');
4 | $this->grav['log']->warning('My warning message');
5 | $this->grav['log']->error('My error message');
6 | $this->grav['log']->critical('My critical message');
7 | $this->grav['log']->alert('My alert message');
8 | $this->grav['log']->emergency('Emergency, emergency, there is an emergency here!');
```

Tous vos messages seront ajoutés au fichier `/logs/grav.log`.

