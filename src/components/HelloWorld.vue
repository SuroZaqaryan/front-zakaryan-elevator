<script>
export default {
  props: [ 'floors', 'elevators' ],

  mounted() {
    for(let i = 0; i <= this.elevators; i++) {
      this.elevatorsPosition.push(0)
    }
  },

  data() {
    return {
      elevatorsPosition: [],
    };
  },

  methods: {
    incrementTop(elevator, ref) {
      let currentElevatorPosition = this.elevatorsPosition[elevator] += 10;
      ref[0].style.bottom = currentElevatorPosition + '%';
    },
  },
};
</script>

<template>
  <div class="elevators">
    <div v-for="elevator in elevators" :key="elevator" class="mine">
      <button @click="incrementTop(elevator, $refs[`elevator-${elevator}`])">Up</button>

      <div :ref="'elevator-' + elevator" class="elevator">
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