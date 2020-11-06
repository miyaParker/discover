<template>
  <div class="container">
    <div class="grid-display">
      <div v-for="team in teams" :key="team.team">
        <div class="card">
          <span class="heading">{{ team.team }}</span>
          <div class="flex">
            <img src="../assets/yellow.png" alt="" />
            <div>
              <span class="block wins"
                >{{ team.home_matches }}/{{ team.home_wins }}</span
              >
              <span class="block wins"
                >{{ team.away_matches }}/{{ team.away_wins }}</span
              >
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import teams from "./teams.json";
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
.heading {
  color: #fdcd00;
  padding: 0 1rem;
  border-bottom: 1px solid grey;
  margin-bottom: 0.5rem;
}
.flex {
  margin-top: 1rem;
  display: flex;
  justify-content: space-around;
  align-items: center;
}
.block {
  display: block;
}
.wins {
  font-size: 1rem;
}
.card {
  cursor: pointer;
  color: #ffffff;
  padding: 1rem 0;
  font-size: 0.9rem;
  box-shadow: 3px 3px 5px 0px #221121;
  border: none;
  border-radius: 0.5rem;
  background-color: #420264;
}
.grid-display {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
  grid-gap: 1rem;
  margin: 1rem 0;
}
</style>