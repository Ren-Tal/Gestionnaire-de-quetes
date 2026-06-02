
<template>
  <div class="card">
    <div v-if="!isEditing"> 
      <h2>{{ quest.title }}</h2>
      <h3>{{ quest.description }}</h3>
      <button @click="startEdit" id="edit-btn">Edit</button>
      <button @click="deleteCard" id="delete-btn" v-if="quest.status !== 'Terminé'" >X</button>
    </div>
    <div v-else class="edit-form">
      <input v-model="editedQuest.title" :placeholder="quest.title" />
      <input v-model="editedQuest.description" :placeholder="quest.description" />
      <button @click="saveEdit" id="save-btn">Save</button>
      <button @click="cancelEdit" id="cancel-btn">Cancel</button>
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
      editedQuest: { ...this.quest }
    }
  },
  methods: {
    cancelEdit() {
      this.isEditing = false;
      this.editedQuest = { ...this.quest };
    },
    startEdit() {
      this.editedQuest = { ...this.quest };
      this.isEditing = true;
    },
    deleteCard() {
      this.$emit('delete', this.quest.id);
    },
    saveEdit() {
      this.$emit('update', { updatedQuest: { ...this.editedQuest} , questId: this.quest.id });
      this.isEditing = false;
    }
  }
}
</script>

<style scoped>
.card {
  background: #f4e4c1;
  border: 2px solid #c9922a;
  padding: 1rem;
  gap: 1rem;
  border-radius: 4px;

  width: 90%;
  word-break: break-word;
}

.card h2 {
  color: #4c2103;
  margin: 0;
  font-size: 1.5rem;
}
.card h3 {
  color: #664913;
  margin: 0.5rem 0;
  font-size: 1.2rem;
}



#delete-btn {
  background: #c9922a;
  color: white;
  border: none;
  display:block;
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
  font: bold 1.2rem 'Arial', sans-serif;
  margin: 0;
}

.edit-form input, .edit-form select {
  padding: 0.4rem;
  border: 1px solid #c9922a;
  border-radius: 4px;
  background: #f4e4c1;
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



</style>




