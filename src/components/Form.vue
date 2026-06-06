<template>
  <div class="form">
    <h2>Ajouter une quête</h2>
    <input v-model="newQuest.title" placeholder="Titre de la quête" required />
    <input v-model="newQuest.description" placeholder="Description de la quête" />
    <input v-model="newQuest.difficulty" placeholder="Difficulté" />
    <input v-model="newQuest.reward" placeholder="Récompense" required />
    <select v-model="newQuest.status">
      <option value="Disponible">Disponible</option>
      <option value="En Cours">En Cours</option>
      <option value="Terminé">Terminé</option>
      <option value="Abandonné">Abandonné</option>
    </select>
    <div class="form-actions">
      <button @click="submitQuest" id="save-btn"><img src="/save.png" alt="Save" /></button>
      <button @click="$emit('cancel')" id="cancel-btn"><img src="/effacer.png" alt="Annuler" /></button>
    </div>
  </div>
</template>

<script>
export default {
  emits: ['add-quest', 'cancel'],
  data() {
    return {
      newQuest: {
        title: '',
        description: '',
        difficulty: '',
        reward: '',
        status: 'Disponible',
      },
    }
  },
  methods: {
    submitQuest() {
      if (!this.newQuest.title) {
        alert('Le titre de la quête est requis.')
        return
      }
      if(this.newQuest.difficulty!=='') {
        const validDifficulties = ['SSS', 'SS', 'S', 'A', 'B', 'C','D']
        if (!validDifficulties.includes(this.newQuest.difficulty.toUpperCase())) {
          alert('La difficulté doit être l\'une des suivantes : SSS, SS, S, A, B, C, D ou laissée vide.')
          return
        }
        this.newQuest.difficulty = this.newQuest.difficulty.toUpperCase()
      }
      this.$emit('add-quest', { ...this.newQuest })
      this.newQuest = {
        title: '',
        description: '',
        difficulty: '',
        reward: '',
        status: 'Disponible',
      }
    },
  },
}
</script>

<style scoped>
.form {
  display: flex;
  flex-direction: column;
  align-items: center;    /* ← centre tout horizontalement */
  gap: 0.75rem;
  padding: 2.5rem 3rem;   /* ← plus de padding pour rentrer dans le parchemin */
  background-image: url('/Cartes.png');
  background-size: 100% 100%;
  background-repeat: no-repeat;
  border: none;
  width: 350px;
  min-height: 400px;
  background-color: transparent;
}

.form h2 {
  font-family: 'TaFont', serif; /* ← ta font RPG */
  color: #1a0a00;
  text-align: center;
  margin-bottom: 0.5rem;
}

.form input,
.form select {
  width: 80%;              /* ← pas toute la largeur, reste dans le parchemin */
  padding: 0.4rem 0.75rem;
  border: 1px solid #c9922a;
  border-radius: 4px;
  background: rgba(244, 228, 193, 0.8);
  color: #2e1707;
  font-size: 0.9rem;
  text-align: center;      /* ← texte centré dans les champs */
}

.form-actions {
  display: flex;
  gap: 1rem;
  justify-content: center;
  margin-top: 0.5rem;
}

.form button {
  padding: 0.4rem;
  background: #c9922a;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-weight: bold;
}

.form button:hover {
  background: #a87820;
}

#save-btn img, #cancel-btn img {
  width: 35px;
  height: 35px;
}

#save-btn, #cancel-btn {
  background: transparent;
  border: none;
  cursor: pointer;
  padding: 0.3rem;
}
</style>
