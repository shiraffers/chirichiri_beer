<template>
  <div class="container">
    <h3>Home</h3>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {};
  },
  async created() {
    console.log(process.env.API);
    const { data } = await axios.get(`${process.env.API}/beers`);
    this.beers = data.Items.sort((a, b) => (a.id > b.id ? 1 : -1));
  }
};
</script>

<style scoped>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}
1