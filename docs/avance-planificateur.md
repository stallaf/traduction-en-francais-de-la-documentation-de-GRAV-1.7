<h1 class="rem">Planificateur</h1>

Le planificateur Grav est une nouvelle fonctionnalité ajoutée dans Grav 1.6 qui permet d'exécuter des tâches de manière périodique. Le traitement sous-jacent repose sur le planificateur **cron** du serveur, mais une fois qu'une seule entrée a été ajoutée au service cron, tous les travaux et les horaires spécifiques peuvent être configurés via Grav.

L'un des principaux avantages de l'utilisation du planificateur pour gérer les tâches est qu'elles peuvent être effectuées sans aucune interaction de l'utilisateur et indépendamment du frontal. Des tâches telles que l'effacement périodique du cache, les sauvegardes, la synchronisation, l'indexation de recherche, etc., sont toutes des candidats de choix pour les tâches planifiées.

<h2 id="Installation">Installation.
<a href="#Installation" class="toc-anchor after"></a></h2>

La première étape pour configurer le planificateur et le préparer pour les tâches consiste à ajouter la commande `bin/grav scheduler` au service cron. L'approche la plus simple consiste à utiliser la commande CLI elle-même pour générer la commande appropriée à exécuter pour l'installation :

```console
$ | bin/grav scheduler -i
  |
  | Install Scheduler
  | =================
  |
  | [ERROR] You still need to set up Grav's Scheduler in your crontab
  |
  | ! [NOTE] To install, run the following command from your terminal:
  |
  | (crontab -l; echo "* * * * * cd /Users/andym/grav;/usr/local/bin/php bin/grav scheduler 1>> /dev/null 2>&1") | crontab - 
```

Sur mon système Mac, la commande complète requise est affichée, il vous suffit donc de copier et coller l'intégralité dans votre terminal et d'appuyer sur Entrée.

<div class="notice info">
Vous devez être connecté au shell avec le même utilisateur que votre serveur Web. Cela permet de s'assurer que l'utilisateur qui exécute les commandes schdeduler correspond à l'utilisateur du serveur Web qui doit interagir avec ces fichiers. Si vous installez l'entrée crontab avec un autre utilisateur (par exemple <code>root</code>), tous les fichiers créés seront créés en tant qu'utilisateur <code>root</code> et non en tant qu'utilisateur du<code>webserver</code> serveur Web, ce qui peut entraîner des problèmes.
</div>

```console
$ | (crontab -l; echo "* * * * * cd /Users/andym/grav;/usr/local/bin/php bin/grav scheduler 1>> /dev/null 2>&1") | crontab -
```

Vous n'obtiendrez pas de réponse, mais vous ne devriez pas non plus recevoir d'erreurs. Après cela, vous pouvez confirmer que tout semble bon en réexécutant la commande `bin/grav scheduler -i` :

```console
$ | bin/grav scheduler -i
  |
  | Install Scheduler
  | =================
  |
  | [OK] All Ready! You have already set up Grav's Scheduler in your crontab
```

Vous pouvez également obtenir la commande nécessaire à partir du plugin d'administration en naviguant simplement vers **Tools → Scheduler**.

<h2 id="Principes de base de la planification">Principes de base de la planification.
<a href="#Principes de base de la planification" class="toc-anchor after"></a></h2>

Afin de planifier une tâche, la fréquence est contrôlée par un format flexible.

```console
* * * * * *
| | | | | |
| | | | | +-- Year              (range: 1900-3000)
| | | | +---- Day of the Week   (range: 1-7, 1 standing for Monday)
| | | +------ Month of the Year (range: 1-12)
| | +-------- Day of the Month  (range: 1-31)
| +---------- Hour              (range: 0-23)
+------------ Minute            (range: 0-59)
```

Quelques exemples:

`0 * * * *`exécuté une fois par heure (toutes les heures à la minute zéro) `0 0 * * *` exécuté une fois par jour (tous les jours à minuit et à la minute zéro) `0 0 1 * *` exécuté une fois par mois (le premier jour de chaque mois à minuit et minute zéro) `0 0 1 1 *` exécuté une fois par an (le premier jour du premier mois chaque année à minuit et minute zéro)

Options avancées:

`*/5 * * * *` exécuté toutes les 5 minutes.

<h2 id="Fichier de configuration">Fichier de configuration.
<a href="#Fichier de configuration" class="toc-anchor after"></a></h2>

Vous pouvez voir quelles tâches sont actuellement disponibles pour le planificateur en exécutant la commande `bin/grav scheduler -j` :

```console
$ | bin/grav scheduler -j
  |
  | Scheduler Jobs Listing
  | ======================
  |
  | ┌─────────────────────┬────────────────────────────────────┬───────────┬─────────┬──────────────────┬─────────┐
  | │ Job ID              │ Command                            │ Run At    │ Status  │ Last Run         │ State   │
  | ├─────────────────────┼────────────────────────────────────┼───────────┼─────────┼──────────────────┼─────────┤
  | │ cache-purge         │ Grav\Common\Cache::purgeJob        │ * * * * * │ Success │ 2019-02-21 11:23 │ Enabled │
  | │ cache-clear         │ Grav\Common\Cache::clearJob        │ * * * * * │ Success │ 2019-02-21 11:23 │ Enabled │
  | │ default-site-backup │ Grav\Common\Backup\Backups::backup │ 0 3 * * * │ Ready   │ Never            │ Enabled │
  | │ pages-backup        │ Grav\Common\Backup\Backups::backup │ * 3 * * * │ Success │ 2018-09-20 09:55 │ Enabled │
  | │ ls-job              │ ls                                 │ * * * * * │ Success │ 2019-02-21 11:23 │ Enabled │
  | └─────────────────────┴────────────────────────────────────┴───────────┴─────────┴──────────────────┴─────────┘
  |
  | ![NOTE] For error details run "bin/grav scheduler -d"
```
  
 Le planificateur Grav est contrôlé par un fichier de configuration primaire. Ceci est situé dans `user/config/scheduler.yaml` et est nécessaire pour que n'importe quel travail soit `enabled` afin de s'exécuter.

Ci-dessous, la configuration montre les travaux qui sont disponibles et s'ils sont activés pour s'exécuter ou non. Définissez simplement une entrée sur `disabled` pour l'empêcher de s'exécuter.

```yaml
status:
   ls-job: enabled
   cache-purge: enabled
   cache-clear: enabled
   default-site-backup: enabled
   pages-backup: enabled
```

Pour voir plus de détails sur les **erreurs** potentielles ou pour voir la prochaine fois que le travail sera exécuté, vous pouvez utiliser la commande `/bin/grav scheduler -d` :

```console
$ | bin/grav scheduler -d
  |
  | Job Details
  | ===========
  |
  | ┌─────────────────────┬──────────────────┬──────────────────┬────────┐
  | │ Job ID              │ Last Run         │ Next Run         │ Errors │
  | ├─────────────────────┼──────────────────┼──────────────────┼────────┤
  | │ cache-purge         │ 2019-02-21 11:29 │ 2019-02-21 11:31 │ None   │
  | │ cache-clear         │ 2019-02-21 11:29 │ 2019-02-21 11:31 │ None   │
  | │ default-site-backup │ Never            │ 2019-02-22 03:00 │ None   │
  | │ pages-backup        │ 2018-09-20 09:55 │ 2019-02-22 03:00 │ None   │
  | │ ls-job              │ 2019-02-21 11:29 │ 2019-02-21 11:31 │ None   │
  | └─────────────────────┴──────────────────┴──────────────────┴────────┘
```

<h2 id="Exécution manuelle des tâches">Exécution manuelle des tâches.
<a href="#Exécution manuelle des tâches" class="toc-anchor after"></a></h2>

La commande CLI fournit un moyen simple d'exécuter manuellement n'importe quel travail. En fait, c'est ce que fait le planificateur lorsqu'il s'exécute périodiquement.

    $ | bin/grav scheduler

Cela exécutera les tâches en mode silencieux, mais vous pouvez également voir les détails de ce qui s'exécute en utilisant :

planificateur bin/grav -v

    $ | bin/grav scheduler -v

<h2 id="Grav System Jobs">Grav System Jobs.
<a href="#Grav System Jobs" class="toc-anchor after"></a></h2>

Le noyau Grav fournit quelques tâches prêtes à l'emploi. Celles-ci incluent certaines tâches de type maintenance utiles :

* `cache-purge` - Cette tâche est utile si vous utilisez la mise en cache `file` de Grav car elle efface les anciens fichiers qui ont expiré. Il s'agit d'une excellente tâche à planifier car sinon, il faudrait qu'un utilisateur efface manuellement les anciens caches. Si vous ne suivez pas cela et que votre espace de fichier est limité, vous risquez de manquer d'espace et de planter le serveur.

* `cache-clear` - L'effacement du cache est le travail qui fonctionne de la même manière que la commande `bin/grav clear` que vous exécuteriez manuellement. Vous pouvez configurer si vous souhaitez utiliser un effacement de cache `standard` ou la variante `all` qui supprime tous les fichiers et dossiers du dossier `cache/` pour un effacement de cache plus approfondi.

* `default-site-backup` - La tâche de sauvegarde par défaut disponible via la nouvelle configuration Grav Backup. Vous pouvez créer des configurations de sauvegarde personnalisées, et celles-ci seront également disponibles pour s'exécuter en tant que tâche planifiée.

<h2 id="Tâches personnalisées">Tâches personnalisées.
<a href="#Tâches personnalisées" class="toc-anchor after"></a></h2>

Le Grav Scheduler peut être configuré manuellement avec n'importe quel nombre de tâches personnalisées. Ceux-ci peuvent être configurés dans le même fichier de configuration `scheduler.yaml` référencé ci-dessus. Par exemple, le `ls-job` référencé ci-dessus serait configuré :

```yaml
1 | custom_jobs:
2 |   ls-job:
3 |     command: ls
4 |     args: '-lah'
5 |     at: '* * * * *'
6 |     output: logs/cron-ls.out
7 |     output_mode: overwrite
8 |    email: user@email.com
```

La commande doit être n'importe quel script local pouvant être exécuté à partir de la ligne de commande du terminal. Seuls les attributs `command` et `at` sont requis.

<h2 id="Tâches fournies par le plugin">Tâches fournies par le plugin.
<a href="#Tâches fournies par le plugin" class="toc-anchor after"></a></h2>

L'une des fonctionnalités les plus puissantes du Grav Scheduler est la possibilité pour les plugins tiers de fournir leurs propres tâches. Un excellent exemple de ceci est fourni par le plugin `TNTSearch`. TNTSearch est un moteur de recherche de texte complet qui nécessite que le contenu soit indexé avant de pouvoir être recherché. Ce travail d'indexation peut être effectué de différentes manières, mais le Grav Scheduler vous permet de réindexer votre contenu périodiquement plutôt que d'avoir à le faire manuellement.

La première étape consiste pour votre plugin à s'abonner à l'événement `onSchedulerInitialized()`. Et puis créez une méthode dans votre fichier de plugin qui peut ajouter une tâche personnalisée lorsqu'elle est appelée :

```php
1  | public function onSchedulerInitialized(Event $e): void
2  | {
3  |     $config = $this->config();
4  |
5  |     if (!empty($config['scheduled_index']['enabled'])) {
6  |         $scheduler = $e['scheduler'];
7  |         $at = $config['scheduled_index']['at'] ?? '* * * * *';
8  |         $logs = $config['scheduled_index']['logs'] ?? '';
9  |         $job = $scheduler->addFunction('Grav\Plugin\TNTSearchPlugin::indexJob', [], 'tntsearch-index');
10 |         $job->at($at);
11 |         $job->output($logs);
12 |         $job->backlink('/plugins/tntsearch');
13 |     }
14 | }
```

Ici, vous pouvez voir comment une configuration de planificateur pertinente est obtenue à partir des paramètres de configuration du plugin TNTSearch, puis un nouveau Job est créé avec une fonction **statique** appelée `indexJob()`.

