<template>
    <div>
        <h1>Current Weather</h1>
        {{ currentWeather }}
    </div>
</template>

<script>
import axios from 'axios'

export default {
    name: 'ShowCurrentWeatherComponent',
    data() {
        return {
            currentWeather: null
        }
    },
    mounted() {
        axios
            .get('http://api.openweathermap.org/data/2.5/weather?q=Slagelse,dk&appid=becea41c15a8e7e9c71432a09c2b2432')
            .then(response => (this.currentWeather = response))
            .catch(error => console.log(error))
    }

    // Weather API: https://home.openweathermap.org/
    // http://api.openweathermap.org/data/2.5/weather?q={city name},{state},{country code}&appid={your api key}
    // http://api.openweathermap.org/data/2.5/weather?q=Slagelse,dk&appid=becea41c15a8e7e9c71432a09c2b2432
}
</script>

<style lang="stylus" scoped>
h1
    color: red
</style>