<style scoped></style>

<template>
    <div class="Column">
        <h2>{{ column.title }}</h2>
        <Card :quest="quest" v-for="quest in column.quests" :key="quest.id" />
    </div>
</template>

<style scoped>
.column {
  background: #e8d5a3;
  border: 2px solid #c9922a;
  padding: 1rem;
  border-radius: 4px;
  min-width: 250px;
}

.column h2 {
  color: #2e1707;
  margin: 0 0 1rem;
  font-size: 1.5rem;
  font: bold 1.5rem 'Arial', sans-serif;
}
</style>

<script>
import Card from './Card.vue'
export default {
    props: ['column'],
    components: {
        Card
    },
}
</script>





