<template>
  <div class="container">
    <div class="flex filter">
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
            :options="seasons"
            :criteria="'Filter by season'"
          />
          <Dropdown
            @search-in-table="searchInput"
            :options="teams"
            :criteria="'Filter by team'"
          />
          <Dropdown
            @search-in-table="searchInput"
            :options="venues"
            :criteria="'Filter by venue'"
          />
          <Search
            :placeholder="'search for teams and venues'"
            @input="searchInput"
          />
        </div>
      </nav>
      <div v-bind:class="{ card: true, hidden, absolute:true }">
        <Dropdown
          class="mb-1"
          @search-in-table="searchInput"
          :options="seasons"
          :criteria="'Filter by season'"
        />
        <Dropdown
          class="mb-1"
          @search-in-table="searchInput"
          :options="teams"
          :criteria="'Filter by team'"
        />
        <Dropdown
          class="mb-1"
          @search-in-table="searchInput"
          :options="venues"
          :criteria="'Filter by venue'"
        />
        <Search
          class="mb-1"
          :placeholder="'search for teams and venues'"
          @input="searchInput"
        />
      </div>
    </div>
    <div class="grid">
      <div v-for="match in matches" :key="match.id" class="card">
        <div class="flex heading">
          <span>{{ match.venue }}, {{ match.city }}</span>
          <span>{{ match.date }}</span>
        </div>
        <div class="flex details">
          <div class="flex">
            <img src="../assets/red.png" alt="" width="48" height="48" />
            <span>{{ match.team1 }}</span>
          </div>
          vs
          <div class="flex">
            <span>{{ match.team2 }}</span>
            <img src="../assets/yellow.png" alt="" width="48" height="48" />
          </div>
        </div>
        <div>
          <span v-show="match.win_by_runs != '0'"
            >{{ match.winner }} won by {{ match.win_by_runs }} runs
          </span>
          <span v-show="match.win_by_wickets != '0'"
            >{{ match.winner }} won by {{ match.win_by_wickets }} wickets
          </span>
        </div>
      </div>
    </div>
    <div>
      <button v-on:click="loadData" class="button">Load More Matches</button>
    </div>
  </div>
</template>
<script>
import Search from "./Search";
import Dropdown from "./Dropdown";
import { teams, seasons, venues } from "../data/index";
export default {
  name: "Matches",
  components: {
    Search,
    Dropdown,
  },
  data() {
    return {
      data: [],
      matches: [],
      limit: 30,
      busy: false,
      teams,
      seasons,
      venues,
      hidden: true,
    };
  },
  methods: {
    searchInput() {
      return this.matches
    },
    fetchData() {
      const dataList = localStorage.getItem("matches");
      this.data = JSON.parse(dataList);
      this.loadData();
    },
    loadData() {
      this.busy = true;
      const append = this.data.slice(
        this.matches.length,
        this.limit + this.matches.length
      );
      this.matches = [...this.matches, ...append];
      this.busy = false;
    },
    filterClicked() {
      this.hidden = this.hidden ? false : true;
    },
  },
  created() {
    if (window.localStorage) {
      if (localStorage.getItem("matches") === null) {
        import("../data/matches.json").then(({ default: data }) => {
          localStorage.setItem("matches", JSON.stringify(data));
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
.container {
  padding-bottom: 1rem;
}
.button {
  border: none;
  padding: 0.5rem 1rem;
  border-radius: 0.5rem;
}
.card {
  color: #ffffff;
  padding: 1rem 0;
  font-size: 0.9rem;
  border: none;
  border-radius: 0.5rem;
  background-color:rgb(10, 10, 10);
  cursor: pointer;
}
.hidden {
  display: none;
}
.absolute{
  position: relative;
  top:50%;
  z-index: 1;
}
.heading {
  color: #696969;
  padding: 0 1rem;
  border-bottom: 1px solid grey;
  margin-bottom: 0.5rem;
}
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
  grid-gap: .5rem;
  margin: 1rem 0;
}
.filter{
  justify-content: space-around;
  flex-direction: column;
}
.flex {
  padding: 0.5rem 0.5rem;
  display: flex;
  justify-content: space-around;
  align-items: center;
}
.details {
  padding: 0 1rem;
}
</style>