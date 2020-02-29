<template lang="html">
  <div v-if="city">
    <h2>{{city.name}}</h2>
    <div id="details">
      <p>{{city.weather}}</p>
      <p>{{city.celsius}} °C</p>
      <p>{{city.fahrenheit}} °F</p>
    </div>
      <!-- :class="city.weather.toLowerCase()" -->
      <canvas id="canvas" width="578" height="200"></canvas>
  </div>
</template>

<script>
export default {
  props: ['city'],
  mounted() {
    this.draw()
  },
  methods: {
    draw: function() {
      let context = canvas.getContext('2d');
      context.beginPath();
      context.moveTo(170, 80);
      context.bezierCurveTo(120, 100, 130, 150, 230, 150);
      context.bezierCurveTo(250, 150, 300, 170, 340, 150);
      context.bezierCurveTo(450, 170, 460, 120, 410, 90);
      context.bezierCurveTo(400, 60, 370, 40, 350, 50);
      context.bezierCurveTo(320, 10, 270, 10, 240, 50);
      context.bezierCurveTo(200, 30, 180, 50, 170, 80);
      context.closePath();
      context.fillStyle = '#D3D3D3';
      context.fill();
    }
  }
}
</script>

<style lang="css" scoped>
  h2 {
    text-align: center;
    font-size: 1.5em;
  }

  #details {
    width: 50rem;
    font-size: 1em;
  }

  #canvas {
    width: 45rem;
    margin-left: 20rem;
    margin-bottom: 10rem;
  }

</style>
