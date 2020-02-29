<template lang="html">
  <div class="">
    <select @change="handleChange">
      <option value="" disabled selected>Select your city</option>
      <option v-for="(city, index) in cities" :value="index" :key="index">{{city.name}}</option>
    </select>
  </div>
</template>

<script>
import {eventBus} from '../main.js'

export default {
  name: 'cities-select',
  props: ['cities'],
  methods: {
    handleChange() {
      // this.search.value = "";
      eventBus.$emit('city-selected', this.cities[event.target.value]);
    }
  }
}
</script>

<style lang="css" scoped>
</style>
