<template>
  <NavBar @entity="setEntity" />
  <Matches v-show="entity == 'Matches'" />
  <Teams v-show="entity == 'Teams'" />
  <Players v-show="entity == 'Players'" />
</template>
<script>
import NavBar from "./components/NavBar";
import Matches from './components/Matches';
import Teams from "./components/Teams";
import Players from "./components/Players";
export default {
  components: {
    Matches,
    Players,
    Teams,
    NavBar,
  },
  data() {
    return {
      entity: "Matches",
    };
  },
  methods: {
    setEntity(value) {
      this.entity = value;
    },
  },
};
</script>
<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Roboto&display=swap');
#app {
  background-color: #010100;
  font-family: Roboto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}

#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
