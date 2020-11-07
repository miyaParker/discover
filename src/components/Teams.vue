<template>
  <div class="container">
    <div v-for="team in teams" :key="team.team">
      <div class="card">
        <div class="grid-display">
          <p class="heading">{{ team.team }}</p>
         <span><img src="../assets/yellow.png" alt="" width="32" height="32" /></span> 
          <span class="block big-text"
            >{{ team.home_matches }}/{{ team.home_wins }}</span
          >
          <span class="block big-text"
            >{{ team.away_matches }}/{{ team.away_wins }}</span
          >
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import teams from "../data/teams.json";
export default {
  name: "Teams",
  data() {
    return {
      teams,
    };
  },
};
</script>
<style scoped>
.flex {
  margin-top: 1rem;
  display: flex;
  justify-content: space-around;
  align-items: center;
}

.big-text {
  font-size: 1rem;
}
.card {
  color: #ffffff;
  font-size: 1rem;
  border: none;
  border-radius: 0.5rem;
  background-color: rgb(10, 10, 10);
  cursor: pointer;
  margin-bottom: .5rem;
}
.grid-display {
  display: grid;
  justify-content: flex-start;
  align-items: center;
  grid-template-columns: repeat(4, 1fr);
  margin: 1rem;
}
</style>