<template>
  <div class="elevator" v-bind:style="{ height: floorHeight }"></div>
</template>

<script>
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
};
</script>

<style scoped>
.elevator {
  width: 78px;
  display: flex;
  background-color: teal;
}
</style>
