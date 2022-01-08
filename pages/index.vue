<template>
  <div class="container-main">
    <v-img
      class="container-picture"
      :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''"
    >
      <div class="container-input">
        <v-text-field
          class="input-city"
          clearable
          background-color="white"
          color="black"
          outlined
          placeholder="Введите город"
          prepend-inner-icon="mdi-cloud-search-outline"
          @keypress="fetchWeather"
          v-model="city"
        ></v-text-field>
      </div>
      <div class="container-weather-picture">
        <div class="weather-data" v-if="typeof weather.main != 'undefined'">
          <div class="city">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="degres">{{ Math.round(weather.main.temp) + '.℃' }}</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
          <div class="date-and-time">{{ dateAndTime() }}</div>
        </div>
      </div>
    </v-img>
    <v-snackbar v-model="snackbar" color="red" min-width="20" top right>
      {{ text }}
    </v-snackbar>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        api_key: '1d39919eb692cfa08aa61b33fb97519d',
        url_base: 'http://api.openweathermap.org/data/2.5/',
        text: 'Такой город не найден',
        weather: {},
        city: '',
        snackbar: false
      }
    },
    methods: {
      async fetchWeather(e) {
        if (e.key === 'Enter') {
          const res = await fetch(
            `${this.url_base}weather?q=${this.city}&units=metric&APPID=${this.api_key}`
          )
          const data = await res.json()
          this.setResults(data)
        }
      },
      setResults(results) {
        this.weather = results
        if (this.weather.cod === '404') {
          this.snackbar = true
        }
      },
      dateAndTime() {
        let d = new Date()
        let months = ['01', '02', '03', '04', '05', '06', '07', '08', '09', '10', '11', '12']
        let days = [
          'Понедельник',
          'Вторник',
          'Среда',
          'Четверг',
          'Пятница',
          'Суббота',
          'Воскресенье'
        ]

        let day = days[d.getDay()]
        let date = d.getDate()
        let month = months[d.getMonth()]
        let year = d.getFullYear()
        return `${day} ${date}. ${month}. ${year}`
      }
    }
  }
</script>

<style lang="sass">
  @import '~assets/_variables.sass'
  @import url('https://fonts.googleapis.com/css2?family=Oswald:wght@200&display=swap')

  .container-main
    background-color: $dark
    width: 300px
    height: 300px
    border-radius: 0 10px 10px 10px
    .container-picture
      width: 300px
      height: 300px
      border-radius: 0 10px 10px 10px
      transition: 0.4s
      background-image: url('./static/day.jpg')
    .container-picture.warm
      background-image: url('./static/night.jpg')

  .container-input
    padding: 5px
    .input-city
      width: 400px
      height: 40px
      font-size: 25px
      font-family: 'Oswald', sans-serif
      border-radius: 20px
      text-align: center


  .container-weather-picture
    display: flex
    .weather-data
      padding: 10px
      font-size: 20px
      color: white
      .city
        font-size: 30px
        font-family: 'Oswald', sans-serif
      .degres
        margin-left: 20px
        padding: 5px
        font-size: 30px
        width: 90px
        height: 50px
        background: #45C2B7
        border-radius: 15px
      .weather
        margin-left: 20px
        font-size: 40px
        font-family: 'Oswald', sans-serif
      .date-and-time
        margin-top: 50px
        font-size: 15px
        font-family: 'Oswald', sans-serif
</style>
