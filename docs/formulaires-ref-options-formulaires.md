<h1 class="rem">Référence : Options de formulaire</h1>

<h2 id="Name">Name
<a href="#Name" class="toc-anchor after"></a></h2>

Il n'y a pas d'options obligatoires pour les formulaires. Cependant, comme indiqué dans la vue d'ensemble des [formulaires frontaux](formulaires-frontaux/.md), il est fortement recommandé d'avoir au moins un nom de formulaire :

```yaml
1 | form:
2 |   name: my-form
```

Cela **doit être unique** pour votre site Grav. En effet, le nom du formulaire sert d'identifiant unique pour ce formulaire via le système. Un formulaire peut être référencé par ce nom à partir de n'importe quelle autre page.

<h2 id="Method">Method
<a href="#Method" class="toc-anchor after"></a></h2>

Cette option vous permet de contrôler si le formulaire doit être soumis via `POST` ou `GET`. La valeur par défaut est `POST`. Notez également que si vous avez un champ de fichier dans votre formulaire, la méthode obtiendra également `enctype="multipart/form-data"` ajouté :

```yaml
1 | form:
2 |   method: GET
```

<h2 id="Action">Action
<a href="#Action" class="toc-anchor after"></a></h2>

L'action par défaut va être la route en tant que page actuelle. Cela a du sens la plupart du temps car le formulaire doit être traité par la même page qui héberge le formulaire. Il arrive parfois que vous souhaitiez remplacer l'action pour spécifier une extension de fichier différente (`.json` peut-être) ou même cibler une ancre de page spécifique :

```yaml
1 | form:
2 |   action: '/contact-us#contact-form'
```

Vous pouvez même traiter le formulaire sur une autre page si cette page est celle où vous souhaitez gérer les résultats. Cela peut également être utilisé comme technique pour modifier le modèle de la réponse par rapport à celui utilisé dans le formulaire d'origine :

```yaml
1 | form:
2 |   action: /contact-us/ajax-process
```

Où vous avez un fichier de page appelé `form-messages.html.twig` qui renvoie uniquement les données du message. Sinon, vous pouvez utiliser l'approche ci-dessous...

<h2 id="Template">Template
<a href="#Template" class="toc-anchor after"></a></h2>

Habituellement, le modèle Twig de la page qui affiche le formulaire est parfaitement capable de gérer tous les messages de réussite/échec ou les réponses de validation en ligne. Cependant, il est parfois utile de renvoyer la réponse du formulaire en utilisant un modèle Twig différent. Un bon exemple de cela est lorsque vous souhaitez traiter votre formulaire via Ajax. Vous souhaitez probablement que le code HTML pour les messages de réussite/échec soit renvoyé par le modèle, afin qu'ils puissent être réinjectés dans la page par JavaScript :

```yaml
1 | form:
2 |   template: form-messages
```

<h2 id="ID">ID
<a href="#ID" class="toc-anchor after"></a></h2>

La possibilité de définir un champ `id` CSS au niveau du formulaire. S'il n'est pas fourni, le nom du formulaire est utilisé.

```yaml
1 | form:
2 |   id: my-form-id
```

<h2 id="Classes">Classes
<a href="#Classes" class="toc-anchor after"></a></h2>

Vous pouvez également définir des classes explicites sur le formulaire. Il n'y a pas de valeurs par défaut ici.

```yaml
1 | form:
2 |   classes: 'form-style form-surround'
```

<h2 id="Inline Errors">Inline Errors
<a href="#Inline Errors" class="toc-anchor after"></a></h2>

La définition d'erreurs en ligne dans le fichier ou la définition de démarquage du formulaire permet l'affichage d'erreurs en ligne, un outil de dépannage important.

```yaml
1 | form:
2 |   inline_errors: true
```

<h2 id="Client-side Validation">Client-side Validation
<a href="#Client-side Validation" class="toc-anchor after"></a></h2>

La désactivation de la validation côté client vous permettra de voir les erreurs en ligne et la validation détaillée côté serveur qui vont au-delà de la validation côté client HTML5. Vous pouvez désactiver la validation côté client via form.yaml ou dans la définition du formulaire.

```yaml
1 | form:
2 |   client_side_validation: false
```

<h2 id="XSS Checks">XSS Checks
<a href="#XSS Checks" class="toc-anchor after"></a></h2>

Par défaut, Grav 1.7 et les versions ultérieures activent diverses vérifications XSS dans tous les formulaires. Les paramètres par défaut peuvent être trouvés à partir de la [configuration de la sécurité](../configuration#Securité). Cependant, vous pouvez remplacer ces paramètres par formulaire ou par champ, par exemple, vous pouvez désactiver les vérifications XSS dans l'ensemble du formulaire en :

```yaml
1 | form:
2 |   xss_check: false
```

<div class="notice info">
<strong>AVERTISSEMENT :</strong> Il n'est pas recommandé de désactiver toutes les vérifications XSS, mais de remplacer les règles spécifiques par champ. Tous les exemples ici fonctionneront également à l'intérieur d'un champ de formulaire.
</div>

Vous pouvez activer ou désactiver des règles individuelles en remplaçant la configuration principale. Les règles qui ne sont pas remplacées utiliseront la configuration de sécurité :

```yaml
1 | form:
2 |   xss_check:
3 |      enabled_rules:
4 |         on_events: false
5 |         invalid_protocols: false
6 |         moz_binding: false
7 |         html_inline_styles: false
8 |         dangerous_tags: false
```

Mieux encore, vous pouvez également autoriser des protocoles et des balises spécifiques :

```yaml
1 | form:
2 |   xss_check:
3 |      safe_protocols:
4 |         - javascript
5 |      safe_tags:
6 |         - iframe
```

<h2 id="Keep Alive">Keep Alive
<a href="#Keep Alive" class="toc-anchor after"></a></h2>

Vous pouvez vous assurer que vos formulaires n'échouent pas à se soumettre lorsque votre session expire, en activant l'option `keep_alive` sur le formulaire. En activant ceci, une requête AJAX sera faite à Grav avant l'expiration de votre session pour la garder "fraîche":

```yaml
1 | form:
2 |   keep_alive: true
```

<h2 id="Fieldsets">Fieldsets
<a href="#Fieldsets" class="toc-anchor after"></a></h2>

Vous pouvez configurer des balises `<fieldset></fieldset>` pour les champs de votre formulaire à l'aide de la désignation `fieldset:` dans le formulaire.

```yaml
1  | form:
2  |     name: Example Form
3  |     fields:
4  |        example:
5  |           type: fieldset
6  |           id: my-fieldset
7  |           legend: 'Test Fieldset'
8  |           fields:
9  |              first_field: { type: text, label: 'First Field' }
10 |              second_field: { type: text, label: 'Second Field' }
```

Le formulaire ci-dessus sort comme suit :

```html
1  | <form action="/grav/example/forms" class="" id="my-example-form" method="post" name="Example Form">
2  |  <fieldset id="my-fieldset">
3  |     <legend>Test Fieldset</legend>
4  |        <div class="form-group">
5  |           <div class="form-label-wrapper">
6  |              <label class="form-label">First Field</label>
7  |           </div>
8  |           <div class="form-data" data-grav-default="null" data-grav-disabled="true" data-grav-field="text">
9  |              <div class="form-input-wrapper">
10 |                 <input class="form-input" name="data[first_field]" type="text" value="">
11 |              </div>
12 |           </div>
13 |        </div>
14 |        <div class="form-group">
15 |           <div class="form-label-wrapper">
16 |              <label class="form-label">Second Field</label>
17 |           </div>
18 |        <div class="form-data" data-grav-default="null" data-grav-disabled="true" data-grav-field="text">
19 |        <div class="form-input-wrapper">
20 |           <input class="form-input" name="data[second_field]" type="text" value="">
21 |        </div>
22 |      </div>
23 |    </div>
24 |  </fieldset>
25 | </form>
```

Dans l'exemple ci-dessus, les champs apparaissent dans l'ensemble de champs `my-fieldset`. Vous remarquerez également que les balises `<legend></legend>` sont prises en charge via l'option `legend:`.   

