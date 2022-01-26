<template>
  <div class="flex column">
    <div class="game-stats text-white">
      currPlayer: {{ currentPlayer.value }} elims1:{{ player1Elims }} elims2:{{
        player2Elims
      }}
      <br />
      pos1: {{ pos1.value }} pos2: {{ pos2.value }}
    </div>
    <div class="grid-container">
      <div class="cell" v-for="i in 49" :key="i">
        <Cell :cellNum="i" :pos1="pos1" :pos2="pos2"></Cell>
      </div>
    </div>
    <div class="text-center">Last Drawn Num: {{ lastDrawnNum }}</div>
    <div class="flex flex-center justify-content" style="margin: 10px">
      <q-btn :disable="currentPlayer != 1" label="player1 move" color="primary" @click="move(1)" />
      <q-btn :disable="currentPlayer != 2" label="player2 move" color="primary" @click="move(2)" />
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import Cell from './Cell.vue'
const kattaArray = ref([4, 9, 13, 22, 25, 28, 37, 41, 46])
const player1Path = ref([
  46, 47, 48, 49, 42, 35, 28, 21, 14, 7, 6, 5, 4, 3, 2, 1, 8, 15, 22, 29,
  36, 43, 44, 45, 37, 30, 23, 16, 9, 10, 11, 12, 13, 20, 27, 34, 41, 40,
  39, 38, 31, 24, 17, 18, 19, 26, 33, 32, 25,
]);
const player2Path = ref([
  4, 3, 2, 1, 8, 15, 22, 29, 36, 43, 44, 45, 46, 47, 48, 49, 42, 35, 28,
  21, 14, 7, 6, 5, 13, 20, 27, 34, 41, 40, 39, 38, 37, 30, 23, 16, 9, 10,
  11, 12, 19, 26, 33, 32, 31, 24, 17, 18, 25,
])
const turnNum = ref(0)
const pos1 = ref(0)
const pos2 = ref(0)
const player1Elims = ref(0)
const player2Elims = ref(0)
const lastDrawnNum = ref(0)
const currentPlayer = ref(1)
const totalPlayers = ref(2)
const move = (player) => {
  //incr turn num
  lastDrawnNum.value = Math.ceil(Math.random() * 6);
  if (player == 1) {
    //return if player has elims before moving inner grid or if player pos > grid lenght
    if (
      (player1Elims.value == 0 && pos1.value + lastDrawnNum.value > 23) ||
      pos1.value + lastDrawnNum.value > 48
    )
      return;
    pos1.value += lastDrawnNum.value;
  } else {
    if (
      (player2Elims.value == 0 && pos2.value + lastDrawnNum.value > 23) ||
      pos2.value + lastDrawnNum.value > 48
    )
      return;
    pos2.value += lastDrawnNum.value;
  }
  checkElims(player);
  checkWin(player);
  if (lastDrawnNum.value != 6) moveTurn();
  turnNum.value++;
};
const checkElims = (player) => {
  if (player == 1) {
    if (player1Path.value[pos1.value] == player2Path.value[pos2.value]) {
      player1Elims.value++;
      pos2.value = 0;
      $q.value.notify({
        message: `player 1 eliminated player 2!`,
        color: `info`,
      });
    }
  } else {
    if (player1Path.value[pos1.value] == player2Path.value[pos2.value]) {
      pos1.value = 0;
      player2Elims.value++;
      $q.value.notify({
        message: `player 2 eliminated player 1!`,
        color: `info`,
      });
    }
  }
};
const checkWin = (player) => {
  if (player == 1) {
    if (pos1.value >= 48)
      $q.value.notify({
        message: `Player 1 won the game!`,
        color: `positive`,
      });
  } else {
    if (pos2.value >= 48)
      $q.value.notify({
        message: `Player 2 won the game!`,
        color: `positive`,
      });
  }
};
const moveTurn = () => {
  currentPlayer.value++;
  if (currentPlayer.value > totalPlayers.value) currentPlayer.value = 1;
};

</script>

<style>
.grid-container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr 1fr 1fr 1fr;
  grid-template-rows: 1fr 1fr 1fr 1fr 1fr 1fr 1fr;
  /* gap: 1px; */
}
/* .cell {
  height: 70px;
  width: 70px;
  text-align: center;
  padding: 10px 20px;
  border: 1px solid white;
  color: white;
} */
</style>
