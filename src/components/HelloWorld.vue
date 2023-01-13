<script>
export default {
  props: [ 'floors', 'elevators' ],

  data() {
    return {
      bottom: 0,
    };
  },

  methods: {
    incrementTop(elevator, ref) {
      this.bottom += 10;
      ref[0].style.bottom = this.bottom + '%';
    },
  },
};
</script>

<template>
  <div class="elevators">
    <div v-for="elevator in elevators" :key="elevator" class="mine">
      <button @click="incrementTop(elevator, $refs[`elevator-${elevator}`])">Up</button>

      <div v-bind:ref="'elevator-' + elevator" class="elevator">
        {{ floors }}
      </div>
    </div>
  </div>
</template>

<style scoped>
.elevators {
  display: flex;
  gap: 20px;
}

.mine {
  position: relative;
  background: rgb(136, 95, 19);
  width: 200px;
  height: 100vh;
}

.elevator {
  position: absolute;
  bottom: 0;
  width: 200px;
  height: 200px;
  border: 1px solid;
  transition: 0.5s ease;
}
</style>