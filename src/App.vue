<template>
  <div class="App" :class="{ dark: isDark }">
    <h1>Board of Quests</h1>
    <button @click="isDark = !isDark" class="theme-btn">
      <img :src="isDark ? '/light_mode.png' : '/dark_mode.png'" alt="Toggle Dark Mode" />
    </button>
    <Board />
  </div>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body, html {
  min-height: 100vh;
}

@media (max-width: 768px) {
  .App h1 {
    font-size: 2rem;
    padding-top: 0.5rem;
  }
}

.App {
  position: relative;
  min-height: 100vh;
  background-image: none;
  font-family: 'Berenika', serif;
}

.App::before {
  content: '';
  position: fixed;
  inset: 0;
  background-image:
    linear-gradient(to bottom, transparent 50%, rgba(20, 10, 5, 0.9) 75%, rgb(20, 10, 5) 100%),
    url('/background.jpg');
  background-size: cover;
  background-position: center top;
  background-color: rgb(20, 10, 5);
  z-index: -1;          /* ← derrière tout le contenu */
  transition: filter 0.5s ease;
}

.App.dark::before {
  filter: hue-rotate(180deg) brightness(0.8);
}

/* Mode sombre — pas de filter ici ! */

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
  transition: color 0.5s ease;
}

.dark h1 {
  color: #514e7e;
  text-shadow: 0 0 15px rgba(160, 100, 200, 0.6), 2px 2px 4px rgba(0, 0, 0, 0.9);
}

.theme-btn {
  position: fixed;
  bottom: 2rem;
  left: 2rem;
  width: 60px;
  height: 60px;
  border-radius: 50%;
  background: #c9922a;
  border: 2px solid #2e1707;
  cursor: pointer;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.5);
  z-index: 1000;        /* ← z-index élevé */
  filter: none;         /* ← pas de filtre */
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background 0.3s ease, transform 0.2s ease;
}

.theme-btn:hover {
  background: #a87820;
  transform: scale(1.1);
}

.theme-btn img {
  width: 30px;
  height: 30px;
}

.dark .dark .btn-float img {
  filter: hue-rotate(180deg) brightness(1.2);
}

.dark .theme-btn, .dark .btn-float {
  background: #514e7e;
  border-color: #422e07;
}

/* Mode sombre — filter uniquement sur les éléments du board */
.dark .board {
  filter: brightness(0.8) hue-rotate(180deg);
}

.dark .card {
  filter: saturate(0.5) hue-rotate(20deg);
}

.dark .column {
  filter: brightness(0.95);
}

.dark .column h2 {
  color: #b1b66f;
  text-shadow: 0 0 10px rgba(198, 200, 100, 0.5), 1px 1px 3px rgba(0, 0, 0, 0.7);
}

.dark .search-field {
  background: rgba(50, 50, 50, 0.8);
  color: #ccc;
  filter: hue-rotate(180deg) brightness(0.8);
}

.dark .search-bar {
  background: rgba(50, 50, 50, 0.8);
  color: #ccc;
  filter: hue-rotate(180deg) brightness(0.8);
}

.dark .stats {
  color: #929956;
  filter: hue-rotate(180deg) brightness(0.8);
}

/* Boutons fixed — jamais affectés par les filtres */
.btn-float {
  filter: none !important;
  z-index: 1000 !important;
}
</style>

<script>
import Board from './components/Board.vue'

export default {
  components: {
    Board,
  },
  data() {
    return {
      isDark: false
    }
  }
}
</script>
