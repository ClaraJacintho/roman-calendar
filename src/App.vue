<template>
  <div id="app">
    <h1>Roman Calendar Converter</h1>
    <p>Convert from the Gregorian Calandar to the roman dating system. More information on <a href="https://en.wikipedia.org/wiki/Roman_calendar">Wikipedia</a> </p>
    <div id="datepicker">
      <datepicker :inline="true" @selected="convert" format="dd/MM/yyyy"></datepicker>
    </div>
    <h2>{{this.date}}</h2>
  </div>
</template>

<script>
import Datepicker from 'vuejs-datepicker'
import romanCalendar from './assets/kalendae_romani.json'
import translations from './assets/calendar_translations.json'
export default {
  name: 'App',
  components: {
    Datepicker
  },
  data () {
    return {
      date: ''
    }
  },
  methods: {
    convert (date) {
      const month = date.getMonth() + 1
      const day = date.getDate()
      const year = date.getFullYear()
      const romanYear = year + 753
      let festivity = ''
      let romanMonth = translations.monthNames[month]
      let romanDay = ''
      for (const monthSchema of romanCalendar) {
        if (monthSchema.months.includes(month)) {
          if (day === 1) {
            festivity = 'Kalendis'
          } else if (day === monthSchema.nonis) {
            festivity = 'Nonis'
          } else if (day === monthSchema.idibus) {
            festivity = 'Idibus'
          } else if (day < monthSchema.nonis) {
            festivity = 'Nonas'
            romanDay = translations.dayNames[monthSchema.nonis - day]
          } else if (day < monthSchema.idibus) {
            festivity = 'Idus'
            romanDay = translations.dayNames[monthSchema.idibus - day]
          } else {
            festivity = 'Kalendas'
            let totalDays
            if (month === 2 && this.isLeapYear(year)) {
              totalDays = 29
            } else {
              totalDays = monthSchema.days
            }
            romanDay = translations.dayNames[totalDays - day + 1]
            romanMonth = translations.monthNames[month + 1]
          }
          break
        }
      }
      this.date = romanDay + ' ' + festivity + ' ' + romanMonth + ' Ab Urbe Condita ' + romanYear
    },
    isLeapYear (year) {
      // eslint-disable-next-line
      return !((year % 4) && (year % 100) || !(year % 400));
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #ca5959;
  margin-top: 60px;
  height: 100vh
}

#datepicker {
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
