<h1 class="rem">Du côté serveur</h1>

Protéger votre installation Grav côté serveur consiste à utiliser des options sensibles pour votre serveur et PHP. Ce guide ne couvre pas les paramètres du serveur sur lequel vous exécutez Grav, ni les conditions idéales, mais met plutôt en évidence quelques conseils et meilleures pratiques pour sécuriser Grav ou des liens vers des ressources qui détaillent comment sécuriser le serveur. **Ceci est pertinent pour un serveur de production, pas pour le développement local, et déconseillé aux utilisateurs novices !**

<h2 id="Grav et configuration par défaut">Grav et configuration par défaut.
<a href="#Grav et configuration par défaut" class="toc-anchor after"></a></h2>

Pour Grav, vous devez toujours utiliser une configuration spécifique à un répertoire à jour et pertinente pour votre serveur. Ceux-ci se trouvent dans le [référentiel GitHub](https://github.com/getgrav/grav/tree/develop/webserver-configs). De plus, mettez à jour périodiquement votre installation de Grav au fur et à mesure que de nouveaux correctifs de sécurité sont implémentés dans les nouvelles versions - pour plus de détails, consultez le [CHANGELOG](https://github.com/getgrav/grav/blob/develop/CHANGELOG.md).

<h2 id="Paramétrage PHP">Paramétrage PHP.
<a href="#Paramétrage PHP" class="toc-anchor after"></a></h2>

Avant de vous mêler de la configuration de PHP, sachez que la plupart des hébergeurs partagés auprès desquels vous louez de l'espace d'hébergement auront probablement déjà configuré des paramètres par défaut sensibles et sécurisés. De plus, dans la plupart des cas, ils ne vous permettent pas de le modifier vous-même. Avant de désactiver ou de modifier une configuration, vous devez vous familiariser avec les exigences de Grav, y compris les [extensions PHP](https://github.com/getgrav/grav/blob/develop/composer.json) et la manière dont les modifications les affecteront.

Généralement, la configuration de PHP est modifiée via `php.ini`. Vous pouvez trouver l'emplacement de ce fichier à partir de la ligne de commande avec la commande php `--ini`, ou si vous n'avez pas accès aux commandes directes, créez un fichier nommé `phpinfo.php` dans le dossier racine public de votre serveur Web qui contient `<? php phpinfo(); ?>` et ouvrez-le avec votre navigateur. Le chemin sera répertorié sous "Fichier de configuration chargé". Une fois localisé, supprimez le fichier `phpinfo.php`.

Quelques recommandations générales :

* **Gardez toujours votre version de PHP à jour** : utilisez une [version de PHP prise en charge](https://php.net/supported-versions.php), de préférence une version en développement actif et stable. Par exemple, PHP 5.6 et PHP 7.0 n'auront des correctifs de sécurité implémentés que jusqu'en décembre 2018, tandis que PHP 7.1 reste en développement actif aux côtés de PHP 7.2.
* Envisagez de désactiver publiquement l'affichage des erreurs et de la version PHP : [article PHP.earth](https://php.earth/doc/security/intro#php-configuration).
* Utilisez un utilisateur distinct avec des autorisations restreintes pour exécuter PHP pour Grav : [Autorisations dans Docs](/depannage-autorisations)</span>.
* Utilisez Suhosin pour une [protection avancée de PHP](https://suhosin.org/stories/feature-list.html).

<h2 id="Configuration du serveur Web">Configuration du serveur Web.
<a href="#Configuration du serveur Web" class="toc-anchor after"></a></h2>

Les logiciels de serveur Web ou de serveur HTTP courants incluent Nginx et Apache, ainsi que des alternatives plus modernes telles que LiteSpeed ou CaddyServer. Les [configurations de serveur Web](https://github.com/getgrav/grav/tree/develop/webserver-configs) susmentionnées incluent les valeurs par défaut nécessaires pour Grav, mais vous pouvez sécuriser davantage le serveur Web grâce à sa configuration. Quelques ressources pertinentes :

* [Comment sécuriser Nginx](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-on-ubuntu-14-04) sur DigitalOcean et les [meilleures pratiques de sécurité de Nginx WebServer](https://www.cyberciti.biz/tips/linux-unix-bsd-nginx-webserver-security.html) sur nixCraft.
* [Apache Web Server Hardening & Security Guide](https://geekflare.com/apache-web-server-hardening-security/) sur Geek Flare, et [Apache Web Server Security and Hardening Tips](https://www.tecmint.com/apache-security-tips/) sur Tecmint.
* [Moyens d'améliorer la sécurité dans Litespee](https://bobcares.com/blog/ways-of-improving-security-in-litespeed/)d sur Bobcares.

<h2 id="Paramétrage du serveur">Paramétrage du serveur.
<a href="#Paramétrage du serveur" class="toc-anchor after"></a></h2>

Vous devez **toujours garder votre système d'exploitation (OS) à jour**. Les systèmes d'exploitation sont vulnérables aux exploits et aux intrusions, encore plus que PHP, et doivent être mis à jour aussi fréquemment que possible. De plus, vous devez **toujours tenir à jour les autres logiciels** : votre installation ne se limite pas à OS, PHP et Grav. D'autres progiciels sont également des facteurs de risque et doivent être mis à jour fréquemment.

Pour protéger la connexion de vos utilisateurs à votre site, vous devez activer et appliquer [HTTPS avec un certificat SSL](https://php.earth/doc/security/ssl). Cela garantit que toutes les communications entre le serveur et le navigateur restent privées et cryptées. Des certificats et des services gratuits sont disponibles via, par exemple, [Let's Encrypt](https://letsencrypt.org/about/) ou [CloudFlare](https://www.cloudflare.com/ssl/).

Si votre serveur fonctionne sous Linux, activez [Security Enhanced Linux](https://selinuxproject.org/page/Main_Page). SELinux est généralement activé par défaut et [vaut bien la peine de l'avoir](https://www.computerworld.com/article/2717423/security/why-selinux-is-more-work--but-well-worth-the-trouble.html). D'autres recommandations pour SysAdmins sont disponibles sur [nixCraft](https://www.cyberciti.biz/tips/php-security-best-practices-tutorial.html).

