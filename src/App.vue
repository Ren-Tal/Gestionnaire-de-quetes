<template>
  <div class="App">
    <h1>Board of Quests</h1>
    <Board />
  </div>
</template>

<style>
* {
  margin: 0 0 0 0;
  padding: 0;
  box-sizing: border-box;
}

body,
html {
  min-height: 100vh;
}

@media (max-width: 768px) {
  .App h1 {
    font-size: 2rem;
    padding-top: 0.5rem;
  }
}

.App {
  min-height: 100vh;
  background-image:
    linear-gradient(to bottom, transparent 50%, rgba(20, 10, 5, 0.9) 75%, rgb(20, 10, 5) 100%),
    url('/background.jpg');
  background-size: cover;
  background-position: center top;
  background-attachment: fixed;
  background-color: rgb(20, 10, 5);
  font-family: 'Berenika', serif;
}

@font-face {
  font-family: 'Berenika';
  src: url('/fonts/Berenika-Book.ttf');
}

@font-face {
  font-family: 'Berenika';
  src: url('/fonts/Berenika-Bold.ttf');
  font-weight: bold;
}

@font-face {
  font-family: 'Berenika';
  src: url('/fonts/Berenika-BookOblique.ttf');
  font-style: italic;
}

.App h1 {
  text-align: center;
  color: #c29742;
  font-size: 4rem;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.8);
}
</style>

<script>
import Board from './components/Board.vue'

export default {
  components: {
    Board,
  },
}
</script>
