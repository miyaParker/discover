<template>
  <div class="container-fluid">
    <div class="grid">
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
          <img src="./media/filter.png" alt="" />
        </button>
        <div class="collapse navbar-collapse" id="navbarSupportedContent">
          <Filter
            @search-in-table="searchInput"
            :options="seasons"
            :criteria="'Filter by season'"
          />
          <Filter
            @search-in-table="searchInput"
            :options="teams"
            :criteria="'Filter by team'"
          />
          <Filter
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
      <div v-bind:class="{ card: true, hidden: hidden }">
        <Filter
          class="mb-1"
          @search-in-table="searchInput"
          :options="seasons"
          :criteria="'Filter by season'"
        />
        <Filter
          class="mb-1"
          @search-in-table="searchInput"
          :options="teams"
          :criteria="'Filter by team'"
        />
        <Filter
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
      <div v-for="match in matchesArray" :key="match.id" class="card">
        <div class="flex heading">
          <span>{{ match.venue }}, {{ match.city }}</span>
          <span>{{ match.date }}</span>
        </div>
        <div class="flex details">
          <div class="flex">
            <img src="./media/red.png" alt="" />
            <span>{{ match.team1 }}</span>
          </div>
          vs
          <div class="flex">
            <span>{{ match.team2 }}</span>
            <img src="./media/yellow.png" alt="" />
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
      <button v-on:click="loadMore">Load More</button>
    </div>
  </div>
</template>
<script>
import Search from "./Search";
import Filter from "./Filter";
import matches from "./matches.json";
import { teams, seasons, venues } from "./data";
export default {
  name: "Matches",
  components: {
    Search,
    Filter,
  },
  data() {
    return {
      matchesArray: [],
      limit: 6,
      busy: false,
      teams,
      seasons,
      venues,
      hidden: true,
    };
  },
  mounted() {},
  methods: {
    searchInput(value) {
      this.matchesArray = this.matchesArray.filter((match) =>
        Object.values(match).includes(value)
      );
    },
    loadMore() {
      this.busy = true;
      const append = matches.slice(
        this.matchesArray.length,
        this.limit + this.matchesArray.length
      );
      this.matchesArray = this.matchesArray.concat(append);
      this.busy = false;
    },
    filterClicked() {
      this.hidden = this.hidden ? false : true;
    },
    
  },
  created() {
      this.loadMore();
    },
};
</script>
<style scoped>
th {
  font-size: 0.8rem;
  text-transform: uppercase;
}
tr {
  font-size: 0.8rem;
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
.hidden {
  display: none;
}
.heading {
  color: #fdcd00;
  padding: 0 1rem;
  border-bottom: 1px solid grey;
  margin-bottom: 0.5rem;
}
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
  grid-gap: 1rem;
  margin: 1rem 0;
}
.flex {
  padding: 0.5rem 0.5rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.details {
  padding: 0 1rem;
}
</style>