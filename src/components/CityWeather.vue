<template lang="html">
  <div v-if="city">
    <span>
    <h2 :class="city.celsius > 10 ? 'warm' : 'cold'">{{city.name}}</h2>
    <div id="details">
      <p>{{city.weather}}</p>
    </div>
    <div id="temp">
      <p>{{city.celsius}} °C</p>
      <p>{{city.fahrenheit}} °F</p>
    </div>
      <canvas id="canvas" width="700" height="400">Current weather in {{city.name}} is {{city.weather}}</canvas>
    </span>
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
      angle: 200,
      drops: [],
      flakes: []
    }
  },
  mounted() {
    requestAnimationFrame(this.anim),
    eventBus.$on('city-selected', (city) => {
      this.currentWeather = city.weather
      if (this.currentWeather === "Rain") {
        this.createRaindrops()
      } else if (this.currentWeather === "Snow") {
        this.createSnowflakes()
      }
    })
  },
  methods: {
    anim: function(timestamp) {
      if (!start) start = timestamp;
      let ctx = canvas.getContext('2d');
      ctx.clearRect(0, 0, canvas.width, canvas.height);

      if (this.currentWeather === "Clouds") {
        this.cloud()

      } else if (this.currentWeather === "Clear") {
        this.sunny()
        let radius = 25 + 100 * Math.abs(Math.cos(this.angle));
          this.angle += 2 / 150

      } else if (this.currentWeather === "Rain") {
        this.drops.forEach((drop) => {
          this.rain(drop.x, drop.y);
          drop.y += 1;
          if (drop.y > canvas.height) {
            drop.y = 0;
          }
        })

      } else if (this.currentWeather === "Snow") {
        this.flakes.forEach((flake) => {
          this.snow(flake.x, flake.y);
          flake.y += 1;
          if (flake.y > canvas.height) {
            flake.y = 0;
          }
        })
        
      } else {
        this.sorry()
      }
      // Don't touch this, else laptop will overheat and die
      if ((timestamp - start) < 2000) {
        start = null;
        requestAnimationFrame(this.anim)
      }
    },
      cloud: function() {
        let ctx = canvas.getContext('2d');
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        ctx.beginPath();
        ctx.moveTo(170, 80);
        ctx.bezierCurveTo(110, 100, 130, 150, 230, 150);
        ctx.bezierCurveTo(250, 150, 300, 170, 340, 150);
        ctx.bezierCurveTo(450, 170, 460, 120, 410, 90);
        ctx.bezierCurveTo(400, 60, 370, 40, 350, 50);
        ctx.bezierCurveTo(320, 10, 270, 10, 240, 50);
        ctx.bezierCurveTo(200, 30, 180, 50, 170, 80);
        ctx.closePath();
        ctx.fillStyle = '#D3D3D3';
        ctx.fill();
      },
      sunny: function() {
        let ctx = canvas.getContext('2d');
        let radius = 25 + 100 * Math.abs(Math.cos(this.angle));
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        ctx.beginPath();
        ctx.arc(310, 150, radius, 0, 2 * Math.PI);
        ctx.closePath();
        ctx.fillStyle = '#ffcc00';
        ctx.fill();
      },
      rain: function (posX, posY) {
        let ctx = canvas.getContext('2d');
        ctx.beginPath();
        ctx.moveTo(posX - 5, posY);
        ctx.lineTo(posX, posY - 7);
        ctx.lineTo(posX + 5, posY);
        ctx.arc(posX, posY, 5, 0, Math.PI);
        ctx.closePath();
        ctx.fillStyle = '#7094cf';
        ctx.fill();
      },
      createRaindrops: function() {
        this.drops = []
        this.drops.push({ x: 180, y: 0 });
        this.drops.push({ x: 280, y: 50 });
        this.drops.push({ x: 350, y: 120 });
        this.drops.push({ x: 470, y: 40 });
        this.drops.push({ x: 300, y: 170 });
        this.drops.push({ x: 400, y: 290 });
        this.drops.push({ x: 500, y: 0 });
        this.drops.push({ x: 160, y: 220 });
      },
      snow: function (posX, posY) {
        let ctx = canvas.getContext('2d');
        ctx.beginPath();
        ctx.arc(posX, posY, 7, 0, Math.PI * 2, false);
        ctx.closePath();
        ctx.fill();
        ctx.fillStyle = '#d3e1e6';
      },
      createSnowflakes: function() {
        this.flakes = []
        this.flakes.push({ x: 180, y: 0 });
        this.flakes.push({ x: 280, y: 50 });
        this.flakes.push({ x: 350, y: 120 });
        this.flakes.push({ x: 470, y: 40 });
        this.flakes.push({ x: 300, y: 170 });
        this.flakes.push({ x: 400, y: 290 });
        this.flakes.push({ x: 500, y: 0 });
        this.flakes.push({ x: 160, y: 120 });
        this.flakes.push({ x: 240, y: 30 });
        this.flakes.push({ x: 320, y: 10 });
        this.flakes.push({ x: 420, y: 300 });
        this.flakes.push({ x: 200, y: 100 });
      },
      sorry: function() {
        let ctx = canvas.getContext('2d');
        ctx.font = "1.8em Nunito";
        ctx.fillStyle = "slategrey";
        ctx.textAlign = "center";
        ctx.fillText("Sorry, we haven't animated your weather just yet!", canvas.width/2, canvas.height/2);
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
