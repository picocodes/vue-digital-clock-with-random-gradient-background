<script setup>
  import { ref, onBeforeMount, onBeforeUnmount, computed } from 'vue'

  const hours = ref(0)
  const minutes = ref(0)
  const seconds = ref(0)
  const bgColor = ref([getDarkColor()]);

  function getDarkColor() {
    var color = '#';
    for (var i = 0; i < 6; i++) {
        color += Math.floor(Math.random() * 10);
    }console.log(color)
    return color;
	}

  // Generate random dark rgb color.
  function randomColor() {
      const r = Math.floor(Math.random() * 256)
      const g = Math.floor(Math.random() * 256)
      const b = Math.floor(Math.random() * 256)
      return `rgb(${r}, ${g}, 0)`
  }
  
  function prependZero( val ) {
    return val < 10 ? `0${val}` : `${val}`;
  }

  // Update the clock every second.
  let interval;

  onBeforeMount( () => {
    if ( ! interval ) {
      interval = setInterval(function() {
      const _date = new Date();
      hours.value = prependZero( _date.getHours() );
      minutes.value = prependZero( _date.getMinutes() );
      seconds.value = prependZero( _date.getSeconds() );
      bgColor.value.push( randomColor() );
        
      if ( bgColor.value.length > 4 ) {
        bgColor.value.shift();
      }
    }, 1000);
    }
  })
  
  onBeforeUnmount( () => {
    if ( interval ) {
      clearInterval(interval);
    }
  })

  const style = computed( () => {
     let gradient = bgColor.value.join(', ');

    return {
      backgroundImage: `radial-gradient(circle, ${gradient})`,
    }
  })
</script>

<template>
  <div class="wrapper" :style="style">
    <div class="container">
          <span id="hours" class="time">{{ hours }}</span>
          <span class="colon">:</span>
          <span id="minutes" class="time">{{ minutes }}</span>
          <span class="colon">:</span>
          <span id="seconds" class="time">{{ seconds }}</span>
      </div>
  </div>
</template>

<style scoped>
	body {
      margin: 0;
      padding: 0;
  }

  .wrapper {
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
  }
 
  .container {
      font-family: Arial, Helvetica, sans-serif;
      color: #fff;
      font-weight: bold;
      font-size: 70px;
      width: 550px;
      height: 150px;
      display: flex;
      align-items: center;
      justify-content: space-between;
  }

  .time {
      background-color: #fff;
      height: 80%;
      width: 150px;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 60px;
      color: #202020;
      border-radius: 5px;
      box-shadow: 0 15px 30px rgba(0, 0, 0, 0.3);
      letter-spacing: 3px;
  }

  .colon {
      font-weight: bolder;
      font-size: 60px;
      color: #fff;
  }

  @media (max-width: 500px) {
      html {
          font-size: 50%;
      }
      .container {
          margin: 0 10px;
      }
      .time {
          height: 60%;
          width: 100px;
      }
  }
</style>
