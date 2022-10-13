<template>
  <div class="elevator__shaft">
    <elevator
      :currentFloor="currentFloor"
      :isMoving="isMoving"
      :floorHeight="floorHeight"
      :isWaiting="isWaiting"
    />
  </div>
</template>

<script>
import Elevator from "@/components/Elevator";
export default {
  components: { Elevator },
  props: {
    currentFloor: {
      type: Number,
      required: false,
    },
    floorHeight: {
      type: String,
      required: true,
    },
    floors: {
      type: Number,
      required: true,
    },
    isMoving: {
      type: Boolean,
      required: true,
    },
    isWaiting: {
      type: Boolean,
      required: true,
    },
  },
};
</script>

<style scoped>
.elevator__shaft {
  margin-left: 20px;
  width: 80px;
  height: 100%;
  display: flex;
  flex-direction: column-reverse;
  border-right: 1px solid rgb(85, 85, 85);
  border-left: 1px solid rgb(85, 85, 85);
}
</style>
