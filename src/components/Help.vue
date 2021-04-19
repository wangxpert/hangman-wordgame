<template>
  <div>
    <h4>Let's guess the name of a country.</h4>
    <div>
      <strong>Keyboard operation: </strong>
      A-Z: Enter letter, ESC: Restart
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';

@Component({ name: 'Help' })
export default class Help extends Vue {}
</script>

<style scoped>
  h4 {
    margin: 0;
  }
</style>
