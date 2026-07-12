# Salon Paps — site vitrine + réservation en direct (démo commerciale)

## Contexte business
- Salon réel : **Salon Paps** (coiffure mixte & esthétique, coloriste/visagiste), 37 Rue Racine, 76600 Le Havre, tél. 02 35 43 52 65.
- Site **démo non encore vendu** au salon. Éditeur légal provisoire : Tommy (tommyabengali9@gmail.com) — voir `legal.html`, à mettre à jour avec le SIRET réel du salon au moment de la vente.
- Contrairement à 66th Barber Street, ce salon **n'a pas de lien Planity ni d'outil de réservation en ligne existant** — d'où un vrai système de créneaux disponibles développé sur mesure (voir plus bas).
- Identité voulue par le client (Tommy) : chic, épuré, effet "waw", intuitif, mise en avant des prix, suivi temps réel des horaires d'ouverture, réservation facile avec créneaux réellement disponibles.

## Stack
- Site statique HTML/CSS/JS (polices Playfair Display + Inter), palette crème/terracotta/blush.
- **Netlify Functions** (`netlify/functions/slots.js`, CommonJS) + **Netlify Blobs** (`@netlify/blobs`) pour stocker les créneaux pris par jour (clé = date, valeur = `{taken:[...], bookings:[...]}`).
- Règles d'ouverture codées en dur dans `slots.js` (`HOURS`) : Mar-Ven 9h-19h, Sam 9h-17h, fermé Lundi/Dimanche. **Si les horaires du salon changent, modifier cet objet.**
- Le widget front (`index.html`, section `#reserver`) appelle `GET /.netlify/functions/slots?date=YYYY-MM-DD` pour lister les créneaux, et `POST` la même fonction pour réserver (avec un fallback Netlify Forms nommé "reservation" pour notification email).
- **Variables d'environnement Netlify requises** (déjà configurées sur ce site, scope Builds+Functions+Runtime, tous contextes) :
  - `BLOBS_SITE_ID` — l'ID du site Netlify (nécessaire car l'injection automatique du contexte Blobs ne fonctionnait pas dans ce déploiement).
  - `BLOBS_TOKEN` — Personal Access Token Netlify dédié ("salon-paps blobs", sans expiration).
  - Si tu recrées le site Netlify ou changes de compte, il faudra régénérer ces deux valeurs et les remettre dans Project configuration → Environment variables.
- `netlify.toml` : `command = "npm install"` (indispensable pour bundler `@netlify/blobs` même sans build front), `functions.directory = "netlify/functions"`, `node_bundler = "esbuild"`.
- JSON-LD schema.org inclut un `aggregateRating` (4.8 / 312 avis) — **cette note vient d'une source vérifiée (fiche Google du salon)**, contrairement à 66th Barber Street. Ne pas la modifier sans revérifier.

## État des prix (IMPORTANT, en attente)
- Aucune grille tarifaire officielle et fiable n'a été trouvée en ligne pour ce salon (Planity, PagesJaunes, Groupon, etc. ne listent rien de solide).
- Le site affiche actuellement des prix "sur devis" en placeholder (`<div class="price tbd">sur devis</div>` dans `index.html`).
- **Reste à faire : obtenir les vrais tarifs directement auprès du salon**, puis remplacer les placeholders par la grille réelle.

## Fichiers clés
- `index.html` — site complet (hero, transformations, prestations avec prix, réservation, horaires temps réel).
- `legal.html` — mentions légales & confidentialité.
- `netlify/functions/slots.js` — logique de créneaux + Blobs (voir ci-dessus).
- `netlify.toml`, `package.json` — config déploiement/dépendances.
- `images/` — photos du salon (façade, intérieur, transformations coiffure), déjà compressées (max 1600px, qualité 82).

## Bugs connus déjà résolus
- Le widget affichait "le salon est fermé" pour toutes les dates à cause d'une erreur de configuration Netlify Blobs (`siteID`/`token` non injectés automatiquement) — corrigé en passant ces valeurs explicitement via les variables d'environnement ci-dessus. Si l'erreur revient après un changement de compte/site Netlify, c'est probablement la même cause.

## Prochaines étapes possibles
- Récupérer les vrais tarifs du salon.
- Éventuellement dupliquer le check-up de présentation et le script d'appel déjà faits pour 66th Barber Street.
- Vente du site pas encore conclue.
