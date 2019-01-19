<template>
  <div class="game">
    <div class="game-board">
      <Board :squares="current.squares" @square-clicked="handleClick" />
    </div>
    <div class="game-info">
      <div>{{status}}</div>
      <ol>
        <li v-for="(desc, move) in moves" :key="move">
          <button @click="jumpTo(move)">{{desc}}</button>
        </li>
      </ol>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import Board from './Board.vue';
import { Squares } from '@/types';

@Component({
  components: {
    Board,
  },
})
export default class Game extends Vue {
  private xIsNext = true;
  private history: Squares[] = [{
    squares: Array(9).fill(''),
  }];
  private stepNumber = 0;

  private get status(): string {
    if (this.winner) {
      return 'Winner: ' + this.winner;
    }
    return 'Next player: ' + (this.xIsNext ? 'X' : 'O');
  }


  private get current(): Squares {
    return this.history[this.stepNumber];
  }

  private get winner(): string | null {
    return this.calculateWinner(this.current.squares);
  }

  private get moves(): string[] {
    return this.history.map((step, move) => {
      return move ?
        'Go to move #' + move :
        'Go to game start';
    });
  }

  private handleClick(i: number): void {
    this.history = this.history.slice(0, this.stepNumber + 1);
    const squares = this.current.squares.slice();
    if (this.calculateWinner(squares) || squares[i]) {
      return;
    }
    squares[i] = this.xIsNext ? 'X' : 'O';
    this.history.push({ squares });
    this.xIsNext = !this.xIsNext;
    this.stepNumber = this.history.length - 1;
  }

  private jumpTo(step: number): void {
    this.stepNumber = step;
    this.xIsNext = (step % 2) === 0;
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

