<template>
  <div id="nav" class="flex">
    <h2>Discover</h2>
    <ul>
      <li
        v-on:click="displayMatches($event)"
        v-bind:class="{ active: entity == 'Matches' }"
      >
        Matches
      </li>
      <li
        v-on:click="displayMatches($event)"
        v-bind:class="{ active: entity == 'Players' }"
      >
        Players
      </li>
      <li
        v-on:click="displayMatches($event)"
        v-bind:class="{ active: entity == 'Teams' }"
      >
        Teams
      </li>
    </ul>
  </div>
</template>
<script>
export default {
  name: "NavBar",
  data() {
    return {
      entity: "Matches",
    };
  },
  methods: {
    displayMatches(e) {
      this.entity = e.target.innerText;
      this.$emit("entity", this.entity);
    },
  },
};
</script>
<style scoped>
* {
  box-sizing: border-box;
  margin: 0;
}
#nav {
  padding: 1rem;
  margin-bottom: 1rem;
}
div {
  color: #fff;
}
li {
  padding: 0.5rem 1rem;
  display: inline;
  text-decoration: none;
  cursor: pointer;
}
ul {
  display: grid;
  grid-template-columns: repeat(3,1fr);
}
.flex {
  display: flex;
  justify-content: space-around;
}
.active {
  color: #ffffff;
  border: none;
  border-radius: 0.4rem;
  background-color:#F1C40F;
}
</style>