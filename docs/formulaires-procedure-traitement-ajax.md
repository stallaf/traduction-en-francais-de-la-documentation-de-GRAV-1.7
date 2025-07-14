<h1 class="rem">Procédure : Soumission Ajax</h1>

<h2 id="Envoi de formulaires via Ajax">Envoi de formulaires via Ajax.
<a href="#Envoi de formulaires via Ajax" class="toc-anchor after"></a></h2>

Le mécanisme par défaut pour le traitement des formulaires repose sur la soumission de formulaires de style HTML standard qui entraîne l'envoi du contenu d'un formulaire HTML au serveur via `POST` ou` GET` (la valeur par défaut est `POST`). Une fois le formulaire [traité](./formulaires-ref-index-formulaires.md), et [executé](./formulaires-ref-formulaire-actions.md)</a> les résultats sont renvoyés au formulaire (ou à une [page redirigée](./formulaires-ref-formulaire-actions.md#Redirection)) où les messages sont affichés et le formulaire peut être modifié pour une nouvelle soumission si nécessaire.

Cela implique un rechargement de page, ce qui est parfois indésirable. C'est là qu'un formulaire soumis via JavaScript en utilisant Ajax ou XHR est l'option préférée. Heureusement, les capacités de formulaire de Grav sont à la hauteur de la tâche.

<h2 id="Création du formulaire">Création du formulaire.
<a href="#Création du formulaire" class="toc-anchor after"></a></h2>

Vous pouvez créer n'importe quel formulaire standard que vous aimez, donc pour cet exemple, nous garderons le formulaire aussi simple que possible pour nous concentrer sur les parties de gestion Ajax. Tout d'abord, nous allons créer un formulaire dans une page appelée : forms/ajax-test/ et créer une page de formulaire appelée form.md :

```yaml
---
1  | title: Ajax Test-Form
2  | form:
3  |     name: ajax-test-form
4  |     action: '/forms/ajax-test'
5  |     template: form-messages
6  |     refresh_prevention: true
7  | 
8  |     fields:
9  |         name:
10 |             label: Your Name
11 |             type: text
12 | 
13 |     buttons:
14 |         submit:
15 |             type: submit
16 |             value: Submit
17 | 
18 |     process:
19 |         message: 'Thank you for your submission!'
20 | ---
```

Comme vous pouvez le voir, il s'agit d'un formulaire très basique qui demande simplement votre nom et fournit un bouton d'envoi. La seule chose qui ressort est le modèle `template : form-messages`. Comme indiqué dans la section [Formulaires frontaux](./formulaires-frontaux.md), vous pouvez fournir un modèle Twig personnalisé avec lequel afficher le résultat du traitement du formulaire. C'est un excellent moyen pour nous de traiter le formulaire, puis de renvoyer simplement les messages via Ajax et de les injecter dans la page. Il existe déjà un modèle `form-messages.html.twig` fourni avec le plugin de formulaires qui fait exactement cela.

<div class="notice info">
REMARQUE : nous utilisons une action codée en dur <code>action: '/forms/ajax-test'</code> afin que l'ajax ait une URL cohérente plutôt que de laisser le formulaire définir l'action sur la route de la page actuelle. Cela résout un problème avec la requête Ajax qui ne gère pas correctement les redirections. Sinon, cela peut entraîner des problèmes sur la page d'accueil. Il n'est pas nécessaire que ce soit la page de formulaire actuelle, il doit simplement s'agir d'un itinéraire cohérent et accessible.
</div>

![Formulaire 1](https://learn.getgrav.org/user/pages/06.forms/02.forms/06.how-to-ajax-submission/simple-form.png)

<h2 id="Contenu de la page">Contenu de la page.
<a href="#Contenu de la page" class="toc-anchor after"></a></h2>

Dans cette même page, il faut mettre un peu de HTML et de JavaScript :

* Vanilla JS

```javascript
1  | <div id="form-result"></div>
2  |
3  | <script>
4  | document.addEventListener('DOMContentLoaded', function() {
5  |     const form = document.querySelector('#ajax-test-form');
6  |     form.addEventListener('submit', function(event) {
7  |         event.preventDefault();
8  | 
9  |         const result = document.querySelector('#form-result');
10 |         const action = form.getAttribute('action');
11 |         const method = form.getAttribute('method');
12 | 
13 |         fetch(action, {
14 |             method: method,
15 |             body: new FormData(form)
16 |         })
17 |         .then(function(response) {
18 |             if (response.ok) {
19 |                 return response.text();
20 |             } else {
21 |                 return response.json();
22 |             }
23 |         })
24 |         .then(function(output) {
25 |             if (result) {
26 |                 result.innerHTML = output;
27 |             }
28 |         })
29 |         .catch(function(error) {
30 |             if (result) {
31 |                 result.innerHTML = 'Error: ' + error;
32 |             }
33 | 
34 |             throw new Error(error);
35 |         });
36 |     });
37 | });
38 | </script>
```

* jQuery

```javascript
1  | <div id="form-result"></div>
2  | 
3  | <script>
4  | $(document).ready(function(){
5  | 
6  |     var form = $('#ajax-test-form');
7  |     form.submit(function(e) {
8  |         // prevent form submission
9  |         e.preventDefault();
10 | 
11 |         // submit the form via Ajax
12 |         $.ajax({
13 |             url: form.attr('action'),
14 |             type: form.attr('method'),
15 |             dataType: 'html',
16 |             data: form.serialize(),
17 |             success: function(result) {
18 |                 // Inject the result in the HTML
19 |                 $('#form-result').html(result);
20 |             }
21 |         });
22 |     });
23 | });
24 | </script>
```

Nous définissons d'abord un espace réservé div avec l'ID `#form-result` à utiliser comme emplacement pour injecter les résultats du formulaire.

Nous utilisons ici la syntaxe JQuery pour plus de simplicité, mais vous pouvez évidemment utiliser le code JavaScript de votre choix tant qu'il remplit une fonction similaire. Nous arrêtons d'abord l'action de soumission par défaut du formulaire et effectuons un appel Ajax à l'action du formulaire avec les données du formulaire sérialisées. Le résultat de cet appel est ensuite défini sur cette div que nous avons créée précédemment.

![Fomulaire 2](https://learn.getgrav.org/user/pages/06.forms/02.forms/06.how-to-ajax-submission/submitted-form.png)

