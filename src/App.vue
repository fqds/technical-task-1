<template>
  <div class="app">
    <div class="container">
      <elevator-shaft :floors="floors" :floorHeight="floorHeight" v-for="elevator in elevators" />
      <div>
        <floor
          v-for="floor in floors"
          :floor="floor"
          :floorHeight="floorHeight"
        />
        <my-hr />
      </div>
    </div>
  </div>
</template>

<script>
import ElevatorShaft from "@/components/ElevatorShaft";
import Floor from "@/components/Floor";

export default {
  components: { ElevatorShaft, Floor },
  data() {
    return {
      floors: 7, // Количество этажей
      elevators: 7, // Количество лифтов
      floorHeight: "100%",
    };
  },
  methods: {
    elevatorCall(floor) {
      console.log(floor);
    },
  },
  mounted() {
    this.floorHeight = 100 / this.floors + "%"; // люблю джаваскрипт =
    console.log(this.floorHeight, this.floors);
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.container {
  height: 100vh;
  padding: 15px 20px;
  display: flex;
  flex-direction: row;
}
</style>
