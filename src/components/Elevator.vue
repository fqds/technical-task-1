<template>
  <div
    class="elevator"
    v-bind:style="{
      height: floorHeight,
      marginBottom: liftHeight,
      transition: transition,
    }"
  >
    <elevator-status-bar
      v-if="isMoving"
      :floor="currentFloor"
      :direction="direction"
      style="margin-top: 10px"
    />
  </div>
</template>

<script>
import ElevatorStatusBar from "@/components/ElevatorStatusBar";

export default {
  components: {
    ElevatorStatusBar,
  },
  props: {
    currentFloor: {
      type: Number,
      required: false,
    },
    floorHeight: {
      type: String,
      required: true,
    },
    isMoving: {
      type: Boolean,
      required: true,
    },
  },
  data() {
    return {
      latestFloor: 1,
      liftHeight: 0,
      transition: "all 0s linear",
      direction: false,
    };
  },
  watch: {
    currentFloor() {
      this.liftHeight =
        (this.currentFloor - 1) *
          this.floorHeight.substring(0, this.floorHeight.length - 2) +
        "vh";
      this.transition =
        "all " + Math.abs(this.latestFloor - this.currentFloor) + "s linear";
      if (this.currentFloor - this.latestFloor > 0) {
        this.direction = true;
      } else {
        this.direction = false;
      }
      this.latestFloor = this.currentFloor;
    },
  },
};
</script>

<style scoped>
.elevator {
  width: 78px;
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: teal;
}
</style>
