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
      <div>
        <label for>アルコール度数</label>
        <input type="text" v-model="alcohol" />
      </div>
      <input type="submit" value="追加" />
    </form>
    <li class="lists" v-for="(beer, index) in beers" :key="index" :class="{ active: beer.isActive}">
      id: {{beer.id}}
      商品名: {{beer.title}}
      値段: {{beer.price}}円
       度数: {{beer.alcohol}}円
      <button
        @click="edit(beer)"
      >編集</button>
      <button @click="deleteItem(beer.id)">削除</button>
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
      price: null,
      alcohol: null
    };
  },
  async created() {
    const { data } = await axios.get(`${process.env.API}/beers`);

    // this.beers = data.Items.sort((a, b) => (a.id > b.id ? 1 : -1));
    data.Items.forEach(beer => {
      this.beers.push({
        id: this.beers.length + 1,
        title: beer.title,
        price: beer.price,
        alcohol: beer.alcohol,
        isActive: false
      });
    });
  },
  methods: {
    async handleSubmit() {
      console.log("aaa");
      console.log(this.price);
      const { data } = await axios.post(`${process.env.API}/beers`, {
        id: this.beers.length + 1,
        title: this.title,
        price: this.price,
        alcohol: this.alcohol
      });
      this.beers.push({
        id: this.beers.length + 1,
        title: this.title,
        price: this.price,
        alcohol: this.alcohol
      });
      this.title = null;
      this.price = null;
      this.alcohol = null;
    },
    edit(beer) {
      this.$router.push({
        name: "beers-id",
        params: {
          id: beer.id,
          beer: beer
        }
      });
    },
    deleteItem(id) {
      this.beers[id - 1].isActive = true;
    }
  }
};
</script>

<style lang="scss">
.active {
  color: $beer;
  text-decoration: line-through;
}
</style>