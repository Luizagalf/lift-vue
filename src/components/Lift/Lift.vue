<template>
  <canvas
    :id="liftData.id"
    v-bind:width="55"
    v-bind:height="buttons.length * 80"
    :class="liftData.state ? 'blink' : ''"
  ></canvas>
</template>

<script lang="ts">
import { LiftType } from "@/types/LiftType";
import { onMounted, PropType, watch } from "vue";
import { defineComponent } from "vue";
import { BtnType } from "@/types/BtnType";

export default defineComponent({
  name: "Lift",
  props: {
    liftData: { type: Object as PropType<LiftType>, required: true },
    buttons: { type: Object as PropType<BtnType[]>, required: true },
    setActiveLiftState: { type: Function, required: true },
    setLiftState: { type: Function, required: true }
  },

  setup(props): void {
    let positionY = (props.buttons.length - 1) * 80;
    let floor = 1;

    const createLift = (): void => {
      let canvas = document.getElementById(
        props.liftData.id
      ) as HTMLCanvasElement;
      let ctx = canvas.getContext("2d") as CanvasRenderingContext2D;

      ctx.beginPath();
      ctx.clearRect(0, 0, 55, props.buttons.length * 80);
      ctx.fillStyle = "#2F70AF";
      ctx.rect(0, positionY, 55, 80);
      ctx.fill();

      props.buttons.forEach((button, index) => {
        if (index !== 0) {
          ctx.moveTo(0, index * 80);
          ctx.lineTo(55, index * 80);
          ctx.stroke();
          ctx.strokeStyle = "#00457e";
        }
      });

      ctx.font = "24px sans-serif";
      ctx.fillStyle = "#f6e8f2";
      ctx.fillText(
        `${
          props.liftData.active && !props.liftData.state
            ? floor > props.liftData.floor
              ? `▼ ${props.liftData.floor}`
              : `▲ ${props.liftData.floor}`
            : ""
        } `,
        3,
        positionY + 30
      );
    };

    const moveLift = (position: number, direction: string): void => {
      let startLift: number = setInterval((): void => {
        createLift();
        if (direction === "up") {
          positionY--;
        } else {
          positionY++;
        }

        if (positionY === position) {
          clearInterval(startLift);
          props.setActiveLiftState(props.liftData.id);
          props.setLiftState(props.liftData.id);
          floor = props.liftData.floor;

          setTimeout(() => {
            props.setLiftState(props.liftData.id);
          }, 3000);
          createLift();
        }
      });
    };

    onMounted((): void => {
      createLift();
    });

    watch(
      () => props.liftData.active,
      (): void => {
        if (props.liftData.active && floor !== props.liftData.floor) {
          if (floor > props.liftData.floor) {
            moveLift(
              props.buttons.length * 80 - props.liftData.floor * 80,
              "down"
            );
          } else {
            moveLift(
              props.buttons.length * 80 - props.liftData.floor * 80,
              "up"
            );
          }
        }
      }
    );
    watch(
      () => props.liftData.floor,
      (): void => {
        if (!props.liftData.active) {
          positionY = props.buttons.length * 80 - props.liftData.floor * 80;
          floor = props.liftData.floor;
          createLift();
        }
      }
    );
  }
});
</script>

<style lang="scss" src="./lift.scss" scoped></style>
