<script>
export default {
  props: [ 'floors', 'elevators' ],

  mounted() {
    for (let i = 0; i <= this.elevators; i++) {
      this.elevatorsPosition.push(0)
    }
    this.elevatorHeight = this.$refs.elevators.clientHeight / this.floors;
  },

  data() {
    return {
      elevatorsPosition: [], // Elevators position (starter position equal 0)
      elevatorHeight: 0, // Elevator height
    };
  },

  methods: {
    incrementTop(elevator, ref) {

      // Get the nearest minimum number
      let nearestMin = Math.min(...Array.from(Array(this.elevators), (_, index) => index + 1))

      let currentElevatorPosition = this.elevatorsPosition[nearestMin] + this.elevatorHeight * (elevator - 1);

      ref[nearestMin - 1].style.bottom = currentElevatorPosition + 'px';
    },

    transitionEnd() {
      console.log('Finished')
    },
  },

  computed: {
    toUp() {
      return {
        height: this.elevatorHeight - 1 + 'px'
      };
    },
  },
};
</script>

<template>
  <div class="elevators" ref="elevators">

    <div class="floors">
      <hr v-for="floor in floors + 1" :key="floor"/>
    </div>

    <transition-group
        name="fade"
        tag="div"
        enter-active-class="animated fadeIn" leave-active-class="animated fadeOut"
        style="display: flex">

      <div v-for="elevator in elevators" :key="elevator" class="mine">
        <transition name="fade">
          <div @transitionend="transitionEnd" :style="toUp" ref="elevator" class="elevator">
            {{ elevator }}
          </div>
        </transition>
      </div>

    </transition-group>

    <div class="buttons">
      <button v-for="floor in floors" :key="floor"
              @click="incrementTop(floor, $refs['elevator'])">
        Up {{ floor }}
      </button>
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
  box-sizing: border-box;
  border: 1px solid;
  transition: 3s ease;
}

.floors {
  width: 100%;
  position: absolute;
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.floors hr {
  width: 100%;
  margin: 0;
  color: yellow;
}

.buttons {
  display: flex;
  flex-direction: column-reverse;
  justify-content: space-around;
  z-index: 1;
}

.buttons button {
  padding: 6px 16px;
  border: none;
  cursor: pointer;
}
</style>