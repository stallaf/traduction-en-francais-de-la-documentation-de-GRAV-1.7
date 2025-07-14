<h1 class="rem">Authentification à 2 facteurs</h1>

![Profil administration 1](https://learn.getgrav.org/user/pages/05.admin-panel/06.security/01.2fa/auth3.gif)

L'authentification à 2 facteurs (2FA) est une excellente mesure de sécurité qui utilise une méthode d'authentification de type horloge tournante qui génère des codes à six chiffres que vous pouvez utiliser en plus de votre nom d'utilisateur et de votre mot de passe pour accéder à l'administrateur.

Pour profiter de cette fonctionnalité, vous devrez télécharger une application prenant en charge 2FA telle que [Authy](https://authy.com/) ou [Google Authenticator](https://play.google.com/store/apps/details?hl=en). Cette application agira comme un porte-clés virtuel pour les codes d'authentification.

<h2 id="Comment le configurer">Comment le configurer
<a href="#Comment le configurer" class="toc-anchor after"></a></h2>

![Profil administration 2](https://learn.getgrav.org/user/pages/05.admin-panel/06.security/01.2fa/2fa_1.jpeg)

La configuration de l'authentification à 2 facteurs dans Grav est facile. Tout ce que vous avez à faire est d'accéder à **Plugins> Panneau d'administration> Bases** dans l'administration.

Ici, vous trouverez l'authentification à 2 facteurs. Vous pouvez choisir d'activer cette fonctionnalité en sélectionnant **Oui**. Cela permettra aux utilisateurs de configurer une authentification à 2 facteurs sur leurs comptes.

![Profil administration 3](https://learn.getgrav.org/user/pages/05.admin-panel/06.security/01.2fa/2fa_2.jpeg)

Maintenant, vous pouvez sélectionner votre image d'avatar pour accéder aux paramètres de votre profil d'utilisateur. Ensuite, vous voudrez définir l'option **2FA Enabled** sur **Oui**.

Un code QR apparaîtra avec une clé secrète 2FA. Notez la clé et mettez-la dans un endroit sûr.

![Profil administration 4](https://learn.getgrav.org/user/pages/05.admin-panel/06.security/01.2fa/2fa_4.png)

À l'aide de l'application d'authentification de votre choix, scannez le code QR ou entrez la clé secrète pour enregistrer votre clé 2FA. Enregistrez votre page de profil pour verrouiller vos paramètres 2FA.

![Profil administration 5](https://learn.getgrav.org/user/pages/05.admin-panel/06.security/01.2fa/2fa_5.png)

Un badge 2FA violet apparaîtra désormais à côté de votre nom dans la barre latérale. Ce badge vous permet de savoir que 2FA est actif sur le compte.

Vous pouvez maintenant vous déconnecter et vous reconnecter. Vous serez accueilli avec les mêmes champs de nom d'utilisateur et de mot de passe, mais une fois que vous aurez entré ces informations, il vous sera demandé de fournir un code supplémentaire à six chiffres. Ce code se trouve dans votre application d'authentification. Il se réinitialise toutes les 30 secondes, de sorte que le code n'est bon que pendant cette courte période. Un nouveau code sera généré pour le remplacer.

C'est ça! Vous avez maintenant un site Grav plus sécurisé !

Oh, et si vous voulez changer votre clé 2FA, tout ce que vous avez à faire est d'appuyer sur le gros bouton rouge **Régénérer**.

<h2 id="Questions fréquemment posées">Questions fréquemment posées
<a href="#Questions fréquemment posées" class="toc-anchor after"></a></h2>

<h3 id="Que se passe-t-il si je perds l'accès à mon appareil 2FA ?">Que se passe-t-il si je perds l'accès à mon appareil 2FA ?
<a href="#Que se passe-t-il si je perds l'accès à mon appareil 2FA ?" class="toc-anchor after"></a></h3>

Ne vous inquiétez pas! Tout n'est pas perdu.

Votre statut 2FA et votre clé de hachage sont stockés dans le système de fichiers de votre site sur votre fichier YAML utilisateur. Par exemple, si votre compte utilisateur est `admin`, accédez à **ROOT/user/accounts/admin.yaml** et recherchez ces deux lignes :

```yaml
1 | twofa_enabled : true
2 | twofa_secret : RQX46XTTBK7QMMB6VR4RAUNWOYVXXTSR
```

Définissez simplement **twofa_enabled** sur `false` et enregistrez. Vous devriez maintenant pouvoir accéder à votre site en utilisant uniquement votre nom d'utilisateur et votre mot de passe. Alternativement, vous pouvez utiliser le **twofa_secret** pour enregistrer votre compte sur l'application d'authentification de votre choix.

<h3 id="Que faire si mon secret 2FA est compromis ?">Que faire si mon secret 2FA est compromis ?
<a href="#Que faire si mon secret 2FA est compromis ?" class="toc-anchor after"></a></h3>

Si vous pensez que votre secret 2FA peut être compromis, vous pouvez générer une nouvelle clé et invalider l'ancienne en sélectionnant le gros bouton rouge **Régénérer** dans les paramètres de votre profil utilisateur depuis l'administration.

