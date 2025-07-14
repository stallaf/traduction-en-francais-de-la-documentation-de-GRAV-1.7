<h1 class="rem">Priorisation des plugins</h1>

Lorsque plusieurs plugins écoutent les mêmes crochets d'événement (voir la page [Plugins > Crochets d'événement pour plus de détails)](/crochets-evenements), les différents gestionnaires sont exécutés par ordre de "priorité". La priorité est simplement un nombre. Plus le nombre est élevé, plus le gestionnaire s'exécutera tôt.

Dans de rares cas, les utilisateurs peuvent avoir besoin de modifier les priorités de certains gestionnaires. Cela peut être fait sans toucher au code original du plugin.

Déterminez d'abord précisément quels gestionnaires doivent être modifiés et comment. Il s'agit d'une tâche avancée qui nécessite que vous soyez capable de lire le fichier `.php` du plugin. Normalement, les crochets d'événement, les fonctions de gestionnaire et les priorités par défaut se trouvent dans la fonction `onPluginsInitialized()` d'un plugin.

Créez ensuite le fichier `user/config/priorities.yaml`. Les données doivent être structurées comme suit :

```yaml
pluginName:
   eventName:
      handlerName: [integer]
```

Ainsi, par exemple, disons que vous avez un plugin appelé `essential` qui écoute l'événement `onPageInitialized`, déclenchant la fonction `handlePage` avec une priorité de 0. Disons alors que vous découvrez que vous avez besoin que cette priorité soit de 100 pour vous assurer qu'il s'exécute avant un autre brancher. Vous ajouteriez ce qui suit à votre fichier `user/config/priorities.yaml` :

```yaml
essential:
   onPageInitialized:
      handlePage: 100
```
      

