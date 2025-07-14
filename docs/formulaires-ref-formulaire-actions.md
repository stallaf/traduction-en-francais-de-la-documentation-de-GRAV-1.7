<h1 class="rem">Référence : Actions avec formulaire</h1>

<h2 id="Actions avec formulaires">Actions avec formulaires.
<a href="#Actions avec formulaires" class="toc-anchor after"></a></h2>

Nous avons vu quelques exemples d'actions de formulaire dans l'exemple de formulaire simple ci-dessus. Détaillons les actions que vous pouvez utiliser.

<h3 id="E-mail">E-mail.
<a href="#E-mail" class="toc-anchor after"></a></h3>

Envoie un e-mail avec les options spécifiées.

Exemple:

```yaml
1 | process:
2 |   - email:
3 |      from: "{{ config.plugins.email.from }}"
4 |      to: "{{ config.plugins.email.to }}"
5 |      subject: "Contact by {{ form.value.name|e }}"
6 |      body: "{% include 'forms/data.html.twig' %}"
```

Envoie un e-mail à partir de l'adresse e-mail spécifiée dans la configuration du plugin Email, l'envoie à cette même adresse e-mail (c'est un formulaire de contact, nous nous l'envoyons). À moins que vous ne souhaitiez utiliser d'autres valeurs, vous pouvez librement omettre from et to, car ils sont déjà configurés par défaut pour utiliser ces valeurs. L'e-mail a l'objet et le corps définis. Dans ce cas, le corps est généré par le fichier `forms/data.html.twig`, qui se trouve dans le modèle actif (Antimatter et les autres thèmes principaux l'ont, mais il n'est pas garanti que tous les thèmes l'incluent).

Antimatter le met à

```twig
1 | {% for field in form.fields %}
2 |         <div><strong>{{ field.label }}</strong>:  {{ string(form.value(field.name)|e) }}</div>
4 | {% endfor %}
```

En bref, il boucle simplement les valeurs et les imprime dans le corps de l'e-mail.

<div class="notice info">
Reportez-vous à la documentation du plug-in de messagerie pour d'autres <a href="https://github.com/getgrav/grav-plugin-email#emails-sent-with-forms">options de messagerie de formulaire importantes</a>, notamment les <a href="https://github.com/getgrav/grav-plugin-email#multi-part-mime-messages">corps de message en plusieurs parties</a> (bon pour les scores anti-spam), `reply_to` et les <a href="https://github.com/getgrav/grav-plugin-email#sending-attachments">pièces jointes</a>.
</div>

<h3 id="Attribut d'e-mail dynamique">Attribut d'e-mail dynamique.
<a href="#Attribut d'e-mail dynamique" class="toc-anchor after"></a></h3>

Si vous souhaitez par exemple définir le champ `email.from` à partir d'une entrée Form, vous pouvez récupérer son contenu et l'utiliser de cette manière :

    from : "{{ form.value.email|e }}"

Dans ce cas, nous récupérons le champ "email" du formulaire et l'utilisons pour l'attribut "de". De cette façon, le propriétaire du site recevra un e-mail et pourra répondre directement à l'e-mail saisi dans le formulaire.

<h3 id="Redirection">Redirection.
<a href="#Redirection" class="toc-anchor after"></a></h3>

Redirige l'utilisateur vers une autre page. L'action est immédiate, donc si vous l'utilisez, vous devrez probablement la mettre en bas de la liste des actions.

```yaml
1 | process:
2 |   - redirect: '/forms/landing-page'
```

Vous pouvez également définir tout ou partie du champ de redirection `redirect` à partir d'une entrée de formulaire ou d'un champ de formulaire masqué. Vous pouvez obtenir son contenu et l'utiliser de cette manière :

    redirect: "/path to/location/{{ form.value.hiddenfield }}"

Dans ce cas, nous récupérons le champ "hiddenfield" du formulaire et l'utilisons pour la dernière partie de l'emplacement de redirection. Cela peut être utile lors de la création de formulaires qui, par exemple, redirigent vers un téléchargement une fois terminés.

<h3 id="Message">Message.
<a href="#Message" class="toc-anchor after"></a></h3>

Définit un message à afficher sur la page suivante. Fonctionne si vous définissez également une action d'affichage, qui redirige l'utilisateur vers une autre page. Notez que vous pouvez utiliser Twig dans le message si vous le souhaitez.

```yaml
1 | process:
2 |   - message: Thank you for your feedback!
3 |   - display: thankyou
```

<h3 id="Message de validation">Message de validation.
<a href="#Message de validation" class="toc-anchor after"></a></h3>

Vous pouvez utiliser l'action de message pour déclencher en cas d'échec de la validation. Par example:

```yaml
1 | username:
2 |    type: text
3 |    label: Username
4 |    validate:
5 |      required: true
6 |      message: My custom message when validation fails!
```

Cela vous permettra d'écrire un message personnalisé que les utilisateurs verront en cas d'échec de la validation.

<h3 id="Affichage">Affichage.
<a href="#Affichage" class="toc-anchor after"></a></h3>

Après avoir soumis le formulaire, l'utilisateur peut être redirigé vers une autre page. Cette page sera une sous-page du formulaire, donc par exemple, si votre formulaire se trouve dans `/form`, vous pouvez rediriger les utilisateurs vers `/form/thankyou` avec le code suivant :

```yaml
1 | process:
2 |   - display: thankyou
```

Le plugin Form fournit un modèle de données de formulaire `formdata` adapté à la page de destination du processus, car il génère le résultat de la soumission du formulaire. Dans l'exemple ci-dessus, vous pouvez créer une page pages`/form/thankyou/formdata.md`.

Si vous redirigez vers une sous-page, `display: thankyou` fonctionne parfaitement. Si vous redirigez vers un chemin de page absolu, comme `site.com/thankyou`, faites-le précéder de `/`, par exemple : `: /thankyou`.

Antimatter et les thèmes compatibles fournissent le modèle `formdata.html.twig` Twig, qui ressemble à ceci :

```twig
1  | {% extends 'partials/base.html.twig' %}
2  | 
3  | {% block content %}
4  | 
5  |     {{ content|raw }}
6  | 
7  |     <div class="alert">{{ form.message|e }}</div>
8  |     <p>Here is the summary of what you wrote to us:</p>
9  | 
10 |     {% include "forms/data.html.twig" %}
11 | 
12 | {% endblock %}
```

Si la page `thankyou/formdata.md` est

```yaml
1 - ---
2 | title: Email sent
3 | cache_enable: false
4 | process:
5 |     twig: true
6 | ---
7 | 
8 | ## Email sent!
```

La sortie sera une page avec le "Email envoyé!" titre, suivi d'un message de confirmation et des données du formulaire saisies dans la page précédente.

Vous pouvez utiliser n'importe quel type de page comme page de destination. Créez simplement le vôtre et définissez le type de page de destination en conséquence.

<h3 id="Sauvegarde">Sauvegarde.
<a href="#Sauvegarde" class="toc-anchor after"></a></h3>

Enregistre les données du formulaire dans un fichier. Le fichier est enregistré dans le dossier `user/data`, dans un sous-dossier nommé comme paramètre `form.name`. Le formulaire **doit** avoir un nom pour que cette action réussisse, et le sous-dossier doit être créé avec les autorisations appropriées avant que les données puissent y être enregistrées, car un nouveau répertoire ne sera pas créé s'il n'en existe pas. Par example:

<div class="notice info">
Le préfixe de fichier <code>fileprefix</code>et le corps <code>body</code>peuvent contenir le balisage Twig.
</div>

```yaml
1 | process:
2 |   - save:
3 |      fileprefix: feedback-
4 |      dateformat: Ymd-His-u
5 |      extension: txt
6 |      body: "{% include 'forms/data.txt.twig' %}"
7 |      operation: create
```

Le corps est tiré du fichier `templates/forms/data.html.twig` du thème, fourni par Antimatter et des thèmes mis à jour.

<div class="notice note">
L'opération <code>operation</code> peut être soit créer <code>create</code>(par défaut) pour créer un nouveau fichier par soumission de formulaire, soit ajouter <code>add</code> pour ajouter à un seul fichier.
</div>

<div class="notice note">
Notez que l'opération d'ajout nécessite maintenant un nom de fichier statique : à définir, voir l'exemple ci-dessous.
</div>

```yaml
1 | process:
2 |   - save:
3 |      filename: feedback.txt
4 |      body: "{% include 'forms/data.txt.twig' %}"
5 |      operation: add
```

<h3 id="Captcha">Captcha.
<a href="#Captcha" class="toc-anchor after"></a></h3>

Pour valider également le captcha côté serveur, ajoutez l'action de processus captcha.

```yaml
1 | process:
2 |   - captcha:
3 |   recaptcha_secret: ENTER_YOUR_CAPTCHA_SECRET_KEY
```

<div class="notice info">
Le <code>recaptcha_secret</code> est facultatif et utilisera les valeurs de configuration du plugin Form si vous les avez fournies ici.
</div>

<h3 id="Adresse IP de l'utilisateur">Adresse IP de l'utilisateur.
<a href="#Adresse IP de l'utilisateur" class="toc-anchor after"></a></h3>

Affichez l'adresse IP de l'utilisateur sur la sortie. Placez-le au-dessus des processus d'e-mail / de sauvegarde dans le 'form.md' pour vous assurer qu'il est utilisé par le ou les processus de sortie.

```yaml
1 | process:
2 |   - ip:
3 |      label: User IP Address
```

<h3 id="Horodatage">Horodatage.
<a href="#Horodatage" class="toc-anchor after"></a></h3>

Ajoutez un horodatage de soumission de formulaire à la sortie. Placez-le au-dessus des processus d'e-mail / de sauvegarde dans le 'form.md' pour vous assurer qu'il est utilisé par le ou les processus de sortie.

    process:
    - timestamp:
        label: Submission Timestamp

<h3 id="Réinitialiser le formulaire après envoi">Réinitialiser le formulaire après envoi.
<a href="#Réinitialiser le formulaire après envoi" class="toc-anchor after"></a></h3>

Par défaut, le formulaire n'est pas effacé après l'envoi. Ainsi, si vous n'avez pas d'action d'affichage `display`et que l'utilisateur est renvoyé à la page de formulaire, celle-ci est toujours remplie avec les données saisies. Si vous voulez éviter cela, ajoutez une action de réinitialisation `reset` :

```yaml
1 | process:
2 |   - reset: true
```

<h3 id="Mémoriser les valeurs des champs">Mémoriser les valeurs des champs.
<a href="#Mémoriser les valeurs des champs" class="toc-anchor after"></a></h3>

À l'aide de l'action mémoriser `remember`, vous pouvez autoriser vos utilisateurs à "rappeler" certaines valeurs de champ depuis la dernière fois qu'un formulaire a été soumis. Ceci est particulièrement utile pour les formulaires qui sont soumis à plusieurs reprises, comme une soumission anonyme qui nécessite des informations sur l'expéditeur.

<div class="notice note">
<a href="https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/autocomplete">HTML5</a> et le <a href="../formulaires-ref-options-formulaires">plugin Grav's Form</a> le fournissent déjà de manière limitée via le navigateur, alors utilisez-le. Cependant, vous pouvez constater que la saisie semi-automatique ne fonctionne pas de manière fiable pour certains utilisateurs et champs.
</div/>

<div class="notice note">
L'action de mémorisation <code>remember</code><strong>utilise des cookies</strong> pour stocker la dernière valeur, elle ne fonctionnera donc que sur le même appareil et navigateur où le navigateur est configuré pour les autoriser à partir de votre site.
</div>

Pour utiliser cette action, listez simplement les noms des champs que vous souhaitez mémoriser.

Par exemple, un formulaire de recommandation médicale en ligne est un bon cas d'utilisation. Ceux-ci sont généralement remplis à partir du même ordinateur avec certaines valeurs de champ qui changent rarement et sont ennuyeuses à remplir à plusieurs reprises.

```yaml
1 | process:
2 |   - remember:
3 |      - referrer-name
4 |      - referrer-address
5 |      - referrer-specialty
6 |      - preferred-practitioner    
```

<h3 id="Actions personnalisées">Actions personnalisées.
<a href="#Actions personnalisées" class="toc-anchor after"></a></h3>

Vous pouvez vous "accrocher" à un traitement de formulaire et effectuer n'importe quel type d'opération. Effectuez un traitement personnalisé, ajoutez des données pour une application Web en ligne, et même enregistrez-les dans une base de données.

Pour ce faire, dans le champ de traitement du formulaire, ajoutez votre propre nom d'action de traitement, par exemple "votreaction".

```yaml
1 | process:
2 |    yourAction: true
```

Ensuite, créez un plugin simple.

Dans son fichier PHP principal, inscrivez-vous à l'événement `onFormProcessed`

```php
1  | namespace Grav\Plugin;
2  | use Grav\Common\Plugin;
3  | use RocketTheme\Toolbox\Event\Event;
4  | 
5  | class EmailPlugin extends Plugin
6  | {
7  |     public static function getSubscribedEvents()
8  |     {
9  |         return [
10 |             'onFormProcessed' => ['onFormProcessed', 0]
11 |         ];
12 |     }
13 | }
```

Fournissez ensuite un gestionnaire pour l'action saveToDatabase :

```php
1  | public function onFormProcessed(Event $event)
2  |     {
3  |         $form = $event['form'];
4  |         $action = $event['action'];
5  |         $params = $event['params'];
6  | 
7  |         switch ($action) {
8  |             case 'yourAction':
9  |                 //do what you want
10 |         }
11 |     }
```

Si votre traitement risque de mal tourner et que vous souhaitez arrêter les prochaines actions de formulaire, qui sont exécutées en série, vous pouvez arrêter le traitement en appelant `stopPropagation` sur l'objet $event :

```php
1 | $event->stopPropagation();
2 | return;
```

Un exemple de code avec la gestion des formulaires est disponible dans le plug-in Form et dans les référentiels du plug-in Email.

<h3 id="Un exemple de gestion de formulaire personnalisé">Un exemple de gestion de formulaire personnalisé.
<a href="#Un exemple de gestion de formulaire personnalisé" class="toc-anchor after"></a></h3>

Le plugin Form offre cette possibilité d'envoyer des e-mails, d'enregistrer des fichiers, de définir des messages d'état et c'est vraiment pratique. Parfois, cependant, vous avez besoin d'un contrôle total. C'est par exemple ce que fait le plugin Login.

Il définit le frontmatter de la page `login.md ` :

```yaml
1  | title: Login
2  | template: form
3  | 
4  | form:
5  |     name: login
6  | 
7  |     fields:
8  |         - name: username
9  |           type: text
10 |           placeholder: Username
11 |           autofocus: true
12 | 
13 |         - name: password
14 |           type: password
15 |           placeholder: Password
```

Le plugin Forms génère et affiche correctement le formulaire. Notez qu'aucun processus `process`n'est défini.

Les boutons de formulaire `buttons` manquent également, car ils sont ajoutés manuellement dans `templates/login.html.twig`. C'est là que l'action `action` et la tâche `task` du formulaire sont également définies.

Dans ce cas, `task` est `login.login` et l'action `action` est définie sur l'URL de la page.

Lorsqu'un utilisateur appuie sur "Connexion" dans le formulaire, Grav appelle l'événement `onTask.login.login`.

`user/plugins/login/login.php` se connecte à `onTask.login.login` à son fichier `classes/controller.php`, et c'est là que l'authentification se produit.

