<template>
  <div v-if="loading">
    <p>性別: {{face.Gender}}</p>
    <p>メガネ: {{face.Eyeglasses}}</p>
    <p>ひげ: {{face.Mustache}}</p>
    <p>年齢: {{face.AgeRangeLow}} ~ {{face.AgeRangeHigh}}</p>
    <p>笑顔: {{face.Smile}}</p>
    <label for>感情</label>
    <div v-for="(emotion, i) in face.Emotions" :key="i">{{emotion.Type}}: {{emotion.Confidence}}</div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      loading: false,
      face: {
        Gender: "",
        Eyeglasses: false,
        Emotions: [],
        AgeRangeHigh: 0,
        Mustache: false,
        id: 0,
        Smile: 0,
        AgeRangeLow: 0
      }
    };
  },
  async created() {
    const url = `https://hxzzjthykg.execute-api.ap-northeast-1.amazonaws.com/v1/faces/${this.$route.params.id}`;
    const { data } = await axios.get(url);
    console.log(data);
    this.face = data.Item;
    this.loading = true;
  }
};
</script>

<style>
</style>