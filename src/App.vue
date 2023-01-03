<template>
  <div class="app">
    <Lift />
    <ul>
      <li v-for="btn in buttons" v-bind:key="btn.id">
        <Button :data="btn" :action="setActiveState(btn.id)" />
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import { Options, Vue } from "vue-class-component";
import Lift from "@/components/Lift.vue";
import Button from "@/components/Button.vue";
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
        { id: 0, floor: 1, active: false },
        { id: 1, floor: 1, active: false },
        { id: 2, floor: 1, active: false },
        { id: 3, floor: 1, active: false }
      ] as LiftType[],
      buttons: [
        { id: 10, name: 1, active: false },
        { id: 11, name: 2, active: false },
        { id: 12, name: 3, active: false },
        { id: 13, name: 4, active: false },
        { id: 14, name: 5, active: false }
      ] as BtnType[],
      value: 0 as number
    };
  },
  methods: {
    setActiveState(id: number) {
      this.buttons.forEach((button: BtnType) => {
        if (button.id === id) {
          button.active = !button.active;
        }
      });
    }
  }
});
</script>

<style lang="scss" src="@/app.scss" scoped></style>
