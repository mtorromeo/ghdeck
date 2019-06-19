<template>
  <div
    class="card"
    :class="{
      'card--hidden': !this.modifier,
      'card--miss': ['∅', 'curse'].includes(this.modifier),
      'card--critical': ['2×', 'bless'].includes(this.modifier),
      'card--curse': this.modifier == 'curse',
      'card--bless': this.modifier == 'bless',
      'card--positive': this.modifier && this.modifier[0] == '+',
      'card--negative': this.modifier && this.modifier[0] == '-',
      'card--shuffle': ['∅', '2×'].includes(this.modifier),
    }"
    :tabindex="modifier ? '0' : modifier"
    @click="$emit('click')"
  >
    <div v-if="modifier" class="card__modifier">{{ modifierText }}</div>
    <div v-if="counter" class="card__counter">{{ counter }}</div>
  </div>
</template>

<script>
export default {
  name: 'Card',

  props: {
    modifier: {
      type: String,
      validator: m => ['', '-2', '-1', '0', '+1', '+2', '∅', '2×', 'curse', 'bless'].includes(m),
    },
    counter: Number,
  },

  computed: {
    tag() {
      return typeof this.modifier === 'undefined' ? 'button' : 'div';
    },

    modifierText() {
      switch (this.modifier) {
        case 'curse':
          return '∅';
        case 'bless':
          return '2×';
        default:
          return this.modifier;
      }
    },
  },
};
</script>

<style scoped>
.card {
  position: relative;
  font-size: calc((100vw - 20px) * 0.40);
  text-align: center;
  width: 100%;
  border-radius: calc((100vw - 20px) * 0.05);
  height: calc((100vw - 20px) * 0.68);
  margin-bottom: 10px;
  background-color: #aa8872;
  border: 10px solid #32241c;
  text-shadow: 0 0 10px #32241c;
  color: white;
}

.card:last-child {
  margin-bottom: 0;
}

.card__modifier {
  line-height: calc((100vw - 20px) * 0.68);
  height: calc((100vw - 20px) * 0.68);
}

.card__counter {
  font-size: 2rem;
  position: absolute;
  bottom: .7em;
  right: 1em;
}

.card--hidden {
  cursor: pointer;
}

.card--positive {
  background-color: #a5b67b;
  border-color: #172b18;
  text-shadow: 0 0 10px #172b18;
}

.card--negative {
  background-color: #cc8e7e;
  border-color: #49100f;
  text-shadow: 0 0 10px #49100f;
}

.card--miss {
  background-color: #f0d38a;
  border-color: #56411f;
  text-shadow: 0 0 10px #56411f;
}

.card--critical {
  background-color: #b796b6;
  border-color: #130d25;
  text-shadow: 0 0 10px #130d25;
}

.card--curse .card__modifier::before {
  position: absolute;
  top: 0;
  left: 4%;
  content: '⚡';
  display: block;
  font-size: .5em;
}

.card--curse .card__modifier::after {
  position: absolute;
  top: 0;
  right: 4%;
  content: '⚡';
  display: block;
  font-size: .5em;
}

.card--bless .card__modifier::before {
  position: absolute;
  top: 0;
  left: 7%;
  content: '⊛';
  display: block;
  font-size: .5em;
}

.card--bless .card__modifier::after {
  position: absolute;
  top: 0;
  right: 7%;
  content: '⊛';
  display: block;
  font-size: .5em;
}

.card--shuffle .card__modifier::after {
  position: absolute;
  bottom: 5%;
  right: 5%;
  content: '⟳';
  display: block;
  font-size: .5em;
  line-height: 1em;
}
</style>
