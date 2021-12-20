<template>
<div class="controller">
  <p class="player" :class="{selected: player === 1}">X</p>
  <div>
  <div v-for="x in 3" :key="x" :class="`x${x}`">
    <button class="buttonBoard buttonBoard_hover" v-for="y in 3" :key="y" @click="toggle(x,y)" :class="`y${y}`">{{ showPlayers(x, y) }}</button>
  </div>
  </div>
  <p class="player" :class="{selected: player === -1}">O</p>
</div>

<button v-show="isPlaying" class="reset" @click="$emit('reset')">RESTART</button>

</template>

<script>
import _ from 'lodash';

export default {
  name: 'Board',
  emits: [
    "reset", 
    "endGame", 
    "changePlayer"
  ],
  data() {
    return {
      board: [
        [0, 0, 0],
        [0, 0, 0],
        [0, 0, 0]
      ],
      player: 1,
      check: 0,
      moves: 0,
      isPlaying: true
    }
  },
  methods: {
    toggle(x, y) {
      if (this.board[x-1][y-1] === 0 ) {
        event.target.style.color = "#363636";
        event.target.classList.remove("buttonBoard_hover");
        this.board[x-1][y-1] += this.player;
        this.moves++;
        if (this.moves > 4) {
          this.checkWinner(this.moves);
        }
        this.player = this.player === 1 ? -1 : 1;
      }
    },
    showPlayers(x, y) {
      if (this.board[x - 1][y - 1] === 1) {
        return "X";
      } else if (this.board[x - 1][y - 1] === -1){
        return "O";
      } else {
        return ".";
      }
    },
    checkWinner(moves) {
      //horizontal win
      for (let x = 0; x < 3; x++) {
        this.check = _.sum(this.board[x]);
        if (this.check === 3 || this.check === -3) {
          return this.showWinner(this.check);
        }
      }
      this.check = 0;

      //vertical win
      for (let y = 0; y < 3; y++) {
        for (let x = 0; x < 3; x++) {
          this.check += this.board[x][y]
          if (this.check === 3 || this.check === -3) {
            return this.showWinner(this.check);
          }
        }
        this.check = 0;
      }

      //diagonal from R to L win
      for(let i = 0, z = 2; i < 3; i++, z--){
        this.check += this.board[i][z];
        if (this.check === 3 || this.check === -3) {
          return this.showWinner(this.check);
        }
      }
      this.check = 0;

      //diagonal from L to R win
      for (let i = 0; i < 3; i++) {
        this.check += this.board[i][i]
        if (this.check === 3 || this.check === -3) {
          return this.showWinner(this.check);
        }
      }
      this.check = 0;

      //if last move, end with a draw
      if (moves === 9) {
        return this.showWinner(0);
      }
      this.$emit("changePlayer", this.player);
    },
    showWinner(result) {
      this.isPlaying = false;
      if (result === 3) {
        result = "X";
      } else if (result === -3) {
        result = "O";
      } else {
        result = 0;
      }
      this.$emit("changePlayer", 0)
      this.$emit("endGame", result);
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
  font-size: 6rem;
  color: transparent;
  border: 4px solid #363636;
  background-color: #41B883;
}
.buttonBoard_hover:hover,
.reset:hover {
  background-color: #369b6e;
}
.reset {
  font-family: 'Oswald', sans-serif;
  min-width: 10%;
  background-color: #41B883;
  color: #363636;
  border-radius: 25px;
  font-size: 1.2em;
  padding: 10px;
  border: transparent;
}
.player {
  font-size: 3em;
  width: 10%;
  border-radius: 25px;
  color: #363636;
  background-color:#35495E;
}
.selected {
  background-color: #41B883;
  color: #363636;
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
  border-top-left-radius: 45px;
}
.x1 > .y2 {
  border-top: transparent;
}
.x1 > .y3 {
  border-top: transparent;
  border-right: transparent;
  border-top-right-radius: 45px;
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
  border-bottom-left-radius: 45px;
}
.x3 > .y2 {
  border-bottom: transparent;
}
.x3 > .y3 {
  border-bottom: transparent;
  border-right: transparent;
  border-bottom-right-radius: 45px;
}
</style>
