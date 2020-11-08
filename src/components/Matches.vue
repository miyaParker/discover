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
          <span style="color: white; padding: 0.5rem"> Filters </span>
        </button>
        <div class="collapse navbar-collapse" id="navbarSupportedContent">
          <Dropdown
            v-on:change="setSeason"
            @search-in-table="searchInput"
            :options="seasons"
            :criteria="'Filter by season'"
          />
          <Dropdown
            v-on:change="setTeam"
            @search-in-table="searchInput"
            :options="teams"
            :criteria="'Filter by team'"
          />
          <Dropdown
            v-on:change="setVenue"
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
      <div v-bind:class="{ card: true, hidden, absolute: true }">
        <Dropdown
          v-on:change="setSeason"
          class="mb-1"
          @search-in-table="searchInput"
          :options="seasons"
          :criteria="'Filter by season'"
        />
        <Dropdown
          v-on:change="setTeam"
          class="mb-1"
          @search-in-table="searchInput"
          :options="teams"
          :criteria="'Filter by team'"
        />
        <Dropdown
          v-on:change="setVenue"
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
      <span>
        <img
          src="../assets/loader.png"
          alt=""
          width="30"
          height="30"
          v-show="busy === true"
          class="loader"
        />
      </span>
      <button v-on:click="loadData(data)" class="button">
        Load More Matches
      </button>
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
      season: "",
      team: "",
      venue: "",
      filterList: [],
      data: [],
      matches: [],
      limit: 30,
      busy: false,
      teams,
      seasons,
      venues,
      value: "",
      hidden: true,
    };
  },
  methods: {
    setTeam() {
      this.team = this.value;
      this.filterDataList();
    },
    setVenue() {
      this.venue = this.value;
      this.filterDataList();
    },
    setSeason() {
      this.season = this.value;
      this.filterDataList();
    },
    filterDataList() {
      this.resetLists();
      this.filterList = this.data.filter(
        (match) =>
          Object.values(match).includes(this.season) &&
          Object.values(match).includes(this.team) &&
          Object.values(match).includes(this.venue)
      );
      this.loadData(this.filterList);
    },
    resetLists() {
      this.matches = [];
      this.filterList = [];
    },
    searchInput(value) {
      this.value = value;
    },
    fetchData() {
      const dataList = localStorage.getItem("matches");
      this.data = JSON.parse(dataList);
      this.loadData(this.data);
    },
    loadData(dataList) {
      this.busy = true;
      const append = dataList.slice(
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
.loader {
  animation: roll 3s infinite;
  transform: rotate(30deg);
}
@keyframes roll {
  0% {
    transform: rotate(0);
  }
  100% {
    transform: rotate(360deg);
  }
}
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
  background-color: rgb(10, 10, 10);
  cursor: pointer;
}
.hidden {
  display: none;
}
.absolute {
  position: relative;
  top: 50%;
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
  grid-gap: 0.5rem;
  margin: 1rem 0;
}
.filter {
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