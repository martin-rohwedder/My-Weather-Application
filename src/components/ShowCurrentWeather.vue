<template>
    <div>
        <section v-if="errored">
            <p>Sorry, but something happened when we tried to get the data. Please try again.</p>
        </section>

        <section v-else>
            <div v-if="loading">
                The data is loading ...
            </div>

            <div v-else>
                {{ currentWeather }}
            </div>
        </section>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    name: 'ShowCurrentWeatherComponent',
    data() {
        return {
            currentWeather: null,
            loading: true,
            errored: false
        }
    },
    mounted() {
        axios
            .get('http://api.openweathermap.org/data/2.5/weather?q=Slagelse,dk&appid=becea41c15a8e7e9c71432a09c2b2432')
            .then(response => (this.currentWeather = response.data))
            .catch(error => {
                console.log(error)
                this.errored = true
            })
            .finally(() => this.loading = false)
    }

    // Weather API: https://home.openweathermap.org/
    // http://api.openweathermap.org/data/2.5/weather?q={city name},{state},{country code}&appid={your api key}
    // http://api.openweathermap.org/data/2.5/weather?q=Slagelse,dk&appid=becea41c15a8e7e9c71432a09c2b2432
}
</script>

<style lang="stylus" scoped>

</style>