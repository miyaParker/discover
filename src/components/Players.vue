<template>
  <div class="container">
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
            <img src="../assets/player.png" alt="" />
            <div>
              <span class="block detail">{{ player.Player_Name }}</span>
              <span class="block">{{ player.DOB }}</span>
              <span>{{ player.Country }}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div>
      <button v-on:click="loadData" class="button">Load More</button>
    </div>
  </div>
</template>
<script>
import Search from "./Search";
import Filter from "./Filter";
import { seasons } from "./data";
export default {
  name: "Players",
  data() {
    return {
      seasons,
      data: [],
      players: [],
      limit: 12,
      busy: false,
    };
  },
  components: {
    Search,
    Filter,
  },
  methods: {
    searchInput(value) {
      this.players = this.players.filter((player) =>
        Object.values(player).includes(value)
      );
    },
    fetchData() {
      const dataList = localStorage.getItem("players");
      this.data = JSON.parse(dataList);
      console.log("...fetching players");
      this.loadData();
    },
    loadData() {
      this.busy = true;
      console.log("...loading players");
      const append = this.data.slice(
        this.players.length,
        this.limit + this.players.length
      );
      this.players = this.players.concat(append);
      this.busy = false;
      console.log("...finished loading players");
    },
    filterClicked() {
      this.hidden = this.hidden ? false : true;
    },
  },
  created() {
    if (window.localStorage) {
      if (localStorage.getItem("players") === null) {
        import("./players.json").then(({ default: data }) => {
          localStorage.setItem("players", JSON.stringify(data));
          this.fetchData();
        });
      } else {
        this.fetchData();
      }
    }
  },
};
</script>
<style scoped>
.button {
  border: none;
  padding: 0.5rem 1rem;
  border-radius: 0.5rem;
}
.detail {
  font-size: 1rem;
}
.heading {
  color: #fdcd00;
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
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
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