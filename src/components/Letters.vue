<template>
  <div>
    <button v-for="(item, index) in letters" :key="`${index}`">
      {{item}}
    </button>
  </div>

</template>

<script lang="ts">
import { Component, Vue, Prop } from 'vue-property-decorator';

@Component({ name: 'Letters' })
export default class Letters extends Vue {
  @Prop({ required: true }) letters!: string;
}
</script>

<style scoped>
button {
  font-size: 1rem;
  width: 2rem;
  height: 2rem;
  margin: 2px;
  pointer-events: none;
}
</style>
