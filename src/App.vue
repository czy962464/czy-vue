<template>
  <div id="app">
    <div class='dell' @click='handelClick'>{{message}}</div>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      message: 'hello world'
    }
  },
  methods: {
    handelClick () {
      this.message = 'bye'
    }
  }
}
</script>

<style>
  .dell{
    color: red
  }
</style>
