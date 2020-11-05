
<template>
  <NavBar @entity="setEntity" />
  <Matches v-show="entity == 'Matches'" />
  <Teams v-show="entity == 'Teams'" />
  <Players v-show="entity == 'Players'" />
</template>
<script>
import { defineAsyncComponent } from "vue";
import NavBar from "./components/NavBar";
const Matches = defineAsyncComponent(() => import("./components/Matches"));
const Teams = defineAsyncComponent(() => import("./components/Teams"));
const Players = defineAsyncComponent(() => import("./components/Players"));
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
      console.log(this.entity);
    },
  },
};
</script>
<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Carme&display=swap');
#app {
  background-color:#340744;
  font-family: Carme;
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
