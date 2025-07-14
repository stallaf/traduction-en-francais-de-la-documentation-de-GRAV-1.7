<h1 class="rem">Sauvegardes</h1>

Le système de sauvegarde de Grav a été complètement réécrit pour Grav 1.6 afin de fournir plus de fonctionnalités. Les améliorations incluent :

* Intégration dans le nouveau Grav [Scheduler](/avance-planificateur) pour permettre aux sauvegardes hors ligne de s'exécuter quand vous le souhaitez
* Possibilité de créer plusieurs **profils** de sauvegarde, chacun avec son propre ensemble de fichiers, d'exclure les règles de chemin et de fichier et de programmer la configuration
* Nouvelles options de **purge automatique** basées sur `number`, `space` ou `time`.
* Nouvelle page dédiée aux sauvegardes dans la section **Outils** du plugin d'administration.

<h2 id="Configuration">Configuration.
<a href="#Configuration" class="toc-anchor after"></a></h2>

Pour la rétrocompatibilité, la configuration par défaut imite le système avant Grav 1.6, cependant, elle a maintenant une limite de 5 Go par défaut pour l'espace de sauvegarde. Vous devez copier le fichier de configuration par défaut (`system/config/backups.yaml`) dans votre `user/config/`.

<div class="notice info">Si vous utilisez le plugin admin et enregistrez la configuration, le fichier <code>user/config/backups.yaml</code> sera créé automatiquement.
</div>

La configuration par défaut est la suivante :

```yaml
1  | purge:
2  |     trigger: space
3  |     max_backups_count: 25
4  |     max_backups_space: 5
5  |     max_backups_time: 365
6  |
7  | profiles:
8  |   -
9  |     name: 'Default Site Backup'
10 |     root: '/'
11 |     schedule: false
12 |     schedule_at: '0 3 * * *'
13 |     exclude_paths: "/backup\r\n/cache\r\n/images\r\n/logs\r\n/tmp"
14 |     exclude_files: ".DS_Store\r\n.git\r\n.svn\r\n.hg\r\n.idea\r\n.vscode\r\nnode_modules"
```

<h3 id="Purge">Purge.
<a href="#Purge" class="toc-anchor after"></a></h3>

* `space` - purgera les anciennes sauvegardes lorsque vous atteignez la limite d'espace. Contrôlé par `max_backups_space` mesuré en `GB`
* `time` - purgera les anciennes sauvegardes au-delà d'un certain nombre de jours. Contrôlé par `max_backups_time` mesuré en `days`
* `number` - purgera les anciennes sauvegardes au-delà d'un certain nombre de sauvegardes. Contrôlé par `max_backups_count`.

<h3 id="Profils">Profils.
<a href="#Profils" class="toc-anchor after"></a></h3>

Un tableau de profils peut être configuré. Le profil `Default Site Backup` est configuré de la même manière que la sauvegarde Grav par défaut dans les versions précédentes. Par défaut, la sauvegarde n'est pas automatiquement traitée par le planificateur, mais vous pouvez définir `schedule: true` et configurer l'option `schedule_at`: avec une expression [cron préférée](https://crontab.guru/).

Un exemple d'ensemble de profils plus complexe pourrait être :

```yaml
1  | profiles:
2  |   -
3  |     name: 'Default Site Backup'
4  |     root: /
5  |     exclude_paths: "/backup\r\n/cache\r\n/images\r\n/logs\r\n/tmp"
6  |     exclude_files: ".DS_Store\r\n.git\r\n.svn\r\n.hg\r\n.idea\r\n.vscode\r\nnode_modules"
7  |     schedule: true
8  |     schedule_at: '0 4 * * *'
9  |  -
10 |     name: 'Pages Backup'
11 |     root: 'page://'
12 |     exclude_files: .git
13 |     schedule: true
14 |     schedule_at: '* 3 * * *'
```

<h2 id="Commande CLI">Commande CLI.
<a href="#Commande CLI" class="toc-anchor after"></a></h2>

Ceci est couvert plus en détail dans la section [Cli Console -> Grav Command](/cli-commandes-grav), mais voici un exemple d'exécution manuelle de la sauvegarde :

```console
$ | cd ~/workspace/portfolio
$ | bin/grav backup
  |
  | Grav Backup
  | ===========
  |
  | Choose a backup?
  |   [0] Default Site Backup
  |   [1] Pages Backup
  | 
  | Archiving 36 files [===================================================] 100% < 1 sec Done...
  |
  |  [OK] Backup Successfully Created: /users/joe/workspace/portfolio/backup/pages_backup--20190227120510.zip
```

