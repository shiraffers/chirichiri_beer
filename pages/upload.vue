<template>
  <div class="contents">
    <form action="
    " @submit.prevent="handleSubmit">
      <label v-show="!uploadedImage" class="input-item__label">
        画像を選択
        <input type="file" @change="onFileChange" />
      </label>
      <input type="submit" />
    </form>
    <div class="preview-item">
      <img v-show="uploadedImage" class="preview-item-file" :src="uploadedImage" alt />
      <div v-show="uploadedImage" class="preview-item-btn" @click="remove">
        <p class="preview-item-name">{{ img_name }}</p>
      </div>
    </div>
    <button @click="hundleClick">get</button>
  </div>
</template>

<script>
import axios from "axios";
import aws from "aws-sdk";

var albumBucketName = "daichi-chirichiri-beer";
var bucketRegion = "us-ease-1";

AWS.config.update({
  region: bucketRegion
});

var s3 = new AWS.S3({
  apiVersion: "2006-03-01",
  params: { Bucket: albumBucketName }
});
export default {
  components: {},
  data() {
    return {
      uploadedImage: "",
      img_name: "",
      files: []
    };
  },
  methods: {
    onFileChange(e) {
      const files = e.target.files || e.dataTransfer.files;
      this.createImage(files[0]);
      this.files = files;
      this.img_name = files[0].name;
    },
    // アップロードした画像を表示
    createImage(file) {
      const reader = new FileReader();
      reader.onload = e => {
        this.uploadedImage = e.target.result;
      };
      reader.readAsDataURL(file);
    },
    remove() {
      this.uploadedImage = false;
    },
    async handleSubmit() {
      console.log(this.files[0]);
      const url =
        "https://134m4mbvlk.execute-api.us-east-1.amazonaws.com/v1/beers";
      let config = {
        headers: {
          "Content-Type": "image/jpeg"
        }
      };
      const { data } = await axios.post(url, this.files[0], config);
      console.log(data);
    },
    hundleClick() {
      console.log("get");
      axios
        .get("https://134m4mbvlk.execute-api.us-east-1.amazonaws.com/v1/beers")
        .then(res => {
          console.log(res.data);
        });
      // const { data } = axios.get(
      //   "https://134m4mbvlk.execute-api.us-east-1.amazonaws.com/v1/beers"
      // );
      // console.log(data);
    }
  }
};
</script>