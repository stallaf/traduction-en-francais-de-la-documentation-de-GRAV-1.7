<h1 class="rem">Configuration recommandée</h1>

Comme pour toutes les autres applications, il est important que vous vérifiiez les options de configuration pour sécuriser et optimiser votre site.

<h2 id="Site de production">Site de production.
<a href="#Site de production" class="toc-anchor after"></a></h2>

Il est important de sécuriser votre site de production en durcissant la configuration. Pour ce faire, nous vous recommandons de définir votre configuration principale dans `user/config/` pour qu'elle contienne les paramètres par défaut que vous souhaitez utiliser dans votre environnement de production et de remplacer ces paramètres dans l'environnement de votre site de développement, tels que `user/env/localhost` ou user`/env /site.local`. Vous pouvez également remplacer les paramètres du site de production via les environnements, par exemple si vous utilisez une configuration multisite sur plusieurs domaines.

<h3 id="Configuration du système (<code>user/config/system.yaml</code>)"> Configuration du système (<code>user/config/system.yaml</code>).
<a href="#Configuration du système (<code>user/config/system.yaml</code>)" class="toc-anchor after"></a></h3>

```yaml
force_ssl: true       # Use HTTPS only (redirect from HTTP -> HTTPS)

cache:
  enabled: true       # Greatly speeds up the site
  check:
    method: hash      # Optimization, disables file modification checks for pages

twig:
  cache: true         # Greatly speeds up the site
  debug: false        # We do not want to display debug messages
  auto_reload: false  # Optimization, disables file modification checks for twig files
  autoescape: true    # Protects from many XSS attacks, but requires twig updates if used in older sites/themes/plugins

errors:
  display: 0          # Display only a simple error
  log: true           # Log errors for later inspection

debugger:
  enabled: false      # Never keep debugger enabled in a live site.
  censored: true      # In case if you happen to enable debugger, avoid displaying sensitive information

session:
  enabled: true       # NOTE: Disable sessions if you do not use user login and/or forms.
  secure: true        # Use this as your site should be using HTTPS only
  httponly: true      # Protects session cookies against client side scripts and XSS
  samesite: Strict    # Prevent all cross-site scripting attacks
  split: true         # Separate admin session from the site session for added security

strict_mode:          # Test your site before changing these. Removes backward compatibility and improves site security.
  yaml_compat: false
  twig_compat: false
  blueprint_compat: false
```

<h2 id="Site de développement">Site de développement.
<a href="#Site de développement" class="toc-anchor after"></a></h2>

Pour le serveur de développement, nous devons modifier quelques paramètres pour faciliter la mise à jour du site.

<h2 id="Configuration du système (<code>utilisateur/env/localhost/config/system.yaml</code>)">Configuration du système (<code>user/env/localhost/config/system.yaml</code>).
<a href="#Configuration du système (<code>utilisateur/env/localhost/config/system.yaml</code>)" class="toc-anchor after"></a></h2>

<div class = "notice tip">
<strong>ASTUCE</strong> : remplacez localhost par le nom de votre serveur local.
</div>

```yaml
force_ssl: false      # If the development site doesn't use SSL

cache:
  enabled: true       # Still keep cache enabled
  check:
    method: file      # Allow updating pages without clearing cache

twig:
  cache: true         # Still keep cache enabled
  debug: true         # We want to display debug messages
  auto_reload: true   # We may be editing twig files

errors:
  display: 1          # Display full backtrace if there are errors

debugger:
  enabled: true       # Debugger is handy to have
  censored: false     # We may want to see secure content in debugger

session:
  secure: false       # If the development site doesn't use SSL
  httponly: false     # If the development site doesn't use SSL

strict_mode:          # These settings help you to keep your site updated to use the latest standards
  yaml_compat: false
  twig_compat: false
  blueprint_compat: false
```
  
