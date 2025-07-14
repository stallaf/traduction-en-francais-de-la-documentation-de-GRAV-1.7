<h1 class="rem">Formulaires frontaux</h1>

Le plugin **Form** vous donne la possibilité de créer pratiquement n'importe quel type de formulaire frontal. Il s'agit essentiellement d'un kit de construction de formulaire, que vous pouvez utiliser dans vos propres pages. Avant d'aller plus loin, n'oubliez pas d'installer le [plugin **Form**](https://github.com/getgrav/grav-plugin-form) avec le formulaire d'installation `bin/gpm install form` s'il n'est pas encore installé.

Pour comprendre le fonctionnement du plugin** Form**, commençons par expliquer comment créer un formulaire simple.

<div class="notice warning">
Avec la version <strong>Form 2.0</strong>, il est désormais nécessaire de transmettre le <strong>nom du formulaire</strong> en tant que champ masqué. Si vous utilisez le form-plugin-provided <code>forms.html.twig</code>, cela est géré automatiquement, cependant, si vous avez remplacé le <code>forms.html.twig</code> par défaut dans votre thème ou plugin, vous devez ajouter manuellement <code>{% include "forms/ fields/formname/formname.html.twig" %}</code> dans votre fichier Twig de rendu de formulaire.
</div>

<h2 id="Créer un formulaire unique simple">Créer un formulaire unique simple.
<a href="#Créer un formulaire unique simple" class="toc-anchor after"></a></h2>

Pour ajouter un formulaire à une page de votre site, créez une page et définissez son fichier de page sur "Formulaire". Vous pouvez le faire via le panneau d'administration, ou directement via le système de fichiers en nommant la page `form.md`.

Ainsi, par exemple, `user/pages/03.your-form/form.md`.

Le contenu de cette page sera :

```yaml
1  | ---
2  | title: A Page with an Example Form
3  | form:
4  |     name: contact-form
5  |     fields:
6  |        name: name
7  |        label: Name
8  |        placeholder: Enter your name
9  |        autofocus: on
10 |        autocomplete: on
11 |        type: text
12 |        validate:
13 |           required: true
14 | 
15 |        email
16 |           label: Email
17 |           placeholder: Enter your email address
18 |           type: email
19 |           validate:
20 |              required: true
21 | 
22 |     buttons:
23 |        submit:
24 |           type: submit
25 |           value: Submit
26 |        reset:
27 |           type: reset
28 |           value: Reset
29 | 
30 |     process:
31 |        email:
32 |           from: "{{ config.plugins.email.from }}"
33 |           to:
34 |              - "{{ config.plugins.email.to }}"
35 |              - "{{ form.value.email }}"
36 |           subject: "[Feedback] {{ form.value.name|e }}"
37 |           body: "{% include 'forms/data.html.twig' %}"
38 |        save:
39 |           fileprefix: feedback-
40 |           dateformat: Ymd-His-u
41 |           extension: txt
42 |           body: "{% include 'forms/data.txt.twig' %}"
43 |        message: Thank you for your feedback!
44 |        display: thankyou
45 | 
46 | ---
47 | 
48 | # My Form
49 | 
50 | Regular **markdown** content goes here...
```

<div class="notice tip">
Il s'agit du contenu du fichier <code>form.md</code>, lorsqu'il est visualisé via le système de fichiers. Pour ce faire via Admin Plugin, ouvrez la page en <strong>mode expert</strong>, copiez la partie entre les triples tirets <code>---</code> et collez-la dans le champ Frontmatter.
</div>

Cela suffit pour afficher un formulaire dans la page, sous le contenu de la page. C'est un formulaire simple avec un nom, un champ email, deux boutons : un pour soumettre le formulaire et un pour réinitialiser les champs. Pour plus d'informations sur les champs disponibles fournis par le plugin Form, [consultez la section suivante](formulaires-ref-options-formulaires.md).

Que se passe-t-il lorsque vous appuyez sur le bouton `Submit` ? Il exécute les actions du `process` en série. Pour découvrir d'autres actions, [consultez les options disponibles](formulaires-ref-formulaire-actions.md)</id> ou [créez la vôtre](formulaires-ref-formulaire-actions.md).

1. Un e-mail est envoyé à l'adresse e-mail saisie, avec pour objet `[Feedback] [name entered]`. Le corps de l'email est défini dans le fichier `forms/data.html.twig` du thème utilisé.

2. Un fichier est créé dans `user/data` pour stocker les données d'entrée du formulaire. Le modèle est défini dans `forms/data.txt.twig` du thème utilisé.

3. La sous-page de `thankyou` s'affiche, ainsi que le message passé. La page de `thankyou` doit être une sous-page de la page contenant le formulaire.

<div class="notice tip">
Assurez-vous d'avoir configuré le plugin <strong>Email</strong> pour vous assurer qu'il a la bonne configuration afin d'envoyer des emails avec succès.
</div>

<h2 id="Formulaires multiples">Formulaires multiples.
<a href="#Formulaires multiples" class="toc-anchor after"></a></h2>

Avec la sortie de **Form Plugin v2.0**, vous pouvez désormais définir plusieurs formulaires sur une seule page. La syntaxe est similaire mais chaque formulaire est différencié par le nom du formulaire, en l'occurrence `contact-form` et `newletter-form` :

```yaml
1  | forms:
2  |     contact-form:
3  |         fields:
4  |             ...
5  |         buttons:
6  |             ...
7  |         process:
8  |             ...
9  | 
10 |     newsletter-form:
11 |         fields:
12 |             ...
13 |         buttons:
14 |             ...
15 |         process:
16 |             ...
```

Vous pouvez même utiliser ce format pour des formulaires uniques, en fournissant simplement un formulaire sous `form:` :

```yaml
1 | forms:
2 |     contact-form:
3 |         fields:
4 |             ...
5 |         buttons:
6 |             ...
7 |         process:
8 |             ...
```

<h2 id="Affichage des formulaires de Twig">Affichage des formulaires de Twig.
<a href="#Affichage des formulaires de Twig" class="toc-anchor after"></a></h2>

Le moyen le plus simple d'inclure un formulaire consiste simplement à inclure un fichier Twig dans le modèle qui affiche la page où le formulaire est défini. Par example:

```twig
1 | {% include "forms/form.html.twig" %}
```

Cela utilisera le modèle Twig fourni par le plugin Form lui-même. À son tour, il affichera le formulaire tel que vous l'avez défini dans la page et gérera l'affichage d'un message de réussite ou d'erreurs lorsque le formulaire est soumis.

Il existe cependant une méthode plus puissante d'affichage des formulaires qui peut tirer parti du nouveau support multi-formulaires. Avec cette méthode, vous passez en fait un paramètre `form:` au modèle Twig en spécifiant le formulaire que vous souhaitez afficher :

```twig
1 | {% include "forms/form.html.twig" with { form: forms('contact-form') } %}
```
  
En utilisant cette méthode, vous pouvez choisir un nom spécifique d'un formulaire à afficher. Vous pouvez même fournir le nom d'un formulaire défini dans d'autres pages. Tant que tous vos noms de formulaires sont uniques sur votre site, Grav trouvera et affichera le formulaire correct !

Vous pouvez même afficher plusieurs formulaires sur une seule page :

```twig
1 | # Contact Form
2 | {% include "forms/form.html.twig" with { form: forms('contact-form') } %}
3 |
4 | # Newsletter Signup
5 | {% include "forms/form.html.twig" with { form: forms('newsletter-form') } %}
```

Une autre façon d'afficher un formulaire consiste à référencer la route de la page plutôt que le nom du formulaire à l'aide d'un tableau, par exemple :

```twig
1 | # Contact Form
2 | {% include "forms/form.html.twig" with { form: forms( {route:'/forms/contact'} ) } %}
```

Cela trouvera le premier formulaire de la page avec route `/forms/contact`

<h2 id="Affichage des formulaires dans le contenu de la page">Affichage des formulaires dans le contenu de la page.
<a href="#Affichage des formulaires dans le contenu de la page" class="toc-anchor after"></a></h2>

Vous pouvez également afficher un formulaire à partir du contenu de votre page (par exemple `default.md`) directement sans que cette page ait même un formulaire défini à l'intérieur. Passez simplement le nom ou l'itinéraire au formulaire.

<div class="notice info">
Le <strong>traitement Twig</strong> doit être activé et le <strong>cache de page</strong> doit être désactivé pour garantir que le formulaire est traité de manière dynamique sur la page et non mis en cache de manière statique et que la gestion du formulaire peut se produire.
</div>

```yaml
1  | ---
2  | title: Page with Forms
3  | process:
4  |   twig: true
5  | cache_enable: false
6  | ---
7  | 
8  | # Contact Form
9  | {% include "forms/form.html.twig" with {form: forms('contact-form')} %}
10 | # Newsletter Signup
11 | {% include "forms/form.html.twig" with {form: forms( {route: '/12 | newsletter-signup'} ) } %}
```

<h2 id="Formulaires modulaires">Formulaires modulaires.
<a href="#Formulaires modulaires" class="toc-anchor after"></a></h2>

Avec les versions précédentes du plugin Form, pour obtenir un formulaire à afficher dans une sous-page **modulaire** de votre page modulaire globale, vous deviez définir le formulaire dans la **page modulaire de niveau supérieur**. De cette façon, le formulaire serait traité et disponible pour être affiché dans la sous-page modulaire.

Dans **Form v2.0**, vous pouvez désormais définir le formulaire directement dans la sous-page modulaire comme n'importe quel autre formulaire. Cependant, s'il n'est pas trouvé, le plugin de formulaire cherchera dans la 'page actuelle', c'est-à-dire la page modulaire de niveau supérieur pour un formulaire, il est donc entièrement rétrocompatible avec la façon de faire 1.0.

Vous pouvez également configurer le modèle Twig de votre sous-page modulaire pour utiliser un formulaire d'une autre page, comme dans les exemples ci-dessus.

<div class="notice note">
Lorsque vous utilisez un formulaire défini dans une sous-page modulaire, vous devez définir l'<strong>action :</strong> sur la page modulaire parente et configurer votre formulaire avec une <strong>redirect :</strong> ou <strong>fficher :</strong> l'action, car cette sous-page modulaire n'est pas une page appropriée à charger sur le formulaire soumission car il n'est <strong>pas routable</strong>, et donc pas accessible par un navigateur.
</div>

Voici un exemple qui existe dans `form/modular/_form/form.md` :

```yaml
1  | ---
2  | title: Modular Form
3  | 
4  | form:
5  |   action: '/form/modular'
6  |   inline_errors: true
7  |   fields:
8  |     person.name:
9  |       type: text
10 |       label: Name
11 |       validate:
12 |         required: true
13 | 
14 |   buttons:
15 |     submit:
16 |       type: submit
17 |       value: Submit
18 | 
19 |   process:
20 |     message: "Thank you from your submission <b>{{ form.value('person.name') }}</b>!"
21 |     reset: true
22 |     display: '/form/modular'  
23 | ---
24 |
25 | ## Modular Form
```
 
