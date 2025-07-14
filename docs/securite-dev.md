<h1 class="rem">Développeurs</h1>

Lors de la création d'un plugin ou d'un thème pour Grav, il est non seulement important de suivre les meilleures pratiques, mais de déterminer si votre travail ouvre des voies d'attaque pour les intrus potentiels sur un site. Grav étant un CMS à fichier plat et dépendant de peu de dépendances, il est par nature plus sécurisé que des systèmes similaires, mais des canaux non sécurisés peuvent être créés par inadvertance.

<h2 id="Les meilleures pratiques">Les meilleures pratiques.
<a href="#Les meilleures pratiques" class="toc-anchor after"></a></h2>

Voici quelques recommandations sur la meilleure façon de créer une extension sécurisée et fiable pour Grav, et doivent être considérées comme des connaissances essentielles pour tout auteur de thème ou de plugin.

* Lors de l'écriture de modèles Twig qui génèrent des informations soumises par l'utilisateur, [échappez toujours l'entrée](https://twig.symfony.com/doc/1.x/filters/escape.html), cela inclut également les actifs.
* Le code PHP doit [nettoyer](https://php.net/manual/en/filter.filters.sanitize.php) l'entrée et la sortie.
* Les plans doivent privilégier les options prédéfinies : lorsque cela est possible, donnez à l'utilisateur un ensemble de choix plutôt qu'une entrée brute.
* Soyez conscient de la façon dont la mémoire et l'utilisation du processeur affectent l'extension et évitez d'utiliser les ressources système de manière injustifiée.
* Utilisez l'écosystème de fonctionnalités et de procédures de Grav plutôt que d'écrire du code non testé, et envisagez des [bibliothèques tierces](https://packagist.org/) testées au combat si vous en avez besoin de plus.
N'utilisez pas de [fonctions PHP non sécurisées](https://www.owasp.org/index.php/PHP_Security_Cheat_Sheet#Other_Injection_Cheat_Sheet). Lisez également les [propres recommandations de PHP](https://php.net/manual/en/security.php) et le [guide du PHP Security Consortium](http://phpsec.org/projects/guide/) sur le sujet.
* Utilisez des [exceptions](https://php.net/manual/en/language.exceptions.php) spécifiques aux erreurs, et non des rapports d'erreurs, lorsqu'un script doit échouer. N'incluez jamais de données d'utilisateur, d'installation ou de système dans des exceptions ou un débogage visible publiquement.

<h2 id="CMS à fichier plat">CMS à fichier plat.
<a href="#CMS à fichier plat" class="toc-anchor after"></a></h2>

Grav a des [exigences de base](/prerequis) limitées et modernes, et notamment son architecture de fichier plat atténue le besoin d'une base de données. Ceci est avantageux car un vecteur d'attaque commun est la base de données d'un système. Le nettoyage et la sécurisation des entrées sont une tâche beaucoup plus difficile lorsque l'ensemble du CMS repose sur une base de données, et les attaques par injection SQL peuvent automatiquement essayer d'exécuter des instructions SQL même sur des hôtes distants.

