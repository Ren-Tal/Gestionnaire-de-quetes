# Board of Quests

## Description du projet

Board of Quests est un gestionnaire de tâches façon Trello, réinterprété dans un univers RPG médiéval-fantastique. Plutôt que de gérer des tâches classiques, l'utilisateur gère des **quêtes** affichées sur un tableau de guilde en bois, chaque quête étant représentée par un parchemin avec son titre, sa description, sa difficulté et sa récompense.

Le tableau est organisé en 4 colonnes représentant le cycle de vie d'une quête : **Disponible** → **En Cours** → **Terminé** (ou **Abandonné**). Les quêtes peuvent être créées, modifiées, supprimées et déplacées d'une colonne à l'autre par Drag & Drop, avec une sauvegarde automatique dans le navigateur.

## Pourquoi le thème RPG
Etant moi-même adepte du monde des RPG, j'ai choisis ce theme non seulement parce qu'il m'ai plus proche mais surtout pour la liberté qui s'offre dans le designe de celui-ci. Faire ce projet etait très plaisant grâce a cette possibilité.

## Architecture des composants
| Composant | Rôle |
|---|---|
| `App.vue` | Racine de l'app, gère le thème clair/sombre |
| `Board.vue` | Détient toutes les données (colonnes et quêtes), gère la recherche, les filtres, les statistiques et le localStorage |
| `Column.vue` | Affiche les quêtes d'une colonne, gère le drag and drop |
| `Card.vue` | Affiche et permet l'édition d'une quête |
| `Form.vue` | Formulaire de création d'une nouvelle quête |

## Démarche de développement
Pour ce projet, j'ai choisi la démarche suivante :
 - **Etape 1:** La carte statique
 - **Etape 2:** Le developpement des colonnes et de la Board en utilisant les cartes
 - **Etape 3:** L'implementation des props et des emit qui permettent le transfert d'information d'une composante a une autre
 - **Etape 4:** Le formulaire qui permet d'ajouter des cartes + l'edition des cartes deja presentes
 - **Etape 5:** L'ajout des v-if et computed pour l'affichage ainsi que les statistiques et la recherche filtrée
 - **Etape 6:** Persistance avec le localStorage
 - **Bonus:** finir les differents bonus et de finir le designe de tout le site (sachant que ce point je l'ai un peu fait au fur et a mesure du travail)

### 1. Partir du statique pour comprendre la structure
J'ai commencé par codé une carte statique dans `Card.vue`. Il s'agissait juste de commencé avec une interface pour avoir une idée sur la taille des cartes et le differents contenu de celle-ci avant de commencer a faire un peu la structure de `Column.vue` et de `Board.vue`.

### 2. Introduire les props pour rendre les composants réutilisables
Une fois la carte statique fonctionnelle, je l'ai rendue générique avec des `props` pour qu'un seul composant `Card.vue` puisse afficher n'importe quelle quête. J'ai ensuite fini de construire `Column.vue` et `Board.vue` par-dessus, en utilisant `v-for` pour générer dynamiquement les colonnes et les cartes à partir d'un tableau de données.

### 3. Faire communiquer les composants dans les deux sens
Pour la suppression d'une carte, j'ai mis en place le pattern `props` vers le bas / `emit` vers le haut : `Card.vue` émet un événement de suppression, que `Column.vue` relaie à son tour vers `Board.vue`, seul détenteur des données. Ce même principe a ensuite servi pour la modification des quêtes et pour le déplacement entre colonnes.

### 4. Construire les interactions utilisateur
J'ai ajouté un formulaire global (`Form.vue`) avec `v-model` pour créer des quêtes, avec validation du titre. J'ai ensuite ajouté l'édition directement sur la carte (bascule entre un mode affichage et un mode édition via une variable locale `isEditing`), puis le glisser-déposer entre colonnes avec la librairie `vue-draggable-plus` pour le changement des status, ce qui a n'etait pas facile a implementé vu que cela demandé une synchronisation des données local du parent avec des `watch` croisées.

### 5. Enrichir l'affichage avec de la logique conditionnelle et des données calculées
J'ai utilisé `v-if` pour adapter l'affichage des cartes selon leur statut (couleur, badge de victoire, masquage du bouton supprimer sur les quêtes terminées), puis des propriétés `computed` pour calculer les statistiques du tableau (nombre total de quêtes, taux de complétion) et pour filtrer les quêtes en temps réel selon une recherche textuelle.

### 6. Rendre les données persistantes
Enfin, j'ai mis en place la sauvegarde automatique avec un `watch` profond sur les colonnes dans `Board.vue`, couplé à `localStorage` et `JSON.stringify`/`JSON.parse`, avec restauration des données au chargement de la page dans le hook `created()`.

### 7. Aller plus loin avec les bonus
Une fois les fonctionnalités obligatoires terminées, j'ai ajouté : 
 - La recherche dynamique avec des filtres par champ (titre, description, status, ...)
 - Des statistique en temps réel pour voir le nombre de quêtes presentent, finis ainsi qu'un taux de réussite
 - Un CSS plus dans le style avec quelques animations (les sceaux de cire pulsants, les cartes qui s'inclines lorsque la souris passe dessus, l'ouverture du formulaire qui n'est plus present en bas de la page mais n'est   accessible que par le bouton en bas a droite, ...)
 - Un affichage adaptif pour mobil
 - Un thème clair/sombre qui change avec le bouton en bas a gauche
 - un système de tags pour les quêtes. Celle-ci doivent être séparé par une virgule lors de l'ajout d'une nouvelle carte avec le formulaire ou l'edit d'une carte déja présente. La recherche filtré peut être faite avec les tags aussi 

## Fonctionnalités obligatoires
- [x] Tableau à 4 colonnes (Disponible, En Cours, Terminé, Abandonné)
- [x] Cartes générées dynamiquement (`v-for`) avec titre, description, difficulté, récompense et tags
- [x] Formulaire d'ajout avec validation (`v-model`)
- [x] Édition directe sur les cartes
- [x] Communication `props`/`emit` entre tous les composants
- [x] Rendu conditionnel (`v-if`) : badge de victoire, couleurs selon le statut, masquage du bouton supprimer
- [x] Persistance avec `localStorage`

## Bonus implémentés
- [x] Drag & Drop entre colonnes (`vue-draggable-plus`)
- [x] Recherche dynamique + filtres avancés par champ
- [x] Statistiques en temps réel
- [x] Animations
- [x] Responsive
- [x] Thème clair/sombre
- [x] Système de tags

