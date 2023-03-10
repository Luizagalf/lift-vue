<template>
  <div class="app">
    <ul class="buttons">
      <li v-for="btn in buttons" v-bind:key="btn.id">
        <Button :data="btn" :setActiveBtnState="setActiveBtnState" />
      </li>
    </ul>
    <ul
      class="lifts"
      :style="{
        height: `${buttons.length * 80}px`
      }"
    >
      <li v-for="lift in lifts" v-bind:key="lift.id">
        <Lift
          :liftData="lift"
          :buttons="buttons"
          :setActiveLiftState="setActiveLiftState"
          :setLiftState="setLiftState"
        />
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import Lift from "@/components/Lift/Lift.vue";
import Button from "@/components/Button/Button.vue";
import { BtnType } from "@/types/BtnType";
import { LiftType } from "@/types/LiftType";
import { CallType } from "@/types/CallType";
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
        { id: "3", floor: 1, active: false, state: false },
        { id: "4", floor: 1, active: false, state: false },
        { id: "5", floor: 1, active: false, state: false }
      ] as LiftType[],
      buttons: [
        { id: "16", name: 7, active: false },
        { id: "15", name: 6, active: false },
        { id: "14", name: 5, active: false },
        { id: "13", name: 4, active: false },
        { id: "12", name: 3, active: false },
        { id: "11", name: 2, active: false },
        { id: "10", name: 1, active: false }
      ] as BtnType[],
      calls: [] as CallType[]
    };
  },
  mounted() {
    if (localStorage.getItem("lifts")) {
      this.lifts = JSON.parse(localStorage.getItem("lifts") as string);
      for (const lift of this.lifts) {
        lift.state = false;
      }
    }
    if (localStorage.getItem("buttons")) {
      this.buttons = JSON.parse(localStorage.getItem("buttons") as string);
    }
    if (localStorage.getItem("calls")) {
      this.calls = JSON.parse(localStorage.getItem("calls") as string);
    }
  },
  methods: {
    setActiveBtnState(id: string): void {
      for (const button of this.buttons) {
        if (button.id === id) {
          let newCall = true;

          for (const call of this.calls) {
            if (call.floor === button.name) {
              newCall = false;
            }
          }

          for (const lift of this.lifts) {
            if (lift.floor === button.name) {
              newCall = false;
            }
          }

          if (newCall) {
            this.calls.push({ floor: button.name, active: true });
            button.active = true;
            this.takeNewCall();
          }
          break;
        }
      }
      this.setLocalStorage();
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
      this.setLocalStorage();
    },

    setLiftState(id: string): void {
      for (const lift of this.lifts) {
        if (lift.id === id) {
          lift.state = !lift.state;
          this.takeNewCall();
          break;
        }
      }
      this.setLocalStorage();
    },

    takeNewCall(): void {
      for (const lift of this.lifts) {
        if (!lift.state && !lift.active && this.calls.length) {
          lift.active = true;
          lift.floor = this.calls[0].floor;
          this.calls.shift();
          break;
        }
      }
      this.setLocalStorage();
    },

    setLocalStorage(): void {
      localStorage.setItem("lifts", JSON.stringify(this.lifts));
      localStorage.setItem("buttons", JSON.stringify(this.buttons));
      localStorage.setItem("calls", JSON.stringify(this.calls));
    }
  }
});
</script>

<style lang="scss" src="@/app.scss" scoped></style>
