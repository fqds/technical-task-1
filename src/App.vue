<template>
  <link
    rel="stylesheet"
    href="https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:opsz,wght,FILL,GRAD@20..48,100..700,0..1,-50..200"
  />
  <div class="app">
    <div class="container">
      <elevator-shaft
        v-for="elevator in elevatorArray"
        :floors="floors"
        :floorHeight="floorHeight"
        :currentFloor="elevator[0]"
        :isMoving="!elevator[1] && elevator[2] === 0"
        :isWaiting="!!elevator[2]"
      />
      <div>
        <floor
          v-for="floor in floorArray"
          :floor="floor[0]"
          :isButtonPressed="floor[1]"
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
      elevators: 5, // Количество лифтов
      floorHeight: "96vh",
      elevatorArray: [],
      elevatorQueue: [],
      floorArray: [],
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
      this.elevatorArray[elevator] = [queueFloor, false, 0];
      setTimeout(() => {
        console.log("лифт", elevator, "приехал");

        this.floorArray[this.floors - queueFloor][1] = false;
        this.waitOnThisFloor(elevator, queueFloor);
      }, timeout);
    },
    waitOnThisFloor(elevator, floor) {
      this.elevatorArray[elevator][1] = false;
      this.elevatorArray[elevator][2] += 1;
      setTimeout(() => {
        this.elevatorArray[elevator][2] -= 1;
        if (!this.elevatorArray[elevator][2]) {
          console.log("лифт", elevator, "готов к отправке");
          this.elevatorArray[elevator][1] = true;
          this.elevatorQueue.splice(
            this.elevatorQueue[this.getQueueElementIndex(floor)],
            1
          );
          for (let i = 0; i < this.elevatorQueue.length; i++) {
            if (this.elevatorQueue[i][1]) {
              this.elewatorOnTheWay(elevator, this.elevatorQueue[i][0]);
              break;
            }
            console.log("эллементу очереди ", i, "лифт не требуется");
          }
        }
      }, 3000);
    },
    addToElevatorQueue(floor) {
      // если этаж есть в очереди ничего не делаем

      console.log(this.getQueueElementIndex(floor), this.elevatorQueue);

      let flag = true;
      for (let i = 0; i < this.elevators; i++) {
        if (this.elevatorArray[i][0] === floor) {
          this.waitOnThisFloor(i);
          flag = false;
          break;
        }
      }
      if (flag) {
        this.elevatorQueue.push([floor, true]);
        console.log("этаж", floor, "добавлен в очередь");
        this.floorArray[this.floors - floor][1] = true;
        this.callFreeElevator();
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
    this.floorHeight = 96 / this.floors + "vh"; // Люблю джаваскрипт =)
    for (let i = 0; i < this.elevators; i++) {
      this.elevatorArray.push([1, true, 0]); // Первое - этаж, второе - готовость к отправке, третье - isWaiting
    }
    for (let i = 0; i < this.floors; i++) {
      this.floorArray.push([this.floors - i, false]);
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
  padding: 2vh 20px;
  display: flex;
  flex-direction: row;
}
</style>
