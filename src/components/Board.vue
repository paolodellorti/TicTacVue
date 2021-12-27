<template>
  <div class="controller">
    <p class="player" :class="{selected: player === 1}">X</p>
    <div>
    <div v-for="x in 3" :key="x" :class="`x${x}`">
      <button class="buttonBoard buttonBoard_hover" v-for="y in 3" :key="y" @click="toggle(x,y)" :class="`y${y}`">
        {{ showPlayers(x, y) }}
        <span @mouseover="showPreview(x, y)" @mouseout="deletePreview(x, y)"> {{ preview }}  </span>
      </button>
    </div>
    </div>
    <p class="player" :class="{selected: player === -1}">O</p>
  </div>

  <button v-show="isPlaying" class="reset" @click="$emit('reset')">RESTART</button>

  <div class="cursors">
    {{ playerComputed }}
  </div>

</template>

<script>
import _ from 'lodash';

export default {
  name: 'Board',
  emits: [
    "reset", 
    "endGame" 
  ],
  data() {
    return {
      board: [
        [0, 0, 0],
        [0, 0, 0],
        [0, 0, 0]
      ],
      player: 1,
      moves: 0,
      isPlaying: true,
      preview: ""
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
        this.player = this.player === -1 ? 1 : -1;
      }
    },
    showPlayers(x, y) {
      let box = this.board[x - 1][y - 1];
      return box === 1 ? "X"
           : box === -1 ? "O"
           : "."
    },
    showPreview(x, y) {
      if (this.board[x - 1][y - 1] === 0) {
        return this.preview === 1 ? "X": "O"    
      }  
    },
    checkWinner(moves) {
      //horizontal win
      for (let x = 0, check = 0; x < 3; x++) {
        check = _.sum(this.board[x]);
        if (check === 3 || check === -3) {
          return this.showWinner(check);
        }
      }

      //vertical win
      for (let y = 0; y < 3; y++) {
        for (let x = 0, check = 0; x < 3; x++) {
          check += this.board[x][y]
          if (check === 3 || check === -3) {
            return this.showWinner(check);
          }
        }
      }

      //diagonal from R to L win
      for(let i = 0, z = 2, check = 0; i < 3; i++, z--){
        check += this.board[i][z];
        if (check === 3 || check === -3) {
          return this.showWinner(check);
        }
      }

      //diagonal from L to R win
      for (let i = 0, check = 0; i < 3; i++) {
        check += this.board[i][i]
        if (check === 3 || check === -3) {
          return this.showWinner(check);
        }
      }

      //if last move, end with a draw
      if (moves === 9) {
        return this.showWinner(0);
      }
    },
    showWinner(result) {
      this.isPlaying = false;
      result = result === 3 ? "X"
             : result === -3 ? "O"
             : 0;
      this.$emit("endGame", result);
    }
  },
  mounted() {
    let cursor = document.querySelector(".cursors");
    document.addEventListener("mousemove", function(event) {
      cursor.style.left = event.pageX + "px";
      cursor.style.top = event.pageY + "px";
    })
  },
  computed: {
    playerComputed() {
      return this.player === 1 ? "X" 
           : this.player === -1 ? "O" 
           : "."
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
  transition: all 0.3s;
}
.buttonBoard_hover:hover,
.reset:hover {
  background-color: #369b6e;
  transition: all 0.3s;
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
  transition: all 0.3s;
}
.player {
  font-size: 3em;
  width: 10%;
  border-radius: 25px;
  color: #363636;
  background-color:#35495E;
  transition: all 0.3s;
}
.selected {
  background-color: #41B883;
  transition: all 0.3s;
}
.controller {
  display: flex;
  justify-content: space-around;
  align-items: center;
  margin: 30px auto;
}
.cursors {
  position: absolute;
  z-index: 100;
  pointer-events: none;
  transform: translate(-50%, -50%);
  opacity: 0.7;
  font-size: 6rem;
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
