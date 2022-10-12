<template>
  <div class="app">
    <div class="container">
      <elevator-shaft
        :floors="floors"
        :floorHeight="floorHeight"
        v-for="elevator in elevators"
      />
      <div>
        <floor
          v-for="floor in floors"
          :floor="1 + floors - floor"
          :floorHeight="floorHeight"
          @elevatorCall="addToElevatorQueue"
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
      floors: 10, // Количество этажей
      elevators: 3, // Количество лифтов
      floorHeight: "100%",
      elevatorArray: [],
      elevatorQueue: [],
    };
  },
  methods: {
    callFreeElevator() {
      let nearestToFloor = -1;

      let entryQueueFloor = -1;
      for (let i = 0; i < this.elevatorQueue.length; i++) {
        if (this.elevatorQueue[i][1]) {
          entryQueueFloor = i;
          break;
        }
      }
      if (entryQueueFloor != -1) {
        for (let i = 0; i < this.elevators; i++) {
          if (this.elevatorArray[i][1]) {
            nearestToFloor = i;
            break;
          }
        }
        if (nearestToFloor != -1) {
          for (let i = nearestToFloor + 1; i < this.elevators; i++) {
            if (
              Math.abs(
                this.elevatorArray[nearestToFloor][0] -
                  this.elevatorQueue[entryQueueFloor][0]
              ) >
                Math.abs(
                  this.elevatorArray[i][0] -
                    this.elevatorQueue[entryQueueFloor][0]
                ) &&
              this.elevatorArray[i][1]
            ) {
              nearestToFloor = i;
            }
          }
          this.elewatorOnTheWay(nearestToFloor, entryQueueFloor);
        }
      }
    },
    elewatorOnTheWay(elevator, queuePosition) {
      console.log(
        "лифт",
        elevator,
        "с этажа",
        this.elevatorArray[elevator][0],
        "едет к этажу",
        this.elevatorQueue[queuePosition][0]
      );
      let timeout =
        1000 *
        Math.abs(
          this.elevatorArray[elevator][0] - this.elevatorQueue[queuePosition][0]
        );

      this.elevatorQueue[queuePosition][1] = false;
      this.elevatorArray[elevator] = [
        this.elevatorQueue[queuePosition][0],
        false,
      ];
      setTimeout(() => {
        console.log("лифт", elevator, "приехал");
        this.elevatorQueue[queuePosition][0]=0
        setTimeout(() => {
          console.log("лифт", elevator, "готов к отправке");
          this.elevatorArray[elevator][1] = true
          console.log(this.elevatorArray, this.elevatorQueue);
        }, 3000);
      }, timeout);
    },
    addToElevatorQueue(floor) {
      let flag = true;
      for (let i = 0; i < this.elevatorQueue.length; i++) {
        if (this.elevatorQueue[i][0] === floor) {
          flag = false;
          break;
        }
      }
      // если этаж есть в очереди ничего не делаем
      if (flag) {
        console.log("этаж", floor, "добавлен в очередь");
        this.elevatorQueue.push([floor, true]); // этаж, нужен ли на нем лифт
        this.callFreeElevator();
      }
    },
  },
  mounted() {
    this.floorHeight = 100 / this.floors + "%"; // Люблю джаваскрипт =)
    for (let i = 0; i < this.elevators; i++) {
      this.elevatorArray.push([1, true]); // Первое - этаж, второе - состояние
    }
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
