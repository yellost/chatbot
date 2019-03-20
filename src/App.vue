<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <Chatbot/>
  </div>
</template>

<script>
import Chatbot from './components/Chatbot.vue'

export default {
  name: 'app',
  components: {
    Chatbot
  }
}
</script>

<style lang="scss">
@import 'assets/css/style.scss';
</style>
