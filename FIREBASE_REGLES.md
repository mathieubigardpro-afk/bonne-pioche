# Sécuriser la base Firebase de Bonne Pioche

## Situation actuelle

Les deux applications utilisent la Realtime Database `bonne-pioche-9cb86` (europe-west1)
en accès REST, sans authentification. La confidentialité repose uniquement sur le secret
du code de foyer (ex. `fac-citron-miel-42`) : quiconque connaît un code peut lire et
écrire les données de ce foyer. Personne ne peut trouver un code par simple navigation
(il faut le deviner), mais des règles ouvertes `.read: true / .write: true` à la racine
permettraient de lister toute la base. Il faut donc interdire la lecture à la racine.

## Règles recommandées

Dans la [console Firebase](https://console.firebase.google.com) → Realtime Database →
Règles, remplacer le contenu par :

```json
{
  "rules": {
    ".read": false,
    ".write": false,
    "rooms": {
      "$room": {
        ".read": true,
        ".write": true,
        ".validate": "$room.matches(/^(bp|fac)-[a-z0-9-]{4,60}$/)"
      }
    }
  }
}
```

(Les applications rangent chaque foyer sous `/rooms/<code>` — les règles suivent
cette structure.)

Ce que ça change :

- **Impossible de lire la racine** (`GET /.json` renvoie « permission denied ») :
  on ne peut plus énumérer les foyers existants ni aspirer toute la base.
- **Chaque foyer reste accessible à qui connaît son code exact** — c'est le modèle
  « le code est la clé » sur lequel reposent les deux applications ; aucune modification
  du code de l'app n'est nécessaire.
- **Les noms de foyers sont contraints** au format généré par l'application
  (`fac-…` pour Bonne Pioche Facile, `bp-…` pour les codes générés dans les réglages V1).

## Limites assumées (et pistes futures)

- Un code deviné ou divulgué donne accès au foyer. Parade actuelle : codes à
  ~50 000 combinaisons pour Facile (mot-mot-nombre sur 24 mots), 16 caractères
  aléatoires pour V1, et vérification d'existence à l'appairage.
- Pour aller plus loin un jour : Firebase Auth anonyme + règle
  `auth != null`, ou un jeton par foyer stocké dans `profil`. Ça impose de modifier
  l'application ; à envisager si l'usage dépasse le cadre familial.
- Penser aussi à activer **App Check** (console Firebase) si l'app est publiée
  sur l'App Store, pour limiter l'accès aux clients légitimes.

## Vérification après déploiement des règles

```bash
# doit échouer (permission denied) :
curl "https://bonne-pioche-9cb86-default-rtdb.europe-west1.firebasedatabase.app/.json"

# doit répondre (null ou données) :
curl "https://bonne-pioche-9cb86-default-rtdb.europe-west1.firebasedatabase.app/rooms/fac-test-verif-00.json"
```
