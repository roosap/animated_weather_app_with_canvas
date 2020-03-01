<template>
  <div id="app">
    <div id="logo">
      <h1>ilma<span id="dot">.</span></h1>
    </div>
    <div id="menu">
      <input type="text" v-model="search" placeholder="Filter cities">
       <cities-select :cities="filteredCities"/>
       <city-weather :city="selectedCity" v-if="selectedCity"/>
    </div>
    <div id="footer">
      <p>Sponsored by <span id="spend">Spend<span id="dot">.</span></span></p>
    </div>
  </div>
</template>

<script>
import CitiesSelect from './components/CitiesSelect';
import CityWeather from './components/CityWeather';
import {eventBus} from './main.js';

export default {
  name: 'App',
  data() {
    return {
      cities: [],
      selectedCity: {},
      search: ""
    }
  },
  mounted() {
    this.fetchWeatherData();

    eventBus.$on('city-selected', (city) => {
      this.selectedCity = city;
    })
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
        this.cityURL(2648579), // Glasgow
        this.cityURL(3871336), // Santiago
        this.cityURL(5879400) // Anchorage
      ];

      Promise.all(urls.map(url => fetch(url)
        .then(response => response.json())
        .then(data => this.cities = [...this.cities, this.filterData(data)])
      ))
    },
    cityURL(id){
      return `https://api.openweathermap.org/data/2.5/weather?id=${id}&appid=12b0c4fd863f4ff711615b989ae19f37`
    },
    filterData(data){
      data.weather = data.weather[0].main;
      data.celsius = (parseInt(data.main.temp) - 273.15).toFixed();
      data.fahrenheit = (parseInt(data.celsius) * (9/5) + 32).toFixed();
      return data;
    }
  },
  computed: {
    filteredCities: function() {
      return this.cities.filter(city => {
        return city.name.toLowerCase().includes(this.search.toLowerCase())
      })
    }
  },
  components: {
    "cities-select": CitiesSelect,
    "city-weather": CityWeather
  }

}
</script>

<style>

  @import url('https://fonts.googleapis.com/css?family=Nunito+Sans&display=swap');

  #logo {
    font-family: 'Nunito Sans';
    color: slategrey;
    font-size: 2.5em;
    text-align: center;
    padding-bottom: 0;
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

  #spend {
    font-size: 1.2em;
  }

  #app {
    margin-left: 2rem;
  }
</style>
