<template>
  <div>
    <button @click="getData">Click</button>
  </div>
</template>

<script>
import axios from "axios";
export default {
  created() {},
  methods: {
    async getData() {
      const token =
        "xoxp-7077341829-431849450133-785968332995-e90cd3ce7f476550c6496a52c28d872f";
      const url = `https://slack.com/api/users.list?token=${token}`;

      const { data } = await axios.get(url);
      console.log(data);
      const lists = data.members.map(res => {
        return { id: res.id, name: res.profile.real_name };
      });
      //   console.log(lists);
      const pretty = JSON.stringify(lists, null, 4);
      console.log(pretty);
    }
  }
};
</script>

<style>
</style>