<template>
  <div class="app">
    <ul class="lifts">
      <li v-for="lift in lifts" v-bind:key="lift.id">
        <Lift
          :liftData="lift"
          :setActiveLiftState="setActiveLiftState"
          :setLiftState="setLiftState"
        />
      </li>
    </ul>
    <ul class="buttons">
      <li v-for="btn in buttons" v-bind:key="btn.id">
        <Button :data="btn" :setActiveBtnState="setActiveBtnState" />
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import Lift from "@/components/Lift/Lift.vue";
import Button from "@/components/Button/Button.vue";
import { BtnType } from "@/types/BtnType";
import { LiftType } from "@/types/LiftType";
import { defineComponent } from "vue";

export default defineComponent({
  name: "App",
  components: {
    Lift,
    Button
  },
  data: () => {
    return {
      lifts: [
        { id: "0", floor: 1, active: false, state: false },
        { id: "1", floor: 1, active: false, state: false },
        { id: "2", floor: 1, active: false, state: false },
        { id: "3", floor: 1, active: false, state: false }
      ] as LiftType[],
      buttons: [
        { id: "14", name: 5, active: false },
        { id: "13", name: 4, active: false },
        { id: "12", name: 3, active: false },
        { id: "11", name: 2, active: false },
        { id: "10", name: 1, active: false }
      ] as BtnType[]
    };
  },
  methods: {
    setActiveBtnState(id: string): void {
      for (const button of this.buttons) {
        if (button.id === id) {
          button.active = !button.active;
          if (button.active) {
            for (const lift of this.lifts) {
              if (!lift.active && !lift.state) {
                lift.active = true;
                lift.floor = button.name;
                break;
              }
            }
          }
          break;
        }
      }
    },

    setActiveLiftState(id: string): void {
      for (const lift of this.lifts) {
        if (lift.id === id) {
          lift.active = false;
          for (const button of this.buttons) {
            if (button.name === lift.floor) {
              button.active = false;
              break;
            }
          }
          break;
        }
      }
    },

    setLiftState(id: string): void {
      for (const lift of this.lifts) {
        if (lift.id === id) {
          lift.state = !lift.state;
          break;
        }
      }
    }
  }
});
</script>

<style lang="scss" src="@/app.scss" scoped></style>
