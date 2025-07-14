<h1 class="rem">Protection contre le <em>Flood</em></h1>

![Image Flood 1](https://learn.getgrav.org/user/pages/05.admin-panel/06.security/02.rate-limiting/login.gif)

Les attaques par force brute sont un choix populaire pour les intrus de sites Web. Il peut s'agir d'une personne que vous connaissez essayant de deviner votre mot de passe encore et encore jusqu'à ce qu'elle réussisse enfin ou d'un bot inondant votre site de tentatives de connexion jusqu'à ce que le mot de passe soit finalement découvert.

La fonction de protection contre le *flood* de Grav (également connue sous le nom de limitation de débit) rend ces types d'attaques exceptionnellement difficiles. Il vous permet de définir un certain nombre de tentatives de connexion infructueuses dans un laps de temps spécifique avant que le compte ne soit temporairement verrouillé. De plus, vous pouvez limiter le nombre de demandes de réinitialisation de mot de passe appliquées aux comptes avant de verrouiller cette fonctionnalité.

<h2 id="Ce dont vous aurez besoin">Ce dont vous aurez besoin
<a href="#Ce dont vous aurez besoin" class="toc-anchor after"></a></h2>

Cette fonctionnalité est gérée via le [plugin de connexion](https://github.com/getgrav/grav-plugin-login), qui doit déjà être installé et activé si vous utilisez le panneau d'administration.

<h2 id="Comment le configurer">Comment le configurer
<a href="#Comment le configurer" class="toc-anchor after"></a></h2>

![Image Flood 2](https://learn.getgrav.org/user/pages/05.admin-panel/06.security/02.rate-limiting/2fa_3.jpeg)

Les paramètres de la protection contre les inondations de Grav se trouvent dans le plugin de connexion. Accédez simplement à **Admin > Plugins > Connexion** et sélectionnez l'onglet **Sécurité**.

Ici, vous pouvez définir les éléments suivants :

* Nombre maximal de réinitialisations de mot de passe avant le verrouillage
* Intervalle maximal de réinitialisation du mot de passe
* Nombre maximal d'échecs de connexion avant le verrouillage
* Intervalle maximal des échecs de connexion

Cela vous permettra de déterminer combien de réinitialisations de mot de passe ou de connexions échouées sont autorisées dans un laps de temps défini avant que le verrouillage ne se produise. Cette déconnexion est temporaire et dure aussi longtemps que l'intervalle défini.

