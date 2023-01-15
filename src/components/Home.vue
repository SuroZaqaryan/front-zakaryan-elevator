<script>
export default {
  props: ['floors', 'elevators'],

  mounted() {
    for (let i = 0; i < this.elevators; i++) this.waitingCalls.push([]);
    this.elevatorsPosition = Array(this.elevators + 1).fill(0)
    this.elevatorHeight = this.$refs.elevators.clientHeight / this.floors;
  },

  data() {
    return {
      elevatorHeight: 0, // Elevator height
      currentElevator: -1, // Current elevator
      elevatorsPosition: [], // Elevators position (starter position equal 0)
      waitingCalls: [],
      nearestMin: '',
      test: -1,
    };
  },

  methods: {
    addPendingCalls(elevator) {
      this.test += 1;

      if (this.currentElevator < this.elevators - 1) this.currentElevator += 1
      else this.currentElevator = 0

      this.waitingCalls[this.currentElevator].push(this.elevatorHeight * (elevator - 1))
      let elevators = [...Array.from(Array(this.elevators), (_, index) => index + 1)]

      this.nearestMin = elevators[this.currentElevator] - 1;
    },

    incrementTop(elevator, ref, nearestMins = this.nearestMin) {
      // Get the nearest minimum number for call elevator
      ref[nearestMins].style.transition = 'all 6s ease';
      // Elevator movement
      ref[nearestMins].style.bottom = this.elevatorsPosition[nearestMins] + this.waitingCalls[nearestMins][0] + 'px';

      // (ontransitionend) When elevator animation finished
      ref[nearestMins].ontransitionend = () => {
        const timeout = ms => new Promise(resolve => setTimeout(resolve, ms));
        timeout(0).then(() => ref[nearestMins].classList.add('expectation'));

        timeout(2000).then(() => {
          this.waitingCalls[nearestMins].splice(0, 1)
          ref[nearestMins].classList.remove('expectation')
          this.incrementTop(elevator, ref, nearestMins);
        });
      }
    },
  },

  computed: {
    height() {
      return `${this.elevatorHeight - 1}px`
    },
  },
}
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
      <button v-for="floor in floors" :key="floor"
              @click="addPendingCalls(floor), incrementTop(floor, $refs['elevator'])">
        <span>{{ floor }}</span>
        <ul>
          <li class="item inner-border"/>
        </ul>
      </button>
    </div>
  </div>
</template>

<style scoped>
@import '../assets/styles/style.css';
</style>