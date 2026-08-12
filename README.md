# 🍽️ Bonne Pioche

*« On ne se prive pas. On pioche mieux. »*

Application de planification de repas pour deux sportifs épicuriens en prépa Hyrox. Chaque semaine, on swipe ensemble sur des recettes façon Tinder, l'app compose le menu Lundi→Dimanche et génère la liste de courses toute seule.

## Le concept

- **🃏 Pioche** — deux modes : 🤝 *Ensemble* (on tranche à deux carte par carte) ou 💘 *Chacun son tour* (chacun swipe de son côté sur les mêmes cartes, seuls les matchs communs entrent au menu, avec re-pioche par manches tant qu'il manque des plats) : 7 dîners batch-cooking (cuisinés le soir en double portion, remangés le lendemain midi), puis les petits-déjeuners, puis les collations.
- **📅 Semaine** — le menu généré depuis les swipes, réorganisable jour par jour, avec remplacement de plat à la volée.
- **🛒 Courses** — liste agrégée automatiquement à partir du menu, groupée par rayon, cochable en magasin, copiable en un tap.
- **❤️ Favoris** — les coups de cœur sont conservés et re-proposés en priorité dans les pioches suivantes.
- **🧠 Anti-répétition** — l'app mémorise les 3 dernières semaines et évite de resservir les mêmes plats.
- **📡 Synchro deux téléphones** (optionnelle) — avec une base Firebase gratuite (voir `GUIDE_FIREBASE.md`), chacun installe l'app sur son téléphone : pioche à distance chacun à son rythme, matchs 💘 révélés des deux côtés, menu, liste de courses et favoris partagés en temps quasi réel.

## La base de recettes

400 recettes originales générées et validées sur mesure (dont 40 « express » réellement faisables en 5–10 minutes chrono) : 7 univers de dîners (Méditerranée & Levant, Asie douce, Amérique latine, Terroir & Europe revisités, Océan, Veggie protéiné, et « Levant une-plaque — esprit Simple », inspiré de la philosophie du livre de Yotam Ottolenghi : ≤ 10 ingrédients, cuisson une-plaque, gros goûts sans effort), 3 univers de petits-déjeuners et 3 de collations.

Toutes respectent le cahier des charges santé du foyer :

- **Côlon irritable / reflux** : zéro friture, épices douces, acidité maîtrisée, oignon/ail bien cuits et modérés, cuissons douces.
- **Psoriasis / inflammation** : poissons gras riches en oméga-3, curcuma, gingembre, légumes colorés, huile d'olive ; ultra-transformé et charcuterie industrielle exclus, viande rouge rare.
- **Sport intensif** : dîners à 30–44 g de protéines par portion, macros complètes affichées (protéines, kcal, glucides, lipides, fibres — estimations).

## Utilisation

Ouvrir simplement **[l'app en ligne](https://mathieubigardpro-afk.github.io/bonne-pioche/)** — ou `index.html` dans n'importe quel navigateur.

Il existe aussi **[Bonne Pioche Facile](https://mathieubigardpro-afk.github.io/bonne-pioche-facile/)** : la même application, simplifiée (3 onglets, gros textes, appairage guidé) pour les membres de la famille moins à l'aise avec un téléphone.

Sur téléphone : ouvrir l'URL, puis **Partager → Sur l'écran d'accueil** (iOS) ou **⋮ → Ajouter à l'écran d'accueil** (Android). L'app s'installe comme une vraie application.

Sans synchro configurée, les données restent locales à l'appareil. Avec la synchro (⚙️ dans l'app + `GUIDE_FIREBASE.md`), tout le foyer est partagé entre vos deux téléphones.

## Sous le capot

- Un seul fichier `index.html` autonome : HTML + CSS + JS + les 400 recettes embarquées. Aucune dépendance, aucun compte, aucun traceur publicitaire. La synchro à deux téléphones (optionnelle) passe par une base Google Firebase hébergée en Europe — détail complet dans [confidentialite.html](confidentialite.html).
- Un service worker (`sw.js`) garde une copie de l'app : elle s'ouvre même sans réseau.
- Base de recettes générée par une orchestration multi-agents (Claude Fable 5 orchestrateur + 10 agents Sonnet en parallèle, un univers culinaire chacun), puis validée programmatiquement : schéma, doublons, cohérence kcal/macros, normalisation des unités pour l'agrégation des courses.

---

*Projet personnel de Mathieu — généré avec Claude (Cowork).*
