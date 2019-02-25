<template>
  <div id="login">
    <h1>Login</h1>
    <input type="text" name="username" v-model="input.username" placeholder="Username">
    <input type="password" name="password" v-model="input.password" placeholder="Password">
    <button type="button" @click="login">Login</button>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Login",
  data() {
    return {
      input: {
        username: "",
        password: ""
      }
    };
  },
  methods: {
    async login() {
      try {
        const response = await axios.post(
          "https://online-account-uat.vndirect.com.vn/login",
          this.input
        );
        
        localStorage.setItem('tokenId', response.data);
        this.$emit('setToken', response.data)
      } catch (err) {
        alert(err);
      }
    }
  }
};
</script>

<style lang='stylus' scoped>
#login
  width 500px
  border 1px solid #cccccc
  background-color #ffffff
  margin auto
  margin-top 200px
  padding 20px

</style>