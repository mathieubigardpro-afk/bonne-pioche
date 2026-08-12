# 📡 Guide : activer la synchro entre vos deux téléphones

Dix minutes, une seule fois, depuis un ordinateur. À la fin, vous aurez une petite base de données gratuite dans le cloud, et vos deux téléphones partageront pioche, menu, courses et favoris.

## Étape 1 — Créer le projet Firebase (~4 min)

1. Va sur **https://console.firebase.google.com** et connecte-toi avec un compte Google.
2. Clique **Créer un projet** → nomme-le `bonne-pioche` → désactive Google Analytics (inutile) → **Créer**.

## Étape 2 — Créer la base de données (~3 min)

1. Dans le menu de gauche : **Créer → Realtime Database** (⚠️ *Realtime Database*, pas "Cloud Firestore").
2. Clique **Créer une base de données** → choisis l'emplacement **Belgique (europe-west1)** → démarre en **mode verrouillé** → **Activer**.
3. En haut de la page s'affiche l'URL de ta base, du type :
   `https://bonne-pioche-xxxxx-default-rtdb.europe-west1.firebasedatabase.app`
   **Copie-la précieusement** — c'est elle que l'app te demandera.

## Étape 3 — Autoriser l'app à lire/écrire (~2 min)

1. Toujours dans Realtime Database, onglet **Règles**.
2. Remplace tout le contenu par exactement ceci, puis **Publier** :

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

Ce que ça fait : toute la base est fermée, sauf les "foyers" (`rooms`). Un foyer n'est accessible qu'à qui connaît son code — c'est le principe du code secret. Pour deux utilisateurs, le niveau de risque est celui d'un lien de partage privé.

> ℹ️ **Source de vérité** : ces règles sont détaillées et expliquées dans
> [`FIREBASE_REGLES.md`](FIREBASE_REGLES.md) (le document le plus récent). En cas de
> différence entre les deux fichiers, c'est celui-là qui fait foi.

## Étape 4 — Connecter les deux téléphones (~2 min)

Sur **ton** téléphone (une fois l'app installée depuis l'URL GitHub Pages) :

1. Touche **⚙️** en haut à droite de l'app.
2. Colle l'**URL de la base** (étape 2).
3. Touche **🎲 Générer** pour créer le code du foyer.
4. Choisis **Joueur 1**, mets ton prénom → **Connecter ✅** (le point vert s'allume à côté du ⚙️).
5. Touche **📋 Copier l'invitation** et envoie-la à ta copine.

Sur **son** téléphone : mêmes réglages avec l'URL et le code de l'invitation, mais elle choisit **Joueur 2** et son prénom.

C'est tout. Lancez une **📡 Pioche à distance** depuis l'onglet Pioche : chacun swipe quand il veut, les matchs 💘 se révèlent quand vous avez fini tous les deux, et le menu validé apparaît sur les deux téléphones.

## Bon à savoir

- **Gratuit** : le palier gratuit de Firebase (1 Go de stockage, 10 Go/mois de transfert) représente des années d'utilisation pour deux personnes.
- **Le code du foyer est votre secret** : quiconque le connaît (avec l'URL) peut lire et modifier vos menus. Ne le publiez nulle part. Pour en changer : ⚙️ → 🎲 Générer un nouveau code sur les deux téléphones (l'historique repart de zéro).
- **Hors ligne** : sans réseau, l'app continue de fonctionner en local (menu et liste déjà chargés) ; le point à côté du ⚙️ passe au rouge et la synchro reprend au retour du réseau.
- **Sans synchro** : les modes 🤝 Ensemble et 💘 Tour à tour marchent toujours à 100 % sans Firebase, sur un seul téléphone.
