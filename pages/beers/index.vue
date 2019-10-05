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



    <form @submit.prevent="handleSubmit" class="beer_form">
      <div style="width: 100%;">
        <div style="text-align: center; margin-top: 20px;">
          <label style="font-size: 32px;">ビールを追加</label><br>
           <input type="text" class="block" placeholder="ビール名" v-model="title" />
        </div>
        <div style="text-align: center; margin-top: 20px;">
          <input type="text" placeholder="値段" class="block" v-model="price" />
        </div>
        <div style="text-align: center; margin-top: 20px;">
          <input type="text" placeholder="アルコール度数" class="block" v-model="alcohol" />度
        </div>
        <div style="text-align: center; margin-top: 20px; ">
          <input type="submit" style="width: 15%; color: #F39800;border-radius: 15px;border-color: gray;" value="追加" />
        </div>
         
      </div>
    </form>
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
.beer_form {
  padding-top: 20px;
}
.form_label {
  font-size: 30px;
}
.block{
  text-align: center;
  width: 200px;
  border-radius: 15px;
  border-color: gray;
  transition: 0.25s;
}
.block:focus{
  width: 300px
}
</style>