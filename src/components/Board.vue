<script>
import Column from './Column.vue'
import Formu from './Form.vue'

export default {
  components: {
    Column,
    Formu,
  },
  computed: {
    totalQuests() {
      return this.Columns.reduce((total, column) => total + column.quests.length, 0)
    },
    completedQuests() {
      const terminee = this.Columns.find((col) => col.title === 'Terminé')
      return terminee ? terminee.quests.length : 0
    },
    completedRate() {
      if (this.totalQuests === 0) return 0
      return Math.round((this.completedQuests / this.totalQuests) * 100)
    },
    filteredColumns() {
      return this.Columns.map((col) => ({
        ...col,
        quests: col.quests.filter((quest) => {
          const query = this.searchQuery.toLowerCase()
          switch (this.searchField) {
            case 'titre':
              return quest.title.toLowerCase().includes(query)
            case 'description':
              return quest.description.toLowerCase().includes(query)
            case 'reward':
              return quest.reward.toLowerCase().includes(query)
            case 'difficulty':
              return quest.difficulty.toLowerCase().includes(query)
            default:
              return (
                quest.title.toLowerCase().includes(query) ||
                quest.description.toLowerCase().includes(query) ||
                quest.reward.toLowerCase().includes(query) ||
                quest.difficulty.toLowerCase().includes(query)
              )
          }
        }),
      }))
    },
  },
  methods: {
    update({ columnId, quests }) {
      const column = this.Columns.find((col) => col.id === columnId)
      if (column) {
        column.quests = quests.map((quest) => ({
          ...quest,
          status: column.title,
        }))
      }
    },
    deleteQuest({ columnId, questId }) {
      const column = this.Columns.find((col) => col.id === columnId)
      if (column) {
        column.quests = column.quests.filter((quest) => quest.id !== questId)
      }
    },
    addQuest(newQuest) {
      const column = this.Columns.find((col) => col.title === newQuest.status)
      const sameTitleQuest = column.quests.find((quest) => quest.title === newQuest.title)
      if (sameTitleQuest) {
        alert('Une quête avec ce titre existe déjà dans cette colonne.')
        return
      }
      if (column) {
        column.quests.push({ id: this.counter++, ...newQuest })
        this.showForm = false
      }
    },
    updateQuest({ columnId, updatedQuest, questId }) {
      const column = this.Columns.find((col) => col.id === columnId)
      if (column) {
        const questIndex = column.quests.findIndex((quest) => quest.id === questId)
        if (questIndex !== -1) {
          const sameTitleQuest = column.quests.find(
            (quest) => quest.title === updatedQuest.title && quest.id !== questId,
          )
          if (sameTitleQuest) {
            alert('Une quête avec ce titre existe déjà dans cette colonne.')
            return
          }
          column.quests[questIndex] = { ...updatedQuest }
        }
      }
    },
  },
  data() {
    return {
      searchField: 'tout',
      searchQuery: '',
      counter: 5,
      showForm: false,
      Columns: [
        {
          id: 1,
          title: 'Disponible',
          quests: [
            {
              id: 1,
              title: "Trouver l'épée magique",
              description: "L'épée magique est cachée dans la forêt enchantée.",
              status: 'Disponible',
              reward: "500 pièces d'or",
              Difficulty: 'A',
            },
            {
              id: 2,
              title: 'Sauver la princesse',
              description: 'La princesse est retenue captive dans le château du dragon.',
              status: 'Disponible',
              reward: "1000 pièces d'or",
              Difficulty: 'B',
            },
          ],
        },
        {
          id: 2,
          title: 'En Cours',
          quests: [
            {
              id: 3,
              title: 'Défendre le château',
              description: 'Défendre le château contre les attaques ennemies.',
              status: 'En Cours',
              reward: "1500 pièces d'or",
              Difficulty: 'C',
            },
          ],
        },
        {
          id: 3,
          title: 'Terminé',
          quests: [
            {
              id: 4,
              title: 'Trouver le trésor caché',
              description: 'Le trésor est caché dans la grotte mystérieuse.',
              status: 'Terminé',
              reward: "2000 pièces d'or",
              Difficulty: 'A',
            },
          ],
        },
        {
          id: 4,
          title: 'Abandonné',
          quests: [
            {
              id: 5,
              title: 'Explorer la forêt interdite',
              description: 'Explorer la forêt interdite à la recherche de secrets.',
              status: 'Abandonné',
              reward: "500 pièces d'or",
              Difficulty: 'S',
            },
          ],
        },
      ],
    }
  },
  created() {
    const saved = localStorage.getItem('columns')
    if (saved) {
      this.Columns = JSON.parse(saved)
    }
  },
  watch: {
    Columns: {
      handler(newColumns) {
        localStorage.setItem('columns', JSON.stringify(newColumns))
      },
      deep: true,
    },
  },
}
</script>

<template>
  <div class="board-container">
    <input v-model="searchQuery" placeholder="Rechercher une quête..." class="search-bar" />
    <select v-model="searchField" class="search-field">
      <option value="tout">Tout</option>
      <option value="titre">Titre</option>
      <option value="description">Description</option>
      <option value="reward">Récompense</option>
      <option value="difficulty">Difficulté</option>
    </select>
    <div class="stats">
      <p>Total Quêtes: {{ totalQuests }}</p>
      <p>Quêtes Terminées: {{ completedQuests }}</p>
      <p>Taux de Completion: {{ completedRate }}%</p>
    </div>
    <div class="board">
      <Column
        :column="column"
        v-for="column in filteredColumns"
        :key="column.id"
        @delete-quest="deleteQuest"
        @update-quest="updateQuest"
        @update-quests="update"
      />
    </div>
    <button class="btn-float" @click="showForm = true">
      <img src="/form.png" alt="Ajouter" />
    </button>
    <div class="modal-overlay" v-if="showForm" @click.self="showForm = false">
      <Transition name="fade" appear>
        <div class="modal-content">
          <Formu @add-quest="addQuest" @cancel="showForm = false" />
        </div>
      </Transition>
    </div>
  </div>
</template>

<style>
@media (max-width: 768px) {
  .board {
    flex-direction: column !important; /* ← !important pour forcer */
    gap: 1rem;
    background-image: none;
    background-color: rgba(20, 10, 5, 0.8);
    padding: 0.5rem;
    width: 100%;
    overflow: hidden;
  }

  .column {
    width: 100% !important;
    max-width: 100% !important;
    min-width: 0 !important;
    flex: none !important;
  }

  .stats {
    position: static !important;
    top: 0;
    right: 0;
    width: 100%;
    height: auto;
    background-image: none;
    margin-top: 4rem;
    font-size: 0.8rem;
    flex-direction: row;
    justify-content: space-around;
  }

  .search-bar {
    padding-top: 0.5rem;
    width: 100%;
  }

  .board-container {
    display: flex;
    flex-direction: column;
    padding-top: 0.5rem;
  }
  
  .search-field {
    padding: 0 0;
  }
}

.board {
  display: flex;
  gap: 1rem;
  padding: 1rem;
  border-radius: 4px;
  background-image:
    linear-gradient(to bottom, transparent 50%, rgba(20, 10, 5, 0.9) 90%, rgb(20, 10, 5) 100%),
    url('/board.png');
  background-size: 101% 100%;
  background-position: center top;
  background-attachment: absolute;
  background-color: rgb(20, 10, 5); /* ← le reste de la page sera brun foncé */
  flex-direction: row;
}

.board-container {
  padding: 1rem;
  padding-top: 100px; /* ← espace pour les stats */
}

.stats {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 0.3rem;
  color: #c29742;
  font-weight: bold;
  font-size: 1rem;
  background-image: url('/stats.png');
  background-size: 100% 100%;
  background-repeat: no-repeat;
  background-position: right top;
  border: none;
  width: 450px;
  height: 180px;
  padding: 1rem 2rem;

  position: absolute; /* ← fixed au lieu de absolute */
  top: 1rem;
  right: 1rem;
  z-index: 50; /* ← au dessus du reste mais sous le modal */
}

.stats p {
  margin: 0;
  text-shadow: 1px 1px 2px rgba(255, 10, 100, 0.7);
}

.search-bar {
  padding: 0.5rem 1rem;
  border: 1px solid #c9922a;
  border-radius: 4px;
  background: rgba(244, 228, 193, 0.8);
  color: #2e1707;
  font-size: 1rem;
  margin-top: -4.5rem;
  display: block;
  width: 300px;
}

/* Bouton flottant */
.btn-float {
  position: fixed;
  bottom: 2rem;
  right: 2rem;
  width: 60px;
  height: 60px;
  border-radius: 50%;
  background: #c9922a;
  border: 2px solid #2e1707;
  cursor: pointer;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.5);
  z-index: 100;
}

.btn-float img {
  width: 30px;
  height: 30px;
}

.btn-float:hover {
  background: #a87820;
}

/* Overlay sombre derrière le formulaire */
.modal-overlay {
  position: fixed;
  inset: 0; /* ← couvre tout l'écran */
  background: rgba(0, 0, 0, 0.6);
  backdrop-filter: blur(4px); /* ← flou derrière */
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 200;
}

.modal-content {
  padding: 1rem;
  min-width: 320px;
}

.fade-enter-active,
.fade-leave-active {
  transition: all 0.3s ease;
}

.fade-enter-from {
  opacity: 0;
  transform: scale(0.8) translateY(-30px); /* ← arrive du haut en grandissant */
}

.fade-leave-to {
  opacity: 0;
  transform: scale(0.8) translateY(-30px); /* ← repart vers le haut en rétrécissant */
}

.search-field {
  padding: 0.5rem 1rem;
  border: 2px solid #c9922a;
  border-radius: 4px;
  background: #f4e4c1;
  color: #2e1707;
  font-size: 1rem;
  margin: 0.5rem 0;
  cursor: pointer;
}
</style>
