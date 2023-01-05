<template>
  <canvas
    :id="liftData.id"
    width="55"
    height="400"
    :class="liftData.state ? 'blink' : ''"
  ></canvas>
</template>

<script lang="ts">
import { LiftType } from "@/types/LiftType";
import { onMounted, PropType, watch } from "vue";
import { defineComponent } from "vue";

export default defineComponent({
  name: "Lift",
  props: {
    liftData: { type: Object as PropType<LiftType>, required: true },
    setActiveLiftState: { type: Function, required: true },
    setLiftState: { type: Function, required: true }
  },
  setup(props) {
    let positionY = 320;
    let floor = 1;

    const createLift = (): void => {
      let canvas = document.getElementById(
        props.liftData.id
      ) as HTMLCanvasElement;
      let ctx = canvas.getContext("2d") as CanvasRenderingContext2D;

      ctx.beginPath();
      ctx.clearRect(0, 0, 55, 400); // очистка холста
      ctx.fillStyle = "#2F70AF";
      ctx.rect(0, positionY, 55, 80); // x, y, width, height
      ctx.fill();
    };

    const moveLift = (position: number, direction: string): void => {
      let startLift = setInterval(() => {
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
        }
      });
    };

    onMounted(() => {
      createLift();
    });

    watch(
      () => props.liftData.active,
      () => {
        if (props.liftData.active && floor !== props.liftData.floor) {
          switch (props.liftData.floor) {
            case 2:
              if (floor > props.liftData.floor) {
                moveLift(242, "down");
              } else {
                moveLift(240, "up");
              }
              break;
            case 3:
              if (floor > props.liftData.floor) {
                moveLift(161, "down");
              } else {
                moveLift(159, "up");
              }
              break;
            case 4:
              if (floor > props.liftData.floor) {
                moveLift(80, "down");
              } else {
                moveLift(78, "up");
              }
              break;
            case 5:
              if (floor > props.liftData.floor) {
                moveLift(0, "down");
              } else {
                moveLift(-2, "up");
              }
              break;
            case 1:
            default:
              if (floor > props.liftData.floor) {
                moveLift(322, "down");
              }
          }
        }
      }
    );
  }
});
</script>

<style lang="scss" src="./lift.scss" scoped></style>
