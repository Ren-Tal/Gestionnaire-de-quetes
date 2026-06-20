# 🗡️ Board of Quests

## Description du projet

Board of Quests est un gestionnaire de tâches façon Trello, réinterprété 
dans un univers RPG médiéval-fantastique. Plutôt que de gérer des tâches 
classiques, l'utilisateur gère des **quêtes** affichées sur un tableau de 
guilde en bois, chaque quête étant représentée par un parchemin avec son 
titre, sa description, sa difficulté et sa récompense.

Le tableau est organisé en 4 colonnes représentant le cycle de vie d'une 
quête : **Disponible** → **En Cours** → **Terminé** (ou **Abandonné**). 
Les quêtes peuvent être créées, modifiées, supprimées et déplacées d'une 
colonne à l'autre par glisser-déposer, avec une sauvegarde automatique 
dans le navigateur.

## Pourquoi le thème RPG
[à toi de compléter]

## Lancer le projet
\`\`\`bash
npm install
npm run dev
\`\`\`

## Démarche de développement

### 1. Partir du statique pour comprendre la structure
J'ai commencé par coder une carte en dur dans `Card.vue`, sans aucune 
logique Vue, pour bien comprendre comment structurer le HTML et le CSS 
d'une quête avant d'y ajouter de la dynamique.

### 2. Introduire les props pour rendre les composants réutilisables
Une fois la carte statique fonctionnelle, je l'ai rendue générique avec 
des `props` pour qu'un seul composant `Card.vue` puisse afficher 
n'importe quelle quête. J'ai ensuite construit `Column.vue` et `Board.vue` 
par-dessus, en utilisant `v-for` pour générer dynamiquement les colonnes 
et les cartes à partir d'un tableau de données.

### 3. Faire communiquer les composants dans les deux sens
Pour la suppression d'une carte, j'ai mis en place le pattern 
`props` vers le bas / `emit` vers le haut : `Card.vue` émet un événement 
de suppression, que `Column.vue` relaie à son tour vers `Board.vue`, seul 
détenteur des données. Ce même principe a ensuite servi pour la 
modification des quêtes et pour le déplacement entre colonnes.

### 4. Construire les interactions utilisateur
J'ai ajouté un formulaire global (`Form.vue`) avec `v-model` pour créer 
des quêtes, avec validation du titre. J'ai ensuite ajouté l'édition 
directement sur la carte (bascule entre un mode affichage et un mode 
édition via une variable locale `isEditing`), puis le glisser-déposer 
entre colonnes avec la librairie `vue-draggable-plus`, ce qui a demandé 
de synchroniser une copie locale des quêtes avec les données du parent 
via des `watch` croisés.

### 5. Enrichir l'affichage avec de la logique conditionnelle et des données calculées
J'ai utilisé `v-if` pour adapter l'affichage des cartes selon leur statut 
(couleur, badge de victoire, masquage du bouton supprimer sur les quêtes 
terminées), puis des propriétés `computed` pour calculer les statistiques 
du tableau (nombre total de quêtes, taux de complétion) et pour filtrer 
les quêtes en temps réel selon une recherche textuelle.

### 6. Rendre les données persistantes
Enfin, j'ai mis en place la sauvegarde automatique avec un `watch` 
profond sur les colonnes, couplé à `localStorage` et `JSON.stringify`/
`JSON.parse`, avec restauration des données au chargement de la page 
dans le hook `created()`.

### 7. Aller plus loin avec les bonus
Une fois les fonctionnalités obligatoires terminées, j'ai ajouté : la 
recherche dynamique avec filtres par champ, des statistiques en temps 
réel, des animations CSS (survol, ouverture du formulaire, sceaux de 
cire pulsants), un mode responsive pour mobile, un thème clair/sombre, 
et un système de tags sur les quêtes.

## Architecture des composants
| Composant | Rôle |
|---|---|
| `App.vue` | Racine de l'app, gère le thème clair/sombre |
| `Board.vue` | Détient toutes les données (colonnes et quêtes), gère la recherche, les filtres, les statistiques et le localStorage |
| `Column.vue` | Affiche les quêtes d'une colonne, gère le drag and drop |
| `Card.vue` | Affiche et permet l'édition d'une quête |
| `Form.vue` | Formulaire de création d'une nouvelle quête |

## Fonctionnalités obligatoires
- ✅ Tableau à 4 colonnes (Disponible, En Cours, Terminé, Abandonné)
- ✅ Cartes générées dynamiquement (`v-for`) avec titre, description, difficulté, récompense et tags
- ✅ Formulaire d'ajout avec validation (`v-model`)
- ✅ Édition directe sur les cartes
- ✅ Communication `props`/`emit` entre tous les composants
- ✅ Rendu conditionnel (`v-if`) : badge de victoire, couleurs selon le statut, masquage du bouton supprimer
- ✅ Persistance avec `localStorage`

## Bonus implémentés
- ✅ Drag & Drop entre colonnes (`vue-draggable-plus`)
- ✅ Recherche dynamique + filtres par champ
- ✅ Statistiques en temps réel
- ✅ Animations
- ✅ Responsive
- ✅ Thème clair/sombre
- ✅ Système de tags

## Technologies utilisées
- Vue.js 3 (Options API)
- vue-draggable-plus
- CSS pur (sans framework)
