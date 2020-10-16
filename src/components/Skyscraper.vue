<template>
  <div v-for="(item, index) in skyscraper" :key="index">
    <input
      type="checkbox"
      v-for="(floor, idx) in item"
      :key="idx + 'floor'"
      v-model="item[idx]"
      @change.prevent="floorChange"
    />
  </div>
</template>
<script lang="ts">
import { reactive, computed } from "vue";
export default {
  setup() {
    // 初始建筑宽度
    const floorW = 5;
    // 当前楼层
    let activeL = 22;
    let time = 250;
    const core = 9;
    let startCore = 7;
    let walkingState = "right";
    const skyscraper: boolean[][] = reactive(
      JSON.parse(JSON.stringify(new Array(24).fill(new Array(18).fill(false))))
    );

    function goType(type: string) {
      if (type === "left") {
        startCore--;
        [
          skyscraper[activeL][startCore],
          skyscraper[activeL][startCore + floorW],
        ] = [true, false];
      } else if (type === "right") {
        startCore++;
        [
          skyscraper[activeL][startCore - 1],
          skyscraper[activeL][startCore + floorW - 1],
        ] = [false, true];
      }
    }

    function activeArchitecture() {
      setTimeout(() => {
        if (walkingState === "right") {
          if (startCore + floorW < 18) {
            goType("right");
          } else {
            walkingState = "left";
            goType("left");
          }
        } else {
          if (startCore > 0) {
            goType("left");
          } else {
            walkingState = "right";
            goType("right");
          }
        }
        activeArchitecture();
      }, time);
    }

    function reset() {
      for (let index = 0; index < floorW; index++) {
        skyscraper[skyscraper.length - 1][7 + index] = true;
        skyscraper[skyscraper.length - 2][7 + index] = true;
        console.log(skyscraper);
      }
    }
    function floorChange() {
      console.log(2);
      if (activeL > 0) {
        activeL--;
        time -= 10;
        console.log(time);
        
      }
    }
    reset();
    activeArchitecture();
    return {
      skyscraper,
      reset,
      floorChange,
    };
  },
};
</script>