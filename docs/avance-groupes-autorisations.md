<h1 class="rem">Groupes et autorisations</h1>

<div class = "notice info">
Voir la <a href="../dashboard-faq">FAQ Grav Admin</a>, pour savoir comment gérer les utilisateurs.
</div>

<h2 id="Définir des groupes">Définir des groupes.
<a href="#Définir des groupes" class="toc-anchor after"></a></h2>

Par défaut, Grav ne fournit aucun groupe. Vous devez les définir.

Les groupes sont définis dans le fichier user/config/groups.yaml. Si ce fichier n'existe pas encore, créez-le.

Voici un exemple de définition de groupes d'utilisateurs :

```yaml
1  | registered:
2  |   icon: users
3  |   readableName: 'Registered Users'
4  |   description: 'The group of registered users'
5  |   access:
6  |     site:
7  |       login: true
8  | paid:
9  |   readableName: 'Paid Members'
10 |   description: 'The group of paid members'
11 |   icon: money
12 |   access:
13 |     site:
14 |       login: true
15 |       paid: true
16 | administrators:
17 |   groupname: administrators
18 |   readableName: Administrators
19 |   description: 'The group of administrators'
20 |   icon: child
21 |   access:
22 |     admin:
23 |       login: true
24 |     site:
25 |       login: true
```

Nous définissons ici 3 groupes.

<h2 id="Affectation d'un utilisateur à un groupe">Affectation d'un utilisateur à un groupe.
<a href="#Affectation d'un utilisateur à un groupe" class="toc-anchor after"></a></h2>

Chaque utilisateur peut être affecté à un groupe.

Ajoutez simplement

```yaml
1 | groups:
2 |   - paid
```

au fichier yaml d'un utilisateur sous `user/accounts`.

Vous pouvez ajouter plusieurs groupes :

```yaml
1 | groups:
2 |   - administrators
3 |   - another-group
```

Vous pouvez également modifier les informations de groupe d'un utilisateur via le plug-in d'administration.

<h2 id="Autorisations">Autorisations.
<a href="#Autorisations" class="toc-anchor after"></a></h2>

Les utilisateurs affectés à un groupe héritent des autorisations du groupe. Vous pouvez par exemple définir un groupe qui a la permission site.paid en ajoutant :

```yaml
1 | access:
2 |   site:
3 |     paid: false
```

à la définition de groupe dans user/config/groups.yaml.

Lorsqu'un utilisateur est affecté à ce groupe, il hérite de l'autorisation site.paid: true.

Lorsqu'un utilisateur appartient à plusieurs groupes, il suffit qu'un groupe fournisse une autorisation, et celle-ci sera ajoutée aux autorisations de l'utilisateur.
Ajustement des autorisations au niveau de l'utilisateur

Vous pouvez également affiner les autorisations au niveau de l'utilisateur, comme d'habitude. Avec les groupes, vous pouvez définir une autorisation globale et la refuser au niveau de l'utilisateur, en ajoutant

```yaml
1 | access:
2 |   site:
3 |     paid: false
```

au fichier yaml d'un utilisateur.

<div class = "notice info">
Voir la <a href="../dashboard-faq">FAQ Grav Admin</a>, pour en savoir plus sur les autorisations disponibles.
</div>

