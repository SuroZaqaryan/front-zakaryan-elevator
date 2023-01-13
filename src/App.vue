<template>
  <main>
    <HelloWorld :floors="5" :elevators="2"/>
  </main>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  }
}
</script>

<style>
body {
  background: rgb(88, 88, 88);
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

main {
  position: relative;
  height: 100vh;
}
</style>
