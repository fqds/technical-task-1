<template>
  <button class="btn" v-bind:style="{ borderColor: color }">
    <div class="btn__circleBorder" v-bind:style="{ borderColor: color }">
      <div
        class="btn__circle"
        v-bind:style="{ borderColor: color, backgroundColor: color }"
      ></div>
    </div>
  </button>
</template>

<script>
export default {
  props: {
    color: {
      type: String,
      default: "teal",
    },
  },
  name: "my-button",
};
</script>

<style scoped>
.btn {
  padding: 5px;
  border: 1px solid;
  display: flex;
}
.btn:hover {
  background-color: rgb(209, 209, 209);
  transition: 0.2s;
}
.btn__circle {
  padding: 4px;
  border-radius: 100%;
  border: 1px solid;
  background-color: teal;
}
.btn__circleBorder {
  padding: 2px;
  border-radius: 100%;
  border: 1px solid;
}
</style>
