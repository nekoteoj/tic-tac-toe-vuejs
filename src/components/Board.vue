<template>
  <div>
    <div class="status">{{status}}</div>
    <template v-for="i in 3">
      <div class="board-row" :key="i">
        <Square v-for="j in 3"
          :key="3*i + j - 4"
          @square-clicked="handleClick(3*i + j - 4)"
          :value="3*i + j - 4"
          :content="squares[3*i + j - 4]" />
      </div>
    </template>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import Square from './Square.vue';

@Component({
  components: {
    Square,
  },
})
export default class Board extends Vue {
  private squares = Array(9).fill('');
  private xIsNext = true;

  private get status(): string {
    const winner = this.calculateWinner(this.squares.slice());
    if (winner) {
      return 'Winner: ' + winner;
    }
    return 'Next player: ' + (this.xIsNext ? 'X' : 'O');
  }

  private handleClick(i: number): void {
    const sqaures = this.squares.slice();
    sqaures[i] = this.xIsNext ? 'X' : 'O';
    this.squares = sqaures;
    this.xIsNext = !this.xIsNext;
  }

  private calculateWinner(squares: string[]): string | null {
    const lines = [
      [0, 1, 2],
      [3, 4, 5],
      [6, 7, 8],
      [0, 3, 6],
      [1, 4, 7],
      [2, 5, 8],
      [0, 4, 8],
      [2, 4, 6],
    ];
    for (const line of lines) {
      const [a, b, c] = line;
      if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
        return squares[a];
      }
    }
    return null;
  }
}
</script>
