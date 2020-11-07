<template>
  <form v-on:submit="submitInput($event)">
    <input type="text" :placeholder="placeholder"/>
    <button>search</button>
  </form>
</template>
<script>
export default {
  name: "Search",
  data() {
    return {
      searchInput: "",
    };
  },
  props: {
    placeholder: {
      type: String,
    },
  },
  methods: {
    submitInput(e){
      e.preventDefault()
    },
    fetchInput() {
      this.$emit("input", this.searchInput);
    },
  },
};
</script>
<style scoped>
*{
  box-sizing: border-box;
}
input {
  margin-right: 1rem;
  display: inline;
  border-radius: 0.5rem;
  border: none;
  padding: 0.6rem;
  background-color: ghostwhite;
}
button {
  border-radius: 0.5rem;
  border: none;
  padding: 0.6rem 1rem;
}
button:hover{
  background-color: #F1C40F;
  color: white;
}
</style>