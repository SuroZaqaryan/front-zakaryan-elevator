<template>
  <main>
    <Home :floors="5" :elevators="1"/>
  </main>
</template>

<script>
import Home from './components/Home.vue'

export default {
  name: 'App',
  components: {
    Home
  }
}
</script>

<style>
body {
  background: rgb(88, 88, 88);
  margin: 0;
  padding: 0;
  overflow: hidden;
  box-sizing: border-box;
}

main {
  position: relative;
  height: 100vh;
}
</style>
