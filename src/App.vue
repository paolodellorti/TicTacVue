<template>
<Header />
<Board :key="forceKey" @reset="forceupdate" @endGame="endGame"/>
<EndGame v-if="!isPlaying" :result="result" @reset="forceupdate"/>

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
      isPlaying: true
    }
  },
  methods: {
    forceupdate() {
      this.isPlaying = true;
      this.forceKey++;
    },
    endGame(result) {
      this.result = result;
      this.isPlaying = false;
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Oswald:wght@500&display=swap');

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
</style>
