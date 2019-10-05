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
  </div>
</template>

<script>
import axios from "axios";
export default {
  components: {},
  data() {
    return {
      uploadedImage: "",
      img_name: ""
    };
  },
  methods: {
    onFileChange(e) {
      const files = e.target.files || e.dataTransfer.files;
      this.createImage(files[0]);
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
      const url =
        "https://console.aws.amazon.com/apigateway/home?region=us-east-1#/apis/ezh4ta8lql/stages/v1";
      const { data } = await axios.post(url, {
        image: this.uploadedImage
      });
      console.log(data);
    }
  }
};
</script>