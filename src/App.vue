<template>
  <div id="app">
    <h1>Date Picker</h1>
    <DatePicker ref="datePicker" :selected="dueTime" @selected="onDateSelected"></DatePicker>
  </div>
</template>

<script>
// import Hello from './components/Hello'
import DatePicker from './components/date-picker'

export default {
  name: 'app',
  components: {
    DatePicker
  },
  data () {
    return {
      dueTime: new Date('2017-07-14')
    }
  },
  methods: {
    onDateSelected (date) {
      this.dueTime = new Date(date)
      console.log(this.dueTime)
    }
  }
}
</script>

<style scoped>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
