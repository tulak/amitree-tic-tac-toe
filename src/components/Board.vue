<template>
  <div class="board">
    <div v-for="(row, index) in rows" class="row" :key="index">
      <div v-for="(col, colIndex) in row" class="col" :key="colIndex" @click="$emit('selectColumn', colIndex)">
        {{col}}
      </div>
    </div>
  </div>
</template>

<script>


export default {
  props: ['rows']
}
</script>

<style scoped>
.board {
  width: 300px;
  border: 1px black solid;
  display: flex;
  flex-direction: column;
}

.row {
  border: 1px black solid;
  display: flex;
  flex-direction: row;
}

.col {
  border: 1px red solid;
  width: 50px;
  height: 50px;
}

.col.red {
  background-color: red;
}

.col.blue {
  background-color: blue;
}
</style>
