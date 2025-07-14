<h1 class="rem">Objets flexibles</h1>

**Flex Objects** est un nouveau concept de Grav 1.7, qui ajoute la prise en charge des types de données personnalisés qui peuvent être facilement intégrés à votre site. La prise en charge des types personnalisés et des fonctionnalités d'administration est fournie par le [plugin **Flex Objects**](https://github.com/trilbymedia/grav-plugin-flex-objects), qui est requis par le [panneau d'administration](panneau-admin-intro.md). Ce plugin gère également la création d'objets dans le frontend au cas où vous auriez besoin que les utilisateurs puissent soumettre de nouveaux objets ou y apporter des modifications.

Les fonctionnalités de base de Grav telles que les [comptes d'utilisateurs](compte-utilisateur.md), les [groupes d'utilisateurs](groupe-utilisateur.md) et les [pages](dashboard-pages.md) ont déjà été converties en objets Flex, bien qu'elles ne soient utilisées que dans le **panneau d'administration**.

<div class="notice info">
Les <strong>répertoires Flex</strong> de cette documentation n'ont rien à voir avec l'ancien <strong>plugin Flex Directories</strong>. En fait, l'ancien plugin a été remplacé par cette fonctionnalité avec le <strong>plugin Flex Objects</strong>.
</div>

<h2 id="Introduction">Introduction.
<a href="#Introduction" class="toc-anchor after"></a></h2> 

**Flex** est un ensemble de **répertoires** d'un seul **type**. Grav a ses propres types intégrés, tels que les *comptes d'utilisateurs* et les *pages*. Les plugins et les thèmes peuvent également définir leurs propres types et les enregistrer dans Grav. Avec **Flex Objects Plugin**, vous pouvez également créer vos propres types et répertoires personnalisés.

<h3 id="Flex">Flex.
<a href="#Flex" class="toc-anchor after"></a></h3> 

[Flex](avance-flex-flex.md) est un conteneur pour les **répertoires Flex**.

Cela donne un point d'accès unique pour toutes les données du site, étant donné que les données se trouvent dans un répertoire Flex. Cela rend tous les objets disponibles pour chaque page et plug-in de votre site.

<div class="notice note">
<strong>ASTUCE</strong> : Même si les <em>comptes d'utilisateur</em> ou les <em>pages</em> Flex ne sont pas activés, vous pouvez toujours accéder à leurs versions Flex à la fois dans l'interface et dans le panneau d'administration.
</div>

<h3 id="Type flexible">Type flexible.
<a href="#Type flexible" class="toc-anchor after"></a></h3> 

**Flex Type** est le modèle de votre **répertoire Flex**.

Il définit tout ce qui est nécessaire pour afficher et modifier le contenu : structure des données, champs de formulaire, autorisations, fichiers de modèle, même couche de stockage.

<h3 id="Répertoire flexible">Répertoire flexible.
<a href="#Répertoire flexible" class="toc-anchor after"></a></h3> 
 
[Flex Directory](avance-flex-repertoire.md) conserve une collection **d'objets Flex** d'un seul **type Flex**.

Chaque répertoire contient une **collection d'objets** avec une prise en charge facultative des **index** pour accélérer les requêtes vers le **stockage**.

<h3 id="Collection flexible">Collection flexible.
<a href="#Collection flexible" class="toc-anchor after"></a></h3> 

[Flex Collection](avance-flex-collection.md) est une structure qui contient des **objets Flex**.

La collection ne contient généralement que les objets nécessaires pour afficher la page ou pour effectuer l'action donnée. Il fournit des outils utiles pour filtrer ou manipuler davantage les données ainsi que des méthodes pour rendre l'ensemble de la collection.

<h3 id="Objet flexible">Objet flexible.
<a href="#Objet flexible" class="toc-anchor after"></a></h3> 

[Flex Object](avance-flex-objets-flex.md) est une instance unique d'un **type Flex**.

L'objet représente une seule entité. L'objet donne accès à ses propriétés, y compris toutes les données associées, telles que [Media](media.md). L'objet sait également comment **s'afficher** ou quel **formulaire** utiliser pour modifier son contenu. Les actions telles que la création, la mise à jour et la suppression d'objets sont prises en charge par l'objet lui-même.

<h3 id="Index flexible">Index flexible.
<a href="#Index flexible" class="toc-anchor after"></a></h3> 

**Flex Index** est utilisé pour effectuer des requêtes rapides vers **Flex Directory**.

Il contient des métadonnées pour les** objets Flex**, mais pas les objets eux-mêmes.

<h3 id="Stockage flexible">Stockage flexible.
<a href="#Stockage flexible" class="toc-anchor after"></a></h3> 

**Flex Storage** est une couche de stockage pour les **objets Flex**.

Il peut s'agir d'un seul fichier, d'un ensemble de fichiers dans un seul dossier ou d'un ensemble de dossiers. Flex prend également en charge les stockages personnalisés, tels que les stockages de base de données.

<h3 id="Formulaire flexible">Formulaire flexible.
<a href="#Formulaire flexible" class="toc-anchor after"></a></h3> 

**Flex Form** s'intègre au **pluin de formulaire** et permet de créer ou de modifier un **objet Flex**.

Flex prend en charge plusieurs vues, ce qui permet de modifier différentes parties de l'objet.

<h3 id="Administration flexible">Administration flexible.
<a href="#Administration flexible" class="toc-anchor after"></a></h3> 

[Flex Administration](avance-flex-administration.md) est implémenté par **Flex Objects Plugin**.

Il ajoute une nouvelle section au **plugin d'administration** permettant aux administrateurs de site de gérer les **objets flexibles**. Chaque **répertoire Flex** est livré avec une liste de contrôle d'accès de type CRUD, qui peut être utilisée pour restreindre des parties d'Admin et des actions qu'elles contiennent à certains utilisateurs.

<h2 id="Limites actuelles">Limites actuelles.
<a href="#Limites actuelles" class="toc-anchor after"></a></h2> 

Il reste encore beaucoup de travail à faire. Voici les limitations actuelles lors de l'utilisation d'objets Flex :

* La prise en charge multilingue n'a été implémentée que pour les **pages**, l'administrateur ne peut pas encore être entièrement traduit
* Le frontend n'a qu'un routage de base ; pour vos tâches personnalisées, telles que l'enregistrement, vous avez besoin de votre propre implémentation
* Les fonctionnalités de mise à jour en masse n'ont pas encore été implémentées dans Admin (dans le code, elles sont faciles)
* En raison des limitations d'indexation, il n'est pas recommandé d'utiliser Flex pour les objets qui sont constamment mis à jour
* La personnalisation de votre **Flex Type** nécessite une bonne connaissance du codage et la création de vos propres classes.

