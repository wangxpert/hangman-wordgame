<template>
  <div id="app">
    <help />
    <br />
    <stick-man :fails="fails" />
    <letters :letters="guessedWord" />
    <br />
    <keyboard :usedLetters="usedLetters" @key="processLetter" @restart="restartGame" />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import Help from './components/Help.vue';
import StickMan from './components/StickMan.vue';
import Letters from './components/Letters.vue';
import Keyboard from './components/Keyboard.vue';

import { MAX_ATTEMPT_COUNT } from './constants';
import json from './assets/words_country.json';

@Component({
  components: {
    Help,
    StickMan,
    Letters,
    Keyboard,
  },
})
export default class App extends Vue {
  fails = 0;

  guessedWord = '';

  secretWord = '';

  foundLetters = '';

  usedLetters = '';

  words = json.words;

  startGame() {
    const min = 0;
    const max = this.words.length - 1;
    const index = Math.floor(Math.random() * (max - min) + min);
    this.secretWord = this.words[index].toUpperCase();
    this.guessedWord = '';
    this.foundLetters = '';
    this.usedLetters = '';
    this.fails = 0;
    this.guessedWord = this.secretWord.split('').map(() => '_').join('');
  }

  onKeyPressed(evt: KeyboardEvent) {
    const key = evt.key.toUpperCase();
    this.processLetter(key);
  }

  restartGame() {
    if (window.confirm('Are you going to restart the game?')) {
      this.startGame();
    }
  }

  processLetter(letter: string) {
    if (
      this.fails >= 0 && this.fails < MAX_ATTEMPT_COUNT && !this.usedLetters.includes(letter) && letter >= 'A' && letter <= 'Z') {
      if (this.secretWord.includes(letter)
            && !this.foundLetters.includes(letter)) {
        this.foundLetters += letter;
        this.guessedWord = this.secretWord.split('')
          .map((l) => (this.foundLetters.includes(l) ? l : '_')).join('');
        if (this.guessedWord === this.secretWord) {
          this.fails = -1;
        }
      } else {
        this.fails += 1;
        if (this.fails === MAX_ATTEMPT_COUNT) {
          this.guessedWord = this.secretWord;
        }
      }
      this.usedLetters += letter;
    }
    if (letter === 'ESCAPE') {
      this.restartGame();
    }
  }

  mounted() {
    window.addEventListener('keypress', this.onKeyPressed);
    this.startGame();
  }

  unmounted() {
    window.removeEventListener('keypress', this.onKeyPressed);
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
