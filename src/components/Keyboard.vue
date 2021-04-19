<template>
  <div id="buttons">
    <div v-for="row in keys" :key="row">
      <button
        class="btn"
        v-for="key in row"
        :key="key"
        :disabled="usedLetters.includes(key)"
        @click="click(key, $event)"
      >
        {{key}}
      </button>
    </div>
    <button class="btn btn-restart" @click="restart">Restart</button>
  </div>
</template>

<script lang="ts">
import { Component, Vue, Prop } from 'vue-property-decorator';

@Component({ name: 'Keyboard' })
export default class Keyboard extends Vue {
  @Prop({ required: true }) usedLetters!: string;

  keys = [
    'QWERTYUIOP',
    'ASDFGHJKL',
    'ZXCVBNM',
  ];

  click(key: string, evt: { target: HTMLElement }) {
    this.$emit('key', key);
    evt.target.blur();
  }

  restart(evt: { target: HTMLElement }) {
    this.$emit('restart');
    evt.target.blur();
  }
}
</script>

<style scoped>
.btn {
  font-size: 1rem;
  width: 2rem;
  height: 2rem;
  font-weight: bold;
  margin: 2px;
}

.btn-restart {
  width: 6rem;
  margin-top: 5px;
}
</style>
