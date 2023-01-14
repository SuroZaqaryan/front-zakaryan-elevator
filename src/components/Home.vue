<script>
export default {
  props: ['floors', 'elevators'],

  mounted() {
    this.elevatorsPosition = Array(this.elevators + 1).fill(0)
    this.elevatorHeight = this.$refs.elevators.clientHeight / this.floors;
  },

  data() {
    return {
      elevatorsPosition: [], // Elevators position (starter position equal 0)
      elevatorHeight: 0, // Elevator height
      currentElevator: -1, // Current elevator
      transitionStart: true,
    };
  },

  methods: {
    calcNextElevator() {
      if (this.currentElevator < this.elevators - 1) {
        return this.currentElevator += 1;
      } else {
        return this.currentElevator = 0;
      }
    },

    incrementTop(elevator, ref) {
      // Get the nearest minimum number for call elevator
      let elevators = [...Array.from(Array(this.elevators), (_, index) => index + 1)]
      let nearestMin = elevators[this.calcNextElevator()] - 1;

      if(this.transitionStart) {
        ref[nearestMin].style.transition = 'all 3s ease';
        // Elevator movement
        ref[nearestMin].style.bottom = this.elevatorsPosition[nearestMin] + this.elevatorHeight * (elevator - 1) + 'px';

        ref[nearestMin].addEventListener('transitionstart', () => this.transitionStart = false);

        // (ontransitionend) When elevator animation finished
        ref[nearestMin].ontransitionend = () => {
          const timeout = ms => new Promise(resolve => setTimeout(resolve, ms));

          timeout(0).then(() => ref[nearestMin].classList.add('expectation'));

          timeout(3000).then(() => {
            ref[nearestMin].classList.remove('expectation')
            this.transitionStart = true;
          });
        }
      }
    },
  },

  computed: {
    height() {
      return `${this.elevatorHeight - 1}px`
    }
  }
};
</script>

<template>
  <div class="elevators" ref="elevators">
    <div class="floors">
      <hr v-for="floor in floors + 1" :key="floor"/>
    </div>

    <div v-for="elevator in elevators" :key="elevator" class="mine">
      <div :style="{ height: height }" ref="elevator" class="elevator">
        {{ elevator }}
      </div>
    </div>

    <div class="buttons">
      <button v-for="floor in floors" :key="floor" @click="incrementTop(floor, $refs['elevator'])">
        Up {{ floor }}
      </button>
    </div>
  </div>
</template>

<style scoped>
@import '../assets/styles/style.css';
</style>