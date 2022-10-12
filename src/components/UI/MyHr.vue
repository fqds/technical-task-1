<template>
  <hr class="full-screen-hr" />
</template>

<script>
export default {
  name: "my-hr",
};
</script>

<style scoped>
.full-screen-hr {
    position: absolute;
    left: 20px;
    right: 20px;
    border-color: rgba(0, 0, 0, 0.2);
    z-index: -1;
}
</style>
