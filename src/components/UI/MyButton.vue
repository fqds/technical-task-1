<template>
  <button class="btn">
    <div class="btn__circleBorder">
      <div class="btn__circle"></div>
    </div>
  </button>
</template>

<script>
export default {
  name: "my-button",
};
</script>

<style scoped>
.btn {
  padding: 5px;
  border: 1px solid teal;
  display: flex;
}
.btn:hover {
  background-color: rgb(209, 209, 209);
  transition: 0.2s;
}
.btn__circle {
  padding: 4px;
  border-radius: 100%;
  border: 1px solid teal;
  background-color: teal;
}
.btn__circleBorder {
  padding: 2px;
  border-radius: 100%;
  border: 1px solid teal;
}
</style>
