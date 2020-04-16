<template>
  <div id="app">
    <div>
      Current player: {{currentPlayer}}
    </div>
    <div align="center">
      <board :rows="rows" @selectColumn="playTurn" />
    </div>
  </div>
</template>

<script>
import Board from './components/Board.vue'

const RED_PLAYER = 'red'
const BLUE_PLAYER = 'blue'

function isWinner(board, color) {
  let won = false
  let rows = board.length
  let cols = board[0].length

  for(let rowI = 0; rowI < rows; rowI++) {
    for(let colI = 0; colI < cols; colI++) {
      if(board[rowI][colI] != color) continue;

      if(traverse(board, rowI, colI, color, 't', 1)) return true
      if(traverse(board, rowI, colI, color, 'l', 1)) return true
      if(traverse(board, rowI, colI, color, 'r', 1)) return true
      if(traverse(board, rowI, colI, color, 'b', 1)) return true
      if(traverse(board, rowI, colI, color, 'tr', 1)) return true
      if(traverse(board, rowI, colI, color, 'tl', 1)) return true
      if(traverse(board, rowI, colI, color, 'br', 1)) return true
      if(traverse(board, rowI, colI, color, 'bl', 1)) return true
    }
  }

  return false;
}

function traverse(board, rowI, colI, color, dir, count) {
  if(count >= 4) return true

  let newRowI = rowI
  let newColI = colI

  switch(dir) {
    case 't': 
      newRowI = newRowI - 1
      newColI = newColI
      break;
    case 'l': 
      newRowI = newRowI
      newColI = newColI - 1
      break;
    case 'r': 
      newRowI = newRowI
      newColI = newColI + 1
      break;
    case 'b': 
      newRowI = newRowI + 1
      newColI = newColI
      break;
    case 'tr': 
      newRowI = newRowI + 1
      newColI = newColI + 1
      break;
    case 'tl': 
      newRowI = newRowI +1
      newColI = newColI -1
      break;
    case 'br': 
      newRowI = newRowI +1
      newColI = newColI +1
      break;
    case 'bl': 
      newRowI = newRowI +1
      newColI = newColI -1
      break;
  }

  if(!board[newRowI]) return false
  if(!board[newRowI][newColI]) return false
  if(board[newRowI][newColI] != color) return false

  return traverse(board, newRowI, newColI, color, dir, count+1)
}

function inject(board, col, color) {
  let free = true

  for(let rowNum = 0; rowNum <= board.length; rowNum ++ ) {
    if(board[rowNum][col] != 0) {
      // continue;
    } else if(!board[rowNum+1] || board[rowNum+1][col] != 0) {
      board[rowNum][col] = color
      break;
    }
  }

  return Array.from(board)
}

export default {
  name: 'App',
  components: {
    Board
  },

  data () {
    return {
      currentPlayer: RED_PLAYER,
      rows: [
        [0,0,0,0,0,0],
        [0,0,0,0,0,0],
        [0,0,0,0,0,0],
        [0,0,0,0,0,0],
        [0,0,0,0,0,0],
        [0,0,0,0,0,0],
        [0,0,0,0,0,0]
      ]
    }
  },

  methods: {
    playTurn(col) {
      let newBoard = inject(this.rows, col, this.currentPlayer)
      this.rows = newBoard

      console.log("Winner", this.currentPlayer, isWinner(this.rows, this.currentPlayer))
      if(this.currentPlayer == RED_PLAYER) {
        this.currentPlayer = BLUE_PLAYER
        } else {
        this.currentPlayer = RED_PLAYER
      }
    }
  }


}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
