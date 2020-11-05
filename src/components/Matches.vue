<template>
  <div class="container-fluid">
    <div class="flex">
      <div>
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
      </div>
      <Search
        :placeholder="'search for teams and venues'"
        @input="searchInput"
      />
    </div>
    <div class="grid">
      <div v-for="match in matches" :key="match.id" class="card">
        <div class="flex heading">
          <span>{{ match.venue }}, {{ match.city }}</span>
          <span>{{ match.date }}</span>
        </div>
        <div class="flex details">
          <div class="flex">
            <img src="./media/red.png" alt="">
            <span>{{ match.team1 }}</span>
          </div>
          vs
          <div class="flex">
             <span>{{ match.team2 }}</span>
            <img src="./media/yellow.png" alt="">
          </div>
        </div>
        <div>
          <span v-if="match.win_by_runs != '0'"
            >{{ match.winner }} won by {{ match.win_by_runs }} runs
          </span>
          <span v-if="match.win_by_wickets != '0'"
            >{{ match.winner }} won by {{ match.win_by_wickets }} wickets
          </span>
        </div>
      </div>
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
      matches,
      teams,
      seasons,
      venues,
    };
  },
  methods: {
    searchInput(value) {
      this.matches = matches;
      this.matches = this.matches.filter((match) =>
        Object.values(match).includes(value)
      );
    },
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
  color: #FFFFFF;
  padding: 1rem 0;
  font-size: 0.9rem;
  box-shadow: 3px 3px 5px 0px #221121;
  border: none;
  border-radius: 0.5rem;
  background-color:#420264;
  cursor: pointer;
}
.heading {
  color: #FDCD00;
  padding: 0 1rem;
  border-bottom: 1px solid grey;
  margin-bottom: 0.5rem;
}
.grid {
  display: grid;
  grid-template-columns:1fr 1fr 1fr 1fr;
  grid-gap: 1rem;
  margin: 1rem 0;
}
.flex {
  padding:.5rem .5rem ;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.details {
  padding: 0 1rem;
}
</style>