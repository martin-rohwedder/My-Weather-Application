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

                <br />
                <br />
                
                <div v-for="(value) in this.currentWeather.weather" v-bind:key="value.id">
                    {{ value.description }}
                </div>

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
            currentWeather: {},
            loading: true,
            errored: false,
        }
    },
    mounted() {
        axios
            .get('http://api.openweathermap.org/data/2.5/weather?q=Slagelse,dk&appid=becea41c15a8e7e9c71432a09c2b2432&lang=da')
            .then(response => (this.currentWeather = response.data))
            .catch(error => {
                console.log(error)
                this.errored = true
            })
            .finally(() => {
                this.loading = false
            })
    },
    methods: {
        getCurrentWeather(cityName, country, language) {
            axios
                .get('http://api.openweathermap.org/data/2.5/weather?q=' + cityName + ',' + country + '&appid=becea41c15a8e7e9c71432a09c2b2432&lang=' + language)
                .then(response => (this.currentWeather = response.data))
                .catch(error => {
                    console.log(error)
                    this.errored = true
                })
                .finally(() => {
                    this.loading = false
                })
        }
    }

    // Weather API: https://home.openweathermap.org/
    // http://api.openweathermap.org/data/2.5/weather?q={city name},{state},{country code}&appid={your api key}
    // http://api.openweathermap.org/data/2.5/weather?q=Slagelse,dk&appid=becea41c15a8e7e9c71432a09c2b2432&lang=da
}
</script>

<style lang="stylus" scoped>

</style>