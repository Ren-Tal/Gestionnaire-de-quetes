
<template>
  <div class="App">
    <Board />
  </div>
</template>

<style>
* {
  margin: 0 0 0 0;
  padding: 0;
  box-sizing: border-box;
}

body, html {
  min-height: 100vh;
}

.App {
  min-height: 100vh;
  background-image: 
    linear-gradient(
      to bottom,
      transparent 50%,
      rgba(20, 10, 5, 0.9) 75%,
      rgb(20, 10, 5) 100%
    ),
    url('/background.jpg');
  background-size: cover;
  background-position: center top;
  background-attachment: fixed;
  background-color: rgb(20, 10, 5); /* ← le reste de la page sera brun foncé */
}
</style>

<script>
import Board from './components/Board.vue';

export default {
  components: {
    Board
  },
}
</script>





