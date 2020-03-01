<template lang="html">
  <div v-if="city">
    <h2 :class="city.celsius > 10 ? 'warm' : 'cold'">{{city.name}}</h2>
    <div id="details">
      <p>{{city.weather}}</p>
    </div>
    <div id="temp">
      <p>{{city.celsius}} °C</p>
      <p>{{city.fahrenheit}} °F</p>
    </div>
      <canvas id="canvas" width="700" height="600">Current weather in {{city.name}} is {{city.weather}}</canvas>
  </div>
</template>

<script>


import {eventBus} from '../main.js';
let start = null;

export default {
  props: ['city'],
  data() {
    return {
      currentWeather: null,
      rectY: 300,
      rectW: 200,
      rectX: 50,
      drops: [],
    }
  },
  mounted() {
    requestAnimationFrame(this.anim),
    eventBus.$on('city-selected', (city) => {
      this.currentWeather = city.weather
      // this.draw(this.currentWeather)

      // If weather = rain, create an array of raindrops
    })
  },
  methods: {
    anim: function(timestamp) {
      if (!start) start = timestamp;
      console.log('anim frame')
      let context = canvas.getContext('2d');
      context.clearRect(0, 0, canvas.width, canvas.height);

      // Update the x / y position of the raindrop
      // This will become a for loop to update EACH drop
      this.rectY += 1;
      if (this.rectY > canvas.height) {
        this.rectY = 0;
      }

      // After doing the position update, call the right draw method to draw the weather
      if (this.currentWeather === "Clouds") {
        this.cloud();
      } else if (this.currentWeather === "Thunderstorm") {
          this.thunderstorm()
      } else if (this.currentWeather === "Clear") {
        this.sunny()
      } else if (this.currentWeather === "Rain") {
        // When multipe drops, loop through each and call this.rain(x, y) for each drop
        this.rain(this.rectX, this.rectY)
      }

      // Don't touch this
      if ((timestamp - start) < 5000) {
        start = null;
        requestAnimationFrame(this.anim)
      }
    },
      cloud: function() {
        let context = canvas.getContext('2d');
        context.clearRect(0, 0, canvas.width, canvas.height);
        context.beginPath();
        context.moveTo(170, 80);
        context.bezierCurveTo(110, 100, 130, 150, 230, 150);
        context.bezierCurveTo(250, 150, 300, 170, 340, 150);
        context.bezierCurveTo(450, 170, 460, 120, 410, 90);
        context.bezierCurveTo(400, 60, 370, 40, 350, 50);
        context.bezierCurveTo(320, 10, 270, 10, 240, 50);
        context.bezierCurveTo(200, 30, 180, 50, 170, 80);
        // context.translate(105, 0);
        context.closePath();
        context.fillStyle = '#D3D3D3';
        context.fill();
      },
      thunderstorm: function() {
        let context = canvas.getContext('2d');
        context.clearRect(0, 0, canvas.width, canvas.height);
        context.beginPath();
        context.moveTo(170, 80);
        context.bezierCurveTo(120, 100, 130, 150, 230, 150);
        context.bezierCurveTo(250, 150, 300, 170, 340, 150);
        context.bezierCurveTo(450, 170, 460, 120, 410, 90);
        context.bezierCurveTo(400, 60, 370, 40, 350, 50);
        context.bezierCurveTo(320, 10, 270, 10, 240, 50);
        context.bezierCurveTo(200, 30, 180, 50, 170, 80);
        context.closePath();
        context.fillStyle = '#404040';
        context.fill();
      },
      sunny: function() {
        let context = canvas.getContext('2d');
        context.clearRect(0, 0, canvas.width, canvas.height);
        context.beginPath();
        context.arc(250, 150, 125, 0, 2 * Math.PI);
        context.fillStyle = '#ffcc00';
        context.fill();
      },
      rain: function (posX, posY) {
        let context = canvas.getContext('2d');

        context.beginPath();
        context.moveTo(this.rectX - 5, this.rectY);
        context.lineTo(this.rectX, this.rectY - 7);
        context.lineTo(this.rectX + 5, this.rectY);
        context.arc(this.rectX, this.rectY, 5, 0, Math.PI);

        context.closePath();
        context.fillStyle = '#404040';
        context.fill();
      }

  }
}

</script>

<style lang="css" scoped>
  h2 {
    text-align: center;
    font-size: 1.2em;
    border: 3px dotted #89B802;
    padding: 3px 3px;
    border-radius: 15px;
    margin-left: 30rem;
    margin-right: 30rem;
  }

  #details {
    text-align: center;
    font-size: 1em;
    margin-top: 2rem;
    margin-bottom: 0;
  }

  #temp {
    text-align: center;
    font-size: 1.6em;
    margin-left: 40rem;
    display: block;
  }

  #canvas {
    width: 30rem;
    margin-left: 30rem;
    margin-bottom: 10rem;
    border: 2px solid red;
  }

  .cold {
    transition: border 0.8s ease;
    border: 3px dotted #b5caeb;
  }

  .warm {
    transition: border 0.8s ease;
    border: 3px dotted #89B802;
  }

</style>
