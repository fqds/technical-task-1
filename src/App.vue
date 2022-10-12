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
          this.elewatorOnTheWay(
            nearestToFloor,
            this.elevatorQueue[entryQueueFloor][0]
          );
        }
      }
    },
    elewatorOnTheWay(elevator, queueFloor) {
      console.log(
        "лифт",
        elevator,
        "с этажа",
        this.elevatorArray[elevator][0],
        "едет к этажу",
        queueFloor
      );
      let timeout =
        1000 * Math.abs(this.elevatorArray[elevator][0] - queueFloor);
      this.elevatorQueue[this.getQueueElementIndex(queueFloor)][1] = false;
      this.elevatorArray[elevator] = [queueFloor, false];
      setTimeout(() => {
        console.log("лифт", elevator, "приехал");

        this.elevatorQueue.splice(
          this.elevatorQueue[this.getQueueElementIndex(queueFloor)],
          1
        );
        setTimeout(() => {
          console.log("лифт", elevator, "готов к отправке");
          this.elevatorArray[elevator][1] = true;
          console.log(this.elevatorArray, this.elevatorQueue);
        }, 3000);
      }, timeout);
    },
    addToElevatorQueue(floor) {
      // если этаж есть в очереди ничего не делаем

      this.elevatorQueue.push([floor, true]);

      console.log(this.getQueueElementIndex(floor));

      if (this.getQueueElementIndex(floor) === this.elevatorQueue.length - 1) {
        console.log("этаж", floor, "добавлен в очередь");
        this.callFreeElevator();
      } else {
        this.elevatorQueue.splice(
          this.elevatorQueue[this.getQueueElementIndex(floor)],
          1
        );
      }
    },
    getQueueElementIndex(floor) {
      for (let i = 0; i < this.elevatorQueue.length; i++) {
        if (this.elevatorQueue[i][0] === floor) {
          return i;
        }
      }
      return -1;
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
