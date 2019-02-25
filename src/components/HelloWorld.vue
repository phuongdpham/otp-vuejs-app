<template>
  <div class="hello">
    <a
      class="btn-sign-in"
      href="https://id-uat.vndirect.com.vn/login?httpReferer=http://localhost:8080/callback"
    >Sign in with VNDirect</a>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String
  }
};
</script>

<style lang="stylus">
.hello
  .btn-sign-in
    background orange
    color #FFF
    padding 1rem 2rem
    text-decoration none
</style>
