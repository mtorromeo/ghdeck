<template>
  <div id="app">
    <div class="toolbar">
      <button :disabled="!canShuffle" type="button" @click="shuffle">Shuffle</button>
      <button type="button" :disabled="curseCount >= 10" @click="add('curse')">+Curse ({{ curseCount }})</button>
      <button type="button" :disabled="blessCount >= 10" @click="add('bless')">+Bless ({{ blessCount }})</button>
    </div>

    <div class="decks">
      <Card :style="{visibility: available.length ? '' : 'hidden'}" :counter="available.length" @click="pick"/>
      <Card v-if="lastPicked" :modifier="lastPicked"/>
    </div>
  </div>
</template>

<script>
import Card from './components/Card.vue';

function shuffle(a) {
  for (let i = a.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [a[i], a[j]] = [a[j], a[i]];
  }
  return a;
}

export default {
  name: 'app',

  components: {
    Card,
  },

  data() {
    return {
      available: [
        '0',
        '0',
        '0',
        '0',
        '0',
        '0',
        '+1',
        '+1',
        '+1',
        '+1',
        '+1',
        '-1',
        '-1',
        '-1',
        '-1',
        '-1',
        '-2',
        '+2',
        '2×',
        '∅',
      ],
      discarded: [],
    };
  },

  computed: {
    lastPicked() {
      return this.discarded.length ? this.discarded[this.discarded.length - 1] : '';
    },

    canShuffle() {
      return this.discarded.some(e => ['∅', '2×'].includes(e));
    },

    blessCount() {
      return this.count('bless');
    },

    curseCount() {
      return this.count('curse');
    },
  },

  mounted() {
    this.shuffle();
  },

  methods: {
    shuffle() {
      for (const card of this.discarded) {
        if (!['curse', 'bless'].includes(card)) {
          this.available.push(card);
        }
      }
      this.discarded = [];
      this.available = shuffle(this.available);
    },

    pick() {
      if (!this.available.length) {
        this.shuffle();
      }

      this.discarded.push(this.available.pop());
    },

    add(card) {
      this.available.push(card);
      this.available = shuffle(this.available);
    },

    count(card) {
      return this.available.reduce((acc, cur) => acc + (cur === card ? 1 : 0), 0);
    },
  },
};
</script>

<style>
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  background-color: black;
  color: white;
}

#app {
  display: flex;
  flex-direction: column;

  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

.toolbar {
  height: 10vh;
  display: flex;
  flex-direction: row;
  flex: 1 1 auto;
}

.toolbar button {
  height: 10vh;
  line-height: 10vh;
  font-size: 3vh;
  padding: 0 .5em;
  flex: 1 1 auto;
}

.decks {
  display: flex;
  flex-direction: column;
  padding: 10px;
}
</style>
