<template lang="html">
  <div v-if="city">
    <h2 :class="city.celsius > 10 ? 'warm' : 'cold'">{{city.name}}</h2>
    <div id="details">
      <p>{{city.weather}}</p>
      <p>{{city.celsius}} °C</p>
      <p>{{city.fahrenheit}} °F</p>
    </div>
      <canvas id="canvas" width="578" height="400">Current weather in {{city.name}} is {{city.weather}}</canvas>
  </div>
</template>

<script>
import {eventBus} from '../main.js';

export default {
  props: ['city'],
  data() {
    return {
      currentWeather: null
    }
  },
  mounted() {
    eventBus.$on('city-selected', (city) => {
      this.currentWeather = city.weather
      this.draw(this.currentWeather)
    })
  },
  methods: {
    draw: function(weather) {
        if (weather === "Clouds") {
          this.cloud()
      } else if (weather === "Thunderstorm") {
          this.thunderstorm()
      } else if (weather === "Clear") {
        this.sunny()
      // } else if (weather === "Rain") {
      //   this.rain()
      };
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
      }
      // rain: function() {
      //   this.cloud()
      //   let ctx = canvas.getContext('2d');
      //   ctx.save();
      //   this.drop(10, 10);
      //   ctx.moveTo(300, 500);
      //   ctx.lineTo(x, y - 7);
      //   ctx.lineTo(x + 5, y);
      //   ctx.arc(x, y, 5, 0, Math.PI);
      //   ctx.closePath();
      //   y = y + 3;
      //   if (y == canvas.height) {
      //     y = 0
      //   }
      //   let m = setTimeout('drop(' + x + ',' + y + ')', 20);
      // }
      // }
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
    width: 50rem;
    font-size: 1em;
  }

  #canvas {
    width: 30rem;
    margin-left: 30rem;
    margin-bottom: 1rem;
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
