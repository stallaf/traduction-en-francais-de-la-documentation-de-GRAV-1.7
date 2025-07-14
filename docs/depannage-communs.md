<h1 class="rem">Problèmes communs</h1>

Ici, vous pouvez trouver des informations sur les problèmes et problèmes soulevés sur le [forum Grav](https://getgrav.org/forum) et sur le [serveur Discord Chat](https://chat.getgrav.org/) qui se produisent assez fréquemment pour que nous pensions gagner du temps et répertorier le problème et la solution pertinente dans un emplacement facile à trouver.

<h2 id="Impossible de se connecter au GPM">Impossible de se connecter au GPM.
<a href="#Impossible de se connecter au GPM" class="toc-anchor after"></a></h2>

**Problème** : Le GPM est injoignable et vous obtenez cette erreur dans le panneau d'administration

**La solution** :

Tout d'abord, assurez-vous que PHP a installé cURL et OpenSSL. Vous pouvez vérifier cela dans le panneau d'administration, dans Configuration -> Info. Vous devriez voir une section "OpenSSL" avec `OpenSSL support: enabled`. Idem pour cURL, une section avec support `cURL support: enabled`.

Si cela vous convient, assurez-vous que vous n'êtes pas derrière un proxy. Si c'est le cas, [configurez-le](/configuration) dans la configuration du système Grav et [assurez-vous qu'il n'y a pas de problèmes de connexion](/depannage-rpoxy).

Ensuite, vérifiez vos [autorisations](/depannage-autorisations).

Si après tout ce qui précède, vous rencontrez toujours des problèmes de connexion avec GPM, nous avons remarqué que sur certains serveurs (principalement des machines locales exécutant Windows), il y a des problèmes lors de la vérification du certificat SSL de getgrav.org, même s'il s'agit d'une [note A](https://www.ssllabs.com/ssltest/analyze.html?d=getgrav.org&hideResults=on). Pour contourner ce problème, nous avons ajouté une nouvelle configuration système `system.gpm.verify_peer` qui est activée par défaut. Définissez-le sur false et réessayez.

Si, à ce stade, cela ne fonctionne toujours pas, contactez-nous ou signalez-le si vous avez été dirigé ici via le chat/forum.

Vérifiez également que la commande CLI fonctionne, en ouvrant une connexion SSH au serveur et en exécutant l'index `bin/gpm` et vérifiez si c'est juste dans Admin que vous obtenez cette erreur, ou dans la ligne de commande également.

<h2 id="L'interface d'administration ne défile pas">L'interface d'administration ne défile pas.
<a href="#L'interface d'administration ne défile pas" class="toc-anchor after"></a></h2>

**Problème** : lors de l'accès à l'interface du plugin d'administration, la page ne défile pas

**Solution** : Plusieurs causes ont été signalées, mais les solutions les plus courantes sont les suivantes.

* Rechargez complètement la page en vidant le cache de votre navigateur, puis en l'actualisant.
* Assurez-vous que vous utilisez la dernière version de Grav et passez à la langue par défaut - l'anglais. Si cela résout le problème de défilement, veuillez signaler la langue défectueuse [comme un problème](https://github.com/getgrav/grav-plugin-admin/issues/).
* Si vous utilisez CloudFlare pour HTTPS ou en tant que CDN, leur optimisation JS - qui est activée par défaut - peut bloquer le rendu des scripts. Pour le désactiver, connectez-vous à CloudFlare et sélectionnez le domaine concerné, puis effectuez l'une des actions suivantes :

      1. Pour désactiver complètement cette optimisation, accédez à "Vitesse" et faites défiler jusqu'à "Rocket Loader".
        * Réglez-le sur "Off" et CloudFlare ne bloquera pas le script, mais vous ne bénéficierez pas non plus de leur optimisation.
      2. Pour désactiver uniquement l'optimisation de l'interface d'administration de Grav, accédez à "Règles de page" et cliquez sur le bouton "Créer une règle de page".
        * Pour le champ "Si l'URL correspond", renseignez votre nom de domaine, suivi de `/admin`, par exemple : `example.com/admin`.
        * Cliquez sur « Ajouter un paramètre » et, dans la liste déroulante, recherchez « Rocket Loader ». Lorsqu'il est sélectionné, modifiez la valeur dans "Sélectionner une valeur" sur **off**.
        * Laissez le champ "Ordre" tel quel, par défaut, il est défini sur **First**.
        * Enfin, cliquez sur le bouton "Enregistrer et déployer"

Si aucune des solutions ci-dessus ne fonctionne, veuillez vérifier la console de votre navigateur pour toute erreur JavaScript signalée ; Dans Chrome ou Firefox, appuyez sur F12 ou Ctrl+Maj+I, puis cliquez sur l'onglet "Console". Signalez les erreurs [comme un problème](https://github.com/getgrav/grav-plugin-admin/issues/).

<h2 id="Échec de la récupération">Échec de la récupération.
<a href="#Échec de la récupération" class="toc-anchor after"></a></h2>

À l'intérieur de l'administration, une fenêtre contextuelle rouge "Fetch Failed" peut parfois apparaître. Si cela se produit de temps en temps, ne vous inquiétez pas car cela pourrait simplement signifier un problème de connexion.

Mais s'il apparaît à chaque fois, un problème rencontré par certains utilisateurs est que `mod_security` bloque les requêtes réseau de Grav.

Cela peut être résolu en trouvant et en désactivant les règles qui sont déclenchées, qui, selon la configuration de mod_security, peuvent être différentes d'un cas à l'autre.

Si vous utilisez votre propre serveur, un guide sur la façon de procéder peut être trouvé dans [http://www.inmotionhosting.com/support/website/modsecurity/find-and-disable-specific-modsecurity-rules](https://www.inmotionhosting.com/support/website/modsecurity/find-and-disable-specific-modsecurity-rules), sinon contactez simplement votre fournisseur d'hébergement et illustrez le problème.

Problème connexe : [admin#951](https://github.com/getgrav/grav-plugin-admin/issues/951)

<h2 id="L'API Zend OPcache est restreinte">L'API Zend OPcache est restreinte.
<a href="#L'API Zend OPcache est restreinte" class="toc-anchor after"></a></h2>

Si vous exécutez PHP avec Zend OPache et que vous recevez cette erreur, votre configuration OPCache actuelle [limite l'accès à la fonction API OPcache aux scripts uniquement à partir d'une chaîne spécifiée](https://php.net/manual/en/opcache.configuration.php). La solution la plus simple consiste à trouver l'emplacement de cette directive soit dans votre fichier `php.ini`, soit dans un fichier `opcache.ini` spécialisé qui est inséré dans votre fichier `php.in` global et de définir cette valeur sur rien :

    1 | opcache.restrict_api=

Il s'agit d'un problème avec tout hébergement géré par [ServerPilot](https://serverpilot.io/) avec PHP 7.2 activé. Un ticket a été soumis pour résoudre ce problème de leur côté.

<h2 id="Le partage LinkedIn et l'indexation Wayback Machine ne fonctionnent pas">Le partage LinkedIn et l'indexation Wayback Machine ne fonctionnent pas.
<a href="#Le partage LinkedIn et l'indexation Wayback Machine ne fonctionnent pas" class="toc-anchor after"></a></h2>

**Problème** : Le partage de pages avec LinkedIn et la propagation des données de la page ne fonctionnent pas. La Wayback Machine n'indexe pas correctement les pages de mon site Web.

**Solution** : activez WebServer Gzip ou la compression Gzip. Les deux peuvent être utilisés, mais au moins un doit être actif pour que ces fonctions particulières fonctionnent sur certains cas de serveur.

Ce [problème](https://github.com/getgrav/grav/issues/1639) est apparu pour les utilisateurs sur des environnements de serveur spécifiques. En particulier, avec les serveurs AWS basés sur le cloud, les utilisateurs rencontraient des problèmes pour partager des pages Web à partir de leurs sites Grav sur LinkedIn ou pour les indexer correctement par la Wayback Machine. Ce problème a été résolu en activant la compression WebServer Gzip ou Gzip.

<h2 id="Impossible de faire défiler dans Admin sur CloudFlare">Impossible de faire défiler dans Admin sur CloudFlare.
<a href="#Impossible de faire défiler dans Admin sur CloudFlare" class="toc-anchor after"></a></h2>

Pour les utilisateurs de CloudFlare, la possibilité de faire défiler l'Admin peut être interrompue. Il existe des solutions à cela, comme suit:

Dans l'interface de CloudFlare, accédez à **Speed** et désactivez **Rocket Loader** (ou via une règle de page).

Il peut également être désactivé en mode "automatique" (par défaut) avec un **attribut de données** sur les scripts : `script data-cfasync="false" src="/javascript.js"></script>`.

Un exemple de règle de page serait la correspondance d'URL `example.com/staging/*/admin`, où le `*` est un caractère générique indiquant n'importe quel nom de dossier. Pour les paramètres, ajoutez `Rocket Loader` et sélectionnez **Off**.

