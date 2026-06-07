<template>
  <div
    class="card"
    :class="{
      'status-disponible': quest.status === 'Disponible',
      'status-encours': quest.status === 'En Cours',
      'status-termine': quest.status === 'Terminé',
      'status-abandonne': quest.status === 'Abandonné',
    }"
  >
    <div v-if="!isEditing">
      <h2>{{ quest.title }}</h2>
      <h3>{{ quest.description }}</h3>
      <h3 v-bind="quest.difficulty!==''" >Difficulté: {{ quest.difficulty }}</h3>
      <p v-if="quest.status === 'Terminé'">
        <img src="/badge.png" alt="Trophy" /> Quête Terminée! récompense : {{ quest.reward }}
      </p>
      <p v-if="quest.status === 'En Cours' || quest.status === 'Disponible'">
        récompense : {{ quest.reward }}
      </p>
      <p v-if="quest.status === 'Abandonné'">
        🕳️ Quête Abandonnée! récompense perdue : {{ quest.reward }}
      </p>
      <div class="tags" v-if="quest.tags && quest.tags.length">
        <span v-for="tag in quest.tags" :key="tag" class="tag">
          #{{ tag }}
        </span>
      </div>
      <div class="card-actions">
        <button @click="startEdit" id="edit-btn"><img src="/edit.png" alt="Edit" /></button>
        <button @click="deleteCard" id="delete-btn"><img src="/effacer.png" alt="Delete" /></button>
      </div>
      <img v-if="quest.status === 'En Cours'" src="/En cours.png" class="sealC" />
      <img v-if="quest.status === 'Terminé'" src="/Termine.png" class="sealT" />
      <img v-if="quest.status === 'Abandonné'" src="/abondonné.png" class="sealA" />
    </div>
    <div v-else class="edit-form">
      <input v-model="editedQuest.title" :placeholder="quest.title" />
      <input
        v-model="editedQuest.description"
        :placeholder="quest.description || 'Description de la quête'"
      />
      <input v-model="editedQuest.difficulty" :placeholder="quest.difficulty || 'Difficulté'" />
      <input v-model="editedQuest.reward" :placeholder="quest.reward || 'Récompense'" />
      <input v-model="editedQuest.tagsInput" :placeholder="quest.tags ? quest.tags.join(', ') : 'Tags (séparés par des virgules)'" />
      <div class="edit-actions">
        <button @click="saveEdit" id="save-btn"><img src="/save.png" alt="Save" /></button>
        <button @click="cancelEdit" id="cancel-btn"><img src="/effacer.png" alt="Cancel" /></button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['quest'],
  emits: ['delete', 'update'],
  data() {
    return {
      isEditing: false,
      editedQuest: { ...this.quest },
    }
  },
  methods: {
    cancelEdit() {
      this.isEditing = false
      this.editedQuest = { ...this.quest }
    },
    startEdit() {
      this.editedQuest = { ...this.quest }
      this.isEditing = true
    },
    deleteCard() {
      this.$emit('delete', this.quest.id)
    },
    saveEdit() {
      if (!this.editedQuest.title) {
        alert('Le titre de la quête est requis.')
        return
      }
      if(this.editedQuest.difficulty!=='') {
        const validDifficulties = ['SSS', 'SS', 'S', 'A', 'B', 'C','D']
        if (!validDifficulties.includes(this.editedQuest.difficulty.toUpperCase())) {
          alert('La difficulté doit être l\'une des suivantes : SSS, SS, S, A, B, C, D ou laissée vide.')
          return
        }
        this.editedQuest.difficulty = this.editedQuest.difficulty.toUpperCase()
      }
      this.$emit('update', { updatedQuest: { ...this.editedQuest }, questId: this.quest.id })
      this.isEditing = false
    },
  },
}
</script>

<style scoped>

@media (max-width: 768px) {
  .card {
    width: 100%;            /* ← pleine largeur sur mobile */
    padding: 1.5rem 2rem;
  }
}

.card {
  position: relative;
  background-image: url('/Cartes.png');
  background-size: 105% 102%;
  background-repeat: no-repeat;
  border: none;
  padding: 2rem 3rem;
  width: 90%;
  min-height: 200px;
  word-break: break-word;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  cursor: pointer;
}

.card:hover {
    transform: translateY(-4px) rotate(0.5deg); 
    box-shadow: 0 8px 20px rgba(0, 0, 0, 0.4);
}

.card h2 {
  color: #1a0a00;
  font-size: 1.2rem;
  text-align: center;
  text-shadow: 1px 1px 2px rgba(255, 220, 150, 0.5);
}
.card h3 {
  color: #422e07;
  margin: 0.5rem 0;
  font-size: 0.95rem;
  text-align: center;
  text-shadow: 1px 1px 2px rgba(255, 220, 150, 0.5);
}

.card p {
  color: #2e1707;
  margin: 0.5rem 0;
  text-align: center;
  font-size: 1rem;
  font-style: italic;
  text-shadow: 1px 1px 2px rgba(255, 220, 150, 0.5);
}

#delete-btn {
  background: #c9922a;
  color: white;
  border: none;
  display: block;
  height: 30px;
  width: 30px;
  border-radius: 50%;
  cursor: pointer;
  align-items: center;
  margin-top: 1rem;
}

#edit-btn {
  background: #c9922a;
  color: white;
  border: none;
  padding: 0.5rem 1rem;
  border-radius: 4px;
  cursor: pointer;
  align-items: center;
  margin-top: 1rem;
}

.edit-form {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  padding: 1rem;
  border-radius: 4px;
  max-width: 300px;
  margin-bottom: 1rem;
  margin-top: 1rem;
  margin-bottom: 1rem;
}

.edit-form h3 {
  color: #2e1707;
  font:
    bold 1.2rem 'Arial',
    sans-serif;
  margin: 0;
}

.edit-form input,
.edit-form select {
  padding: 0.4rem;
  border: 1px solid #c9922a;
  border-radius: 4px;
  background: #ebc375;
  color: #2e1707;
  font-size: 0.9rem;
}

.edit-form button {
  padding: 0.4rem;
  background: #c9922a;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-weight: bold;
}

.edit-form button:hover {
  background: #a87820;
}

#edit-btn img,
#delete-btn img,
#save-btn img,
#cancel-btn img {
  width: 35px;
  height: 35px;
}

#edit-btn,
#delete-btn,
#save-btn,
#cancel-btn {
  background: transparent;
  border: none;
  cursor: pointer;
  padding: 0.3rem;
}

.sealC {
  position: absolute; /* ← positionné par rapport à .card */
  bottom: 25px;
  right: 40px;
  width: 70px;
  height: 70px;
  animation: pulse 2s infinite;
}

.sealT {
  position: absolute; /* ← positionné par rapport à .card */
  bottom: 10px;
  right: 40px;
  width: 100px;
  height: 90px;
  animation: pulse 2s infinite;
}

.sealA {
  position: absolute; /* ← positionné par rapport à .card */
  bottom: 25px;
  right: 40px;
  width: 70px;
  height: 70px;
  animation: pulse 2s infinite;
}

.card-actions {
  display: flex;
  justify-content: bottom; /* ← centre les boutons */
  gap: 0.5rem;
  margin-top: 0.5rem;
}
.edit-actions {
  display: flex;
  justify-content: center; /* ← centre les boutons */
  gap: 0.5rem;
  margin-top: 0.5rem;
}

@keyframes pulse {
    0%   { transform: scale(1); }
    50%  { transform: scale(1.08); }
    100% { transform: scale(1); }
}

.tags {
  display: flex;
  flex-wrap: wrap;
  gap: 0.3rem;
  justify-content: center;
  margin-top: 0.5rem;
}

.tag {
  background: rgba(174, 42, 201, 0.616);
  color: #2b1403;
  padding: 0.2rem 0.5rem;
  border-radius: 10px;
  font-size: 0.75rem;
  border:2px solid #6d21ac;
  font-style: italic;
}
</style>
