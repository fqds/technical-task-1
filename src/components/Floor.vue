<template>
  <div class="floor" v-bind:style="{ height: floorHeight }">
    <my-hr />
    <div class="floor__container">
      <h3>{{ floor }}</h3>
      <div class="floor__btn">
        <my-button @click="buttonPressed" v-if="!isButtonPressed"></my-button>
        <my-button v-else :color="'darkorange'"></my-button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    floor: {
      type: Number,
      required: true,
    },
    floorHeight: {
      type: String,
      required: true,
    },
    isButtonPressed: {
      type: Boolean,
      required: true,
    },
  },
  methods: {
    buttonPressed() {
      this.$emit("elevatorCall", this.floor);
    },
  },
};
</script>

<style scoped>
.floor {
  width: 60px;
  display: block;
}
.floor__container {
  padding-top: 5px;
  display: flex;
  align-items: center;
  flex-direction: column;
  justify-content: center;
}
.floor__btn {
  padding-top: 3px;
}
</style>
