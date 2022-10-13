<template>
  <div id="blink" class="elevator__status">
    <span v-if="direction" class="material-symbols-outlined">
      arrow_upward
    </span>
    <span v-else class="material-symbols-outlined"> arrow_downward </span>
    {{ floor }}
  </div>
</template>

<script>
export default {
  props: {
    direction: {
      type: Boolean,
      required: true,
    },
    floor: {
      type: Number,
      required: true,
    },
  },
};
</script>

<style scoped>
.elevator__status {
  padding: 5px 10px;
  border-radius: 5px;
  background-color: black;
  color: white;
}
.material-symbols-outlined {
  color: white;
  font-size: 12px;
}
#blink {
  animation: blink 2s linear infinite;
}
@keyframes blink {
  0% { background-color: rgba(34, 34, 34, 0.8); }
  50% { background-color: rgba(34, 34, 34, 0); }
  100% { background-color: rgba(34, 34, 34, 0.8); }
}
</style>
