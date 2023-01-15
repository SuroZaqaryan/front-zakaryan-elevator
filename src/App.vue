<template>
  <main>
    <Home :floors="7" :elevators="4"/>
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
