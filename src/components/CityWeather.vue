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
      // flakes: [],
      // start: null
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
      // } else if (weather === "Snow") {
      //   this.snow()
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
      // addFlake: function() {
      //   let context = canvas.getContext('2d');
      //   let x = Math.floor(Math.random() * canvas.width) + 1;
      //   let y = 0;
      //   let s = Math.floor(Math.random() * 3) + 1;
      //   this.flakes.push({"x":x, "y":y, "s":s});
      // },
      // snow: function() {
      //   window.requestAnimationFrame(this.step);
      //   let context = canvas.getContext('2d');
      //   this.addFlake();
      //   for (let i = 0; i < this.flakes.length; i++) {
      //     context.fillStyle = "rgba(255,255,255,.75)";
      //     context.beginPath();
      //     context.arc(this.flakes[i].x, this.flakes[i].y+=this.flakes[i].s*.5, this.flakes[i].s*.5, 0, Math.PI*2, false);
      //     context.fill();
      //       if (this.flakes[i].y > canvas.height) {
      //         this.flakes.splice(i, 1);
      //       }
      //   }
      // },
      // step: function(timestamp) {
      //   if (!this.start) this.start = timestamp;
      //   let progress = timestamp - this.start;
      //   let element = this.cloud();
      //   element.style.transform = 'translateX(' + Math.min(progress / 10, 200) + 'px)';
      //   if (progress < 2000) {
      //     window.requestAnimationFrame(step);
      //   }
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
