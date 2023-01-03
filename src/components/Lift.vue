<template>
  <canvas id="canvas" width="55" height="500" ref="myCanvas"></canvas>
</template>

<script lang="ts">
export default {
  name: "Lift",
  mounted() {
    let canvas = document.getElementById("canvas") as HTMLCanvasElement; // получение холста в документе
    let ctx = canvas.getContext("2d") as CanvasRenderingContext2D; // получение контекста рисования
    let positionY = 420; // начальная позиция X координаты

    // window.setInterval(() => {
    //   ctx.beginPath();
    //   ctx.clearRect(0, 0, 55, 500); // очистка холста
    //   ctx.fillStyle = "#2F70AF"; // цвет заливки
    //   ctx.rect(0, positionY, 55, 80); // x, y, width, height
    //   ctx.fill();
    //   positionY--; // изменение позиции
    //   if (positionY < 0) {
    //     positionY = 420;
    //   }
    // }, 20);
  }
};
</script>

<style scoped>
#canvas {
  /* height: 500px;
  width: 400px; */
  border: 2px solid #00457e;
}
</style>
