<template>
  <div></div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {};
  },
  async created() {
    const url = `https://hxzzjthykg.execute-api.ap-northeast-1.amazonaws.com/v1/faces/${this.$route.params.id}`;
    const { data } = await axios.get(url);
    console.log(data);
  }
};
</script>

<style>
</style>