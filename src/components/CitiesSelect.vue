<template lang="html">
  <div>
    <span v-if="clickStat"><select @change="handleChange" class="dropdown">
      <option v-for="(city, index) in cities" :value="index" :key="index">{{city.name}}</option>
    </select></span>
</div>
</template>

<script>
import {eventBus} from '../main.js'

export default {
  name: 'cities-select',
  props: ['cities', 'clickStatus'],
  data() {
    return {
      clickStat: false
    }
  },
  mounted() {
    eventBus.$on('clicked', (status) => {
      this.clickStat = status
    })
  },
  methods: {
    handleChange() {
      eventBus.$emit('city-selected', this.cities[event.target.value]);
    }
  }
}
</script>

<style lang="css" scoped>
  .dropdown {
    display: block;
    font-size: 0.7em;
    border: 1px solid dimgrey;
    border-radius: 0em;
    line-height: 1.3;
    padding: 2px 2px;
    width: 10rem;
    appearance: none;
  }

  .dropdown:hover {
    border-color: #89B802;
    width: 12rem;
  }

  button {
    display: block;
    font-size: 0.8em;
    border: 1px solid dimgrey;
    border-radius: 0em;
    line-height: 1.3;
    padding: 2px 2px;
    width: 10rem;
    appearance: none;
  }

  button:hover {
    border-color: #89B802;
    width: 12rem;
  }

</style>
