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

    const stopLift = (
      topPosition: number,
      bottomPosition: number,
      startLift: number
    ): void => {
      if (positionY > topPosition) {
        positionY--;
      }
      if (positionY < bottomPosition) {
        positionY++;
      }

      console.log(positionY, topPosition, bottomPosition);

      if (positionY === topPosition || positionY === bottomPosition) {
        clearInterval(startLift);
        props.setActiveLiftState(props.liftData.id);
        props.setLiftState(props.liftData.id);
        setTimeout(() => {
          props.setLiftState(props.liftData.id);
        }, 3000);
      }
    };

    onMounted(() => {
      createLift();
    });

    watch(
      () => props.liftData.active,
      () => {
        if (props.liftData.active) {
          let startLift = setInterval(() => {
            createLift();

            switch (props.liftData.floor) {
              case 2:
                stopLift(240, 160, startLift);
                break;
              case 3:
                stopLift(159, 80, startLift);
                break;
              case 4:
                stopLift(78, 0, startLift);
                break;
              case 5:
                stopLift(-3, -80, startLift);
                break;
              case 1:
              default:
                stopLift(321, 321, startLift);
                break;
            }
          }, 10);
        }
      }
    );
  }
});
</script>

<style lang="scss" src="./lift.scss" scoped></style>
