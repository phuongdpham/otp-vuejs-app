<template>
  <div id="app">
    <TokenIssue v-if="this.tokenId" :token-id="tokenId" />
    <Login v-else />
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import Login from './components/Login.vue';
import  TokenIssue from './components/TokenIssue.vue'

export default {
  name: 'app',
  components: {
    HelloWorld,
    Login,
    TokenIssue,
  },
  data() {
    return {
      tokenId: null,
    }
  },
  mounted() {
    const uri = window.location.search.substring(1)
    const params = new URLSearchParams(uri)
    const tokenId = params.get('token-id')
    if (tokenId) {
      localStorage.setItem('tokenId', tokenId)
    }

    const localTokenId = localStorage.getItem('tokenId')
    this.tokenId = localTokenId
  },
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
