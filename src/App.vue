<template>
  <div id="app">
    <Calendar />
  </div>
</template>

<script>
import Calendar from './components/CalendarView.vue'

export default {
  name: 'App',
  components: {
    Calendar
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 20px auto 0;
}
</style>
