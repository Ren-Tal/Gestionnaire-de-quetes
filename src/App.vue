
<template>
  <div id="board">
    <Column :column="column" v-for="column in Columns" :key="column.id" />
  </div>
</template>

<style scoped>
#board {
  display: flex;
  gap: 1rem;
  padding: 1rem;
}
</style>

<script>
import Column from './components/Column.vue';
//import Card from './components/Card.vue';
export default {
  components: {
    Column
  },
  data() {
    return {
      Columns : [
        {
          id: 1,
          title: 'To Do',
          quests: [
            { id: 1, title: 'Trouver l\'épée magique', status: 'To Do' },
            { id: 2, title: 'Sauver la princesse', status: 'To Do' }
          ]
        },  
        {
          id: 2,
          title: 'In Progress',
          quests: [
            { id: 3, title: 'Défendre le château', status: 'In Progress' }
          ]
        },
        {
          id: 3,
          title: 'Done',
          quests: [
            { id: 4, title: 'Trouver le trésor caché', status: 'Done' }
          ]
        }
      ]
    }
  }
}
</script>





