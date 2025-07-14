<h1 class="rem">Commande de plugin</h1>

Les plugins peuvent s'intégrer au `bin/plugin` CLI de Grav et améliorer les fonctionnalités du plugin via Terminal en exécutant des tâches spécifiques.

Comme expliqué dans la section [Grav CLI](./cli-commandes-grav.md), vous devez utiliser un terminal pour pouvoir exécuter des commandes.

Lors de l'exécution de la commande `bin/plugin`, tous les plugins qui fournissent l'intégration CLI seront répertoriés.

<img class= img src="https://learn.getgrav.org/user/pages/07.cli-console/03.grav-cli-plugin/bin-plugin.png" alt="commande de plugin 1">

La première option transmise à `bin/plugin` est toujours le slug du plugin (c'est-à-dire *erreur*, *connexion*, etc.). La deuxième option est la commande réelle fournie par le plugin.

En fonction de l'implémentation du plugin, il pourrait y avoir d'autres options suivantes et vous pouvez en savoir plus sur chacune d'elles en exécutant la commande `bin/plugin [slug] list`.

<h2 id="Commandes et options réservées">Commandes et options réservées.
<a href="#Commandes et options réservées" class="toc-anchor after"></a></h2>

Certaines *commandes* et *options* réservées sont toujours disponibles à partir de n'importe quel plugin. Ceux-ci sont également particulièrement importants à garder à l'esprit si vous êtes un développeur et que vous souhaitez implémenter vos propres tâches, car vous ne pourrez utiliser aucune des commandes et options réservées.

**Commandes**
<table>
<tr>
   <td> <code>help</code> </td>
   <td> Affiche l'aide d'une commande.</td>
</tr>
<tr>
   <td> <code>list</code> </td>
   <td> Répertorie les commandes</td>.
</tr>
</table>

**Options**
<table>
<tr>
   <td> <code>-h, --help</code> </td>
   <td> Afficher un message d'aide.</td>
</tr>   
<tr>
   <td> <code>-q,&nbsp;--quiet</code> </td>
   <td> N'affiche aucun message.</td>
</tr>
<tr>
   <td> <code>-v,&nbsp;--version</code> </td>
   <td> Afficher la version de l'application.</td>
</tr>
<tr>
   <td> <code>--ansi</code> </td>
   <td> Forcer la sortie ANSI.</td>
</tr>
<tr>
   <td> <code>--no-ansi</code> </td>
   <td> Désactiver la sortie ANSI.</td>
</tr>
<tr>
   <td> <code>-n,&nbsp;--no-interaction</code> </td>
   <td> Ne pas poser de question interactive.</td>
</tr>
<tr>
   <td> <code>-v|vv|vvv,&nbsp;--verbose</code> </td>
   <td> Augmente la verbosité des messages : 1 pour une sortie normale, 2 pour une<br> sortie plus détaillée et 3 pour le débogage.</td>
</tr>
</table>

<h2 id="Comment utiliser la CLI pour les plugins">Comment utiliser la CLI pour les plugins.
<a href="#Comment utiliser la CLI pour les plugins" class="toc-anchor after"></a></h2>

Les commandes **list** et **help** sont très utiles lorsque vous ne savez pas encore comment utiliser un certain plugin CLI.

Avec **list**, vous pouvez accéder à toutes les commandes disponibles et afficher une description rapide de ce que chacune d'elles fait.

Voici un exemple avec le plugin **login** lorsque nous exécutons la commande `bin/plugin login list`.

<div class="notice note">
Ne pas spécifier de commande après le slug du plugin qui est automatiquement <code>list</code> par défaut. Cela signifie que <code>bin/plugin [slug] list</code> et <code>bin/plugin [slug]</code> sont équivalents.
</div>

<img class= img src="https://learn.getgrav.org/user/pages/07.cli-console/03.grav-cli-plugin/bin-plugin-login.png" alt="commande de plugin 2">

Comme vous pouvez le constater, la plupart des options et commandes correspondent à la <id class="ancre">liste réservée</id>. Les commandes réelles offertes par le plugin de connexion sont `add-user`, `new-user` et `newuser`.

Comme vous pouvez le constater, la description de l'aide des 3 commandes est identique. En effet, par choix, les 3 commandes sont exactement les mêmes. **add-user** et **newuser** sont en fait des alias pour **new-user**, ce qui permet de deviner facilement la commande sans la connaître ni s'en souvenir.

Maintenant que nous savons que le plugin de connexion est livré avec une commande `new-user`, nous devons seulement apprendre à l'utiliser. C'est là que la commande d'aide réservée entre en place. Lançons `bin/plugin login help new-user`.

<img class= img src="https://learn.getgrav.org/user/pages/07.cli-console/03.grav-cli-plugin/bin-plugin-newuser.png" alt="commande de plugin 3">

Nous avons maintenant une compréhension complète de la commande `new-user` et nous savons comment l'utiliser. Essayons de créer un nouvel utilisateur. Étant donné que toutes les options sont facultatives par définition, nous omettrons volontairement le mot de passe (on nous le demandera ultérieurement dans une invite).

```console
$ | bin/plugin login newuser -u joeuser -e joeuser@grav.org -P b -N "Joe User" -t "Site Administrator"
  | Creating new user
  |
  | Enter a password: *********
  | Repeat the password: *********
  |
  | Success! User joeuser created.
```

<h2 id="Développeurs : intégrez la CLI dans le plugin">Développeurs : intégrez la CLI dans le plugin.
<a href="#Développeurs : intégrez la CLI dans le plugin" class="toc-anchor after"></a></h2>

En tant que développeur, vous souhaiterez peut-être créer des commandes CLI que les administrateurs ou les utilisateurs pourront exécuter. Il est extrêmement facile d'ajouter une telle fonctionnalité dans un plugin.

La première chose que vous voulez faire est de créer un sous-dossier `cli/` à la racine de votre plugin. Ce dossier sera traité par `bin/plugin` et analysé pour les classes de commandes.

La CLI de Grav est basée sur l'excellent [composant de la console Symfony](https://symfony.com/doc/current/components/console/introduction.html) et vous pouvez à peu près suivre leur documentation pour une référence complète, il n'y a que quelques éléments importants à prendre en compte.

1. Le nom du fichier de classe est standard. Il doit commencer par une majuscule et se terminer par Command.php.
    * `Hello.php` : WRONG
    * `helloworldCommand.php` : WRONG
    * `HelloworldCommand.php` : CORRECT
    * `HelloWorldCommand.php` : CORRECT

2. Vous devez toujours étendre `ConsoleCommand`, cela vous offrira des extras Grav tels que des couleurs formatées, un accès direct à l'instance Grav et à d'autres utilitaires ([plus de détails](https://github.com/getgrav/grav/blob/develop/system/src/Grav/Console/ConsoleTrait.php)).

3. La console Symfony nécessite une méthode `execute`. Lors de l'extension de **ConsoleCommand**, cela devient `serve`.

Vous trouverez ci-dessous un exemple simple pour vous aider à démarrer. Vous pouvez le tester, tel quel, en l'enregistrant sous `HelloCommand.php` et en le plaçant sous le dossier racine `cli/` de votre plugin (**user/plugins/my_plugin/cli/HelloCommand.php)**.

```php
1  | <?php
2  | namespace Grav\Plugin\Console;
3  | 
4  | use Grav\Console\ConsoleCommand;
5  | use Symfony\Component\Console\Input\InputArgument;
6  | use Symfony\Component\Console\Input\InputOption;
7  | 
8  | /**
9  |  * Class HelloCommand
10 |  *
11 |  * @package Grav\Plugin\Console
12 |  */
13 | class HelloCommand extends ConsoleCommand
14 | {
15 |     /**
16 |      * @var array
17 |      */
18 |     protected $options = [];
19 | 
20 |     /**
21 |      * Greets a person with or without yelling
22 |      */
23 |     protected function configure()
24 |     {
25 |         $this
26 |             ->setName("hello")
27 |             ->setDescription("Greets a person.")
28 |             ->addArgument(
29 |                'name',
30 |                 InputArgument::REQUIRED,
31 |                 'The name of the person that should be greeted'
32 |             )
33 |             ->addOption(
34 |                 'yell',
35 |                 'y',
36 |                 InputOption::VALUE_NONE,
37 |                 'Wheter the greetings should be yelled or quieter'
38 |             )
39 |             ->setHelp('The <info>hello</info> greets someone.')
40 |         ;
41 |     }
42 | 
43 |     /**
44 |      * @return int|null|void
45 |      */
46 |     protected function serve()
47 |     {
48 |         // Collects the arguments and options as defined
49 |         $this->options = [
50 |             'name' => $this->input->getArgument('name'),
51 |             'yell' => $this->input->getOption('yell')
52 |         ];
53 | 
54 |         // Prepare the strings we want to output and wraps the name into a cyan color
55 |         // More colors available at:
56 |         // https://github.com/getgrav/grav/blob/develop/system/src/Grav/Console/ ConsoleTrait.php
57 |         $greetings = 'Greetings, dear <cyan>' . $this->options['name'] . '</cyan>!';
58 | 
59 |         // If the optional `--yell` or `-y` parameter are passed in, let's convert 60 | everything to uppercase
60 |        if ($this->options['yell']) {
61 |             $greetings = strtoupper($greetings);
62 |         }
63 | 
64 |         // finally we write to the output the greetings
65 |         $this->output->writeln($greetings);
66 |     }
67 | }
```

<img class= img src="https://learn.getgrav.org/user/pages/07.cli-console/03.grav-cli-plugin/grav-plugin-hello.png" alt="commande de plugin 4">

<div class="notice note">
Un autre bon exemple simple peut être trouvé dans le <a href="https://github.com/getgrav/grav-plugin-error/blob/develop/cli/LogCommand.php">plugin d'erreur (LogCommand.php)</a>, Si vous cherchez un exemple plus complexe, vous devriez jeter un œil au <a href="https://github.com/getgrav/grav-plugin-login/blob/develop/cli/NewUserCommand.php">plugin de connexion (NewUserCommand.php)</a>.

</div>


