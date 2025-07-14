<h1 class="rem">Référence : Index des champs de formulaire</h1>

<h2 id="Attributs de champ communs">Attributs de champ communs.
<a href="#Attributs de champ communs" class="toc-anchor after"></a></h2>

Chaque champ accepte une liste d'attributs que vous pouvez utiliser. Chaque champ peut partager ces attributs communs, mais des champs particuliers peuvent les ignorer. La meilleure façon de vérifier quels attributs sont autorisés sur un champ est de vérifier la description du champ dans cette page et de voir quels attributs sont mentionnés.

Cette liste fournit un terrain d'entente, il n'est donc pas nécessaire de répéter la description d'un champ commun.

| **ATTRIBUT**       | **DESCRIPTION**
| ---------          | ---------
| <a name="autocomplete"></a>`autocomplete`  | Accepte `on` ou`off`
| <a name="autofocus"></a>`autofocus`        | Si activé, autofocus sur ce champ
| <a name="classes"></a>`classes`            | Accepte une chaîne avec une ou plusieurs classes CSS à ajouter
| <a name="default"></a>`default`            | Définit la valeur par défaut du champ
| <a name="disabled"></a>`disabled`          | Définit l'état désactivé du champ
| <a name="help"></a>`help`                  | Ajoute une info-bulle au champ
| <a name="id"></a>`id`                      | Définit l'identifiant du champ. Définit également l'attribut `for` sur l'étiquette
| <a name="label"></a>`label`                | Définit le libellé du champ
| <a name="display_label"></a>`display_label` | Accepte `true` ou `false`
| <a name="labelclasses"></a>`labelclasses`  | Accepte une chaîne avec une ou plusieurs classes CSS à ajouter
| <a name="name"></a>`name`                  | Définit le nom du champ
| <a name="novalidate"></a>`novalidate`      | Définit l'état novalidate du champ
| <a name="outerclasses"></a>`ouyterclasses` | Classes ajoutées à la div qui incluent l'étiquette et le champ
| <a name="placeholder"></a>`placeholder`    | Définit la valeur de l'espace réservé du champ
| <a name="readonly"></a>`readonly`          | Définit l'état du champ en lecture seule
| <a name="size"></a>`size`                  | Définit la taille du champ, qui à son tour ajoute une classe à son conteneur. <br>Les valeurs valides sont `large`, `x-small`, `medium`, `long`, `small`. Vous pouvez <br>bien sûr en ajouter plus dans le modèle que vous voyez, lorsqu'il est utilisé dans <br>le frontend
| `style`            | Définit le style du champ
| `title`            | Définit la valeur du titre du champ
| `type`             | Définit le type de champ
| `validate.required` | S'il est défini sur une valeur positive, définit le champ comme requis.
| `validate.pattern`  | Définit un modèle de validation
| `validate.message`  | Définit le message affiché si la validation échoue

Pour ajouter des attributs personnalisés, vous pouvez utiliser :

`attributes:
  key: value`

Pour ajouter des valeurs data-* personnalisées, vous pouvez utiliser :

`datasets:
  key: value`
                  
Les définitions d'`attributes` et de `datasets` présentées ci-dessus conduisent à la définition de champ suivante :

`<input name="data[name]" value="" type="text" class="form-input " key="value" data-key="value">`

<div class="notice tip">
REMARQUE : Vous pouvez définir des valeurs positives de plusieurs manières : <code>'on'</code>, <code>true</code>, <code>1</code>. Les autres valeurs sont interprétées comme négatives.
</div>
---

<h2 id="Champs disponibles">Champs disponibles.
<a href="#Champs disponibles" class="toc-anchor after"></a></h2>

<h3 id="Champ Captcha de base">Champ Captcha de base.
<a href="#Champ Captcha de base" class="toc-anchor after"></a></h3>

Ajouté dans Forms `7.0.0` comme alternative locale au champ Google ReCaptcha. Ce champ est particulièrement utile lorsque vous traitez du SPAM dans les formulaires de contact lorsque vous ne voulez pas vous soucier des tracas ou peut-être des restrictions GPDR qui accompagnent l'offre de Google. Il utilise des polices **résistantes à l'OCR** pour dissuader les attaques et peut être configuré avec des codes à copier ou de simples questions mathématiques.

![Captcha basique](https://learn.getgrav.org/user/pages/06.forms/02.forms/02.fields-available/basic-captcha_field.gif)

Le type de champ `basic-captcha` est entièrement configurable via la configuration de `forms` mais est livré avec des valeurs par défaut raisonnables. La configuration globale de Basic-Captcha est configurée dans votre fichier de configuration de formulaire global (généralement `user/config/plugins/form.yaml`). Les options par défaut sont :

```yaml
1  | basic_captcha:
2  |     type: characters            # options: [characters | math]
3  |     chars:
4  |        length: 6                 # number of chars to output
5  |        font: zxx-noise.ttf       # options: [zxx-noise.ttf | zxx- camo.ttf | zxx-xed.ttf  | zxx-sans.ttf]
6  |        bg: '#cccccc'             # 6-char hex color
7  |        text: '#333333'           # 6-char hex color
8  |        size: 24                  # font size in px
9  |        start_x: 5                # start position in x direction in px
10 |        start_y: 30               # start position in y direction in px
11 |        box_width: 135            # box width in px
12 |        box_height: 40            # box height in px
13 |     math:
14 |        min: 1                    # smallest digit
15 |        max: 12                   # largest digit
16 |        operators: ['+','-','*']  # operators that can be used in math
```

Exemple:

```yaml
1 | basic-captcha:
2 |   type: basic-captcha
3 |   placeholder: copy the 6 characters
4 |   label: Are you human?
```
Cela nécessite également un élément `process:` matching pour garantir que le formulaire est correctement validé.

<div class="notice note">
Il doit s'agir de la première entrée de la section <code>process :</code> du formulaire pour garantir que le formulaire ne sera pas traité en cas d'échec de la validation captcha.
</div>

Exemple:

```yaml
1 | process:
2 |   basic-captcha:
3 |      message: Humanity verification failed, please try again...
```

<h3 id="Champ Captcha du tourniquet (Cloudflare)">Champ Captcha du tourniquet (Cloudflare).
<a href="#Champ Captcha du tourniquet (Cloudflare)" class="toc-anchor after"></a></h3>

Depuis Form `v7.1.0`, Grav ajoute la prise en charge du nouveau champ Cloudflare Turnstile. Ce champ est une nouvelle façon de prévenir le SPAM dans les formulaires et constitue une excellente alternative au champ Google ReCaptcha et aux restrictions **GPDR** fournies avec l'offre de Google. Ce champ est particulièrement utile lorsqu'il s'agit de spam dans les formulaires de contact. [En savoir plus sur le tourniquet](https://blog.cloudflare.com/fr-fr/turnstile-private-captcha-alternative-fr-fr/).  <a href="#ndt1" class="ndt">(1)</a>

<h4 id="Avantages par rapport à Google ReCaptcha">Avantages par rapport à Google ReCaptcha.
<a href="#Avantages par rapport à Google ReCaptcha" class="toc-anchor after"></a></h4>

1. Conforme au RGPD et axé sur la confidentialité des utilisateurs ;
2. Vérification de défi extrêmement rapide ;
3. Très simple à mettre en œuvre à la fois dans Cloudflare et Grav, pas d'interface utilisateur ni de paramètres complexes à configurer ;
4. Pas de solutions de contournement sophistiquées pour les soumissions de formulaires asynchrones (ajax), cela fonctionne ! ;
5. Expérience utilisateur exceptionnelle par rapport à ReCaptcha, plus besoin de compter les voitures, les feux tricolores ou autres bêtises ;
6. Construit sur l'apprentissage automatique, il s'améliorera au fil du temps et s'adaptera aux nouveaux vecteurs d'attaque ;
7. Analyses exhaustives sur l'efficacité du défi, [voir capture d'écran](https://blog.cloudflare.com/content/images/2022/09/image1-64.png).

<h4 id="L'intégration">L'intégration.
<a href="#L'intégration" class="toc-anchor after"></a></h4>

Avant d'intégrer Grav Forms avec Turnstile, vous devez d'abord [créer un nouveau site Turnstile](https://dash.cloudflare.com/login), vous pouvez également suivre le [tutoriel officiel "démarrer"](https://developers.cloudflare.com/turnstile/get-started/). Ici, vous pouvez également choisir le type de widget que vous souhaitez utiliser, il peut être soit géré, non interactif ou invisible. Il est important de noter que vous ne pouvez changer le type de widget que depuis Cloudflare, vous ne pourrez pas le configurer via Grav. Cependant, si un choix n’est pas satisfait, vous pourrez le modifier plus tard si nécessaire. [Apprenez-en davantage sur les différents types de widgets](https://developers.cloudflare.com/turnstile/reference/widget-types/).

<div class="notice note">
Assurez-vous d'ajouter tout domaine sur lequel vous pourriez avoir besoin d'utiliser le champ Tourniquet, cela peut inclure votre environnement local.
</div>

Une fois que vous avez créé un site, vous recevrez un `site_key` et un `site_secret` que vous devrez configurer dans le fichier de configuration de votre formulaire (généralement `user/config/plugins/form.yaml`). Vous pouvez ignorer la balise de script suggérée, car Grav s'en charge pour vous.

Les options par défaut sont :

```yaml
1 | turnstile:
2 |   theme: light
3 |   site_key: <Your Turnstile Site Key>
4 |   secret_key: <Your Turnstile Secret Key>
```

Enfin, vous aurez également besoin d'un élément matching `process:` pour garantir que le formulaire soit correctement validé.

<div class="notice note">
Il doit s'agir de la première entrée de la section <code>process :</code> du formulaire pour garantir que le formulaire ne sera pas traité en cas d'échec de la validation captcha.
</div>

<h4 id="Exemple">Exemple.
<a href="#Exemple" class="toc-anchor after"></a></h4>

Un exemple typique de formulaire de contact ressemblerait à ce qui suit.

```yaml
1  | form:
2  |   name: contact
3  |   fields:
4  |     name:
5  |       label: Name
6  |       type: text
7  |       validate:
8  |         required: true
9  |     email:
10 |       label: Email
11 |       type: email
12 |       validate:
13 |         required: true
14 |     message:
15 |       label: Message
16 |       type: textarea
17 |       validate:
18 |         required: true
19 |     captcha:
20 |        type: turnstile
21 |        theme: light
22 |   buttons:
23 |     submit:
24 |       type: submit
25 |       value: Submit
26 |   process:
27 |     turnstile: true
28 |     email:
29 |       subject: "[Acme] {{ form.value.name|e }}"
30 |       reply_to: "{{ form.value.name|e }} <{{ form.value.email }}>"
31 |     message: Thanks for contacting us!
32 |     reset: true
33 |     display: '/'
```

<h3 id="Champ Google Captcha (ReCaptcha)">Champ Google Captcha (ReCaptcha).
<a href="#Champ Google Captcha (ReCaptcha)" class="toc-anchor after"></a></h3>

Le type de champ `captcha` est utilisé pour ajouter un élément Google reCAPTCHA à votre formulaire. Contrairement à d'autres éléments, il ne peut être utilisé qu'une seule fois dans un formulaire.

<div class="notice note">
Vous devez configurer vos configurations Google reCAPTCHA dans la <a href="https://www.google.com/recaptcha/admin">console d'administration reCAPTCHA</a>.
</div>

Depuis la version `3.0`, le champ prend en charge 3 variantes de reCAPTCHA. La configuration globale de reCAPTCHA est mieux définie dans votre fichier de configuration de formulaire global (généralement `user/config/plugins/form.yaml`). Les options par défaut sont :

```yaml
1 | recaptcha:
2 |   version: 2-checkbox
3 |   theme: light
4 |   site_key:
5 |   secret_key:
```

Ces options doivent être définies en fonction des éléments suivants :

| **CLEF**        | **VALEURS**
| ---------       | ---------
| version         | Par défaut à `2-checkbox`, mais peut également être `2-invisible` ou `3`
| theme           | Par défaut `light`, mais peut aussi être `dark` (ne fonctionne actuellement que pour <br>les versions `2-x`)
| site_key        | Votre clé de site Google
| secret_key      | Votre clé secrète Google

<div class="notice info">
Veuillez vous assurer que le domaine du site est répertorié dans la configuration reCAPTCHA de Google.
</div>

Dans la définition du formulaire, l'attribut `name` du champ captcha doit être `g-recaptcha-response`. La raison en est que Google reCAPTCHA stocke le code de confirmation Captcha dans un champ nommé `g-recaptcha-response`.

Exemple:

```yaml
1 | g-recaptcha-response:
2 |   type: captcha
3 |   label: Captcha
```

Vous pouvez également fournir un message d'échec `recaptcha_not_validated` personnalisé, mais si vous ne le faites pas, celui par défaut est fourni par le plugin Form. Si vous souhaitez définir un `recaptcha_site_key` spécifique au formulaire plutôt que de le définir globalement dans la configuration du formulaire, vous pouvez également le définir.

```yaml
1 | g-recaptcha-response:
2 |   type: captcha
3 |   label: Captcha
4 |   recaptcha_site_key: ENTER_YOUR_CAPTCHA_PUBLIC_KEY
5 |   recaptcha_not_validated: 'Captcha not valid!'
```

| **ATTRIBUT**                | **DESCRIPTION**
| ---------                   | ----------
| `recaptcha_site_key`        | La clé de site Google reCAPTCHA (facultatif)
| `recaptcha_not_validated`   | Le message à afficher si le captcha n'est pas valide

| **Attributs communs autorisés**  |
| ---------                        |
| [help](#help)                    |
| [label](#label)                  |
| [name](#name)                    | 
| [outerclasses](#outerclasses)    |
| [validate.required](#validate.required)   |

Cela nécessite également un élément `process: matching` pour garantir que le formulaire est correctement validé.

<div class="notice note">
Il doit s'agir de la première entrée de la section <code>process :</code> du formulaire pour garantir que le formulaire ne sera pas traité si la validation ReCaptcha échoue.
</div>

```yaml
1 | process:
2 |   captcha: true
```   

<h4 id="Validation Captcha côté serveur">Validation Captcha côté serveur
<a href="#Validation Captcha côté serveur" class="toc-anchor after"></a></h4>

Le code ci-dessus validera le Captcha dans le frontend et empêchera la soumission du formulaire s'il n'est pas correct. Pour valider également le captcha côté serveur, ajoutez l'action captcha process à vos formulaires :

```yaml
1 | process:
2 |   captcha: true
```  

Vous pouvez également fournir un `message` de réussite facultatif, mais si vous ne le faites pas, aucun `message` spécifique ne s'affichera en cas de réussite. Si vous souhaitez définir un `recaptcha_secret` spécifique au formulaire plutôt que de le définir globalement dans la configuration du formulaire, vous pouvez également le définir.

```yaml
1 | process:
2 |   captcha:
3 |      recaptcha_secret: ENTER_YOUR_CAPTCHA_SECRET_KEY
4 |      message: 'Successfully passed reCAPTCHA!'
```

[Voir l'exemple de formulaire de contact](formulaires-ex-formulaire-contact.md) pour le voir en action.

- - - 

<h3 id="Champ d'une case à cocher">Champ d'une case à cocher.
<a href="#Champ d'une case à cocher" class="toc-anchor after"></a></h3>

<img class="img"src="https://learn.getgrav.org/user/pages/06.forms/02.forms/02.fields-available/checkbox_field.gif" alt="Champ d'une case à cocher">

Le type de champ `checkbox` est utilisé pour ajouter une seule case à cocher à votre formulaire.

Exemple:

```yaml
1 | agree_to_terms:
2 |   type: checkbox
3 |   label: "Agree to the terms and conditions"
4 |   validate:
5 |       required: true
```

| **Attributs communs autorisés**      |      
| ---------                            |   
| [autofocus](#autofocus)              |
| [classes](#classes)                  |
| [default](#default)                  |
| [disabled](#disabled)                |
| [id](#id)                            |
| [label](#label)                      |
| [name](#name)                        |
| [novalidate ](#novalidate)           |
| [outerclasses](#outerclasses)        |
| [size](#size)                        |
| [style](#style)                      |
| [validate.required](#validate.required) |
| [validate.pattern](#validate.pattern)|
| [validate.message](#validate.message)|

- - - 

<h3 id="Champ de plusieurs cases à cocher">Champ de plusieurs cases à cocher.
<a href="#Champ de plusieurs cases à cocher" class="toc-anchor after"></a></h3>

<img class="img"src="https://learn.getgrav.org/user/pages/06.forms/02.forms/02.fields-available/checkboxes_field.gif" alt="Champ de plusieurs cases à cocher">

Le type de champ `checkboxes` est utilisé pour ajouter un groupe de cases à cocher à votre formulaire.

Exemples:

```yaml
1  | pages.process:
2  |    type: checkboxes
3  |    label: PLUGIN_ADMIN.PROCESS
4  |    help: PLUGIN_ADMIN.PROCESS_HELP
5  |    default:
6  |        markdown: true
7  |        twig: true
8  |    options:
9  |        markdown: Markdown
10 |        twig: Twig
11 |    use: keys
```

```yaml
1 | my_field:
2 |     type: checkboxes
3 |     label: A couple of checkboxes
4 |     default:
5 |         - option1
6 |         - option2
7 |     options:
8 |         option1: Option 1
9 |         option2: Option 2
```

| **ATTRIBUT**    | **DESCRIPTION**
| ---------       | ---------
| `use`           | Lorsqu'elle est définie sur `keys`, la case à cocher stockera la valeur de la clé de l'élément <br>lors de la soumission du formulaire. Sinon, il utilisera la valeur de l'élément.
| `options`       | Un tableau d'options clé-valeur qui seront autorisées.

| **Attributs communs autorisés**         |      
| ---------                               |   
| [autofocus](#autofocus)                 |
| [classes](#classes)                     |
| [default](#default)                     |
| [disabled](#disabled)                   |
| [help](#help)                           |
| [id](#id)                               |
| [label](#label)                         |
| [name](#name)                           |
| [outerclasses](#outerclasses)           |
| [size](#size)                           |
| [style](#style)                         |
| [validate.required](#validate.required) |
| [validate.pattern](#validate.pattern)   |
| [validate.message](#validate.message)   |

<div class="notice info">
REMARQUE : Le champ des cases à cocher ne prend pas en charge l'action de processus <code>remember</code>
</div>

- - - 

<h3 id="Champ conditionnel">Champ conditionnel.
<a href="#Champ conditionnel" class="toc-anchor after"></a></h3>

Le type de champ `conditional` est utilisé pour afficher conditionnellement d'autres champs basés sur une condition.

Exemples:

Si votre condition renvoie déjà un `true` ou `false`, vous pouvez simplement utiliser ce format simplifié :

```yaml
1 | my_conditional:
2 |   type: conditional
3 |   condition: config.plugins.yourplugin.enabled
4 |   fields: # The field(s) below will be displayed only if the plugin named yourplugin is enabled
5 |      header.mytextfield:
6 |         type: text
7 |         label: A text field
```

Cependant, si vous avez besoin de conditions plus complexes, vous pouvez exécuter une logique qui renvoie `'true'` ou `'false`' sous forme de chaînes, et le champ le comprendra également.

```yaml
1 | my_conditional:
2 |   type: conditional
3 |   condition: "config.site.something == 'custom'"
4 |  fields: # The field(s) below will be displayed only if the `site` configuration option `something` equals `custom`
5 |     header.mytextfield:
6 |         type: text
7 |         label: A text field
```

| **ATTRIBUT**       | **DESCRIPTION**
| ---------       | ---------
| `condition`     | La condition évaluée par twig. Toute variable accessible par twig peut être évaluée

| **Attributs communs autorisés**   |    
| ---------                         |   
| [disabled](#disabled)             |
| [id](#id)                         |
| [label](#labael)                  |
| [name](#name)                     |

- - - 

<h3 id="Champ Date">Champ Date.
<a href="#Champ Date" class="toc-anchor after"></a></h3>

![Champ Date](https://learn.getgrav.org/user/pages/06.forms/02.forms/02.fields-available/date_field.gif)

Le type de champ `date` est utilisé pour ajouter un champ de saisie de `date` HTML5.

Exemple:

```yaml
-
  type: date
  label: Enter a date
  validate.min: "2014-01-01"
  validate.max: "2018-12-31"
```

| **ATTRIBUT**       | **DESCRIPTION**
| ---------       | ---------
| `validate.min`  | Définit l'attribut `min` du champ <br> (voir [http://html5doctor.com/the-woes-of-date-input/#feature-min-max-attributes](http://html5doctor.com/the-woes-of-date-input/#feature-min-max-attributes))
| `validate.max`  | Définit l'attribut `max` du champ <br>(voir [http://html5doctor.com/the-woes-of-date-input/#feature-min-max-attributes](http://html5doctor.com/the-woes-of-date-input/#feature-min-max-attributes))

| **Attributs communs autorisés**         |      
| ---------                               |   
| [autofocus](#autofocus)                 |
| [classes](#classes)                     |
| [default](#default)                     |
| [disabled](#disabled)                   |
| [help](#help)                           |
| [id](#id)                               |
| [label](#label)                         |
| [name](#name)                           |
| [novalidate](#novalidate)               |
| [outerclasses](#outerclasses)           |
| [readonly](#readonly)                   |
| [size](#size)                           |
| [style](#style)                         |
| [title](#title)                         |
| [validate.required](#validate.required) |
| [validate.pattern](#validate.pattern)   |
| [validate.message](#validate.message)   |

- - - 

<h3 id="Champ d'affichage">Champ d'affichage.
<a href="#Champ d'affichage" class="toc-anchor after"></a></h3>

![Champ d'affichage](https://learn.getgrav.org/user/pages/06.forms/02.forms/02.fields-available/display_field.jpg)

Le type de champ d'affichage `display` est utilisé pour afficher du texte ou des instructions à l'intérieur du formulaire. Peut accepter le contenu de démarquage

Exemple:

```yaml
1 | test:
2 |     type: display
3 |     size: large
4 |     label: Instructions
5 |     markdown: true
6 |     content: "This is a test of **bold** and _italic_ in a text/display
7 |     field\n\nanother paragraph...."
```

| **ATTRIBUT**    | **DESCRIPTION**
| ---------       | ---------
| `markdown`      | Valeur booléenne qui active le traitement de démarque sur le champ de contenu.
| `content`       | Le contenu textuel à afficher.

| **Attributs communs autorisés**   |      
| ---------                         |   
| [help](#help)                     |
| [label](#label)                   |
| [name](#name)                     |
| [id](#id)                         |
| [outerclasses](#outerclasses)     |
| [size](#size)                     |
| [style](#style)                   |

- - - 

<h3 id="Champ e-mail">Champ e-mail.
<a href="#Champ e-mail" class="toc-anchor after"></a></h3>

![Champ e-mail](https://learn.getgrav.org/user/pages/06.forms/02.forms/02.fields-available/email_field.gif)

Le type de champ `email` est utilisé pour présenter un champ de saisie de texte qui accepte un email, en utilisant [l'entrée email HTML5.](http://html5doctor.com/html5-forms-input-types/#input-email)

<div class="notice info">
Les e-mails sont insensibles à la casse par conception. Assurez-vous que la logique de votre application gère correctement les e-mails en majuscules, minuscules ou mixtes.
</div>

Exemple:

```yaml
1 | header.email:
2 |   type: email
3 |   autofocus: true
4 |   label: Email
```

| **ATTRIBUT**    | **DESCRIPTION**
| ---------       | ---------
| `minlength`     | Longueur minimale du texte.
| `maxlength`     | Longueur maximale du texte.
| `validate.min`  | Identique à minlength.
| `validate.max`  | Identique à maxlength.

| **Attributs communs autorisés**         |     
| ---------                               |   
| [autofocus](#autofocus)                 |
| [classes](#classes)                     |
| [default](#dafault)                     |
| [disabled](#disabled)                   |
| [help](#help)                           |
| [id](#id)                               |
| [label](#label)                         |
| [name](#name)                           |
| [novalidate](#novalidate)               |
| [outerclasses](#outerclasses)           |
| [readonly](#readonly)                   |
| [size](#size)                           |
| [style](#style)                         |
| [title](#title)                         |
| [validate.required](#validate.required) |
| [validate.pattern](#validate.pattern)   |
| [validate.message](#validate.message)   |

- - - 

<h3 id="Champ de fichier">Champ de fichier.
<a href="#Champ de fichier" class="toc-anchor after"></a></h3>

Avec le type de champ de fichier`file`, vous pouvez permettre aux utilisateurs de télécharger des fichiers via le formulaire. Le champ par défaut n'autorise **qu'un seul fichier**, de type **image** et sera téléchargé sur la page **courante** où le formulaire a été déclaré.

```yaml
1 | # Default settings
2 | my_files:
3 |   type: file
4 |   multiple: false
5 |   destination: 'self@'
6 |   accept:
7 |     - image/*

```

| **ATTRIBUT**    | **DESCRIPTION**
| ---------       | ---------
| `multiple`      | Peut être `true` ou `false`, lorsqu'il est défini sur **true**,<br> plusieurs fichiers peuvent être sélectionnés en même temps.
| `destination`   | Peut être **@self, @page:/route, local/rel/path/** ou un flux PHP.<br> Mis à **@self**, les fichiers seront téléchargés là où  le formulaire a été déclaré (.md actuel).<br> L'utilisation de **@page:/route** sera téléchargée vers l'itinéraire spécifié d'une page, si elle<br> existe (par exemple, **@page:/blog/a-blog-post**).<br> Défini sur « local/rel/path » : peut être n'importe quel chemin relatif à l'instance Grav. Par<br> exemple, utilisateur/images/téléchargements. Si le chemin n'existe pas, il sera créé, alors<br>  assurez-vous qu'il est accessible en écriture.<br> Vous pouvez également définir la valeur sur n'importe quel flux PHP valide reconnu par<br> Grav comme user-data://my-form ou theme://media/uploads.
| `accept`        | Prend un tableau de types MIME autorisés. Par exemple, pour autoriser uniquement les<br>  fichiers gifs et mp4 : `accept: ['image/gif', 'video/mp4']`.

<div class="notice tip">
Le champ Fichier dans l'administrateur est un peu différent, permettant également de supprimer un fichier téléchargé sur un formulaire, car le cas d'utilisation dans l'administrateur est de télécharger puis d'associer un fichier à un champ.
</div>

| **Attributs communs autorisés**         |     
| ---------                               |   
| [help](#help)                           |
| [label](#label)                         |
| [name](#name)                           |
| [outerclasses](#outerclasses)           |

Par défaut, dans Admin, le champ de fichier `file` écrasera un fichier téléchargé portant le même nom qu'un fichier plus récent, contenu dans le même dossier que celui dans lequel vous souhaitez le télécharger, sauf si vous définissez `avoid_overwriting` sur `true` dans la définition du champ.

- - - 

<h3 id="Champ caché">Champ caché.
<a href="#Champ caché" class="toc-anchor after"></a></h3>

Le type de champ masqué `hidden` permet d'ajouter un élément masqué à un formulaire.

Exemple:

```yaml
1 | header.some_field:
2 |   type: hidden
3 |   default: my-value
```

| **ATTRIBUT**    | **DESCRIPTION**
| ---------       | ---------
| `name`          | Le nom du champ. S'il est manquant, le nom du champ est obtenu à partir de l'élément<br> de définition du champ (dans l'exemple ci-dessus : `header.some_field`)

| **Attributs communs autorisés**  |      
| ---------                        |   
| [default](#default)              |

- - - 

<h3 id="Champ pot de miel">Champ pot de miel.
<a href="#Champ pot de miel" class="toc-anchor after"></a></h3>

Le type de champ pot de miel `honeypot` crée un champ caché qui, une fois rempli, renverra une erreur. C'est un moyen utile d'empêcher les robots de remplir et de soumettre un formulaire.

Exemple:

```yam
1 |   fields:
2 |      honeypot:
3 |         type: honeypot
```

Il s'agit d'un simple champ de texte qui n'apparaît pas sur le front-end. Les robots, qui détectent les champs dans le code et les remplissent automatiquement, rempliront probablement le champ. L'erreur empêche ce formulaire d'être correctement soumis. L'erreur revient à côté de l'élément de formulaire, plutôt qu'en haut dans un bloc de message.

Un champ de pot de miel est une alternative populaire aux champs de captcha.

- - - 

<h3 id="Champ ignore">Champ ignore.
<a href="#Champ ignore" class="toc-anchor after"></a></h3>

Le type de champ Ignorer `ignore` peut être utilisé pour supprimer les champs inutilisés lors de l'extension à partir d'un autre Blueprint

Exemple:

```yaml
1 | header.process:
2 |   type: ignore
3 |   content:
4 |   type: ignore
```

- - - 

<h3 id="Champ nombre">Champ nombre.
<a href="#Champ nombre" class="toc-anchor after"></a></h3>

Le type de champ nombre `number` est utilisé pour présenter un champ de saisie de texte qui n'accepte que des nombres, en utilisant l'entrée numérique HTML5.

Exemple:

```yaml
1 | header.count:
2 |   type: number
3 |   label: 'How Much?'
4 |   validate:
5 |     min: 10
6 |     max: 360
7 |     step: 10
```

| **ATTRIBUT**       | **DESCRIPTION**
| ---------       | ---------
| `valider.min`   | Valeur minimale.
| `valider.max`   | Valeur maximale.
| `valider.step`  | Qui incrémente pour passer à la vitesse supérieure

| **Attributs communs autorisés**         |      
| ---------                               |   
| [autofocus](#autofocus)                 |
| [classes](#classes)                     |
| [default](#dafault)                     |
| [disabled](#disabled)                   |
| [help](#help)                           |
| [id](#id)                               |
| [label](#label)                         |
| [name](#name)                           |
| [novalidate](#novalidate)               |
| [outerclasses](#outerclasses)           |
| [readonly](#readonly)                   |
| [size](#size)                           |
| [style](#style)                         |
| [title](#title)                         |
| [validate.required](#validate.required) |
| [validate.pattern](#validate.pattern)   |
| [validate.message](#validate.message)   |

- - - 

<h3 id="Champ Mot de passe">Champ Mot de passe.
<a href="#Champ Mot de passe" class="toc-anchor after"></a></h3>

Le type de champ de mot de passe `password` est utilisé pour présenter un champ de saisie de texte de mot de passe.

Exemple:

```
1 | password:
2 |   type: password
3 |   label: Password
```

| **Attributs communs autorisés**         |      
| ---------                               |   
| [autofocus](#autofocus)                 |
| [classes](#classes)                     |
| [default](#default)                     |
| [disabled](#disabled)                   |
| [help](#help)                           |
| [id](#id)                               |
| [label](#label)                         |
| [name](#name)                           |
| [novalidate](#novalidate)               |
| [outerclasses](#outerclasses)           |
| [readonly](#readonly)                   |
| [size](#size)                           |
| [style](#style)                         |
| [title](#title)                         |
| [validate.required](#validate.required) |
| [validate.pattern](#validate.pattern)   |
| [validate.message](#validate.message)   |

- - - 

<h3 id="Champ radio">Champ radio.
<a href="#Champ radio" class="toc-anchor after"></a></h3>

<img class="img"src="https://learn.getgrav.org/user/pages/06.forms/02.forms/02.fields-available/radio_field.gif" alt="Champ radio">

Le type champ `radio` permet de présenter un ensemble de champs radio

Exemple:

```yaml
1 | my_choice:
2 |   type: radio
3 |   label: Choice
4 |   default: markdown
5 |   options:
6 |      markdown: Markdown
7 |      twig: Twig
```

| **ATTRIBUT**    | **DESCRIPTION**
| ---------       | ---------
| `options`       | Un tableau d'options clé-valeur qui seront autorisées.

| **Attributs communs autorisés**         |      
| ---------                               |   
| [default](#default)                     |
| [disabled](#disabled)                   |
| [help](#help)                           |
| [id](#id)                               |
| [label](#label)                         |
| [name](#name)                           |
| [outerclasses](#outerclasses)           |
| [validate.required](#validate.required) |
| [validate.pattern](#validate.pattern)   |
| [validate.message](#validate.message)   |

- - - 

<h3 id="Champ plage">Champ plage.
<a href="#Champ plage" class="toc-anchor after"></a></h3>

<img class="img"src="https://learn.getgrav.org/user/pages/06.forms/02.forms/02.fields-available/range_field.gif" alt="Champ plage">

Le champ de plage `range` est utilisé pour présenter un [champ de saisie de plage](http://html5doctor.com/html5-forms-input-types/#input-range).

Exemple:

```yaml
1 | header.choose_a_number_in_range:
2 |   type: range
3 |   label: Choose a number
4 |   validate:
5 |     min: 1
6 |     max: 10
```

| **Attributs communs autorisés**         |      
| ---------                               |   
| [autofocus](#autofocus)                 |
| [classes](#classes)                     |
| [default](#default)                     |
| [disabled](#disabled)                   |
| [help](#help)                           |
| [id](#id)                               |
| [label](#label)                         |
| [name](#name)                           |
| [novalidate](#novalidate)               |
| [outerclasses](#outerclasses)           |
| [readonly](#readonly)                   |
| [size](#size)                           |
| [style](#style)                         |
| [title](#title)                         |
| [validate.required](#validate.required) |
| [validate.pattern](#validate.pattern)   |
| [validate.message](#validate.message)   |

- - - 

<h3 id="Champ de section">Champ de section.
<a href="#Champ de section" class="toc-anchor after"></a></h3>

Le type de champ `Section` est utilisé pour diviser une page de paramètres en sections.

Exemple:

```yaml
1 | content:
2 |     type: section
3 |     title: PLUGIN_ADMIN.DEFAULTS
4 |     underline: true
5 | 
6 |     fields:
7 | 
8 |         #..... subfields
```

| **ATTRIBUT**    | **DESCRIPTION**
| ---------       | ---------
| `title`         | Un titre de rubrique.
| `text`          | Un texte à afficher en dessous.
| `security`      | Un tableau d'informations d'identification dont un utilisateur a besoin pour visualiser<br> cette section.
| `title_level`   | Définissez une balise de titre personnalisée. Par défaut : `h3`.

- - - 

<h3 id="Champ sélection">Champ sélection.
<a href="#Champ sélection" class="toc-anchor after"></a></h3>

![Champ sélection](https://learn.getgrav.org/user/pages/06.forms/02.forms/02.fields-available/select_field.gif)

Le type de champ de sélection `select` est utilisé pour présenter un champ de sélection.

Exemple 1:

```yaml
1  | pages.order.by:
2  |    type: select
3  |     size: long
4  |     classes: fancy
5  |     label: 'Default Ordering'
6  |     help: 'Pages in a list will render using this order unless it is overridden'
7  |     options:
8  |        default: 'Default - based on folder name'
9  |        folder: 'Folder - based on prefix-less folder name'
10 |        title: 'Title - based on title field in header'
11 |        date: 'Date - based on date field in header'
```

Exemple 2 - Désactivation des options individuelles :

```yaml
1  | my_element:
2  |     type: select
3  |     size: long
4  |     classes: fancy
5  |     label: 'My Select Element'
6  |     help: 'Use the disabled key:value to display but disable a particular option'
7  |     options:
8  |         option1:
9  |            value: 'Option 1'
10 |         option2:
11 |            value: 'Option 2'
12 |         option3:
13 |            disabled: true
14 |            value: 'Option 3'
```

| **ATTRIBUT**    | **DESCRIPTION**
| ---------       | ---------
| `options`       | Un tableau d'options clé-valeur qui seront autorisées. La clé sera soumise par le<br>  formulaire.
| `multiple`      | Autoriser le formulaire à accepter plusieurs valeurs.

Si vous définissez `multiple` sur true, vous devez ajouter

```yaml
1 | pages.order.by:
2 |   validate:
3 |      type: array
```

Sinon, le tableau des valeurs sélectionnées ne sera pas enregistré correctement.

| **Attributs communs autorisés**         |      
| ---------                               |   
| [autofocus](#autofocus)                 |
| [classes](#classes)                     |
| [default](#default)                     |
| [disabled](#disabled)                   |
| [help](#help)                           |
| [id](#id)                               |
| [label](#label)                         |
| [name](#name)                           |
| [novalidate](#novalidate)               |
| [outerclasses](#outerclasses)           |
| [size](#size)                           |
| [style](#style)                         |
| [title](#title)                         |
| [validate.required](#validate.required) |
| [validate.pattern](#validate.pattern)   |
| [validate.message](#validate.message)   |

- - - 

<h3 id="Champ sélection Optgroup">Champ sélection Optgroup.
<a href="#Champ sélection Optgroup" class="toc-anchor after"></a></h3>

![Champ sélection Optgroup](https://learn.getgrav.org/user/pages/06.forms/02.forms/02.fields-available/select_optgroup_field.gif)

Le type de champ `select_optgroup` est utilisé pour présenter un champ de sélection avec des regroupements.

Exemple:

```yaml
1  | -header.newField:
2  |    type: select_optgroup
3  |    label: Test Optgroup Select Field
4  |    options:
5  |       - OptGroup1:
6  |         - Option1
7  |         - Option2
8  |       - OptGroup2:
9  |         - Option3
10 |         - Option4
```

| **ATTRIBUT**    | **DESCRIPTION**
| ---------       | ---------
| `options`       | Un tableau d'options clé-valeur qui seront autorisées.
| `multiple`      | Autoriser le formulaire à accepter plusieurs valeurs.

| **Attributs communs autorisés**         |      
| ---------                               |   
| [autofocus](#autofocus)                 |
| [classes](#classes)                     |
| [default](#default)                     |
| [disabled](#disabled)                   |
| [help](#help)                           |
| [id](#id)                               |
| [label](#label)                         |
| [name](#name)                           |
| [novalidate](#novalidate)               |
| [outerclasses](#outerclasses)           |
| [size](#size)                           |
| [style](#style)                         |
| [validate.required](#validate.required) |
| [validate.pattern](#validate.pattern)   |
| [validate.message](#validate.message)   |

- - - 

<h3 id="Champ d'espacement">Champ d'espacement.
<a href="#Champ d'espacement" class="toc-anchor after"></a></h3>

Le type de champ d'espacement `spacer` est utilisé pour ajouter du texte, un titre ou une balise hr

Exemple:

```yaml
1 | test:
2 |   type: spacer
3 |   title: A title
4 |   text: Some text
5 |   underline: true
```

| **ATTRIBUT**    | **DESCRIPTION**
| ---------       | ---------
| `title`         | Ajouter un titre h3 au formulaire.
| `text`          | Ajoutez du texte. Si le titre est défini, ajoutez-le après le titre.
| `underline`     | Booléen, ajouter une balise `<hr>` si positif.

- - - 

<h3 id="Onglets / Champs d'onglet">Onglets / Champs d'onglet.
<a href="#Onglets / Champs d'onglet" class="toc-anchor after"></a></h3>

![Onglets](https://learn.getgrav.org/user/pages/06.forms/02.forms/02.fields-available/tabs_field_bp.gif)

Les types de champs d'onglet `tabs`et `tab` sont utilisés pour diviser les champs de formulaire contenus en onglets.

Exemple:

```yaml
1  | tabs:
2  |   type: tabs
3  |   active: 1
4  | 
5  |   fields:
6  |     content:
7  |        type: tab
8  |        title: PLUGIN_ADMIN.CONTENT
9  | 
10 |        fields:
11 | 
12 |           # .... other subfields
13 | 
14 |     options:
15 |        type: tab
16 |        title: PLUGIN_ADMIN.OPTIONS
17 |
18 |        fields:
19 | 
20 |           # .... other subfields
```

| **Attribut**    | **Description**
| ---------       | ---------
| `active`        | Le numéro d'onglet actif.

- - - 

<h3 id="Champ téléphone">Champ téléphone.
<a href="#Champ téléphone" class="toc-anchor after"></a></h3>

Le type de champ `tel` est utilisé pour présenter un champ de saisie de texte qui accepte un numéro de téléphone, en utilisant [l'entrée tel HTML5](http://html5doctor.com/html5-forms-input-types/#input-tel).

Exemple:

```yaml
1 | header.phone:
2 |   type: tel
3 |   label: 'Your Phone Number'
```

| **ATTRIBUT**    | **DESCRIPTION**
| ---------       | ---------
| `minlength`     | Longueur minimale du texte.
| `maxlength`     | Longueur maximale du texte.
| `validate.min`  | Identique à minlength.
| `validate.max`  | Identique à maxlength.

| **Attributs communs autorisés**         |      
| ---------                               |   
| [autofocus](#autofocus)                 |
| [classes](#classes)                     |
| [default](#default)                     |
| [disabled](#disabled)                   |
| [help](#help)                           |
| [id](#id)                               |
| [label](#label)                         |
| [name](#name)                           |
| [novalidate](#novalidate)               |
| [outerclasses](#outerclasses)           |
| [readonly](#readonly)                   |
| [size](#size)                           |
| [style](#style)                         |
| [title](#title)                         |
| [validate.required](#validate.required) |
| [validate.pattern](#validate.pattern)   |
| [validate.message](#validate.message)   |

- - - 

<h3 id="Champ de texte">Champ de texte.
<a href="#Champ de texte" class="toc-anchor after"></a></h3>

![Champ de texte](https://learn.getgrav.org/user/pages/06.forms/02.forms/02.fields-available/text_field.gif)

Le champ `text` est utilisé pour présenter un champ de saisie de texte.

Exemple:

```yaml
1 | header.title:
2 |   type: text
3 |   autofocus: true
4 |   label: PLUGIN_ADMIN.TITLE
5 |   minlength: 10
6 |   maxlength: 255
```
 
| **ATTRIBUT**       | **DESCRIPTION**
| ---------       | ---------
| `prepend`       | Ajouter du texte ou du code HTML au début d'un champ.
| `append`        | Ajouter du texte ou du code HTML à la fin d'un champ.
| `minlength`     | Longueur minimale du texte.
| `maxlength`     | Longueur maximale du texte.
| `validate.min`  | Iidentique à minlength.
| `validate.max`  | Iidentique à maxlength.

| **Attributs communs autorisés**         |      
| ---------                               |   
| [autofocus](#autofocus)                 |
| [classes](#classes)                     |
| [default](#default)                     |
| [disabled](#disabled)                   |
| [help](#help)                           |
| [id](#id)                               |
| [label](#label)                         |
| [name](#name)                           |
| [novalidate](#novalidate)               |
| [outerclasses](#outerclasses)           |
| [readonly](#readonly)                   |
| [size](#size)                           |
| [style](#style)                         |
| [title](#title)                         |
| [validate.required](#validate.required) |
| [validate.pattern](#validate.pattern)   |
| [validate.message](#validate.message)   |

- - - 

<h3 id="Champ de zone de texte">Champ de zone de texte.
<a href="#Champ de zone de texte" class="toc-anchor after"></a></h3>

![Champ de zone de texte](https://learn.getgrav.org/user/pages/06.forms/02.forms/02.fields-available/textarea_field.gif)

Le champ `textarea` est utilisé pour présenter un champ de saisie textarea.

Exemple:

```yaml
1 | header.content:
2 |   type: textarea
3 |   autofocus: true
4 |   label: PLUGIN_ADMIN.CONTENT
5 |   minlength: 10
6 |   maxlength: 255
```

| **ATTRIBUT**    | **DESCRIPTION**
| ---------       | ---------
| `rows`          | Ajoutez un attribut rows avec la valeur associée à cette propriété.
| `cols`          | Ajoutez un attribut cols avec la valeur associée à cette propriété.
| `minlength`     | Longueur minimale du texte.
| `maxlength`     | Longueur maximale du texte.
| `validate.min`  | Identique à minlength.
| `validate.max`  | Identique à maxlength.

| **Attributs communs autorisés**         |      
| ---------                               |   
| [autofocus](#autofocus)                 |
| [classes](#classes)                     |
| [default](#default)                     |
| [disabled](#disabled)                   |
| [help](#help)                           |
| [id](#id)                               |
| [label](#label)                         |
| [name](#name)                           |
| [novalidate](#novalidate)               |
| [outerclasses](#outerclasses)           |
| [readonly](#readonly)                   |
| [size](#size)                           |
| [style](#style)                         |
| [title](#title)                         |
| [validate.required](#validate.required) |
| [validate.pattern](#validate.pattern)   |
| [validate.message](#validate.message)   |

- - - 

<h3 id="Champ bascule">Champ bascule.
<a href="#Champ bascule" class="toc-anchor after"></a></h3> 

<img class="img"src="https://learn.getgrav.org/user/pages/06.forms/02.forms/02.fields-available/toggle_field_bp.gif" alt="Champ bascule">

Le type de champ à bascule `toggle` est un type d'entrée marche/arrêt, avec des étiquettes configurables.

Exemple:

```yaml
1  | summary.enabled:
2  |     type: toggle
3  |     label: PLUGIN_ADMIN.ENABLED
4  |     highlight: 1
5  |     help: PLUGIN_ADMIN.ENABLED_HELP
6  |     options:
7  |         1: PLUGIN_ADMIN.YES
8  |         0: PLUGIN_ADMIN.NO
9  |     validate:
10 |        type: bool
```

| **ATTRIBUT**    | **DESCRIPTION**
| ---------       | ---------
| `highlight`     | La touche de l'option à mettre en surbrillance (définie en vert lorsqu'elle est sélectionnée).
| `options`       | La liste des options de valeur-clé.

| **Attribut communs autorisés**          |      
| ---------                               |   
| [default](#default)                     |
| [help](#help)                           |
| [label](#label)                         |
| [name](#name)                           |
| [style](#style)                         |
| [toggleable](#toogleable)               |
| [validate.required](#validate.required) |
| [validate.pattern](#validate.pattern)   |
| [validate.message](#validate.message)   |

- - -

<h3 id="Champ d'URL">Champ d'URL.
<a href="#Champ d'URL" class="toc-anchor after"></a></h3> 

Le type de champ `url` est utilisé pour présenter un champ de saisie de texte qui accepte une URL, en utilisant [l'entrée url HTML5](http://html5doctor.com/html5-forms-input-types/#input-url).

Exemple:

```yaml
1 | header.phone:
2 |   type: url
3 |   label: 'Your Phone Number'
```

| **ATTRIBUT**    | **DESCRIPTION**
| ---------       | ---------
| `minlength`     | Longueur minimale du texte.
| `maxlength`     | Longueur maximale du texte.
| `validate.min`  | Identique à minlength.
| `validate.max`  | Identique à maxlength.

| **Attributs communs autorisés**         |      
| ---------                               |   
| [autofocus](#autofocus)                 |
| [classes](#classes)                     |
| [default](#default)                     |
| [disabled](#disabled)                   |
| [help](#help)                           |
| [id](#id)                               |
| [label](#label)                         |
| [name](#name)                           |
| [novalidate](#novalidate)               |
| [outerclasses](#outerclasses)           |
| [readonly](#readonly)                   |
| [size](#size)                           |
| [style](#style)                         |
| [title](#title)                         |
| [validate.required](#validate.required) |
| [validate.pattern](#validate.pattern)   |
| [validate.message](#validate.message)   |

<h2 id="Champs actuellement non documentés">Champs actuellement non documentés.
<a href="#Champs actuellement non documentés" class="toc-anchor after"></a></h2>

| **CHAMP**    | **DESCRIPTION**
| ---------    | ---------
| **Array**    ||
| **Avatar**   ||
| **Color**    ||
| **Columns**  ||
| **Column**   ||
| **Datetime** ||
| **Fieldset** ||
| **Formname** ||
| **Month**    ||
| **Signature**||
| **Switch**   ||
| **Time**     ||
| **Unique Id**||
| **Value** 	||
| **Week**     ||

- - -

<div id="ndt1">NDT : la version anglaise d'origine est <a href ="https://blog.cloudflare.com/turnstile-private-captcha-alternative/">ici</a>.</div>

- - - 

