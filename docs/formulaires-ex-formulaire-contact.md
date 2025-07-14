<h1 class="rem">Exemple : Formulaire de contact</h1>

<h2 id="Formulaire de contact simple">Formulaire de contact simple.
<a href="#Formulaire de contact simple" class="toc-anchor after"></a></h2>

Le **plugin Grav Form** est le moyen le plus simple d'avoir des formulaires sur votre site. Voyons comment créer un formulaire de contact simple.

<h3 id="Un exemple vivant">Un exemple vivant.
<a href="#Un exemple vivant" class="toc-anchor after"></a></h3>

Le squelette de l'article Sora a une page de formulaire prête à être consultée lors de la lecture de ce didacticiel :

[Page en direct](http://demo.getgrav.org/soraarticle-skeleton/contact)

[Fichier markdown de la page](https://raw.githubusercontent.com/getgrav/grav-skeleton-soraarticle-blog/develop/pages/03.contact/form.md)

<h3 id="Configurer la page">Configurer la page.
<a href="#Configurer la page" class="toc-anchor after"></a></h3>

Vous pouvez mettre un formulaire dans n'importe quelle page de votre site. Tout ce que vous avez à faire est de renommer le fichier markdown de la page en `form.md`, ou d'ajouter un en-tête de [modèle](../en-tete-frontmatter#Template) dans le frontmatter de la page, pour qu'il utilise le modèle de formulaire `form`.

<div class="notice info">
Le modèle de votre page, ou le modèle parent de la page, doit implémenter la balise <code>{% block content %}</code> pour que le <strong>Plugin Grav Form</strong> puisse afficher vos entrées sur la page.
</div>

Les champs de formulaire et les instructions de traitement sont définis dans le frontmatter YAML de la page, il vous suffit donc d'ouvrir le fichier Markdown de la page avec votre éditeur préféré et d'y insérer le code suivant :

```yaml
1  | ---
2  | title: Contact Form
3  | 
4  | form:
5  |     name: contact
6  |
7  |     fields:
8  |         name:
9  |           label: Name
10 |           placeholder: Enter your name
11 |           autocomplete: on
12 |           type: text
13 |           validate:
14 |             required: true
15 | 
16 |         email:
17 |           label: Email
18 |           placeholder: Enter your email address
19 |           type: email
20 |           validate:
21 |             required: true
22 | 
23 |         message:
24 |           label: Message
25 |           placeholder: Enter your message
26 |           type: textarea
27 |           validate:
28 |             required: true
29 | 
30 |         g-recaptcha-response:
31 |           label: Captcha
32 |           type: captcha
33 |           recaptcha_not_validated: 'Captcha not valid!'
34 | 
35 |     buttons:
36 |         submit:
37 |           type: submit
38 |           value: Submit
39 |         reset:
40 |           type: reset
41 |           value: Reset
42 | 
43 |     process:
44 |         captcha: true
45 |         save:
46 |             fileprefix: contact-
47 |             dateformat: Ymd-His-u
48 |             extension: txt
49 |             body: "{% include 'forms/data.txt.twig' %}"
50 |         email:
51 |             subject: "[Site Contact Form] {{ form.value.name|e }}"
52 |             body: "{% include 'forms/data.html.twig' %}"
53 |         message: Thank you for getting in touch!
54 |         display: thankyou
55 | ---
56 | 
57 | # Contact form
58 | 
59 | Some sample page content
```

<div class="notice tip">
Assurez-vous d'avoir configuré les adresses e-mail "E-mail de" et "E-mail vers" dans le plugin E-mail avec votre adresse e-mail.
</div>

<div class="notice info">
Cet exemple utilise Google reCAPTCHA via le <a href="../formulaires-ref-options-formulaires"> champ captcha</a>, et vous devez configurer votre <code>site_key</code> et <code>secret_key</code> dans le plugin de formulaire pour que cela fonctionne. Si vous ne souhaitez pas utiliser Google reCaptcha, supprimez simplement le champ <code>g-recaptcha-response</code> et le processus <code>captcha: true</code>.
</div>

Maintenant, dans le dossier de la page, créez un sous-dossier nommé thankyou/, créez un nouveau fichier nommé `formdata.md`. Et collez le code suivant dans le fichier :

```yaml
1  | ---
2  | title: Email sent
3  | cache_enable: false
4  | process:
5  |     twig: true
6  | ---
7  |
8  | ## Email sent!
```

C'est ça!

[Page en direct](http://demo.getgrav.org/soraarticle-skeleton/contact)

[Fichier de page markdown](https://raw.githubusercontent.com/getgrav/grav-skeleton-soraarticle-blog/develop/pages/03.contact/form.md)

<div class="notice tip">
Les pages modulaires sont un peu différentes. Dans ce cas, voir aussi <a href="../formulaires-procedure-formulaire-pages">utilisation des formulaires dans les pages modulaires</a>.
</div>

Lorsque les utilisateurs soumettent le formulaire, le plugin vous enverra un e-mail (comme défini dans les paramètres de formulaire `form` du plugin Grav Email) et enregistrera les données saisies dans le dossier data/ .

<div class="notice note">
Pour plus de détails sur la mise en place et la configuration du courrier électronique, veuillez lire la <a href="https://github.com/getgrav/grav-plugin-email/blob/develop/README.mddocumentation du plug-in de courrier électronique]">documentation du plugin Email de courrier électronique</a>.
</div>

Vous pouvez activer le plugin **Grav Data Manager** pour voir ces données dans le **plugin Admin**.

<div class="notice tip">
À l'avenir, nous voulons que Grav soit capable de générer dynamiquement des formulaires à partir du plugin d'administration.
</div>
