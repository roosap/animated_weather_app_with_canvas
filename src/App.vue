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
  components: {
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
