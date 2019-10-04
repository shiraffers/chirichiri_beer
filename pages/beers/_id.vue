<template>
  <div>
    <p>id: {{beer.id}}</p>
    <p>商品名: {{beer.title}}</p>
    <p>値段: {{beer.price}}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      beer: this.$route.params.beer
    };
  },
  mounted() {
    console.log(this.$route.params);
  }
};
</script>

<style>
</style>