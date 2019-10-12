<template>
  <div v-if="loading" style="width: 50%;">
    <h3>-分析結果-</h3>
    <img :src="$route.params.uploadedImage" width="300px" />
    <p>性別: {{face.Gender === "Male" ? "男性" : "女性"}}</p>
    <p>メガネ: {{face.Eyeglasses === false ? "なし": "あり"}}</p>
    <p>口ひげ: {{face.Mustache === false ? "なし": "あり"}}</p>
    <p>年齢: {{face.AgeRangeLow}}歳 ~ {{face.AgeRangeHigh}}歳</p>
    <!-- <div class="mb-3">
      笑顔: {{face.Smile | truncation}}%
      <div class="progress-bar bg-success" :style="{width: `${face.Smile}%`, height: `20px;`}"></div>
    </div>-->

    <div v-for="(emotion, i) in face.Emotions" :key="i" class="mb-3">
      {{emotion.Type | translation}}: {{emotion.Confidence | truncation}}%
      <div
        class="progress-bar bg-success"
        :style="{width: `${emotion.Confidence}%`, height: `20px;`}"
      ></div>
    </div>
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
  filters: {
    translation: function(value) {
      switch (value) {
        case "CONFUSED":
          return "困惑";
        case "CALM":
          return "穏やか";
        case "FEAR":
          return "恐怖";
        case "DISGUSTED":
          return "嫌悪";
        case "ANGRY":
          return "怒り";
        case "HAPPY":
          return "幸福";
        case "SAD":
          return "悲しみ";
        case "SURPRISED":
          return "驚き";
      }
    },
    truncation: function(value) {
      return Math.ceil(value);
    }
  },
  async created() {
    console.log(this.$route.params);
    const url = `https://hxzzjthykg.execute-api.ap-northeast-1.amazonaws.com/v1/faces/${this.$route.params.id}`;
    const { data } = await axios.get(url);
    console.log(data);
    this.face = data.Item;
    this.loading = true;
  }
};
</script>

<style scoped>
.progress-bar {
  height: 20px;
}
</style>