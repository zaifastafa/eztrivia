<template>
  <!-- starPower hack for proper animation display on style binding -->
  <div class="container" :style="starPower">
    <app-game-over-modal v-if="isGameOver"></app-game-over-modal>

    <main>
      <header :class="classHeader">
        <h1 class="neon-text spacing">EZTRIVIA</h1>
        <app-about></app-about>
      </header>
      <!-- Primary page components -->
      <transition name="fade" mode="out-in">
        <component :is="currentView"></component>
        <!-- show element to test loading state -->
        <!-- <app-loader></app-loader> -->
      </transition>
    </main>
  </div>
</template>

<script>
import About from "./components/About.vue";
import GameBoard from "./components/GameBoard.vue";
import GameOverModal from "./components/GameOverModal.vue";
import Intro from "./components/Intro.vue";
import Loader from "./components/Loader.vue";
export default {
  components: {
    "app-about": About,
    "app-game-board": GameBoard,
    "app-game-over-modal": GameOverModal,
    "app-intro": Intro,
    "app-loader": Loader,
  },

  computed: {
    aboutShow() {
      return this.$store.state.aboutShow;
    },
    currentView() {
      return this.$store.state.currentView;
    },
    isGameOver() {
      return this.$store.state.isGameOver;
    },
    // Expand and contract header when changing game state
    classHeader() {
      let classHeader = {
        grow: this.currentView === "app-intro",
        small: this.currentView !== "app-intro",
      };
      return classHeader;
    },
    // HACK: set overflow to hidden on primary page container for correct answer animation.
    // Linked to StarPower.vue and starPower state
    starPower() {
      return this.$store.state.starPower
        ? "overflow: hidden"
        : "overflow: inherit";
    },
  },

  methods: {
    aboutToggle() {
      this.$store.commit("aboutToggle");
    },
  },

  created() {
    this.aboutToggle();
    // Fetch categories from open trivia using vue-resource, send to store
    this.$http
      .get("https://opentdb.com/api_category.php")
      .then((response) => {
        return response.json();
      })
      .then((data) => {
        this.$store.state.categories = data.trivia_categories;
      });
  },
};
</script>

<style lang="scss">
@import "main.scss";

$header-shrink-scale: 0.66;

body {
  color: $color-white;
  background-color: black;
  background-image: radial-gradient(
      white,
      rgba(255, 255, 255, 0.2) 2px,
      transparent 40px
    ),
    radial-gradient(white, rgba(255, 255, 255, 0.15) 1px, transparent 30px),
    radial-gradient(white, rgba(255, 255, 255, 0.1) 2px, transparent 40px),
    radial-gradient(
      rgba(255, 255, 255, 0.4),
      rgba(255, 255, 255, 0.1) 2px,
      transparent 30px
    );
  background-size: 550px 550px, 350px 350px, 250px 250px, 150px 150px;
  background-position: 0 0, 40px 60px, 130px 270px, 70px 100px;
  display: flex;
  font-family: "Solway", serif;
  justify-content: center;
  text-align: center;
}

.h1,
h2,
h3 {
  color: #fff;
}

header {
  color: $color-white;
  transform: scale(1); //width: 100%;
}

h1,
p {
  margin: 0;
}

h1 {
  font-size: 6em;
}
main {
  align-items: center;
  animation: fade-in 1.5s ease; // defined in main.scss
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  height: 100%;
}

.container {
  width: 100%;
}

.icon-menu,
.icon-close {
  transition: all 0.3s ease;

  &:hover {
    color: $color-med;
  }
}

.grow {
  animation: grow 0.5s ease forwards; //background-color: $color-dark;
}

.small {
  animation: squeeze 0.5s ease forwards; //background-color: $color-darkest;
}

@keyframes grow {
  from {
    transform: scale($header-shrink-scale);
    padding: 0.25em 0;
  }
  to {
    transform: scale(1);
    padding: 1em;
  }
}

@keyframes squeeze {
  from {
    transform: scale(1);
    padding: 1em;
  }
  to {
    transform: scale($header-shrink-scale);
    padding: 0.5em 0;
  }
}

.expand-enter-active {
  animation: open 0.75s ease forwards;
}

.expand-leave-active {
  animation: close 0.5s ease forwards;
}

@media (max-width: 600px) {
}
</style>
