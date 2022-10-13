<template>
  <div
    class="elevator"
    v-bind:style="{
      height: floorHeight,
      marginBottom: liftHeight,
      transition: transition,
    }"
  ></div>
</template>

<script>
import { toNumber } from "@vue/shared";

export default {
  props: {
    currentFloor: {
      type: Number,
      required: false,
    },
    floorHeight: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      latestFloor: 1,
      liftHeight: 0,
      transition: "all 0s linear",
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
      this.latestFloor = this.currentFloor;
    },
  },
};
</script>

<style scoped>
.elevator {
  width: 78px;
  display: flex;
  background-color: teal;
}
</style>
