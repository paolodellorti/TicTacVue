<template>
<div class="controller">
  <p class="player" :class="{selected: player === 1}">X</p>
  <div>
  <div v-for="x in 3" :key="x" :class="`x${x}`">
    <button class="buttonBoard buttonBoard_hover" v-for="y in 3" :key="y" @click="toggle(x,y)" :class="`y${y}`">{{ showSymbol(x, y) }}</button>
  </div>
  </div>
  <p class="player" :class="{selected: player === -1}">O</p>
</div>

<button class="reset" @click="forceUpdate">RESET</button>


</template>

<script>
import _ from 'lodash';

export default {
  name: 'Board',
  emits: ["forceUpdate"],
  data() {
    return {
      board: [
        [0, 0, 0],
        [0, 0, 0],
        [0, 0, 0]
      ],
      player: 1,
      check: 0
    }
  },
  methods: {
    toggle(x, y) {
      if (this.board[x-1][y-1] === 0 ) {
        event.target.style.color = "#35495E";
        event.target.classList.remove("buttonBoard_hover");
        this.board[x-1][y-1] += this.player;
        this.checkWinner();
        this.player = this.player === 1 ? -1 : 1;
      }
    },
    showSymbol(x, y) {
      if (this.board[x - 1][y - 1] === 1) {
        return "X"
      } else if (this.board[x - 1][y - 1] === -1){
        return "O"
      } else {
        return "."
      }
    },
    checkWinner() {
      //horizontal win
      for (let x = 0; x < 3; x++) {
        this.check = _.sum(this.board[x]);
        if (this.check === 3 || this.check === -3) {
          return this.showWinner();
        }
      }
      this.check = 0;
      //vertical win
      for (let y = 0; y < 3; y++) {
        for (let x = 0; x < 3; x++) {
          this.check += this.board[x][y]
          if (this.check === 3 || this.check === -3) {
            return this.showWinner();
          }
        }
        this.check = 0;
      }
      this.check = 0;
      //diagonal from R to L win
      this.check = this.board[0][2] + this.board[1][1] + this.board[2][0]
      if (this.check === 3 || this.check === -3) {
        return this.showWinner();
      }
      this.check = 0;
      //diagonal from L to R win
      for (let i = 0; i < 3; i++) {
        this.check += this.board[i][i]
        if (this.check === 3 || this.check === -3) {
          return this.showWinner();
        }
      }
      this.check = 0;
    },
    showWinner() {
      this.forceUpdate();
      console.log("hai vinto!!");
    },
    forceUpdate() {
      console.log("e forzati dio cannnn");
      this.$emit("forceUpdate");
    }
  }
}
</script>

<style>
.buttonBoard {
  font-family: 'Oswald', sans-serif;
  width: 150px;
  height: 150px;
  margin: 0;
  padding: 0;
  font-size: 5rem;
  color: transparent;
  border: 2px solid #35495E;
  background-color: #41B883;
}
.buttonBoard_hover:hover,
.reset:hover {
  background-color: rgba(65,184,131,0.7);
}
.reset {
  font-family: 'Oswald', sans-serif;
  min-width: 10%;
  background-color: #41B883;
  color: #35495E;
  border-radius: 25px;
  font-size: 1.2em;
  padding: 5px;
  border: transparent;
}
.player {
  font-size: 3em;
  width: 10%;
  border-radius: 25px;
  color: #41B883;
  background-color:#35495E;
}
.selected {
  background-color: #41B883;
  color: #35495E;
}
.controller {
  display: flex;
  justify-content: space-around;
  align-items: center;
  margin: 30px auto;
}

/* style personalizzato per ogni cella */
.x1 > .y1 {
  border-top: transparent;
  border-left: transparent;
}
.x1 > .y2 {
  border-top: transparent;
}
.x1 > .y3 {
  border-top: transparent;
  border-right: transparent;
}
.x2 > .y1 {
  border-left: transparent;
}
.x2 > .y3 {
  border-right: transparent;
}
.x3 > .y1 {
  border-bottom: transparent;
  border-left: transparent;
}
.x3 > .y2 {
  border-bottom: transparent;
}
.x3 > .y3 {
  border-bottom: transparent;
  border-right: transparent;
}
</style>
