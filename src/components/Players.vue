<template>
  <div class="container">
    <div class="flex">
      <nav class="navbar navbar-expand-lg navbar-dark">
        <button
          v-on:click="filterClicked"
          class="navbar-toggler"
          type="button"
          data-toggle="collapse"
          data-target="#navbarSupportedContent"
          aria-controls="navbarSupportedContent"
          aria-expanded="false"
          aria-label="Toggle navigation"
        >
          <img src="../assets/filter.png" alt="" width="24" height="24" />
          <span style="color: white; padding:.5rem"> Filters </span>
        </button>
        <div class="collapse navbar-collapse" id="navbarSupportedContent">
          <Dropdown
            @search-in-table="searchInput"
            :options="country"
            :criteria="'Filter by country'"
          />
          <Search :placeholder="'search for a player'" @input="searchInput" />
        </div>
      </nav>
      <div v-bind:class="{ card: true, hidden, absolute:true }">
        <Dropdown
          class="mb-1"
          @search-in-table="searchInput"
          :options="country"
          :criteria="'Filter by country'"
        />
        <Search
          class="mb-1"
          :placeholder="'search for teams and venues'"
          @input="searchInput"
        />
        <button>Apply filters</button>
      </div>
    </div>
    <div class="grid-display">
      <div v-for="player in players" :key="player.name">
        <div class="card">
          <div class="flex">
            <img src="../assets/player.png" alt="" width="48" height="48" />
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
import Dropdown from "./Dropdown";
import { country } from "../data/index";
export default {
  name: "Players",
  data() {
    return {
      country,
      data: [],
      players: [],
      limit: 30,
      busy: false,
      hidden: true,
    };
  },
  components: {
    Search,
    Dropdown,
  },
  methods: {
    searchInput() {
      return this.players;
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
        import("../data/players.json").then(({ default: data }) => {
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
.hidden {
  display: none;
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
  border: none;
  border-radius: 0.5rem;
  background-color: rgb(10, 10, 10);
  cursor: pointer;
}
.absolute{
  position: absolute;
  top:50%;
  z-index: 1;
}
.block {
  display: block;
}
.grid-display {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  grid-gap: 0.5rem;
  margin: 1rem 0;
}
.flex {
  margin-top: 1rem;
  display: flex;
  justify-content: space-around;
  align-items: center;
}
</style>