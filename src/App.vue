<template>
  <Header />
  <Board :key="forceKey" @reset="forceupdate" @endGame="endGame" />
    <transition name="fade">
      <EndGame v-show="isEnded" :result="result" @reset="forceupdate" />
    </transition>
</template>

<script>
import Header from './components/Header.vue';
import Board from './components/Board.vue';
import EndGame from './components/EndGame.vue';

export default {
  name: 'App',
  components: {
    Header,
    Board,
    EndGame
  },
  data() {
    return {
      forceKey: 0,
      result: "",
      isEnded: false
    }
  },
  methods: {
    forceupdate() {
      this.isEnded = false;
      this.forceKey++;
    },
    endGame(result) {
      this.result = result;
      this.isEnded = true;
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Oswald:wght@500&display=swap');

* {
  cursor: none;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  font-family: 'Oswald', sans-serif;
}
img {
  width: 5%;
}
body {
  overflow-y: hidden;
  background-color: #363636;
  -webkit-touch-callout: none;
  -webkit-user-select: none;
   -khtml-user-select: none;
     -moz-user-select: none;
      -ms-user-select: none;
          user-select: none;
  min-width: 600px;
}
.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to {
  opacity: 0;
}
</style>
