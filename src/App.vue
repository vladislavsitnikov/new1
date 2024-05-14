<template>
  <div class="container">
    <div class="square" :style="{ top: squareTop + 'px', left: squareLeft + 'px' }"></div>
    <button @click="toggleAnimation">{{ animationRunning ? 'Стоп' : 'Почати' }}</button>
  </div>
</template>

<script>
import { ref } from 'vue';
import flightData from './flight_data.json'; 

export default {
  setup() {
    const squareTop = ref(200);
    const squareLeft = ref(200); 
    const animationRunning = ref(false);
    let intervalId = null;
    let data = flightData; 

    const startFlight = () => {
      let index = Math.floor(Math.random() * data.length); 
      intervalId = setInterval(() => {
        const point = data[index];
        const speedPerMin = point.speed / 60; 
        const deltaX = Math.cos(point.direction * Math.PI / 180) * speedPerMin; 
        const deltaY = Math.sin(point.direction * Math.PI / 180) * speedPerMin;

        if (squareLeft.value + deltaX >= 0 && squareLeft.value + deltaX <= 380 &&
            squareTop.value + deltaY >= 0 && squareTop.value + deltaY <= 380) {
          squareLeft.value += deltaX;
          squareTop.value += deltaY;
        }

        if (++index === data.length) {
          index = 0; 
        }
      }, 1000);

      animationRunning.value = true;
    };

    const stopFlight = () => {
      clearInterval(intervalId);
      animationRunning.value = false;
      squareLeft.value = 200;
      squareTop.value = 200;
    };

    const toggleAnimation = () => {
      if (animationRunning.value) {
        stopFlight();
      } else {
        startFlight();
      }
    };

    return {
      squareTop,
      squareLeft,
      animationRunning,
      toggleAnimation,
    };
  }
};
</script>

<style>
.container {
  position: relative;
  width: 400px;
  height: 400px;
  border: 1px solid black;
  background-image: url(./assets/Безымянный.png);
}

.square {
  position: absolute;
  width: 34px;
  height: 34px;
  background-repeat: no-repeat;
  background-image: url(./assets/plane.png);
  transition: top 0.5s ease, left 0.5s ease;
  backdrop-filter: brightness(0.5);
}
</style>
