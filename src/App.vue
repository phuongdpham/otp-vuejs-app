<template>
  <div id="app">
    <TokenIssue v-if="tokenId" :token-id="tokenId" />
    <Login
      v-else
      @setToken="setToken"
    />
  </div>
</template>

<script>
import Login from './components/Login.vue';
import TokenIssue from './components/TokenIssue.vue';

export default {
  name: 'app',
  components: {
    Login,
    TokenIssue
  },
  data() {
    return {
      tokenId: null
    };
  },
  mounted() {
    const localTokenId = localStorage.getItem('tokenId');

    if (!localTokenId) {
      const uri = window.location.search.substring(1);
      const params = new URLSearchParams(uri);
      const tokenId = params.get('token-id');
      if (tokenId) {
        localStorage.setItem('tokenId', tokenId);
        this.tokenId = tokenId;
      }
    } else {
      this.tokenId = localTokenId;
    }
  },
  methods: {
    setToken(token) {
      this.tokenId = token
    },
  },
};
</script>

<style lang="stylus">
#app
  font-family 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  text-align center
  color #2c3e50
  margin-top 60px

</style>
