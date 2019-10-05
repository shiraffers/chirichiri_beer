<template>
  <div>
    <nav class="nav navbar-nav">
      <ul class>
        <li v-for="(item,index) in items" :key="index" class="block">
          <router-link :to="item.path">{{item.title}}</router-link>
        </li>
      </ul>
    </nav>
  </div>
</template>

<script scoped>
export default {
  data() {
    return {
      items: [
        {
          title: "Beers",
          path: "/"
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
    font-size: 30px;
    color: #000;
  }
  a:hover {
    text-decoration: none;
    color: #f6ad49;
  }
  .block {
    margin: 0 10px;
  }
}
</style>
