<script setup lang="ts">
import { ref } from "vue";

const canvas = ref<HTMLCanvasElement>();
let isMouseClicked = false;
const size = ref(5);
let color = "#111";
let x1 = 0
let y1 = 0;
let selectedShape = "circle";



const DrawCircle = (x: number, y: number) => {
  let ctx = canvas.value?.getContext("2d");
  if (ctx) {
    ctx.beginPath();
    ctx.arc(x, y, size.value, 0, 2 * Math.PI);
    ctx.fillStyle = color;
    ctx.fill()
  }
}


const onMouseDown = (e: MouseEvent) => {
  x1 = e.offsetX
  y1 = e.offsetY
  isMouseClicked = true
}
const onMouseUp = () => {
  x1 = 0;
  y1 = 0;
  isMouseClicked = false
}
const onMouseMove = (e: MouseEvent) => {
  if (isMouseClicked) {

    if (selectedShape == 'circle') {
      let x = e.offsetX;
      let y = e.offsetY;
      DrawCircle(x, y);
    }

    if (selectedShape == 'line') {
      let x2 = e.offsetX
      let y2 = e.offsetY
      drawLine(x1, y1, x2, y2);
      x1 = x2;
      y1 = y2;
    }
  }
}

const increase = () => {
  size.value += 5
  if (size.value > 50) {
    size.value = 50
  }
}

const decrease = () => {
  size.value -= 5
  if (size.value < 5) {
    size.value = 5
  }
}

const handleColorChange = (e: Event) => {
  let eventTarget = e.target as HTMLInputElement
  color = eventTarget.value
}

const clearCanvas = () => {
  let ctx = canvas.value?.getContext("2d");
  if (ctx && canvas.value) {
    ctx.fillStyle = "#fff";
    ctx.fillRect(0, 0, canvas.value.width, canvas.value.height);
  }
}

const drawLine = (x1: number, y1: number, x2: number, y2: number) => {
  let ctx = canvas.value?.getContext("2d");
  if (ctx) {
    ctx.beginPath();
    ctx.moveTo(x1, y1);
    ctx.lineTo(x2, y2);
    ctx.lineWidth = size.value;
    ctx.strokeStyle = color
    ctx.stroke();
  }
}

const handleShapeSelection = (shape: string) => {
  selectedShape = shape;
}
</script>

<template>
  <div class="toolbar">
    <button @click="increase">+</button>
    <span>{{ size }}</span>
    <button @click="decrease">-</button>
    <input type="color" @change="handleColorChange" />
    <button @click="handleShapeSelection('circle')">&#9899;</button>
    <button @click="handleShapeSelection('line')">&#9866;</button>
    <button @click="clearCanvas">&#9746;</button>
  </div>
  <canvas
    ref="canvas"
    height="500"
    width="500"
    @mousedown="onMouseDown"
    @mousemove="onMouseMove"
    @mouseup="onMouseUp"
  ></canvas>
</template>

<style>
body {
  margin: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
}

canvas {
  border: 1px solid black;
}

.toolbar {
  width: 480px;
  background-color: rgb(68, 68, 148);
  padding: 10px;
  display: flex;
}

.toolbar * {
  margin-right: 10px;
  height: 30px;
  width: 30px;
  background-color: #fff;
}

.toolbar span {
  display: inline-flex;
  align-items: center;
  justify-content: center;
}

.toolbar *:last-child {
  margin-left: auto;
}
</style>
