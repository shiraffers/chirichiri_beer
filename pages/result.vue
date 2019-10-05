<template>
  <div>
    <Chart :items="items"></Chart>
  </div>
</template>

<style>
</style>

<script>
import Chart from '../components/layout/Chart';
import axios from 'axios';

export default {
  components: {
    Chart
  },
  data() {
    return {
        items: []
    }
  },
  async created() {
    const { data } = await axios.get(`${process.env.API}/beers`);

    this.items = data.Items.sort((a, b) => (a.id > b.id ? 1 : -1));
    console.log(this.items.map(value => value.title));
  }
}
</script>
