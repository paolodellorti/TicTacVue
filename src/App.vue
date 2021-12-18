<template>
  <MyTitle />
  <Board class="myboard" :key="forceKey" @forceUpdate="forceupdate"/>
</template>

<script>
import Board from './components/Board.vue';
import MyTitle from './components/MyTitle.vue'

export default {
  name: 'App',
  components: {
    Board,
    MyTitle
  },
  data() {
    return {
      forceKey: 0
    }
  },
  methods: {
    forceupdate() {
      this.forceKey++;
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
  color: #2c3e50;
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
}
</style>
