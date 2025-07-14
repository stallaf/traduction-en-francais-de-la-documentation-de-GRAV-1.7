<h1 class="rem">Sécurité - Aperçu</h1>

Si vous découvrez un éventuel problème de sécurité lié à Grav ou à l'une de ses extensions, veuillez envoyer un e-mail à l'équipe principale à [contact@getgrav.org](mailto:contact@getgrav.org) et nous le traiterons dès que possible.

Les problèmes ne doivent pas être divulgués publiquement - y compris sur GitHub, Discord ou le forum Discourse - avant que l'équipe principale n'ait eu la possibilité de les examiner et de contacter les parties concernées pour les résoudre. De plus, si le problème n'est pas une menace potentielle pour les utilisateurs de Grav, il devrait probablement être soumis [comme un problème](https://github.com/getgrav/grav/blob/develop/CONTRIBUTING.md#bug-reports) à la place. Si vous n'êtes pas sûr, contactez-nous et nous vous aiderons à déterminer à qui appartient le rapport.

<h2 id="Soumettre un rapport">Soumettre un rapport.
<a href="#Soumettre un rapport" class="toc-anchor after"></a></h2>

Lorsque vous avez découvert une vulnérabilité potentielle dans le noyau de Grav ou dans l'une de ses extensions, il est conseillé de faire preuve de diligence raisonnable en la signalant :

1. Incluez les **numéros de version** de Grav et de toutes les extensions installées, ainsi que le composant auquel le problème se rapporte.
2. Décrivez la vulnérabilité **de manière détaillée et concise**, afin de passer moins de temps à rechercher sa source.
3. Notez les étapes exactes nécessaires pour **reproduire l'environnement** dans lequel la vulnérabilité se produit : quels paramètres sont définis dans system.yaml, quel contenu est créé et quels paramètres système sont appliqués ?
4. Si possible, décrivez la source de la vulnérabilité et comment **la corriger** afin que les développeurs puissent à la fois la reconstruire et la sécuriser.

<h3 id="Divulgation responsable">Divulgation responsable.
<a href="#Divulgation responsable" class="toc-anchor after"></a></h3>

Grav suit le modèle de *divulgation responsable* pour la soumission des vulnérabilités découvertes. Cela signifie qu'une fois qu'un problème est découvert, testé et démontré avec succès, le ou les développeurs doivent disposer d'un délai pour corriger la vulnérabilité avant qu'elle ne soit divulguée publiquement. En effet, la recherche et le test de solutions aux problèmes signalés prennent du temps et sont sensibles au facteur temps, et Grav est un projet open source dont les auteurs n'ont pas un temps illimité à y consacrer. Il est donc recommandé de proposer également comment résoudre le problème ou le corriger, si vous connaissez le code correspondant.

<h2 id="Processus de résolution">Processus de résolution.
<a href="#Processus de résolution" class="toc-anchor after"></a></h2>

Si votre signalement est exact et qu'un nouveau problème de sécurité se reproduit, l'équipe principale s'en occupera dès que possible. Lorsque cela sera fait, le problème et sa solution seront inclus dans le [référentiel public de rapports](/securite-rapport). Vous serez crédité par votre nom et avec un lien facultatif vers votre site Web/profil de médias sociaux, mais si vous préférez, vous pouvez demander qu'il soit crédité d'un pseudonyme ou d'un "journaliste anonyme".

Les rapports et les problèmes restent confidentiels jusqu'à ce que le problème soit résolu. Dans le cas où le mainteneur d'une extension ne parvient pas à résoudre le problème en temps opportun, l'extension est supprimée de Grav Package Manner jusqu'à ce qu'elle soit résolue.

<h2 id="Versions prises en charge">Versions prises en charge.
<a href="#Versions prises en charge" class="toc-anchor after"></a></h2>

Seule la version `major.minor` actuelle de Grav est prise en charge. Cela signifie que les correctifs sont implémentés dans `major.minor.patch`, mais pas de manière régressive vers l'arrière pour les anciennes versions de Grav. Maintenir votre installation à jour est important, et de nombreux changements sont bénéfiques même s'ils ne sont pas explicitement nécessaires du point de vue de la sécurité.

<h2 id="Niveaux de risque">Niveaux de risque.
<a href="#Niveaux de risque" class="toc-anchor after"></a></h2>

Grav en tant que logiciel comporte cinq niveaux de risque :

* Très critique
* Critique
* Modérément critique
* Moins critique
* Pas critique

Ceux-ci sont calculés sur la base du "[Common Misuse Scoring System](https://www.nist.gov/news-events/news/2012/07/software-features-and-inherent-risks-nists-guide-rating-software)" (CMSS) du [National Institute of Standards and Technology](https://www.nist.gov/) (NIST). Faute d'une calculatrice facilement disponible pour Grav, utilisez [RiskCalc](https://security.drupal.org/riskcalc) de Drupal [(notes](https://www.mydropwizard.com/blog/understanding-drupal-security-advisories-risk-calculator)).


