<template>
  <div class="container">
    <h3>beers</h3>
    <form @submit.prevent="handleSubmit">
      <div>
        <label for>ビール名</label>
        <input type="text" v-model="title" />
      </div>
      <div>
        <label for>値段</label>
        <input type="text" v-model="price" />
      </div>
      <input type="submit" value="追加" />
    </form>
    <li v-for="(beer, index) in beers" :key="index">
      id: {{beer.id}}
      商品名: {{beer.title}}
      値段: {{beer.price}}円
    </li>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      beers: [],
      title: "",
      price: null
    };
  },
  async created() {
    const { data } = await axios.get(`${process.env.API}/beers`);
    console.log(data);
    this.beers = data.Items.sort((a, b) => (a.id > b.id ? 1 : -1));
  },
  methods: {
    handleSubmit() {
      console.log("aaa");
      console.log(this.price);
      axios
        .post(`${process.env.API}/beers`, {
          id: 3,
          title: this.title,
          price: this.price
        })
        .then(res => {
          console.log(res);
        });
    }
  }
};
</script>

<style>