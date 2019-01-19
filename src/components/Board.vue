<template>
  <div>
    <template v-for="i in 3">
      <div class="board-row" :key="i">
        <Square v-for="j in 3"
          :key="3*i + j - 4"
          @square-clicked="squareClicked(3*i + j - 4)"
          :value="3*i + j - 4"
          :content="squares[3*i + j - 4]" />
      </div>
    </template>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue, Emit } from 'vue-property-decorator';
import Square from './Square.vue';

@Component({
  components: {
    Square,
  },
})
export default class Board extends Vue {
  @Prop({ default: Array(9).fill('') }) private squares!: string[];
  private xIsNext = true;

  @Emit('square-clicked')
  private squareClicked(i: number): number {
    return i;
  }
}
</script>
