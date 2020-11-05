<template>
  <div class="container-fluid">
    <div class="flex">
      <Filter
        @search-in-table="searchInput"
        :options="seasons"
        :criteria="'Filter by season'"
      />
      <Search :placeholder="'search for a player'" @input="searchInput" />
    </div>

    <div class="grid-display">
      <div v-for="player in players" :key="player.name">
        <div class="card">
          <div class="flex">
            
            <img src="./media/player.png" alt="" />
            <div>
              <span class="block detail">{{ player.Player_Name}}</span>
              <span class="block">{{ player.DOB}}</span>
              <span >{{player.Country}}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import Search from "./Search";
import Filter from "./Filter";
import players from "./players.json";
import { seasons } from "./data";
export default {
  name: "Players",
  data() {
    return {
      players,
      seasons,
    };
  },
  components: {
    Search,
    Filter,
  },
  methods: {
    searchInput(value) {
      console.log(value);
      this.players = players;
      this.players = this.players.filter((player) =>
        Object.values(player).includes(value)
      );
    },
  },
};
</script>
<style scoped>
.detail{
  font-size: 1rem;
}
.heading {
  color: #FDCD00;
  padding: 0 1rem;
  border-bottom: 1px solid grey;
  margin-bottom: 0.5rem;
}
.card {
  color: #ffffff;
  padding: 1rem 0;
  font-size: 0.9rem;
  box-shadow: 3px 3px 5px 0px #221121;
  border: none;
  border-radius: 0.5rem;
  background-color: #420264;
  cursor: pointer;
}
.block {
  display: block;
}
.grid-display {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr;
  grid-gap: 1rem;
  margin: 1rem 0;
}
.flex {
  margin-top: 1rem;
  display: flex;
  justify-content: space-around;
  align-items: center;
}
</style>