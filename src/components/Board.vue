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
        quests: col.quests.filter((quest) =>
          quest.title.toLowerCase().includes(this.searchQuery.toLowerCase()),
        ),
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
            },
            {
              id: 2,
              title: 'Sauver la princesse',
              description: 'La princesse est retenue captive dans le château du dragon.',
              status: 'Disponible',
              reward: "1000 pièces d'or",
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
      <div class="modal-content">
        <Formu @add-quest="addQuest" @cancel="showForm = false" />
      </div>
    </div>
  </div>
</template>

<style>
.board {
  display: flex;
  gap: 1rem;
  padding: 1rem;
  border-radius: 4px;
  background-image: 
    linear-gradient( 
      to bottom,
      transparent 50%,
      rgba(20, 10, 5, 0.9) 90%,
      rgb(20, 10, 5) 100%
    ),
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

  position: absolute;   /* ← fixed au lieu de absolute */
  top: 1rem;
  right: 1rem;
  z-index: 50;       /* ← au dessus du reste mais sous le modal */
}

.stats p {
  margin: 0;
  text-shadow: 1px 1px 2px rgba(255,10,100,0.7);
}

.search-bar {
  padding: 0.5rem 1rem;
  border: 2px solid #745d32;
  border-radius: 4px;
  background: #d6b263;
  color: #2e1707;
  font-size: 1rem;
  margin-bottom: 3.5rem;
  margin-top: -1.5rem;
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
  background: #f9e5b6;
  border: 2px solid #c9922a;
  border-radius: 8px;
  padding: 1rem;
  min-width: 320px;
}

</style>
