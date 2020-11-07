<template>
  <select v-on:change="fetchInput" v-model="filterOption">
    <option disabled value="">{{ criteria }}</option>
    <option v-for="option in options" :key="option.value" :value="option.value">
      {{ option.text }}
    </option>
  </select>
</template>
<script>
export default {
  name: "Dropdown",
  data() {
    return {
      filterOption: "",
    };
  },
  props: {
    criteria: {
      type: String,
    },
    options: {
      type: Array,
    },
  },
  methods: {
    fetchInput() {
        console.log(this.filterOption)
      this.$emit("search-in-table", this.filterOption);
    },
  },
};
</script>
<style scoped>
select {
  border: none;
  border-radius: 0.5rem;
  padding: 0.7rem;
  background-color: ghostwhite;
  margin-right: 1rem;
  
}
</style>