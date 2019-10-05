<template>
  <div>
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
      <ul class="navbar-nav">
        <li v-for="(item,index) in items" :key="index" class="nav-item block">
          <router-link :to="item.path">{{item.title}}</router-link>
        </li>
      </ul>
    </nav>
  </div>
</template>

<script>
export default {
  data() {
    return {
      items: [
        {
          title: "Home",
          path: "/"
        },
        {
          title: "About",
          path: "/about"
        },
        {
          title: "Beers",
          path: "/beers"
        },
        {
          title: "Upload",
          path: "/upload"
        },
         {
          title: "result",
          path: "/result"
        }
      ]
    };
  }
};
</script>

<style lang="scss">
ul {
  margin: 0 auto;
  list-style: none;
  display: flex;
  justify-content: space-around;
  width: 30%;
  line-height: 80px;
  a {
    font-size:30px;
    color: #000;
  }
  a:hover{
    text-decoration: none;
    color:#F6AD49;
  }
  .block{
    margin:0 10px;
  }
}
</style>
