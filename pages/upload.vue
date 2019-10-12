
<template>
  <div class="contents">
    <form action="
  " @submit.prevent="handleSubmit">
      <label v-show="!uploadedImage" class="input-item__label">画像を選択</label>
      <input type="file" @change="onFileChange" />
      <div class="preview-item">
        <img v-show="uploadedImage" class="preview-item-file" :src="uploadedImage" alt />
        <div v-show="uploadedImage" class="preview-item-btn" @click="remove">
          <p class="preview-item-name">{{ img_name }}</p>
        </div>
      </div>
      <input type="submit" id="apply-upload" v-show="uploadedImage" />
    </form>
  </div>
</template>

<script>
import axios from "axios";
import AWS from "aws-sdk";

const s3_client = () => {
  AWS.config.region = "ap-northeast-1"; // リージョン
  AWS.config.credentials = new AWS.CognitoIdentityCredentials({
    IdentityPoolId: process.env.IdentityPoolId
  });
  AWS.config.credentials.get(function(err) {
    if (!err) {
      console.log("Cognite success!");
    }
  });
  return new AWS.S3({ params: { Bucket: "shiraffers" } });
};

export default {
  components: {},
  data() {
    return {
      uploadedImage: "",
      img_name: "",
      files: [],
      itemLength: 0
    };
  },
  created() {
    AWS.config.region = "ap-northeast-1"; // リージョン
    AWS.config.credentials = new AWS.CognitoIdentityCredentials({
      IdentityPoolId: process.env.IdentityPoolId
    });
    AWS.config.credentials.get(function(err) {
      if (!err) {
        console.log("Cognite success!");
      }
    });
    var vm = this;
    var dynamo = new AWS.DynamoDB.DocumentClient();
    var params = {
      TableName: "Faces"
    };
    dynamo.scan(params, function(err, data) {
      if (err) {
        console.log("エラー = " + err);
      } else {
        vm.itemLength = data.Count;
      }
    });
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
      var timestamp = new Date().getTime();
      var filename = "file" + timestamp + ".jpg";
      var vm = this;
      s3_client().putObject(
        {
          Key: filename,
          ContentType: this.files[0].type,
          Body: this.files[0],
          ACL: "public-read"
        },
        function(err, data) {
          if (data !== null) {
            console.log(data);
            vm.$router.push({
              name: "faces-id",
              params: {
                id: vm.itemLength + 1
              }
            });
          } else {
            alert("アップロード失敗.");
          }
        }
      );
    }
  }
};
</script>

<style lang="scss">
.preview-item {
  width: 500px;
  height: 500px;
  img {
    width: 100%;
    height: 100%;
  }
}
</style>