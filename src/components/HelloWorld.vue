<template>
  <div class="hello">
    <div class="red-text">Red hello world text (check css bellow)</div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  }
}
</script>

<style lang="stylus">
.hello
  .red-text
    color red
</style>
