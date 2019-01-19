<template>
  <button class="square" @click="squareClicked">
    {{this.content}}
  </button>
</template>

<script lang="ts">
import { Component, Prop, Vue, Emit } from 'vue-property-decorator';

@Component
export default class Square extends Vue {
  @Prop({ default: '' }) private value!: number;
  @Prop({ default: '' }) private content!: string;

  @Emit()
  private squareClicked(): number {
    return Number(this.value);
  }
}
</script>

