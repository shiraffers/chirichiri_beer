<template>
  <div v-if="loading" class="row">
    <div class="col-sm-6">
      <h3>-分析結果-</h3>
      <br />顔面偏差値は
      <span class="total">{{total | truncation}}</span>です！
      <br />
      <div>
        <img :src="$route.params.uploadedImage" width="300px" />
      </div>

      <br />
      <br />
      <p>性別: {{face.Gender === "Male" ? "男性" : "女性"}}</p>
      <p>メガネ: {{face.Eyeglasses === false ? "なし": "あり"}}</p>
      <p>口ひげ: {{face.Mustache === false ? "なし": "あり"}}</p>
      <p>年齢: {{face.AgeRangeLow}}歳 ~ {{face.AgeRangeHigh}}歳</p>
      <!-- <div class="mb-3">
      笑顔: {{face.Smile | truncation}}%
      <div class="progress-bar bg-success" :style="{width: `${face.Smile}%`, height: `20px;`}"></div>
      </div>-->
    </div>
    <div class="col-sm-6 mt-5">
      <div v-for="(emotion, i) in face.Emotions" :key="i">
        {{emotion.Type | translation}}: {{emotion.Confidence | truncation}}%
        <div
          class="progress-bar bg-success"
          :style="{width: `${emotion.Confidence}%`, height: `20px;`}"
        ></div>
      </div>
    </div>
  </div>
</template>


<script>
import axios from "axios";
import face from "./face.json";

export default {
  data() {
    return {
      loading: false,
      total: 75,
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
  mounted() {},
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
      return Math.ceil(value * 10) / 10;
    }
  },
  methods: {
    diffArray(arr1, arr2) {
      return arr1
        .concat(arr2)
        .filter(item => !arr1.includes(item) || !arr2.includes(item));
    }
  },
  async created() {
    const url = `https://hxzzjthykg.execute-api.ap-northeast-1.amazonaws.com/v1/faces/${this.$route.params.id}`;
    const { data } = await axios.get(url);

    this.face = data.Item;
    console.log(data.Item);
    const faceData = data.Item.Landmarks;

    let result = face.map((itemA, i) => {
      return {
        X: itemA.Y - faceData[i].X,
        Y: itemA.Y - faceData[i].Y
      };
    });
    console.log(result);
    let xsum = 0;
    let ysum = 0;
    result.forEach(res => {
      xsum += res.X;
      ysum += res.Y;
    });

    console.log(xsum, ysum);

    this.total -= 0 - xsum + (0 - ysum);
    console.log(this.total);
    this.loading = true;

    // console.log({ face });
    // console.log({ faceData });
    // console.log(result);
  }
};
</script>

<style scoped>
.progress-bar {
  height: 20px;
}
.total {
  font-size: 22px;
}
</style>