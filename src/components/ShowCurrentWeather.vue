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
                <div id="weather-content">
                    <div v-for="(value, index) in this.currentWeather.weather" v-bind:key="index">
                        <div class="test">
                            <div v-if="value.main === 'Clear'">
                                <img v-bind:src="iconUrlClearDay" class="weather-icon-image" alt="">
                            </div>
                            <div v-else-if="value.main === 'Clouds'">
                                <img v-bind:src="iconUrlCloud" class="weather-icon-image" alt="">
                            </div>
                            <div v-else-if="value.main === 'Rain'">
                                <img v-bind:src="iconUrlRain" class="weather-icon-image" alt="">
                            </div>
                            <div v-else-if="value.main === 'Snow'">
                                <img v-bind:src="iconUrlSnow" class="weather-icon-image" alt="">
                            </div>
                            <div v-else-if="value.main === 'Thunderstorm'">
                                <img v-bind:src="iconUrlThunder" class="weather-icon-image" alt="">
                            </div>
                        </div>
                        <div class="test2" v-if="index === 0">
                            {{ value.description }}
                        </div>
                    </div>
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
            iconUrlClearDay: require('../assets/weather-icons/day.svg'),
            iconUrlClearNight: require('../assets/weather-icons/night.svg'),
            iconUrlCloudDay: require('../assets/weather-icons/cloudy-day-2.svg'),
            iconUrlCloudNight: require('../assets/weather-icons/cloudy-night-2.svg'),
            iconUrlCloud: require('../assets/weather-icons/cloudy.svg'),
            iconUrlRainAndSun: require('../assets/weather-icons/rainy-3.svg'),
            iconUrlRain: require('../assets/weather-icons/rainy-5.svg'),
            iconUrlSnow: require('../assets/weather-icons/snowy-6.svg'),
            iconUrlThunder: require('../assets/weather-icons/thunder.svg'),
        }
    },
    mounted() {
        // axios
        //     .get('http://api.openweathermap.org/data/2.5/weather?q=Slagelse,dk&appid=becea41c15a8e7e9c71432a09c2b2432&lang=da&units=metric')
        //     .then(response => (this.currentWeather = response.data))
        //     .catch(error => {
        //         console.log(error)
        //         this.errored = true
        //     })
        //     .finally(() => {
        //         this.loading = false
        //     })

        this.getCurrentWeather('Slagelse', 'dk', 'da', 'metric')
    },
    methods: {
        getCurrentWeather(cityName, country, language, units) {
            axios
                .get('http://api.openweathermap.org/data/2.5/weather?q=' + cityName + ',' + country + '&appid=becea41c15a8e7e9c71432a09c2b2432&lang=' + language + '&units=' + units)
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
#weather-content
    display flex
    flex-direction column
    align-items center

.test
    display flex
    flex 1
    width 50vw
    align-items center

.test2
    display flex
    flex 1

.weather-icon-image
    width: 450px

</style>