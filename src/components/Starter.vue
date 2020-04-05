<template lang="html">
    <section class="flex-center-col">
        <button class="neon-text" @click="startGame">START</button>

        <!-- Category dropdown selector -->
        <div class="dropdown" @click="dropdownToggle">
            <!-- dropdown label -->
            <div class="dropdown-label">
                <h2>Select a Topic</h2>
            </div>
            <h3>{{ currentCategory.name }}</h3>
            <ul>
                <li @click="chooseCategory({ name: 'Random', id: 9 })">Random</li>
                <li v-for="category in categories" @click="chooseCategory(category)">{{ category.name }}</li>
            </ul>
        </div>
    </section>
</template>

<script>
    export default {
        computed: {
            aboutShow() {
                return this.$store.state.aboutShow;
            },
            categories() {
                // Return categories from state
                return this.$store.state.categories;
            },
            currentCategory() {
                return this.$store.state.currentCategory;
            },
            mode() {
                // Return mode from state. Used to determine selected button style.
                return this.$store.state.solo;
            }
        },
        methods: {
            // Send category data to store, set selected category
            chooseCategory(category) {
                this.$store.commit('setCurrentCategory', category);
            },
            // Toggle animation/display classes for category list
            dropdownToggle() {
                this.toggleExpand = !this.toggleExpand;
                if (this.dropdownClass.open) {
                    this.dropdownClass.open = false;
                    this.dropdownClass.close = true;
                } else {
                    this.dropdownClass.open = true;
                    this.dropdownClass.close = false;
                }
            },
            // Call selectMode mutation
            selectMode(mode) {
                this.$store.commit('selectMode', mode);
            },
            startGame() {
                this.$store.dispatch('startGame');
                // Close About menu if open
                if (this.aboutShow) {
                    this.$store.commit('aboutToggle');
                }
            },
        },
    }
</script>

<style lang="scss" scoped>@import ".././main.scss";

@media (max-width: 600px) {
    section {
        padding: 0 2.5%;
    }
}

.expand-less,
.expand-more,
h2,
li {
    cursor: pointer;
}

h3 {
    color: $color-white;
    margin: 0;
}

li {
    margin: 0.5em;

    &:hover {
        color: $color-main;
    }
}

ul {
    max-height: 0;
    text-align: left;
    // opacity: 0;
}

// Selected mode button
.active {
    background-color: $color-darkest;
    border: 2px solid $color-darkest;
    color: $color-white;
}

.dropdown-label {
    align-items: center;
    display: flex;
    justify-content: center;
}

// Category dropdown expand icon
.expand-less,
.expand-more {
    font-size: 2em;
    padding-top: 7px;
    margin-left: 15px;
    transition: all 0.3s ease;
}

.expand-more {
    color: $color-white;

    &:hover {
        color: $color-darkest;
    }
}

.expand-less {
    color: $color-darkest;

    &:hover {
        color: $color-white;
    }
}

button.neon-text {
    background-color: $color-main;
    color: $color-white;
}
</style>
