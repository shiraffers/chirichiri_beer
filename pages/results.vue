<template>
  <div>笑顔度: {{face.details}}</div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      face: null
    };
  },
  async created() {
    const url =
      "https://hxzzjthykg.execute-api.ap-northeast-1.amazonaws.com/v1/faces/2";
    const { data } = await axios.get(url);
    this.face = data.Item;
  }
};
</script>

<style>
</style>