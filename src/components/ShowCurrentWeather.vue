<template>
    <div>
        <h1>Current Weather</h1>
    </div>
</template>

<script>
export default {
    name: 'ShowCurrentWeatherComponent'

    // Weather API: https://home.openweathermap.org/
    // api.openweathermap.org/data/2.5/weather?q={city name},{state},{country code}&appid={your api key}
    // api.openweathermap.org/data/2.5/weather?q=Slagelse,dk&appid=becea41c15a8e7e9c71432a09c2b2432
}
</script>

<style lang="stylus" scoped>
h1
    color: red
</style>