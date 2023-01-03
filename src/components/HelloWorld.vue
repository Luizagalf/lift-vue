<template>
  <canvas id="canvas" width="600" height="300"></canvas>
</template>

<script lang="ts">
let canvas = document.getElementById("canvas") as HTMLCanvasElement; // получение холста в документе
let ctx = canvas.getContext("2d") as CanvasRenderingContext2D; // получение контекста рисования
let positionX = 75; // начальная позиция X координаты

window.setInterval(() => {
  ctx.beginPath();
  ctx.clearRect(0, 0, 600, 300); // очистка холста
  ctx.fillStyle = "pink"; // цвет заливки
  ctx.rect(positionX, 75, 75, 75); // x, y, width, height
  ctx.fill();
  positionX++; // изменение позиции
  if (positionX > 600) {
    positionX = 75;
  }
}, 60); // запуск каждые 60 милисекунд
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#canvas {
  width: 600px;
  height: 300px;
  margin: 40px;
  border: 2px solid gray;
}
</style>
