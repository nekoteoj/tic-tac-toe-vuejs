<template>
  <div class="game">
    <div class="game-board">
      <Board />
    </div>
    <div class="game-info">
      <div>{/* status */}</div>
      <ol>{/* TODO */}</ol>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import Board from './Board.vue';

@Component({
  components: {
    Board,
  },
})
export default class Game extends Vue {}
</script>

