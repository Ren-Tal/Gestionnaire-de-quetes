<template>
    <div class="form">
        <h2>Ajouter une quête</h2>
        <input v-model="newQuest.title" placeholder="Titre de la quête" />
        <input v-model="newQuest.description" placeholder="Description de la quête" />
        <select v-model="newQuest.status">
            <option value="Disponible">To Do</option>
            <option value="En Cours">In Progress</option>
            <option value="Terminé">Done</option>
            <option value="Abandonné">Abandoned</option>
        </select>
        <button @click="submitQuest">Ajouter</button>
    </div>
</template>

<script>
export default {
    emits: ['add-quest'],
    data() {
        return {
            newQuest: {
                title: '',
                description: '',
                status: 'Disponible'
            }
        }
    },
    methods: {
        submitQuest() {
            if(!this.newQuest.title) {
                alert('Le titre de la quête est requis.');
                return
            }
            this.$emit('add-quest', { ...this.newQuest });
            this.newQuest = {
                title: '',
                description: '',
                status: 'Disponible'
            }
        }
    }
}
</script>

<style scoped>
.form {
  display: flex;
  flex-direction: column; 
  gap: 0.5rem;
  padding: 1rem;
  background: #f9e5b6;
  border: 2px solid #c9922a;
  border-radius: 4px;
  max-width: 300px;       
  margin-bottom: 1rem;
  margin-top: 1rem;
  margin-bottom: 1rem;
}

.form h3 {
  color: #2e1707;
  font: bold 1.2rem 'Arial', sans-serif;
  margin: 0;
}

.form input,
.form select {
  padding: 0.4rem;
  border: 1px solid #c9922a;
  border-radius: 4px;
  background: #f4e4c1;
  color: #2e1707;
  font-size: 0.9rem;
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
</style>