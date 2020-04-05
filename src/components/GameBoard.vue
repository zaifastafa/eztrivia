<template lang="html">
  <section>
    <app-questions></app-questions>
    <app-score-dots></app-score-dots>
    <app-star-power></app-star-power>
  </section>
</template>

<script>
import Questions from './Questions.vue';
import ScoreDots from './ScoreDots.vue';
import StarPower from './StarPower.vue';
export default {
  components: {
    'app-questions': Questions,
    'app-score-dots': ScoreDots,
    'app-star-power': StarPower,
  },
  computed: {
    mode() {
      return this.$store.getters.mode;
    },
    turn() {
      return this.$store.getters.turn;
    }
  }
}
</script>

<style lang="scss" scoped>@import ".././main.scss";

h2 {
    font-weight: bold;
}

section {
  max-width: 600px;
  position: relative;
}

@media (max-width: 600px) {
  h2 {
    margin: 0;
  }
}
</style>
