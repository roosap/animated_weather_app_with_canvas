<template>
  <div id="app">
  <div id="logo">
    <h1>ilma<span id="dot">.</span></h1>
  </div>
  <div id="footer">
    <p>Sponsored by Spend<span id="dot">.</span></p>
  </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      cities: [],
      selectedCity: null // change to empty object?
    }
  },
  mounted() {
    // fetch('https://api.openweathermap.org/data/2.5/weather?id=6255152&appid=12b0c4fd863f4ff711615b989ae19f37')
    // .then(res => res.json())
    // .then(data => this.cities = [...this.cities, this.formatData(data)])
    this.fetchWeatherData();
  },
  methods: {
    fetchWeatherData() {
      const urls = [
        this.cityURL(6255152), // Antarctica
        this.cityURL(3333229), // Edinburgh
        this.cityURL(658225), // Helsinki
        this.cityURL(2636790), // Stornoway
        this.cityURL(7521757), // Svalbard
        this.cityURL(1850147), // Tokyo
        this.cityURL(1791247), // Wuhan
        this.cityURL(361058), // Alexandria
        this.cityURL(1070940), // Antananarivo
        this.cityURL(1819729), // Hong Kong
        this.cityURL(3067696), // Prague
        this.cityURL(526346), // Mirnyy
        this.cityURL(6356055), // Barcelona
        this.cityURL(3054643), // Budapest
        this.cityURL(1856215), // Nagano
        this.cityURL(2648579) // Glasgow
      ];

      Promise.all(urls.map(url => fetch(url)
        .then(response => response.json())
        .then(data => this.cities = [...this.cities, data])
      ))
    },
    cityURL(id){
      return `https://api.openweathermap.org/data/2.5/weather?id=${id}&appid=12b0c4fd863f4ff711615b989ae19f37`
    }
  }

}
</script>

<style>

  @import url('https://fonts.googleapis.com/css?family=Nunito+Sans&display=swap');

  #logo {
    font-family: 'Nunito Sans';
    color: slategrey;
    font-size: 3em;
    text-align: center;
  }

  #dot {
    color: #89B802;
  }

  #footer {
    text-align: right;
    width: 15%;
    height: auto;
    margin-right: 5rem;
    float: right;
  }
</style>
