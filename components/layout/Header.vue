<template>
  <div>
    <ul>
      <li v-for="(item,index) in items" :key="index">
        <router-link :to="item.path">{{item.title}}</router-link>
      </li>
    </ul>
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
        }
      ]
    };
  }
};
</script>

<style lang="scss">
ul {
  list-style: none;
  display: flex;
  justify-content: space-around;
  width: 30%;
  line-height: 80px;
  a {
    color: #000;
  }
}
</style>
