# web-swissroc-phishing

Simulation de **sensibilisation au phishing** à usage interne, pilotée par le
Service IT de Swissroc (DSI : Damien Walther). Ce site n'a qu'un but pédagogique :
amener un collaborateur à reconnaître une fausse page de connexion.

## Principe de conception (important)

- **Aucun identifiant n'est collecté.** Les pages sont 100 % statiques : pas de
  backend, pas de base de données, pas d'appel réseau sur les saisies, pas de
  stockage de mot de passe. Au clic sur « Se connecter », on se contente de
  rediriger. Ce qui a été tapé est ignoré et disparaît.
- **Le piège doit rester détectable.** Le signal principal est volontairement
  laissé visible : l'URL. Le site est sur `swisssroc.com` (trois « s ») et la
  « connexion Microsoft » ne se fait pas sur `login.microsoftonline.com`. Un
  collaborateur vigilant a une vraie chance de repérer la supercherie.

## Arborescence et flux

```
/                              → page de connexion imitant Microsoft
/sites/swissroc-intranet/      → faux Swissroc Hub + message « vous êtes piégé »
/404.html                      → renvoie tout chemin inconnu vers la connexion
/CNAME                         → domaine custom (hébergement GitHub Pages)
```

Flux reproduisant un vrai SSO Microsoft :

1. Le lien de l'e-mail pointe vers `…/sites/swissroc-intranet/`.
2. Accès « non authentifié » → redirection vers `/` (connexion).
3. Saisie puis « Se connecter » → retour sur `…/sites/swissroc-intranet/?from=login`.
4. La page nettoie l'URL et affiche la révélation + la vidéo obligatoire.

## Vidéo obligatoire

Dans `sites/swissroc-intranet/index.html`, bloc `.video-frame` : remplacer le
placeholder par l'`<iframe>` d'embed (YouTube ou SharePoint Stream).

## Hébergement

Prévu pour un hébergement statique (GitHub Pages ou Cloudflare Pages) sur le
domaine `sharepoint.swisssroc.com`. Voir le fichier `CNAME`.

> Cadre : exercice autorisé en interne. À coupler avec une validation
> RH/direction et une information générale préalable des collaborateurs
> (nLPD / droit du travail suisse).
