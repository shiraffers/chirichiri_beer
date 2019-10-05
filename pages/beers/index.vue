<template>
  <div class="container">
    
    <h3>beers</h3>










    <table class="table table-responsive-sm table-hover table-outline mb-0">
                    <thead class="thead-light">
                      <tr>
                        <th>ビール名</th>
                        <th>値段</th>
                        <th>アルコール度数</th>
                      </tr>
                    </thead>
                    <tbody>
                       <tr class="lists" v-for="(beer, index) in beers" :key="index">
                        <td class="text-center">
                          <div>{{beer.title}}</div>
                          <div class="small text-muted">
                            <span>New</span> | Registered: Jan 1, 2015</div>
                        </td>
                        <td>
                          <div>{{beer.price}}円</div>
                          <div class="small text-muted">
                            <span>New</span> | Registered: Jan 1, 2015</div>
                        </td>
                        <td>
                          <div class="clearfix">
                            <div class="float-left">
                              <strong>{{beer.alcohol}}</strong>
                            </div>
                          </div>
                          <div class="progress progress-xs">
                            <div class="progress-bar bg-success" role="progressbar" style="width: 50%" :aria-valuenow="beer.alcohol" aria-valuemin="0" aria-valuemax="100"></div>
                          </div>
                        </td>
                      </tr>
                    </tbody>
                  </table>



                  
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
        <input type="text" v-model="alcohol" />度数
      </div>
      <input type="submit" value="追加" />
    </form>
    <li class="lists" v-for="(beer, index) in beers" :key="index">
      id: {{beer.id}}
      商品名: {{beer.title}}
      値段: {{beer.price}}円
       度数: {{beer.alcohol}}度
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

    this.beers = data.Items.sort((a, b) => (a.id > b.id ? 1 : -1));
          console.log(this.beers);
  },
  methods: {
    async handleSubmit() {
      console.log("aaa");

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