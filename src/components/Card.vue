
<template>
  <div class="card">
    <h2>{{ quest.title }}</h2>
    <h3>Status: {{ quest.status }}</h3>
  </div>  
</template>

<script>

export default {
  props: ['quest']
}
</script>

<style scoped>
.card {
  background: #f4e4c1;
  border: 2px solid #c9922a;
  padding: 1rem;
  gap: 1rem;
  border-radius: 4px;

  max-width: 300px;
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
</style>




