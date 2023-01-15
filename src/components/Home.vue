<script>
export default {
  props: ['floors', 'elevators'],

  mounted() {
    for (let i = 0; i < this.elevators; i++) this.waitingCalls.push([]);
    for (let i = 0; i < this.elevators; i++) this.floorsGroup.push([]);

    this.elevatorsPosition = Array(this.elevators + 1).fill(0)
    this.elevatorHeight = this.$refs.elevators.clientHeight / this.floors;
  },

  data() {
    return {
      refButton: '',
      currentElevator: -1, // Current elevator
      elevatorHeight: 0, // Elevator height
      nearestMin: 0,
      elevatorsPosition: [], // Elevators position (starter position equal 0)
      waitingCalls: [],
      btnsActiveGroup: [], // Adding an active button
      floorsGroup: [],
    };
  },

  methods: {
    addPendingCalls(elevator, refButton, refElevator) {
      this.refButton = refButton;
      this.btnsActiveGroup.push(elevator - 1)

      if (this.currentElevator < this.elevators - 1) this.currentElevator += 1
      else this.currentElevator = 0

      this.floorsGroup[this.currentElevator].push(elevator)
      refElevator[this.currentElevator].textContent = this.floorsGroup[this.currentElevator][0]

      // Adding an Active Color to a Button
      this.btnsActiveGroup.forEach(i => { this.refButton[i].classList.add('btn-active') })

      this.waitingCalls[this.currentElevator].push(this.elevatorHeight * (elevator - 1))
      let elevators = [...Array.from(Array(this.elevators), (_, index) => index + 1)]
      this.nearestMin = elevators[this.currentElevator] - 1;
    },

    incrementTop(refElevator, nearestMins = this.nearestMin) {
      // Get the nearest minimum number for call elevator
      refElevator[nearestMins].style.transition = 'all 3s ease';
      // Elevator movement
      refElevator[nearestMins].style.bottom = this.elevatorsPosition[nearestMins] + this.waitingCalls[nearestMins][0] + 'px';

      // When elevator animation finished
      refElevator[nearestMins].ontransitionend = () => {
        const timeout = ms => new Promise(resolve => setTimeout(resolve, ms));

        timeout(0).then(() => {
          refElevator[nearestMins].classList.add('expectation')
          this.refButton[this.btnsActiveGroup[0]].classList.remove('btn-active')
          this.btnsActiveGroup.shift();
        });

        timeout(3000).then(() => {
          // Showing floor number
          this.floorsGroup[nearestMins].shift();
          refElevator[nearestMins].textContent = this.floorsGroup[nearestMins][0]

          this.waitingCalls[nearestMins].shift();
          // Remove the active button class when the floor is reached
          refElevator[nearestMins].classList.remove('expectation');
          this.incrementTop(refElevator, nearestMins);
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
      <hr v-for="floor in floors + 1" :key="floor" />
    </div>

    <div v-for="elevator in elevators" :key="elevator" class="mine">
      <div :style="{ height: height }" ref="elevator" class="elevator">
        <h3 style="font-size: 20px; background: red; font-weight: bold" />
        {{ elevator }}
      </div>
    </div>

    <div class="buttons">
      <button v-for="floor in floors" :key="floor" ref="button"
        @click="addPendingCalls(floor, $refs['button'], $refs['elevator']), incrementTop($refs['elevator'])">
        <span>{{ floor }}</span>
        <ul>
          <li class="item inner-border" />
        </ul>
      </button>
    </div>
  </div>
</template>

<style scoped>
@import '../assets/styles/style.css';
</style>